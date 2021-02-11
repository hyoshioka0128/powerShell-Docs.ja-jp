---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 07/26/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/remove-job?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-Job
ms.openlocfilehash: 52613dae3ba73227818c6c0dec40183ba20ce2a3
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603238"
---
# Remove-Job

## 概要
PowerShell バックグラウンドジョブを削除します。

## SYNTAX

### SessionIdParameterSet (既定値)

```
Remove-Job [-Force] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### JobParameterSet

```
Remove-Job [-Job] <Job[]> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameParameterSet

```
Remove-Job [-Force] [-Name] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceIdParameterSet

```
Remove-Job [-Force] [-InstanceId] <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### FilterParameterSet

```
Remove-Job [-Force] [-Filter] <Hashtable> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### StateParameterSet

```
Remove-Job [-State] <JobState> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### CommandParameterSet

```
Remove-Job [-Command <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットは、 `Remove-Job` コマンドレットによって開始された PowerShell バックグラウンドジョブ、 `Start-Job` または `Invoke-Command` **AsJob** パラメーターをサポートするなどのコマンドレットによって開始された PowerShell バックグラウンドジョブを削除します。

を使用すると、 `Remove-Job` すべてのジョブを削除したり、選択したジョブを削除したりできます。 ジョブは、 **名前**、 **ID**、 **インスタンス id**、 **コマンド**、または **状態** によって識別されます。 または、ジョブオブジェクトをパイプラインの下に送信することもでき `Remove-Job` ます。 パラメーターまたはパラメーター値が指定されていない場合、に `Remove-Job` よる影響はありません。

PowerShell 3.0 以降、では、 `Remove-Job` スケジュールされたジョブやワークフロージョブなどのカスタムジョブの種類を削除できます。 たとえば、は、 `Remove-Job` スケジュールされたジョブ、ディスク上のスケジュールされたジョブのすべてのインスタンス、およびトリガーされたすべてのジョブインスタンスの結果を削除します。

実行中のジョブを削除しようとすると、は `Remove-Job` 失敗します。 `Stop-Job`コマンドレットを使用して、実行中のジョブを停止します。 または、 `Remove-Job` **Force** パラメーターと共にを使用して、実行中のジョブを削除します。

バックグラウンドジョブを削除するか、PowerShell セッションを閉じるまで、ジョブはグローバルジョブキャッシュに残ります。

## 例

### 例 1: 名前を使用してジョブを削除する

この例では、変数とパイプラインを使用して、名前を指定してジョブを削除します。

```powershell
$batch = Get-Job -Name BatchJob
$batch | Remove-Job
```

`Get-Job`**Name** パラメーターを使用して、ジョブ **batchjob** を指定します。 ジョブオブジェクトは変数に格納され `$batch` ます。 のオブジェクト `$batch` は、パイプラインの下にに送信され `Remove-Job` ます。

別の方法として、などの **ジョブ** パラメーターを使用することも `Remove-Job -Job $batch` できます。

### 例 2: セッション内のすべてのジョブを削除する

この例では、現在の PowerShell セッションのすべてのジョブが削除されます。

```powershell
Get-job | Remove-Job
```

`Get-Job` 現在の PowerShell セッションのすべてのジョブを取得します。 ジョブオブジェクトは、パイプラインからに送信され `Remove-Job` ます。

### 例 3: NotStarted ジョブを削除する

この例では、開始されていない現在の PowerShell セッションからすべてのジョブを削除します。

```powershell
Remove-Job -State NotStarted
```

`Remove-Job`**State** パラメーターを使用して、ジョブの状態を指定します。

### 例 4: フレンドリ名を使用してジョブを削除する

この例では、現在のセッションから、実行中のジョブを含め、* batch * * で終わるすべてのジョブを削除します。

```powershell
Remove-Job -Name *batch -Force
```

`Remove-Job`**name** パラメーターを使用してジョブ名のパターンを指定します。 このパターンには、 `*` **batch** で終了するすべてのジョブ名を検索するためのアスタリスク () ワイルドカードが含まれています。 **Force** パラメーターは、を実行しているジョブを削除します。

### 例 5: Invoke-Command によって作成されたジョブを削除する

次の例では、を使用してリモートコンピューターで開始されたジョブを `Invoke-Command` **AsJob** パラメーターと共に削除します。

この例では **AsJob** パラメーターを使用しているため、ジョブオブジェクトはローカルコンピューター上に作成されます。
ただし、ジョブはリモートコンピューター上で実行されます。 その結果、ジョブを管理するには、ローカルのコマンドを使用することになります。

```powershell
$job = Invoke-Command -ComputerName Server01 -ScriptBlock {Get-Process} -AsJob
$job | Remove-Job
```

`Invoke-Command`**Server01** コンピューターでジョブを実行します。 **AsJob** パラメーターは、 **ScriptBlock** をバックグラウンドジョブとして実行します。 ジョブオブジェクトは変数に格納され `$job` ます。 `$job`変数オブジェクトは、パイプラインの下にに送信され `Remove-Job` ます。

### 例 6: Invoke-Command と Start-Job によって作成されたジョブを削除する

この例では、を使用して起動されたリモートコンピューター上のジョブを実行する方法を示し `Invoke-Command` `Start-Job` ます。 ジョブオブジェクトはリモートコンピューター上に作成され、リモートコマンドを使用してジョブを管理します。 リモートコマンドを実行する場合は、永続的な接続が必要です `Start-Job` 。

```powershell
$S = New-PSSession -ComputerName Server01
Invoke-Command -Session $S -ScriptBlock {Start-Job -ScriptBlock {Get-Process} -Name MyJob}
Invoke-Command -Session $S -ScriptBlock {Remove-Job -Name MyJob}
```

`New-PSSession`**Server01** コンピューターへの永続的な接続である **PSSession** を作成します。 接続は変数に保存され `$S` ます。

`Invoke-Command` に保存されているセッションに接続 `$S` します。 **ScriptBlock** は、を使用して `Start-Job` リモートジョブを開始します。 ジョブはコマンドを実行 `Get-Process` し、 **Name** パラメーターを使用して、わかりやすいジョブ名 **myjob** を指定します。

`Invoke-Command` は、 `$S` セッションを使用してを実行 `Remove-Job` します。 **Name** パラメーターは、 **myjob** という名前のジョブが削除されることを指定します。

### 例 7: InstanceId を使用してジョブを削除する

この例では、 **InstanceId** に基づいてジョブを削除します。

```powershell
$job = Start-Job -ScriptBlock {Get-Process PowerShell}
$job | Format-List -Property *
Remove-Job -InstanceId ad02b942-8007-4407-87f3-d23e71955872
```

```Output
State         : Completed
HasMoreData   : True
StatusMessage :
Location      : localhost
Command       : Get-Process PowerShell
JobStateInfo  : Completed
Finished      : System.Threading.ManualResetEvent
InstanceId    : ad02b942-8007-4407-87f3-d23e71955872
Id            : 3
Name          : Job3
ChildJobs     : {Job4}
PSBeginTime   : 7/26/2019 11:36:56
PSEndTime     : 7/26/2019 11:36:57
PSJobTypeName : BackgroundJob
Output        : {}
Error         : {}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
Information   : {}
```

`Start-Job` バックグラウンドジョブを開始し、ジョブオブジェクトを変数に保存し `$job` ます。

のオブジェクト `$job` は、パイプラインの下にに送信され `Format-List` ます。 **Property** パラメーターはアスタリスク () を使用して `*` 、すべてのオブジェクトのプロパティがリストに表示されることを指定します。

`Remove-Job` では、 **InstanceId** パラメーターを使用して、削除するジョブを指定します。

## PARAMETERS

### -Command

指定された単語をコマンドに含んでいるジョブを削除します。 コンマ区切りの配列を入力できます。

```yaml
Type: System.String[]
Parameter Sets: CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Confirm

