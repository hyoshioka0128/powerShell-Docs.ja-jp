---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/remove-event?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-Event
ms.openlocfilehash: 3193de9020f2f54795bde9d5cce1bb86c4431ef9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601192"
---
# Remove-Event

## 概要
イベントをイベント キューから削除します。

## SYNTAX

### BySource (既定値)

```
Remove-Event [-SourceIdentifier] <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByIdentifier

```
Remove-Event [-EventIdentifier] <Int32> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Remove-Event`コマンドレットは、現在のセッションのイベントキューからイベントを削除します。

このコマンドレットは、現在キューにあるイベントのみを削除します。 イベントの登録またはサブスクライブ解除を取り消すには、コマンドレットを使用し `Unregister-Event` ます。

## 例

### 例 1: ソース識別子を使用してイベントを削除する

```
PS C:\> Remove-Event -SourceIdentifier "ProcessStarted"
```

このコマンドは、イベントキューからプロセスのソース識別子が開始されたイベントを削除します。

### 例 2: イベント識別子でイベントを削除する

```
PS C:\> Remove-Event -EventIdentifier 30
```

このコマンドは、イベント ID が 30 のイベントをイベント キューから削除します。

### 例 3: すべてのイベントを削除する

```
PS C:\> Get-Event | Remove-Event
```

このコマンドは、イベント キューからすべてのイベントを削除します。

## PARAMETERS

### -EventIdentifier

コマンドレットが削除するイベント識別子を指定します。 すべてのコマンドには、 **EventIdentifier** パラメーターまたは **SourceIdentifier** パラメーターが必要です。

```yaml
Type: System.Int32
Parameter Sets: ByIdentifier
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SourceIdentifier

このコマンドレットによってイベントが削除されるソース識別子を指定します。 ワイルドカードは使用できません。 すべてのコマンドには、 **EventIdentifier** パラメーターまたは **SourceIdentifier** パラメーターが必要です。

```yaml
Type: System.String
Parameter Sets: BySource
Aliases:

Required: True
Position: 0
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

### PSEventArgs (システム管理)

パイプを使用して、イベントをからにパイプすることができ `Get-Event` `Remove-Event` ます。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

Linux または macOS プラットフォームで使用できるイベントソースがありません。

イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

## 関連リンク

[Get-Event](Get-Event.md)

[New-Event](New-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)
