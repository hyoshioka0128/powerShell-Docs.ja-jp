---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/resume-job?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Resume-Job
ms.openlocfilehash: 6d08d9053e100cb53d37a6e4931d118f90c35a6a
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388537"
---
# Resume-Job

## 概要
中断されたジョブを再開します。

## SYNTAX

### SessionIdParameterSet (既定値)

```
Resume-Job [-Wait] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### JobParameterSet

```
Resume-Job [-Job] <Job[]> [-Wait] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameParameterSet

```
Resume-Job [-Wait] [-Name] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceIdParameterSet

```
Resume-Job [-Wait] [-InstanceId] <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### StateParameterSet

```
Resume-Job [-Wait] [-State] <JobState> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### FilterParameterSet

```
Resume-Job [-Wait] [-Filter] <Hashtable> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレット `Resume-Job` `Suspend-Job` は、コマンドレットや [about_Suspend ワークフロー](../PSWorkflow/about/about_Suspend-Workflow.md) アクティビティなどを使用して、中断されたワークフロージョブを再開します。 ワークフロージョブが再開されると、ジョブエンジンによって、状態、メタデータ、および保存されたリソース (チェックポイントなど) からの出力が再構築されます。 ジョブは、状態またはデータを失うことなく再開されます。
ジョブの状態は、 **Suspended** から **Running** に変更されます。

のパラメーターを使用し `Resume-Job` て、名前、ID、インスタンス ID、またはパイプを使用してジョブを選択し `Get-Job` ます (コマンドレットによって返されるものなど) `Resume-Job` 。 プロパティ フィルターを使用して再開するジョブを選択することもできます。

既定では、 `Resume-Job` すべてのジョブがまだ再開されていない場合でも、は直ちにを返します。 指定したすべてのジョブが再開されるまでコマンド プロンプトが表示されないようにするには、 **Wait** パラメーターを使用します。

`Resume-Job`コマンドレットは、ワークフロージョブなど、カスタムのジョブの種類でのみ機能します。 コマンドレットを使用して開始されたものなど、標準のバックグラウンドジョブでは機能しません `Start-Job` 。 サポートされていない種類のジョブを送信すると、は `Resume-Job` 終了エラーを生成し、実行を停止します。

ワークフロー ジョブを識別するには、ジョブの **PSJobTypeName** プロパティで **PSWorkflowJob** の値を検索します。 特定のカスタムジョブの種類がコマンドレットをサポートしているかどうかを判断するには、 `Resume-Job` カスタムジョブの種類のヘルプトピックを参照してください。

カスタムのジョブの種類で Job コマンドレットを使用する前に、カスタムのジョブの種類をサポートするモジュールをインポートします。そのためには、コマンドレットを使用するか、 `Import-Module` モジュールのコマンドレットを取得または使用します。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: ID でジョブを再開する

この例のコマンドは、ジョブが中断状態のワークフロー ジョブであることを確認した後、このジョブを再開します。 最初のコマンドは、 `Get-Job` コマンドレットを使用してジョブを取得します。 出力に、ジョブが中断状態のワークフロー ジョブであることが示されています。 2番目のコマンドは、コマンドレットの **id** パラメーターを使用して `Resume-Job` 、 **id** 値が4のジョブを再開します。

```
PS C:\> Get-Job EventJob
Id     Name            PSJobTypeName   State         HasMoreData     Location   Command
--     ----            -------------   -----         -----------     --------   -------
4      EventJob        PSWorkflowJob   Suspended     True            Server01   \\Script\Share\Event.ps1

PS C:\> Resume-Job -Id 4
```

### 例 2: 名前を指定してジョブを再開する

このコマンドは、 **Name** パラメーターを使用して、ローカル コンピューター上の複数のワークフロー ジョブを再開します。

```
PS C:\> Resume-Job -Name WorkflowJob, InventoryWorkflow, WFTest*
```

### 例 3: カスタムプロパティ値を使用する

