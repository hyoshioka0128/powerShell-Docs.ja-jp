---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/suspend-job?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Suspend-Job
ms.openlocfilehash: 9b18782fae77fa0776aa2cfaa39b74a2292099d9
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388469"
---
# Suspend-Job

## 概要
ワークフロー ジョブを一時的に停止します。

## SYNTAX

### SessionIdParameterSet (既定値)

```
Suspend-Job [-Force] [-Wait] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### JobParameterSet

```
Suspend-Job [-Job] <Job[]> [-Force] [-Wait] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameParameterSet

```
Suspend-Job [-Force] [-Wait] [-Name] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceIdParameterSet

```
Suspend-Job [-Force] [-Wait] [-InstanceId] <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### FilterParameterSet

```
Suspend-Job [-Force] [-Wait] [-Filter] <Hashtable> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### StateParameterSet

```
Suspend-Job [-Force] [-Wait] [-State] <JobState> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Suspend-Job` ワークフロージョブを中断します。 中断とは、一時的にワークフロージョブを中断または一時停止することを意味します。 ワークフローを実行しているユーザーは、このコマンドレットを使用してワークフローを中断できます。 ワークフローを中断するワークフロー内のコマンドである、中断ワークフローアクティビティを補完し https://go.microsoft.com/fwlink/?LinkId=267141 ます。

`Suspend-Job`コマンドレットは、ワークフロージョブに対してのみ機能します。 コマンドレットを使用して開始されたものなど、標準のバックグラウンドジョブでは機能しません `Start-Job` 。

ワークフロー ジョブを識別するには、ジョブの PSJobTypeName プロパティで **PSWorkflowJob** の値を検索します。 特定のカスタムジョブの種類がコマンドレットをサポートしているかどうかを判断するには、 `Suspend-Job` カスタムジョブの種類のヘルプトピックを参照してください。

ワークフロー ジョブを中断すると、ワークフロー ジョブは次のチェックポイントまで実行された後で中断されます。その後、ワークフロー ジョブ オブジェクトが直ちに返されます。 ジョブを取得する前に中断が完了するまで待機するに **Wait** `Suspend-Job` は、またはコマンドレットの wait パラメーターを使用し `Wait-Job` ます。 ワークフロージョブが中断されている場合、ジョブの **State** プロパティの値は中断されます。

正しく中断できるかどうかはチェックポイントに依存します。 現在のジョブの状態、メタデータ、および出力がチェックポイントに保存されるため、状態やデータを失うことなくワークフロージョブを再開できます。 ワークフロージョブにチェックポイントがない場合、正常に中断することはできません。 実行中のワークフローにチェックポイントを追加するには、 **PSPersist** ワークフロー共通パラメーターを使用します。 **Force** パラメーターを使用すると、ワークフロージョブを即座に中断し、チェックポイントのないワークフロージョブを中断できますが、操作によって状態とデータが失われる可能性があります。

ワークフロージョブ ( **Psworkflowjob** ) などのカスタムジョブの種類で job コマンドレットを使用する前に、カスタムジョブの種類をサポートするモジュールをインポートします。そのためには、コマンドレットを使用するか、 `Import-Module` またはモジュールのコマンドレットを使用します。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: 名前を指定してワークフロージョブを中断する

この例では、ワークフロー ジョブを中断する方法を示します。

最初のコマンドは、ワークフローを作成し `Get-SystemLog` ます。 ワークフローは、アクティビティを使用して `CheckPoint-Workflow` ワークフローのチェックポイントを定義します。

2番目のコマンドは、すべてのワークフローに共通の **AsJob** パラメーターを使用し `Get-SystemLog` て、ワークフローをバックグラウンドジョブとして実行します。 このコマンドは、 **JobName** ワークフロー共通パラメーターを使用して、ワークフロー ジョブのフレンドリ名を指定します。

3番目のコマンドは、 `Get-Job` コマンドレットを使用してワークフロージョブを取得し `Get-SystemLogJob` ます。 出力には、 **PSJobTypeName** プロパティの値が PSWorkflowJob であることが示されています。

4番目のコマンドは、 `Suspend-Job` コマンドレットを使用してジョブを中断し `Get-SystemLogJob` ます。 ジョブはチェックポイントまで実行された後、中断されます。

```
#Sample WorkflowWorkflow Get-SystemLog
{
    $Events = Get-WinEvent -LogName System
    CheckPoint-Workflow
    InlineScript {\\Server01\Scripts\Analyze-SystemEvents.ps1 -Events $Events}
}

