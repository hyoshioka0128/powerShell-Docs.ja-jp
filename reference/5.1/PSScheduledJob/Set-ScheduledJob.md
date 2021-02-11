---
external help file: Microsoft.PowerShell.ScheduledJob.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PSScheduledJob
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/psscheduledjob/set-scheduledjob?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-ScheduledJob
ms.openlocfilehash: 6144d9f19b86727bc09d07e94f4bcf158e3b7071
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94387908"
---
# Set-ScheduledJob

## 概要
スケジュールされたジョブを変更します。

## SYNTAX

### ScriptBlock (既定値)

```
Set-ScheduledJob [-Name <String>] [-ScriptBlock <ScriptBlock>] [-Trigger <ScheduledJobTrigger[]>]
 [-InitializationScript <ScriptBlock>] [-RunAs32] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-ScheduledJobOption <ScheduledJobOptions>]
 [-InputObject] <ScheduledJobDefinition> [-MaxResultCount <Int32>] [-PassThru] [-ArgumentList <Object[]>]
 [-RunNow] [-RunEvery <TimeSpan>] [<CommonParameters>]
```

### FilePath

```
Set-ScheduledJob [-Name <String>] [-FilePath <String>] [-Trigger <ScheduledJobTrigger[]>]
 [-InitializationScript <ScriptBlock>] [-RunAs32] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-ScheduledJobOption <ScheduledJobOptions>]
 [-InputObject] <ScheduledJobDefinition> [-MaxResultCount <Int32>] [-PassThru] [-ArgumentList <Object[]>]
 [-RunNow] [-RunEvery <TimeSpan>] [<CommonParameters>]
```

### 実行

```
Set-ScheduledJob [-InputObject] <ScheduledJobDefinition> [-ClearExecutionHistory] [-PassThru]
 [<CommonParameters>]
```

## Description
**Set-ScheduledJob** コマンドレットは、ジョブで実行するコマンドや、ジョブを実行するために必要な資格情報など、スケジュールされたジョブのプロパティを変更します。
このコマンドレットを使用して、スケジュールされたジョブの実行履歴をクリアすることもできます。

このコマンドレットを使用するには、まず Get-ScheduledJob コマンドレットを使用して、スケジュールされたジョブを取得します。
次に、スケジュールされたジョブを **Set ScheduledJob** にパイプするか、変数にジョブを保存し、 *InputObject* パラメーターを使用してジョブを識別します。
**Set-ScheduledJob** の他のパラメーターを使用して、ジョブのプロパティを変更したり、実行履歴をクリアしたりします。

**Set ScheduledJob** を使用して、スケジュールされたジョブのトリガーとオプションを変更できますが、Jobtrigger、Set jobtrigger、および Set-ScheduledJobOption コマンドレットを使用すると、これらのタスクをより簡単に実行できます。
スケジュールされた新しいジョブを作成するには、Register-ScheduledJob コマンドレットを使用します。

**Set ScheduledJob** の *Trigger* パラメーターは、ジョブを開始する1つまたは複数のジョブトリガーを追加します。
*Trigger* パラメーターは省略可能であるため、スケジュールされたジョブを作成するときにトリガーを追加し、後でジョブトリガーを追加します。次に、 *runnow* パラメーターを追加してすぐにジョブを開始し、Start-Job コマンドレットを使用して、スケジュールされていないジョブを他のジョブのテンプレートとして保存します。

**設定-scheduledjob** は、Windows PowerShell に含まれている psscheduledjob モジュールのジョブスケジュールコマンドレットのコレクションの1つです。

スケジュールされたジョブの詳細については、PSScheduledJob モジュールの概要トピックを参照してください。
PSScheduledJob モジュールをインポートし、次のように入力し `Get-Help about_Scheduled*` ます。または about_Scheduled_Jobs を参照してください。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: ジョブを実行するスクリプトを変更する

```
PS C:\> Get-ScheduledJob -Name "Inventory"
Id         Name            Triggers        Command                                  Enabled
--         ----            --------        -------                                  -------
1          Inventory       {1}             C:\Scripts\Get-Inventory.ps1             True

The second command uses the Get-ScheduledJob cmdlet to get the Inventory scheduled job. A pipeline operator (|) sends the scheduled job to the **Set-ScheduledJob** cmdlet. The **Set-ScheduledJob** cmdlet uses the *Script* parameter to specify a new script, Get-FullInventory.ps1. The command uses the *Passthru* parameter to return the scheduled job after the change.
PS C:\> Get-ScheduledJob -Name "Inventory" | Set-ScheduledJob -FilePath "C:\Scripts\Get-FullInventory.ps1" -Passthru
Id         Name            Triggers        Command                                  Enabled
--         ----            --------        -------                                  -------
1          Inventory       {1}             C:\Scripts\Get-FullInventory.ps1         True
```

