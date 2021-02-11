---
title: PowerShell 開発のための Visual Studio Code の使用
description: PowerShell 開発のための Visual Studio Code の使用
ms.date: 11/07/2019
ms.openlocfilehash: 8a4ceb3da669716915449af2d211aaf2ae61bb4f
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390304"
---
# <a name="using-visual-studio-code-for-powershell-development"></a>PowerShell 開発のための Visual Studio Code の使用

[Visual Studio Code][vscode] は、Microsoft によるクロスプラットフォームのスクリプト エディターです。 [PowerShell 拡張機能][psext]と共に使用すると、充実した対話型のスクリプト編集エクスペリエンスがもたらされ、信頼性の高い PowerShell スクリプトを簡単に記述できます。 PowerShell スクリプトの記述に使用するエディターとしては、PowerShell 拡張機能を備えた Visual Studio Code をお勧めします。

次の PowerShell バージョンがサポートされています。

- PowerShell 7 以上 (Windows、macOS、および Linux)
- PowerShell Core 6 (Windows、macOS、および Linux)
- Windows PowerShell 5.1 (Windows のみ)

> [!NOTE]
> Visual Studio Code は、[Visual Studio][] と同じものではありません。

## <a name="getting-started"></a>作業の開始

開始する前に、システムに PowerShell があることを確認します。 Windows、macOS、Linux 上の最近のワークロードに対しては、次のリンクを参照してください。

- [Linux への PowerShell のインストール][install-pscore-linux]
- [macOS への PowerShell のインストール][install-pscore-macos]
- [Windows への PowerShell のインストール][install-pscore-windows]

従来の Windows PowerShell ワークロードについては、「[Windows PowerShell のインストール][install-winps]」を参照してください。

> [!IMPORTANT]
> [Windows PowerShell ISE][ise] は引き続き Windows で使用できます。 ただし、アクティブな機能開発の対象ではなくなりました。 ISE は、PowerShell 6 以降では機能しません。 Windows のコンポーネントとして、セキュリティや優先順位の高いサービスに関する修正プログラムが引き続き公式でサポートされます。
> Windows から ISE が削除される予定はありません。

## <a name="editing-with-visual-studio-code"></a>Visual Studio Code を使用した編集

1. Visual Studio Code をインストールします。 詳細については、「[Visual Studio Code の設定][vsc-setup]」という概要ページを参照してください。

   プラットフォーム別のインストール手順があります。

   - **Windows**: [Windows 上での Visual Studio Code の実行][vsc-setup-win]に関するページに記載されているインストール手順に従ってください。
   - **macOS**: [macOS 上での Visual Studio Code の実行][vsc-setup-macOS]に関するページに記載されているインストール手順に従ってください。
   - **Linux**: [Linux 上での Visual Studio Code の実行][vsc-setup-linux]に関するページに記載されているインストール手順に従ってください。

1. PowerShell 拡張機能をインストールします。

   1. コンソールで「`code`」と入力するか、Visual Studio Code Insiders をインストールした場合は「`code-insiders`」と入力して、Visual Studio Code アプリを起動します。
   1. Windows または Linux の場合、<kbd>Ctrl</kbd>+<kbd>P</kbd> を押し、**Quick Open** を起動します。 macOS の場合、<kbd>Cmd</kbd>+<kbd>P</kbd> を押します。
   1. Quick Open を開き、「`ext install powershell`」と入力し、**Enter** キーを押します。
   1. サイド バーに **[拡張機能]** ビューが開きます。 Microsoft の PowerShell の拡張機能を選択します。
      次の画像のような Visual Studio Code の画面が表示されるはずです。

      ![Visual Studio Code - PowerShell 拡張機能の表示](media/using-vscode/vscode.png)

   1. Microsoft の PowerShell 拡張機能の、 **[インストール]** ボタンをクリックします。
   1. インストール後、 **[インストール]** ボタンが **[再読み込み]** に変わった場合は、 **[再読み込み]** をクリックします。
   1. Visual Studio Code が再読み込みされたら、編集が可能になります。

