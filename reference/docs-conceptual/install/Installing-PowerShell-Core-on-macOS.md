---
title: macOS への PowerShell のインストール
description: macOS への PowerShell のインストールに関する情報
ms.date: 02/02/2021
ms.openlocfilehash: 8132d88f4104696c5580a44b26247a24643f1b5b
ms.sourcegitcommit: 40b6d8e9b6d791ac69e2ff85224e900b21552bc1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536543"
---
# <a name="installing-powershell-on-macos"></a>macOS への PowerShell のインストール

PowerShell 7.0 以降には macOS 10.13 以降が必要です。 すべてのパッケージは GitHub [リリース][] ページにあります。 パッケージがインストールされたら、ターミナルから `pwsh` を実行します。

> [!NOTE]
> PowerShell 7.1 はインプレース アップグレードで、PowerShell Core 6.x と 7.0 は削除されます。
>
> `/usr/local/microsoft/powershell/6` フォルダーは `/usr/local/microsoft/powershell/7` に置き換えられました。
>
> PowerShell コアの旧バージョンを PowerShell 7.1 と side-by-side 実行する必要がある場合、[バイナリ アーカイブ](#binary-archives)手法を使用し、必要なバージョンをインストールします。

macOS に PowerShell をインストールするには、いくつかの方法があります。 以下のいずれかの方法を選択します。

- Homebrew を使用してインストールする。 Homebrew は、macOS 用の推奨されるパッケージ マネージャーです。
- [直接ダウンロード](#installation-via-direct-download)を使用して PowerShell をインストールする。
- [バイナリ アーカイブ](#binary-archives)からインストールする。

PowerShell をインストールした後、[OpenSSL](#installing-dependencies) をインストールする必要があります。 PowerShell リモート処理および CIM 操作の場合は OpenSSL が必要です。

## <a name="installation-of-latest-stable-release-via-homebrew-on-macos-1013-or-higher"></a>macOS 10.13 以降で Homebrew を使用した最新の安定版リリースのインストール

`brew` コマンドが見つからない場合、[指示][brew]に従い、Homebrew をインストールする必要があります。

これで PowerShell をインストールできます。

```sh
brew install --cask powershell
```

最後に、インストールが正常に動作していることを確認します。

```sh
pwsh
```

新しいバージョンの PowerShell がリリースされたら、Homebrew の式を更新し、PowerShell をアップグレードしてください。

```sh
brew update
brew upgrade powershell --cask
```

> [!NOTE]
> 上記のコマンドは PowerShell (pwsh) ホスト内から呼び出すことができますが、その場合、アップグレードを完了するには、PowerShell シェルを終了し、再起動して、`$PSVersionTable` に表示される値を更新する必要があります。

[brew]: https://brew.sh/

## <a name="installation-of-latest-preview-release-via-homebrew-on-macos-1013-or-higher"></a>macOS 10.13 以降で Homebrew を使用した最新のプレビュー リリースのインストール

Homebrew をインストールしたら、PowerShell をインストールできます。 最初に、[Cask-Versions][cask-versions] パッケージをインストールします。これにより、cask パッケージの代替バージョンをインストールすることができます。

```sh
brew tap homebrew/cask-versions
```

これで PowerShell をインストールできます。

```sh
brew install --cask powershell-preview
```

最後に、インストールが正常に動作していることを確認します。

```sh
pwsh-preview
```

新しいバージョンの PowerShell がリリースされたら、Homebrew の式を更新し、PowerShell をアップグレードしてください。

```sh
brew update
brew upgrade powershell-preview --cask
```

> [!NOTE]
> 上記のコマンドは PowerShell (pwsh) ホストから呼び出すことができますが、その場合、PowerShell シェルを終了し、再起動して、アップグレードを完了し、
> `$PSVersionTable` に表示される値を更新する必要があります。

Homebrew tap メソッドを使用した PowerShell のインストールは、安定バージョンと LTS バージョンでもサポートされています。

```sh
brew install powershell/tap/powershell
```

これで、インストールを確認できます。

```sh
pwsh
```

新しいバージョンの PowerShell がリリースされたら、次のコマンドを実行します。

```sh
brew upgrade powershell
```

> [!NOTE]
> cask メソッドと tap メソッドのどちらを使用する場合でも、新しいバージョンの PowerShell に更新するときに、PowerShell の初期インストールに使用したものと同じメソッドを使用します。 別のメソッドを使用する場合、新しい pwsh セッションを開くと、古いバージョンの PowerShell が引き続き使用されます。
>
> 別のメソッドを使用することを選択した場合は、[Homebrew link メソッドを使用して](https://docs.brew.sh/Manpage#link-ln-options-formula)問題を修正する方法があります。

## <a name="installation-via-direct-download"></a>直接ダウンロードによるインストール

[リリース][] ページから macOS マシンに PKG パッケージ `powershell-7.1.1-osx-x64.pkg` をダウンロードします。

ファイルをダブルクリックして画面の指示に従うか、ターミナルからインストールします。

```sh
sudo installer -pkg powershell-7.1.1-osx-x64.pkg -target /
```

[OpenSSL](#installing-dependencies) をインストールします。 PowerShell リモート処理および CIM 操作の場合は OpenSSL が必要です。

## <a name="install-as-a-net-global-tool"></a>.NET グローバル ツールとしてインストールする

[.NET Core SDK](/dotnet/core/sdk) が既にインストールされている場合は、PowerShell を [.NET グローバル ツール](/dotnet/core/tools/global-tools)として簡単にインストールできます。

```
dotnet tool install --global PowerShell
```

dotnet tool install によって、`PATH` 環境変数に `~/.dotnet/tools` が追加されます。 ただし、現在実行中のシェルには更新された `PATH` が設定されていません。 新しいシェルからは、「`pwsh`」と入力すると PowerShell を起動できるはずです。

[OpenSSL](#installing-dependencies) をインストールします。 PowerShell リモート処理および CIM 操作の場合は OpenSSL が必要です。

## <a name="binary-archives"></a>バイナリ アーカイブ

macOS プラットフォームで高度な展開シナリオを実行するために、PowerShell バイナリ `tar.gz` アーカイブが用意されています。 この方法を使用してインストールする場合は、依存関係を手動でインストールする必要もあります。

[OpenSSL](#installing-dependencies) をインストールします。 PowerShell リモート処理および CIM 操作の場合は OpenSSL が必要です。

### <a name="installing-binary-archives-on-macos"></a>macOS へのバイナリ アーカイブのインストール

```sh
# Download the powershell '.tar.gz' archive
curl -L -o /tmp/powershell.tar.gz https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-osx-x64.tar.gz

# Create the target folder where powershell will be placed
sudo mkdir -p /usr/local/microsoft/powershell/7.1.1

# Expand powershell to the target folder
sudo tar zxf /tmp/powershell.tar.gz -C /usr/local/microsoft/powershell/7.1.1

# Set execute permissions
sudo chmod +x /usr/local/microsoft/powershell/7.1.1/pwsh

# Create the symbolic link that points to pwsh
sudo ln -s /usr/local/microsoft/powershell/7.1.1/pwsh /usr/local/bin/pwsh
```

## <a name="installing-dependencies"></a>依存関係のインストール

PowerShell リモート処理および CIM 操作の場合は OpenSSL が必要です。 必要に応じて、MacPorts を使用して OpenSSL をインストールできます。

> [!NOTE]
> MacPorts と Homebrew を同じシステムで一緒に使用すると、問題が発生する可能性があります。 ただし、Homebrew には OpenSSL 1.0 用のパッケージがありません。 詳細については、[MacPorts の FAQ](https://trac.macports.org/wiki/FAQ) に関する記事を参照してください。

1. Xcode コマンド ライン ツールをインストールします。 MacPorts には Xcode ツールが必要です。

   ```sh
   xcode-select --install
   ```

1. MacPorts をインストールします。 手順が必要な場合は、[インストール ガイド](https://www.macports.org/install.php)をご覧ください。
1. `sudo port selfupdate` を実行して MacPorts を更新します。
1. `sudo port upgrade outdated` を実行して MacPorts パッケージをアップグレードします。
1. `sudo port install openssl10` を実行して OpenSSL をインストールします。
1. PowerShell で使用できるようにライブラリをリンクします。

   ```sh
   sudo mkdir -p /usr/local/opt/openssl
   sudo ln -s /opt/local/lib/openssl-1.0 /usr/local/opt/openssl/lib
   ```

## <a name="uninstalling-powershell"></a>PowerShell のアンインストール

Homebrew を使って PowerShell をインストールした場合は、次のコマンドを使ってアンインストールします。

```sh
brew cask uninstall powershell
```

直接ダウンロードで PowerShell をインストールした場合、手動で PowerShell を削除する必要があります。

```sh
sudo rm -rf /usr/local/bin/pwsh /usr/local/microsoft/powershell
```

追加の PowerShell パスを削除するには、このドキュメントの「[パス](#paths)」セクションを参照し、`sudo rm` を使用してパスを削除してください。

> [!NOTE]
> Homebrew でインストールした場合、この操作は不要です。

## <a name="paths"></a>パス

- `$PSHOME` は `/usr/local/microsoft/powershell/7.1.1/` です
- ユーザー プロファイルは `~/.config/powershell/profile.ps1` から読み込まれます
- 既定のプロファイルは `$PSHOME/profile.ps1` から読み込まれます
- ユーザー モジュールは `~/.local/share/powershell/Modules` から読み込まれます
- 共有モジュールは `/usr/local/share/powershell/Modules` から読み込まれます
- 既定のモジュールは `$PSHOME/Modules` から読み込まれます
- PSReadline 履歴は `~/.local/share/powershell/PSReadLine/ConsoleHost_history.txt` に記録されます

プロファイルには、PowerShell のホスト単位の構成が考慮されています。 そのため、既定のホスト固有のプロファイルは、同じ場所の `Microsoft.PowerShell_profile.ps1` に存在します。

PowerShell は、macOS の [XDG ベース ディレクトリ仕様][xdg-bds]を尊重しています。

macOS は BSD から派生しているので、プレフィックスに `/opt` ではなく `/usr/local` が使用されます。 そのため、`$PSHOME` は `/usr/local/microsoft/powershell/7.1.1/` となり、シンボリック リンクは `/usr/local/bin/pwsh` に配置されます。

## <a name="installation-support"></a>インストールのサポート

Microsoft は、このドキュメントでインストール方法をサポートしています。 他のソースには、利用可能な別のインストール方法が存在する可能性があります。 そのようなツールと方法は機能しても、Microsoft ではそれらの方法をサポートできません。

## <a name="additional-resources"></a>その他のリソース

- [Homebrew Web][brew]
- [Homebrew Github リポジトリ][GitHub]
- [Homebrew-Cask][cask]

[brew]: http://brew.sh/
[Cask]: https://github.com/Homebrew/homebrew-cask
[cask-versions]: https://github.com/Homebrew/homebrew-cask-versions
[GitHub]: https://github.com/Homebrew
[リリース]: https://aka.ms/powershell-release?tag=stable
[xdg-bds]: https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html
