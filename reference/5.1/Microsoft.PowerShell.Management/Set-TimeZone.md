---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 09/18/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-timezone?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-TimeZone
ms.openlocfilehash: 3acf02b567c1fecd8089b12fac7c32b42abfec77
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94343183"
---
# Set-TimeZone

## 概要
システムのタイムゾーンを指定したタイムゾーンに設定します。

## SYNTAX

### 名前 (既定値)

```
Set-TimeZone [-Name] <String> [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Id

```
Set-TimeZone -Id <String> [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObject

```
Set-TimeZone [-InputObject] <TimeZoneInfo> [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Set-TimeZone` システムのタイムゾーンを指定されたタイムゾーンに設定します。

## 例

### 例 1: タイムゾーンを Id で設定する

この例では、ローカルコンピューターのタイムゾーンをロシア標準時に設定します。

```powershell
Set-TimeZone -Id "Russian Standard Time" -PassThru
```

```Output
Id                         : Russian Standard Time
DisplayName                : (UTC+03:00) Moscow, St. Petersburg
StandardName               : Russia TZ 2 Standard Time
DaylightName               : Russia TZ 2 Daylight Time
BaseUtcOffset              : 03:00:00
SupportsDaylightSavingTime : True
```

### 例 2: タイムゾーンを名前で設定する

この例では、ローカルコンピューターのタイムゾーンをロシア標準時に設定します。

```powershell
Set-TimeZone -Name "Russia TZ 2 Standard Time"
```

前の例に示したように、タイムゾーンの **Id** と **名前** は必ずしも一致するとは限りません。
**Name** パラメーターは、 **TimeZoneInfo** オブジェクトの **standardname** または **daylightname** プロパティと一致する必要があります。

## PARAMETERS

### -Id

このコマンドレットが設定するタイムゾーンの ID を指定します。 次のコマンドを実行して、タイムゾーン Id の完全な一覧を取得でき `Get-TimeZone -ListAvailable` ます。

```yaml
Type: System.String
Parameter Sets: Id
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InputObject

入力として使用する **TimeZoneInfo** オブジェクトを指定します。

```yaml
Type: System.TimeZoneInfo
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

このコマンドレットによって設定されるタイムゾーンの名前を指定します。 タイムゾーン名の完全な一覧を取得するには、コマンドを実行し `Get-TimeZone -ListAvailable` ます。

```yaml
Type: System.String
Parameter Sets: Name
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### System.string, TimeZoneInfo, System.string, System.string

## 出力

## 注

## 関連リンク

[Get-TimeZone](Get-TimeZone.md)
