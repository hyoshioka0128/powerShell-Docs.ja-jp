---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/unregister-event?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Unregister-Event
ms.openlocfilehash: 863dbba4c106f1f57c9396823692620bb358b646
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600803"
---
# Unregister-Event

## 概要
イベントのサブスクリプションを取り消します。

## SYNTAX

### BySource (既定値)

```
Unregister-Event [-SourceIdentifier] <String> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ById

```
Unregister-Event [-SubscriptionId] <Int32> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Unregister-Event`コマンドレットでは `Register-EngineEvent` 、、 `Register-ObjectEvent` 、またはコマンドレットを使用して作成されたイベントサブスクリプションをキャンセルし `Register-WmiEvent` ます。

イベント サブスクリプションが取り消されると、セッションからイベント サブスクライバーが削除され、サブスクライブされたイベントはイベント キューに追加されません。 コマンドレットを使用して作成されたイベントへのサブスクリプションをキャンセルすると、 `New-Event` 新しいイベントもセッションから削除されます。

`Unregister-Event` イベントキューからイベントを削除しません。 イベントを削除するには、コマンドレットを使用し `Remove-Event` ます。

## 例

### 例 1: ソース id によってイベントサブスクリプションを取り消す

```
PS C:\> Unregister-Event -SourceIdentifier "ProcessStarted"
```

このコマンドは、ソース識別子が ProcessStarted ているイベントサブスクリプションをキャンセルします。

イベントのソース識別子を検索するには、コマンドレットを使用し `Get-Event` ます。 イベントサブスクリプションのソース識別子を検索するには、コマンドレットを使用し `Get-EventSubscriber` ます。

### 例 2: サブスクリプション id によってイベントサブスクリプションを取り消す

```
PS C:\> Unregister-Event -SubscriptionId 2
```

このコマンドは、サブスクリプション識別子が 2 のイベント サブスクリプションを取り消します。

イベントサブスクリプションのサブスクリプション識別子を検索するには、コマンドレットを使用し `Get-EventSubscriber` ます。

### 例 3: すべてのイベントサブスクリプションを取り消す

```
PS C:\> Get-EventSubscriber -Force | Unregister-Event -Force
```

このコマンドは、セッション内のすべてのイベント サブスクリプションを取り消します。

このコマンドは、コマンドレットを使用して、 `Get-EventSubscriber` セッション内のすべてのイベントサブスクライバーオブジェクトを取得します。これには、イベント登録コマンドレットの **supportevent** パラメーターを使用して非表示になっているサブスクライバーも含まれます。

パイプライン演算子 () を使用して `|` 、サブスクライバーオブジェクトをに送信します。これにより `Unregister-Event` 、セッションからサブスクライバーオブジェクトが削除されます。 タスクを完了するには、で **Force** パラメーターも必要です `Unregister-Event` 。

## PARAMETERS

### -Force

、、およびの **Supportevent** パラメーターを使用して非表示にされたサブスクリプションも含め、すべてのイベントサブスクリプションをキャンセルし `Register-ObjectEvent` `Register-WmiEvent` `Register-EngineEvent` ます。

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

### -SourceIdentifier

このコマンドレットでイベントサブスクリプションをキャンセルするソース識別子を指定します。

各コマンドには、 **SourceIdentifier** パラメーターまたは **SubscriptionId** パラメーターを含める必要があります。

```yaml
Type: System.String
Parameter Sets: BySource
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SubscriptionId

このコマンドレットによってイベントサブスクリプションが取り消されるソース識別子 ID を指定します。

各コマンドには、 **SourceIdentifier** パラメーターまたは **SubscriptionId** パラメーターを含める必要があります。

```yaml
Type: System.Int32
Parameter Sets: ById
Aliases:

Required: True
Position: 0
Default value: None
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

### PSEventSubscriber (システム管理)

パイプを使用して、出力をからにパイプすることができ `Get-EventSubscriber` `Unregister-Event` ます。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## 注

Linux または macOS プラットフォームで使用できるイベントソースがありません。

イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

`Unregister-Event` コマンドレットを使用して `New-Event` イベントをサブスクライブしていない限り、コマンドレットを使用して作成されたイベントを削除することはできません `Register-EngineEvent` 。 セッションからカスタム イベントを削除するには、プログラムを使用してそれを削除するか、セッションを閉じる必要があります。

## 関連リンク

[Get-Event](Get-Event.md)

[Get-EventSubscriber](Get-EventSubscriber.md)

[New-Event](New-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)
