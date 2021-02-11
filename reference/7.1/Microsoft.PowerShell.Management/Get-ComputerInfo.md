---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-computerinfo?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-ComputerInfo
ms.openlocfilehash: 4a39714995c31a4d44177312dd8e5dad9ab43712
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94342333"
---
# Get-ComputerInfo

## 概要
システムおよびオペレーティングシステムのプロパティの統合オブジェクトを取得します。

## SYNTAX

```
Get-ComputerInfo [[-Property] <String[]>] [<CommonParameters>]
```

## Description

`Get-ComputerInfo`コマンドレットは、システムとオペレーティングシステムのプロパティの統合オブジェクトを取得します。
このコマンドレットは、Windows PowerShell 5.1 で導入されました。

## 例

### 例 1: すべてのコンピューターのプロパティを取得する

```powershell
Get-ComputerInfo
```

このコマンドは、すべてのシステムおよびオペレーティングシステムのプロパティをコンピューターから取得します。

### 例 2: すべてのコンピューターのオペレーティングシステムのプロパティを取得する

```powershell
Get-ComputerInfo -Property "os*"
```

このコマンドは、すべてのオペレーティングシステムのプロパティをコンピューターから取得します。

## PARAMETERS

### -プロパティ

このコマンドレットによって表示されるコンピュータープロパティを文字列配列として指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String[]

## 出力

### Microsoft. PowerShell. 管理. ComputerInfo

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク
