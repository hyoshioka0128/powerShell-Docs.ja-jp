---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/receive-job?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Receive-Job
ms.openlocfilehash: 3a11bdb27f3fe16b7b66e82821a3ffe8fa5f6cfa
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599545"
---
# Receive-Job

## 概要
現在のセッションの PowerShell バックグラウンドジョブの結果を取得します。

## SYNTAX

### Location (既定値)

```
Receive-Job [-Job] <Job[]> [[-Location] <String[]>] [-Keep] [-NoRecurse] [-Force] [-Wait] [-AutoRemoveJob]
 [-WriteEvents] [-WriteJobInResults] [<CommonParameters>]
```

### [ComputerName]

```
Receive-Job [-Job] <Job[]> [[-ComputerName] <String[]>] [-Keep] [-NoRecurse] [-Force] [-Wait] [-AutoRemoveJob]
 [-WriteEvents] [-WriteJobInResults] [<CommonParameters>]
```

### セッション

```
Receive-Job [-Job] <Job[]> [[-Session] <PSSession[]>] [-Keep] [-NoRecurse] [-Force] [-Wait] [-AutoRemoveJob]
 [-WriteEvents] [-WriteJobInResults] [<CommonParameters>]
```

### NameParameterSet

```
Receive-Job [-Keep] [-NoRecurse] [-Force] [-Wait] [-AutoRemoveJob] [-WriteEvents] [-WriteJobInResults]
 [-Name] <String[]> [<CommonParameters>]
```

### InstanceIdParameterSet

```
Receive-Job [-Keep] [-NoRecurse] [-Force] [-Wait] [-AutoRemoveJob] [-WriteEvents] [-WriteJobInResults]
 [-InstanceId] <Guid[]> [<CommonParameters>]
```

### SessionIdParameterSet

```
Receive-Job [-Keep] [-NoRecurse] [-Force] [-Wait] [-AutoRemoveJob] [-WriteEvents] [-WriteJobInResults]
 [-Id] <Int32[]> [<CommonParameters>]
```

## Description

コマンドレットは、コマンド `Receive-Job` `Start-Job` レットまたは任意のコマンドレットの **AsJob** パラメーターを使用して開始されたジョブなど、PowerShell のバックグラウンドジョブの結果を取得します。
すべてのジョブの結果の取得、名前、ID、インスタンス ID、コンピューター名、場所、セッションによる指定、ジョブ オブジェクトの送信を行うことができます。

PowerShell バックグラウンドジョブを開始すると、ジョブが開始されますが、結果はすぐには表示されません。 代わりに、バックグラウンド ジョブを表すオブジェクトが返されます。
このジョブ オブジェクトには、ジョブに関する有用な情報が含まれていますが、結果は含まれません。
このメソッドを使用すると、ジョブの実行中にセッションで作業を続行できます。
PowerShell のバックグラウンドジョブの詳細については、「 [about_Jobs](./About/about_Jobs.md)」を参照してください。

`Receive-Job`コマンドレットは、コマンドの送信時に生成された結果を取得し `Receive-Job` ます。
結果がまだ完了していない場合は、追加 `Receive-Job` のコマンドを実行して残りの結果を取得できます。

既定では、ジョブの結果は受信時にシステムから削除されますが、再度受信できるように **Keep** パラメーターを使用して結果を保存することができます。
ジョブの結果を削除するには、 `Receive-Job` **Keep** パラメーターを指定せずにコマンドを再度実行するか、セッションを閉じるか、コマンドレットを使用して `Remove-Job` セッションからジョブを削除します。

Windows PowerShell 3.0 以降では、 `Receive-Job` ワークフロージョブやスケジュールされたジョブのインスタンスなどのカスタムジョブの種類の結果も取得します。
がカスタムのジョブの種類の結果を取得できるようにするには `Receive-Job` 、コマンドを実行する前に、コマンドを実行する前に、カスタムジョブの種類をサポートするモジュールをセッションにインポートします。そのためには、 `Receive-Job` コマンドレットを使用するか、 `Import-Module` またはモジュールのコマンドレットを取得します。
特定のカスタム ジョブの種類については、カスタムのジョブの種類機能のドキュメントを参照してください。

## 例

### 例 1: 特定のジョブの結果を取得する

```powershell
$job = Start-Job -ScriptBlock {Get-Process}
Receive-Job -Job $job
```

これらのコマンドは、の **job** パラメーターを使用して、 `Receive-Job` 特定のジョブの結果を取得します。

