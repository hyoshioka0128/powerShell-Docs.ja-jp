---
external help file: PSDiagnostics-help.xml
Module Name: PSDiagnostics
ms.date: 11/27/2018
online version: https://docs.microsoft.com/powershell/module/psdiagnostics/get-logproperties?view=powershell-6&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-LogProperties
ms.openlocfilehash: 63f9f644a81886dab498e46fce90d4ff2f7be91b
ms.sourcegitcommit: 9b28fb9a3d72655bb63f62af18b3a5af6a05cd3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/07/2020
ms.locfileid: "93217075"
---
# Get-LogProperties

## 概要
Windows イベントログのプロパティを取得します。

## SYNTAX

```
Get-LogProperties [-Name] <Object> [<CommonParameters>]
```

## Description

このコマンドレットは、Windows イベントログの構成設定を取得します。 このコマンドレットは、コマンドレットとコマンドレットによって使用され `Enable-PSTrace` `Disable-PSTrace` ます。

## 例

### 例 1: Windows PowerShell イベントログの構成設定を取得する

```powershell
Get-LogProperties 'Windows PowerShell'
```

```Output
Name       : Windows PowerShell
Enabled    : True
Type       : Admin
Retention  : False
AutoBackup : False
MaxLogSize : 15728640
```

## PARAMETERS

### -Name

イベントプロバイダーの名前。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

## 出力

### Microsoft. 診断. LogDetails

**Psdiagnostics** モジュールは、 **logdetails** クラスを名前空間に追加し `Microsoft.PowerShell.Diagnostics` ます。

## 注

## 関連リンク

[Set-LogProperties](Set-LogProperties.md)

[Enable-PSTrace](Enable-PSTrace.md)

[Disable-PSTrace](Disable-PSTrace.md)
