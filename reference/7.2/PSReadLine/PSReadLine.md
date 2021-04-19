---
Download Help Link: https://aka.ms/powershell72-help
Help Version: 7.2.0.0
Locale: en-US
Module Guid: 5714753b-2afd-4492-a5fd-01d9e2cff8b5
Module Name: PSReadLine
ms.date: 02/10/2020
schema: 2.0.0
title: PSReadLine
ms.openlocfilehash: 9425f72ce4002fa871ef6b687d76f92ddf6b489e
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103193900"
---
# PSReadLine モジュール

## 説明

PSReadLine モジュールには、PowerShell でコマンドライン編集環境をカスタマイズできるコマンドレットが含まれています。 この記事では、PSReadLine v1.0 の現在のベータ版について説明します。

> [!NOTE]
> PowerShell 7.0 以降では、スクリーンリーダープログラムが検出されると、PowerShell は Windows で PSReadLine の自動読み込みをスキップします。 現在、PSReadLine は、スクリーンリーダーではうまく機能しません。 Windows での PowerShell 7.0 の既定の表示と書式設定は正常に機能します。 必要に応じて、モジュールを手動で読み込むことができます。

## PSReadLine コマンドレット

### [PSConsoleHostReadLine](PSConsoleHostReadLine.md)
PSReadLine のメインエントリポイント。

### [Get-PSReadLineKeyHandler](Get-PSReadLineKeyHandler.md)
PSReadLine モジュールのバインドされたキー関数を取得します。

### [Get-PSReadLineOption](Get-PSReadLineOption.md)
構成可能なオプションの値を取得します。

### [PSConsoleHostReadLine](PSConsoleHostReadLine.md)
この関数は、PSReadLine のメインエントリポイントです。

### [-PSReadLineKeyHandler を削除します。](Remove-PSReadLineKeyHandler.md)
キーバインドを削除します。

### [設定-PSReadLineKeyHandler](Set-PSReadLineKeyHandler.md)
ユーザー定義キーまたは PSReadLine キーハンドラー関数にキーをバインドします。

### [設定-PSReadLineOption](Set-PSReadLineOption.md)
**Psreadline** でのコマンドライン編集の動作をカスタマイズします。
