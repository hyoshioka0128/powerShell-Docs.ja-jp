---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-job?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Job
ms.openlocfilehash: e93a46d863fb560c70d9257270af1e6f43d3f248
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391189"
---
# Get-Job

## 概要
現在のセッションで実行されている PowerShell バックグラウンドジョブを取得します。

## SYNTAX

### SessionIdParameterSet (既定値)

```
Get-Job [-IncludeChildJob] [-ChildJobState <JobState>] [-HasMoreData <Boolean>] [-Before <DateTime>]
 [-After <DateTime>] [-Newest <Int32>] [[-Id] <Int32[]>] [<CommonParameters>]
```

### InstanceIdParameterSet

```
Get-Job [-IncludeChildJob] [-ChildJobState <JobState>] [-HasMoreData <Boolean>] [-Before <DateTime>]
 [-After <DateTime>] [-Newest <Int32>] [-InstanceId] <Guid[]> [<CommonParameters>]
```

### NameParameterSet

```
Get-Job [-IncludeChildJob] [-ChildJobState <JobState>] [-HasMoreData <Boolean>] [-Before <DateTime>]
 [-After <DateTime>] [-Newest <Int32>] [-Name] <String[]> [<CommonParameters>]
```

### StateParameterSet

```
Get-Job [-IncludeChildJob] [-ChildJobState <JobState>] [-HasMoreData <Boolean>] [-Before <DateTime>]
 [-After <DateTime>] [-Newest <Int32>] [-State] <JobState> [<CommonParameters>]
```

### CommandParameterSet

```
Get-Job [-IncludeChildJob] [-ChildJobState <JobState>] [-HasMoreData <Boolean>] [-Before <DateTime>]
 [-After <DateTime>] [-Newest <Int32>] [-Command <String[]>] [<CommonParameters>]
```

### FilterParameterSet

```
Get-Job [-Filter] <Hashtable> [<CommonParameters>]
```

## Description

`Get-Job`コマンドレットは、現在のセッションで開始されたバックグラウンドジョブを表すオブジェクトを取得します。 を使用すると、 `Get-Job` コマンドレットを使用するか、 `Start-Job` 任意のコマンドレットの **AsJob** パラメーターを使用して開始されたジョブを取得できます。

パラメーターを指定しない場合、 `Get-Job` コマンドは現在のセッション内のすべてのジョブを取得します。 のパラメーターを使用して、 `Get-Job` 特定のジョブを取得できます。

が返すジョブオブジェクトには、 `Get-Job` ジョブに関する有用な情報が含まれていますが、ジョブの結果は含まれていません。 結果を取得するには、 `Receive-Job` コマンドレットを使用します。

Windows PowerShell のバックグラウンドジョブは、現在のセッションと対話せずにバックグラウンドで実行されるコマンドです。 通常は、バックグラウンドジョブを使用して、完了までに時間がかかる複雑なコマンドを実行します。 Windows PowerShell のバックグラウンド ジョブの詳細については、「[about_Jobs](./about/about_Jobs.md)」を参照してください。

Windows PowerShell 3.0 以降では、 `Get-Job` コマンドレットは、ワークフロージョブやスケジュールされたジョブのインスタンスなどのカスタムジョブの種類も取得します。 ジョブの種類を調べるには、ジョブの **PSJobTypeName** プロパティを使用します。

でカスタムのジョブの種類を取得できるようにするには、 `Get-Job` コマンドを実行する前に、コマンドを実行する前に、カスタムジョブの種類をサポートするモジュールをセッションにインポートし `Get-Job` ます。これには、コマンドレットを使用するか、 `Import-Module` を使用するか、またはモジュールのコマンドレットを取得します。 特定のカスタム ジョブの種類については、カスタムのジョブの種類機能のドキュメントを参照してください。

## 例

### 例 1: 現在のセッションで開始されているすべてのバックグラウンドジョブを取得する

このコマンドは、現在のセッションで開始されたすべてのバックグラウンド ジョブを取得します。 ローカル コンピューター上でジョブが実行される場合でも、他のセッションで作成されたジョブは含まれません。