この例では、スケジュールされたジョブで実行するスクリプトを変更する方法を示します。

最初のコマンドは、Get-ScheduledJob コマンドレットを使用して、スケジュールされたジョブのインベントリを取得します。
出力には、ジョブで Get-Inventory.ps1 スクリプトが実行されることが示されています。

このコマンドは必須ではありません。スクリプトの変更の結果を表示するためにのみ含まれています。

### 例 2: スケジュールされたジョブの実行履歴を削除する

```
PS C:\> Get-ScheduledJob BackupArchive | Set-ScheduledJob -ClearExecutionHistory
```

このコマンドは、スケジュールされたジョブ BackupArchive の現在の実行履歴と保存されているジョブの結果を削除します。

このコマンドは、Get-ScheduledJob コマンドレットを使用して、スケジュールされたジョブスケジュール backuparchive を取得します。
パイプライン演算子 (|) により、ジョブを **Set-ScheduledJob** コマンドレットに渡して、ジョブを変更します。
**Set ScheduledJob** コマンドレットは、 *ClearExecutionHistory* パラメーターを使用して、実行履歴と保存された結果を削除します。

スケジュールされたジョブの実行履歴と保存されているジョブの結果の詳細については、「about_Scheduled_Jobs」を参照してください。

### 例 3: リモートコンピューター上のスケジュールされたジョブを変更する

```
PS C:\> Invoke-Command -Computer "Server01, Server02" -ScriptBlock {Get-ScheduledJob | Set-ScheduledJob -InitializationScript \\SrvA\Scripts\SetForRun.ps1}
```

このコマンドは、Server01 コンピューターと Server02 コンピューター上のスケジュールされたすべてのジョブの初期化スクリプトを変更します。

このコマンドは、Invoke-Command コマンドレットを使用して、Server01 コンピューターと Server02 コンピューター上でコマンドを実行します。

リモートコマンドは、コンピューター上のスケジュールされたすべてのジョブを取得する Get-ScheduledJob コマンドで開始します。
スケジュールされたジョブは、 **Set scheduledjob** コマンドレットにパイプされます。このコマンドレットは、初期化スクリプトを SetForRun.ps1 に変更します。

## PARAMETERS

### -ArgumentList
*FilePath* パラメーターで指定されたスクリプトのパラメーター、または *ScriptBlock* パラメーターで指定されたコマンドの値を指定します。

```yaml
Type: System.Object[]
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -認証
ユーザーの資格情報の認証に使用するメカニズムを指定します。
このパラメーターの有効値は、次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential

既定値は Default です。 このパラメーターの値の詳細については、PowerShell SDK の [Authenticationmechanism 列挙](/dotnet/api/system.management.automation.runspaces.authenticationmechanism) に関する説明を参照してください。

注意: ユーザーの資格情報が認証対象のリモートコンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。
このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。
リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: ScriptBlock, FilePath
Aliases:
Accepted values: Default, Basic, Negotiate, NegotiateWithImplicitCredential, Credssp, Digest, Kerberos

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ClearExecutionHistory
スケジュールされたジョブの現在の実行履歴と保存されている結果を削除します。

ジョブの実行履歴とジョブの結果は、スケジュールされたジョブと共に、ジョブが作成されたコンピューターの $home\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJobs ディレクトリに保存されます。
実行履歴を表示するには、Get-Job コマンドレットを使用します。
ジョブの結果を取得するには、Receive-Job コマンドレットを使用します。

このパラメーターによって、タスク スケジューラから Windows イベント ログに書き込まれたイベントが影響を受けることはありません。また、Windows PowerShell がジョブの結果の保存を停止することもありません。
保存されるジョブの結果の数を管理するには、 *MaxResultCount* パラメーターを使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Execution
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential
スケジュールされたジョブを実行するアクセス許可を持つユーザー アカウントを指定します。
既定値は現在のユーザーです。

User01 や Domain01\user01」などのユーザー名を入力するか、Get-Credential コマンドレットのような **PSCredential** オブジェクトを入力します。
ユーザー名のみを入力すると、パスワードの入力を促すメッセージが表示されます。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath
スケジュールされたジョブで実行するスクリプトを指定します。
ローカル コンピューター上の .ps1 ファイルのパスを入力します。
スクリプト パラメーターの既定値を指定するには、 *ArgumentList* パラメーターを使用します。
スケジュールされたすべてのジョブには、 *ScriptBlock* 値または *FilePath* 値が必ず設定されています。

```yaml
Type: System.String
Parameter Sets: FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InitializationScript
Windows PowerShell スクリプト (.ps1) の完全修飾パスを指定します。
初期化スクリプトは、 *ScriptBlock* パラメーターで指定されたコマンドまたは *FilePath* パラメーターで指定されスクリプトの前に、バックグラウンド ジョブ用に作成されたセッションで実行されます。
初期化スクリプトを使用すると、ファイル、関数、またはエイリアスの追加、ディレクトリの作成、前提条件の確認など、セッションを構成することができます。