を実行する前に確認メッセージを表示 `Remove-Job` します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Filter

関連付けられたハッシュテーブルに設定されているすべての条件を満たすジョブを削除します。 ジョブのプロパティをキー、ジョブのプロパティ値を値とするハッシュ テーブルを入力します。

このパラメーターは、ワークフロー ジョブ、スケジュールされたジョブなどの、カスタムのジョブの種類に対してのみ機能します。 これは、を使用して作成されたものなど、標準のバックグラウンドジョブでは機能しません `Start-Job` 。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: FilterParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Force

ジョブの状態が **Running** の場合でも、ジョブを削除します。 **Force** パラメーターが指定されていない場合、は `Remove-Job` 実行中のジョブを削除しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: SessionIdParameterSet, JobParameterSet, InstanceIdParameterSet, NameParameterSet, FilterParameterSet
Aliases: F

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

指定された **Id** を持つバックグラウンドジョブを削除します。コンマ区切りの配列を入力できます。 ジョブの **Id** は、現在のセッション内のジョブを識別する一意の整数です。

ジョブの **Id** を検索するには、パラメーターを指定せずにを使用し `Get-Job` ます。

```yaml
Type: System.Int32[]
Parameter Sets: SessionIdParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InstanceId

指定した **InstanceId** のジョブを削除します。 コンマ区切りの配列を入力できます。 **InstanceId** は、ジョブを識別する一意の GUID です。

ジョブの **InstanceId** を検索するには、を使用 `Get-Job` します。

```yaml
Type: System.Guid[]
Parameter Sets: InstanceIdParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ジョブ

