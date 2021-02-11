---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/stop-job?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Stop-Job
ms.openlocfilehash: 479c099d2d5daf1cc50c5c645be053ff4ae02bdc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605188"
---
# Stop-Job

## 概要
PowerShell バックグラウンドジョブを停止します。

## SYNTAX

### SessionIdParameterSet (既定値)

```
Stop-Job [-PassThru] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### JobParameterSet

```
Stop-Job [-Job] <Job[]> [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameParameterSet

```
Stop-Job [-PassThru] [-Name] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceIdParameterSet

```
Stop-Job [-PassThru] [-InstanceId] <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### StateParameterSet

```
Stop-Job [-PassThru] [-State] <JobState> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### FilterParameterSet

```
Stop-Job [-PassThru] [-Filter] <Hashtable> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Stop-Job`コマンドレットでは、進行中の PowerShell バックグラウンドジョブを停止します。 このコマンドレットを使用すると、すべてのジョブを停止したり、名前、ID、インスタンス ID、または状態に基づいて選択したジョブを停止したり、ジョブオブジェクトをに渡したりすることができ `Stop-Job` ます。

を使用すると、 `Stop-Job` `Start-Job` コマンドレットまたは任意のコマンドレットの **AsJob** パラメーターを使用して開始されたジョブなど、バックグラウンドジョブを停止できます。 バックグラウンドジョブを停止すると、PowerShell はそのジョブキューで保留中のすべてのタスクを完了し、ジョブを終了します。 このコマンドが送信された後、新しいタスクはキューに追加されません。

このコマンドレットは、バックグラウンド ジョブを削除しません。 ジョブを削除するには、 `Remove-Job` コマンドレットを使用します。

Windows PowerShell 3.0 以降では、は、 `Stop-Job` ワークフロージョブやスケジュールされたジョブのインスタンスなどのカスタムジョブの種類も停止します。 カスタムジョブの種類を使用してジョブを停止できるようにするに `Stop-Job` は、コマンドを実行する前に、コマンドを実行する前に、カスタムジョブの種類をサポートするモジュールをセッションにインポートします。そのためには、 `Stop-Job` コマンドレットを使用するか、 `Import-Module` またはモジュールのコマンドレットを取得します。 特定のカスタム ジョブの種類については、カスタムのジョブの種類機能のドキュメントを参照してください。

## 例

### 例 1: Invoke-Command を使用してリモートコンピューター上のジョブを停止する

```powershell
$s = New-PSSession -ComputerName Server01 -Credential Domain01\Admin02
$j = Invoke-Command -Session $s -ScriptBlock {Start-Job -ScriptBlock {Get-EventLog System}}
Invoke-Command -Session $s -ScriptBlock { Stop-job -Job $Using:j }
```

この例では、コマンドレットを使用して、 `Stop-Job` リモートコンピューター上で実行されているジョブを停止する方法を示します。

コマンドレットを使用してジョブをリモートで実行することによってジョブが開始されたため `Invoke-Command` `Start-Job` 、ジョブオブジェクトはリモートコンピューターに格納されます。 `Invoke-Command`コマンドをリモートで実行するには、別のコマンドを使用する必要があり `Stop-Job` ます。 リモート バックグラウンド ジョブの詳細については、「about_Remote_Jobs」を参照してください。

最初のコマンドは、Server01 コンピューター上に PowerShell セッション (**PSSession**) を作成し、そのセッションオブジェクトを変数に格納し `$s` ます。 このコマンドは、ドメイン管理者の資格情報を使用します。

2番目のコマンドは、コマンドレットを使用して、 `Invoke-Command` `Start-Job` セッションでコマンドを実行します。 ジョブのコマンドは、システム イベント ログ内のすべてのイベントを取得します。 結果として得られるジョブオブジェクトは変数に格納され `$j` ます。

3 番目のコマンドは、ジョブを停止します。 コマンドレットを使用して、 `Invoke-Command` `Stop-Job` Server01 上の **PSSession** でコマンドを実行します。 ジョブオブジェクトはローカルコンピューター上の変数であるに格納されるので、このコマンドは、Using スコープ修飾子を使用して、 `$j` `$j` ローカル変数としてを識別します。
スコープ修飾子の使用の詳細については、「 [about_Remote_Variables](about/about_Remote_Variables.md)」を参照してください。

コマンドが完了すると、ジョブが停止し、の **PSSession** を `$s` 使用できるようになります。

### 例 2: バックグラウンドジョブを停止する

```powershell
Stop-Job -Name "Job1"
```

このコマンドは、Job1 バックグラウンド ジョブを停止します。

### 例 3: いくつかのバックグラウンドジョブを停止する

```powershell
Stop-Job -Id 1, 3, 4
```

このコマンドは、3 つのジョブを停止します。
ID によって識別しています。

### 例 4: すべてのバックグラウンドジョブを停止する

```powershell
Get-Job | Stop-Job
```

このコマンドは、現在のセッションのすべてのバックグラウンド ジョブを停止します。

### 例 5: すべてのブロックされたバックグラウンドジョブを停止する

```powershell
Stop-Job -State Blocked
```

このコマンドは、すべてのブロックされているジョブを停止します。

### 例 6: インスタンス ID を使用してジョブを停止する

```powershell
Get-Job | Format-Table ID, Name, Command, @{Label="State";Expression={$_.JobStateInfo.State}},
InstanceID -Auto
```

```Output
Id Name Command                 State  InstanceId
-- ---- -------                 -----  ----------
1 Job1 start-service schedule Running 05abb67a-2932-4bd5-b331-c0254b8d9146
3 Job3 start-service schedule Running c03cbd45-19f3-4558-ba94-ebe41b68ad03
5 Job5 get-service s*         Blocked e3bbfed1-9c53-401a-a2c3-a8db34336adf
```

```powershell
Stop-Job -InstanceId e3bbfed1-9c53-401a-a2c3-a8db34336adf
```

これらのコマンドは、インスタンス ID に基づいてジョブを停止する方法を示しています。

最初のコマンドは、 `Get-Job` コマンドレットを使用して、現在のセッションのジョブを取得します。 このコマンドは、パイプライン演算子 () を使用し `|` てコマンドにジョブを送信し `Format-Table` ます。これにより、各ジョブの指定されたプロパティのテーブルが表示されます。 この表には各ジョブのインスタンス ID が含まれます。 ジョブの状態を表示するには、集計プロパティを使用します。

2番目のコマンドは、InstanceID パラメーターを持つコマンドを使用して、 `Stop-Job` 選択したジョブを停止します。 

### 例 7: リモートコンピューター上のジョブを停止する

```powershell
$j = Invoke-Command -ComputerName Server01 -ScriptBlock {Get-EventLog System} -AsJob
$j | Stop-Job -PassThru
```

```Output
Id    Name    State      HasMoreData     Location         Command
--    ----    ----       -----------     --------         -------
5     Job5    Stopped    True            user01-tablet    get-eventlog system
```

この例では、コマンドレットを使用して、 `Stop-Job` リモートコンピューター上で実行されているジョブを停止する方法を示します。

ジョブはコマンドレットの **AsJob** パラメーターを使用して開始されたため、ジョブが `Invoke-Command` リモートコンピューター上で実行されている場合でも、ジョブオブジェクトはローカルコンピューターに配置されます。 そのため、ローカルコマンドを使用してジョブを停止することができ `Stop-Job` ます。

最初のコマンドは、 `Invoke-Command` コマンドレットを使用して、Server01 コンピューター上でバックグラウンドジョブを開始します。 このコマンドは、**AsJob** パラメーターを使用して、バックグラウンド ジョブとしてリモート コマンドを実行します。

このコマンドは、ジョブオブジェクトを返します。これは、コマンドレットから返されるものと同じジョブオブジェクトです `Start-Job` 。
このコマンドは、ジョブオブジェクトを変数に保存し `$j` ます。

2番目のコマンドは、パイプライン演算子を使用して、変数内のジョブをに送信し `$j` `Stop-Job` ます。 このコマンドは、 **PassThru** パラメーターを使用して、 `Stop-Job` ジョブオブジェクトを返すように指示します。 ジョブオブジェクトの表示では、ジョブの状態が Stopped であることが確認されます。

リモート バックグラウンド ジョブの詳細については、「about_Remote_Jobs」を参照してください。

## PARAMETERS

### -Filter

条件のハッシュテーブルを指定します。 このコマンドレットは、すべての条件に適合するジョブを停止します。
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

### -Id

このコマンドレットが停止するジョブの Id を指定します。 既定は、現在のセッション内のすべてのジョブです。

ID は、現在のセッションでジョブを一意に識別する整数です。 インスタンス ID よりも覚えやすく、入力も簡単ですが、現在のセッションでのみ一意です。 1つ以上の Id をコンマで区切って入力できます。 ジョブの ID を検索するには、「」と入力 `Get-Job` します。

```yaml
Type: System.Int32[]
Parameter Sets: SessionIdParameterSet
Aliases:

Required: True
Position: 0
Default value: All jobs
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InstanceId

このコマンドレットが停止するジョブのインスタンス Id を指定します。 既定値はすべてのジョブです。

インスタンス ID は、コンピューター上のジョブを一意に識別する GUID です。 ジョブのインスタンス ID を検索するには、を使用 `Get-Job` します。

```yaml
Type: System.Guid[]
Parameter Sets: InstanceIdParameterSet
Aliases:

Required: True
Position: 0
Default value: All jobs
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ジョブ

このコマンドレットが停止するジョブを指定します。 ジョブが格納されている変数、またはジョブを取得するコマンドを入力します。 パイプライン演算子を使用して、コマンドレットにジョブを送信することもでき `Stop-Job` ます。 既定では、は、 `Stop-Job` 現在のセッションで開始されたすべてのジョブを削除します。

```yaml
Type: System.Management.Automation.Job[]
Parameter Sets: JobParameterSet
Aliases:

Required: True
Position: 0
Default value: All jobs
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Name

このコマンドレットが停止するジョブのフレンドリ名を指定します。 コンマ区切りの一覧でジョブの名前を入力するか、ワイルドカード文字 (*) を使用してジョブの名前パターンを入力します。 既定では、は、 `Stop-Job` 現在のセッションで作成されたすべてのジョブを停止します。

フレンドリ名は一意であるとは限らないため、名前によってジョブを停止する場合は、 **WhatIf** パラメーターと **Confirm** パラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: All jobs
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -状態

ジョブの状態を指定します。 このコマンドレットは、指定された状態のジョブのみを停止します。 このパラメーターの有効値は、次のとおりです。

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

ジョブの状態の詳細については、「 [Jobstate 列挙型](/dotnet/api/system.management.automation.jobstate)」を参照してください。

```yaml
Type: System.Management.Automation.JobState
Parameter Sets: StateParameterSet
Aliases:
Accepted values: NotStarted, Running, Completed, Failed, Stopped, Blocked, Suspended, Disconnected, Suspending, Stopping, AtBreakpoint

Required: True
Position: 0
Default value: All jobs
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

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

### -WhatIf

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

### System. Automation. RemotingJob

パイプを使用してジョブオブジェクトをこのコマンドレットに渡します。

## 出力

### なし、システムの管理. PSRemotingJob

**PassThru** パラメーターを指定した場合、このコマンドレットはジョブオブジェクトを返します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

## 関連リンク

[Get-Job](Get-Job.md)

[Invoke-Command](Invoke-Command.md)

[Receive-Job](Receive-Job.md)

[Remove-Job](Remove-Job.md)

[Start-Job](Start-Job.md)

[Wait-Job](Wait-Job.md)

[about_Job_Details](About/about_Job_Details.md)

[about_Remote_Jobs](About/about_Remote_Jobs.md)

[about_Remote_Variables](About/about_Remote_Variables.md)

[about_Jobs](About/about_Jobs.md)

[about_Scopes](About/about_scopes.md)
