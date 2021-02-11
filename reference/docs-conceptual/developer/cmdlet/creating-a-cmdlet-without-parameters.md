---
ms.date: 09/13/2016
ms.topic: reference
title: パラメーターなしでコマンドレットを作成する
description: パラメーターなしでコマンドレットを作成する
ms.openlocfilehash: 5df27ac4c1f6dfcc3e7596d93f8db0f97aae71c1
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646548"
---
# <a name="creating-a-cmdlet-without-parameters"></a>パラメーターなしでコマンドレットを作成する

このセクションでは、パラメーターを使用せずにローカルコンピューターから情報を取得するコマンドレットを作成し、その情報をパイプラインに書き込む方法について説明します。 ここで説明するコマンドレットは、ローカルコンピューターのプロセスに関する情報を取得し、その情報をコマンドラインに表示する Get-Proc コマンドレットです。

> [!NOTE]
> コマンドレットを記述するときは、Windows PowerShell®参照アセンブリがディスクにダウンロードされることに注意してください (既定では C:\Program Files\Reference Assemblies\Microsoft\WindowsPowerShell\v1.0 にあります)。 グローバルアセンブリキャッシュ (GAC) にはインストールされません。

## <a name="naming-the-cmdlet"></a>コマンドレットの名前付け

コマンドレット名は、コマンドレットによって実行されるアクションを示す動詞と、コマンドレットが処理する項目を示す名詞で構成されます。 このサンプル Get-Proc コマンドレットはプロセスオブジェクトを取得するので、 [Verbscommon](/dotnet/api/System.Management.Automation.VerbsCommon) 列挙型によって定義された動詞 "Get" を使用し、"Proc" という名詞を使用して、コマンドレットがプロセス項目で動作することを示します。

コマンドレットに名前を付けるときは、#、() {} [] &-/\ $;: "' <> &#124;  のいずれの文字も使用しないでください。 @ ` .

### <a name="choosing-a-noun"></a>名詞の選択

固有の名詞を選択する必要があります。 製品名の短縮版をプレフィックスとして使用することをお勧めします。 この種類のコマンドレット名の例は、" `Get-SQLServer` " です。

### <a name="choosing-a-verb"></a>動詞の選択

一連の承認されたコマンドレット動詞名から動詞を使用する必要があります。 承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

## <a name="defining-the-cmdlet-class"></a>コマンドレットクラスの定義

コマンドレット名を選択したら、コマンドレットを実装する .NET クラスを定義します。 このサンプル Get-Proc コマンドレットのクラス定義を次に示します。

```csharp
[Cmdlet(VerbsCommon.Get, "Proc")]
  public class GetProcCommand : Cmdlet
```

```vb
<Cmdlet(VerbsCommon.Get, "Proc")> _
Public Class GetProcCommand
    Inherits Cmdlet
```

クラス定義の前に、構文を使用[](/dotnet/api/System.Management.Automation.CmdletAttribute)して、 `[Cmdlet(verb, noun, ...)]` このクラスをコマンドレットとして指定することに注意してください。 これは、すべてのコマンドレットに必要な唯一の属性であり、Windows PowerShell ランタイムでそれらを正しく呼び出すことができます。 必要に応じて、クラスをさらに宣言する属性キーワードを設定できます。 サンプルの Getのコマンドクラスの属性宣言では、Get-Proc コマンドレットの名詞と動詞の名前のみを宣言することに注意してください。

> [!NOTE]
> すべての Windows PowerShell 属性クラスに対して設定できるキーワードは、属性クラスのプロパティに対応しています。

コマンドレットのクラスに名前を付けるときは、クラス名にコマンドレット名を反映することをお勧めします。 これを行うには、"VerbNounCommand" という形式を使用し、"Verb" と "名詞" をコマンドレット名に使用されている動詞と名詞に置き換えます。 前のクラス定義に示されているように、サンプルの Get-Proc コマンドレットは、Getによって実行されるクラスを定義 [します。](/dotnet/api/System.Management.Automation.Cmdlet) このクラスは、system.servicemodel 基本クラスから派生します。

> [!IMPORTANT]
> Windows PowerShell ランタイムに直接アクセスするコマンドレットを定義する場合は、.NET クラスを [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) 基底クラスから派生させる必要があります。 このクラスの詳細については、「 [パラメーターセットを定義するコマンドレットを作成](./adding-parameter-sets-to-a-cmdlet.md)する」を参照してください。

> [!NOTE]
> コマンドレットのクラスは、明示的にパブリックとしてマークされている必要があります。 パブリックとしてマークされていないクラスは、既定で内部に設定され、Windows PowerShell ランタイムによって検出されません。

Windows PowerShell では、コマンドレットのクラスとして、 [powershell](/dotnet/api/Microsoft.PowerShell.Commands) の名前空間が使用されます。 コマンドレットクラスを API 名前空間のコマンド名前空間に配置することをお勧めします (例、xxx. .PS. コマンド)。

