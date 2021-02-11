---
description: PSReadLine では、PowerShell コンソールでのコマンドライン編集エクスペリエンスが向上しています。
Locale: en-US
ms.date: 11/23/2020
online version: https://docs.microsoft.com/powershell/module/psreadline/about/about_psreadline?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: PSReadLine について
ms.openlocfilehash: 4836abfec465ba7cdfb6800c1e60104fba19ce08
ms.sourcegitcommit: 560a9f3c3148acab4655e91e8b07745ab74d5d26
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "99600977"
---
# <a name="psreadline"></a>PSReadLine

## <a name="about_psreadline"></a>about_PSReadLine

## <a name="short-description"></a>簡単な説明

PSReadLine では、PowerShell コンソールでのコマンドライン編集エクスペリエンスが向上しています。

## <a name="long-description"></a>長い説明

PSReadLine 2.2 では、PowerShell コンソールの強力なコマンドライン編集エクスペリエンスが提供されます。 次の機能を提供します。

- コマンドラインの構文の色分け
- 構文エラーを視覚的に示します。
- より優れたマルチラインエクスペリエンス (編集と履歴の両方)
- カスタマイズ可能なキーバインド
- Cmd モードと Emacs モード
- 多くの構成オプション
- Bash スタイル補完 (Cmd モードでは省略可能、Emacs モードでは既定)
- Emacs yank/kill-リング
- PowerShell トークンベースの "word" の移動と強制終了
- 予測 IntelliSense

PSReadLine 2.2.0 には、次の2つの新しい予測 IntelliSense 機能が追加されました。

- **PredictionViewStyle** パラメーターを追加して、新しいを選択できるようにしました `ListView` 。
- PSReadLine を PS 7.1 で導入された api に接続して、 `CommandPrediction` ユーザーがカスタムソースから提案を表示できる予測モジュールをインポートできるようにしました。

PSReadLine には PowerShell 3.0 以降が必要です。 PSReadLine は、既定のコンソールホスト、Visual Studio Code、ウィンドウターミナルで動作します。 PowerShell ISE では機能しません。

PSReadLine 2.2.0 は PowerShell 7.2 に付属しており、サポートされているすべてのバージョンの PowerShell でサポートされています。 PowerShell ギャラリーからインストールできます。
サポートされているバージョンの PowerShell に PSReadLine 2.2.0 をインストールするには、次のコマンドを実行します。

```powershell
Install-Module -Name PSReadLine -AllowPrerelease
```

> [!NOTE]
> PowerShell 7.0 以降では、スクリーンリーダープログラムが検出されると、PowerShell は Windows で PSReadLine の自動読み込みをスキップします。 現在、PSReadLine は、スクリーンリーダーではうまく機能しません。 Windows での PowerShell 7.0 の既定の表示と書式設定は正常に機能します。 必要に応じて、モジュールを手動で読み込むことができます。

## <a name="predictive-intellisense"></a>予測 IntelliSense

予測 IntelliSense は、ユーザーがコマンドを正常に完了できるように、タブ補完の概念を追加したものです。 ユーザーは、ユーザーの履歴や追加のドメイン固有のプラグインからの照合予測に基づいて、完全なコマンドを検出、編集、および実行することができます。

### <a name="enable-predictive-intellisense"></a>予測 IntelliSense を有効にする

既定では、予測 IntelliSense は無効になっています。 予測を有効にするには、次のコマンドを実行します。

```powershell
Set-PSReadLineOption -PredictionSource History
```

**PredictionSource** パラメーターでは、ドメイン固有の要件とカスタムの要件に対してプラグインを受け入れることもできます。

予測 IntelliSense を無効にするには、次のように実行します。

```powershell
Set-PSReadLineOption -PredictionSource None
```

クラス **[PSConsoleReadLine]** では、次の関数を使用できます。

## <a name="basic-editing-functions"></a>基本的な編集関数

### <a name="abort"></a>中止

現在のアクション (増分履歴検索など) を中止します。

- .Emacs `<Ctrl+g>`

### <a name="acceptandgetnext"></a>AcceptAndGetNext

現在の入力を実行しようとしました。 (AcceptLine など) 実行できる場合は、次に ReadLine が呼び出されたときに履歴から次の項目を再度呼び出します。

- .Emacs `<Ctrl+o>`

### <a name="acceptline"></a>AcceptLine

現在の入力を実行しようとしました。 現在の入力が不完全な場合 (たとえば、終わりかっこ、角かっこ、または引用符がない場合)、継続のプロンプトは次の行に表示され、PSReadLine は現在の入力を編集するためのキーを待機します。

- コマンド: `<Enter>`
- .Emacs `<Enter>`
- Vi の挿入モード: `<Enter>`

### <a name="addline"></a>AddLine

継続プロンプトが次の行に表示され、PSReadLine が現在の入力を編集するためのキーを待機します。 これは、単一行が単独で完全に入力されている場合でも、複数行の入力を1つのコマンドとして入力する場合に便利です。

- コマンド: `<Shift+Enter>`
- .Emacs `<Shift+Enter>`
- Vi の挿入モード: `<Shift+Enter>`
- Vi コマンドモード: `<Shift+Enter>`

### <a name="backwarddeletechar"></a>BackwardDeleteChar

カーソルの前の文字を削除します。

- Cmd: `<Backspace>` 、 `<Ctrl+h>`
- Emacs: `<Backspace>` 、 `<Ctrl+Backspace>` 、 `<Ctrl+h>`
- Vi の挿入モード: `<Backspace>`
- Vi コマンドモード: `<X>` 、 `<d,h>`

### <a name="backwarddeleteline"></a>BackwardDeleteLine

Like BackwardKillLine-行の先頭から先頭までのテキストを削除しますが、削除されたテキストはキルリングには挿入されません。

- コマンド: `<Ctrl+Home>`
- Vi 挿入モード: `<Ctrl+u>` 、 `<Ctrl+Home>`
- Vi コマンドモード: `<Ctrl+u>` 、 `<Ctrl+Home>` 、 `<d,0>`

### <a name="backwarddeleteword"></a>BackwardDeleteWord

前の単語を削除します。

- Vi コマンドモード: `<Ctrl+w>` 、 `<d,b>`

### <a name="backwardkillline"></a>BackwardKillLine

入力の先頭からカーソルまでの入力をクリアします。 クリアテキストはキルリングに配置されます。

- Emacs: `<Ctrl+u>` 、 `<Ctrl+x,Backspace>`

### <a name="backwardkillword"></a>BackwardKillWord

