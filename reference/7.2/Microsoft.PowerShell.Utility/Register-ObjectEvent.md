---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/27/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/register-objectevent?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-ObjectEvent
ms.openlocfilehash: 89f4d30df0dbc1b74ba28604ed686cad64a3a594
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599901"
---
# Register-ObjectEvent

## 概要
Microsoft .NET Framework オブジェクトによって生成されるイベントをサブスクライブします。

## SYNTAX

```
Register-ObjectEvent [-InputObject] <PSObject> [-EventName] <String> [[-SourceIdentifier] <String>]
 [[-Action] <ScriptBlock>] [-MessageData <PSObject>] [-SupportEvent] [-Forward] [-MaxTriggerCount <Int32>]
 [<CommonParameters>]
```

## Description

`Register-ObjectEvent`コマンドレットは、ローカルコンピューターまたはリモートコンピューター上の .net オブジェクトによって生成されるイベントをサブスクライブします。

サブスクライブされているイベントが発生すると、セッションのイベント キューにそのイベントが追加されます。 イベントキュー内のイベントを取得するには、コマンドレットを使用し `Get-Event` ます。

のパラメーターを使用して、 `Register-ObjectEvent` キュー内のイベントを識別するのに役立つイベントのプロパティ値を指定できます。 また、 **Action** パラメーターを使用して、サブスクライブされたイベントが発生したときに実行するアクションを指定し、 **Forward** パラメーターを使用してローカルセッションのイベントキューにリモートイベントを送信することもできます。

イベントにサブスクライブするとき、イベント サブスクライバーがセッションに追加されます。 セッションのイベント サブスクライバーを取得するには、`Get-EventSubscriber` コマンドレットを使用します。 サブスクリプションをキャンセルするには、`Unregister-Event` コマンドレットを使用して、セッションからイベント サブスクライバーを削除します。

## 例

### 例 1: 新しいプロセスの開始時にイベントをサブスクライブする

この例は、新しいプロセスの開始時に生成されたイベントをサブスクライブします。

このコマンドは、 **Managementeventwatcher** オブジェクトを使用して、 **event到着** イベントを取得します。 クエリオブジェクトは、イベントが **Win32_Process** クラスのインスタンス作成イベントであることを指定します。

```powershell
$queryParameters = '__InstanceCreationEvent', (New-Object TimeSpan 0,0,1),
    "TargetInstance isa 'Win32_Process'"
$Query = New-Object System.Management.WqlEventQuery -ArgumentList $queryParameters
$ProcessWatcher = New-Object System.Management.ManagementEventWatcher $Query
Register-ObjectEvent -InputObject $ProcessWatcher -EventName "EventArrived"
```

### 例 2: イベントに応答するアクションを指定する

アクションを指定した場合には、発生するイベントはイベント キューに追加されません。 その代わり、指定したアクションが発生したイベントに応答します。 この例では、新しいプロセスが開始されたことを示すインスタンス作成イベントが発生すると、新しい **Processcreated** イベントが発生します。

```powershell
$queryParameters = '__InstanceCreationEvent', (New-Object TimeSpan 0,0,1),
    "TargetInstance isa 'Win32_Process'"
$Query = New-Object System.Management.WqlEventQuery -ArgumentList $queryParameters
$ProcessWatcher = New-Object System.Management.ManagementEventWatcher $query
$newEventArgs = @{
    SourceIdentifier = 'PowerShell.ProcessCreated'
    Sender = $Sender
    EventArguments = $EventArgs.NewEvent.TargetInstance
}
$Action = { New-Event @newEventArgs }
Register-ObjectEvent -InputObject $ProcessWatcher -EventName "EventArrived" -Action $Action
```

```Output
Id   Name               PSJobTypeName   State       HasMoreData   Location   Command
--   ----               -------------   -----       -----------   --------   -------
 5   3db2d67a-efff-...                 NotStarted   False                    New-Event @newEventArgs
```