このコマンドは、カスタム プロパティ値を使用して、再開するワークフロー ジョブを識別します。 このコマンドは、 **Filter** パラメーターを使用して、 **CustomID** プロパティでワークフロー ジョブを識別します。 さらに、 **State** パラメーターを使用して、再開操作を開始する前に、ワークフロー ジョブが中断状態にあることを確認します。

```
PS C:\> Resume-Job -Filter @{CustomID="T091291"} -State Suspended
```

### 例 4: リモートコンピューター上のすべての中断されたジョブを再開する

このコマンドは、Srv01 リモート コンピューター上のすべての中断されているジョブを再開します。

```
PS C:\> Invoke-Command -ComputerName Srv01 -ScriptBlock {Get-Job -State Suspended | Resume-Job}
```

このコマンドは、コマンドレットを使用して、 `Invoke-Command` Srv01 コンピューターでコマンドを実行します。 Remote コマンドは、コマンドレットの **State** パラメーターを使用して `Get-Job` 、コンピューター上のすべての中断されたジョブを取得します。 パイプライン演算子 () は、中断された `|` ジョブをコマンドレットに送信し `Resume-Job` ます。これにより、ジョブが再開されます。

### 例 5: ジョブが再開されるまで待機する

このコマンドは、 **Wait** `Resume-Job` 指定されたすべてのジョブが再開された後にのみ、Wait パラメーターを使用してを返すように指示します。 **Wait** パラメーターは、ジョブが再開された後でスクリプトが続行されることを前提としているスクリプトで特に便利です。

```
PS C:\> Resume-Job -Name WorkflowJob, InventoryWorkflow, WFTest* -Wait
```

### 例 6: 自身を中断するワークフローを再開する

このコードサンプルは、 `Suspend-Workflow` ワークフロー内のアクティビティを示しています。

`Test-Suspend`Server01 コンピューター上のワークフロー。 ワークフローを実行すると、ワークフローはアクティビティを実行 `Get-Date` し、結果を変数に格納し `$a` ます。 次に、アクティビティを実行し `Suspend-Workflow` ます。 このアクティビティは、チェックポイントを取得し、ワークフローを中断した後、ワークフロー ジョブ オブジェクトを返します。 `Suspend-Workflow` ワークフローがジョブとして明示的に実行されていない場合でも、ワークフロージョブオブジェクトを返します。

`Resume-Job``Test-Suspend`Job8 のワークフローを再開します。 ここでは、 **Wait** パラメーターを使用して、ジョブが再開されるまでコマンド プロンプトを保持しています。

コマンドレットでは、 `Receive-Job` ワークフローの結果を取得し `Test-Suspend` ます。 ワークフローの最後のコマンドは、現在の日付と時刻の間の経過時間と、ワークフローが中断される前に変数に保存された日時を表す **TimeSpan** オブジェクトを返し `$a` ます。

```
#SampleWorkflow
Workflow Test-Suspend
{
    $a = Get-Date
    Suspend-Workflow
    (Get-Date)- $a
}

PS C:\> Test-Suspend -PSComputerName Server01
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
8      Job8            PSWorkflowJob   Suspended     True            Server01             Test-Suspend

PS C:\> Resume-Job -Name "Job8" -Wait
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
8      Job8            PSWorkflowJob   Running       True            Server01             Test-Suspend

PS C:\> Receive-Job -Name Job8
        Days              : 0
        Hours             : 0
        Minutes           : 0
        Seconds           : 19
        Milliseconds      : 823
        Ticks             : 198230041
        TotalDays         : 0.000229432917824074
        TotalHours        : 0.00550639002777778
        TotalMinutes      : 0.330383401666667
        TotalSeconds      : 19.8230041
        TotalMilliseconds : 19823.0041
        PSComputerName    : Server01
```