```
PS C:\> Get-Job
```

### 例 2: インスタンス ID を使用してジョブを停止する

ジョブのインスタンス ID を取得し、それを使用してジョブを停止する方法を次のコマンドに示します。 一意ではないジョブ名とは異なり、インスタンス ID は一意です。

最初のコマンドは、 `Get-Job` コマンドレットを使用してジョブを取得します。 この例では、 **Name** パラメーターを使用してジョブを識別します。 このコマンドは、を返すジョブオブジェクトを `Get-Job` 変数に格納し `$j` ます。 この例では、指定された名前のジョブが 1 つだけあります。 2番目のコマンドは、変数内のオブジェクトの **InstanceId** プロパティを取得 `$j` し、変数に格納し `$ID` ます。 3番目のコマンドは、変数の値を表示し `$ID` ます。 4番目のコマンドは、 `Stop-Job` コマンドレットを使用してジョブを停止します。
**InstanceId** パラメーターを使用してジョブと変数を識別し、 `$ID` ジョブのインスタンス ID を表します。

```
PS C:\> $j = Get-Job -Name Job1
PS C:\> $ID = $j.InstanceID
PS C:\> $ID

Guid
----
03c3232e-1d23-453b-a6f4-ed73c9e29d55

PS C:\> Stop-Job -InstanceId $ID
```

### 例 3: 特定のコマンドを含むジョブを取得する

このコマンドは、コマンドを含むシステム上のジョブを取得し `Get-Process` ます。 コマンドは、の **コマンド** パラメーターを使用して `Get-Job` 、取得するジョブを制限します。 コマンドは、ワイルドカード文字 () を使用して `*` 、コマンド文字列内の任意の場所にコマンドを含むジョブを取得し `Get-Process` ます。

```
PS C:\> Get-Job -Command "*get-process*"
```

### 例 4: パイプラインを使用して特定のコマンドを含むジョブを取得する

前の例のコマンドと同様に、このコマンドは、コマンドを含むシステム上のジョブを取得し `Get-Process` ます。 このコマンドは、パイプライン演算子 () を使用し `|` て、文字列を引用符で囲んで `Get-Job` コマンドレットに送信します。 これは、前のコマンドと同等です。

```
PS C:\> "*get-process*" | Get-Job
```

### 例 5: 開始されていないジョブを取得する

このコマンドは、作成されている一方でまだ開始されていないジョブのみを取得します。 これには、後で実行するようにスケジュール設定されているジョブとまだスケジュール設定されていないジョブが含まれます。

```
PS C:\> Get-Job -State NotStarted
```

### 例 6: 名前が割り当てられていないジョブを取得する

このコマンドは、job で始まるジョブ名を持つすべてのジョブを取得します。 `job<number>`はジョブの既定の名前であるため、このコマンドは、明示的に名前が割り当てられていないすべてのジョブを取得します。

```
PS C:\> Get-Job -Name Job*
```

### 例 7: ジョブオブジェクトを使用してコマンド内のジョブを表す

この例では、を使用して `Get-Job` ジョブオブジェクトを取得し、そのジョブオブジェクトを使用してコマンド内のジョブを表す方法を示します。

最初のコマンドは、コマンドレットを使用して、 `Start-Job` `Get-Process` ローカルコンピューター上でコマンドを実行するバックグラウンドジョブを開始します。 このコマンドは、の **name** パラメーターを使用して `Start-Job` 、ジョブにフレンドリ名を割り当てます。 2番目のコマンドは、を使用し `Get-Job` てジョブを取得します。 この例では、の **Name** パラメーターを使用して `Get-Job` ジョブを識別します。 このコマンドは、結果として得られるジョブオブジェクトを変数に保存し `$j` ます。 3番目のコマンドは、変数内の job オブジェクトの値を表示し `$j` ます。 **State** プロパティの値は、ジョブが完了したことを示します。 **HasMoreData** プロパティの値は、このジョブに関してまだ取得されていない結果があることを示しています。 4番目のコマンドは、 `Receive-Job` コマンドレットを使用してジョブの結果を取得します。 ジョブを表すために、変数内の job オブジェクトを使用し `$j` ます。 また、パイプライン演算子を使用して、ジョブオブジェクトをに送信することもでき `Receive-Job` ます。