たとえば、新しいファイルを作成するには、 **[ファイル]、[新規]** の順にクリックします。 保存するには、 **[ファイル]、[保存]** の順にクリックし、`HelloWorld.ps1` のようなファイル名を入力します。 ファイルを閉じるには、ファイル名の横の `X` をクリックします。 Visual Studio Code を終了するには、 **[ファイル] > [終了]** を選択します。

### <a name="installing-the-powershell-extension-on-restricted-systems"></a>制限されているシステムへの PowerShell 拡張機能のインストール

一部のシステムは、すべてのコード署名の検証を要求するように設定されています。 次のエラーが表示される場合があります。

```
Language server startup failed.
```

この問題は、PowerShell の実行ポリシーが Windows グループ ポリシーによって設定されている場合に発生する可能性があります。 PowerShell エディター サービスおよび Visual Studio Code 用 PowerShell 拡張機能を手動で承認するには、PowerShell プロンプトを開いて次のコマンドを実行します。

```powershell
Import-Module $HOME\.vscode\extensions\ms-vscode.powershell*\modules\PowerShellEditorServices\PowerShellEditorServices.psd1
```

"**この信頼されていない発行元からのソフトウェアを実行しますか?** " というメッセージが表示されます。 `A` キーを押してファイルを実行します。 次に、Visual Studio Code を開き、PowerShell 拡張機能が正しく機能していることを確認します。 まだ問題が解決しない場合は、[GitHub の問題][]でお知らせください。

