---
ms.date: 09/06/2019
keywords: powershell,コマンドレット
title: PowerShell 5.0 ISE の新機能
description: この記事では、Windows PowerShell Integrated Scripting Environment (ISE) バージョン 5.0 に導入された新機能と更新された機能について説明します。
ms.openlocfilehash: 75d37d0dafe381c84898ac48343336cd525d2dd1
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92660832"
---
# <a name="whats-new-in-the-windows-powershell-50-ise"></a>Windows PowerShell 5.0 ISE の新機能

この記事では、Windows PowerShell Integrated Scripting Environment (ISE) バージョン 5.0 に導入された新機能と更新された機能について説明します。

> [!NOTE]
> PowerShell ISE は、アクティブな機能開発の対象ではなくなりました。 Windows に付属するコンポーネントとして、セキュリティや優先順位の高いサービスに関する修正プログラムが引き続き公式でサポートされます。
> 現在、Windows から ISE が削除される予定はありません。
>
> PowerShell v6 以降では、ISE はサポートされていません。 ISE の代替をお探しのユーザーは、[PowerShell 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)と共に [Visual Studio Code](https://code.visualstudio.com/) を使用することをお勧めします。

## <a name="feature-description"></a>機能の説明

Windows PowerShell ISE は、グラフィカルで直感的な環境でスクリプトおよびモジュールを記述、実行、テストすることができるホスト アプリケーションです。 構文の色分け、タブ補完、視覚的なデバッグ機能、Unicode 準拠、状況依存のヘルプなどの主要な機能により、多彩なスクリプトの操作性が提供されます。

詳細については、「[Windows PowerShell ISE の紹介](../ise/Introducing-the-Windows-PowerShell-ISE.md)」を参照してください。

次の表に、Windows PowerShell における Windows PowerShell ISE のこのリリースの新機能と変更された機能を示します。

## <a name="intellisense"></a>IntelliSense

> ISE 3.0 に追加

IntelliSense は、Windows PowerShell ISE の一部である自動補完アシスタンス機能です。
IntelliSense により、文字を入力していくと、一致する可能性のあるコマンドレット、パラメーター、パラメーター値、ファイル、またはフォルダーのクリック可能なメニューが表示されます。

**この変更の利点**

IntelliSense の追加により、Windows PowerShell ISE を使用してスクリプトを作成する際に、コマンドレットと構文をより簡単に見つけられるようになりました。 また新しいスクリプトの作成中に Windows PowerShell ISE を使用して Windows PowerShell を理解することもできます。

**動作の相違点**

Windows PowerShell ISE でコマンドレットを入力すると、スクロールとクリックが可能なメニューが表示され、適切なコマンドを参照して選択できます。

## <a name="snippets"></a>スニペット

> ISE 3.0 に追加

*スニペット* は、Windows PowerShell ISE で作成するスクリプトに挿入できる Windows PowerShell コードの短いセクションです。 Windows PowerShell ISE にはスニペットの既定のセットが付属しています。 Windows PowerShell ISE での作業中に、`New-Snippet` コマンドレットを使用してスニペットを追加できます。

**この変更の利点**

スニペットを使用すると、スクリプトをすばやくアセンブルして作成し、環境を自動化できます。

**動作の相違点**

Windows PowerShell 3.0 以降でスニペットを使用するには、 **[編集]** メニューで **[スニペットの開始]** をクリックするか、<kbd>Ctrl</kbd>+<kbd>J</kbd> キーを押します。

## <a name="add-on-tools"></a>アドオン ツール

> PowerShell 3.0 に追加

Windows PowerShell ISE で、オブジェクト モデルを使用するアドオン ツールがサポートされるようになりました。 これらのアドオンは、コンソールに垂直方向または水平方向のウィンドウとして表示される Windows Presentation Foundation (WPF) コントロールです。 1 つのウィンドウ内に複数のアドオン ツールがある場合は、タブ形式のコントロールとして表示されます。 また、サード パーティ製のアドオン ツールも追加または削除できます。 詳細については、「[Windows PowerShell ISE スクリプト オブジェクト モデルの目的](../ise/object-model/Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)」を参照してください。

**この変更の利点**

アドオンにより、機能の追加やスクリプト作成エクスペリエンスの向上を実現するツールを使って、Windows PowerShell ISE を拡張およびカスタマイズできます。

**動作の相違点**

Windows PowerShell ISE 3.0 以降には、 **Commands** アドオンが付属しています。 **Commands** アドオンを使用すると、コマンドレットを参照し、 **[スクリプト]** ウィンドウと **[コンソール]** ウィンドウで同時にコマンドレットに関するヘルプにアクセスできます。

追加のアドオンは、 **[アドオン]** メニューの **[アドオン ツール Web サイトを開く]** コマンドを使用して検索することができます。

## <a name="restart-manager-and-auto-save"></a>再起動マネージャーと自動保存

> PowerShell 3.0 に追加

Windows PowerShell ISE では、開いているスクリプトを 2 分おきに別の場所に自動的に保存するようになりました。 予期しないクラッシュまたはリブート後に Windows PowerShell ISE が再起動すると、最後のセッションで開いていたスクリプトが (そのスクリプトが保存されていなかった場合でも) 回復されます。

自動保存の間隔を変更するには、コンソール ウィンドウで `$psise.Options.AutoSaveMinuteInterval` コマンドを実行します。

**この変更の利点**

Windows PowerShell ISE 内での作業で、開いているスクリプトが自動的に保存されるようになりました。

**動作の相違点**

Windows PowerShell ISE 2.0 では、スクリプトは自動的に保存されません。

## <a name="most-recently-used-list"></a>最近使用した一覧

> PowerShell 3.0 に追加

Windows PowerShell ISE にファイルの最近使用した一覧が用意されるようになりました。 Windows PowerShell ISE でファイルを開くと、ファイルは **[ファイル]** メニューの最近使用した一覧に追加されます。

最近使用した一覧に含まれるファイル数の規定値を変更するには、[コンソール] ウィンドウで、`$psise.Options.MruCount` コマンドを実行します。

**この変更の利点**

最近使用した一覧を使用して、頻繁に使用されるファイルに簡単にアクセスできるようになりました。

**動作の相違点**

Windows PowerShell ISE 2.0 には、最近使用した一覧は用意されていません。

## <a name="console-pane"></a>コンソール ウィンドウ

> PowerShell 3.0 に追加

Windows PowerShell ISE の最初のリリースで使用できた、独立したコマンド ウィンドウと出力ウィンドウが 1 つのコンソール ウィンドウに統合されました。 コンソール ウィンドウの機能と外観は、標準的な Windows PowerShell コンソールと類似していますが、次のような機能強化が行われています。

- XML 構文を含む入力テキストの構文の色分け (出力テキストを除く)
- IntelliSense
- かっこの一致
- エラー表示
- 全角 Unicode のサポート
- <kbd>F1</kbd> キーによる状況依存のヘルプ
- <kbd>Ctrl</kbd>+<kbd>F1</kbd> キーによる状況依存の Show-Command
- 複雑なスクリプトおよび右から左への記述のサポート
- フォントのサポート
- Zoom
- 行選択モードとブロック選択モード
- <kbd>上方向</kbd>キーを押して履歴をコンソールに表示したときにコマンド ラインに入力されたコンテンツを保存する機能

**この変更の利点**

これらのコンソール ウィンドウの変更を追加したことにより、コンソール インターフェイスとの一貫性がより向上したスクリプトの操作性が提供されます。

**動作の相違点**

Windows PowerShell ISE 2.0 では、コマンド ウィンドウと出力ウィンドウは個別に表示されます。

## <a name="command-line-switches"></a>コマンド ライン スイッチ

> PowerShell 3.0 に追加

コマンド ライン (「 **Powershell_ise.exe** 」を入力) から Windows PowerShell ISE を起動する場合、次の新しいコマンド ライン スイッチを追加できます。

- `-NoProfile`:`$profile` を実行せずに Windows PowerShell ISE を起動する
- `-Help`:ヘルプ ウィンドウを表示する
- `-mta`:マルチスレッド アパートメント モードで Windows PowerShell ISE を起動する。 Windows PowerShell ISE の既定の操作モードは、シングル スレッド アパートメント モード、つまり `-sta` です。

**この変更の利点**

これらのコマンド ライン スイッチを追加すると、Windows PowerShell ISE が動作する環境を制御することができます。

**動作の相違点**

Windows PowerShell ISE 2.0 では、これらのコマンド ライン スイッチは認識されません。

## <a name="new-editor-features"></a>エディターの新機能

> PowerShell 3.0 に追加

その他の Windows PowerShell ISE 編集機能として、次のものがあります。

- **XML 構文の色分け** - Windows PowerShell ISE では、Windows PowerShell 構文を色分けする方法と同じ方法で XML 構文が色分けされるようになりました。
- **かっこの照合** - Windows PowerShell ISE にはかっこの照合と強調表示を行う機能が含まれ、次のように使用できます。たとえば、始めかっこを選択している場合、 **[一致する項目に移動]** コマンドまたは <kbd>Ctrl</kbd>+<kbd>]</kbd> キーを使用すると、終わりかっこが見つかります。
- **アウトライン表示** 。スクリプト ウィンドウでは、アウトラインがサポートされます。これにより、左余白のプラス記号またはマイナス記号をクリックすると、コードのセクションを折りたたんだり展開したりできます。 かっこ、または `#region` タグと `#endregion` タグを使用して、折りたたみ可能なセクションの先頭と末尾をマークすることができます。 すべての領域を展開する、または折りたたむには、<kbd>Ctrl</kbd>+<kbd>M</kbd> キーを押します。
- **ドラッグ アンド ドロップ テキスト編集** - Windows PowerShell ISE で、ドラッグ アンド ドロップによるテキスト編集がサポートされるようになりました。 任意のテキストのブロックを選択し、そのテキストをドラッグしてエディターまたはコンソール内の別の場所に移動できます。 <kbd>Ctrl</kbd> キーを押したまま選択したテキストをドラッグし、マウス ボタンを放すと、テキストが新しい場所にコピーされます。 このバージョンの Windows PowerShell ISE では、ファイルを Windows PowerShell ISE 上にドラッグ アンド ドロップすると、Windows PowerShell ISE でファイルが開かれます。
- **解析エラーの表示** - 解析エラーは赤い下線で示されます。 示されたエラーをポイントすると、コード内で見つかった問題がヒントのテキストに表示されます。
- **ズーム** - ズーム スライダー (Windows PowerShell ISE ウィンドウの右下隅にあります) を使用するか、コンソール ウィンドウで `$psise.options.Zoom` コマンドを入力することで、コンソールのコンテンツのズーム倍率を設定できます。
- **リッチ テキストのコピーと貼り付け** - Windows PowerShell ISE でクリップボードにコピーすると、元の選択範囲のフォント、サイズ、および色の情報が保存されます。
- **ブロック選択** - テキストのブロックを選択するには、 <kbd>Alt</kbd> キーを押したままマウスでスクリプト ウィンドウ内のテキストを選択するか、 <kbd>Alt</kbd>+<kbd>Shift</kbd>+<kbd>方向</kbd>キーを押します。