```
PS C:\> Start-Job -ScriptBlock {Get-Process} -Name MyJob
PS C:\> $j = Get-Job -Name MyJob
PS C:\> $j
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
6      MyJob           BackgroundJob   Completed     True            localhost            Get-Process

PS C:\> Receive-Job -Job $j
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    124       4    13572      12080    59            1140 audiodg
    783      16    11428      13636   100             548 CcmExec
     96       4     4252       3764    59            3856 ccmsetup
...
```


### 例 8: 別の方法で開始されたジョブを含むすべてのジョブを取得する

この例では、 `Get-Job` コマンドレットが、別の方法を使用して起動された場合でも、現在のセッションで開始されたすべてのジョブを取得できることを示しています。

最初のコマンドは、 `Start-Job` コマンドレットを使用して、ローカルコンピューター上でジョブを開始します。 2番目のコマンドは、コマンドレットの **AsJob** パラメーターを使用して、 `Invoke-Command` S1 コンピューターでジョブを開始します。 ジョブに含まれるコマンドがリモート コンピューター上で実行される場合でもジョブ オブジェクトがローカル コンピューターに作成されるため、ローカル コマンドを使用してジョブを管理します。 3番目のコマンドは、コマンドレットを使用して、 `Invoke-Command` `Start-Job` S2 コンピューター上でコマンドを実行します。 この方法を使用すると、ジョブオブジェクトがリモートコンピューター上に作成されるため、リモートコマンドを使用してジョブを管理します。 4番目のコマンドは、を使用して、 `Get-Job` ローカルコンピューターに格納されているジョブを取得します。 Windows PowerShell 3.0 で導入されたジョブの **PSJobTypeName** プロパティには、コマンドレットを使用して開始したローカルジョブ `Start-Job` がバックグラウンドジョブであり、コマンドレットを使用してリモートセッションで開始されたジョブがリモートジョブであることが示されてい `Invoke-Command` ます。 5番目のコマンドは、を使用して `Invoke-Command` `Get-Job` S2 コンピューターでコマンドを実行します。このサンプル出力は、コマンドの結果を示して `Get-Job` います。 S2 コンピューター上では、ジョブはローカル ジョブとして表示されます。 コンピューター名は localhost で、ジョブの種類はバックグラウンドジョブです。リモートコンピューターでバックグラウンドジョブを実行する方法の詳細については、「 [about_Remote_Jobs](./about/about_Remote_Jobs.md)」を参照してください。

```
PS C:\> Start-Job -ScriptBlock {Get-EventLog System}
PS C:\> Invoke-Command -ComputerName S1 -ScriptBlock {Get-EventLog System} -AsJob
PS C:\> Invoke-Command -ComputerName S2 -ScriptBlock {Start-Job -ScriptBlock {Get-EventLog System}}
PS C:\> Get-Job
Id     Name       PSJobTypeName   State         HasMoreData     Location        Command
--     ----       -------------   -----         -----------     --------        -------
1      Job1       BackgroundJob   Running       True            localhost       Get-EventLog System
2      Job2       RemoteJob       Running       True            S1              Get-EventLog System

PS C:\> Invoke-Command -ComputerName S2 -ScriptBlock {Start-Job -ScriptBlock {Get-EventLog System}}
Id    Name     PSJobTypeName  State      HasMoreData   Location   Command
--    ----     -------------  -----      -----------   -------    -------
4     Job4     BackgroundJob  Running    True          localhost  Get-Eventlog System
```

### 例 9: 失敗したジョブを調査する

このコマンドは、が返すジョブオブジェクトを使用して `Get-Job` 、ジョブが失敗した原因を調査する方法を示します。
また、各ジョブの子ジョブを取得する方法も示します。

