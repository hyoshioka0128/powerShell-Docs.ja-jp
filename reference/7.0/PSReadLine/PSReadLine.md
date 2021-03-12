---
Download Help Link: https://go.microsoft.com/fwlink/?linkid=2113630
Help Version: 7.0.1.0
keywords: powershell
Locale: en-US
Module Guid: 5714753b-2afd-4492-a5fd-01d9e2cff8b5
Module Name: PSReadLine
ms.date: 02/10/2020
schema: 2.0.0
title: PSReadLine
ms.openlocfilehash: fc059318507b7a875eedf47692c1b6e3572efbbc
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103195566"
---
# PSReadLine モジュール

## 説明

PSReadLine モジュールには、PowerShell でコマンドライン編集環境をカスタマイズできるコマンドレットが含まれています。 この記事では、PSReadLine v2.0 について説明します。 このバージョンは、PowerShell v6 と Windows 10 10 月2018更新プログラム (ビルド 1809) に付属しています。

> [!NOTE]
> PowerShell 7.0 以降では、スクリーンリーダープログラムが検出されると、PowerShell は Windows で PSReadLine の自動読み込みをスキップします。 現在、PSReadLine は、スクリーンリーダーではうまく機能しません。 Windows での PowerShell 7.0 の既定の表示と書式設定は正常に機能します。 必要に応じて、モジュールを手動で読み込むことができます。

## PSReadLine コマンドレット

### [PSConsoleHostReadLine](PSConsoleHostReadLine.md)
PSReadLine のメインエントリポイント。

### [Get-PSReadLineKeyHandler](Get-PSReadLineKeyHandler.md)
PSReadLine モジュールのキーバインドを取得します。

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

