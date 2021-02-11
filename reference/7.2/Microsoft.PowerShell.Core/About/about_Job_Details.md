---
description: ローカルコンピューターとリモートコンピューターのバックグラウンドジョブの詳細について説明します。
Locale: en-US
ms.date: 10/16/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_job_details?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Job_Details
ms.openlocfilehash: 1ceb0be9cc140b69afdebaaf336dad75e482aa22
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603055"
---
# <a name="about-job-details"></a>ジョブの詳細について

## <a name="short-description"></a>簡単な説明
ローカルコンピューターとリモートコンピューターのバックグラウンドジョブの詳細について説明します。

## <a name="detailed-description"></a>詳しい説明

このトピックでは、バックグラウンドジョブの概念について説明し、PowerShell でのバックグラウンドジョブの動作に関する技術情報を提供します。

このトピックでは、 [about_Jobs](about_Jobs.md)、 [about_Thread_Jobs](about_Thread_Jobs.md)、および [about_Remote_Jobs](about_Remote_Jobs.md) に関するトピックを補足します。

### <a name="about-background-jobs"></a>バックグラウンドジョブについて

バックグラウンドジョブは、コマンドまたは式を非同期的に実行します。 コマンドレット、関数、スクリプト、またはその他のコマンドベースのタスクが実行されている可能性があります。 これは、長い時間を要するコマンドを実行するように設計されていますが、バックグラウンドで任意のコマンドを実行するために使用できます。

同期コマンドを実行すると、コマンドが完了するまで PowerShell コマンドプロンプトは表示されません。 ただし、バックグラウンドジョブでは PowerShell プロンプトは抑制されません。 バックグラウンドジョブを開始するコマンドは、ジョブオブジェクトを返します。
プロンプトはすぐに返されるので、バックグラウンドジョブの実行中に他のタスクを操作できます。

ただし、バックグラウンドジョブを開始すると、ジョブが非常に短時間で実行された場合でも、すぐに結果を取得することはできません。 返されるジョブオブジェクトには、ジョブに関する有用な情報が含まれていますが、ジョブの結果は含まれていません。 ジョブの結果を取得するには、別のコマンドを実行する必要があります。 また、コマンドを実行してジョブを停止し、ジョブが完了するまで待機して、ジョブを削除することもできます。

バックグラウンドジョブのタイミングを他のコマンドから独立させるには、各バックグラウンドジョブが独自の PowerShell セッションで実行されます。 ただし、これは、ジョブを実行するためだけに作成され、その後破棄される一時的な接続である場合もあれば、複数の関連ジョブやコマンドを実行するために使用できる永続的な **PSSession** である場合もあります。

### <a name="using-the-job-cmdlets"></a>Job コマンドレットの使用

コマンドを使用し `Start-Job` て、ローカルコンピューター上でバックグラウンドジョブを開始します。
`Start-Job` ジョブオブジェクトを返します。 コマンドレットを使用して、ローカルコンピューターで開始されたジョブを表すオブジェクトを取得することもでき `Get-Job` ます。

ジョブの結果を取得するには、コマンドを使用し `Receive-Job` ます。 ジョブが完了していない場合は、 `Receive-Job` 部分的な結果が返されます。 また、コマンドレットを使用して、 `Wait-Job` セッションで開始された1つまたはすべてのジョブが完了するまで、コマンドプロンプトを表示しないようにすることもできます。

バックグラウンドジョブを停止するには、 `Stop-Job` コマンドレットを使用します。 ジョブを削除するには、 `Remove-Job` コマンドレットを使用します。

コマンドレットの動作の詳細については、各コマンドレットのヘルプトピックを参照し、 [about_Jobs](about_Jobs.md)を参照してください。

### <a name="starting-background-jobs-on-remote-computers"></a>リモートコンピューターでバックグラウンドジョブを開始しています

ローカルコンピューターまたはリモートコンピューターでバックグラウンドジョブを作成および管理できます。 バックグラウンドジョブをリモートで実行するには、コマンドレットの **AsJob** パラメーター (など) を使用する `Invoke-Command` か、 `Invoke-Command` コマンドレットを使用して `Start-Job` コマンドをリモートで実行します。 対話型セッションでバックグラウンドジョブを開始することもできます。

リモートバックグラウンドジョブの詳細については、「 [about_Remote_Jobs](about_Remote_Jobs.md)」を参照してください。

