---
external help file: PSDiagnostics-help.xml
Module Name: PSDiagnostics
ms.date: 11/27/2018
online version: https://docs.microsoft.com/powershell/module/psdiagnostics/set-logproperties?view=powershell-6&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-LogProperties
ms.openlocfilehash: d6ad6f8cc299351cc85d4ed7e7b9682a1d90307b
ms.sourcegitcommit: 9b28fb9a3d72655bb63f62af18b3a5af6a05cd3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/07/2020
ms.locfileid: "93217080"
---
# Set-LogProperties

## 概要
Windows イベントログのプロパティを変更します。

## SYNTAX

```
Set-LogProperties [-LogDetails] <LogDetails> [-Force] [<CommonParameters>]
```

## Description

このコマンドレットは、Windows イベントログの構成設定を変更します。 このコマンドレットは、コマンドレットとコマンドレットによって使用され `Enable-PSTrace` `Disable-PSTrace` ます。

このコマンドレットは、管理者特権の PowerShell セッションから実行する必要があります。

## 例

### 例 1: Windows PowerShell イベントログの保有期間の設定を変更する

```powershell
$logDetails = Get-LogProperties 'Windows PowerShell'
$logDetails.Retention = $True
Set-LogProperties -LogDetails $logDetails
Get-LogProperties 'Windows PowerShell'
```

```Output
Name       : Windows PowerShell
Enabled    : True
Type       : Admin
Retention  : True
AutoBackup : False
MaxLogSize : 15728640
```

## PARAMETERS

### -Force

プロンプトを表示せずに変更を強制するために使用します。

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

### -LogDetails

イベントログに割り当てられる更新された構成設定。

```yaml
Type: Microsoft.PowerShell.Diagnostics.LogDetails
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

### Microsoft. 診断. LogDetails

完全に構成された **Logdetails** オブジェクトをコマンドレットに渡す必要があり `Set-LogProperties` ます。
したがって、1つの設定を変更するには、を使用して現在の構成を取得する必要があり `Get-LogProperties` ます。

## 出力

### なし

## 注

このコマンドレットは、管理者特権の PowerShell セッションから実行する必要があります。

## 関連リンク

[Get-LogProperties](Get-LogProperties.md)

[Enable-PSTrace](Enable-PSTrace.md)

[Disable-PSTrace](Disable-PSTrace.md)
