---
description: スケジュールされたジョブに関する問題の解決方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/psscheduledjob/about/about_scheduled_jobs_troubleshooting?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Scheduled_Jobs_Troubleshooting
ms.openlocfilehash: aac2133cee4abdd7e50e7b433104b9578d74b0a8
ms.sourcegitcommit: 1dfd5554b70c7e8f4e3df19e29c384a9c0a4b227
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101685877"
---
# <a name="about-scheduled-jobs-troubleshooting"></a>スケジュールされたジョブのトラブルシューティングについて

## <a name="short-description"></a>簡単な説明

スケジュールされたジョブに関する問題の解決方法について説明します。

## <a name="long-description"></a>長い説明

このドキュメントでは、PowerShell のスケジュールされたジョブ機能を使用するときに発生する可能性がある問題のいくつかについて説明し、これらの問題に対する解決策を提案します。

PowerShell のスケジュールされたジョブを使用する前に、「 [about_Scheduled_Jobs](about_Scheduled_Jobs.md) 」および関連するスケジュールされたジョブに関するトピックを参照してください。

**Psscheduledjob** モジュールに含まれるコマンドレットの詳細については、「 [psscheduledjob](xref:PSScheduledJob)」を参照してください。

## <a name="cant-find-job-results"></a>ジョブの結果が見つかりません

### <a name="basic-method-for-getting-job-results-in-powershell"></a>PowerShell でジョブ結果を取得するための基本的な方法

スケジュールされたジョブを実行すると、スケジュールされたジョブのインスタンスが作成されます。 スケジュールされたジョブインスタンスの結果を表示、管理、および取得するには、Job コマンドレットを使用します。

> [!NOTE]
> スケジュールされたジョブのインスタンスでジョブコマンドレットを使用するには、 **Psscheduledjob** モジュールをセッションにインポートする必要があります。 **Psscheduledjob** モジュールをインポートするには、などの `Import-Module PSScheduledJob` スケジュールされたジョブコマンドレットを入力するか、使用し `Get-ScheduledJob` ます。

スケジュールされたジョブのすべてのインスタンスの一覧を取得するには、コマンドレットを使用し `Get-Job` ます。

```powershell
Import-Module PSScheduledJob
Get-Job ProcessJob
```

```Output
Id     Name         PSJobTypeName   State         HasMoreData     Location
--     ----         -------------   -----         -----------     --------
43     ProcessJob   PSScheduledJob  Completed     False           localhost
44     ProcessJob   PSScheduledJob  Completed     False           localhost
45     ProcessJob   PSScheduledJob  Completed     False           localhost
46     ProcessJob   PSScheduledJob  Completed     False           localhost
47     ProcessJob   PSScheduledJob  Completed     False           localhost
48     ProcessJob   PSScheduledJob  Completed     False           localhost
49     ProcessJob   PSScheduledJob  Completed     False           localhost
50     ProcessJob   PSScheduledJob  Completed     False           localhost
```

コマンドレットでは、 `Get-Job` **プロセスジョブ** オブジェクトをパイプラインの下に送信します。 コマンドレットでは、 `Format-Table` スケジュールされたジョブインスタンスの **Name**、 **ID**、および **psbegintime** プロパティをテーブルに表示します。

```powershell
Get-Job ProcessJob | Format-Table -Property Name, ID, PSBeginTime -Auto
```

```Output
Name       Id PSBeginTime
----       -- ---------
ProcessJob 43 11/2/2011 3:00:02 AM
ProcessJob 44 11/3/2011 3:00:02 AM
ProcessJob 45 11/4/2011 3:00:02 AM
ProcessJob 46 11/5/2011 3:00:02 AM
ProcessJob 47 11/6/2011 3:00:02 AM
ProcessJob 48 11/7/2011 12:00:01 AM
ProcessJob 49 11/7/2011 3:00:02 AM
ProcessJob 50 11/8/2011 3:00:02 AM
```

スケジュールされたジョブのインスタンスの結果を取得するには、コマンドレットを使用し `Receive-Job` ます。 次のコマンドは、ProcessJob (ID = 50) の最新のインスタンスの結果を取得します。

```powershell
Receive-Job -ID 50
```

### <a name="basic-method-for-finding-job-results-on-disk"></a>ディスク上のジョブ結果を検索するための基本的な方法

スケジュールされたジョブを管理するには、やなどのジョブコマンドレットを使用し `Get-Job` `Receive-Job` ます。

`Get-Job`がジョブインスタンスを取得しない場合、またはジョブの結果を取得しない場合は、 `Receive-Job` 実行履歴ファイルでディスク上のジョブを検索できます。
実行履歴には、トリガーされたすべてのジョブインスタンスのレコードが含まれます。