現在の単語の先頭からカーソルまでの入力をクリアします。 カーソルが単語の間にある場合は、前の単語の先頭からカーソルまでの入力がクリアされます。 クリアテキストはキルリングに配置されます。

- Cmd: `<Ctrl+Backspace>` 、 `<Ctrl+w>`
- Emacs: `<Alt+Backspace>` 、 `<Escape,Backspace>`
- Vi の挿入モード: `<Ctrl+Backspace>`
- Vi コマンドモード: `<Ctrl+Backspace>`

### <a name="cancelline"></a>CancelLine

入力を画面に残したまま、現在の入力を取り消しますが、プロンプトが再び評価されるように、ホストに戻ります。

- Vi の挿入モード: `<Ctrl+c>`
- Vi コマンドモード: `<Ctrl+c>`

### <a name="copy"></a>コピー

選択したリージョンをシステムクリップボードにコピーします。 領域が選択されていない場合は、行全体をコピーします。

- コマンド: `<Ctrl+C>`

### <a name="copyorcancelline"></a>CopyOrCancelLine

テキストが選択されている場合は、クリップボードにコピーします。それ以外の場合は、行をキャンセルします。

- コマンド: `<Ctrl+c>`
- .Emacs `<Ctrl+c>`

### <a name="cut"></a>［切り取り］

削除されたテキストをシステムクリップボードに配置した、選択した領域を削除します。

- コマンド: `<Ctrl+x>`

### <a name="deletechar"></a>Dele

カーソルの下の文字を削除します。

- コマンド: `<Delete>`
- .Emacs `<Delete>`
- Vi の挿入モード: `<Delete>`
- Vi コマンドモード: `<Delete>` 、 `<x>` 、 `<d,l>` 、 `<d,Spacebar>`

### <a name="deletecharorexit"></a>Deleの Arorexit

カーソルの下の文字を削除するか、行が空の場合はプロセスを終了します。

- .Emacs `<Ctrl+d>`

### <a name="deleteendofbuffer"></a>DeleteEndOfBuffer

複数行バッファーの末尾までを削除します。

- Vi コマンドモード: `<d,G>`

### <a name="deleteendofword"></a>DeleteEndOfWord

単語の末尾まで削除します。

- Vi コマンドモード: `<d,e>`

### <a name="deleteline"></a>Deleteline.invoke に

複数行バッファーの現在の論理行を削除して、元に戻すことができるようにします。

- Vi コマンドモード: `<d,d>` 、 `<d,_>`

### <a name="deletepreviouslines"></a>削除した行

複数行バッファーの前に要求された論理行と現在の論理行を削除します。

- Vi コマンドモード: `<d,k>`

### <a name="deleterelativelines"></a>DeleteRelativeLines

バッファーの先頭から複数行バッファーの現在の論理行に削除します。

ほとんどの Vi コマンドと同様に、コマンドには、 `<d,g,g>` 絶対行番号を指定する数値引数 (現在の行番号と共に、削除する行の範囲を構成する) を付加することができます。
指定しない場合、数値引数の既定値は1です。これは、複数行バッファーの最初の論理行を参照します。

複数行から削除される実際の行数は、現在の論理行番号と指定された数値引数の差として計算されます。したがって、負の数になる可能性があります。 そのため、メソッド名の _相対_ 部分です。

- Vi コマンドモード: `<d,g,g>`

### <a name="deletenextlines"></a>Deleの配信行

複数行バッファーで現在の論理行と次に要求された論理行を削除します。

- Vi コマンドモード: `<d,j>`

### <a name="deletelinetofirstchar"></a>DeleteLineToFirstChar

複数行バッファーの現在の論理行の最初の空白以外の文字からを削除します。

- Vi コマンドモード: `<d,^>`

### <a name="deletetoend"></a>DeleteToEnd

行の末尾まで削除します。

- Vi コマンドモード: `<D>` 、 `<d,$>`

### <a name="deleteword"></a>DeleteWord

次の単語を削除します。

- Vi コマンドモード: `<d,w>`

### <a name="forwarddeleteline"></a>ForwardDeleteLine

同じように、Forwardは、行の末尾から最後までのテキストを削除しますが、削除されたテキストはキルリングには挿入されません。

- コマンド: `<Ctrl+End>`
- Vi の挿入モード: `<Ctrl+End>`
- Vi コマンドモード: `<Ctrl+End>`

### <a name="insertlineabove"></a>InsertLineAbove

現在の行の上にカーソルがある場所に関係なく、現在の行の上に新しい空の行が作成されます。 カーソルが新しい行の先頭に移動します。

- コマンド: `<Ctrl+Enter>`

### <a name="insertlinebelow"></a>InsertLineBelow

カーソルが現在の行にある場所に関係なく、現在の行の下に新しい空の行が作成されます。 カーソルが新しい行の先頭に移動します。

- コマンド: `<Shift+Ctrl+Enter>`

### <a name="invertcase"></a>InvertCase

現在の文字の大文字と小文字の区別を反転し、次の文字に移動します。

- Vi コマンドモード: `<~>`

### <a name="killline"></a>行の行

カーソルから入力の末尾までの入力をクリアします。 クリアテキストはキルリングに配置されます。

- .Emacs `<Ctrl+k>`

### <a name="killregion"></a>"/" 領域

カーソルとマークの間のテキストを強制終了します。

- 関数はバインド解除されています。

### <a name="killword"></a>文字の候補

カーソルから現在の単語の末尾までの入力をクリアします。 カーソルが単語の間にある場合は、カーソルから次の単語の末尾まで、入力がクリアされます。 クリアテキストはキルリングに配置されます。

- Cmd: `<Alt+d>` 、 `<Ctrl+Delete>`
- Emacs: `<Alt+d>` 、 `<Escape,d>`
- Vi の挿入モード: `<Ctrl+Delete>`
- Vi コマンドモード: `<Ctrl+Delete>`

### <a name="paste"></a>貼り付け

システムクリップボードからテキストを貼り付けます。

- Cmd: `<Ctrl+v>` 、 `<Shift+Insert>`
- Vi の挿入モード: `<Ctrl+v>`
- Vi コマンドモード: `<Ctrl+v>`

> [!IMPORTANT]
> **貼り付け** 関数を使用すると、クリップボードバッファーの内容全体が psreadline の入力バッファーに貼り付けられます。 入力バッファーが PowerShell パーサーに渡されます。 コンソールアプリケーションの **右クリック** による貼り付けメソッドを使用して貼り付けた入力は、一度に1文字ずつ入力バッファーにコピーされます。 入力バッファーは、改行文字がコピーされるときにパーサーに渡されます。 そのため、入力は一度に1行ずつ解析されます。 貼り付けメソッドの違いによって、実行の動作が異なります。

