---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-timezone?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-TimeZone
ms.openlocfilehash: 22034eeac6988478a5f2ff87b2582cc5e1acc1c0
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600099"
---
# Get-TimeZone

## 概要
現在のタイムゾーンまたは使用可能なタイムゾーンの一覧を取得します。

## SYNTAX

### 名前 (既定値)

```
Get-TimeZone [[-Name] <String[]>] [<CommonParameters>]
```

### Id

```
Get-TimeZone -Id <String[]> [<CommonParameters>]
```

### ListAvailable

```
Get-TimeZone [-ListAvailable] [<CommonParameters>]
```

## Description

**Get TimeZone** コマンドレットは、現在のタイムゾーンまたは使用可能なタイムゾーンの一覧を取得します。

## 例

### 例 1: 現在のタイムゾーンを取得する

```
PS C:\> Get-TimeZone
Pacific Standard Time
```

このコマンドは、現在のタイムゾーンを取得します。

### 例 2: 指定した文字列に一致するタイムゾーンを取得する

```
PS C:\> Get-TimeZone -Name "*pac*"
Pacific Standard Time (Mexico)

(UTC-08:00) Pacific Time (US &amp; Canada)

Pacific Standard Time

SA Pacific Standard Time

Pacific SA Standard Time

West Pacific Standard Time

Central Pacific Standard Time
```

このコマンドは、指定されたワイルドカードと一致するすべてのタイムゾーンを取得します。

### 例 3: 使用可能なすべてのタイムゾーンを取得する

```
PS C:\> Get-TimeZone -ListAvailable
```

このコマンドは、使用可能なすべてのタイムゾーンを取得します。

## PARAMETERS

### -Id

文字列配列として、このコマンドレットが取得するタイムゾーンの ID または id を指定します。

```yaml
Type: System.String[]
Parameter Sets: Id
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ListAvailable

このコマンドレットがすべての使用可能なタイムゾーンを取得することを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ListAvailable
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

文字列配列として、このコマンドレットが取得するタイムゾーンの名前または名前を指定します。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String[]

## 出力

### TimeZoneInfo []

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク

[Set-TimeZone](Set-TimeZone.md)
