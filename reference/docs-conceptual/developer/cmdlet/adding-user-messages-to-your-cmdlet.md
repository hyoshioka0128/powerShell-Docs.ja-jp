---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットにユーザー メッセージを追加する
description: コマンドレットにユーザー メッセージを追加する
ms.openlocfilehash: de6fcc093af1d01287eed1eb8cc7b81cf5d2fdd8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668339"
---
# <a name="adding-user-messages-to-your-cmdlet"></a>コマンドレットにユーザー メッセージを追加する

コマンドレットでは、Windows PowerShell ランタイムによってユーザーに表示できるいくつかの種類のメッセージを書き込むことができます。 これらのメッセージには、次の種類があります。

- 一般的なユーザー情報を含む詳細なメッセージ。

- トラブルシューティング情報を含むメッセージをデバッグします。

- コマンドレットが予期しない結果になる可能性のある操作を実行しようとしていることを示す通知を含む警告メッセージ。

- 実行時間が長い操作を実行するときにコマンドレットが完了した作業量に関する情報を含む進行状況レポートメッセージ。

コマンドレットで書き込むことができるメッセージの数や、コマンドレットが書き込むメッセージの種類に制限はありません。 各メッセージは、コマンドレットの入力処理メソッド内から特定の呼び出しを行うことによって書き込まれます。

## <a name="defining-the-cmdlet"></a>コマンドレットの定義

コマンドレットの作成の最初の手順では、常にコマンドレットに名前を付け、コマンドレットを実装する .NET クラスを宣言します。 任意の種類のコマンドレットは、入力処理メソッドからユーザー通知を書き込むことができます。そのため、一般に、このコマンドレットには、コマンドレットで実行されるシステム変更を示す動詞を使用して名前を指定できます。 承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

Stop-Proc コマンドレットは、システムを変更するように設計されています。したがって、.NET クラスの[system.string 宣言には、](/dotnet/api/System.Management.Automation.CmdletAttribute) attribute キーワードを含め、をに設定する必要があり `SupportsShouldProcess` ます。 `true`

