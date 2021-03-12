---
external help file: PSDiagnostics-help.xml
Locale: en-US
Module Name: PSDiagnostics
ms.date: 11/27/2018
online version: https://docs.microsoft.com/powershell/module/psdiagnostics/disable-pstrace?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disable-PSTrace
ms.openlocfilehash: 50d4118c5805a59d291d8dd17f467e7b47d34b46
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103194512"
---
# Disable-PSTrace

## 概要
Microsoft Windows PowerShell イベントプロバイダーのログを無効にします。

## SYNTAX

```
Disable-PSTrace [-AnalyticOnly] [<CommonParameters>]
```

## Description

このコマンドレットは、Microsoft Windows PowerShell イベントプロバイダーの操作および分析イベントログを無効にします。

このコマンドレットは、管理者特権の PowerShell セッションから実行する必要があります。

## 例

### 例 1: PowerShell の分析イベントログを無効にする

次の例では、Microsoft Windows PowerShell プロバイダーの分析イベントログのみを無効にします。

```powershell
Disable-PSTrace -AnalyticOnly
```

## PARAMETERS

### -分析のみ

このパラメーターを使用すると、コマンドレットは、Microsoft Windows PowerShell プロバイダーの分析イベントログを無効にします。 操作イベントログは変更されません。

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

## 入力

### なし

## 出力

### System.Object

## 注

このコマンドレットは `Get-LogProperties` 、 `Set-LogProperties` コマンドレットとコマンドレットを使用します。

このコマンドレットは、管理者特権の PowerShell セッションから実行する必要があります。

## 関連リンク

[Get-LogProperties](Get-LogProperties.md)

[Set-LogProperties](Set-LogProperties.md)

[Enable-PSTrace](Enable-PSTrace.md)
