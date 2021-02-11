---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 11/06/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/exit-pshostprocess?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Exit-PSHostProcess
ms.openlocfilehash: 368d29b7cdcbe2725399da0896eed87ddb372236
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94342146"
---
# Exit-PSHostProcess

## 概要
ローカルプロセスで対話型セッションを終了します。

## SYNTAX

```
Exit-PSHostProcess [<CommonParameters>]
```

## Description

コマンドレットでは、 `Exit-PSHostProcess` コマンドレットを実行して、開いたローカルプロセスとの対話型セッションを閉じ `Enter-PSHostProcess` ます。 プロセス内で `Exit-PSHostProcess` 実行されているスクリプトのデバッグまたはトラブルシューティングが完了したら、プロセス内からこのコマンドレットを実行します。 PowerShell 6.2 以降では、このコマンドレットは Windows 以外のプラットフォームでサポートされています。

## 例

### 例 1: プロセスを終了する

```
[Process:1520]: PS>  Exit-PSHostProcess
PS>
```

この例では、「」で説明されているように、プロセス内の実行空間で実行されているスクリプトをデバッグするために、アクティブなプロセスで作業してい `Enter-PSHostProcess` ます。 デバッガーを終了するコマンドを入力した後 `exit` 、コマンドレットを実行して `Exit-PSHostProcess` 、プロセスとの対話型セッションを終了します。
コマンドレットでは、プロセス内のセッションを閉じ、プロンプトに戻り `PS C:\>` ます。

## PARAMETERS

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

## 出力

## 注

## 関連リンク

[Enter-PSHostProcess](Enter-PSHostProcess.md)