### <a name="pasteafter"></a>PasteAfter

カーソルの後にクリップボードを貼り付けて、貼り付けられたテキストの末尾にカーソルを移動します。

- Vi コマンドモード: `<p>`

### <a name="pastebefore"></a>PasteBefore

カーソルの前にクリップボードを貼り付けて、貼り付けられたテキストの末尾にカーソルを移動します。

- Vi コマンドモード: `<P>`

### <a name="prependandaccept"></a>PrependAndAccept

' # ' を先頭に付加し、その行を受け入れます。

- Vi コマンドモード: `<#>`

### <a name="redo"></a>やり直す

元に戻す操作を元に戻します。

- コマンド: `<Ctrl+y>`
- Vi の挿入モード: `<Ctrl+y>`
- Vi コマンドモード: `<Ctrl+y>`

### <a name="repeatlastcommand"></a>RepeatLastCommand

最後のテキストの変更を繰り返します。

- Vi コマンドモード: `<.>`

### <a name="revertline"></a>再有効化

すべての入力を現在の入力に戻します。

- コマンド: `<Escape>`
- Emacs: `<Alt+r>` 、 `<Escape,r>`

### <a name="shellbackwardkillword"></a>ShellBackwardKillWord

現在の単語の先頭からカーソルまでの入力をクリアします。 カーソルが単語の間にある場合は、前の単語の先頭からカーソルまでの入力がクリアされます。 クリアテキストはキルリングに配置されます。

関数はバインド解除されています。

### <a name="shellkillword"></a>Shellは Word

カーソルから現在の単語の末尾までの入力をクリアします。 カーソルが単語の間にある場合は、カーソルから次の単語の末尾まで、入力がクリアされます。 クリアテキストはキルリングに配置されます。

関数はバインド解除されています。

### <a name="swapcharacters"></a>スワップ文字

現在の文字とそれより前の文字を交換します。

- .Emacs `<Ctrl+t>`
- Vi の挿入モード: `<Ctrl+t>`
- Vi コマンドモード: `<Ctrl+t>`

### <a name="undo"></a>元に戻す

前の編集を元に戻します。

- コマンド: `<Ctrl+z>`
- Emacs: `<Ctrl+_>` 、 `<Ctrl+x,Ctrl+u>`
- Vi の挿入モード: `<Ctrl+z>`
- Vi コマンドモード: `<Ctrl+z>` 、 `<u>`

### <a name="undoall"></a>UndoAll

行の以前の編集をすべて元に戻します。

- Vi コマンドモード: `<U>`

### <a name="unixwordrubout"></a>UnixWordRubout

現在の単語の先頭からカーソルまでの入力をクリアします。 カーソルが単語の間にある場合は、前の単語の先頭からカーソルまでの入力がクリアされます。 クリアテキストはキルリングに配置されます。

- .Emacs `<Ctrl+w>`

### <a name="validateandacceptline"></a>ValidateAndAcceptLine

現在の入力を実行しようとしました。 現在の入力が不完全な場合 (たとえば、終わりかっこ、角かっこ、または引用符がない場合)、継続のプロンプトは次の行に表示され、PSReadLine は現在の入力を編集するためのキーを待機します。

- .Emacs `<Ctrl+m>`

### <a name="viacceptline"></a>ViAcceptLine

行を受け入れ、挿入モードに切り替えます。

- Vi コマンドモード: `<Enter>`

### <a name="viacceptlineorexit"></a>ViAcceptLineOrExit

(Emacs モードでは Deleと同じですが、文字を削除するのではなく、行を受け取ります)。

- Vi の挿入モード: `<Ctrl+d>`
- Vi コマンドモード: `<Ctrl+d>`

### <a name="viappendline"></a>ViAppendLine

現在の行の下に新しい行が挿入されます。

- Vi コマンドモード: `<o>`

### <a name="vibackwarddeleteglob"></a>ViBackwardDeleteGlob

単語区切り記号として空白文字のみを使用して、前の単語を削除します。

- Vi コマンドモード: `<d,B>`

### <a name="vibackwardglob"></a>ViBackwardGlob

区切り記号として空白文字のみを使用して、カーソルを前の単語の先頭に戻します。

- Vi コマンドモード: `<B>`

### <a name="videletebrace"></a>ViDeleteBrace

対応する中かっこ、かっこ、または角かっこを検索し、中かっこを含め、内のすべての内容を削除します。

- Vi コマンドモード: `<d,%>`

### <a name="videleteendofglob"></a>ViDeleteEndOfGlob

単語の末尾まで削除します。

- Vi コマンドモード: `<d,E>`

### <a name="videleteglob"></a>ViDeleteGlob

次の glob (空白で区切られた単語) を削除します。

- Vi コマンドモード: `<d,W>`

### <a name="videletetobeforechar"></a>ViDeleteToBeforeChar

指定された文字まで削除します。

- Vi コマンドモード: `<d,t>`

### <a name="videletetobeforecharbackward"></a>Videletetobeforo

指定された文字まで削除します。

- Vi コマンドモード: `<d,T>`

### <a name="videletetochar"></a>ViDeleteToChar

指定された文字まで削除します。

- Vi コマンドモード: `<d,f>`

### <a name="videletetocharbackward"></a>ViDeleteToCharBackward

指定された文字まで後方を削除します。

- Vi コマンドモード: `<d,F>`

### <a name="viinsertatbegining"></a>ViInsertAtBegining

挿入モードに切り替え、カーソルを行の先頭に配置します。

- Vi コマンドモード: `<I>`

### <a name="viinsertatend"></a>ViInsertAtEnd

挿入モードに切り替え、カーソルを行の末尾に配置します。

- Vi コマンドモード: `<A>`

### <a name="viinsertline"></a>ViInsertLine

現在の行の上に新しい行が挿入されます。

- Vi コマンドモード: `<O>`

### <a name="viinsertwithappend"></a>ViInsertWithAppend

現在の行の位置から追加します。

- Vi コマンドモード: `<a>`

### <a name="viinsertwithdelete"></a>ViInsertWithDelete

現在の文字を削除し、挿入モードに切り替えます。

- Vi コマンドモード: `<s>`

### <a name="vijoinlines"></a>ViJoinLines

現在の行と次の行を結合します。

- Vi コマンドモード: `<J>`

### <a name="vireplaceline"></a>交換ライン

コマンドライン全体を消去します。

- Vi コマンドモード: `<S>` 、 `<c,c>`

### <a name="vireplacetobeforechar"></a>ViReplaceToBeforeChar

