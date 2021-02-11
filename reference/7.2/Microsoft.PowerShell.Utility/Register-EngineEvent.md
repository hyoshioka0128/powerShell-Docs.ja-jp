---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 02/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/register-engineevent?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-EngineEvent
ms.openlocfilehash: 64d927907f995613d3e2260e5476b406949bc1aa
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602417"
---
# Register-EngineEvent

## 概要
PowerShell エンジンとコマンドレットによって生成されるイベントをサブスクライブし `New-Event` ます。

## SYNTAX

```
Register-EngineEvent [-SourceIdentifier] <String> [[-Action] <ScriptBlock>] [-MessageData <PSObject>]
 [-SupportEvent] [-Forward] [-MaxTriggerCount <Int32>] [<CommonParameters>]
```

## Description

`Register-EngineEvent`コマンドレットは、PowerShell エンジンとコマンドレットによって生成されるイベントをサブスクライブし `New-Event` ます。 **SourceIdentifier** パラメーターを使用してイベントを指定します。

このコマンドレットを使用すると、 **既存** のエンジンイベントと、このコマンドレットによって生成されるイベントをサブスクライブでき `New-Event` ます。 サブスクライブしない場合、これらのイベントはセッション内のイベント キューに自動的に追加されます。 ただし、サブスクライブすることにより、イベントへの転送、イベントに応答するアクションの指定、サブスクリプションの取り消しを実行できます。

サブスクライブされているイベントが発生すると、セッションのイベント キューにそのイベントが追加されます。 イベントキュー内のイベントを取得するには、コマンドレットを使用し `Get-Event` ます。

イベントをサブスクライブすると、イベントサブスクライバーがセッションに追加されます。 セッションのイベント サブスクライバーを取得するには、`Get-EventSubscriber` コマンドレットを使用します。 サブスクリプションをキャンセルするには、`Unregister-Event` コマンドレットを使用して、セッションからイベント サブスクライバーを削除します。

## 例

### 例 1: リモートコンピューターに PowerShell エンジンイベントを登録する

この例では、2台のリモートコンピューター上の PowerShell エンジンイベントに登録します。

```powershell
$S = New-PSSession -ComputerName "Server01, Server02"
Invoke-Command -Session $S {
Register-EngineEvent -SourceIdentifier ([System.Management.Automation.PsEngineEvent]::Exiting) -Forward
}
```

`New-PSSession` 各リモートコンピューター上にユーザー管理セッション (PSSession) を作成します。 `Invoke-Command` コマンドレットは、 `Register-EngineEvent` リモートセッションでコマンドを実行します。
`Register-EngineEvent`**SourceIdentifier** パラメーターを使用してイベントを識別します。 **Forward** パラメーターは、リモートセッションからローカルセッションにイベントを転送するようにエンジンに指示します。

### 例 2: 終了イベントが発生したときに指定されたアクションを実行する

この例では、を実行し `Register-EngineEvent` て、 **終了** イベントが発生したときに特定のアクションを実行する方法を示します。

```powershell
Register-EngineEvent -SourceIdentifier PowerShell.Exiting -SupportEvent -Action {
    Get-History | Export-Clixml $Home\history.clixml
}
```

イベントサブスクリプションを非表示にするには、 **supportevent** パラメーターを追加します。 PowerShell が終了すると、この場合は、終了したセッションからのコマンド履歴がユーザーのディレクトリにエクスポートされ `$Home` ます。

### 例 3: ユーザー定義イベントを作成してサブスクライブする

この例では、ソース **Myeventsource** からのイベントのサブスクリプションを作成します。 これは、ジョブの進行状況を監視するために使用する任意のソースです。 `Register-EngineEvent` は、サブスクリプションの作成に使用されます。 **Action** パラメーターのスクリプトブロックは、イベントデータをテキストファイルに記録します。

```powershell
Register-EngineEvent -SourceIdentifier MyEventSource -Action {
    "Event: {0}" -f $event.messagedata | Out-File c:\temp\MyEvents.txt -Append
}

Start-Job -Name TestJob -ScriptBlock {
    While ($True) {
        Register-EngineEvent -SourceIdentifier MyEventSource -Forward
        Start-Sleep -seconds 2
        "Doing some work..."
        New-Event -SourceIdentifier MyEventSource -Message ("{0} -  Work done..." -f (Get-Date))
    }
}
Start-Sleep -seconds 4
Get-EventSubscriber
Get-Job
```