次のコードは、この Stop-Proc コマンドレットクラスの定義です。 この定義の詳細については、「 [システムを変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください。

```csharp
[Cmdlet(VerbsLifecycle.Stop, "proc",
        SupportsShouldProcess = true)]
public class StopProcCommand : Cmdlet
```

## <a name="defining-parameters-for-system-modification"></a>システム変更のパラメーターの定義

Stop-Proc コマンドレットは、、、およびの3つのパラメーターを定義し `Name` `Force` `PassThru` ます。 これらのパラメーターの定義の詳細については、「 [システムを変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください。

Stop-Proc コマンドレットのパラメーター宣言を次に示します。

```csharp
[Parameter(
           Position = 0,
           Mandatory = true,
           ValueFromPipeline = true,
           ValueFromPipelineByPropertyName = true
)]
public string[] Name
{
  get { return processNames; }
  set { processNames = value; }
}
private string[] processNames;

/// <summary>
/// Specify the Force parameter that allows the user to override
/// the ShouldContinue call to force the stop operation. This
/// parameter should always be used with caution.
/// </summary>
[Parameter]
public SwitchParameter Force
{
  get { return force; }
  set { force = value; }
}
private bool force;

/// <summary>
/// Specify the PassThru parameter that allows the user to specify
/// that the cmdlet should pass the process object down the pipeline
/// after the process has been stopped.
/// </summary>
[Parameter]
public SwitchParameter PassThru
{
  get { return passThru; }
  set { passThru = value; }
}
private bool passThru;
```

## <a name="overriding-an-input-processing-method"></a>入力処理メソッドのオーバーライド

コマンドレットは、入力処理メソッドをオーバーライドする必要があります。ほとんどの場合、この [コマンドレットは](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)、system.object を変更します。 この Stop-Proc コマンドレットは、 [システム管理](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) の入力処理方法をオーバーライドします。 この Stop-Proc コマンドレットの実装では、詳細メッセージ、デバッグメッセージ、および警告メッセージを書き込む呼び出しが行われます。

> [!NOTE]
> この[メソッドの呼び出し](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)方法の詳細については、「システムを[変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください。[このメソッドは](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)、「」を参照してください。

## <a name="writing-a-verbose-message"></a>詳細メッセージを書き込んでいます

特定 [のエラー](/dotnet/api/System.Management.Automation.Cmdlet.WriteVerbose) 条件に関連しない一般的なユーザーレベル情報を書き込むには、system.string メソッドを使用します。 システム管理者は、その情報を使用して、他のコマンドの処理を続行できます。 また、このメソッドを使用して記述された情報は、必要に応じてローカライズする必要があります。

この Stop-Proc コマンドレットの次のコードでは [、system.](/dotnet/api/System.Management.Automation.Cmdlet.WriteVerbose) . [...](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) .....................................................

```csharp
message = String.Format("Attempting to stop process \"{0}\".", name);
WriteVerbose(message);
```

```csharp
message = String.Format("Stopped process \"{0}\", pid {1}.",
                        processName, process.Id);

WriteVerbose(message);
```

## <a name="writing-a-debug-message"></a>デバッグメッセージの書き込み

コマンドレットの操作のトラブルシューティングに使用できるデバッグメッセージを記述するには、このメソッドを [使用します](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug) 。 この呼び出しは、入力処理メソッドによって行われます。

> [!NOTE]
> Windows PowerShell では、 `Debug` 詳細情報とデバッグ情報の両方を表示するパラメーターも定義されています。 コマンド [レットでこの](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug) パラメーターがサポートされている場合は、 [system](/dotnet/api/System.Management.Automation.Cmdlet.WriteVerbose).......................................................

Sample Stop-Proc コマンドレットの次の2つのセクションでは、system......................................[コマンド](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)レットのオーバーライドから、[システム](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug)の呼び出しを示します。

このデバッグメッセージは、システムの [管理](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) が呼び出される直前に書き込まれます。

```csharp
message =
          String.Format("Acquired name for pid {0} : \"{1}\"",
                       process.Id, processName);
WriteDebug(message);
```

このデバッグメッセージは、 [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject) が呼び出される直前に書き込まれます。

```csharp
message =
         String.Format("Writing process \"{0}\" to pipeline",
         processName);
WriteDebug(message);
WriteObject(process);
```

Windows PowerShell は、トレースインフラストラクチャとコマンドレットに対して、すべての [システム](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug) の呼び出しを自動的にルーティングします。 これにより、メソッド呼び出しをホストアプリケーション、ファイル、またはデバッガーにトレースできます。コマンドレット内で追加の開発作業を行う必要はありません。 次のコマンドラインエントリは、トレース操作を実装します。

**PS> トレース-式の停止-proc-file proc .log-command stop-proc notepad**

## <a name="writing-a-warning-message"></a>警告メッセージを書き込んでいます

コマンドレットで、予期しない結果 (読み取り専用ファイルの上書きなど) が発生する可能性がある操作を実行しようとしているときに、 [このメソッドを使用して](/dotnet/api/System.Management.Automation.Cmdlet.WriteWarning) 警告を記述します。

Sample Stop-Proc コマンドレットの次のコードでは [、system.](/dotnet/api/System.Management.Automation.Cmdlet.WriteWarning) . [...](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) ......................................................

```csharp
 if (criticalProcess)
 {
   message =
             String.Format("Stopping the critical process \"{0}\".",
                           processName);
   WriteWarning(message);
} // if (criticalProcess...
```

## <a name="writing-a-progress-message"></a>進行状況メッセージの書き込み

コマンドレット操作の完了に時間がかかる場合は、進行状況を示すメッセージを書き込むために、 [システム管理の進行状況](/dotnet/api/System.Management.Automation.Cmdlet.WriteProgress) が使用されます。 System.servicemodel の呼び出しは、ユーザーに表示するためにホストアプリケーションに送信される、[システムの管理](/dotnet/api/System.Management.Automation.ProgressRecord)...... [progress](/dotnet/api/System.Management.Automation.Cmdlet.WriteProgress)レコードオブジェクトを渡します。

> [!NOTE]
> この Stop-Proc コマンドレットには、 [system.servicemodel メソッドの](/dotnet/api/System.Management.Automation.Cmdlet.WriteProgress) 呼び出しが含まれていません。

次のコードは、項目をコピーしようとしているコマンドレットによって作成された進行状況メッセージの例です。

```csharp
int myId = 0;
string myActivity = "Copy-item: Copying *.* to c:\abc";
string myStatus = "Copying file bar.txt";
ProgressRecord pr = new ProgressRecord(myId, myActivity, myStatus);
WriteProgress(pr);

pr.RecordType = ProgressRecordType.Completed;
WriteProgress(pr);
```

## <a name="code-sample"></a>コード サンプル

完全な C# サンプルコードについては、「 [StopProcessSample02 sample](./stopprocesssample02-sample.md)」を参照してください。

## <a name="define-object-types-and-formatting"></a>オブジェクトの種類と書式を定義する

Windows PowerShell は、.NET オブジェクトを使用してコマンドレット間で情報を渡します。 そのため、コマンドレットで独自の型を定義する必要がある場合や、コマンドレットで別のコマンドレットによって提供される既存の型を拡張する必要がある場合があります。 新しい型を定義する、または既存の型を拡張する方法の詳細については、「 [オブジェクトの型と書式設定の拡張](/previous-versions//ms714665(v=vs.85))」を参照してください。

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後は、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが Windows PowerShell に登録されている場合は、コマンドラインで実行することでテストできます。 サンプル Stop-Proc コマンドレットをテストしてみましょう。 コマンドラインからコマンドレットを使用する方法の詳細については、「 [Windows PowerShell を使用したはじめに](/powershell/scripting/getting-started/getting-started-with-windows-powershell)」を参照してください。

- 次のコマンドラインエントリは、Stop-Proc を使用して、"NOTEPAD" という名前のプロセスを停止し、詳細通知を提供し、デバッグ情報を出力します。

    ```powershell
    PS> stop-proc -Name notepad -Verbose -Debug
    ```

    次のような出力が表示されます。

    ```
    VERBOSE: Attempting to stop process " notepad ".
    DEBUG: Acquired name for pid 5584 : "notepad"

    Confirm
    Continue with this operation?
    [Y] Yes  [A] Yes to All  [H] Halt Command  [S] Suspend  [?] Help (default is "Y"): Y

    Confirm
    Are you sure you want to perform this action?
    Performing operation "stop-proc" on Target "notepad (5584)".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
    VERBOSE: Stopped process "notepad", pid 5584.
    ```

## <a name="see-also"></a>参照

[システムを変更するコマンドレットを作成する](./creating-a-cmdlet-that-modifies-the-system.md)

[Windows PowerShell コマンドレットを作成する方法](/powershell/scripting/developer/cmdlet/writing-a-windows-powershell-cmdlet)

[オブジェクトの種類と書式設定の拡張](/previous-versions//ms714665(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))

[Windows PowerShell SDK](../windows-powershell-reference.md)