指定された文字になるまで置き換えます。

- Vi コマンドモード: `<c,t>`

### <a name="vireplacetobeforecharbackward"></a>Vireplacetobeforo

指定された文字になるまで置き換えます。

- Vi コマンドモード: `<c,T>`

### <a name="vireplacetochar"></a>ViReplaceToChar

指定された文字まで削除します。

- Vi コマンドモード: `<c,f>`

### <a name="vireplacetocharbackward"></a>ViReplaceToCharBackward

指定された文字になるまで置き換えます。

- Vi コマンドモード: `<c,F>`

### <a name="viyankbeginningofline"></a>ViYankBeginningOfLine

バッファーの先頭からカーソルまで Yank。

- Vi コマンドモード: `<y,0>`

### <a name="viyankendofglob"></a>ViYankEndOfGlob

カーソルから単語の末尾まで Yank します。

- Vi コマンドモード: `<y,E>`

### <a name="viyankendofword"></a>ViYankEndOfWord

カーソルから単語の末尾まで Yank します。

- Vi コマンドモード: `<y,e>`

### <a name="viyankleft"></a>ViYankLeft

カーソルの左側の文字を Yank します。

- Vi コマンドモード: `<y,h>`

### <a name="viyankline"></a>ViYankLine

バッファー全体を Yank します。

- Vi コマンドモード: `<y,y>`

### <a name="viyanknextglob"></a>ViYankNextGlob

Yank は、次の単語の先頭にカーソルを置きます。

- Vi コマンドモード: `<y,W>`

### <a name="viyanknextword"></a>ViYankNextWord

カーソルの後の単語を Yank します。

- Vi コマンドモード: `<y,w>`

### <a name="viyankpercent"></a>ViYankPercent

一致する中かっこを Yank します。

- Vi コマンドモード: `<y,%>`

### <a name="viyankpreviousglob"></a>ViYankPreviousGlob

Yank は、単語の先頭からカーソルになります。

- Vi コマンドモード: `<y,B>`

### <a name="viyankpreviousword"></a>ViYankPreviousWord

カーソルの前に単語を Yank します。

- Vi コマンドモード: `<y,b>`

### <a name="viyankright"></a>ViYankRight

カーソルの右側に Yank 文字があります。

- Vi コマンドモード: `<y,l>` 、 `<y,Spacebar>`

### <a name="viyanktoendofline"></a>ViYankToEndOfLine

カーソルからバッファーの末尾までの Yank。

- Vi コマンドモード: `<y,$>`

### <a name="viyanktofirstchar"></a>ViYankToFirstChar

最初の空白以外の文字からカーソルまで Yank ます。

- Vi コマンドモード: `<y,^>`

### <a name="yank"></a>Yank

最後に終了したテキストを入力に追加します。

- .Emacs `<Ctrl+y>`

### <a name="yanklastarg"></a>YankLastArg

前の履歴行の最後の引数を Yank します。 引数を使用すると、最初に呼び出されたときに、YankNthArg と同じように動作します。 複数回呼び出された場合は、代わりに履歴を反復処理し、引数に方向を設定します (負の方向が反転します)。

- コマンド: `<Alt+.>`
- Emacs: `<Alt+.>` 、 `<Alt+_>` 、 `<Escape,.>` 、 `<Escape,_>`

### <a name="yankntharg"></a>YankNthArg

Yank は、前の履歴行から (コマンドの後に) 最初の引数を指定します。
引数を使用して、n 番目の引数 (0 から始まる) を yank します。引数が負の場合は、最後の引数から開始します。

- Emacs: `<Ctrl+Alt+y>` 、 `<Escape,Ctrl+y>`

### <a name="yankpop"></a>YankPop

前の操作が Yank または YankPop であった場合は、以前の引き抜かテキストをキルリングから次に強制終了されたテキストに置き換えます。

- Emacs: `<Alt+y>` 、 `<Escape,y>`

## <a name="cursor-movement-functions"></a>カーソルの移動関数

### <a name="backwardchar"></a>BackwardChar

カーソルを1文字左に移動します。 これにより、カーソルが複数行の入力の前の行に移動する場合があります。

- コマンド: `<LeftArrow>`
- Emacs: `<LeftArrow>` 、 `<Ctrl+b>`

### <a name="backwardword"></a>BackwardWord

カーソルを現在の単語の先頭に戻すか、前の単語の先頭に移動します。 単語の境界は、構成可能な文字のセットによって定義されます。

- コマンド: `<Ctrl+LeftArrow>`
- Emacs: `<Alt+b>` 、 `<Escape,b>`
- Vi の挿入モード: `<Ctrl+LeftArrow>`
- Vi コマンドモード: `<Ctrl+LeftArrow>`

### <a name="beginningofline"></a>BeginningOfLine

入力に複数の行がある場合は、現在の行の先頭に移動します。または、既に行の先頭にある場合は、入力の先頭に移動します。 入力に1行しか含まれていない場合は、入力の先頭に移動します。

- コマンド: `<Home>`
- Emacs: `<Home>` 、 `<Ctrl+a>`
- Vi の挿入モード: `<Home>`
- Vi コマンドモード: `<Home>`

### <a name="endofline"></a>EndOfLine

入力に複数の行がある場合は、現在の行の末尾に移動します。または、既に行の末尾にある場合は、入力の末尾に移動します。 入力に1行しか含まれていない場合は、入力の末尾に移動します。

- コマンド: `<End>`
- Emacs: `<End>` 、 `<Ctrl+e>`
- Vi の挿入モード: `<End>`

### <a name="forwardchar"></a>ForwardChar

カーソルを1文字右に移動します。 これにより、カーソルが複数行入力の次の行に移動する可能性があります。

- コマンド: `<RightArrow>`
- Emacs: `<RightArrow>` 、 `<Ctrl+f>`

### <a name="forwardword"></a>ForwardWord

カーソルを現在の単語の末尾、または単語の間で次の単語の末尾まで移動します。 単語の境界は、構成可能な文字のセットによって定義されます。

- Emacs: `<Alt+f>` 、 `<Escape,f>`

### <a name="gotobrace"></a>GotoBrace

対応する中かっこ、かっこ、または角かっこにジャンプします。

- コマンド: `<Ctrl+]>`
- Vi の挿入モード: `<Ctrl+]>`
- Vi コマンドモード: `<Ctrl+]>`

### <a name="gotocolumn"></a>GotoColumn

Arg で示される列に移動します。

- Vi コマンドモード: `<|>`

### <a name="gotofirstnonblankofline"></a>GotoFirstNonBlankOfLine

カーソルを行の空白以外の最初の文字に移動します。

