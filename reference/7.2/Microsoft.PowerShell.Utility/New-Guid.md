---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-guid?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Guid
ms.openlocfilehash: df7f9000cf66cebce83e3146cd5c95a7d1a78bf8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605190"
---
# New-Guid

## 概要
GUID を作成します。

## SYNTAX

```
New-Guid [<CommonParameters>]
```

## Description

**新しい guid** コマンドレットは、ランダムなグローバル一意識別子 (guid) を作成します。
スクリプトで一意の ID が必要な場合は、必要に応じて GUID を作成できます。

## 例

### 例 1: GUID を作成する

```
PS C:\> New-Guid
Guid
----
0352cf0f-2e7a-4aee-801d-7f27f8344c77
```

このコマンドは、ランダムな GUID を作成します。
または、このコマンドレットの出力を変数に格納して、スクリプト内の他の場所で使用することもできます。

## PARAMETERS

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

## 出力

### System.Guid

このコマンドレットは、GUID を返します。

## 注

## 関連リンク