スケジュールされたジョブのディレクトリに、次のパスにタイムスタンプ付きのディレクトリがあることを確認します。

`$home\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJob\<ScheduledJobName>\Output`

次に例を示します。

`C:\Users<UserName>\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJob\<ScheduledJobName>\Output`

たとえば、 `Get-ChildItem` コマンドレットは、スケジュールされたジョブ **processjob** のディスク上の実行履歴を取得します。

```powershell
$Path = '$home\AppData\Local\Microsoft\Windows\PowerShell'
$Path += '\ScheduledJobs\ProcessJob\Output'
Get-ChildItem $Path
```

```Output
Directory: C:\Users\User01\AppData\Local\Microsoft\Windows\PowerShell
               \ScheduledJobs\ProcessJob\Output

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         11/2/2011   3:00 AM            20111102-030002-260
d----         11/3/2011   3:00 AM            20111103-030002-277
d----         11/4/2011   3:00 AM            20111104-030002-209
d----         11/5/2011   3:00 AM            20111105-030002-251
d----         11/6/2011   3:00 AM            20111106-030002-174
d----         11/7/2011  12:00 AM            20111107-000001-914
d----         11/7/2011   3:00 AM            20111107-030002-376
```

各タイムスタンプの名前付きディレクトリは、ジョブインスタンスを表します。 各ジョブインスタンスの結果は、タイムスタンプが指定されたディレクトリの **Results.xml** ファイルに保存されます。

たとえば、次のコマンドは、スケジュールされた **Processjob** ジョブのすべての保存済みインスタンスの **Results.xml** ファイルを取得します。 **Results.xml** ファイルがない場合、PowerShell はジョブの結果を返すことも表示することもできません。

```powershell
$Path = '$home\AppData\Local\Microsoft\Windows\PowerShell'
$Path += '\ScheduledJobs\ProcessJob\Output\*\Results.xml'
Get-ChildItem $Path
```

```Output
Directory: C:\Users\User01\Appdata\Local\Microsoft\Windows\PowerShell
               \ScheduledJobs\ProcessJob\Output
```

ジョブコマンドレットは、 **Psscheduledjob** モジュールがセッションにインポートされていないため、スケジュールされたジョブインスタンスやその結果を取得できない場合があります。

> [!NOTE]
> スケジュールされたジョブインスタンスでジョブコマンドレットを使用する前に、 **Psscheduledjob** モジュールがセッションに含まれていることを確認します。 **Psscheduledjob** モジュールがないと、ジョブコマンドレットはスケジュールされたジョブインスタンスやその結果を取得できません。

**Psscheduledjob** モジュールをインポートするには、次のようにします。

```powershell
Import-Module PSScheduledJob
```

### <a name="receive-job-cmdlet-might-already-have-returned-the-results"></a>Receive-Job コマンドレットは既に結果を返している可能性があります

`Receive-Job`がジョブインスタンスの結果を返さない場合は、現在の `Receive-Job` セッションで、 **Keep** パラメーターを指定せずにそのジョブインスタンスに対してコマンドが実行されたことが原因である可能性があります。

`Receive-Job` **Keep** パラメーターを指定せずにを使用すると、ジョブの `Receive-Job` 結果が返され、ジョブインスタンスの **Hasのデータ** プロパティが **False** に設定されます。 **False** 値は、がジョブの結果を返し、インスタンスに返される結果がなくなったことを意味し `Receive-Job` ます。 この設定は、標準のバックグラウンドジョブに適していますが、ディスクに保存されているスケジュールされたジョブのインスタンスには適していません。

ジョブインスタンスの結果を再度取得するには、「」と入力して、新しい PowerShell セッションを開始し `PowerShell` ます。 **Psscheduledjob** モジュールをインポートしてから、コマンドを再試行してください `Receive-Job` 。

```powershell
Receive-Job -ID 50
```

```Output
#No results
```

```powershell
PowerShell.exe
```

```Output
Windows PowerShell
Copyright (C) 2012 Microsoft Corporation. All rights reserved.
```

```powershell
Import-Module PSScheduledJob
Receive-Job -ID 50
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id  ProcessName
-------  ------    -----      ----- -----   ------     --  -----------
1213         33    12348      21676    88    25.71   1608  CcmExec
29            4     1168       2920    43     0.02    748  conhost
46            6     2208       4612    45     0.03   1640  conhost
```

### <a name="using-keep-parameter-to-get-results-more-than-one-time-in-a-session"></a>Keep パラメーターを使用してセッションで結果を複数回取得する