- Vi コマンドモード: `<^>` 、 `<_>`

### <a name="movetoendofline"></a>MoveToEndOfLine

カーソルを入力の末尾に移動します。

- Vi コマンドモード: `<End>` 、 `<$>`

### <a name="nextline"></a>NextLine

カーソルを次の行に移動します。

- 関数はバインド解除されています。

### <a name="nextword"></a>NextWord

次の単語の先頭にカーソルを移動します。 単語の境界は、構成可能な文字のセットによって定義されます。

- コマンド: `<Ctrl+RightArrow>`
- Vi の挿入モード: `<Ctrl+RightArrow>`
- Vi コマンドモード: `<Ctrl+RightArrow>`

### <a name="nextwordend"></a>NextWordEnd

カーソルを現在の単語の末尾、または単語の間で次の単語の末尾まで移動します。 単語の境界は、構成可能な文字のセットによって定義されます。

- Vi コマンドモード: `<e>`

### <a name="previousline"></a>前の行

カーソルを前の行に移動します。

- 関数はバインド解除されています。

### <a name="shellbackwardword"></a>ShellBackwardWord

カーソルを現在の単語の先頭に戻すか、前の単語の先頭に移動します。 単語の境界は、PowerShell トークンによって定義されます。

- 関数はバインド解除されています。

### <a name="shellforwardword"></a>ShellForwardWord

次の単語の先頭にカーソルを移動します。 単語の境界は、PowerShell トークンによって定義されます。

- 関数はバインド解除されています。

### <a name="shellnextword"></a>ShellNextWord

カーソルを現在の単語の末尾、または単語の間で次の単語の末尾まで移動します。 単語の境界は、PowerShell トークンによって定義されます。

- 関数はバインド解除されています。

### <a name="vibackwardchar"></a>ViBackwardChar

Vi 編集モードでカーソルを1文字左に移動します。 これにより、カーソルが複数行の入力の前の行に移動する場合があります。

- Vi の挿入モード: `<LeftArrow>`
- Vi コマンドモード: `<LeftArrow>` 、 `<Backspace>` 、 `<h>`

### <a name="vibackwardword"></a>ViBackwardWord

カーソルを現在の単語の先頭に戻すか、前の単語の先頭に移動します。 単語の境界は、構成可能な文字のセットによって定義されます。

- Vi コマンドモード: `<b>`

### <a name="viforwardchar"></a>ViForwardChar

Vi 編集モードでカーソルを1文字右に移動します。 これにより、カーソルが複数行入力の次の行に移動する可能性があります。

- Vi の挿入モード: `<RightArrow>`
- Vi コマンドモード: `<RightArrow>` 、 `<Spacebar>` 、 `<l>`

### <a name="viendofglob"></a>ViEndOfGlob

区切り記号として空白文字のみを使用して、カーソルを単語の末尾に移動します。

- Vi コマンドモード: `<E>`

### <a name="viendofpreviousglob"></a>Viendofの Glob

単語区切り記号として空白文字のみを使用して、前の単語の末尾に移動します。

- 関数はバインド解除されています。

### <a name="vigotobrace"></a>ViGotoBrace

GotoBrace に似ていますが、トークンベースではなく文字ベースです。

- Vi コマンドモード: `<%>`

### <a name="vinextglob"></a>ViNextGlob

単語区切り記号として空白文字のみを使用して、次の単語に移動します。

- Vi コマンドモード: `<W>`

### <a name="vinextword"></a>ViNextWord

次の単語の先頭にカーソルを移動します。 単語の境界は、構成可能な文字のセットによって定義されます。

- Vi コマンドモード: `<w>`

## <a name="history-functions"></a>履歴関数

### <a name="beginningofhistory"></a>BeginningOfHistory

履歴内の最初の項目に移動します。

- .Emacs `<Alt+<>`

### <a name="clearhistory"></a>ClearHistory

PSReadLine の履歴をクリアします。 これは、PowerShell の履歴には影響しません。

- コマンド: `<Alt+F7>`

### <a name="endofhistory"></a>EndOfHistory

履歴内の最後の項目 (現在の入力) に移動します。

- .Emacs `<Alt+>>`

### <a name="forwardsearchhistory"></a>ForwardSearchHistory

履歴を使用した増分転送検索を実行します。

- コマンド: `<Ctrl+s>`
- .Emacs `<Ctrl+s>`

### <a name="historysearchbackward"></a>履歴の Search後方

現在の入力を、PSReadLine 履歴の ' previous ' 項目で置き換えます。この項目は、開始と入力の間の文字とカーソルの間の文字に一致します。

- コマンド: `<F8>`

### <a name="historysearchforward"></a>履歴の Searchforward

現在の入力を、PSReadLine 履歴の ' next ' 項目で置き換えます。この項目は、開始と入力の間の文字とカーソルの間の文字に一致します。

- コマンド: `<Shift+F8>`

### <a name="nexthistory"></a>NextHistory

現在の入力を、PSReadLine 履歴の ' next ' 項目に置き換えます。

- コマンド: `<DownArrow>`
- Emacs: `<DownArrow>` 、 `<Ctrl+n>`
- Vi の挿入モード: `<DownArrow>`
- Vi コマンドモード: `<DownArrow>` 、 `<j>` 、 `<+>`

### <a name="previoushistory"></a>PreviousHistory

現在の入力を、PSReadLine 履歴の ' previous ' 項目に置き換えます。

- コマンド: `<UpArrow>`
- Emacs: `<UpArrow>` 、 `<Ctrl+p>`
- Vi の挿入モード: `<UpArrow>`
- Vi コマンドモード: `<UpArrow>` 、 `<k>` 、 `<->`

### <a name="reversesearchhistory"></a>ReverseSearchHistory

履歴を介して後方検索を段階的に実行します。

- コマンド: `<Ctrl+r>`
- .Emacs `<Ctrl+r>`

### <a name="visearchhistorybackward"></a>Visearchhistory 後方

検索文字列を入力し、AcceptLine で検索を開始します。

- Vi の挿入モード: `<Ctrl+r>`
- Vi コマンドモード: `</>` 、 `<Ctrl+r>`

## <a name="completion-functions"></a>入力候補関数

### <a name="complete"></a>完了

カーソルを囲むテキストに対して入力候補を実行しようとしました。 候補が複数ある場合は、最長の明確なプレフィックスを使用して完了します。 最長の明確な完了を完了しようとすると、候補の一覧が表示されます。

- .Emacs `<Tab>`

### <a name="menucomplete"></a>MenuComplete

