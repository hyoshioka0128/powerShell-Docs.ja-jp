---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/wait-event?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Wait-Event
ms.openlocfilehash: caf2a1c80848d3140e7ad46fdf54dbe71886c633
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601191"
---
# Wait-Event

## 概要
特定のイベントが発生するまで待機してから、実行を継続します。

## SYNTAX

```
Wait-Event [[-SourceIdentifier] <String>] [-Timeout <Int32>] [<CommonParameters>]
```

## Description

`Wait-Event`コマンドレットは、特定のイベントが発生するまで、スクリプトまたは関数の実行を中断します。 イベントが検出されると、実行が再開されます。 待機を取り消すには、 <kbd>ctrl</kbd> + <kbd>C</kbd>キーを押します。

この機能によって、イベントのポーリングに代わる別の方法が提供されます。 また、2つの異なる方法でイベントへの応答を決定することもできます。

- イベントサブスクリプションの **Action** パラメーターを使用する
- イベントが返され、アクションで応答するまで待機しています

## 例

### 例 1: 次のイベントを待機する

この例では、次のイベントが発生するまで待機します。

```powershell
Wait-Event
```

### 例 2: 指定したソース識別子を持つイベントを待機する

この例では、次のイベントが発生し、ソース識別子が ProcessStarted であることを待機します。

```powershell
Wait-Event -SourceIdentifier "ProcessStarted"
```

### 例 3: timer elapsed イベントを待機する

この例では、コマンドレットを使用して、 `Wait-Event` 2000 ミリ秒に設定されているタイマーのタイマーイベントを待機します。

```powershell
$Timer = New-Object Timers.Timer
$objectEventArgs = @{
    InputObject = $Timer
    EventName = 'Elapsed'
    SourceIdentifier = 'Timer.Elapsed'
}
Register-ObjectEvent @objectEventArgs
$Timer.Interval = 2000
$Timer.Autoreset = $False
$Timer.Enabled = $True
Wait-Event Timer.Elapsed
```

```Output
ComputerName     :
RunspaceId       : bb560b14-ff43-48d4-b801-5adc31bbc6fb
EventIdentifier  : 1
Sender           : System.Timers.Timer
SourceEventArgs  : System.Timers.ElapsedEventArgs
SourceArgs       : {System.Timers.Timer, System.Timers.ElapsedEventArgs}
SourceIdentifier : Timer.Elapsed
TimeGenerated    : 4/23/2020 2:30:37 PM
MessageData      :
```

### 例 4: 指定したタイムアウト後にイベントを待機する

この例では、次のイベントが発生し、ソース識別子が **Processstarted** であることを、最大90秒間待機します。 指定された時間が経過すると、待機は終了します。

```powershell
Wait-Event -SourceIdentifier "ProcessStarted" -Timeout 90
```

## PARAMETERS

### -SourceIdentifier

このコマンドレットがイベントを待機するソース識別子を指定します。
既定では、は、 `Wait-Event` 任意のイベントを待機します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -タイムアウト

イベントが発生するまで待機する最大時間を秒単位で指定し `Wait-Event` ます。 既定値は、無期限に待機することを示す -1 です。 コマンドを送信すると、タイミングが開始され `Wait-Event` ます。

指定された時間を超えた場合、イベントが発生していない場合でも、待機が終了し、コマンド プロンプトが返されます。 エラー メッセージは表示されません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: TimeoutSec

Required: False
Position: Named
Default value: -1
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

## 出力

### PSEventArgs (システム管理)

## 注

イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

## 関連リンク

[Get-Event](Get-Event.md)

[Get-EventSubscriber](Get-EventSubscriber.md)

[New-Event](New-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)