アクションは、 `$Sender` `$EventArgs` イベントアクションに対してのみ設定されるおよび自動変数を使用します。

この `Register-ObjectEvent` コマンドは、バックグラウンドジョブとして実行されるアクションを表す job オブジェクトを返します。 やなどのジョブコマンドレットを使用して、 `Get-Job` `Receive-Job` バックグラウンドジョブを管理できます。 詳細については、「[about_Jobs](../Microsoft.PowerShell.Core/About/about_Jobs.md)」を参照してください。

### 例 3: リモートコンピューター上のオブジェクトイベントをサブスクライブする

この例は、リモート コンピューター上のオブジェクト イベントをサブスクライブする方法を示しています。 この例では、 `Enable-ProcessCreationEvent` スクリプトファイルで定義されている関数を使用し `ProcessCreationEvent.ps1` ます。 このスクリプトは、この例のすべてのコンピューターで使用できます。

```powershell
# ProcessCreationEvent.ps1
function  Enable-ProcessCreationEvent {
    $queryParameters = "__InstanceCreationEvent", (New-Object TimeSpan 0,0,1),
        "TargetInstance isa 'Win32_Process'"
    $Query = New-Object System.Management.WqlEventQuery -ArgumentList $queryParameters

    $objectEventArgs = @{
        Input = New-Object System.Management.ManagementEventWatcher $Query
        EventName = 'EventArrived'
        SourceIdentifier = 'WMI.ProcessCreated'
        MessageData = 'Test'
        Forward = $True
    }
    Register-ObjectEvent @objectEventArgs
}

$S = New-PSSession -ComputerName "Server01, Server02"
Invoke-Command -Session $S -FilePath ProcessCreationEvent.ps1
Invoke-Command -Session $S { Enable-ProcessCreationEvent }
```

最初に、2 **台のリモートコンピューター上に** pssession を作成し、変数に保存し `$S` ます。 次に、コマンドレットによって、の `Invoke-Command` 各 pssession でスクリプトが実行され `ProcessCreationEvent.ps1` `$S` ます。 この操作により、 `Enable-ProcessCreationEvent` リモートセッションで関数が作成されます。
最後に、 `Enable-ProcessCreationEvent` リモートセッションで関数を実行します。

 関数には、 `Register-ObjectEvent` **Managementeventwatcher** オブジェクトとその **event到着** イベントを介して、 **Win32_Process** オブジェクトでインスタンス作成イベントをサブスクライブするコマンドが含まれています。

### 例 4: PSEventJob オブジェクトで動的モジュールを使用する

この例では、イベント登録に **アクション** を含めるときに作成される **PSEventJob** オブジェクトで動的モジュールを使用する方法を示します。 まず、タイマーオブジェクトを使用して有効にし、タイマーの間隔を 500 (ミリ秒) に設定します。 `Register-ObjectEvent`コマンドレットは、timer オブジェクトの **Elapsed** イベントを登録します。 **PSEventJob** オブジェクトは変数に保存され、 `$Job` イベントサブスクライバーの **Action** プロパティでも使用できます。 詳細については、「 [Get-EventSubscriber](Get-EventSubscriber.md)」を参照してください。

タイマー間隔が経過するたびに、イベントが発生し、アクションが実行されます。 この場合、 `Get-Random` コマンドレットは 0 ~ 100 の範囲の乱数を生成し、変数に保存し `$Random` ます。

```powershell
$Timer = New-Object Timers.Timer
$Timer.Interval = 500
$Timer.Enabled = $True
$objectEventArgs = @{
    InputObject = $Timer
    EventName = 'Elapsed'
    SourceIdentifier = 'Timer.Random'
    Action = {$Random = Get-Random -Min 0 -Max 100}
}
$Job = Register-ObjectEvent @objectEventArgs
$Job | Format-List -Property *
& $Job.module {$Random}
& $Job.module {$Random}
```