カーソルを囲むテキストに対して入力候補を実行しようとしました。 候補が複数ある場合は、最長の明確なプレフィックスを使用して完了します。 最長の明確な完了を完了しようとすると、候補の一覧が表示されます。

- Cmd: `<Ctrl+@>` 、 `<Ctrl+Spacebar>`
- .Emacs `<Ctrl+Spacebar>`

### <a name="possiblecompletions"></a>PossibleCompletions

候補の候補の一覧を表示します。

- .Emacs `<Alt+=>`
- Vi の挿入モード: `<Ctrl+Spacebar>`
- Vi コマンドモード: `<Ctrl+Spacebar>`

### <a name="tabcompletenext"></a>タブのすべてのタブ

次に使用可能な完了時に、カーソルを囲むテキストを完了します。

- コマンド: `<Tab>`
- Vi コマンドモード: `<Tab>`

### <a name="tabcompleteprevious"></a>TabCompletePrevious

以前に使用可能な入力候補を使用して、カーソルを囲むテキストを完了しようとしました。

- コマンド: `<Shift+Tab>`
- Vi コマンドモード: `<Shift+Tab>`

### <a name="vitabcompletenext"></a>ViTabCompleteNext

必要に応じて現在の編集グループを終了し、Tabends を呼び出します。

- Vi の挿入モード: `<Tab>`

### <a name="vitabcompleteprevious"></a>ViTabCompletePrevious

必要に応じて現在の編集グループを終了し、TabCompletePrevious を呼び出します。

- Vi の挿入モード: `<Shift+Tab>`

## <a name="miscellaneous-functions"></a>その他の関数

### <a name="acceptnextsuggestionword"></a>AcceptNextSuggestionWord

インラインまたは選択した修正候補の次の単語をそのまま使用します。

- 関数はバインド解除されています。

### <a name="acceptsuggestion"></a>AcceptSuggestion

現在のインラインまたは選択された提案を受け入れます。

- 関数はバインド解除されています。

### <a name="capturescreen"></a>CaptureScreen

対話型画面のキャプチャを開始する/下矢印行を選択し、[選択したテキストをテキストと html としてクリップボードにコピーする] をオンにします。

- 関数はバインド解除されています。

### <a name="clearscreen"></a>ClearScreen

画面をクリアし、画面の上部にある現在の線を描画します。

- コマンド: `<Ctrl+l>`
- .Emacs `<Ctrl+l>`
- Vi の挿入モード: `<Ctrl+l>`
- Vi コマンドモード: `<Ctrl+l>`

### <a name="digitargument"></a>DigitArgument

他の関数に渡す新しい桁引数を開始します。

- Cmd:、、、、、、、、、 `<Alt+0>` `<Alt+1>` `<Alt+2>` `<Alt+3>` `<Alt+4>` `<Alt+5>` `<Alt+6>` `<Alt+7>` `<Alt+8>` `<Alt+9>` 、 `<Alt+->`
- Emacs:、、、、、、、、、 `<Alt+0>` `<Alt+1>` `<Alt+2>` `<Alt+3>` `<Alt+4>` `<Alt+5>` `<Alt+6>` `<Alt+7>` `<Alt+8>` `<Alt+9>` 、 `<Alt+->`
- Vi コマンドモード:、、、、、、、、 `<0>` `<1>` `<2>` `<3>` `<4>` `<5>` `<6>` `<7>` `<8>` 、 `<9>`

### <a name="invokeprompt"></a>InvokePrompt

現在のプロンプトを消去し、prompt 関数を呼び出してプロンプトを再表示します。 現在のディレクトリを変更するなど、状態を変更するカスタムキーハンドラーに便利です。

- 関数はバインド解除されています。

### <a name="scrolldisplaydown"></a>ScrollDisplayDown

画面を1画面下へスクロールします。

- コマンド: `<PageDown>`
- .Emacs `<PageDown>`

### <a name="scrolldisplaydownline"></a>ScrollDisplayDownLine

表示を1行下にスクロールします。

- コマンド: `<Ctrl+PageDown>`
- .Emacs `<Ctrl+PageDown>`

### <a name="scrolldisplaytocursor"></a>ScrollDisplayToCursor

カーソルまで画面をスクロールします。

- .Emacs `<Ctrl+End>`

### <a name="scrolldisplaytop"></a>ScrollDisplayTop

画面を上にスクロールします。

- .Emacs `<Ctrl+Home>`

### <a name="scrolldisplayup"></a>ScrollDisplayUp

画面を1画面上へスクロールします。

- コマンド: `<PageUp>`
- .Emacs `<PageUp>`

### <a name="scrolldisplayupline"></a>ScrollDisplayUpLine

表示を1行上へスクロールします。

- コマンド: `<Ctrl+PageUp>`
- .Emacs `<Ctrl+PageUp>`

### <a name="selfinsert"></a>SelfInsert

キーを挿入します。

- 関数はバインド解除されています。

### <a name="showkeybindings"></a>ShowKeyBindings

すべてのバインドされたキーを表示します。

- コマンド: `<Ctrl+Alt+?>`
- .Emacs `<Ctrl+Alt+?>`
- Vi の挿入モード: `<Ctrl+Alt+?>`

### <a name="vicommandmode"></a>ViCommandMode

現在の動作モードを Vi-Insert から Vi-Command に切り替えます。

- Vi の挿入モード: `<Escape>`

### <a name="vidigitargumentinchord"></a>ViDigitArgumentInChord

Vi のいずれかの弦で、他の関数に渡す新しい桁引数を開始します。

- 関数はバインド解除されています。

### <a name="vieditvisually"></a>ViEditVisually

$Env: EDITOR または $env: ビジュアルによって指定されたテキストエディターでコマンドラインを編集します。

- .Emacs `<Ctrl+x,Ctrl+e>`
- Vi コマンドモード: `<v>`

### <a name="viexit"></a>ViExit

シェルを終了します。

- 関数はバインド解除されています。

### <a name="viinsertmode"></a>ViInsertMode

挿入モードに切り替えます。

- Vi コマンドモード: `<i>`

### <a name="whatiskey"></a>WhatIsKey

キーを読み取り、キーがどのようにバインドされているかを通知します。

- コマンド: `<Alt+?>`
- .Emacs `<Alt+?>`

## <a name="selection-functions"></a>選択関数

### <a name="exchangepointandmark"></a>ExchangePointAndMark

カーソルがマークの位置に置かれ、マークがカーソルの位置に移動します。

- .Emacs `<Ctrl+x,Ctrl+x>`

### <a name="selectall"></a>SelectAll

行全体を選択します。

- コマンド: `<Ctrl+a>`

