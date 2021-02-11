---
ms.date: 09/13/2016
ms.topic: reference
title: 終了しないエラーのレポートをコマンドレットに追加する
description: 終了しないエラーのレポートをコマンドレットに追加する
ms.openlocfilehash: 883ff2d522266495e409fb0d45f29713baa6f047
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648677"
---
# <a name="adding-non-terminating-error-reporting-to-your-cmdlet"></a>終了しないエラーのレポートをコマンドレットに追加する

コマンドレットは、 [WriteError][] メソッドを呼び出すことによって終了しないエラーを報告し、引き続き現在の入力オブジェクトまたはその他の受信パイプラインオブジェクトで操作を続行できます。 このセクションでは、入力処理メソッドから終了しないエラーを報告するコマンドレットを作成する方法について説明します。

終了しないエラー (および終了エラー) については、コマンドレットはエラーを識別する system.servicemodel [レコード][] オブジェクトを渡す必要があります。 各エラーレコードは、"エラー識別子" と呼ばれる一意の文字列によって識別されます。 識別子に加えて、各エラーのカテゴリは [ErrorCategory][] 列挙型によって定義された定数によって指定されます。 ユーザーは、変数を "category View" に設定することにより、カテゴリに基づいてエラーを表示でき `$ErrorView` ます。

エラーレコードの詳細については、「 [Windows PowerShell エラーレコード](./windows-powershell-error-records.md)」を参照してください。

## <a name="defining-the-cmdlet"></a>コマンドレットの定義

コマンドレットの作成の最初の手順では、常にコマンドレットに名前を付け、コマンドレットを実装する .NET クラスを宣言します。 このコマンドレットはプロセス情報を取得するため、ここで選択した動詞名は "Get" です。 (情報を取得できるほとんどすべての種類のコマンドレットは、コマンドライン入力を処理できます)。承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](approved-verbs-for-windows-powershell-commands.md)」を参照してください。

このコマンドレットの定義を次に示し `Get-Proc` ます。 この定義の詳細につい [ては、最初のコマンドレットの作成](creating-a-cmdlet-without-parameters.md)に関する説明をご確認ください。

```csharp
[Cmdlet(VerbsCommon.Get, "proc")]
public class GetProcCommand: Cmdlet
```

```vb
<Cmdlet(VerbsCommon.Get, "Proc")> _
Public Class GetProcCommand
    Inherits Cmdlet
```

## <a name="defining-parameters"></a>パラメーターの定義

必要に応じて、コマンドレットで入力を処理するためのパラメーターを定義する必要があります。 この Get-Proc コマンドレットは、「 [Command-Line 入力を処理するパラメーターの追加](adding-parameters-that-process-command-line-input.md)」で説明されているように、 **Name** パラメーターを定義します。

この Get-Proc コマンドレットの **Name** パラメーターのパラメーター宣言を次に示します。

```csharp
[Parameter(
           Position = 0,
           ValueFromPipeline = true,
           ValueFromPipelineByPropertyName = true
)]
[ValidateNotNullOrEmpty]
public string[] Name
{
  get { return processNames; }
  set { processNames = value; }
}
private string[] processNames;
```

```vb
<Parameter(Position:=0, ValueFromPipeline:=True, _
ValueFromPipelineByPropertyName:=True), ValidateNotNullOrEmpty()> _
Public Property Name() As String()
    Get
        Return processNames
    End Get

    Set(ByVal value As String())
        processNames = value
    End Set

End Property
```

## <a name="overriding-input-processing-methods"></a>オーバーライド (入力処理メソッドを)

すべてのコマンドレットで、 [少なくとも][] 1 つの入力処理メソッドをオーバーライドする必要があります。 これらのメソッドについ [ては、最初のコマンドレットの作成](creating-a-cmdlet-without-parameters.md)に関するセクションで説明します。

> [!NOTE]
> コマンドレットでは、各レコードを可能な限り個別に処理する必要があります。