### <a name="child-jobs"></a>子ジョブ

各バックグラウンドジョブは、親ジョブと1つ以上の子ジョブで構成されます。 またはの AsJob パラメーターを使用して開始されたジョブでは、 `Start-Job`  `Invoke-Command` 親ジョブは役員です。 コマンドを実行したり、結果を返したりすることはありません。 コマンドは、実際には子ジョブによって実行されます。 他のコマンドレットを使用して開始されたジョブの動作が異なる場合があります。

子ジョブは親ジョブオブジェクトの **childjobs** プロパティに格納されます。 **Childjobs** プロパティには、1つまたは複数の子ジョブオブジェクトを含めることができます。
子ジョブオブジェクトの **名前**、 **ID**、および **InstanceId** は親ジョブとは異なるので、親ジョブと子ジョブを個別に、または1つの単位として管理できます。

ジョブの親ジョブと子ジョブを取得するには、コマンドレットの **IncludeChildJobs** パラメーターを使用し `Get-Job` ます。 **IncludeChildJob** パラメーターは、Windows PowerShell 3.0 で導入されました。

```powershell
PS> Get-Job -IncludeChildJob

Id Name   PSJobTypeName State      HasMoreData   Location    Command
-- ----   ------------- -----      -----------   --------    -------
1  Job1   RemoteJob     Failed     True          localhost   Get-Process
2  Job2                 Completed  True          Server01    Get-Process
3  Job3                 Failed     False         localhost   Get-Process
```

親ジョブと特定の **状態** 値を持つ子ジョブのみを取得するには、コマンドレットの **childjobstate** パラメーターを使用し `Get-Job` ます。 **Childjobstate** パラメーターは、Windows PowerShell 3.0 で導入されました。

```powershell
PS> Get-Job -ChildJobState Failed

Id Name   PSJobTypeName State      HasMoreData   Location    Command
-- ----   ------------- -----      -----------   --------    -------
1  Job1   RemoteJob     Failed     True          localhost   Get-Process
3  Job3                 Failed     False         localhost   Get-Process
```

すべてのバージョンの PowerShell でジョブの子ジョブを取得するには、親ジョブの **childjob** プロパティを使用します。

```powershell
PS> (Get-Job Job1).ChildJobs

Id Name   PSJobTypeName State      HasMoreData   Location    Command
-- ----   ------------- -----      -----------   --------    -------
2  Job2                 Completed  True          Server01    Get-Process
3  Job3                 Failed     False         localhost   Get-Process
```

`Get-Job`次のコマンドに示すように、子ジョブでコマンドを使用することもできます。

```powershell
PS> Get-Job Job3

Id Name   PSJobTypeName State      HasMoreData   Location    Command
-- ----   ------------- -----      -----------   --------    -------
3  Job3                 Failed     False         localhost   Get-Process
```

子ジョブの構成は、ジョブを開始するために使用するコマンドによって異なります。

- を使用し `Start-Job` てローカルコンピューターでジョブを開始すると、ジョブは、コマンドを実行する executive 親ジョブと子ジョブで構成されます。

- の **AsJob** パラメーターを使用して `Invoke-Command` 1 つまたは複数のコンピューターでジョブを開始すると、ジョブは、各コンピューターで実行される各ジョブの役員親ジョブと子ジョブで構成されます。

- を使用して `Invoke-Command` 1 台以上のリモートコンピューターでコマンドを実行すると、 `Start-Job` 各リモートコンピューターで実行されるローカルコマンドと同じ結果になります。 このコマンドは、各コンピューターのジョブオブジェクトを返します。 ジョブオブジェクトは、コマンドを実行する役員親ジョブと1つの子ジョブで構成されます。

親ジョブは、すべての子ジョブを表します。 親ジョブを管理する場合は、関連付けられている子ジョブも管理します。 たとえば、親ジョブを停止すると、すべての子ジョブが停止します。 親ジョブの結果を取得すると、すべての子ジョブの結果が得られます。

ただし、子ジョブを個別に管理することもできます。 これは、ジョブに関する問題を調査する場合や、の **AsJob** パラメーターを使用して開始された複数の子ジョブの結果を取得する場合に最も役立ち `Invoke-Command` ます。