1つのセッションでジョブインスタンスの結果を複数回取得するには、コマンドレットの **Keep** パラメーターを使用し `Receive-Job` ます。

```powershell
Import-Module PSScheduledJob
Receive-Job -ID 50 -Keep
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id  ProcessName
-------  ------    -----      ----- -----   ------     --  -----------
1213         33    12348      21676    88    25.71   1608  CcmExec
29            4     1168       2920    43     0.02    748  conhost
46            6     2208       4612    45     0.03   1640  conhost
```

```powershell
Receive-Job -ID 50 -Keep
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id  ProcessName
-------  ------    -----      ----- -----   ------     --  -----------
1213         33    12348      21676    88    25.71   1608  CcmExec
29            4     1168       2920    43     0.02    748  conhost
46            6     2208       4612    45     0.03   1640  conhost
```

### <a name="the-scheduled-job-might-be-corrupted"></a>スケジュールされたジョブが破損している可能性があります

スケジュールされたジョブが破損した場合、PowerShell は、破損したスケジュールされたジョブとその結果を削除します。 破損したスケジュールされたジョブの結果を回復することはできません。

スケジュールされたジョブがまだ存在するかどうかを判断するには、コマンドレットを使用し `Get-ScheduledJob` ます。

```powershell
Get-ScheduledJob
```

### <a name="the-number-of-results-might-have-exceeded-the-executionhistorylength"></a>結果の数が Executionhistory Length を超えている可能性があります

スケジュールされたジョブの **Executionhistory length** プロパティによって、ディスクに保存されるジョブインスタンスの数とその結果が決まります。 既定値は 32 です。
スケジュールされたジョブのインスタンス数がこの値を超えると、PowerShell によって最も古いジョブインスタンスが削除され、新しいジョブインスタンスごとに領域が確保されます。

スケジュールされたジョブの **Executionhistory length** プロパティの値を取得するには、次のコマンド形式を使用します。

```powershell
(Get-ScheduledJob <JobName>).ExecutionHistoryLength
```

たとえば、次のコマンドは、スケジュールされた **processjob** ジョブの **executionhistory length** プロパティの値を取得します。

```powershell
(Get-ScheduledJob ProcessJob).ExecutionHistoryLength
```

**Executionhistory length** プロパティの値を設定または変更するには、コマンドレットとコマンドレットの **maxresultcount** パラメーターを使用し `Register-ScheduledJob` `Set-ScheduledJob` ます。

次のコマンドは、 **Executionhistory length** プロパティの値を50に増やします。

```powershell
Get-ScheduledJob ProcessJob | Set-ScheduledJob -MaxResultCount 50
```

### <a name="the-job-instance-results-might-have-been-deleted"></a>ジョブインスタンスの結果が削除された可能性があります

コマンドレットの **ClearExecutionHistory** パラメーターは、 `Set-ScheduledJob` ジョブの実行履歴を削除します。 この機能を使用すると、ディスク領域を解放したり、不要な結果を削除したり、既に使用、分析、または別の場所に保存された結果を削除したりすることができます。

スケジュールされたジョブの実行履歴を削除するには、スケジュールされたジョブの **ClearExecutionHistory** パラメーターを使用します。

次のコマンドは、スケジュールされたジョブ **processjob** の実行履歴を削除します。

```powershell
Get-ScheduledJob ProcessJob | Set-ScheduledJob -ClearExecutionHistory
```

また、 `Remove-Job` コマンドレットはジョブの結果を削除します。 を使用してスケジュールされたジョブを削除すると、 `Remove-Job` 実行履歴とすべてのジョブ結果を含む、ディスク上のジョブのすべてのインスタンスが削除されます。

### <a name="jobs-started-by-using-the-start-job-cmdlet-are-not-saved-to-disk"></a>Start-Job コマンドレットを使用して開始されたジョブがディスクに保存されない

を使用し `Start-Job` てスケジュールされたジョブを開始すると、ジョブトリガーを使用するのではなく、 `Start-Job` 標準のバックグラウンドジョブが開始されます。 バックグラウンドジョブとその結果は、ディスク上のジョブの実行履歴には格納されません。

コマンドレットを使用してジョブを取得し、コマンドレットを使用してジョブの結果を取得することはでき `Get-Job` `Receive-Job` ますが、コマンドレットの Keep パラメーターを使用しない限り、結果を受け取るまでは使用できません `Receive-Job` 。

また、バックグラウンドジョブとその結果はセッションに固有です。作成されたセッションにのみ存在します。 でジョブを削除した場合 `Remove-Job` は、セッションを閉じるか PowerShell を閉じると、ジョブインスタンスとその結果が削除されます。