**この変更の利点**

追加の編集機能によって、より一貫性のある強力な編集環境が提供されます。

**動作の相違点**

これらの編集の拡張機能は Windows PowerShell ISE 2.0 にはなかった機能です。

## <a name="new-help-viewer-window"></a>新しいヘルプ ビューアー ウィンドウ

> PowerShell 3.0 に追加

カーソルがコマンドレット内にあるかコマンドレットの一部を強調表示しているときに <kbd>F1</kbd> キーを押すと、強調表示されたコマンドレットに関する状況依存のヘルプが新しいヘルプ ビューアーで開かれます。 Windows PowerShell の **About** ヘルプを表示するには、コンソール ウィンドウで「`operators`」と入力し、 <kbd>F1</kbd> キーを押します。

この機能を使用する前に、Microsoft Web サイトから Windows PowerShell のヘルプ トピックの最新バージョンをダウンロードしてください。 ヘルプ トピックをダウンロードする最も簡単な方法は、管理者として Windows PowerShell を実行中に、コンソール ウィンドウで `Update-Help` コマンドレットを実行することです。

<kbd>F1</kbd> キーを押したときにヘルプを検索する場所を変更することができます。 **[ツール]** / **[オプション]** メニューで、 **[全般設定]** タブの **[その他の設定]** の下で、 **[オンライン コンテンツの代わりにローカル ヘルプ コンテンツを使用する]** ボックスをオンまたはオフにすることができます。 オンにした場合、クライアントでは、モジュール フォルダー内にあるダウンロード済みのヘルプから、コマンドレットのヘルプが検索されます。 チェックボックスがオフの場合、クライアントではヘルプをオンラインで検索します。