次のコマンドでは、の **AsJob** パラメーターを使用して、 `Invoke-Command` ローカルコンピューターと2台のリモートコンピューターでバックグラウンドジョブを開始します。 このコマンドは、ジョブを変数に保存し `$j` ます。

```powershell
PS> $j = Invoke-Command -ComputerName localhost, Server01, Server02 `
-Command {Get-Date} -AsJob
```

でジョブの Name および **childjob** プロパティを表示すると `$j` 、このコマンドによって、3つの子ジョブ (コンピューターごとに1つ) のジョブオブジェクトが返されたことが示されます。

```powershell
PS> $j | Format-List Name, ChildJobs

Name      : Job3
ChildJobs : {Job4, Job5, Job6}
```

親ジョブを表示すると、ジョブが失敗したことが表示されます。

```powershell
PS> $j

Id Name   PSJobTypeName State      HasMoreData   Location
-- ----   ------------- -----      -----------   --------
3  Job3   RemotingJob   Failed     False         localhost,Server...
```

ただし、 `Get-Job` 子ジョブを取得するコマンドを実行すると、1つの子ジョブが失敗したことが出力に示されます。

```powershell
PS> Get-Job -IncludeChildJobs

Id  Name   PSJobTypeName State      HasMoreData   Location    Command
--  ----   ------------- -----      -----------   --------    -------
3   Job3   RemotingJob   Failed     False         localhost,Server...
4   Job4                 Completed  True          localhost   Get-Date
5   Job5                 Failed     False         Server01    Get-Date
6   Job6                 Completed  True          Server02    Get-Date
```

すべての子ジョブの結果を取得するには、コマンドレットを使用して `Receive-Job` 親ジョブの結果を取得します。 ただし、次のコマンドに示すように、特定の子ジョブの結果を取得することもできます。

```powershell
PS> Receive-Job -Name Job6 -Keep | Format-Table ComputerName,
>> DateTime -AutoSize
ComputerName DateTime
------------ --------
Server02     Thursday, March 13, 2008 4:16:03 PM
```

PowerShell のバックグラウンドジョブの子ジョブ機能を使用すると、実行するジョブをより細かく制御できます。

### <a name="job-types"></a>ジョブの種類

PowerShell では、タスクごとに異なる種類のジョブをサポートしています。 Windows PowerShell 3.0 以降では、開発者は、新しいジョブの種類を PowerShell に追加する "ジョブソースアダプター" を作成し、モジュールにジョブソースアダプターを含めることができます。
モジュールをインポートするときに、セッションで新しいジョブの種類を使用できます。

たとえば、PSScheduledJob モジュールによってスケジュールされたジョブが追加され、PSWorkflow モジュールによってワークフロージョブが追加されます。

カスタムジョブの種類は、標準の PowerShell バックグラウンドジョブとは大きく異なる場合があります。 たとえば、スケジュールされたジョブはディスクに保存されます。これらは、特定のセッションにのみ存在しません。 ワークフロージョブは、中断して再開することができます。

カスタムジョブの管理に使用するコマンドレットは、ジョブの種類によって異なります。 場合によっては、やなどの標準のジョブコマンドレットを使用し `Get-Job` `Start-Job` ます。 特定の種類のジョブのみを管理する特殊なコマンドレットが付属しています。 カスタムのジョブの種類の詳細については、ジョブの種類に関するヘルプトピックを参照してください。

ジョブのジョブの種類を検索するには、 `Get-Job` コマンドレットを使用します。 `Get-Job` さまざまな種類のジョブに対して異なるジョブオブジェクトを返します。 が返すジョブオブジェクトの **PSJobTypeName** プロパティの値は、 `Get-Job` ジョブの種類を示します。

次の表に、PowerShell に付属するジョブの種類を示します。

|    ジョブの種類    |                       説明                        |
| -------------- | -------------------------------------------------------- |
| BackgroundJob  | コマンドレットの使用を開始しました `Start-Job` 。                    |
| RemoteJob      | の **AsJob** パラメーターの使用を開始しました             |
|                | `Invoke-Command` コマンドレットを使用します。                                 |
| PSWorkflowJob  | ワークフローの **AsJob** パラメーターの使用を開始しました。     |
| PSScheduledJob | ジョブトリガーによって開始されるスケジュールされたジョブのインスタンス。 |
| CIMJob         | からコマンドレットの **AsJob** パラメーターの使用を開始しました |
|                | CDXML モジュール。                                            |
| Wmi ジョブ         | からコマンドレットの **AsJob** パラメーターの使用を開始しました |
|                | WMI モジュール。                                              |
| PSEventJob     | を使用してを作成 `Register-ObjectEvent` し、    |
|                | **action パラメーターを** 使用したアクション。                    |

注: コマンドレットを使用して `Get-Job` 特定の種類のジョブを取得する前に、ジョブの種類を追加するモジュールが現在のセッションにインポートされていることを確認してください。
それ以外の場合、 `Get-Job` はその種類のジョブを取得しません。

## <a name="examples"></a>例

次のコマンドは、ローカルのバックグラウンドジョブ、リモートのバックグラウンドジョブ、ワークフロージョブ、およびスケジュールされたジョブを作成します。 次に、コマンドレットを使用して `Get-Job` ジョブを取得します。 `Get-Job` は、スケジュールされたジョブを取得しませんが、スケジュールされたジョブの開始インスタンスを取得します。

ローカルコンピューターでバックグラウンドジョブを開始します。

```powershell
PS> Start-Job -Name LocalData {Get-Process}

