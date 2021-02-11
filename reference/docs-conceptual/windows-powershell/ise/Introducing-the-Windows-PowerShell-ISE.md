---
ms.date: 08/14/2018
title: Windows PowerShell ISE の紹介
description: PowerShell ISE は、Windows PowerShell でコマンドを実行して、単一の Windows ベースのグラフィカル ユーザー インターフェイスでスクリプトの書き込み、テスト、およびデバッグを行えるようにするためのホスト アプリケーションです。
ms.openlocfilehash: ab2b11e5d81933b166d404c0b24c96aa73253895
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663619"
---
# <a name="the-windows-powershell-ise"></a>Windows PowerShell ISE

Windows PowerShell Integrated Scripting Environment (ISE) は、Windows PowerShell のホスト アプリケーションです。 ISE では、コマンドを実行して、単一の Windows ベースのグラフィカル ユーザー インターフェイスでスクリプトの書き込み、テスト、およびデバッグを行うことが可能です。 ISE では、複数行の編集、タブ補完、構文の色分け、選択的な実行、状況依存のヘルプ、および右から左方向の言語のサポートを提供します。 メニュー項目とキーボード ショートカットは、Windows PowerShell コンソールで実行する多数の同じタスクにマッピングされます。 たとえば、ISE でスクリプトをデバッグするとき、編集ウィンドウでコード行を右クリックするとブレークポイントを設定できます。

## <a name="support"></a>サポート

ISE は、Windows PowerShell V2 で初めて導入され、PowerShell V3 で再設計されました。 ISE は、Windows PowerShell V5.1 までのすべてのサポートされているバージョンの Windows PowerShell でサポートされています。

> [!NOTE]
> PowerShell ISE は、アクティブな機能開発の対象ではなくなりました。 Windows に付属するコンポーネントとして、セキュリティや優先順位の高いサービスに関する修正プログラムが引き続き公式でサポートされます。
> 現在、Windows から ISE が削除される予定はありません。
>
> PowerShell v6 以降では、ISE はサポートされていません。 ISE の代替をお探しのユーザーは、[PowerShell 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)と共に [Visual Studio Code](https://code.visualstudio.com/) を使用することをお勧めします。

## <a name="key-features"></a>主な機能

Windows PowerShell ISE の主な機能:

- 複数行の編集:[コマンド] ペインで現在の行の下に空白行を挿入するには、<kbd>Shift</kbd>+<kbd>Enter</kbd> キーを押します。
- 選択して実行:スクリプトの一部を実行するには、実行するテキストを選択して、 **[スクリプトの実行]** ボタンをクリックします。 または、<kbd>F5</kbd> キーを押します。
- 状況依存ヘルプ:「`Invoke-Item`」と入力してから、<kbd>F1</kbd> キーを押します。 ヘルプ ファイルが開き、`Invoke-Item` コマンドレットの記事が表示されます。

Windows PowerShell ISE では、外観の一部をカスタマイズすることができます。 また、独自の Windows PowerShell プロファイル スクリプトもあります。

## <a name="to-start-the-windows-powershell-ise"></a>Windows PowerShell ISE を開始するには

**[スタート]** をクリックし、 **[Windows PowerShell]** を選択して、 **[Windows PowerShell ISE]** をクリックします。
代わりに、任意のコマンド シェルまたは [実行] ボックスに `powershell_ise.exe` を入力することも可能です。

## <a name="to-get-help-in-the-windows-powershell-ise"></a>Windows PowerShell ISE のヘルプを利用するには

**[ヘルプ]** メニューで、 **[Windows PowerShell ヘルプ]** をクリックします。 または、<kbd>F1</kbd> キーを押します。 開かれたファイルに、Windows PowerShell ISE と Windows PowerShell に関する説明が表示されます。これには、`Get-Help` コマンドレットから入手できるヘルプがすべて含まれています。