## <a name="overriding-an-input-processing-method"></a>入力処理メソッドのオーバーライド

System.servicemodel [クラスには、3](/dotnet/api/System.Management.Automation.Cmdlet) つの主要な入力処理メソッドが用意されています。少なくとも1つはコマンドレットがオーバーライドする必要があります。 Windows PowerShell がレコードを処理する方法の詳細については、「 [Windows powershell のしくみ](/previous-versions//ms714658(v=vs.85))」を参照してください。

すべての種類の入力について、Windows PowerShell ランタイムは、処理を有効にするために、system.string[を呼び出します。](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) コマンドレットでいくつかの前処理またはセットアップを実行する必要がある場合は、このメソッドをオーバーライドすることによってこれを行うことができます。

> [!NOTE]
> Windows PowerShell では、コマンドレットが呼び出されたときに指定されるパラメーター値のセットを記述するために "レコード" という用語が使用されます。

コマンドレットがパイプライン入力を受け入れる場合は、この [メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) オーバーライドする必要があります。また、 [必要に応じて、](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) system.object メソッドをオーバーライドする必要があります。 たとえば、コマンドレットを使用してすべての入力を [収集し、](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) コマンドレットの実行時に、一度に1つの要素ではなく全体として入力を操作すると、両方のメソッドがオーバーライドされる可能性があり `Sort-Object` ます。

コマンドレットがパイプライン入力を受け取らない場合 [は、このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) オーバーライドする必要があります。 このメソッドは、並べ替えコマンドレットの場合と同様に、一度に1つの要素に対してコマンドレットを実行できない場合に、その代わりに使用されることに注意して [ください](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) 。

このサンプル Get-Proc コマンドレットはパイプライン入力を受け取る必要があるため、[このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)オーバーライドして、system........................................... コマンド[レットの既定](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing)の実装を使用[します。](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject)メソッドを使用して、プロセスを取得し、コマンドラインに書き込むプロセスを取得するために、[このオーバーライドが](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)実行されます。

```csharp
protected override void ProcessRecord()
{
  // Get the current processes
  Process[] processes = Process.GetProcesses();

  // Write the processes to the pipeline making them available
  // to the next cmdlet. The second parameter of this call tells
  // PowerShell to enumerate the array, and send one process at a
  // time to the pipeline.
  WriteObject(processes, true);
}
```

```vb
Protected Overrides Sub ProcessRecord()

    '/ Get the current processes.
    Dim processes As Process()
    processes = Process.GetProcesses()

    '/ Write the processes to the pipeline making them available
    '/ to the next cmdlet. The second parameter of this call tells
    '/ PowerShell to enumerate the array, and send one process at a
    '/ time to the pipeline.
    WriteObject(processes, True)

End Sub 'ProcessRecord
```

#### <a name="things-to-remember-about-input-processing"></a>入力処理に関する注意事項

- 入力の既定のソースは、コマンドラインでユーザーによって指定された明示的なオブジェクト (文字列など) です。 詳細については、「 [コマンドライン入力を処理するためのコマンドレットの作成](./adding-parameters-that-process-command-line-input.md)」を参照してください。

- 入力処理メソッドは、パイプラインの上流コマンドレットの出力オブジェクトから入力を受け取ることもできます。 詳細については、「 [パイプライン入力を処理するためのコマンドレットの作成](./adding-parameters-that-process-pipeline-input.md)」を参照してください。 コマンドレットは、コマンドラインとパイプラインのソースの組み合わせから入力を受け取ることができることに注意してください。

- ダウンストリームのコマンドレットは、長い間、またはまったく返さない場合があります。 そのため、コマンドレットの入力処理メソッドは、 [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject)の呼び出し中にロックを保持しないようにする必要があります。特に、スコープがコマンドレットインスタンスの範囲を超えているロックです。

> [!IMPORTANT]
> コマンドレットは、system.string [*](/dotnet/api/System.Console.WriteLine) またはそれと同等のものを呼び出すことはできません。

- コマンドレットは、処理が完了したときにクリーンアップするオブジェクト変数を持つ場合があります (たとえば、ファイルハンドルが [システム](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) によって開かれている場合は、そのハンドルを開いたままにして [、プロセスが](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)開かれます。 Windows PowerShell ランタイムでは、オブジェクトのクリーンアップを実行する必要があるため、必ずしも必ず [system.object メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) 呼び出さないことに注意してください。

たとえば、コマンドレットが途中で取り消された場合、またはコマンドレットのいずれかの部分で終了エラーが発生した場合 [は、を](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) 呼び出すことはできません。 したがって、オブジェクトのクリーンアップを必要とするコマンドレットは、ファイナライザーを含む完全な[IDisposable](/dotnet/api/System.IDisposable)パターンを実装する必要があります。これにより、処理の[終了時に](/dotnet/api/System.IDisposable.Dispose)ランタイム[が両方の](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing)を呼び出すことができるようになります。

## <a name="code-sample"></a>コード サンプル

完全な C# サンプルコードについては、「 [GetProcessSample01 sample](./getprocesssample01-sample.md)」を参照してください。

## <a name="defining-object-types-and-formatting"></a>オブジェクトの種類と書式設定の定義

Windows PowerShell は、.NET オブジェクトを使用してコマンドレット間で情報を渡します。 そのため、コマンドレットで独自の型を定義する必要がある場合や、コマンドレットで別のコマンドレットによって提供される既存の型を拡張する必要がある場合があります。 新しい型を定義する、または既存の型を拡張する方法の詳細については、「 [オブジェクトの型と書式設定の拡張](/previous-versions//ms714665(v=vs.85))」を参照してください。

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが Windows PowerShell に登録されている場合は、コマンドラインで実行することでテストできます。 サンプル Get-Proc コマンドレットのコードは小さいですが、Windows PowerShell ランタイムと既存の .NET オブジェクトが引き続き使用されます。これにより、これが役に立ちます。 これをテストして、Get-Proc できることと、その出力を使用する方法について理解を深めてみましょう。 コマンドラインからコマンドレットを使用する方法の詳細については、「 [Windows PowerShell を使用したはじめに](/powershell/scripting/getting-started/getting-started-with-windows-powershell)」を参照してください。

1. Windows PowerShell を起動し、コンピューターで現在実行されているプロセスを取得します。

    ```powershell
    get-proc
    ```

    次のような出力が表示されます。

    ```output
    Handles  NPM(K)  PM(K)  WS(K)  VS(M)  CPU(s)  Id   ProcessName
    -------  ------  -----  -----  -----  ------  --   ----------
    254      7       7664   12048  66     173.75  1200  QCTRAY
    32       2       1372   2628   31       0.04  1860  DLG
    271      6       1216   3688   33       0.03  3816  lg
    27       2       560    1920   24       0.01  1768  TpScrex
    ...
    ```

2. 操作を容易にするために、コマンドレットの結果に変数を割り当てます。

    ```powershell
    $p=get-proc
    ```

3. プロセスの数を取得します。

    ```powershell
    $p.length
    ```

    次のような出力が表示されます。

    ```output
    63
    ```

4. 特定のプロセスを取得します。

    ```powershell
    $p[6]
    ```

    次のような出力が表示されます。

    ```output
    Handles  NPM(K)  PM(K)  WS(K)  VS(M)  CPU(s)  Id    ProcessName
    -------  ------  -----  -----  -----  ------  --    -----------
    1033     3       2400   3336   35     0.53    1588  rundll32
    ```

5. このプロセスの開始時刻を取得します。

    ```powershell
    $p[6].starttime
    ```

    次のような出力が表示されます。

    ```output
    Tuesday, July 26, 2005 9:34:15 AM
    ```

    ```powershell
    $p[6].starttime.dayofyear
    ```

    ```output
    207
    ```

6. ハンドル数が500より大きいプロセスを取得し、結果を並べ替えます。

    ```powershell
    $p | Where-Object {$_.HandleCount -gt 500 } | Sort-Object HandleCount
    ```

    次のような出力が表示されます。

    ```output
    Handles  NPM(K)  PM(K)  WS(K)  VS(M)  CPU(s)  Id   ProcessName
    -------  ------  -----  -----  -----  ------  --   ----------
    568      14      2164   4972   39     5.55    824  svchost
    716       7      2080   5332   28    25.38    468  csrss
    761      21      33060  56608  440  393.56    3300 WINWORD
    791      71      7412   4540   59     3.31    492  winlogon
    ...
    ```

7. `Get-Member`各プロセスで使用可能なプロパティを一覧表示するには、コマンドレットを使用します。

    ```powershell
    $p | Get-Member -MemberType property
    ```

    ```output
        TypeName: System.Diagnostics.Process
    ```

    次のような出力が表示されます。

    ```output
    Name                     MemberType Definition
    ----                     ---------- ----------
    BasePriority             Property   System.Int32 BasePriority {get;}
    Container                Property   System.ComponentModel.IContainer Conta...
    EnableRaisingEvents      Property   System.Boolean EnableRaisingEvents {ge...
    ...
    ```

## <a name="see-also"></a>参照

[コマンドライン入力を処理するコマンドレットの作成](./adding-parameters-that-process-command-line-input.md)

[パイプラインの入力を処理するコマンドレットの作成](./adding-parameters-that-process-pipeline-input.md)

[Windows PowerShell コマンドレットを作成する方法](/powershell/scripting/developer/cmdlet/writing-a-windows-powershell-cmdlet)

[オブジェクトの種類と書式設定の拡張](/previous-versions//ms714665(v=vs.85))

[Windows PowerShell の動作](/previous-versions//ms714658(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))

[Windows PowerShell リファレンス](../windows-powershell-reference.md)

[コマンドレット サンプル](./cmdlet-samples.md)
