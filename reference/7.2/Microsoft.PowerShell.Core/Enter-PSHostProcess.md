---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 07/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enter-pshostprocess?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enter-PSHostProcess
ms.openlocfilehash: 85cbc9d012781873dddaf2a7d220a0e478dcbda2
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602619"
---
# Enter-PSHostProcess

## 概要
ローカルプロセスを使用して、対話型セッションに接続して入力します。

## SYNTAX

### ProcessIdParameterSet (既定値)

```
Enter-PSHostProcess [-Id] <Int32> [[-AppDomainName] <String>] [<CommonParameters>]
```

### ProcessParameterSet

```
Enter-PSHostProcess [-Process] <Process> [[-AppDomainName] <String>] [<CommonParameters>]
```

### ProcessNameParameterSet

```
Enter-PSHostProcess [-Name] <String> [[-AppDomainName] <String>] [<CommonParameters>]
```

### PSHostProcessInfoParameterSet

```
Enter-PSHostProcess [-HostProcessInfo] <PSHostProcessInfo> [[-AppDomainName] <String>]
 [<CommonParameters>]
```

### PipeNameParameterSet

```
Enter-PSHostProcess -CustomPipeName <String> [<CommonParameters>]
```

## Description

`Enter-PSHostProcess`コマンドレットは、ローカルプロセスを使用して対話型セッションに接続し、それに入力します。 PowerShell 6.2 以降では、このコマンドレットは Windows 以外のプラットフォームでサポートされています。

PowerShell をホストする新しいプロセスを作成し、リモートセッションを実行する代わりに、既に PowerShell を実行している既存のプロセスで、リモートの対話型セッションが実行されます。 指定されたプロセスでリモートセッションを操作する場合は、実行中の実行空間を列挙し、またはのいずれかを実行してデバッグする実行空間を選択でき `Debug-Runspace` `Enable-RunspaceDebug` ます。

入力するプロセスは、PowerShell (System.Management.Automation.dll) をホストしている必要があります。
プロセスが検出されたコンピューターの Administrators グループのメンバーであるか、またはプロセスを開始したスクリプトを実行しているユーザーである必要があります。

デバッグする実行空間を選択すると、現在コマンドを実行しているか、デバッガーで停止している場合、リモートデバッグセッションが実行空間に対して開かれます。 その後、他のリモートセッションスクリプトをデバッグする場合と同じ方法で、実行空間スクリプトをデバッグできます。

デバッグセッションからデタッチした後、プロセスとの対話型セッションを終了するには、exit を2回実行します。または、既存のデバッガーの quit コマンドを実行して、スクリプトの実行を停止します。

**Name** パラメーターを使用してプロセスを指定し、指定した名前のプロセスが1つだけ見つかった場合は、プロセスが入力されます。 指定された名前を持つ複数のプロセスが見つかった場合、PowerShell はエラーを返し、指定された名前を持つすべてのプロセスを一覧表示します。

リモートコンピューター上のプロセスへのアタッチをサポートするために、 `Enter-PSHostProcess` 指定したリモートコンピューターでコマンドレットが有効になります。これにより、リモート PowerShell セッション内のローカルプロセスにアタッチできるようになります。

## 例

### 例 1: PowerShell ISE プロセス内の実行空間のデバッグを開始する

この例では、 `Enter-PSHostProcess` powershell コンソール内からを実行して、POWERSHELL ISE プロセスに入ります。 生成された対話型セッションでは、を実行してデバッグする実行空間を見つけて、実行 `Get-Runspace` 空間をデバッグできます。