```Output
State         : Running
Module        : __DynamicModule_53113769-31f2-42dc-830b-8749325e28d6
StatusMessage :
HasMoreData   : True
Location      :
Command       : $Random = Get-Random -Min 0 -Max 100
JobStateInfo  : Running
Finished      : System.Threading.ManualResetEvent
InstanceId    : 47b5ec9f-bfe3-4605-860a-4674e5d44ca8
Id            : 7
Name          : Timer.Random
ChildJobs     : {}
PSBeginTime   : 6/27/2019 10:19:06 AM
PSEndTime     :
PSJobTypeName :
Output        : {}
Error         : {}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
Information   : {}
60
47
```

**PSEventJob** には、アクションを実装する動的スクリプトモジュールを含む **Module** プロパティがあります。 呼び出し演算子 () を使用して、 `&` モジュール内のコマンドを呼び出し、変数の値を表示し `$Random` ます。

モジュールの詳細については、「 [about_Modules](../Microsoft.PowerShell.Core/About/about_Modules.md)」を参照してください。

## PARAMETERS

### -Action

イベントを処理するコマンドを指定します。 **Action** で指定したコマンドは、イベント キューにイベントを送信する時点ではなく、イベントが発生した時点で実行されます。 コマンドを中かっこ ({ }) で囲み、スクリプト ブロックを作成します。

**Action** パラメーターの値には `$Event` 、、、 `$EventSubscriber` `$Sender` 、 `$EventArgs` 、および `$Args` 自動変数を含めることができます。 これらの変数は、イベントに関する情報を **Action** スクリプトブロックに提供します。 詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

アクションを指定すると、は `Register-ObjectEvent` そのアクションを表すイベントジョブオブジェクトを返します。 Job コマンドレットを使用して、イベント ジョブを管理できます。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: False
Position: 101
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EventName

サブスクライブするイベントを指定します。

このパラメーターの値は、.NET オブジェクトが公開するイベントの名前である必要があります。 たとえば、 **Managementeventwatcher** クラスには、 **Event到着** と **停止** という名前のイベントがあります。 イベントのイベント名を検索するには、コマンドレットを使用し `Get-Member` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -転送

コマンドレットがこのサブスクリプションのイベントをリモートセッションに送信することを示します。 このパラメーターは、リモート コンピューターまたはリモート セッションのイベントに登録する場合に使用します。

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

### -InputObject

イベントを生成する .NET オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaxTriggerCount

イベントをトリガーできる最大回数を指定します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MessageData

このイベント サブスクリプションに関連付けられる任意の追加データを指定します。 このパラメーターの値は、このサブスクリプションに関連付けられているすべてのイベントの MessageData プロパティに表示されます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceIdentifier

サブスクリプション用に選択した名前を指定します。 選択した名前は、現在のセッションで一意でなければなりません。 既定値は、PowerShell によって割り当てられる GUID です。

このパラメーターの値は、サブスクライバーオブジェクトの **SourceIdentifier** プロパティの値と、このサブスクリプションに関連付けられているすべてのイベントオブジェクトの値に表示されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 100
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SupportEvent

コマンドレットによってイベントサブスクリプションが非表示になることを示します。 現在のサブスクリプションがより複雑なイベント登録メカニズムの一部であり、個別に検出されない場合は、このパラメーターを使用します。

**Supportevent** パラメーターを使用して作成されたサブスクリプションを表示またはキャンセルするには、 `Get-EventSubscriber` コマンドレットとコマンドレットの Force パラメーターを使用し `Unregister-Event` ます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してオブジェクトをにパイプすることはできません `Register-ObjectEvent` 。

## 出力

### None または PSEventJob。

**Action** パラメーターを使用すると、は `Register-ObjectEvent` **PSEventJob** オブジェクトを返します。 それ以外の場合、出力は生成されません。

## 注

イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

## 関連リンク

[Get-Event](Get-Event.md)

[New-Event](New-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)