## <a name="scheduled-job-doesnt-run"></a>スケジュールされたジョブは実行されません

ジョブがトリガーされた場合、またはスケジュールされたジョブが無効になっている場合は、スケジュールされたジョブが自動的に実行

コマンドレットを使用し `Get-ScheduledJob` て、スケジュールされたジョブを取得します。 スケジュールされたジョブの **Enabled** プロパティの値が **True** であることを確認します。

```powershell
Get-ScheduledJob ProcessJob
```

```Output
Id         Name            Triggers        Command         Enabled
--         ----            --------        -------         -------
4          ProcessJob      {1, 2}          Get-Process     True
```

```powershell
(Get-ScheduledJob ProcessJob).Enabled
```

```Output
True
```

コマンドレットを使用して、スケジュールされた `Get-JobTrigger` ジョブのジョブトリガーを取得します。
ジョブトリガーの **Enabled** プロパティの値が **True** であることを確認します。

```powershell
Get-ScheduledJob ProcessJob | Get-JobTrigger
```

```Output
Id      Frequency    Time                   DaysOfWeek            Enabled
--      ---------    ----                   ----------            -------
1       Weekly       11/7/2011 5:00:00 AM   {Monday, Thursday}    True
2       Daily        11/7/2011 3:00:00 PM                         True
```

```powershell
Get-ScheduledJob ProcessJob|Get-JobTrigger|Format-Table ID, Enabled -Auto
```

```Output
Id Enabled
-- -------
1    True
2    True
```

### <a name="scheduled-jobs-dont-run-automatically-if-job-triggers-are-invalid"></a>ジョブトリガーが無効な場合、スケジュールされたジョブが自動的に実行されない

たとえば、ジョブトリガーでは、過去の日付、または月の5番目の月曜日などの発生していない日付を指定できます。

スケジュールされたジョブは、ジョブトリガーの条件またはジョブオプションが満たされていない場合、自動的には実行されません。

たとえば、特定のユーザーがコンピューターにログオンしたときにのみ実行されるスケジュールされたジョブは、そのユーザーがログオンしていない場合、またはリモートでのみ接続する場合には実行されません。

スケジュールされたジョブのオプションを確認し、それらが満たされていることを確認します。
たとえば、コンピューターがアイドル状態であるか、ネットワーク接続が必要であるか、または長い **IdleDuration** または Brief の **IdleTimeout** が実行されていない場合に、スケジュールされたジョブが実行されることはありません。

コマンドレットを使用して、 `Get-ScheduledJobOption` ジョブオプションとその値を確認します。

```powershell
Get-ScheduledJobOption -Name ProcessJob
```

```Output
StartIfOnBatteries     : False
StopIfGoingOnBatteries : True
WakeToRun              : True
StartIfNotIdle         : True
StopIfGoingOffIdle     : False
RestartOnIdleResume    : False
IdleDuration           : 00:10:00
IdleTimeout            : 01:00:00
ShowInTaskScheduler    : True
RunElevated            : False
RunWithoutNetwork      : True
DoNotAllowDemandStart  : False
MultipleInstancePolicy : IgnoreNew
JobDefinition          : Microsoft.PowerShell.ScheduledJob.ScheduledJobDefinition
```

スケジュールされたジョブオプションの説明については、「 [get-scheduledjoboption](xref:PSScheduledJob.New-ScheduledJobOption)」を参照してください。

### <a name="the-scheduled-job-instance-might-have-failed"></a>スケジュールされたジョブインスタンスが失敗した可能性があります

スケジュールされたジョブコマンドが失敗した場合、PowerShell はエラーメッセージを生成してすぐにレポートを報告します。 ただし、タスクスケジューラによって実行が試行されたときにジョブが失敗した場合、PowerShell でエラーを使用することはできません。

ジョブの失敗を検出して修正するには、次の方法を使用します。

タスクスケジューライベントログでエラーを確認します。 ログを確認するには、イベントビューアーまたは次のような PowerShell コマンドを使用します。

```powershell
Get-WinEvent -LogName Microsoft-Windows-TaskScheduler/Operational |
 Where {$_.Message -like "fail"}
```

タスクスケジューラのジョブレコードを確認します。 PowerShell のスケジュールされたジョブは、次のタスクのスケジュールされたフォルダーに格納されます。

`Task Scheduler Library\Microsoft\Windows\PowerShell\ScheduledJobs`

### <a name="the-scheduled-job-might-not-run-because-of-insufficient-permission"></a>十分な権限がないため、スケジュールされたジョブが実行されない可能性があります