最初のコマンドは、 `Start-Job` コマンドレットを使用して、ローカルコンピューター上でジョブを開始します。 が返すジョブオブジェクトは、 `Start-Job` ジョブが失敗したことを示します。 **State** プロパティの値が失敗しました。

2番目のコマンドは、 `Get-Job` コマンドレットを使用してジョブを取得します。 ここでは、ドット表記を使用してオブジェクトの **JobStateInfo** プロパティの値を取得しています。 パイプライン演算子を使用して、 **Jobstateinfo** プロパティのオブジェクトをコマンドレットに送信し `Format-List` ます。これにより、オブジェクトのすべてのプロパティ () の書式 `*` がリストに設定されます。コマンドの結果は、 `Format-List` ジョブの **Reason** プロパティの値が空白であることを示しています。

3番目のコマンドは、詳細を調査します。 コマンドを使用して `Get-Job` ジョブを取得し、パイプライン演算子を使用してジョブオブジェクト全体をコマンドレットに送信します。これにより、ジョブ `Format-List` のすべてのプロパティが一覧に表示されます。Job オブジェクトのすべてのプロパティの表示は、そのジョブに Job2 という名前の子ジョブが含まれていることを示しています。

4番目のコマンドは、を使用して、 `Get-Job` Job2 子ジョブを表すジョブオブジェクトを取得します。 これは、コマンドが実際に実行されたジョブです。 ここでは、ドットメソッドを使用して、 **Jobstateinfo** プロパティの **Reason** プロパティを取得します。結果には、アクセス拒否エラーによってジョブが失敗したことが示されます。 この場合、ユーザーは Windows PowerShell を起動するときに [管理者として実行] オプションを使用していません。バックグラウンドジョブは、Windows PowerShell のリモート処理機能を使用するため、ローカルコンピューター上でジョブが実行されている場合でも、ジョブを実行するリモート処理用にコンピューターを構成する必要があります。Windows PowerShell でのリモート処理の要件の詳細については、「 [about_Remote_Requirements](./about/about_Remote_Requirements.md)」を参照してください。 トラブルシューティングのヒントについては、「[about_Remote_Troubleshooting](./about/about_Remote_Troubleshooting.md)」を参照してください。

```
PS C:\> Start-Job -ScriptBlock {Get-Process}
Id     Name       PSJobTypeName   State       HasMoreData     Location             Command
--     ----       -------------   -----       -----------     --------             -------
1      Job1       BackgroundJob   Failed      False           localhost            Get-Process

PS C:\> (Get-Job).JobStateInfo | Format-List -Property *
State  : Failed
Reason :

PS C:\> Get-Job | Format-List -Property *
HasMoreData   : False
StatusMessage :
Location      : localhost
Command       : get-process
JobStateInfo  : Failed
Finished      : System.Threading.ManualReset
EventInstanceId    : fb792295-1318-4f5d-8ac8-8a89c5261507
Id            : 1
Name          : Job1
ChildJobs     : {Job2}
Output        : {}
Error         : {}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
StateChanged  :

PS C:\> (Get-Job -Name job2).JobStateInfo.Reason
Connecting to remote server using WSManCreateShellEx api failed. The async callback gave the
following error message: Access is denied.
```

### 例 10: フィルター処理された結果を取得する

この例では、 **Filter** パラメーターを使用してワークフロー ジョブを取得する方法を示します。 Windows PowerShell 3.0 で導入された **Filter** パラメーターは、ワークフロー ジョブ、スケジュールされたジョブなどの、カスタムのジョブの種類でのみ有効です。

最初のコマンドは、 **workflow** キーワードを使用して、ワークフローのワークフローを作成します。 2番目のコマンドは、ワークフローをバックグラウンドジョブとして実行するために、処理されたプロセスワークフローの **AsJob** パラメーターを使用します。 ここでは、ワークフローの **JobName** パラメーターを使用してジョブの名前を指定し、ワークフローの **PSPrivateMetadata** パラメーターを使用してカスタム ID を指定しています。 3番目のコマンドは、の **Filter** パラメーターを使用し `Get-Job` て、 **PSPrivateMetadata** パラメーターで指定されたカスタム ID でジョブを取得します。

