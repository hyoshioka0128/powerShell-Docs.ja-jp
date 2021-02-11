---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 05/01/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-timespan?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-TimeSpan
ms.openlocfilehash: 5808685a5560d1cbf91c6705b90643c35702b28a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603232"
---
# New-TimeSpan

## 概要
TimeSpan オブジェクトを作成します。

## SYNTAX

### 日付 (既定値)

```
New-TimeSpan [[-Start] <DateTime>] [[-End] <DateTime>] [<CommonParameters>]
```

### 時刻

```
New-TimeSpan [-Days <Int32>] [-Hours <Int32>] [-Minutes <Int32>] [-Seconds <Int32>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-TimeSpan` 時間間隔を表す **TimeSpan** オブジェクトを作成します。
**TimeSpan** オブジェクトを使用して、 **DateTime** オブジェクトの時間を加算または減算できます。

パラメーターを指定しない場合、コマンドは、 `New-TimeSpan` 時間間隔0を表す **TimeSpan** オブジェクトを返します。

## 例

### 例 1: 指定された期間の TimeSpan オブジェクトを作成する

このコマンドは、期間が1時間と25分の **TimeSpan** オブジェクトを作成し、という名前の変数に格納し `$TimeSpan` ます。 これにより、 **TimeSpan** オブジェクトの表現が表示されます。

```powershell
$TimeSpan = New-TimeSpan -Hours 1 -Minutes 25
$TimeSpan
```

```Output
Days              : 0
Hours             : 1
Minutes           : 25
Seconds           : 0
Milliseconds      : 0
Ticks             : 51000000000
TotalDays         : 0.0590277777777778
TotalHours        : 1.41666666666667
TotalMinutes      : 85
TotalSeconds      : 5100
TotalMilliseconds : 5100000
```

### 例 2: 時間間隔の TimeSpan オブジェクトを作成する

この例では、コマンドが実行されてから2010年1月1日までの間隔を表す新しい **TimeSpan** オブジェクトを作成します。

**Start** パラメーターの既定値は現在の日付と時刻であるため、このコマンドには **start** パラメーターは必要ありません。

```powershell
New-TimeSpan -End (Get-Date -Year 2010 -Month 1 -Day 1)
```

### 例 3: 現在の日付から90日目を取得する

```powershell
$90days = New-TimeSpan -Days 90
(Get-Date) + $90days
```

これらのコマンドは、現在の日付から 90 日後の日付を返します。

### 例 4: ファイルが更新されてからの TimeSpan を検出する

このコマンドは、 [about_remote](../Microsoft.PowerShell.Core/About/about_Remote.md) ヘルプファイルが最後に更新されてから経過した時間を示します。
このコマンド形式は、任意のファイル、または **Lastwritetime** プロパティを持つその他のオブジェクトで使用できます。

の **Start** パラメーター `New-TimeSpan` には **lastwritetime** のエイリアスがあるため、このコマンドは機能します。 **Lastwritetime** プロパティを持つオブジェクトをパイプすると `New-TimeSpan` 、PowerShell では、 **lastwritetime** プロパティの値が **Start** パラメーターの値として使用されます。

```powershell
Get-ChildItem $PSHOME\en-us\about_remote.help.txt | New-TimeSpan
```

```Output
Days              : 321
Hours             : 21
Minutes           : 59
Seconds           : 22
Milliseconds      : 312
Ticks             : 278135623127728
TotalDays         : 321.916230471907
TotalHours        : 7725.98953132578
TotalMinutes      : 463559.371879547
TotalSeconds      : 27813562.3127728
TotalMilliseconds : 27813562312.7728
```

## PARAMETERS

### -Days

期間の日数を指定します。
既定値は 0 です。

```yaml
Type: System.Int32
Parameter Sets: Time
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -End

期間の終了を指定します。
既定値は、現在の日付と時刻です。

```yaml
Type: System.DateTime
Parameter Sets: Date
Aliases:

Required: False
Position: 1
Default value: Current date and time
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -時間

期間の時間を指定します。
既定値はゼロです。

```yaml
Type: System.Int32
Parameter Sets: Time
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -分

時間間隔を分単位で指定します。
既定値は 0 です。

```yaml
Type: System.Int32
Parameter Sets: Time
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -秒

時間間隔の長さを秒単位で指定します。
既定値は 0 です。

```yaml
Type: System.Int32
Parameter Sets: Time
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Start

期間の開始を指定します。
"3/15/09" などの日付と時刻を表す文字列、または、コマンドからの1つなどの **DateTime** オブジェクトを入力し `Get-Date` ます。 既定値は、現在の日付と時刻です。

**Start** またはそのエイリアスである **lastwritetime** を使用できます。
**Lastwritetime** エイリアスを使用すると、ファイルシステム内のファイルなど、 **lastwritetime** プロパティを持つオブジェクトを `[System.Io.FileIO]` の **Start** パラメーターにパイプすることができ `New-TimeSpan` ます。

```yaml
Type: System.DateTime
Parameter Sets: Date
Aliases: LastWriteTime

Required: False
Position: 0
Default value: Current date and time
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.DateTime

パイプを使用して、その開始時刻を表す **DateTime** オブジェクトをにパイプすることができ `New-TimeSpan` ます。

## 出力

### System.TimeSpan

`New-TimeSpan` 時間間隔を表すオブジェクトを返します。

## 注

## 関連リンク

[取得-日付](Get-Date.md)

[Set-Date](Set-Date.md)

