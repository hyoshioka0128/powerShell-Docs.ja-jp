---
ms.date: 01/02/2020
title: Windows PowerShell ISE の操作
description: この記事では、Windows PowerShell ISE の機能の概要について説明します。
ms.topic: conceptual
ms.custom: ISE-F1-page
ms.openlocfilehash: 91161763c817972a62b4da1558a7ca119d8c8616
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97090448"
---
# <a name="exploring-the-windows-powershell-ise"></a>Windows PowerShell ISE の操作

Windows PowerShell Integrated Scripting Environment (ISE) を使用すると、コマンドやスクリプトを作成、実行、デバッグできます。 Windows PowerShell ISE は、メニュー バー、Windows PowerShell タブ、ツール バー、スクリプト タブ、スクリプト ウィンドウ、コンソール ウィンドウ、ステータス バー、文字サイズ スライダー、状況依存のヘルプで構成されています。

## <a name="menu-bar"></a>メニュー バー

メニュー バーには、 **[ファイル]** 、 **[編集]** 、 **[表示]** 、 **[ツール]** 、 **[デバッグ]** 、 **[アドオン]** 、 **[ヘルプ]** の各メニューがあります。 メニューのボタンを使用すると、スクリプトの記述と実行、および Windows PowerShell ISE でのコマンドの実行に関連したタスクを実行できます。 さらに、[ISE オブジェクト モデルの階層](object-model/The-ISE-Object-Model-Hierarchy.md)を使用するスクリプトを実行して、[アドオン ツール](object-model/The-ISEAddOnTool-Object.md)をメニュー バーに配置することもできます。

## <a name="windows-powershell-tabs"></a>Windows PowerShell タブ

Windows PowerShell タブは、Windows PowerShell スクリプトが動作する環境です。 Windows PowerShell ISE で新しい Windows PowerShell タブを開いて、ローカル コンピューターまたはリモート コンピューターに別個の環境を作成することができます。 最大 8 つの PowerShell タブを同時に開くことができます。

詳細については、「[Windows PowerShell ISE で PowerShell タブを作成する方法](How-to-Create-a-PowerShell-Tab-in-Windows-PowerShell-ISE.md)」を参照してください。

## <a name="toolbar"></a>ツール バー

ツール バーには次のボタンがあります。

|             ボタン             |                                                                                     Function                                                                                     |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[新規作成]**                        | 新しいスクリプトを開きます。                                                                                                                                                              |
| **[ファイル]**                       | 既存のスクリプトまたはファイルを開きます。                                                                                                                                                |
| **および**                       | スクリプトまたはファイルを保存します。                                                                                                                                                          |
| **切り取り**                        | 選んだテキストを切り取り、クリップボードにコピーします。                                                                                                                           |
| **Copy** に設定する必要があります                       | 選択したテキストをクリップボードにコピーします。                                                                                                                                       |
| **[貼り付け]**                      | カーソル位置にクリップボードの内容を貼り付けます。                                                                                                                     |
| **出力ウィンドウをクリア**          | 出力ウィンドウの内容をすべてクリアします。                                                                                                                                           |
| **元に戻す**                       | 実行した直前のアクションを元に戻します。                                                                                                                                     |
| **やり直す**                       | 元に戻した直前のアクションを実行します。                                                                                                                                        |
| **スクリプトを実行**                 | スクリプトを実行します。                                                                                                                                                                   |
| **選択範囲の実行**              | スクリプトの選んだ部分を実行します。                                                                                                                                             |
| **実行を中止**             | 実行中のスクリプトを中止します。                                                                                                                                                  |
| **リモート PowerShell タブの新規作成**  | リモート コンピューターにセッションを確立する PowerShell タブを新規作成します。 ダイアログ ボックスが表示され、リモート接続を確立するために必要な詳細情報を入力するように求められます。 |
| **PowerShell.exe を起動**       | PowerShell コンソールを開きます。                                                                                                                                                      |
| **スクリプト ウィンドウを上に表示**       | スクリプト ウィンドウを画面の上部に移動します。                                                                                                                                 |
| **スクリプト ウィンドウを右側に表示**     | スクリプト ウィンドウを画面の右側に移動します。                                                                                                                               |
| **スクリプト ウィンドウを最大表示** | スクリプト ウィンドウを最大表示します。                                                                                                                                                       |

## <a name="script-tab"></a>スクリプト タブ

編集中のスクリプトの名前を表示します。 スクリプト タブをクリックして、編集するスクリプトを選択できます。

スクリプト タブをポイントすると、スクリプト ファイルへの完全修飾パスがヒントに表示されます。

## <a name="script-pane"></a>スクリプト ウィンドウ

スクリプトを作成して実行することができます。 スクリプト ウィンドウでは、既存のスクリプトを開いたり、編集および実行したりできます。 詳細については、「[Windows PowerShell ISE でスクリプトを記述および実行する方法](How-to-Write-and-Run-Scripts-in-the-Windows-PowerShell-ISE.md)」を参照してください。

## <a name="console-pane"></a>コンソール ウィンドウ

実行したコマンドおよびスクリプトの結果が表示されます。 コマンドは、コンソール ウィンドウで実行できます。 コンソール ウィンドウの内容をコピーおよびクリアすることもできます。

詳細については、以下の記事を参照してください。

- [Windows PowerShell ISE でコンソール ウィンドウを使用する方法](How-to-Use-the-Console-Pane-in-the-Windows-PowerShell-ISE.md)
- [Windows PowerShell ISE でスクリプトをデバッグする方法](How-to-Debug-Scripts-in-Windows-PowerShell-ISE.md)
- [スクリプト ウィンドウとコンソール ウィンドウでタブ補完を使用する方法](How-to-Use-Tab-Completion-in-the-Script-Pane-and-Console-Pane.md)

## <a name="status-bar"></a>ステータス バー

実行したコマンドおよびスクリプトが完了しているかどうかを確認できます。 ステータス バーは、画面の最下部に表示されます。 エラー メッセージの選んだ部分がステータス バーに表示されます。

## <a name="text-size-slider"></a>文字サイズ スライダー

画面の文字のサイズを拡大または縮小することができます。

## <a name="help"></a>ヘルプ

Windows PowerShell ISE のヘルプは docs.microsoft.com にあります。 ヘルプを開くには、 **[ヘルプ]** メニューの **[Windows PowerShell ISE ヘルプ]** をクリックするか、<kbd>F1</kbd> キーを押します。これは、スクリプト ウィンドウまたはコンソール ウィンドウでコマンドレット名の上にカーソルがある場合を除き、どこからでも実行できます。
**[ヘルプ]** メニューからは、`Update-Help` コマンドレットを実行することや、コマンド ウィンドウを表示することもできます。コマンド ウィンドウは、コマンドレットのすべてのパラメーターを示したり、使いやすいフォームにパラメーターを入力できるようにしたりして、コマンドの作成を助けます。

## <a name="see-also"></a>参照

- [Windows PowerShell ISE の紹介](Introducing-the-Windows-PowerShell-ISE.md)
- [Windows PowerShell ISE でプロファイルを使用する方法](How-to-Use-Profiles-in-Windows-PowerShell-ISE.md)
- [Windows PowerShell ISE のアクセシビリティ](Accessibility-in-Windows-PowerShell-ISE.md)
- [Windows PowerShell ISE のキーボード ショートカット](Keyboard-Shortcuts-for-the-Windows-PowerShell-ISE.md)