Id Name        PSJobTypeName   State   HasMoreData   Location   Command
-- ----        -------------   -----   -----------   --------   -------
2  LocalData   BackgroundJob   Running        True   localhost  Get-Process
```

リモートコンピューターで実行されるバックグラウンドジョブを開始します。

```powershell
PS> Invoke-Command -ComputerName Server01 {Get-Process} `
-AsJob -JobName RemoteData

Id  Name        PSJobTypeName  State   HasMoreData   Location   Command
--  ----        -------------  -----   -----------   --------   -------
2   RemoteData  RemoteJob      Running        True   Server01   Get-Process
```

スケジュールされたジョブを作成する

```powershell
PS>  Register-ScheduledJob -Name ScheduledJob -ScriptBlock `
 {Get-Process} -Trigger (New-JobTrigger -Once -At "3 PM")

Id         Name            JobTriggers     Command       Enabled
--         ----            -----------     -------       -------
1          ScheduledJob    1               Get-Process   True
```

ワークフローを作成します。

```powershell
PS> workflow Test-Workflow {Get-Process}
```

ワークフローをジョブとして実行します。

```powershell

PS> Test-Workflow -AsJob -JobName TestWFJob

Id  Name       PSJobTypeName   State   HasMoreData   Location   Command
--  ----       -------------   -----   -----------   --------   -------
2   TestWFJob  PSWorkflowJob   NotStarted     True   localhost  Get-Process
```

ジョブを取得します。 このコマンドは、スケジュールされ `Get-Job` たジョブを取得しませんが、開始されたスケジュールされたジョブのインスタンスを取得します。

```powershell
PS> Get-Job

Id  Name         PSJobTypeName  State     HasMoreData  Location  Command
--  ----         -------------  -----     -----------  --------  -------
2   LocalData    BackgroundJob  Completed True         localhost Get-Process
4   RemoteData   RemoteJob      Completed True         Server01  Get-Process
6   TestWFJob    PSWorkflowJob  Completed True         localhost WorkflowJob
8   ScheduledJob PSScheduledJob Completed True         localhost Get-Process
```

スケジュールされたジョブを取得するには、コマンドレットを使用し `Get-ScheduledJob` ます。

```powershell
PS> Get-ScheduledJob

Id         Name            JobTriggers     Command       Enabled
--         ----            -----------     -------       -------
1          ScheduledJob    1               Get-Process   True
```

## <a name="see-also"></a>関連項目

- [about_Jobs](about_Jobs.md)
- [about_Remote_Jobs](about_Remote_Jobs.md)
- [about_Thread_Jobs](about_Thread_Jobs.md)
- [about_Remote](about_Remote.md)
- [Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)
- [Start-Job](xref:Microsoft.PowerShell.Core.Start-Job)
- [Get-Job](xref:Microsoft.PowerShell.Core.Get-Job)
- [Wait-Job](xref:Microsoft.PowerShell.Core.Wait-Job)
- [Stop-Job](xref:Microsoft.PowerShell.Core.Stop-Job)
- [Remove-Job](xref:Microsoft.PowerShell.Core.Remove-Job)
- [New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)
- [Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)
- [Exit-PSSession](xref:Microsoft.PowerShell.Core.Exit-PSSession)
