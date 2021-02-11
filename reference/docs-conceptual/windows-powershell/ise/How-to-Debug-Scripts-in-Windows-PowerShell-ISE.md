---
ms.date: 01/02/2020
title: Windows PowerShell ISE でスクリプトをデバッグする方法
description: この記事では、Windows PowerShell ISE のビジュアル デバッグ機能を使ってローカル コンピューター上でスクリプトをデバッグする方法について説明します。
ms.openlocfilehash: c2e45e3b165e16c9dcd6852550d13fc2426986fa
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663805"
---
# <a name="how-to-debug-scripts-in-windows-powershell-ise"></a>Windows PowerShell ISE でスクリプトをデバッグする方法

この記事では、Windows PowerShell Integrated Scripting Environment (ISE) のビジュアル デバッグ機能を使ってローカル コンピューター上でスクリプトをデバッグする方法について説明します。

## <a name="how-to-manage-breakpoints"></a>ブレークポイントを管理する方法

ブレークポイントは、操作を一時停止するように指定したスクリプト内の位置です。一時停止している間に、変数の現在の状態や、スクリプトが実行されている環境を確認できます。
スクリプトがブレークポイントで一時停止したら、コンソール ウィンドウでコマンドを実行して、スクリプトの状態を確認できます。 変数を出力したり、その他のコマンドを実行したりできます。 また、現在実行しているスクリプトのコンテキストで表示されているすべての変数の値を変更することもできます。 必要な事項を確認した後、スクリプトの操作を再開できます。

Windows PowerShell のデバッグ環境では、次の 3 種類のブレークポイントを設定できます。

1. **行のブレークポイント** 。 スクリプトの操作中に指定した行に達すると、スクリプトは一時停止します。

1. **変数のブレークポイント。** 指定した変数の値が変更されるたびに、スクリプトは一時停止します。

1. **コマンドのブレークポイント。** スクリプトの操作中、指定したコマンドを実行する前に、スクリプトは一時停止します。 ブレークポイントを必要な操作のみにフィルターで絞り込むために、パラメーターを指定できます。 コマンドには、作成した関数を指定することもできます。

この 3 種類のうち、行のブレークポイントだけは、Windows PowerShell ISE デバッグ環境で、メニューやキーボード ショートカットを使って設定できます。 他の 2 つの種類のブレークポイントは、コンソール ウィンドウから [Set-PSBreakpoint ](/powershell/module/Microsoft.PowerShell.Utility/Set-PSBreakpoint) コマンドレットを使って設定できます。 このセクションでは、Windows PowerShell ISE でメニューが使用可能な場合は使用してデバッグ作業を実行する方法と、スクリプトを使用してさまざまなコマンドをコンソール ウィンドウから実行する方法を説明します。

### <a name="to-set-a-breakpoint"></a>ブレークポイントを設定するには

ブレークポイントをスクリプトに設定するには、まずブレークポイントを保存する必要があります。 行のブレークポイントを設定する行を右クリックしてから、 **[ブレークポイントの設定/解除]** をクリックします。 または、行のブレークポイントを設定する行をクリックしてから、 <kbd>F9</kbd> キーを押すか、 **[デバッグ]** メニューの **[ブレークポイントの設定/解除]** をクリックします。

次のスクリプトは、コンソール ウィンドウから [Set-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Set-PSBreakpoint) コマンドレットを使って変数のブレークポイントを設定する方法の例を示しています。

```powershell
# This command sets a breakpoint on the Server variable in the Sample.ps1 script.
Set-PSBreakpoint -Script sample.ps1 -Variable Server
```

### <a name="list-all-breakpoints"></a>すべてのブレークポイントの一覧を表示

現在の Windows PowerShell セッションで設定されているすべてのブレークポイントを表示します。

**[デバッグ]** メニューの **[ブレークポイントの一覧を表示]** をクリックします。 次のスクリプトは、コンソール ウィンドウから [Get-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Get-PSBreakpoint) コマンドレットを使ってすべてのブレークポイントの一覧を表示する方法の例を示しています。

```powershell
# This command lists all breakpoints in the current session.
Get-PSBreakpoint
```