最初のコマンドは、を使用してジョブを開始し、その `Start-Job` ジョブオブジェクトを変数に格納し `$job` ます。

2番目のコマンドは、 `Receive-Job` コマンドレットを使用してジョブの結果を取得します。
**Job** パラメーターを使用して、ジョブを指定しています。

### 例 2: Keep パラメーターを使用する

```powershell
$job = Start-Job -ScriptBlock {Get-Service dhcp, fakeservice}
$job | Receive-Job -Keep
```

```Output
Cannot find any service with service name 'fakeservice'.
    + CategoryInfo          : ObjectNotFound: (fakeservice:String) [Get-Service], ServiceCommandException
    + FullyQualifiedErrorId : NoServiceFoundForGivenName,Microsoft.PowerShell.Commands.GetServiceCommand
    + PSComputerName        : localhost

Status   Name               DisplayName
------   ----               -----------
Running  dhcp               DHCP Client
```

```powershell
$job | Receive-Job -Keep
```

```Output
Cannot find any service with service name 'fakeservice'.
    + CategoryInfo          : ObjectNotFound: (fakeservice:String) [Get-Service], ServiceCommandException
    + FullyQualifiedErrorId : NoServiceFoundForGivenName,Microsoft.PowerShell.Commands.GetServiceCommand
    + PSComputerName        : localhost

Status   Name               DisplayName
------   ----               -----------
Running  dhcp               DHCP Client
```

この例では、ジョブを変数に格納し、その `$job` ジョブを `Receive-Job` コマンドレットに渡します。 `-Keep`パラメーターは、最初のビューの後にすべての集計ストリームデータを再度取得できるようにするためにも使用されます。

### 例 3: いくつかのバックグラウンドジョブの結果を取得する

の **AsJob** パラメーターを使用して `Invoke-Command` ジョブを開始すると、ジョブがリモートコンピューター上で実行されている場合でも、ジョブオブジェクトがローカルコンピューター上に作成されます。
その結果、ジョブを管理するには、ローカルのコマンドを使用することになります。

また、 **AsJob** を使用すると、PowerShell は、開始された各ジョブの子ジョブを含む1つのジョブオブジェクトを返します。 この例では、ジョブ オブジェクトには、各リモート コンピューターのジョブに 1 つずつ、合計 3 つの子ジョブが含まれます。

```powershell
# Use the Invoke-Command cmdlet with the -AsJob parameter to start a background job that runs a Get-Service command on three remote computers.
# Store the resulting job object in the $j variable
$j = Invoke-Command -ComputerName Server01, Server02, Server03 -ScriptBlock {Get-Service} -AsJob
# Display the value of the **ChildJobs** property of the job object in $j.
# The display shows that the command created three child jobs, one for the job on each remote computer.
# You could also use the -IncludeChildJobs parameter of the Get-Job cmdlet.
$j.ChildJobs
```

```Output
Id   Name     State      HasMoreData   Location       Command
--   ----     -----      -----------   --------       -------
2    Job2     Completed  True          Server01       Get-Service
3    Job3     Completed  True          Server02       Get-Service
4    Job4     Completed  True          Server03       Get-Service
```

```powershell
# Use the Receive-Job cmdlet to get the results of just the Job3 child job that ran on the Server02 computer.
# Use the *Keep* parameter to allow you to view the aggregated stream data more than once.
Receive-Job -Name Job3 -Keep
```

```Output
Status  Name        DisplayName                        PSComputerName
------  ----------- -----------                        --------------
Running AeLookupSvc Application Experience             Server02
Stopped ALG         Application Layer Gateway Service  Server02
Running Appinfo     Application Information            Server02
Running AppMgmt     Application Management             Server02
```

### 例 4: 複数のリモートコンピューターでバックグラウンドジョブの結果を取得する

```powershell
# Use the New-PSSession cmdlet to create three user-managed PSSessions on three servers, and save the sessions in the $s variable.
$s = New-PSSession -ComputerName Server01, Server02, Server03
# Use Invoke-Command run a Start-Job command in each of the PSSessions in the $s variable.
# The job outputs the ComputerName of each server.
# Save the job objects in the $j variable.
$j = Invoke-Command -Session $s -ScriptBlock {Start-Job -ScriptBlock {$env:COMPUTERNAME}}
# To confirm that these job objects are from the remote machines, run Get-Job to show no local jobs running.
Get-Job
```

```Output

```

```powershell
# Display the three job objects in $j.
# Note that the Localhost location is not the local computer, but instead localhost as it relates to the job on each Server.
$j
```

