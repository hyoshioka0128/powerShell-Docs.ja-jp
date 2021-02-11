---
ms.date: 09/13/2016
ms.topic: reference
title: パイプライン入力を処理するパラメーターを追加する
description: パイプライン入力を処理するパラメーターを追加する
ms.openlocfilehash: b150d5be93207d9c010a6d2d4de901b4526f1d79
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668356"
---
# <a name="adding-parameters-that-process-pipeline-input"></a>パイプライン入力を処理するパラメーターを追加する

コマンドレットの1つの入力ソースは、上流のコマンドレットで発生したパイプライン上のオブジェクトです。 このセクションでは、コマンドレットがパイプラインオブジェクトを処理できるように、Get-Proc コマンドレット ( [最初のコマンドレットの作成](./creating-a-cmdlet-without-parameters.md)に関するページで説明) にパラメーターを追加する方法について説明します。

この Get-Proc コマンドレットは、 `Name` パイプラインオブジェクトからの入力を受け入れるパラメーターを使用し、指定された名前に基づいてローカルコンピューターからプロセス情報を取得し、コマンドラインでプロセスに関する情報を表示します。

## <a name="defining-the-cmdlet-class"></a>コマンドレットクラスの定義

コマンドレットの作成の最初の手順では、常にコマンドレットに名前を付け、コマンドレットを実装する .NET クラスを宣言します。 このコマンドレットはプロセス情報を取得するため、ここで選択した動詞名は "Get" です。 (情報を取得できるほとんどすべての種類のコマンドレットは、コマンドライン入力を処理できます)。承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

この Get-Proc コマンドレットの定義を次に示します。 この定義の詳細につい [ては、最初のコマンドレットの作成](./creating-a-cmdlet-without-parameters.md)に関する説明をご確認ください。

```csharp
[Cmdlet(VerbsCommon.Get, "proc")]
public class GetProcCommand : Cmdlet
```

```vb
<Cmdlet(VerbsCommon.Get, "Proc")> _
Public Class GetProcCommand
    Inherits Cmdlet
```

## <a name="defining-input-from-the-pipeline"></a>パイプラインからの入力の定義

このセクションでは、コマンドレットのパイプラインからの入力を定義する方法について説明します。 この Get-Proc コマンドレットは、 `Name` 「 [コマンドライン入力を処理するパラメーターの追加](./adding-parameters-that-process-command-line-input.md)」で説明されているように、パラメーターを表すプロパティを定義します。
(パラメーターの宣言に関する一般的な情報については、「」を参照してください)。

ただし、コマンドレットでパイプラインの入力を処理する必要がある場合は、Windows PowerShell ランタイムによって入力値にバインドされたパラメーターを持つ必要があります。 これを行うには、キーワードを追加するか、またはキーワードを system.string 属性宣言に追加する必要があり `ValueFromPipeline` `ValueFromPipelineByProperty` ます。 [](/dotnet/api/System.Management.Automation.ParameterAttribute) `ValueFromPipeline`コマンドレットが入力オブジェクト全体にアクセスする場合は、キーワードを指定します。 `ValueFromPipelineByProperty`コマンドレットがオブジェクトのプロパティのみにアクセスする場合は、を指定します。

`Name`パイプラインの入力を受け入れるこの Get-Proc コマンドレットのパラメーターのパラメーター宣言を次に示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/GetProcessSample03/GetProcessSample03.cs" range="35-44":::

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

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplesgetproc03#GetProc03VBNameParameter](Msh_samplesgetproc03#GetProc03VBNameParameter)]  -->

前の宣言では、 `ValueFromPipeline` キーワードをに設定します `true` 。これにより、オブジェクトがパラメーターと同じ型である場合、または同じ型に強制的に変換できる場合は、Windows PowerShell ランタイムによってパラメーターが受信オブジェクトにバインドされます。 `ValueFromPipelineByPropertyName`キーワードは、 `true` Windows PowerShell ランタイムがプロパティの受信オブジェクトを確認するようににも設定されてい `Name` ます。 受信オブジェクトにこのようなプロパティがある場合、ランタイムは、 `Name` 受信したオブジェクトのプロパティにパラメーターをバインドし `Name` ます。

> [!NOTE]
> `ValueFromPipeline`パラメーターの属性キーワードの設定は、キーワードの設定よりも優先され `ValueFromPipelineByPropertyName` ます。

## <a name="overriding-an-input-processing-method"></a>入力処理メソッドのオーバーライド

コマンドレットがパイプライン入力を処理する場合は、適切な入力処理メソッドをオーバーライドする必要があります。 基本的な入力処理方法は、 [最初のコマンドレットを作成するとき](./creating-a-cmdlet-without-parameters.md)に導入されます。