PS C:\> Get-SystemLog -AsJob -JobName "Get-SystemLogJob"

PS C:\> Get-Job -Name Get-SystemLogJob
Id     Name              PSJobTypeName   State       HasMoreData     Location   Command
--     ----              -------------   -----       -----------     --------   -------
4      Get-SystemLogJob  PSWorkflowJob   Running     True            localhost   Get-SystemLog

PS C:\> Suspend-Job -Name Get-SystemLogJob
Id     Name              PSJobTypeName   State       HasMoreData     Location   Command
--     ----              -------------   -----       -----------     --------   -------
4      Get-SystemLogJob  PSWorkflowJob   Suspended   True            localhost   Get-SystemLog
```


### 例 2: ワークフロージョブを中断および再開する

この例では、ワークフロー ジョブを中断し、再開する方法を示します。

最初のコマンドは、LogWorkflowJob ジョブを中断します。コマンドは直ちに返されます。 出力は、中断されている場合でも、ワークフロージョブがまだ実行中であることを示しています。

2番目のコマンドは、 `Get-Job` コマンドレットを使用して LogWorkflowJob ジョブを取得します。 出力には、ジョブが正常に完了したことが示されています。

3番目のコマンドは、コマンドレットを使用して `Get-Job` LogWorkflowJob ジョブを取得し、コマンドレットを使用してジョブを `Resume-Job` 再開します。 出力には、ワークフロー ジョブが正常に再開され、現在実行中であることが示されています。

```
PS C:\> Suspend-Job -Name LogWorkflowJob
Id     Name          PSJobTypeName      State         HasMoreData     Location             Command
--     ----          -------------      -----         -----------     --------             -------
67     LogflowJob    PSWorkflowJob      Running       True            localhost            LogWorkflow

PS C:\> Get-Job -Name LogWorkflowJob
Id     Name          PSJobTypeName      State         HasMoreData     Location             Command
--     ----          -------------      -----         -----------     --------             -------
67     LogflowJob    PSWorkflowJob      Suspended     True            localhost            LogWorkflow

