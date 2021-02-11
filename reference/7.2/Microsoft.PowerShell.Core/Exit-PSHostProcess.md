---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 11/06/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/exit-pshostprocess?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Exit-PSHostProcess
ms.openlocfilehash: a1a8c6fcc16bcddc3bfcdade56cd75aadd179b74
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600996"
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