**この変更の利点**

現在のコマンドレットまたはスクリプトを離れることなく状況依存のヘルプを使用することで、統合された学習を行うことができます。

**動作の相違点**

以前のバージョンの Windows PowerShell ISE では、<kbd>F1</kbd> キーを押すと、ローカル コンピューター上のヘルプ ファイルが開きました。 Windows PowerShell ISE 3.0 以降では、検索と構成が可能なコマンドレットのヘルプを含むウィンドウが開きます。 このヘルプ エクスペリエンスは Windows PowerShell ISE 3.0 の新機能であり、更新可能なヘルプは Windows PowerShell 3.0 の新機能です。

## <a name="show-command-cmdlet"></a>Show-Command コマンドレット

> PowerShell 3.0 に追加

`Show-Command` コマンドレットを使用すると、グラフィカルなフォームに情報を入力してコマンドレットや関数を構成または実行できます。 フォームを使用すると、グラフィカルな環境でユーザーが Windows PowerShell で作業できます。
また、上級のスクリプト作成者は `Show-Command` を使用して、簡単な Windows PowerShell ベースの GUI を作成できます。

**この変更の利点**

Windows PowerShell スクリプトで `Show-Command` を使用すると、ユーザーが使い慣れたグラフィカルな環境を提供できます。 また、`Show-Command` は入門ユーザーが Windows PowerShell を学習するのにも役立ちます。

**動作の相違点**

`Show-Command` は Windows PowerShell ISE 3.0 の新機能です。

## <a name="see-also"></a>関連項目

Windows PowerShell ISE の使い方の詳細については、「[Windows PowerShell Integrated Scripting Environment の探索](../ise/exploring-the-windows-powershell-ise.md)」を参照してください。