PS C:\> Get-Job -Name LogWorkflowJob | Resume-Job
Id     Name          PSJobTypeName      State         HasMoreData     Location             Command
--     ----          -------------      -----         -----------     --------             -------
67     LogflowJob    PSWorkflowJob      Running       True            localhost            LogWorkflow
```


### 例 3: リモートコンピューター上のワークフロージョブを中断する

```
PS C:\> Invoke-Command -ComputerName Srv01 -Scriptblock {Suspend-Job -Filter @{CustomID="031589"}
```

このコマンドは、 `Invoke-Command` コマンドレットを使用して、Srv01 リモートコンピューター上のワークフロージョブを中断します。 **Filter** パラメーターの値は、customid 値を指定するハッシュテーブルです。
この **CustomID** は、ジョブのメタデータ ( **PSPrivateMetadata** ) です。

### 例 4: ワークフロージョブが中断されるまで待機する

```
PS C:\> Suspend-Job VersionCheck -Wait
Id     Name          PSJobTypeName      State         HasMoreData     Location             Command
--     ----          -------------      -----         -----------     --------             -------
 5     VersionCheck  PSWorkflowJob      Suspended     True            localhost            LogWorkflow
```

このコマンドは、VersionCheck ワークフロー ジョブを中断します。 このコマンドでは、 **Wait** パラメーターを使用して、ワークフロー ジョブが中断されるのを待機しています。 ワークフロージョブが次のチェックポイントまで実行され、中断されると、コマンドが完了し、ジョブオブジェクトが返されます。

### 例 5: ワークフロージョブを強制的に中断する

```
PS C:\> Suspend-Job Maintenance -Force
```

このコマンドは、強制的に Maintenance ワークフロー ジョブを中断します。 メンテナンスジョブにチェックポイントがありません。 正しく中断することができず、正常に再開されない可能性があります。

## PARAMETERS

### -Filter

条件のハッシュテーブルを指定します。 このコマンドレットは、すべての条件に適合するジョブを中断します。
ジョブのプロパティをキー、ジョブのプロパティ値を値とするハッシュ テーブルを入力します。

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

ワークフロー ジョブを即座に中断します。 この操作により、状態とデータが失われる可能性があります。

既定では、 `Suspend-Job` ワークフロージョブは次のチェックポイントまで実行され、その後中断されます。
このパラメーターを使用すると、チェックポイントがないワークフロー ジョブも中断できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: F

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

このコマンドレットが中断するジョブの Id を指定します。

ID は、現在のセッションでジョブを一意に識別する整数です。 インスタンス ID よりも覚えやすく、入力する方が簡単ですが、現在のセッションでのみ一意です。 1つ以上の Id をコンマで区切って入力できます。 ジョブの ID を検索するには、コマンドレットを使用し `Get-Job` ます。

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

このコマンドレットが中断するジョブのインスタンス Id を指定します。 既定値はすべてのジョブです。

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

### -ジョブ

このコマンドレットが停止するワークフロージョブを指定します。 ワークフロー ジョブが格納されている変数、またはワークフロー ジョブを取得するコマンドを入力します。 ワークフロージョブをコマンドレットにパイプ処理することもでき `Suspend-Job` ます。

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

このコマンドレットが中断するジョブのフレンドリ名を指定します。 1 つまたは複数のワークフロー ジョブ名を入力します。
ワイルドカード文字がサポートされています。

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
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

`Suspend-Job`**実行中** の状態のワークフロージョブのみを中断します。

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

### -Wait

ワークフロージョブが中断状態になるまで、このコマンドレットによってコマンドプロンプトが抑制されることを示します。 既定では、 `Suspend-Job` ワークフロージョブがまだ中断状態になっていない場合でも、は直ちにを返します。

**Wait** パラメーターは、 `Suspend-Job` コマンドをコマンドレットにパイプ処理することと同じです `Wait-Job` 。

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

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

このコマンドレットには、すべての種類のジョブをパイプすることができます。 ただし、が `Suspend-Job` サポートされていない種類のジョブを取得すると、終了エラーが返されます。

## 出力

### システム管理. ジョブ
このコマンドレットは、中断されたジョブを返します。

## 注

- 中断されているジョブを保存するメカニズムと保存先の場所は、ジョブの種類によって異なる可能性があります。 たとえば、中断されているワークフロー ジョブは、既定でフラット ファイル ストアに保存されますが、データベースに保存することもできます。
- 実行状態ではないワークフロージョブを送信すると、に `Suspend-Job` 警告メッセージが表示されます。 警告を抑制するには、値が SilentlyContinue の **Warnings action** 共通パラメーターを使用します。

  ジョブが中断をサポートする種類ではない場合、は `Suspend-Job` 終了エラーを返します。

- 中断されているワークフロージョブ (このコマンドレットによって中断されたジョブを含む) を検索するには、コマンドレットの **state** パラメーターを使用して `Get-Job` 、中断状態のワークフロージョブを取得します。
- 一部のジョブの種類には、Windows PowerShell によるジョブの中断が許可されないオプションまたはプロパティを持つもあります。
  ジョブを中断する試みが失敗した場合は、ジョブのオプションとプロパティで中断が許可されていることを確認します。

## 関連リンク

[Get-Job](Get-Job.md)

[Receive-Job](Receive-Job.md)

[Remove-Job](Remove-Job.md)

[Resume-Job](Resume-Job.md)

[Start-Job](Start-Job.md)

[Stop-Job](Stop-Job.md)

[Suspend-Job](Suspend-Job.md)

[Wait-Job](Wait-Job.md)