この Get-Proc コマンドレットは、ユーザーまたはスクリプトによって提供される入力の **Name** パラメーターを処理するために、 [system.servicemodel メソッドを][]オーバーライドします。 このメソッドは、要求された各プロセス名のプロセスを取得します。名前が指定されていない場合は、すべてのプロセスを取得します。 この上書きの詳細につい [ては、最初のコマンドレットの作成](creating-a-cmdlet-without-parameters.md)に関する説明をご確認ください。

### <a name="things-to-remember-when-reporting-errors"></a>エラーを報告する際の注意事項

エラーの書き込み時にコマンドレットによって渡される、 [システムの管理][] オブジェクトには、コアで例外が必要です。 使用する例外を決定するときは、.NET ガイドラインに従ってください。
基本的に、エラーの意味が既存の例外と同じである場合、コマンドレットはその例外を使用するか、その例外から派生する必要があります。 それ以外の場合は、新しい例外または例外の階層を [system.exception][] クラスから直接派生させる必要があります。

エラー識別子 (ErrorRecord クラスの FullyQualifiedErrorId プロパティを通じてアクセスされる) を作成する場合は、次の点に注意してください。

- 診断を目的とした文字列を使用して、完全修飾識別子を調べるときにエラーの内容とエラーの発生元を特定できます。

- 正しい形式の完全修飾エラー識別子は、次のようになります。

  `CommandNotFoundException,Microsoft.PowerShell.Commands.GetCommandCommand`

前の例では、エラー識別子 (最初のトークン) はエラーの内容を指定し、残りの部分はエラーの発生元を示しています。

- より複雑なシナリオでは、エラー識別子を、検査時に解析できるドット区切りのトークンにすることができます。 これにより、エラー識別子とエラーカテゴリの両方の部分を分岐することができます。

コマンドレットでは、特定のエラー識別子を異なるコードパスに割り当てる必要があります。 エラー識別子の割り当てについては、次の情報を考慮してください。

- エラー識別子は、コマンドレットのライフサイクル全体で一定のままである必要があります。 コマンドレットのバージョン間でエラー識別子のセマンティクスを変更しないでください。
- Tersely が報告されているエラーに対応するエラー識別子には、テキストを使用します。 空白や句読点は使用しないでください。
- コマンドレットで、再現可能なエラー識別子だけを生成するようにします。 たとえば、プロセス識別子を含む識別子を生成しないようにする必要があります。 エラー識別子は、同じ問題が発生している他のユーザーによって認識される識別子に対応している場合にのみ、ユーザーにとって役立ちます。

次の状況では、PowerShell でハンドルされない例外はキャッチされません。

- コマンドレットが新しいスレッドを作成し、そのスレッドで実行されているコードがハンドルされない例外をスローした場合、PowerShell はエラーをキャッチせず、プロセスを終了します。
- オブジェクトのデストラクターにコードが含まれているか、ハンドルされない例外の原因となった Dispose メソッドがある場合、PowerShell はエラーをキャッチせずにプロセスを終了します。

## <a name="reporting-nonterminating-errors"></a>終了しないエラーの報告

いずれかの入力処理方法では、 [WriteError][] メソッドを使用して、出力ストリームに終了しないエラーを報告できます。

この Get-Proc コマンドレットからのコード例を次に示し[ます。この][]コマンドレットは、WriteError メソッドのオーバーライド内からの[][]の呼び出しを示しています。 この場合、指定されたプロセス識別子のプロセスがコマンドレットで見つからない場合、呼び出しが行われます。

