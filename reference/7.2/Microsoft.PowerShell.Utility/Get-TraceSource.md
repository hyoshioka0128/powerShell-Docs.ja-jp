---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-tracesource?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-TraceSource
ms.openlocfilehash: c196d00359c9b20b5dc1fefc0ab2b94655703f6f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602781"
---
# Get-TraceSource

## 概要
トレース用にインストルメント化された PowerShell コンポーネントを取得します。

## SYNTAX

```
Get-TraceSource [[-Name] <String[]>] [<CommonParameters>]
```

## Description

**Get TraceSource** コマンドレットは、現在使用されている PowerShell コンポーネントのトレースソースを取得します。
データを使用して、トレースできる PowerShell コンポーネントを決定できます。
トレースを行うと、コンポーネントは、内部処理の各手順について詳細なメッセージを生成します。
開発者は、トレース データを使用して、データ フロー、プログラム実行、およびエラーを監視できます。

トレースコマンドレットは、PowerShell 開発者向けに設計されていますが、すべてのユーザーが使用できます。

## 例

### 例 1: 名前でトレースソースを取得する

```
PS C:\> Get-TraceSource -Name "*provider*"
```

このコマンドは、プロバイダーを含む名前を持つすべてのトレースソースを取得します。

### 例 2: すべてのトレースソースを取得する

```
PS C:\> Get-TraceSource
```

このコマンドは、トレース可能なすべての PowerShell コンポーネントを取得します。

## PARAMETERS

### -Name

取得するトレースソースを指定します。
ワイルドカードを使用できます。
パラメーター *名は省略可能です。*

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用してトレースソースの名前を含む文字列を **取得** することができます。

## 出力

### System.management.automation.pstracesource (システム管理)

**Get TraceSource** は、トレースソースを表すオブジェクトを返します。

## 注

## 関連リンク

[Set-TraceSource](Set-TraceSource.md)

[Trace-Command](Trace-Command.md)

