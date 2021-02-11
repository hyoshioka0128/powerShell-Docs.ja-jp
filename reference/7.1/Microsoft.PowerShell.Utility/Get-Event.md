---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-event?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Event
ms.openlocfilehash: 0ab2c32fe81f2e5ffa6c292ce0fd00e05685bd2a
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347705"
---
# Get-Event

## 概要
イベント キュー内のイベントを取得します。

## SYNTAX

### BySource (既定値)

```
Get-Event [[-SourceIdentifier] <String>] [<CommonParameters>]
```

### ById

```
Get-Event [-EventIdentifier] <Int32> [<CommonParameters>]
```

## Description

`Get-Event`コマンドレットは、現在のセッションの PowerShell イベントキューにあるイベントを取得します。 すべてのイベントを取得するか、 **EventIdentifier** パラメーターまたは **SourceIdentifier** パラメーターを使用してイベントを指定できます。

イベントは、発生時に、イベント キューに追加されます。 イベントキューには、登録したイベント、コマンドレットを使用して作成されたイベント、 `New-Event` および PowerShell の終了時に発生するイベントが含まれます。 またはを使用し `Get-Event` `Wait-Event` て、イベントを取得できます。

このコマンドレットは、イベント ビューアーのログからイベントを取得しません。 これらのイベントを取得するには、またはを使用し `Get-WinEvent` `Get-EventLog` ます。

## 例

### 例 1: すべてのイベントを取得する

```
PS C:\> Get-Event
```

このコマンドは、イベント キュー内のすべてのイベントを取得します。

### 例 2: ソース識別子でイベントを取得する

```
PS C:\> Get-Event -SourceIdentifier "PowerShell.ProcessCreated"
```

このコマンドは、SourceIdentifier プロパティの値が PowerShell. ProcessCreated であるイベントを取得します。

### 例 3: 生成された時刻に基づいてイベントを取得する

```
PS C:\> $Events = Get-Event
PS C:\> $Events[0] | Format-List -Property *
ComputerName     :
RunspaceId       : c2153740-256d-46c0-a57c-b805917d1b7b
EventIdentifier  : 1
Sender           : System.Management.ManagementEventWatcher
SourceEventArgs  : System.Management.EventArrivedEventArgs
SourceArgs       : {System.Management.ManagementEventWatcher, System.Management.EventArrivedEventArgs}
SourceIdentifier : ProcessStarted
TimeGenerated    : 11/13/2008 12:09:32 PM
MessageData      : PS C:\> Get-Event | Where {$_.TimeGenerated -ge "11/13/2008 12:15:00 PM"}
ComputerName     :
RunspaceId       : c2153740-256d-46c0-a57c-b8059325d1a0
EventIdentifier  : 1
Sender           : System.Management.ManagementEventWatcher
SourceEventArgs  : System.Management.EventArrivedEventArgs
SourceArgs       : {System.Management.ManagementEventWatcher, System.Management.EventArrivedEventArgs}
SourceIdentifier : ProcessStarted
TimeGenerated    : 11/13/2008 12:15:00 PM
MessageData      :
```

この例では SourceIdentifier 以外のプロパティを使用してイベントを取得する方法を示します。

最初のコマンドは、イベントキュー内のすべてのイベントを取得し、変数に保存し `$Events` ます。

2番目のコマンドは、配列表記を使用して、変数内の配列内の最初の (0 インデックス) イベントを取得し `$Events` ます。 このコマンドは、パイプライン演算子 () を使用して `|` コマンドにイベントを送信し `Format-List` ます。これにより、イベントのすべてのプロパティが一覧に表示されます。 これによって、イベント オブジェクトのプロパティを調べることができます。

3番目のコマンドは、コマンドレットを使用して、生成された時刻に基づいてイベントを取得する方法を示して `Where-Object` います。

### 例 4: 識別子を使用してイベントを取得する

```
PS C:\> Get-Event -EventIdentifier 2
```

このコマンドは、イベント識別子が 2 のイベントを取得します。

## PARAMETERS

### -EventIdentifier

このコマンドレットがイベントを取得するイベント識別子を指定します。

```yaml
Type: System.Int32
Parameter Sets: ById
Aliases: Id

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SourceIdentifier

このコマンドレットがイベントを取得するソース識別子を指定します。 既定では、イベント キュー内のすべてのイベントです。 ワイルドカードは使用できません。

```yaml
Type: System.String
Parameter Sets: BySource
Aliases:

Required: False
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

### PSEventArgs (システム管理)

`Get-Event` 各イベントの **PSEventArgs** オブジェクトを返します。 このオブジェクトの説明を表示するには、「」 `Get-Help Get-Event -Full` と入力し、ヘルプトピックの「メモ」セクションを参照してください。

## 注

Linux または macOS プラットフォームで使用できるイベントソースがありません。

イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

`Get-Event`コマンドレットは、次のプロパティを持つ **PSEventArgs** オブジェクト ( **PSEventArgs** ) を返します。

- ComputerName。 イベントが発生したコンピューターの名前。 このプロパティ値は、リモート コンピューターからイベントが転送された場合にのみ入力されます。

- RunspaceId. イベントが発生したセッションを一意に識別する GUID。 このプロパティ値は、リモート コンピューターからイベントが転送された場合にのみ入力されます。

- EventIdentifier. 現在のセッションにおけるイベント通知を一意に識別する整数 (Int32)。

- センダー. イベントを生成したオブジェクト。 **Action** パラメーターの値では、 `$Sender` 自動変数に sender オブジェクトが含まれています。

- SourceEventArgs. 存在する場合は、EventArgs から派生した最初のパラメーター。 たとえば、署名に "オブジェクト送信者, **ElapsedEventArgs** e" という形式の timer elapsed イベントがある場合、 **sourceeventargs** プロパティには **ElapsedEventArgs** が含まれます。 **Action** パラメーターの値では、 `$EventArgs` 自動変数にこの値が含まれています。

- SourceArgs。 元のイベント署名のすべてのパラメーター。 標準イベントシグネチャの場合、は `$Args[0]` 送信者を表し、 `$Args[1]` **sourceeventargs** を表します。 **Action** パラメーターの値では、 `$Args` 自動変数にこの値が含まれています。

- SourceIdentifier. イベント サブスクリプションを識別する文字列。 **Action** パラメーターの値では、自動変数の **SourceIdentifier** プロパティに `$Event` この値が含まれています。

- TimeGenerated. イベントが生成された時刻を表す **DateTime** オブジェクト。
  **Action** パラメーターの値では、自動変数の **timegenerated** プロパティに `$Event` この値が含まれています。

- MessageData. イベント サブスクリプションに関連付けられているデータ。 ユーザーは、イベントを登録するときに、このデータを指定します。 **Action** パラメーターの値では、自動変数の **messagedata** プロパティに `$Event` この値が含まれています。

## 関連リンク

[New-Event](New-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)