```csharp
protected override void ProcessRecord()
{
  // If no name parameter passed to cmdlet, get all processes.
  if (processNames == null)
  {
    WriteObject(Process.GetProcesses(), true);
  }
    else
    {
      // If a name parameter is passed to cmdlet, get and write
      // the associated processes.
      // Write a nonterminating error for failure to retrieve
      // a process.
      foreach (string name in processNames)
      {
        Process[] processes;

        try
        {
          processes = Process.GetProcessesByName(name);
        }
        catch (InvalidOperationException ex)
        {
          WriteError(new ErrorRecord(
                     ex,
                     "NameNotFound",
                     ErrorCategory.InvalidOperation,
                     name));
          continue;
        }

        WriteObject(processes, true);
      } // foreach (...
    } // else
  }
```

### <a name="things-to-remember-about-writing-nonterminating-errors"></a>終了しないエラーの書き込みに関する注意事項

終了しないエラーの場合、コマンドレットは、特定の入力オブジェクトごとに特定のエラー識別子を生成する必要があります。

コマンドレットでは、終了しないエラーによって生成される PowerShell アクションを頻繁に変更する必要があります。 これを行うには、 `ErrorAction` パラメーターとパラメーターを定義し `ErrorVariable` ます。 パラメーターを定義すると、コマンドレットによってユーザーオプションの [system.servicemodel] が表示されます。 `ErrorAction` また[、][]変数を設定することによって、アクションに直接影響を与えることもできます。 `$ErrorActionPreference`

コマンドレットでは、パラメーターを使用して、終了しないエラーを変数に保存でき `ErrorVariable` ます。これは、の設定の影響を受けません `ErrorAction` 。 エラーは、変数名の前にプラス記号 (+) を追加することによって、既存のエラー変数に追加できます。

## <a name="code-sample"></a>コード サンプル

完全な C# サンプルコードについては、「 [GetProcessSample04 sample](./getprocesssample04-sample.md)」を参照してください。

## <a name="define-object-types-and-formatting"></a>オブジェクトの種類と書式を定義する

PowerShell は、.NET オブジェクトを使用してコマンドレット間で情報を渡します。 そのため、コマンドレットで独自の型を定義する必要がある場合や、コマンドレットで別のコマンドレットによって提供される既存の型を拡張する必要がある場合があります。 新しい型を定義する、または既存の型を拡張する方法の詳細については、「 [オブジェクトの型と書式設定の拡張](/previous-versions/ms714665(v=vs.85))」を参照してください。

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが PowerShell に登録されている場合は、コマンドラインで実行することでテストできます。 サンプル Get-Proc コマンドレットをテストして、エラーが報告されているかどうかを確認してみましょう。

- PowerShell を起動し、Get-Proc コマンドレットを使用して "TEST" という名前のプロセスを取得します。

  ```powershell
  get-proc -name test
  ```

  次のような出力が表示されます。

  ```
  get-proc : Operation is not valid due to the current state of the object.
  At line:1 char:9
  + get-proc  <<<< -name test
  ```

## <a name="see-also"></a>参照

[パイプライン入力を処理するパラメーターを追加する](./adding-parameters-that-process-pipeline-input.md)

[Command-Line 入力を処理するパラメーターの追加](./adding-parameters-that-process-command-line-input.md)

[最初のコマンドレットの作成](./creating-a-cmdlet-without-parameters.md)

[オブジェクトの種類と書式設定の拡張](/previous-versions/ms714665(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions/ms714644(v=vs.85))

[Windows PowerShell リファレンス](../windows-powershell-reference.md)

[コマンドレット サンプル](./cmdlet-samples.md)

[System.Exception]: /dotnet/api/System.Exception
[システムの管理. ActionPreference]: /dotnet/api/System.Management.Automation.ActionPreference
[システムの管理....................]: /dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord
[WriteError (システム管理)]: /dotnet/api/System.Management.Automation.Cmdlet.WriteError
[System. Automation. コマンドレット]: /dotnet/api/System.Management.Automation.Cmdlet
[ErrorCategory (システム管理)]: /dotnet/api/System.Management.Automation.ErrorCategory
[システムの管理. ErrorRecord]: /dotnet/api/System.Management.Automation.ErrorRecord