### <a name="selectbackwardchar"></a>SelectBackwardChar

現在の選択範囲を調整して、前の文字を含めます。

- コマンド: `<Shift+LeftArrow>`
- .Emacs `<Shift+LeftArrow>`

### <a name="selectbackwardsline"></a>SelectBackwardsLine

カーソルから行の先頭まで、現在の選択範囲を調整します。

- コマンド: `<Shift+Home>`
- .Emacs `<Shift+Home>`

### <a name="selectbackwardword"></a>SelectBackwardWord

現在の選択範囲を調整して、前の単語を含めます。

- コマンド: `<Shift+Ctrl+LeftArrow>`
- .Emacs `<Alt+B>`

### <a name="selectforwardchar"></a>SelectForwardChar

現在の選択範囲を調整して、次の文字を含めます。

- コマンド: `<Shift+RightArrow>`
- .Emacs `<Shift+RightArrow>`

### <a name="selectforwardword"></a>SelectForwardWord

現在の選択範囲を調整して、ForwardWord を使用して次の単語を含めます。

- .Emacs `<Alt+F>`

### <a name="selectline"></a>SelectLine

カーソルから行の末尾まで、現在の選択範囲を調整します。

- コマンド: `<Shift+End>`
- .Emacs `<Shift+End>`

### <a name="selectnextword"></a>SelectNextWord

現在の選択範囲を調整して、次の単語を含めます。

- コマンド: `<Shift+Ctrl+RightArrow>`

### <a name="selectshellbackwardword"></a>SelectShellBackwardWord

ShellBackwardWord を使用して、前の単語が含まれるように現在の選択範囲を調整します。

- 関数はバインド解除されています。

### <a name="selectshellforwardword"></a>SelectShellForwardWord

現在の選択範囲を調整して、ShellForwardWord を使用する次の単語を含めます。

- 関数はバインド解除されています。

### <a name="selectshellnextword"></a>SelectShellNextWord

現在の選択範囲を調整して、ShellNextWord を使用して次の単語を含めます。

- 関数はバインド解除されています。

### <a name="setmark"></a>SetMark

後続の編集コマンドで使用するために、カーソルの現在の位置をマークします。

- .Emacs `<Ctrl+@>`

## <a name="predictive-intellisense-functions"></a>予測 IntelliSense 関数

> [!NOTE]
> これらの関数を使用するには、予測 IntelliSense を有効にする必要があります。

### <a name="acceptnextwordsuggestion"></a>AcceptNextWordSuggestion

予測 IntelliSense からインライン提案の次の単語を受け取ります。
次のコマンドを実行して、この関数を<kbd>Ctrl</kbd> + <kbd>F</kbd>でバインドできます。

```powershell
Set-PSReadLineKeyHandler -Chord "Ctrl+f" -Function ForwardWord
```

### <a name="acceptsuggestion"></a>AcceptSuggestion

カーソルが現在の行の末尾にある場合に <kbd>→</kbd> を押すことで、予測 IntelliSense からの現在のインライン提案を受け入れます。

## <a name="search-functions"></a>関数の検索

### <a name="charactersearch"></a>文字検索

文字を読み取り、その文字が次に出現するまで検索します。
引数が指定されている場合は、n 番目のオカレンスに対して前方 (負の場合は後方) を検索します。

- コマンド: `<F3>`
- .Emacs `<Ctrl+]>`
- Vi の挿入モード: `<F3>`
- Vi コマンドモード: `<F3>`

### <a name="charactersearchbackward"></a>文字 Search後方

文字を読み取り、その文字が次に出現するまで検索します。 引数が指定されている場合は、n 番目のオカレンスに対して後方 (または負の場合は forward) を検索します。

- コマンド: `<Shift+F3>`
- .Emacs `<Ctrl+Alt+]>`
- Vi の挿入モード: `<Shift+F3>`
- Vi コマンドモード: `<Shift+F3>`

### <a name="repeatlastcharsearch"></a>RepeatLastCharSearch

最後に記録された文字の検索を繰り返します。

- Vi コマンドモード: `<;>`

### <a name="repeatlastcharsearchbackwards"></a>RepeatLastCharSearchBackwards

最後に記録された文字検索を逆方向に繰り返します。

- Vi コマンドモード: `<,>`

### <a name="repeatsearch"></a>RepeatSearch

前と同じ方向に最後の検索を繰り返します。

- Vi コマンドモード: `<n>`

### <a name="repeatsearchbackward"></a>RepeatSearchBackward

前と同じ方向に最後の検索を繰り返します。

- Vi コマンドモード: `<N>`

### <a name="searchchar"></a>SearchChar

次の文字を読み取って検索し、次に文字を検索します。 これは、' ' の機能のためのものです。

- Vi コマンドモード: `<f>`

### <a name="searchcharbackward"></a>SearchCharBackward

次の文字を読み取り、それを検索して後方に移動し、次に文字を戻します。 これは、' ' の機能のためのものです。

- Vi コマンドモード: `<F>`

### <a name="searchcharbackwardwithbackoff"></a>SearchCharBackwardWithBackoff

次の文字を読み取り、それを検索して後方に移動し、次に文字を戻します。 これは、' ' の機能のためのものです。

- Vi コマンドモード: `<T>`

### <a name="searchcharwithbackoff"></a>SearchCharWithBackoff オフ

次の文字を読み取って検索し、次に文字を検索します。 これは、' ' の機能のためのものです。

- Vi コマンドモード: `<t>`

### <a name="searchforward"></a>SearchForward

検索文字列を入力し、AcceptLine で検索を開始します。

- Vi の挿入モード: `<Ctrl+s>`
- Vi コマンドモード: `<?>` 、 `<Ctrl+s>`

## <a name="custom-key-bindings"></a>カスタムキーバインド

PSReadLine は、コマンドレットを使用してカスタムキーバインドをサポートしてい `Set-PSReadLineKeyHandler` ます。 ほとんどのカスタムキーバインドは、たとえば、上記の関数の1つを呼び出します。

```powershell
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
```

ScriptBlock をキーにバインドできます。 ScriptBlock は、ほとんどすべてのことを行うことができます。 いくつかの便利な例を次に示します。

- コマンドラインを編集する
- 新しいウィンドウを開く (例: ヘルプ)
- コマンドラインを変更せずにディレクトリを変更する

ScriptBlock は、次の2つの引数を受け取ります。

- `$key` -カスタムバインドをトリガーしたキーである **[Consolekeyinfo]** オブジェクト。 同じ ScriptBlock を複数のキーにバインドし、キーに応じて異なるアクションを実行する必要がある場合は、$key を確認します。 多くのカスタムバインドでは、この引数は無視します。