> [!NOTE]
> Visual Studio Code 用 PowerShell 拡張機能では、制約付き言語モードでの実行はサポートされていません。 詳細については、[GitHub の問題 #606][i606] を参照してください。

### <a name="choosing-a-version-of-powershell-to-use-with-the-extension"></a>拡張機能で使用する PowerShell のバージョンを選択

Windows PowerShell と PowerShell Core がインストールされている場合、PowerShell の特定のバージョンを PowerShell の拡張機能で使用できるようになりました。 この機能では、異なるオペレーティング システム上のいくつかのよく知られているパスを検索して、PowerShell のインストールが検出されます。

バージョンは次の手順で選択します。

1. Windows または Linux の場合、<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> を使用して **コマンド パレット** を開きます。 macOS の場合、<kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> を使用します。
1. "**セッション**" を検索します。
1. **[PowerShell: Show Session Menu]\(PowerShell: セッション メニューを表示\)** をクリックします。
1. 一覧から、"**PowerShell Core**" など、使用する PowerShell のバージョンを選択します。

PowerShell を一般的ではない場所にインストールしている場合、最初にそれがセッション メニューに表示されない場合があります。 次のように、[独自のカスタム パスを追加して](#adding-your-own-powershell-paths-to-the-session-menu)、セッション メニューを拡張できます。

> [!NOTE]
> PowerShell セッション メニューには、ステータス バーの右下隅にある緑のバージョン番号からアクセスすることもできます。 このバージョン番号をクリックすると、セッション メニューが開かれます。

## <a name="configuration-settings-for-visual-studio-code"></a>Visual Studio Code の構成設定

まず、Visual Studio Code の設定を変更する方法に慣れていない場合は、[Visual Studio Code の設定に関するドキュメント][vsc-settings]を読むことをお勧めします。

ドキュメントを読み終えたら、`settings.json` で構成設定を追加できます。

```json
{
    "editor.renderWhitespace": "all",
    "editor.renderControlCharacters": true,
    "files.trimTrailingWhitespace": true,
    "files.encoding": "utf8bom",
    "files.autoGuessEncoding": true
}
```

これらの設定がすべてのファイルの種類に影響しないようにする場合、Visual Studio Code では言語ごとに構成することもできます。 `[<language-name>]` フィールドに設定を指定して、言語固有の設定を作成します。 次に例を示します。

```json
{
    "[powershell]": {
        "files.encoding": "utf8bom",
        "files.autoGuessEncoding": true
    }
}
```

> [!TIP]
> Visual Studio Code でのファイル エンコードについて詳しくは、[ファイル エンコードの概要][file-encoding]に関する記事をご覧ください。
>
> また、PowerShell 編集用に Visual Studio Code を構成する方法に関するその他のヒントについては、「[Visual Studio Code で ISE のエクスペリエンスをレプリケートする方法][vsc-ise]」もご覧ください。

### <a name="adding-your-own-powershell-paths-to-the-session-menu"></a>セッション メニューへの独自の PowerShell のパスの追加

[Visual Studio Code 設定](https://code.visualstudio.com/docs/getstarted/settings): `powershell.powerShellAdditionalExePaths` を使用して、セッション メニューに他の PowerShell の実行パスを追加できます。

次のように `powershell.powerShellAdditionalExePaths` のリストに項目を追加するか、`settings.json` にない場合はリストを作成します。

```json
{
    // other settings...

    "powershell.powerShellAdditionalExePaths": [
        {
            "exePath": "C:\\Users\\tyler\\Downloads\\PowerShell\\pwsh.exe",
            "versionName": "Downloaded PowerShell"
        }
    ],

    // other settings...
}
```

各項目には次が必要です。

- `exePath`:`pwsh` または `powershell` 実行可能ファイルへのパス。
- `versionName`:セッション メニューに表示されるテキスト。

PowerShell の既定のバージョンを設定するには、セッション メニューに表示されるテキストに値 `powershell.powerShellDefaultVersion` を設定します (`versionName` とも呼ばれます)。

```json
{
    // other settings...

    "powershell.powerShellAdditionalExePaths": [
        {
            "exePath": "C:\\Users\\tyler\\Downloads\\PowerShell\\pwsh.exe",
            "versionName": "Downloaded PowerShell"
        }
    ],

    "powershell.powerShellDefaultVersion": "Downloaded PowerShell",

    // other settings...
}
```

この設定を構成したら、Visual Studio Code を再起動します。または、**コマンド パレット** から現在の Visual Studio Code ウィンドウを再度読み込むには、「**Developer: Reload Window**」と入力します。

これでセッション メニューを開くと、追加した PowerShell バージョンが表示されます。

> [!NOTE]
> これは、ソースから PowerShell をビルドした場合、PowerShell のローカル ビルドをテストする優れた方法です。

### <a name="using-an-older-version-of-the-powershell-extension-for-windows-powershell-v3-and-v4"></a>Windows PowerShell v3 および v4 用の古いバージョンの PowerShell 拡張機能を使用する

現在の PowerShell 拡張機能では [PowerShell v3 と v4][i1310] はサポートされていません。 ただし、PowerShell v3 と v4 をサポートする最新バージョンの拡張機能を使用できます。

> [!CAUTION]
> この古いバージョンの拡張機能に修正プログラムが追加されることはありません。 これは "そのままの状態で" 提供されますが、Windows PowerShell v3 と Windows PowerShell v4 を引き続き使用している場合に利用できます。

まず、[拡張機能] ペインを開いて「`PowerShell`」を検索します。 次に、歯車をクリックして、 **[Install another version...]\(別のバージョンをインストール...\)** を選択します。

![メニュー項目 - 別のバージョンをインストール...](media/using-vscode/install-another-version.png)

次に、バージョン **2020.1.0** を選択します。 このバージョンの拡張機能は、v3 と v4 がサポートされている最後のバージョンです。 拡張機能のバージョンが自動的に更新されないように、必ず次の設定を追加します。

```json
{
    "extensions.autoUpdate": false
}
```

バージョン **2020.1.0** は、近い将来有効になります。 ただし、このバージョンの拡張機能が動作しなくなるような変更が Visual Studio Code で実装される可能性があります。 このことと、サポートがないことから、次のいずれかを強くお勧めします。

- Windows PowerShell 5.1 にアップグレードする
- PowerShell 7 をインストールする。これは Windows PowerShell とのサイド バイ サイド インストールであり、PowerShell 拡張機能と最適に連携します

## <a name="debugging-with-visual-studio-code"></a>Visual Studio Code を使用したデバッグ

### <a name="no-workspace-debugging"></a>ワークスペースを使用しないデバッグ

Visual Studio Code バージョン 1.9 (以降) では、PowerShell スクリプトを含むフォルダーを開かずに PowerShell スクリプトをデバッグできます。

1. **[ファイル] > [ファイルを開く]** の順に選択して、PowerShell スクリプト ファイルを開きます。
1. ブレークポイントの設定 - 行を選択して、<kbd>F9</kbd> キーを押します。
1. <kbd>F5</kbd> キーを押してデバッグを開始します。

[Debug actions]\(デバッグ アクション\) ウィンドウが表示されます。ここでは、デバッガーを中断したり、デバッグをステップ実行、再開、停止したりできます。

### <a name="workspace-debugging"></a>ワークスペースを使用したデバッグ

ワークスペースを使用したデバッグとは、 **[ファイル]** メニューの **[フォルダーを開く]** を使用して開いたフォルダーのコンテキストでデバッグを行うことを言います。開いたフォルダーは、通常は PowerShell プロジェクトのフォルダーまたは Git リポジトリのルートです。 ワークスペースを使用したデバッグでは、現在開いているファイルをデバッグする以外に、複数のデバッグ構成を定義できます。

デバッグ用の構成ファイルを作成するには、次の手順に従います。

1. Windows または Linux の場合、<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>D</kbd> を押し、 **[デバッグ]** ビューを開きます。 macOS の場合、<kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>D</kbd> を押します。
1. **launch.json ファイルの作成** リンクをクリックします。
1. **[環境を選択]** プロンプトで、 **[PowerShell]** を選択します。
1. 使用するデバッグの種類を選択します。

   - **現在のファイルを起動** - 現在アクティブなエディター ウィンドウのファイルを起動してデバッグします
   - **スクリプトを起動** - 指定したファイルまたはコマンドを起動してデバッグします
   - **対話型セッション** - 統合コンソールから実行されたコマンドをデバッグします
   - **アタッチ** - 実行中の PowerShell ホスト プロセスにデバッガーをアタッチします

デバッグ構成を格納する自分のワークスペース フォルダーのルートにディレクトリとファイル `.vscode\launch.json` が Visual Studio Code によって作成されます。 ファイルが Git リポジトリにある場合は、通常、`launch.json` ファイルをコミットすることになるでしょう。 `launch.json` ファイルの内容:

```json
{
  "version": "0.2.0",
  "configurations": [
      {
          "type": "PowerShell",
          "request": "launch",
          "name": "PowerShell Launch (current file)",
          "script": "${file}",
          "args": [],
          "cwd": "${file}"
      },
      {
          "type": "PowerShell",
          "request": "attach",
          "name": "PowerShell Attach to Host Process",
          "processId": "${command.PickPSHostProcess}",
          "runspaceId": 1
      },
      {
          "type": "PowerShell",
          "request": "launch",
          "name": "PowerShell Interactive Session",
          "cwd": "${workspaceRoot}"
      }
  ]
}
```

このファイルは一般的なデバッグ シナリオです。 エディターでこのファイルを開いた場合、 **[構成の追加]** ボタンが表示されます。 このボタンをクリックすることで、PowerShell デバッグ構成をさらに追加できます。 追加すると便利な構成の 1 つは、 **[PowerShell: Launch Script]\(PowerShell: 起動スクリプト\)** です。 この構成では、エディターでどのファイルがアクティブであるかに関係なく、<kbd>F5</kbd> キーを押すたびに使用される省略可能な引数を含むファイルを指定できます。

デバッグ構成が確立されたら、デバッグ セッション中に使用する構成を選択できます。 **[デバッグ]** ビューのツール バーでデバッグ構成ドロップダウンから構成を選択します。

## <a name="troubleshooting-the-powershell-extension-for-visual-studio-code"></a>Visual Studio Code 用 PowerShell 拡張機能のトラブルシューティング

PowerShell スクリプト開発のための Visual Studio Code の使用に関して問題が発生した場合は、GitHub 上の[トラブルシューティング ガイド][troubleshooting]を参照してください

## <a name="useful-resources"></a>有用なリソース

Visual Studio Code 用の PowerShell 拡張機能の使用を開始するのに役立つビデオとブログ記事を、次にいくつか示します。

### <a name="videos"></a>ビデオ

- [既定の PowerShell エディターとして Visual Studio Code を使用する](https://youtu.be/bGn45vIeAMM)
- [Visual Studio Code: PowerShell スクリプトのデバッグの詳細](https://youtu.be/cSbIXmlkr8o)

### <a name="blog-posts"></a>ブログ記事

- [PowerShell の拡張機能][pscdn]
- [Visual Studio Code での PowerShell スクリプトの記述およびデバッグ][debug]
- [Visual Studio Code でのデバッグのガイダンス][psdbgblog]
- [Visual Studio Code での PowerShell のデバッグ][psdbg-gh]
- [Visual Studio Code での PowerShell 開発の概要][getting-started]
- [PowerShell 開発のための Visual Studio Code の編集機能 – パート 1][editing-part1]
- [PowerShell 開発のための Visual Studio Code の編集機能 – パート 2][editing-part2]
- [Visual Studio Code での PowerShell スクリプトのデバッグ – パート 1][debugging-part1]
- [Visual Studio Code での PowerShell スクリプトのデバッグ – パート 2][debugging-part2]

## <a name="powershell-extension-project-source-code"></a>PowerShell 拡張機能プロジェクトのソース コード

PowerShell の拡張機能のソース コードは、[GitHub][psext-src] にあります。

共同作成に関心がある場合は、ぜひ pull request をご活用ください。 [GitHub 上の開発者向けドキュメント][dev-docs]を参照して、作業を開始してください。

<!-- related articles -->
[ise]:                    ../../windows-powershell/ise/Introducing-the-Windows-PowerShell-ISE.md
[install-pscore-linux]:   ../../install/Installing-PowerShell-Core-on-Linux.md
[install-pscore-macos]:   ../../install/Installing-PowerShell-Core-on-macOS.md
[install-pscore-windows]: ../../install/Installing-PowerShell-Core-on-Windows.md
[install-winps]:          ../../install/Installing-Windows-PowerShell.md
[file-encoding]:          understanding-file-encoding.md
[vsc-ise]:                How-To-Replicate-the-ISE-Experience-In-VSCode.md

<!-- blogs -->
[debug]:                  https://devblogs.microsoft.com/powershell/announcing-powershell-language-support-for-visual-studio-code-and-more/
[debugging-part1]:        https://devblogs.microsoft.com/scripting/debugging-powershell-script-in-visual-studio-code-part-1/
[debugging-part2]:        https://devblogs.microsoft.com/scripting/debugging-powershell-script-in-visual-studio-code-part-2/
[editing-part1]:          https://devblogs.microsoft.com/scripting/visual-studio-code-editing-features-for-powershell-development-part-1/
[editing-part2]:          https://devblogs.microsoft.com/scripting/visual-studio-code-editing-features-for-powershell-development-part-2/
[getting-started]:        https://devblogs.microsoft.com/scripting/get-started-with-powershell-development-in-visual-studio-code/
[psdbgblog]:              https://johnpapa.net/debugging-with-visual-studio-code/
[psdbg-gh]:               https://github.com/PowerShell/vscode-powershell/tree/master/examples
[pscdn]:                  https://docs.microsoft.com/archive/blogs/cdndevs/visual-studio-code-powershell-extension

<!-- issues -->
[GitHub の問題]:          https://github.com/PowerShell/vscode-powershell/issues
[i1310]:                  https://github.com/PowerShell/vscode-powershell/issues/1310
[i606]:                   https://github.com/PowerShell/vscode-powershell/issues/606

<!-- product links -->
[Visual Studio]:          https://visualstudio.microsoft.com/
[vscode]:                 https://code.visualstudio.com/
[psext]:                  https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell
[vsc-settings]:           https://code.visualstudio.com/docs/getstarted/settings
[vsc-setup]:              https://code.visualstudio.com/Docs/setup/setup-overview
[vsc-setup-win]:          https://code.visualstudio.com/docs/setup/windows
[vsc-setup-macos]:        https://code.visualstudio.com/docs/setup/mac
[vsc-setup-linux]:        https://code.visualstudio.com/docs/setup/linux
[psext-src]:              https://github.com/PowerShell/vscode-powershell
[troubleshooting]:        https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md
[dev-docs]:               https://github.com/PowerShell/vscode-powershell/blob/master/docs/development.md