```Output
Id   Name     State      HasMoreData   Location   Command
--   ----     -----      -----------   --------   -------
1    Job1     Completed  True          Localhost  $env:COMPUTERNAME
2    Job2     Completed  True          Localhost  $env:COMPUTERNAME
3    Job3     Completed  True          Localhost  $env:COMPUTERNAME
```

```powershell
# Use Invoke-Command to run a Receive-Job command in each of the sessions in the $s variable and save the results in the $Results variable.
# The Receive-Job command must be run in each session because the jobs were run locally on each server.
$results = Invoke-Command -Session $s -ScriptBlock {Receive-Job -Job $Using:j}
```

```Output
Server01
Server02
Server03
```

この例は、3 つのリモート コンピューター上で実行されるバックグラウンド ジョブの結果を取得する方法を示しています。
前の例とは異なり、を使用して `Invoke-Command` コマンドを実行すると、 `Start-Job` 3 台のコンピューターそれぞれに対して3つの独立したジョブが実際に開始されます。 結果として、このコマンドは、3 つの異なるコンピューターでローカルに実行される 3 つのジョブを表す 3 つのジョブ オブジェクトを返します。

> [!NOTE]
> 最後のコマンドでは、 `$j` がローカル変数であるため、スクリプトブロックは **Using** スコープ修飾子を使用して変数を識別し `$j` ます。 スコープ修飾子の **使用** の詳細については、「 [about_Remote_Variables](./About/about_Remote_Variables.md)」を参照してください。

### 例 5: 子ジョブにアクセスする