```
PS C:\> Enter-PSHostProcess -Name powershell_ise
[Process:1520]: PS C:\Test\Documents>

Next, get available runspaces within the process you have entered.
PS C:\> [Process:1520]: PS C:\>  Get-Runspace
Id    Name          InstanceId                               State           Availability
--    -------       -----------                              ------          -------------
1     Runspace1     2d91211d-9cce-42f0-ab0e-71ac258b32b5     Opened          Available
2     Runspace2     a3855043-cb16-424a-a616-685360c3763b     Opened          RemoteDebug
3     MyLocalRS     2236dbd8-2105-4dec-a15a-a27d0bfaacb5     Opened          LocalDebug
4     MyRunspace    771356e9-8c44-4b70-9de5-dd17cb41e48e     Opened          Busy
5     Runspace8     3e517382-a97a-49ba-9c3c-fd21f6664288     Broken          None

The runspace objects returned by Get-Runspace also have a NoteProperty called ScriptStackTrace of
the running command stack, if available.Next, debug runspace ID 4, that is running another user's
long-running script. From the list returned from Get-Runspace, note that the runspace state is
Opened, and Availability is Busy, meaning that the runspace is still running the long-running
script.

PS C:\> [Process:1520]: PS C:\>  (Get-Runspace -Id 4).ScriptStackTrace
Command                    Arguments                           Location
-------                    ---------                           --------
MyModuleWorkflowF1         {}                                  TestNoFile3.psm1: line 6
WFTest1                    {}                                  TestNoFile2.ps1: line 14
TestNoFile2.ps1            {}                                  TestNoFile2.ps1: line 22
<ScriptBlock>              {}                                  <No file>

Start an interactive debugging session with this runspace by running the Debug-Runspace cmdlet.

PS C:\> [Process: 1520]: PS C:\>  Debug-Runspace -Id 4
Hit Line breakpoint on 'C:\TestWFVar1.ps1:83'

At C:\TestWFVar1.ps1:83 char:1
+ $scriptVar = "Script Variable"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[Process: 1520]: [RSDBG: 4]: PS C:\> >

After you are finished debugging, allow the script to continue running without the debugger attached
by running the exit debugger command. Alternatively, you can quit the debugger with the q or Stop
commands.

PS C:\> [Process:346]: [RSDBG: 3]: PS C:\> > exit
[Process:1520]: PS C:\>

When you are finished working in the process, exit the process by running the Exit-PSHostProcess
cmdlet. This returns you to the PS C:\> prompt.

PS C:\> [Process:1520]: PS C:\>  Exit-PSHostProcess
PS C:\>
```

## PARAMETERS

### -AppDomainName

省略した場合に接続するアプリケーションドメイン名を指定します。 **Defaultappdomain** を使用します。 `Get-PSHostProcessInfo`アプリケーションドメイン名を表示するには、を使用します。

```yaml
Type: System.String
Parameter Sets: ProcessIdParameterSet, ProcessParameterSet, ProcessNameParameterSet, PSHostProcessInfoParameterSet
Aliases:

Required: False
Position: 1
Default value: DefaultAppDomain
Accept pipeline input: False
Accept wildcard characters: False
```

### -HostProcessInfo

PowerShell を使用して接続できる **get-pshostprocessinfo** オブジェクトを指定します。 `Get-PSHostProcessInfo`オブジェクトを取得するには、を使用します。

```yaml
Type: Microsoft.PowerShell.Commands.PSHostProcessInfo
Parameter Sets: PSHostProcessInfoParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Id

プロセス ID を指定してプロセスを指定します。 プロセス ID を取得するには、 `Get-Process` コマンドレットを実行します。

```yaml
Type: System.Int32
Parameter Sets: ProcessIdParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

プロセス名でプロセスを指定します。 プロセス名を取得するには、 `Get-Process` コマンドレットを実行します。 また、タスクマネージャーでプロセスの [プロパティ] ダイアログボックスからプロセス名を取得することもできます。

```yaml
Type: System.String
Parameter Sets: ProcessNameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Process

プロセスオブジェクトによってプロセスを指定します。 このパラメーターを使用する最も簡単な方法は、 `Get-Process` 変数に入力するプロセスを返すコマンドの結果を保存し、その変数をこのパラメーターの値として指定することです。

```yaml
Type: System.Diagnostics.Process
Parameter Sets: ProcessParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -CustomPipeName

接続先のカスタム名前付きパイプ名を取得または設定します。 これは通常、と組み合わせて使用され `pwsh -CustomPipeName` ます。

このパラメーターは、PowerShell 6.2 で導入されました。

```yaml
Type: System.String
Parameter Sets: PipeNameParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Diagnostics.Process

## 出力

## 注

`Enter-PSHostProcess` コマンドを実行している PowerShell セッションのプロセスを入力できません。 ただし、別の PowerShell セッションのプロセスや、を実行しているセッションと同時に実行されている PowerShell ISE セッションを入力することができ `Enter-PSHostProcess` ます。

`Enter-PSHostProcess` PowerShell をホストしているプロセスのみを入力できます。 つまり、PowerShell エンジンが読み込まれています。

プロセス内からプロセスを終了するには、「 **exit**」と入力し <kbd>、enter キーを押します。</kbd>

PowerShell 7.1 より前では、SSH 経由のリモート処理は、次ホップのリモート セッションをサポートしていませんでした。 この機能は、WinRM を使用したセッションに限定されていました。 PowerShell 7.1 を使用すると、任意の対話型リモート セッション内で `Enter-PSSession` と `Enter-PSHostProcess` が機能します。

## 関連リンク

[Exit-PSHostProcess](Exit-PSHostProcess.md)

[Get-PSHostProcessInfo](Get-PSHostProcessInfo.md)