### <a name="remove-a-breakpoint"></a>ブレークポイントの解除

ブレークポイントを解除すると、そのブレークポイントは削除されます。

後で使う可能性があるブレークポイントは、解除するのではなく[無効にする](#disable-a-breakpoint)ことができます。 ブレークポイントを解除する行を右クリックしてから、 **[ブレークポイントの設定/解除]** をクリックします。
または、ブレークポイントを解除する行をクリックして、 **[デバッグ]** メニューの **[ブレークポイントの設定/解除]** をクリックします。 次のスクリプトは、コンソール ウィンドウから [Remove-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Remove-PSBreakpoint) コマンドレットを使って、指定した ID のブレークポイントを解除する方法の例を示しています。

```powershell
# This command deletes the breakpoint with breakpoint ID 2.
Remove-PSBreakpoint -Id 2
```

### <a name="remove-all-breakpoints"></a>すべてのブレークポイントの削除

現在のセッション内で定義されたすべてのブレークポイントを削除するには、 **[デバッグ]** メニューの **[すべてのブレークポイントを削除]** をクリックします。

次のスクリプトは、コンソール ウィンドウから [Remove-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Remove-PSBreakpoint) コマンドレットを使ってすべてのブレークポイントを削除する方法の例を示しています。

```powershell
# This command deletes all of the breakpoints in the current session.
Get-PSBreakpoint | Remove-PSBreakpoint
```

### <a name="disable-a-breakpoint"></a>ブレークポイントの無効化

ブレークポイントを無効にしても、そのブレークポイントは削除されず、再び有効にするまでの間オフになります。 特定の行のブレークポイントを無効にするには、ブレークポイントを無効にする行を右クリックし、 **[ブレークポイントの無効化]** をクリックします。 または、ブレークポイントを無効にする行をクリックしてから、 <kbd>F9</kbd> キーを押すか、 **[デバッグ]** メニューの **[ブレークポイントの無効化]** をクリックします。 次のスクリプトは、コンソール ウィンドウから [Disable-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Disable-PSBreakpoint) コマンドレットを使って、指定した ID のブレークポイントを無効にする方法の例を示しています。

```powershell
# This command disables the breakpoint with breakpoint ID 0.
Disable-PSBreakpoint -Id 0
```

### <a name="disable-all-breakpoints"></a>すべてのブレークポイントの無効化

ブレークポイントを無効にしても、そのブレークポイントは削除されず、再び有効にするまでの間オフになります。 現在のセッション内のすべてのブレークポイントを無効にするには、 **[デバッグ]** メニューの **[すべてのブレークポイントの無効化]** をクリックします。 次のスクリプトは、コンソール ウィンドウから [Disable-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Disable-PSBreakpoint) コマンドレットを使ってすべてのブレークポイントを無効にする方法の例を示しています。

```powershell
# This command disables all breakpoints in the current session.
# You can abbreviate this command as: "gbp | dbp".
Get-PSBreakpoint | Disable-PSBreakpoint
```

### <a name="enable-a-breakpoint"></a>ブレークポイントの有効化

特定の行のブレークポイントを有効にするには、ブレークポイントを有効にする行を右クリックし、 **[ブレークポイントの有効化]** をクリックします。 または、ブレークポイントを有効にする行をクリックしてから、 <kbd>F9</kbd> キーを押すか、 **[デバッグ]** メニューの **[ブレークポイントの有効化]** をクリックします。 次のスクリプトは、コンソール ウィンドウから [Enable-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Enable-PSBreakpoint) コマンドレットを使って特定のブレークポイントを有効にする方法の例を示しています。

```powershell
# This command enables breakpoints with breakpoint IDs 0, 1, and 5.
Enable-PSBreakpoint -Id 0, 1, 5
```

### <a name="enable-all-breakpoints"></a>すべてのブレークポイントの有効化

現在のセッション内で定義されたすべてのブレークポイントを有効にするには、 **[デバッグ]** メニューの **[すべてのブレークポイントを有効化]** をクリックします。 次のスクリプトは、コンソール ウィンドウから [Enable-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Enable-PSBreakpoint) コマンドレットを使ってすべてのブレークポイントを有効にする方法の例を示しています。

```powershell
# This command enables all breakpoints in the current session.
# You can abbreviate the command by using their aliases: "gbp | ebp".
Get-PSBreakpoint | Enable-PSBreakpoint
```

## <a name="how-to-manage-a-debugging-session"></a>デバッグ セッションを管理する方法

デバッグを開始する前に、1 つ以上のブレークポイントを設定する必要があります。 ブレークポイントを設定するには、まず、デバッグするスクリプトを保存する必要があります。 ブレークポイントを設定する手順については、「[ブレークポイントを管理する方法](#how-to-manage-breakpoints)」または「[Set-PSBreakpoint](/powershell/module/Microsoft.PowerShell.Utility/Set-PSBreakpoint)」をご覧ください。
デバッグを開始した後は、デバッグを停止するまで、スクリプトを編集できません。 1 つ以上のブレークポイントを設定したスクリプトは、実行前に自動的に保存されます。

### <a name="to-start-debugging"></a>デバッグを開始するには

<kbd>F5</kbd> キーを押すか、ツールバーの **[スクリプトを実行]** アイコンをクリックするか、 **[デバッグ]** メニューの **[実行/続行]** をクリックします。 スクリプトは、最初のブレークポイントを検出するまで実行されます。 ブレークポイントを検出した位置で操作が一時停止し、一時停止した行が強調表示されます。

### <a name="to-continue-debugging"></a>デバッグを続行するには

<kbd>F5</kbd> キーを押すか、ツールバーの **[スクリプトを実行]** アイコンをクリックするか、 **[デバッグ]** メニューの **[実行/続行]** をクリックするか、コンソール ウィンドウで `C` と入力してから <kbd>Enter</kbd> キーを押します。 これにより、次のブレークポイントまで、またはこれ以上ブレークポイントが検出されない場合は、スクリプトの最後までスクリプトの実行が継続されます。

### <a name="to-view-the-call-stack"></a>呼び出し履歴を表示するには

呼び出し履歴には、スクリプト内で現在実行中の位置が表示されます。 スクリプトが別の関数によって呼び出された関数で実行されている場合、そのことは画面の出力に行が追加されることによって示されます。 一番下の行には、元のスクリプトと、関数が呼び出された行が表示されます。 その上の行には、別の関数が呼び出された場合に、その関数と、関数が呼び出された行が表示されます。 一番上の行には、ブレークポイントが設定されている、現在の行の現在のコンテキストが表示されます。

一時停止中に現在の呼び出し履歴を表示するには、 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>D</kbd> キーを押すか、 **[デバッグ]** メニューの **[呼び出し履歴の表示]** をクリックするか、コンソール ウィンドウで `K` と入力してから <kbd>Enter</kbd> キーを押します。

### <a name="to-stop-debugging"></a>デバッグを停止するには

<kbd>Shift</kbd> + <kbd>F5</kbd> キーを押すか、 **[デバッグ]** メニューの **[デバッガーの停止]** をクリックするか、コンソール ウィンドウで `Q` と入力してから <kbd>Enter</kbd> キーを押します。

## <a name="how-to-step-over-step-into-and-step-out-while-debugging"></a>デバッグ中にステップ オーバー、ステップ イン、ステップ アウトする方法

ステッピングは、一度に 1 つのステートメントを実行するプロセスです。 コードの行で停止し、変数の値とシステムの状態を確認できます。 次の表では、ステップ オーバー、ステップ イン、ステップ アウトなどの一般的なデバッグ タスクについて説明します。

| デバッグ タスク |                                                                                                                   説明                                                                                                                    |                                                      PowerShell ISE で実行する方法                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ステップ イン**  | 現在のステートメントを実行し、次のステートメントで停止します。 現在のステートメントが関数やスクリプトの呼び出しの場合、デバッガーはその関数やスクリプトにステップ インします。それ以外の場合、デバッガーは次のステートメントで停止します。                      | <kbd>F11</kbd> キーを押すか、 **[デバッグ]** メニューの **[ステップ イン]** をクリックするか、コンソール ウィンドウに `S` と入力して <kbd>Enter</kbd> キーを押します。                 |
| **ステップ オーバー**  | 現在のステートメントを実行し、次のステートメントで停止します。 現在のステートメントが関数やスクリプトの呼び出しの場合、デバッガーはその関数やスクリプトの全体を実行し、関数を呼び出した後の次のステートメントで停止します。 | <kbd>F10</kbd> キーを押すか、 **[デバッグ]** メニューの **[ステップ オーバー]** をクリックするか、コンソール ウィンドウに `V` と入力して <kbd>Enter</kbd> キーを押します。                 |
| **ステップ アウト**   | 現在の関数からステップ アウトし、関数が入れ子になっている場合は 1 つ上のレベルに移動します。 メイン ボディの場合は、スクリプトが最後まで、または次のブレークポイントまで実行されます。 スキップしたステートメントは実行されますが、ステップ スルーされることはありません。                   | <kbd>Shift</kbd> + <kbd>F11</kbd> キーを押すか、 **[デバッグ]** メニューの **[ステップ アウト]** をクリックするか、コンソール ウィンドウに `O` と入力して <kbd>Enter</kbd> キーを押します。 |
| **続行**   | 最後まで、または次のブレークポイントまで実行を続けます。 スキップした関数と呼び出しは実行されますが、ステップ スルーされることはありません。                                                                                                          | <kbd>F5</kbd> キーを押すか、 **[デバッグ]** メニューの **[実行/続行]** をクリックするか、コンソール ウィンドウに `C` と入力して <kbd>Enter</kbd> キーを押します。               |

## <a name="how-to-display-the-values-of-variables-while-debugging"></a>デバッグ中に変数の値を表示する方法

コードをステップ スルーするとき、スクリプト内の変数の現在値を表示できます。

### <a name="to-display-the-values-of-standard-variables"></a>標準変数の値を表示するには

以下のいずれかの方法を使用します。

- スクリプト ウィンドウで、変数にマウスのカーソルを合わせると、ツール ヒントとしてその変数の値が表示されます。

- コンソール ウィンドウで、変数名を入力して <kbd>Enter</kbd> キーを押します。

ISE のすべてのウィンドウは、常に同じスコープ内にあります。 そのため、スクリプトをデバッグしている間は、コンソール ウィンドウに入力したコマンドはスクリプト スコープで実行されます。 これにより、コンソール ウィンドウを使って、スクリプト内でのみ定義されている変数の値を調べたり、スクリプト内でのみ定義されている関数を呼び出したりできます。

### <a name="to-display-the-values-of-automatic-variables"></a>自動変数の値を表示するには

上記の方法を使えば、スクリプトのデバッグ中にほとんどすべての変数の値を表示できます。 ただし、これらの方法は、次の自動変数に対しては機能しません。

- `$_`

- `$Input`

- `$MyInvocation`

- `$PSBoundParameters`

- `$Args`

これらの変数のいずれかの値を表示しようとする場合は、スクリプト内の変数の値ではなく、デバッガーが使う内部パイプラインでその変数の値を取得します。 いくつかの変数 (`$_`、`$Input`、`$MyInvocation`、`$PSBoundParameters`、`$Args`) についてこれを回避するには、次の方法を使います。

1. スクリプトで、自動変数の値を新しい変数に割り当てます。

1. その新しい変数の値を表示するには、スクリプト ウィンドウ内でマウスのカーソルを新しい変数に合わせるか、コンソール ウィンドウに新しい変数を入力します。

たとえば、`$MyInvocation` 変数の値を表示するには、スクリプト内でその値を新しい変数 (`$scriptName` など) に割り当ててから、マウスのカーソルを合わせるか、`$scriptName` 変数を入力します。

```powershell
# In C:\ps-test\MyScript.ps1
$scriptName = $MyInvocation.MyCommand.Path
```

```PowerShell
# In the Console Pane:
.\MyScript.ps1
$scriptName
```

```Output
C:\ps-test\MyScript.ps1
```

## <a name="see-also"></a>参照

[Windows PowerShell ISE の操作](exploring-the-windows-powershell-ise.md)