```
PS C:\> Workflow WFProcess {Get-Process}
PS C:\> WFProcess -AsJob -JobName WFProcessJob -PSPrivateMetadata @{MyCustomId = 92107}
PS C:\> Get-Job -Filter @{MyCustomId = 92107}
Id     Name            State         HasMoreData     Location             Command
--     ----            -----         -----------     --------             -------
1      WFProcessJob    Completed     True            localhost            WFProcess
```

### 例 11: 子ジョブに関する情報を取得する

この例は、コマンドレットの **IncludeChildJob** パラメーターと **childjobstate** パラメーターを使用した場合の効果を示して `Get-Job` います。

最初のコマンドは、現在のセッション内のジョブを取得します。 出力には、バックグラウンド ジョブ、リモート ジョブ、およびスケジュールされたジョブのいくつかのインスタンスが含まれます。 リモート ジョブ Job4 については、失敗したことが示されています。
2番目のコマンドは、の **IncludeChildJob** パラメーターを使用し `Get-Job` ます。 出力には、子ジョブを持つすべてのジョブの子ジョブが追加されます。この場合、変更後の出力には、Job4 の Job5 子ジョブのみが失敗したことが示されます。 3番目のコマンドは、 **Childjobstate** パラメーターを使用して、値が failed であることを示します。出力には、すべての親ジョブと、失敗した子ジョブのみが含まれます。 5番目のコマンドは、jobs の **Jobstateinfo** プロパティと **Reason** プロパティを使用して、Job5 が失敗した理由を検出します。

```
PS C:\> Get-Job
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
2      Job2            BackgroundJob   Completed     True            localhost            .\Get-Archive.ps1
4      Job4            RemoteJob       Failed        True            Server01, Server02   .\Get-Archive.ps1
7      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
8      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
9      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
10     UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help

PS C:\> Get-Job -IncludeChildJob
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
2      Job2            BackgroundJob   Completed     True            localhost           .\Get-Archive.ps1
3      Job3                            Completed     True            localhost           .\Get-Archive.ps1
4      Job4            RemoteJob       Failed        True            Server01, Server02  .\Get-Archive.ps1
5      Job5                            Failed        False           Server01            .\Get-Archive.ps1
6      Job6                            Completed     True            Server02            .\Get-Archive.ps1
7      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
8      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
9      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
10     UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help

PS C:\> Get-Job -Name Job4 -ChildJobState Failed
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
2      Job2            BackgroundJob   Completed     True            localhost           .\Get-Archive.ps1
4      Job4            RemoteJob       Failed        True            Server01, Server02  .\Get-Archive.ps1
5      Job5                            Failed        False           Server01            .\Get-Archive.ps1
7      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
8      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
9      UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help
10     UpdateHelpJob   PSScheduledJob  Completed     True            localhost            Update-Help

PS C:\> (Get-Job -Name Job5).JobStateInfo.Reason
Connecting to remote server Server01 failed with the following error message:
Access is denied.
```

詳細については、 [about_Remote_Troubleshooting](./about/about_Remote_Troubleshooting.md) のヘルプトピックを参照してください。

## PARAMETERS

### -After

指定された日時以降に終了した完了済みのジョブを取得します。 **Datetime** オブジェクトを入力します。たとえば、コマンドレットによって返されるオブジェクト `Get-Date` や、 **datetime** オブジェクトに変換できる文字列 (やなど) を入力し `Dec 1, 2012 2:00 AM` `11/06` ます。

このパラメーターは、ワークフロー ジョブ、スケジュールされたジョブなどの、 **EndTime** プロパティを持つカスタムのジョブの種類に対してのみ機能します。 コマンドレットを使用して作成されたものなど、標準のバックグラウンドジョブでは機能しません `Start-Job` 。 このパラメーターのサポートについては、ジョブの種類のヘルプ トピックを参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.DateTime
Parameter Sets: SessionIdParameterSet, InstanceIdParameterSet, NameParameterSet, StateParameterSet, CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Before

