---
external help file: PSDiagnostics-help.xml
Locale: en-US
Module Name: PSDiagnostics
ms.date: 11/27/2018
online version: https://docs.microsoft.com/powershell/module/psdiagnostics/enable-pstrace?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enable-PSTrace
ms.openlocfilehash: 2ec01393a46de84b59f06995473bdff6bd5b2b4f
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103195026"
---
# Enable-PSTrace

## 概要
Microsoft Windows PowerShell イベントプロバイダーのログを有効にします。

## SYNTAX

```
Enable-PSTrace [-Force] [-AnalyticOnly]
```

## Description

このコマンドレットは、Microsoft Windows PowerShell イベントプロバイダーの操作および分析イベントログを有効にします。

このコマンドレットは、管理者特権の PowerShell セッションから実行する必要があります。

## 例

### 例 1: PowerShell の分析イベントログを有効にする

次の例では、Microsoft Windows PowerShell プロバイダーの分析イベントログのみを有効にします。

```powershell
Enable-PSTrace -AnalyticOnly
```

## PARAMETERS

### -分析のみ

このパラメーターを使用すると、コマンドレットによって、Microsoft Windows PowerShell プロバイダーの分析イベントログが有効になります。 操作イベントログは変更されません。

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

[Disable-PSTrace](Disable-PSTrace.md)