```Output
SubscriptionId   : 12
SourceObject     :
EventName        :
SourceIdentifier : MyEventSource
Action           : System.Management.Automation.PSEventJob
HandlerDelegate  :
SupportEvent     : False
ForwardEvent     : False

Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
18     MyEventSource                   Running       True                                 …
19     TestJob         BackgroundJob   Running       True            localhost            …
```

`Register-EngineEvent` ジョブ Id 18 が作成されました。 `Start-Job` ジョブ Id 19 が作成されました。 たとえば #4 では、イベントサブスクリプションとジョブを削除してから、ログファイルを調べます。

### 例 4: イベントの登録を解除し、ジョブをクリーンアップする

これは、例3の継続です。 この例では、10秒間待機して、複数のイベントが発生するようにします。 次に、イベントサブスクリプションの登録を解除します。

```powershell
PS> Start-Sleep -seconds 10
PS> Get-EventSubscriber | Unregister-Event
PS> Get-Job

Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
18     MyEventSource                   Stopped       False                                …
19     TestJob         BackgroundJob   Running       True            localhost            …

PS> Stop-Job -Id 19
PS> Get-Job | Remove-Job
PS> Get-Content C:\temp\MyEvents.txt
Event: 2/18/2020 2:36:19 PM -  Work done...
Event: 2/18/2020 2:36:21 PM -  Work done...
Event: 2/18/2020 2:36:23 PM -  Work done...
Event: 2/18/2020 2:36:25 PM -  Work done...
Event: 2/18/2020 2:36:27 PM -  Work done...
Event: 2/18/2020 2:36:29 PM -  Work done...
Event: 2/18/2020 2:36:31 PM -  Work done...
```

コマンドレットでは、 `Unregister-Event` イベントサブスクリプション (ジョブ Id 18) に関連付けられているジョブを停止します。 ジョブ Id 19 はまだ実行中で、新しいイベントを作成しています。 **Job コマンドレット** を使用してジョブを停止し、不要なジョブオブジェクトを削除します。 `Get-Content` ログファイルの内容を表示します。

## PARAMETERS

### -Action

イベントを処理するコマンドを指定します。 **Action** で指定したコマンドは、イベント キューにイベントを送信する時点ではなく、イベントが発生した時点で実行されます。 コマンドを中かっこ ({ }) で囲み、スクリプト ブロックを作成します。

**Action** パラメーターの値には `$Event` 、、、、 `$EventSubscriber` `$Sender` 、および自動変数を含めることができます `$EventArgs` `$Args` 。これにより、イベントに関する情報が **action** スクリプトブロックに提供されます。 詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

アクションを指定すると、は `Register-EngineEvent` そのアクションを表すイベントジョブオブジェクトを返します。 Job コマンドレットを使用して、イベント ジョブを管理できます。

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

### -転送

コマンドレットが、このサブスクリプションのイベントをローカルコンピューター上のセッションに送信することを示します。
このパラメーターは、リモート コンピューターまたはリモート セッションのイベントに登録する場合に使用します。

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

### -MaxTriggerCount

イベントサブスクリプションに対してアクションが実行される最大回数を指定します。

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

イベントに関連付けられている追加のデータを指定します。 このパラメーターの値は、イベント オブジェクトの **MessageData** プロパティに表示されます。

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

サブスクライブするイベントのソース識別子を指定します。 ソース識別子は、現在のセッション内で一意である必要があります。 このパラメーターは必須です。

このパラメーターの値は、サブスクライバー オブジェクトおよびこのサブスクリプションに関連付けられたすべてのイベント オブジェクトの **SourceIdentifier** プロパティの値に表示されます。

値は、イベントのソースに固有です。 これは、コマンドレットで使用するために作成した任意の値にすることができ `New-Event` ます。 PowerShell エンジンは **register-engineevent** 値の powershell **をサポート** しています **。終了と終了**。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 100
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SupportEvent

コマンドレットによってイベントサブスクリプションが非表示になることを示します。 現在のサブスクリプションがより複雑なイベント登録メカニズムの一部であり、個別に検出されない場合は、このパラメーターを追加します。

**Supportevent** パラメーターを使用して作成されたサブスクリプションを表示またはキャンセルするには、 **Force** パラメーターを `Get-EventSubscriber` またはコマンドレットに追加し `Unregister-Event` ます。

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

入力をにパイプすることはできません `Register-EngineEvent` 。

## 出力

### None または PSEventJob。

**Action** パラメーターを使用すると、は `Register-EngineEvent` **PSEventJob** オブジェクトを返します。 それ以外の場合、出力は生成されません。

## 注

Linux または macOS プラットフォームで使用できるイベントソースがありません。

イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

## 関連リンク

[Get-Event](Get-Event.md)

[New-Event](New-Event.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)