指定された日時以前に終了した完了済みのジョブを取得します。 **DateTime** オブジェクトを入力します。

このパラメーターは、ワークフロー ジョブ、スケジュールされたジョブなどの、 **EndTime** プロパティを持つカスタムのジョブの種類に対してのみ機能します。 コマンドレットを使用して作成されたものなど、標準のバックグラウンドジョブでは機能しません `Start-Job` 。 このパラメーターのサポートについては、ジョブの種類のヘルプ トピックを参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.DateTime
Parameter Sets: SessionIdParameterSet, InstanceIdParameterSet, NameParameterSet, StateParameterSet, CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ChildJobState

指定された状態を持つ子ジョブのみを取得します。 このパラメーターの有効値は、次のとおりです。

- NotStarted
- 実行中
- 完了
- 失敗
- 停止済み
- ［ブロック済み］
- Suspended
- [Disconnected]\(切断済み\)
- 中断中
- 停止中

既定で `Get-Job` は、は子ジョブを取得しません。 **IncludeChildJob** パラメーターを使用して、 `Get-Job` すべての子ジョブを取得します。 **ChildJobState** パラメーターを使用した場合、 **IncludeChildJob** パラメーターは効果を持ちません。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.JobState
Parameter Sets: SessionIdParameterSet, InstanceIdParameterSet, NameParameterSet, StateParameterSet, CommandParameterSet
Aliases:
Accepted values: NotStarted, Running, Completed, Failed, Stopped, Blocked, Suspended, Disconnected, Suspending, Stopping, AtBreakpoint

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Command

コマンドの配列を文字列として指定します。 このコマンドレットは、指定されたコマンドを含むジョブを取得します。 既定値はすべてのジョブです。 ワイルドカード文字を使用すると、コマンドパターンを指定できます。

```yaml
Type: System.String[]
Parameter Sets: CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Filter

条件のハッシュテーブルを指定します。 このコマンドレットは、すべての条件に適合するジョブを取得します。
ジョブのプロパティをキー、ジョブのプロパティ値を値とするハッシュ テーブルを入力します。

このパラメーターは、ワークフロー ジョブ、スケジュールされたジョブなどの、カスタムのジョブの種類に対してのみ機能します。 コマンドレットを使用して作成されたものなど、標準のバックグラウンドジョブでは機能しません `Start-Job` 。 このパラメーターのサポートについては、ジョブの種類のヘルプ トピックを参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

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

### -Hasdata

このコマンドレットが、指定された **hasを持つデータ** プロパティ値を持つジョブのみを取得するかどうかを示します。
**HasMoreData** プロパティは、すべてのジョブの結果が現在のセッションで受け取られたかどうかを示します。 より多くの結果を持つジョブを取得するには、の値を指定し `$True` ます。 より多くの結果が得られないジョブを取得するには、の値を指定し `$False` ます。

ジョブの結果を取得するには、コマンドレットを使用し `Receive-Job` ます。

コマンドレットを使用すると、 `Receive-Job` メモリ内のセッション固有のストレージから、返された結果が削除されます。 現在のセッションでジョブのすべての結果が返されると、ジョブの **Hasmore data** プロパティの値がに設定され、 `$False` 現在のセッションにジョブの結果がなくなったことが示されます。 が **Keep** `Receive-Job` `Receive-Job` 結果を削除して、 **hasdata** プロパティの値を変更しないようにするには、の Keep パラメーターを使用します。
詳細を表示するには「`Get-Help Receive-Job`」を入力します。

**HasMoreData** プロパティは、現在のセッションに固有です。 ジョブの結果をディスクに保存するスケジュールされたジョブの種類など、カスタムジョブの種類の結果がセッションの外部に保存される場合は、 `Receive-Job` 別のセッションでコマンドレットを使用して、 **hasdata** の値がの場合でも、ジョブの結果を再び取得でき `$False` ます。 詳細については、カスタムのジョブの種類のヘルプ トピックを参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Boolean
Parameter Sets: SessionIdParameterSet, InstanceIdParameterSet, NameParameterSet, StateParameterSet, CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

このコマンドレットが取得するジョブの Id の配列を指定します。

ID は、現在のセッションでジョブを一意に識別する整数です。 インスタンス ID よりも覚えやすく、入力する方が簡単ですが、現在のセッションでのみ一意です。 1つ以上の Id をコンマで区切って入力できます。 ジョブの ID を検索するには、パラメーターを指定せずに「」と入力 `Get-Job` します。

```yaml
Type: System.Int32[]
Parameter Sets: SessionIdParameterSet
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -IncludeChildJob