プライマリ ジョブ コマンドを実行するスクリプトを指定するには、 *FilePath* パラメーターを使用します。

終了しないエラーなど、初期化スクリプトがエラーを生成した場合、スケジュールされたジョブの現在のインスタンスは実行されず、状態は Failed になります。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject
変更するスケジュールされたジョブを指定します。
**ScheduledJobDefinition** オブジェクトを含む変数を入力するか、Get-ScheduledJob コマンドなどの **ScheduledJobDefinition** オブジェクトを取得するコマンドまたは式を入力します。
パイプを使用して **ScheduledJobDefinition** オブジェクトを **設定** することもできます。

複数のスケジュールされたジョブを指定すると、 **Set-ScheduledJob** はすべてのジョブに同じ変更を加えます。

```yaml
Type: Microsoft.PowerShell.ScheduledJob.ScheduledJobDefinition
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -MaxResultCount
スケジュールされたジョブに対して保持されるジョブの結果のエントリ数を指定します。
既定値は 32 です。

Windows PowerShell では、スケジュールされたジョブのトリガーされた各インスタンスの実行履歴と結果をディスクに保存します。
このパラメーターの値により、このスケジュールされたジョブに対して保存されるジョブ インスタンスの結果の数が決まります。
ジョブ インスタンスの結果の数がこの値を超えた場合は、最新のジョブ インスタンスの結果を保存するために、最も古いジョブ インスタンスの結果が削除されます。

ジョブの実行履歴とジョブの結果は、 \\ \<JobName\> \\ ジョブが作成されたコンピューターの $home \appdata\local\microsoft\windows\powershell\scheduledjobs \ 出力ディレクトリに保存され \<Timestamp\> ます。
実行履歴を表示するには、Get-Job コマンドレットを使用します。
ジョブの結果を取得するには、Receive-Job コマンドレットを使用します。

*MaxResultCount* パラメーターは、スケジュールされたジョブの ExecutionHistoryLength プロパティの値を設定します。

現在の実行履歴とジョブの結果を削除するには、 *ClearExecutionHistory* パラメーターを使用します。

```yaml
Type: System.Int32
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name
スケジュールされたジョブの新しい名前とスケジュールされたジョブのインスタンスを指定します。
名前は、ローカル コンピューター上で一意である必要があります。

変更するスケジュールされたジョブを特定するには、 *InputObject* パラメーターを使用するか、スケジュールされたジョブを Get-ScheduledJob からスケジュールされたジョブにパイプを使用して **設定** します。

このパラメーターによって、ディスク上のジョブ インスタンスの名前が変更されることはありません。
このコマンドが完了した後に開始されるジョブ インスタンスのみが影響を受けます。

```yaml
Type: System.String
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru
作業中の項目を表すオブジェクトを返します。
既定では、このコマンドレットによる出力はありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunAs32
スケジュールされたジョブを 32 ビット プロセスで実行します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunEvery

ジョブを実行する頻度を指定するために使用します。 たとえば、15分ごとにジョブを実行するには、このオプションを使用します。

```yaml
Type: System.TimeSpan
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunNow
**Set scheduledjob** コマンドレットが実行されるとすぐに、ジョブを直ちに開始します。
このパラメーターを使用すると、登録後すぐにタスク スケジューラをトリガーして Windows PowerShell スクリプトを実行する必要がなくなります。また、ユーザーが開始日時を指定したトリガーを作成する必要もありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Get-scheduledjoboption
スケジュールされたジョブのオプションを設定します。
New-ScheduledJobOption コマンドレットを使用して作成したオブジェクトやハッシュテーブルの値など、 **ScheduledJobOptions** オブジェクトを入力します。

スケジュールされたジョブを登録するときにスケジュールされたジョブのオプションを設定することも、Set-ScheduledJobOption または **Set ScheduledJob** コマンドレットを使用してオプションを設定または変更することもできます。

