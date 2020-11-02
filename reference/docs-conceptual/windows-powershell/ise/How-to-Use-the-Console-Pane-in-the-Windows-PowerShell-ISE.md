---
ms.date: 01/02/2020
title: Windows PowerShell ISE でコンソール ウィンドウを使用する方法
description: Windows PowerShell ISE でコンソール ウィンドウを使用する方法
ms.openlocfilehash: 7f6d3f5a3e4e596beb0d5c0bc395e3e7bf39906d
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663657"
---
# <a name="how-to-use-the-console-pane-in-the-windows-powershell-ise"></a>Windows PowerShell ISE でコンソール ウィンドウを使用する方法

Windows PowerShell Integrated Scripting Environment (ISE) のコンソール ウィンドウは、スタンドアロンの Windows PowerShell ISE コンソール ウィンドウとまったく同じ動作をします。

コンソール ウィンドウでコマンドを実行するには、コマンドを入力し、<kbd>Enter</kbd> キーを押します。 順番に実行する複数のコマンドを入力するには、コマンドとコマンドの間で <kbd>Shift</kbd> + <kbd>Enter</kbd> キーを押します。 コマンドを入力する際の支援機能については、「[スクリプト ウィンドウとコンソール ウィンドウでタブ補完を使用する方法](How-to-Use-Tab-Completion-in-the-Script-Pane-and-Console-Pane.md)」をご覧ください。

コマンドを停止するには、ツール バーで **[実行を中止]** をクリックするか、または <kbd>Ctrl</kbd> + <kbd>Break</kbd> キーを押します。 コンテキストが明確な場合は、<kbd>Ctrl</kbd> + <kbd>C</kbd> キーを押してコマンドを停止することもできます。 たとえば、現在のウィンドウでテキストが選択されている場合、<kbd>Ctrl</kbd> + <kbd>C</kbd> キーはコピー操作にマップされます。

Windows PowerShell v3 以降で、出力ウィンドウがコンソール ウィンドウと結合されました。 これの利点は、スタンドアロンの Windows PowerShell コンソールと同様の動作になったことと、この 2 つが独立していたときに必要だった手順の違いが解消されたことです。 次のようにすることができます。

- コンソール ウィンドウからテキストを選択してクリップボードにコピーし、他の任意のウィンドウに貼り付けます。 テキストを選択するには、出力ウィンドウでマウスをクリックし、マウス ボタンを押したまま、取り込みたいテキスト上をドラッグします。 また、<kbd>Shift</kbd> キーを押しながら方向キーを押して、テキストを選択することもできます。 その後、 <kbd>Ctrl</kbd> + <kbd>C</kbd> キーを押すか、ツール バーの **[コピー]** アイコンをクリックします。

- 選択したテキストを現在のカーソル位置に貼り付けます。 ツール バーの **[貼り付け]** アイコンをクリックします。

- コンソール ウィンドウ内のすべてのテキストをクリアします。 コンソール ウィンドウをクリアするには、ツール バーの **[コンソール ウィンドウのクリア]** アイコンをクリックするか、コマンド `Clear-Host` またはそのエイリアスである `cls` を実行します。

## <a name="see-also"></a>参照

- [Windows PowerShell ISE の紹介](Introducing-the-Windows-PowerShell-ISE.md)