このコマンドレットが親ジョブに加えて子ジョブも返すことを示します。

このパラメーターは、 `Get-Job` エラーの原因が子ジョブのプロパティに保存されるため、がコンテナーの親ジョブとジョブの失敗を返すワークフロージョブを調査する場合に特に便利です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: SessionIdParameterSet, InstanceIdParameterSet, NameParameterSet, StateParameterSet, CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InstanceId

このコマンドレットが取得するジョブのインスタンス Id の配列を指定します。 既定値はすべてのジョブです。

インスタンス ID は、コンピューター上のジョブを一意に識別する GUID です。 ジョブのインスタンス ID を検索するには、を使用 `Get-Job` します。

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

### -Name

このコマンドレットが取得するジョブのインスタンスフレンドリ名の配列を指定します。 ジョブの名前を入力するか、またはワイルドカード文字を使用してジョブ名のパターンを入力します。 既定では、は、 `Get-Job` 現在のセッション内のすべてのジョブを取得します。

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

### -最新

取得するジョブの数を指定します。 このコマンドレットは、最後に終了したジョブを取得します。

**Newest** パラメーターは、並べ替えを行わずに、終了時刻の順で最も新しいジョブを返します。 出力を並べ替えるには、コマンドレットを使用し `Sort-Object` ます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: SessionIdParameterSet, InstanceIdParameterSet, NameParameterSet, StateParameterSet, CommandParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -状態

ジョブの状態を指定します。 このコマンドレットは、指定された状態のジョブのみを取得します。 このパラメーターの有効値は、次のとおりです。

- NotStarted
- 実行中
- 完了
- 失敗
- 停止済み
- ［ブロック済み］
- Suspended
- [Disconnected]\(切断済み\)
- 中断中
- 停止中

既定では、は、 `Get-Job` 現在のセッション内のすべてのジョブを取得します。

ジョブの状態の詳細については、「 [Jobstate 列挙型](/dotnet/api/system.management.automation.jobstate)」を参照してください。

```yaml
Type: System.Management.Automation.JobState
Parameter Sets: StateParameterSet
Aliases:
Accepted values: NotStarted, Running, Completed, Failed, Stopped, Blocked, Suspended, Disconnected, Suspending, Stopping, AtBreakpoint

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### System. Automation. RemotingJob

このコマンドレットは、セッション内のジョブを表すオブジェクトを返します。

## 注

ジョブの **PSJobTypeName** プロパティは、ジョブの種類を示します。 プロパティ値は、ジョブの種類の作成者によって決定されます。 次の一覧に、一般的なジョブの種類を示します。

- **Backgroundjob** 。 ローカルジョブは、を使用して開始されました `Start-Job` 。
- **Remotejob** 。 コマンドレットの **AsJob** パラメーターを使用して、 **PSSession** でジョブが開始されました `Invoke-Command` 。
- **Psworkflowjob** 。 ワークフローの **AsJob** 共通パラメーターを使用して開始されたジョブ。

## 関連リンク

[Invoke-Command](Invoke-Command.md)

[Receive-Job](Receive-Job.md)

[Remove-Job](Remove-Job.md)

[Start-Job](Start-Job.md)

[Stop-Job](Stop-Job.md)

[Wait-Job](Wait-Job.md)

[about_Jobs](About/about_Jobs.md)

[about_Job_Details](About/about_Job_Details.md)

[about_Remote_Jobs](About/about_Remote_Jobs.md)