パラメーターは、 `-Keep` ジョブの集計されたストリームの状態を保持して、再び表示できるようにします。 このパラメーターを指定しないと、ジョブの受信時にすべての集計ストリームデータが消去されます。 詳細については、「」を参照してください [about_Job_Details](About/about_Job_Details.md#child-jobs)

> [!NOTE]
> 集計されたストリームには、すべての子ジョブのストリームが含まれます。 引き続き、ジョブオブジェクトと子ジョブオブジェクトを使用して、個々のデータストリームに接続できます。

```powershell
Start-Job -Name TestJob -ScriptBlock {dir C:\, Z:\}
# Without the Keep parameter, aggregated child job data is displayed once.
# Then destroyed.
Receive-Job -Name TestJob
```

```Output
    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---        1/24/2019   7:11 AM                Program Files
d-r---        2/13/2019   8:32 AM                Program Files (x86)
d-r---        10/3/2018  11:47 AM                Users
d-----         2/7/2019   1:52 AM                Windows
Cannot find drive. A drive with the name 'Z' does not exist.
    + CategoryInfo          : ObjectNotFound: (Z:String) [Get-ChildItem], DriveNotFoundException
    + FullyQualifiedErrorId : DriveNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand
    + PSComputerName        : localhost
```

```powershell
# It would seem that the child job data is gone.
Receive-Job -Name TestJob
```

```Output

```

```powershell
# Using the object model, you can still retrieve child job data and streams.
$job = Get-Job -Name TestJob
$job.ChildJobs[0].Error
```

```Output
Cannot find drive. A drive with the name 'Z' does not exist.
    + CategoryInfo          : ObjectNotFound: (Z:String) [Get-ChildItem], DriveNotFoundException
    + FullyQualifiedErrorId : DriveNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand
    + PSComputerName        : localhost
```

## PARAMETERS

### -AutoRemoveJob

このコマンドレットがジョブの結果を返した後にジョブを削除することを示します。
ジョブの結果が増えると、ジョブは削除されますが、 `Receive-Job` メッセージが表示されます。

このパラメーターは、カスタムのジョブの種類に対してのみ機能します。
スケジュールされたジョブのインスタンスなど、ジョブやセッションの外部の種類を保存するジョブのインスタンス用に設計されています。

このパラメーターは、 **Wait** パラメーターなしでは使用できません。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

コンピューターの名前の配列を指定します。

このパラメーターは、ローカル コンピューターに格納されているジョブの結果の中から選択します。
リモートコンピューターで実行されるジョブのデータは取得されません。
リモートコンピューターに格納されているジョブの結果を取得するには、 `Invoke-Command` コマンドレットを使用して `Receive-Job` コマンドをリモートで実行します。

```yaml
Type: System.String[]
Parameter Sets: ComputerName
Aliases: Cn

Required: False
Position: 1
Default value: All computers available
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Force

ジョブが **中断** 状態または **切断** 状態の場合、このコマンドレットは待機を続行することを示します。 既定では、の **wait** パラメーターは、 `Receive-Job` ジョブが次のいずれかの状態にある場合に、待機を返します。

- 完了
- 失敗
- 停止済み
- Suspended
- [接続解除されました。]

**Force** パラメーターは、このコマンドで **Wait** パラメーターも使用されている場合にのみ有効です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

ID の配列を指定します。
このコマンドレットは、指定された Id を持つジョブの結果を取得します。

ID は、現在のセッションでジョブを一意に識別する整数です。
インスタンス ID よりも覚えやすく、入力も簡単ですが、現在のセッションでのみ一意です。 1つ以上の Id をコンマで区切って入力できます。
ジョブの ID を検索するには、を使用 `Get-Job` します。

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

インスタンス Id の配列を指定します。
このコマンドレットは、指定されたインスタンス Id を持つジョブの結果を取得します。

インスタンス ID は、コンピューター上のジョブを一意に識別する GUID です。
ジョブのインスタンス ID を検索するには、コマンドレットを使用し `Get-Job` ます。

```yaml
Type: System.Guid[]
Parameter Sets: InstanceIdParameterSet
Aliases:

Required: True
Position: 0
Default value: All instances
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ジョブ

結果を取得するジョブを指定します。

ジョブまたはジョブを取得するコマンドを含む変数を入力します。
パイプを使用してジョブオブジェクトをにパイプすることもでき `Receive-Job` ます。

```yaml
Type: System.Management.Automation.Job[]
Parameter Sets: Location, Session, ComputerName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -保持

このコマンドレットが、集計されたストリームデータを受け取った後でもシステムに保存することを示します。 既定では、を使用して表示した後、集計ストリームデータは消去され `Receive-Job` ます。

セッションを閉じるか、コマンドレットを使用してジョブを削除すると、 `Remove-Job` 集計されたストリームデータも削除されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Location

場所の配列を指定します。
このコマンドレットは、指定された場所にあるジョブの結果のみを取得します。

```yaml
Type: System.String[]
Parameter Sets: Location
Aliases:

Required: False
Position: 1
Default value: All locations
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

フレンドリ名の配列を指定します。
このコマンドレットは、指定された名前を持つジョブの結果を取得します。
ワイルドカード文字がサポートされています。

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

### -NoRecurse

このコマンドレットが、指定されたジョブからのみ結果を取得することを示します。
既定では、は、 `Receive-Job` 指定されたジョブのすべての子ジョブの結果も取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -セッション

セッションの配列を指定します。
このコマンドレットは、指定された PowerShell セッション (**PSSession**) で実行されたジョブの結果を取得します。
**Pssession** を含む変数、またはコマンドなどの **pssession** を取得するコマンドを入力し `Get-PSSession` ます。

```yaml
Type: System.Management.Automation.Runspaces.PSSession[]
Parameter Sets: Session
Aliases:

Required: False
Position: 1
Default value: All sessions
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Wait

すべてのジョブの結果を受信するまでコマンドプロンプトが表示されないことを示します。
既定では、は `Receive-Job` 使用可能な結果を直ちに返します。

既定では、**Wait** パラメーターは、ジョブが次のいずれかの状態になるまで待機します:

- 完了
- 失敗
- 停止済み
- Suspended
- [接続解除されました。]

ジョブの状態が中断または切断された場合に待機するように **待機** パラメーターを指定するには、 **wait** パラメーターと共に **Force** パラメーターを使用します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

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

### -WriteEvents

このコマンドレットがジョブの完了を待機している間に、ジョブの状態の変化を報告することを示します。

このパラメーターは、**Wait** パラメーターがこのコマンドで使用され、**Keep** パラメーターが省略される場合にのみ有効です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WriteJobInResults

このコマンドレットによってジョブオブジェクトが返され、その後に結果が返されることを示します。

このパラメーターは、**Wait** パラメーターがこのコマンドで使用され、**Keep** パラメーターが省略される場合にのみ有効です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](./About/about_CommonParameters.md)」を参照してください。

## 入力

### システム管理. ジョブ

パイプを使用してジョブオブジェクトをこのコマンドレットに渡します。

## 出力

### PSObject

このコマンドレットは、ジョブのコマンドの結果を返します。

## 注

## 関連リンク

[Get-Job](Get-Job.md)

[Invoke-Command](Invoke-Command.md)

[Remove-Job](Remove-Job.md)

[Start-Job](Start-Job.md)

[Stop-Job](Stop-Job.md)

[Wait-Job](Wait-Job.md)

