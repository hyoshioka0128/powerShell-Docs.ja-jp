---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/27/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-uptime?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Uptime
ms.openlocfilehash: 0b71d59175ed56e7aad6a24035d1eff5503e4d88
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604931"
---
# Get-Uptime

## 概要
前回の起動時からの **TimeSpan** を取得します。

## SYNTAX

### Timespan (既定値)

```
Get-Uptime [<CommonParameters>]
```

### カウンター

```
Get-Uptime [-Since] [<CommonParameters>]
```

## Description

このコマンドレットは、オペレーティングシステムを最後に起動してから経過した時間を返します。

`Get-Uptime`コマンドレットは、PowerShell 6.0 で導入されました。

## 例

### 例 1-前回の起動時からの時間を表示する

```powershell
Get-Uptime
```

```Output
Days              : 9
Hours             : 0
Minutes           : 9
Seconds           : 45
Milliseconds      : 0
Ticks             : 7781850000000
TotalDays         : 9.00677083333333
TotalHours        : 216.1625
TotalMinutes      : 12969.75
TotalSeconds      : 778185
TotalMilliseconds : 778185000
```

### 例 2-最後に起動した時刻を表示する

```powershell
Get-Uptime -Since
```

```Output
Tuesday, June 18, 2019 2:34:56 PM
```

## PARAMETERS

### -以降

コマンドレットが、オペレーティングシステムが最後に起動された時刻を表す **DateTime** オブジェクトを返すようにします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Since
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

### なし

## 出力

### System.TimeSpan

これは、パラメーターが使用されていない場合の既定の戻り値の型です。

### System.DateTime

この型は、 **以降** のパラメーターを使用したときに返されます。

> [!NOTE]
> Windows の高速起動が有効になっている場合、 **Lastbootuptime タイム** に格納されている値は更新されません。 高速スタートアップを無効にするには、コマンドを実行し `Powercfg -h off` ます。
>
> Windows fast スタートアップの詳細については、「 [ウェイクアップからの高速スタートアップの区別](/windows-hardware/drivers/kernel/distinguishing-fast-startup-from-wake-from-hibernation)」を参照してください。

## 注

Windows では、返される値は WMI の **Win32_OperatingSystem** クラスの **lastbootuptime** プロパティと同じです。

## 関連リンク

[Win32_OperatingSystem](/windows/win32/cimwin32prov/win32-operatingsystem#properties)