`Resume-Job`コマンドレットを使用すると、アクティビティを使用して中断されたワークフロージョブを再開でき `Suspend-Workflow` ます。 このアクティビティは、ワークフロー内からワークフローを中断します。 ワークフローでのみ有効です。

の詳細については `Suspend-Workflow` 、「about_Suspend」 (.) を参照してください。/Psworkflow/about_Suspend/)。

## PARAMETERS

### -Filter

条件のハッシュテーブルを指定します。 このコマンドレットは、ハッシュテーブル内のすべての条件に適合するジョブを再開します。 ジョブのプロパティをキー、ジョブのプロパティ値を値とするハッシュ テーブルを入力します。

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

このコマンドレットが再開するジョブの Id の配列を指定します。

ID は、現在のセッションでジョブを一意に識別する整数です。 インスタンス ID よりも覚えやすく、入力する方が簡単ですが、現在のセッションでのみ一意です。 1つ以上の Id をコンマで区切って入力できます。 ジョブの ID を検索するには、を実行 `Get-Job` します。

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

このコマンドレットによって再開されるジョブのインスタンス Id の配列を指定します。 既定値はすべてのジョブです。

インスタンス ID は、コンピューター上のジョブを一意に識別する GUID です。 ジョブのインスタンス ID を検索するには、を実行 `Get-Job` します。

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

再開するジョブを指定します。 ジョブが格納されている変数、またはジョブを取得するコマンドを入力します。 パイプを使用してジョブをコマンドレットに送ることもでき `Resume-Job` ます。

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

このコマンドレットによって再開されるジョブのフレンドリ名の配列を指定します。 1 つまたは複数のジョブ名を入力します。
ワイルドカード文字を使用できます。

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

再開するジョブの状態を指定します。 このパラメーターの有効値は、次のとおりです。

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

このコマンドレットは、 **中断** 状態のジョブのみを再開します。

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

すべてのジョブの結果が再起動されるまで、このコマンドレットによってコマンドプロンプトが抑制されることを示します。 既定では、このコマンドレットは使用可能な結果を直ちに返します。

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

このコマンドレットには、すべての種類のジョブをパイプすることができます。 が `Resume-Job` サポートされていない種類のジョブを取得すると、終了エラーが返されます。

## 出力

### なし、システム管理. ジョブ

このコマンドレットは、 **PassThru** パラメーターを使用する場合に、再開しようとしたジョブを返します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

- `Resume-Job` では、中断されているジョブのみを再開できます。 ジョブを別の状態で送信すると、によってジョブの再開操作が実行されますが、ジョブを再開できなかった `Resume-Job` ことを通知する警告が生成されます。 警告を抑制するには、値が SilentlyContinue の **Warnings action** 共通パラメーターを使用します。
- ジョブがワークフロージョブ ( **Psworkflowjob** ) などの再開をサポートする種類ではない場合、は `Resume-Job` 終了エラーを返します。
- 中断されているジョブを保存するメカニズムと保存先の場所は、ジョブの種類によって異なる可能性があります。 たとえば、中断されているワークフロー ジョブは、既定でフラット ファイル ストアに保存されますが、SQL データベースに保存することもできます。
- ジョブを再開すると、ジョブの状態が **Suspended** から **Running** に変更されます。 このコマンドレットによって再開されたジョブを含め、実行中のジョブを検索するには、コマンドレットの **state** パラメーターを使用し `Get-Job` **て、実行中** の状態のジョブを取得します。
- 一部のジョブの種類には、Windows PowerShell によるジョブの中断が許可されないオプションまたはプロパティを持つもあります。
  ジョブを中断する試みが失敗した場合は、ジョブのオプションとプロパティで中断が許可されていることを確認します。

## 関連リンク

[Get-Job](Get-Job.md)

[Receive-Job](Receive-Job.md)

[Remove-Job](Remove-Job.md)

[Start-Job](Start-Job.md)

[Stop-Job](Stop-Job.md)

[Suspend-Job](Suspend-Job.md)

[Wait-Job](Wait-Job.md)
