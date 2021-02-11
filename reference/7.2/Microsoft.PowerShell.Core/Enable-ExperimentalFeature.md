---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/06/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-experimentalfeature?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enable-ExperimentalFeature
ms.openlocfilehash: 1d022bd17c19d1c332b7ef49522ba29c5b7b8e6f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603038"
---
# Enable-ExperimentalFeature

## 概要
PowerShell の新しいインスタンスの起動時に実験的な機能を有効にします。

## SYNTAX

```
Enable-ExperimentalFeature [-Name] <String[]> [-Scope <ConfigScope>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Enable-ExperimentalFeature`コマンドレットは、名前付き実験機能を `powershell.config.json` PowerShell の起動時に読み取られる設定ファイルに追加することにより、試験的な機能を有効にします。

このコマンドレットは、PowerShell 6.2 で導入されました。

> [!NOTE]
> 実験的な機能の状態に対する変更は、PowerShell の再起動時にのみ有効になります

## 例

### 例 1: 試験的な機能を有効にする

この例では、この実験的な機能が既に無効になっている場合、 `powershell.config.json` PowerShell を再起動した後に、ユーザーがこの機能を有効にするためにファイルが更新されます。
成功すると、何もパイプラインに出力されず、警告メッセージのみが表示されます。

```powershell
Enable-ExperimentalFeature PSImplicitRemotingBatching
```

```Output
WARNING: Enabling and disabling experimental features do not take effect until next start of PowerShell.
```

## PARAMETERS

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

有効にする試験的な機能の名前。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -スコープ

すべての `powershell.config.json` ユーザーに影響するか、現在のユーザーのみに影響するかを更新するかどうかを決定します。

```yaml
Type: System.Management.Automation.Configuration.ConfigScope
Parameter Sets: (All)
Aliases:
Accepted values: AllUsers, CurrentUser

Required: False
Position: Named
Default value: CurrentUser
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
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### ExperimentalFeature

ExperimentalFeature のパイプインスタンスを `Get-ExperimentalFeature` コマンドレットから有効にします。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## 注

試験的な機能の状態に対する変更は、PowerShell の再起動時にのみ有効になります。

## 関連リンク

[Disable-ExperimentalFeature](Disable-ExperimentalFeature.md)

[Get-ExperimentalFeature](Get-ExperimentalFeature.md)

