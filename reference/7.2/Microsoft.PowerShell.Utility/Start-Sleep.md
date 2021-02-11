---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/10/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/start-sleep?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Start-Sleep
ms.openlocfilehash: e4d06fdd1d5a616c24a4dd7bec10ea4dadea4062
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601011"
---
# Start-Sleep

## 概要
指定された期間、スクリプトまたはセッションでアクティビティを中断します。

## SYNTAX

### 秒 (既定値)

```
Start-Sleep [-Seconds] <Double> [<CommonParameters>]
```

### ミリ秒

```
Start-Sleep -Milliseconds <Int32> [<CommonParameters>]
```

## Description

この `Start-Sleep` コマンドレットは、指定された期間、スクリプトまたはセッションでアクティビティを中断します。 操作が完了するのを待つ間、操作を繰り返す前の一時停止中など多くのタスクに使用できます。

## 例

### 例 1: すべてのコマンドを15秒間スリープする

```powershell
Start-Sleep -s 15
```

### 例 2: すべてのコマンドを1.5 秒間スリープする

この例では、セッション内のすべてのコマンドを1秒間、または1秒間スリープ状態にします。

```powershell
Start-Sleep -Seconds 1.5
```

## PARAMETERS

### -ミリ秒

リソースをスリープ状態にする時間をミリ秒単位で指定します。 パラメーターは **m** として省略できます。

```yaml
Type: System.Int32
Parameter Sets: Milliseconds
Aliases: ms

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -秒

リソースをスリープ状態にする時間を秒単位で指定します。 パラメーター名を省略することも、 **s** として省略することもできます。 PowerShell 6.2.0 以降では、このパラメーターで小数値を受け取るようになりました。

```yaml
Type: System.Double
Parameter Sets: Seconds
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.Int32

パイプを使用して、秒数をにパイプすることができ `Start-Sleep` ます。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## 注

- また、組み込みエイリアスであるを参照することもでき `Start-Sleep` `sleep` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。
- `Ctrl+C` から中断 `Start-Sleep` します。
  - `Ctrl+C` はから除外されません `[Threading.Thread]::Sleep` 。 詳細については、「 [Thread メソッド](/dotnet/api/system.threading.thread.sleep)」を参照してください。

## 関連リンク

