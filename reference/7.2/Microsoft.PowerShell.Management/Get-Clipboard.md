---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 09/21/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-clipboard?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Clipboard
ms.openlocfilehash: b0a9e5d1707e0d2f46c25991ddceff27d1b85287
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603237"
---
# Get-Clipboard

## 概要
クリップボードの内容を取得します。

## SYNTAX

```
Get-Clipboard [-Raw] [<CommonParameters>]
```

## Description

`Get-Clipboard`コマンドレットは、クリップボードの内容をテキストとして取得します。 に類似した文字列の配列として、複数行のテキストが返され `Get-Content` ます。

> [!NOTE]
> Linux では、このコマンドレットでは `xclip` ユーティリティをパスに配置する必要があります。 このコマンドレットは macOS ではサポートされていません。

## 例

### 例 1: クリップボードの内容を取得し、それをコマンドラインに表示する

この例では、テキスト "hello" をクリップボードにコピーしました。

```powershell
Get-Clipboard
```

```Output
hello
```

## PARAMETERS

### -Raw

クリップボードの内容全体を取得します。 複数行テキストは、文字列の配列ではなく、単一の複数行文字列として返されます。

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

## 出力

### System.String

## 注

## 関連リンク

[Set-Clipboard](Set-Clipboard.md)