さまざまなオプションとその既定値により、スケジュールされたジョブが実行されるかどうかとそのタイミングが決まります。
ジョブをスケジュールする前に、必ずこれらのオプションを確認してください。
スケジュールされたジョブオプションの説明 (既定値を含む) については、「Get-scheduledjoboption」を参照してください。

ハッシュ テーブルを渡すには、次のキーを使用します。
次のハッシュ テーブルには、キーとその既定値が示されています。

`@{# Power SettingsStartIfOnBattery=$False;StopIfGoingOnBattery=$True; WakeToRun=$False; # Idle SettingsStartIfNotIdle=$False; IdleDuration="00:10:00"; IdleTimeout="01:00:00"; StopIfGoingOffIdle=$True; RestartOnIdleResume=$False;# Security settingsShowInTaskScheduler=$TrueRunElevated=$False;# MiscRunWithoutNetwork=$False;DoNotAllowDemandStart=$False;MultipleInstancePolicy=IgnoreNew# Can be IgnoreNew, Parallel, Queue, StopExisting}`

```yaml
Type: Microsoft.PowerShell.ScheduledJob.ScheduledJobOptions
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptBlock
スケジュールされたジョブで実行するコマンドを指定します。
コマンドを中かっこ ({ }) で囲み、スクリプト ブロックを作成します。
コマンド パラメーターの既定値を指定するには、 *ArgumentList* パラメーターを使用します。

すべての Register-ScheduledJob コマンドで、 *ScriptBlock* パラメーターと *FilePath* パラメーターのどちらかを使用する必要があります。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ScriptBlock
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -トリガー
スケジュールされたジョブのトリガーを指定します。
New-JobTrigger コマンドレットが返すオブジェクトや、ジョブトリガーのキーと値のハッシュテーブルなど、1つまたは複数の **Scheduledjobtrigger** オブジェクトを入力します。

ジョブトリガーは、スケジュールされたジョブを1回だけ、または定期的に、またはイベントが発生したときに自動的に開始します。

ジョブ トリガーは省略可能です。
スケジュールされたジョブを作成するときにトリガーを追加したり、Add-JobTrigger または **Set ScheduledJob** コマンドレットを使用してトリガーを後で追加したり、Start-Job コマンドレットを使用してスケジュールされたジョブをすぐに開始したりすることができます。
また、ジョブ トリガーのないスケジュールされたジョブを作成し、管理することもできます。

ハッシュ テーブルを渡すには、次のキーを使用します。

`@{Frequency="Once" (or Daily, Weekly, AtStartup, AtLogon);At="3am"` (または任意の有効な時刻文字列)。 `DaysOfWeek="Monday", "Wednesday"` (または曜日名の任意の組み合わせ)。 `Interval=2` (または任意の有効な頻度の間隔)。 `RandomDelay="30minutes"` (または任意の有効な timespan 文字列)。 `User="Domain1\User01"` (または任意の有効なユーザー。 AtLogon frequency 値と共にのみ使用されます)

}

```yaml
Type: Microsoft.PowerShell.ScheduledJob.ScheduledJobTrigger[]
Parameter Sets: ScriptBlock, FilePath
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### ScheduledJobDefinition (Microsoft PowerShell)
パイプを使用して、スケジュールされたジョブを **Set-ScheduledJob** に渡すことができます。

## 出力

### [なし] または [ScheduledJobDefinition]
*Passthru* パラメーターを使用すると、 **Set-ScheduledJob** は変更されたスケジュールされたジョブを返します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

## 関連リンク

[Add-JobTrigger](Add-JobTrigger.md)

[Disable-JobTrigger](Disable-JobTrigger.md)

[Disable-ScheduledJob](Disable-ScheduledJob.md)

[Enable-JobTrigger](Enable-JobTrigger.md)

[Enable-ScheduledJob](Enable-ScheduledJob.md)

[Get-JobTrigger](Get-JobTrigger.md)

[Get-ScheduledJob](Get-ScheduledJob.md)

[Get-ScheduledJobOption](Get-ScheduledJobOption.md)

[New-JobTrigger](New-JobTrigger.md)

[New-ScheduledJobOption](New-ScheduledJobOption.md)

[Register-ScheduledJob](Register-ScheduledJob.md)

[Remove-JobTrigger](Remove-JobTrigger.md)

[Set-JobTrigger](Set-JobTrigger.md)

[Set-ScheduledJob](Set-ScheduledJob.md)

[Set-ScheduledJobOption](Set-ScheduledJobOption.md)

[Unregister-ScheduledJob](Unregister-ScheduledJob.md)