スケジュールされたジョブは、ジョブを作成したユーザーのアクセス許可か、またはコマンドで **Credential** パラメーターによって指定されたユーザーのアクセス許可を使用して実行され `Register-ScheduledJob` `Set-ScheduledJob` ます。

コマンドまたはスクリプトを実行する権限がユーザーに与えられていない場合、ジョブは失敗します。

## <a name="cant-get-scheduled-job-or-scheduled-job-is-corrupted"></a>スケジュールされたジョブを取得できないか、スケジュールされたジョブが破損しています

まれに、スケジュールされたジョブが破損したり、解決できない内部矛盾削除が含まれたりすることがあります。 通常、このエラーは、スケジュールされたジョブの XML ファイルが手動で編集され、XML が無効な場合に発生します。

スケジュールされたジョブが破損した場合、PowerShell は、スケジュールされたジョブ、その実行履歴、およびディスクからの結果を削除しようとします。

スケジュールされたジョブを削除できない場合は、コマンドレットを実行するたびに、破損したジョブのエラーメッセージが表示され `Get-ScheduledJob` ます。

破損したスケジュールされたジョブを削除するには、次のいずれかの方法を使用します。

スケジュールされた `<ScheduledJobName>` ジョブのディレクトリを削除します。 **Scheduledjob** ディレクトリは削除しないでください。

ディレクトリの場所:

`$env:UserProfile\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJobs<ScheduledJobName>`

次に例を示します。

`C:\Users<UserName>\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJobs<ScheduledJobName>.`

スケジュールされたジョブを削除するには、タスクスケジューラを使用します。 PowerShell のスケジュールされたタスクは、次のタスクスケジューラパスに表示されます。

`Task Scheduler Library\Microsoft\Windows\PowerShell\ScheduledJobs<ScheduledJobName>`

## <a name="job-cmdlets-cant-consistently-find-scheduled-jobs"></a>ジョブコマンドレットがスケジュールされたジョブを一貫して検出できない

**Psscheduledjob** モジュールが現在のセッションにない場合、ジョブコマンドレットは、スケジュールされたジョブを取得したり、開始したり、結果を取得したりすることはできません。

**Psscheduledjob** モジュールをインポートするには、コマンドレットなど、モジュールのコマンドレットを入力するか、 `Import-Module PSScheduledJob` または実行または取得し `Get-ScheduledJob` ます。
PowerShell 3.0 以降では、モジュールのコマンドレットを取得または使用すると、モジュールは自動的にインポートされます。

**Psscheduledjob** モジュールが現在のセッションにない場合、次のコマンドシーケンスを実行できます。

```powershell
Get-Job ProcessJob
```

```Output
Get-Job : The command cannot find the job because the job name
ProcessJob was not found.
Verify the value of the Name parameter, and then try the command again.
+ CategoryInfo          : ObjectNotFound: (ProcessJob:String) [Get-Job],
PSArgumentException
+ FullyQualifiedErrorId : JobWithSpecifiedNameNotFound,Microsoft.PowerShell.
Commands.GetJobCommand
```

```powershell
Get-Job
Get-ScheduledJob ProcessJob
```

```Output
Id         Name            Triggers        Command      Enabled
--         ----            --------        -------      -------
4          ProcessJob      {1}             Get-Process  True
```

```powershell
Get-Job ProcessJob
```

```Output
Id     Name         PSJobTypeName   State       HasMoreData     Location
--     ----         -------------   -----       -----------     --------
43     ProcessJob   PSScheduledJob  Completed   True            localhost
44     ProcessJob   PSScheduledJob  Completed   True            localhost
45     ProcessJob   PSScheduledJob  Completed   True            localhost
46     ProcessJob   PSScheduledJob  Completed   True            localhost
47     ProcessJob   PSScheduledJob  Completed   True            localhost
48     ProcessJob   PSScheduledJob  Completed   True            localhost
49     ProcessJob   PSScheduledJob  Completed   True            localhost
50     ProcessJob   PSScheduledJob  Completed   True            localhost
```

この動作は、コマンドによって `Get-ScheduledJob` **Psscheduledjob** モジュールが自動的にインポートされ、コマンドが実行されるために発生します。

## <a name="see-also"></a>関連項目

[about_Scheduled_Jobs_Basics](about_Scheduled_Jobs_Basics.md)

[about_Scheduled_Jobs_Advanced](about_Scheduled_Jobs_Advanced.md)

[about_Scheduled_Jobs](about_Scheduled_Jobs.md)

[Psscheduledjob](xref:PSScheduledJob) モジュールのコマンドレット

[タスク スケジューラ](/windows/desktop/TaskSchd/task-scheduler-reference)
