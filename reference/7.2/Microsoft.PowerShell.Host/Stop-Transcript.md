---
external help file: Microsoft.PowerShell.ConsoleHost.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Host
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.host/stop-transcript?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Stop-Transcript
ms.openlocfilehash: 16b41711e98276fee37d56f77f57e7372daa4cf3
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601807"
---
# Stop-Transcript

## 概要
トランスクリプトを停止します。

## SYNTAX

```
Stop-Transcript [<CommonParameters>]
```

## Description

コマンドレットでは、 `Stop-Transcript` コマンドレットによって開始されたトランスクリプトを停止し `Start-Transcript` ます。
または、セッションを終了してトランスクリプトを停止することもできます。

## 例

### 例 1: すべてのトランスクリプトを停止する

```powershell
Stop-Transcript
```

このコマンドは、すべてのトランスクリプトを停止します。

## PARAMETERS

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### System.String

このコマンドレットは、ステータスメッセージと出力ファイルへのパスを含む文字列を返します。

## 注

* トランスクリプトが開始されていない場合、コマンドはエラーになります。

*

## 関連リンク

[Start-Transcript](Start-Transcript.md)