- `$arg` -任意の引数。 多くの場合、これは、ユーザーがキーバインド DigitArgument から渡す整数引数になります。 バインディングが引数を受け入れない場合は、この引数を無視するのが妥当です。

ここでは、コマンドラインを実行せずに履歴に追加する例を見てみましょう。 これは、何らかの操作を忘れた場合に、既に入力したコマンドラインを再入力したくない場合に便利です。

```powershell
$parameters = @{
    Key = 'Alt+w'
    BriefDescription = 'SaveInHistory'
    LongDescription = 'Save current line in history but do not execute'
    ScriptBlock = {
      param($key, $arg)   # The arguments are ignored in this example

      # GetBufferState gives us the command line (with the cursor position)
      $line = $null
      $cursor = $null
      [Microsoft.PowerShell.PSConsoleReadLine]::GetBufferState([ref]$line,
        [ref]$cursor)

      # AddToHistory saves the line in history, but does not execute it.
      [Microsoft.PowerShell.PSConsoleReadLine]::AddToHistory($line)

      # RevertLine is like pressing Escape.
      [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
  }
}
Set-PSReadLineKeyHandler @parameters
```

PSReadLine モジュールフォルダーにインストールされているファイルには、さらに多くの例が表示 `SamplePSReadLineProfile.ps1` されます。

ほとんどのキーバインドでは、コマンドラインを編集するためにいくつかのヘルパー関数を使用します。
これらの Api については、次のセクションで説明します。

## <a name="custom-key-binding-support-apis"></a>カスタムキーバインディングサポート Api

次の関数は PSConsoleReadLine では公開されていますが、キーに直接バインドすることはできません。 多くの場合、カスタムキーバインドに役立ちます。

```csharp
void AddToHistory(string command)
```

コマンドラインを実行せずに、履歴に追加します。

```csharp
void ClearKillRing()
```

Kill リングをクリアします。  これは主にテストに使用されます。

```csharp
void Delete(int start, int length)
```

先頭から長さの文字を削除します。  この操作では、元に戻す/やり直しがサポートされています。

```csharp
void Ding()
```

ユーザー設定に基づいてアクションを実行します。

```csharp
void GetBufferState([ref] string input, [ref] int cursor)
void GetBufferState([ref] Ast ast, [ref] Token[] tokens,
  [ref] ParseError[] parseErrors, [ref] int cursor)
```

これらの2つの関数は、入力バッファーの現在の状態に関する有用な情報を取得します。  1つ目は、単純なケースでよく使用されます。  2つ目は、バインディングが Ast を使用してより高度な処理を実行している場合に使用されます。

```csharp
IEnumerable[Microsoft.PowerShell.KeyHandler]
  GetKeyHandlers(bool includeBound, bool includeUnbound)

IEnumerable[Microsoft.PowerShell.KeyHandler]
  GetKeyHandlers(string[] Chord)
```

この2つの関数は、によって使用され `Get-PSReadLineKeyHandler` ます。 最初のは、すべてのキーバインドを取得するために使用されます。 2番目のは、特定のキーバインドを取得するために使用されます。

```csharp
Microsoft.PowerShell.PSConsoleReadLineOptions GetOptions()
```

この関数は Get-PSReadLineOption によって使用され、カスタムキーバインドではあまり役に立たない可能性があります。

```csharp
void GetSelectionState([ref] int start, [ref] int length)
```

コマンドラインに何も選択されていない場合は、開始と長さの両方で-1 が返されます。 コマンドラインに選択項目がある場合は、選択範囲の先頭と長さが返されます。

```csharp
void Insert(char c)
void Insert(string s)
```

カーソル位置に文字または文字列を挿入します。  この操作では、元に戻す/やり直しがサポートされています。

```csharp
string ReadLine(runspace remoteRunspace,
  System.Management.Automation.EngineIntrinsics engineIntrinsics)
```

これは、PSReadLine へのメインエントリポイントです。 再帰はサポートされないため、カスタムキーバインドでは役に立ちません。

```csharp
void RemoveKeyHandler(string[] key)
```

この関数は Remove-PSReadLineKeyHandler によって使用され、カスタムキーバインドではあまり役に立たない可能性があります。

```csharp
void Replace(int start, int length, string replacement)
```

入力の一部を置換します。 この操作では、元に戻す/やり直しがサポートされています。 これは、undo の単一のアクションとして扱われるため、Delete の後に Insert を使用することをお勧めします。

```csharp
void SetCursorPosition(int cursor)
```

カーソルを指定されたオフセットに移動します。 カーソルの移動は、元に戻すために追跡されません。

```csharp
void SetOptions(Microsoft.PowerShell.SetPSReadLineOption options)
```

この関数は、コマンドレットの設定-PSReadLineOption によって使用されるヘルパーメソッドですが、一時的に設定を変更する必要があるカスタムキーバインドに便利な場合があります。

```csharp
bool TryGetArgAsInt(System.Object arg, [ref] int numericArg,
  int defaultNumericArg)
```

このヘルパーメソッドは、DigitArgument を優先するカスタムバインドに使用されます。 一般的な呼び出しは次のようになります。

```powershell
[int]$numericArg = 0
[Microsoft.PowerShell.PSConsoleReadLine]::TryGetArgAsInt($arg,
  [ref]$numericArg, 1)
```

## <a name="notes"></a>メモ

### <a name="command-history"></a>コマンド履歴

PSReadLine は、コマンドラインから入力したすべてのコマンドとデータを含む履歴ファイルを保持します。 これには、パスワードなどの機密データが含まれる場合があります。 たとえば、コマンドレットを使用した場合、 `ConvertTo-SecureString` パスワードはプレーンテキストとして履歴ファイルに記録されます。 履歴ファイルは、という名前のファイルです `$($host.Name)_history.txt` 。 Windows システムでは、履歴ファイルはに格納され `$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine` ます。 Windows 以外のシステムでは、履歴ファイルはまたはに格納され `$env:XDG_DATA_HOME/powershell/PSReadLine` `$env:HOME/.local/share/powershell/PSReadLine` ます。

### <a name="feedback--contributing-to-psreadline"></a>PSReadLine に貢献しているフィードバック &

[GitHub の PSReadLine](https://github.com/PowerShell/PSReadLine)

プル要求を送信したり、GitHub ページでフィードバックを送信したりしてもかまいません。

## <a name="see-also"></a>参照

PSReadLine は、GNU [readline](https://tiswww.case.edu/php/chet/readline/rltop.html) ライブラリの影響を強く受けます。