この Get-Proc コマンドレットは [、](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) `Name` ユーザーまたはスクリプトによって指定されたパラメーター入力を処理するために、system.object メソッドをオーバーライドします。 このメソッドは、要求された各プロセス名のプロセスを取得します。名前が指定されていない場合は、すべてのプロセスを取得します。 [WriteObject (system.object, system.string)](/dotnet/api/system.management.automation.cmdlet.writeobject#System_Management_Automation_Cmdlet_WriteObject_System_Object_System_Boolean_)の呼び出しが、出力オブジェクトをパイプラインに送信するための出力機構として使用されていることに注意し[てください。](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) この呼び出しの2番目のパラメーターは、をに設定して、 `enumerateCollection` `true` プロセスオブジェクトの配列を列挙し、一度に1つのプロセスをコマンドラインに書き込むように Windows PowerShell ランタイムに指示します。

```csharp
protected override void ProcessRecord()
{
  // If no process names are passed to the cmdlet, get all processes.
  if (processNames == null)
  {
      // Write the processes to the pipeline making them available
      // to the next cmdlet. The second argument of this call tells
      // PowerShell to enumerate the array, and send one process at a
      // time to the pipeline.
      WriteObject(Process.GetProcesses(), true);
  }
  else
  {
    // If process names are passed to the cmdlet, get and write
    // the associated processes.
    foreach (string name in processNames)
    {
      WriteObject(Process.GetProcessesByName(name), true);
    } // End foreach (string name...).
  }
}
```

```vb
Protected Overrides Sub ProcessRecord()
    Dim processes As Process()

    '/ If no process names are passed to the cmdlet, get all processes.
    If processNames Is Nothing Then
        processes = Process.GetProcesses()
    Else

        '/ If process names are specified, write the processes to the
        '/ pipeline to display them or make them available to the next cmdlet.
        For Each name As String In processNames
            '/ The second parameter of this call tells PowerShell to enumerate the
            '/ array, and send one process at a time to the pipeline.
            WriteObject(Process.GetProcessesByName(name), True)
        Next
    End If

End Sub 'ProcessRecord
```

## <a name="code-sample"></a>コード サンプル

完全な C# サンプルコードについては、「 [GetProcessSample03 sample](./getprocesssample03-sample.md)」を参照してください。

## <a name="defining-object-types-and-formatting"></a>オブジェクトの種類と書式設定の定義

Windows PowerShell は、.Net オブジェクトを使用してコマンドレット間で情報を渡します。 そのため、コマンドレットで独自の型を定義する必要がある場合や、コマンドレットで別のコマンドレットによって提供される既存の型を拡張する必要がある場合があります。 新しい型を定義する、または既存の型を拡張する方法の詳細については、「 [オブジェクトの型と書式設定の拡張](/previous-versions//ms714665(v=vs.85))」を参照してください。

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後は、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが Windows PowerShell に登録されている場合は、コマンドラインで実行してテストします。 たとえば、サンプルコマンドレットのコードをテストします。 コマンドラインからコマンドレットを使用する方法の詳細については、「 [Windows PowerShell を使用したはじめに](/powershell/scripting/getting-started/getting-started-with-windows-powershell)」を参照してください。

- Windows PowerShell プロンプトで、次のコマンドを入力して、パイプラインを介してプロセス名を取得します。

  ```powershell
  PS> type ProcessNames | get-proc
  ```

  次のような出力が表示されます。

  ```
  Handles  NPM(K)  PM(K)   WS(K)  VS(M)  CPU(s)    Id  ProcessName
  -------  ------  -----   ----- -----   ------    --  -----------
      809      21  40856    4448    147    9.50  2288  iexplore
      737      21  26036   16348    144   22.03  3860  iexplore
       39       2   1024     388     30    0.08  3396  notepad
     3927      62  71836   26984    467  195.19  1848  OUTLOOK
  ```

- 次の行を入力して、 `Name` "iexplore.exe" というプロセスのプロパティを持つプロセスオブジェクトを取得します。 この例では、 `Get-Process` コマンドレット (Windows PowerShell によって提供) をアップストリームコマンドとして使用して、"iexplore.exe" プロセスを取得します。

  ```powershell
  PS> get-process iexplore | get-proc
  ```

  次のような出力が表示されます。

  ```
  Handles  NPM(K)  PM(K)   WS(K)  VS(M)  CPU(s)    Id  ProcessName
  -------  ------  -----   ----- -----   ------    --  -----------
      801      21  40720    6544    142    9.52  2288  iexplore
      726      21  25872   16652    138   22.09  3860  iexplore
      801      21  40720    6544    142    9.52  2288  iexplore
      726      21  25872   16652    138   22.09  3860  iexplore
  ```

## <a name="see-also"></a>参照

[コマンドライン入力を処理するパラメーターの追加](./adding-parameters-that-process-command-line-input.md)

[最初のコマンドレットの作成](./creating-a-cmdlet-without-parameters.md)

[オブジェクトの種類と書式設定の拡張](/previous-versions//ms714665(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))

[Windows PowerShell リファレンス](../windows-powershell-reference.md)

[コマンドレット サンプル](./cmdlet-samples.md)