削除するジョブを指定します。 ジョブが格納されている変数、またはジョブを取得するコマンドを入力します。 コンマ区切りの配列を入力できます。

パイプラインの下にジョブオブジェクトを送信することができ `Remove-Job` ます。

```yaml
Type: System.Management.Automation.Job[]
Parameter Sets: JobParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Name

指定されたフレンドリ名を持つジョブのみを削除します。 ワイルドカードを使用できます。 コンマ区切りの配列を入力できます。

ジョブのフレンドリ名は、PowerShell セッション内でも一意であるとは限りません。 名前でファイルを削除する場合は、 **WhatIf** パラメーターと **Confirm** パラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -状態

は、指定された状態のジョブのみを削除します。 状態が **Running** のジョブを削除するには、 **Force** パラメーターを使用します。

許容される値:

- AtBreakpoint
- ［ブロック済み］
- 完了
- [Disconnected]\(切断済み\)
- Failed
- NotStarted
- 実行中
- 停止済み
- 停止中
- Suspended
- 中断中

```yaml
Type: System.Management.Automation.JobState
Parameter Sets: StateParameterSet
Aliases:
Accepted values: AtBreakpoint, Blocked, Completed, Disconnected, Failed, NotStarted, Running, Stopped, Stopping, Suspended, Suspending

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -WhatIf

が実行された場合に何が起こるか `Remove-Job` を示します。 コマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. ジョブ

パイプラインの下にジョブオブジェクトを送信することができ `Remove-Job` ます。

## 出力

### なし

`Remove-Job` 出力を生成しません。

## 注

PowerShell ジョブによって新しいプロセスが作成されます。 ジョブが完了すると、プロセスは終了します。 `Remove-Job`を実行すると、ジョブの状態が削除されます。

ジョブが完了前に停止し、そのプロセスが終了していない場合、プロセスは強制的に終了されます。

## 関連リンク

[about_Jobs](./About/about_Jobs.md)

[about_Job_Details](./About/about_Job_Details.md)

[about_Remote_Jobs](./About/about_Remote_Jobs.md)

[Get-Job](Get-Job.md)

[Invoke-Command](Invoke-Command.md)

[Receive-Job](Receive-Job.md)

[Start-Job](Start-Job.md)

[Stop-Job](Stop-Job.md)

[Wait-Job](Wait-Job.md)

