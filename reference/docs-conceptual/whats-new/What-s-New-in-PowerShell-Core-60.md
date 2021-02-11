---
title: PowerShell Core 6.0 の新機能
description: PowerShell Core 6.0 でリリースされた新機能と変更
ms.date: 08/06/2018
ms.openlocfilehash: 18650733117d81e3b8d714960b95b4921122ed32
ms.sourcegitcommit: f9d855dd73b916559a22e337672dea3fbb11c634
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96833827"
---
# <a name="whats-new-in-powershell-core-60"></a>PowerShell Core 6.0 の新機能

[PowerShell Core 6.0][github] は、異種環境とハイブリッド クラウド用に構築されたオープン ソースのクロスプラットフォーム (Windows、macOS、Linux) である、PowerShell の新しいエディションです。

## <a name="moved-from-net-framework-to-net-core"></a>.NET Framework から .NET Core への移行

PowerShell Core では、ランタイムとして [.NET Core 2.0][] を使用します。 .NET core 2.0 を使用することで、PowerShell Core を複数のプラットフォーム (Windows、macOS、Linux) で動作させることができます。 PowerShell Core では、PowerShell のコマンドレットとスクリプトで使用される、.NET Core 2.0 によって提供される API セットも公開します。

Windows PowerShell では、.NET Framework ランタイムを使用して PowerShell エンジンをホストしていました。 これは、Windows PowerShell が .NET Framework によって提供される API セットを公開することを意味します。

.NET Core と .NET Framework の間で共有される API は、[.NET Standard][] の一部として定義されています。

PowerShell Core と Windows PowerShell の間のモジュール/スクリプトの互換性に与える影響の詳細については、[Windows PowerShell との後方互換性](#backwards-compatibility-with-windows-powershell)。

## <a name="support-for-macos-and-linux"></a>macOS と Linux のサポート

PowerShell では、次のように、macOS と Linux が正式にサポートされるようになりました。

- Windows 7、8.1、10
- Windows Server 2008 R2、2012 R2、2016
- [Windows Server 半期チャネル][semi-annual]
- Ubuntu 14.04、16.04、17.04
- Debian 8.7 以降および 9
- CentOS 7
- Red Hat Enterprise Linux 7
- OpenSUSE 42.2
- Fedora 25、26
- macOS 10.12 以降

弊社のコミュニティでは、次のプラットフォーム用のパッケージも提供していますが、正式にはサポートされていません。

- Arch Linux
- Kali Linux
- AppImage (複数の Linux プラットフォームで機能)

また、次のプラットフォーム用の試験的な (サポートされていない) リリースがあります。

- Windows on ARM32/ARM64
- Raspbian (Stretch)

Windows 以外のシステムでより適切に機能するように、PowerShell Core 6.0 には多くの変更が行われました。 これらのいくつかは破壊的変更であり、Windows にも影響します。 その他の変更は、PowerShell Core の Windows 以外のインストールにのみ存在するか適用されます。

- Unix プラットフォームでのネイティブ コマンドのグロビング サポートを追加しました。
- `more` 機能は Linux `$PAGER` に対応し、既定で `less` に設定されます。 これは、ネイティブ バイナリ/コマンドでワイルドカードを使用できるようになったことを意味します (例: `ls *.txt`)。 (#3463)
- 末尾の円記号は、ネイティブ コマンド引数を処理するときに自動的にエスケープされます。 (#4965)
- スクリプトの署名は現在サポートされていないため、Windows 以外のプラットフォームで PowerShell を実行するときは `-ExecutionPolicy` スイッチを無視します。 (#3481)
- Unix プラットフォームで `NoEcho` を受け入れるために ConsoleHost を修正しました。 (#3801)
- Unix プラットフォームで大文字と小文字を区別しないパターン マッチングをサポートするために `Get-Help` を修正しました。 (#3852)
- パッケージに `powershell` man ページが追加されました。

### <a name="logging"></a>ログ記録

macOS では、PowerShell はネイティブ `os_log` API を使用して、Apple の[統合ログ システム][os_log]にログを記録します。 Linux では、PowerShell は、ユビキタス ログ ソリューションである [Syslog][] を使用します。

### <a name="filesystem"></a>ファイルシステム

従来、Windows ではサポートされていなかったファイル名の文字をサポートするために、macOS と Linux について多くの変更が行われました。

- コマンドレットに指定されるパスがスラッシュに依存しなくなりました (/ と \ の両方がディレクトリの区切り記号として機能)。
- XDG Base Directory Specification に準拠するようになり、既定では次のように使用されます。
  - Linux/macOS プロファイルのパスは `~/.config/powershell/profile.ps1` にあります。
  - 履歴保存パスは `~/.local/share/powershell/PSReadline/ConsoleHost_history.txt` にあります。
  - ユーザー モジュール パスは `~/.local/share/powershell/Modules` にあります。
- Unix では、コロン文字を含むファイルとフォルダーの名前がサポートされます。 (#4959)
- コンマを含むスクリプトの名前または完全パスがサポートされます。 (#4136) ([@TimCurwick](https://github.com/TimCurwick) に感謝)
- ナビゲーション コマンドレットのワイルドカードの展開を抑制するために `-LiteralPath` が使用されるタイミングを検出します。 (#5038)
- *nix `ls -R` や Windows の `DIR /S` ネイティブ コマンドのように動作するよう `Get-ChildItem` を更新しました。 `Get-ChildItem` では、再帰的検索時に発生するシンボリック リンクを返し、これらのリンク先のディレクトリは検索しなくなりました。 (#3780)

### <a name="case-sensitivity"></a>大文字小文字の区別

Linux と macOS では大文字と小文字を区別する傾向があります。一方、Windows では大文字と小文字は区別されませんが、大文字と小文字は保持されます。
一般に、PowerShell では大文字と小文字が区別されません。

たとえば、macOS と Linux では環境変数の大文字と小文字が区別されるため、`PSModulePath` 環境変数の大文字と小文字の区別が標準化されています。 (#3255) モジュールの名前を判別するためにファイル パスを使用する場合は、`Import-Module` の大文字と小文字が区別されません。 (#5097)

## <a name="support-for-side-by-side-installations"></a>サイド バイ サイド インストールのサポート

PowerShell Core は、Windows PowerShell とは別にインストール、構成、実行されます。
PowerShell Core には "ポータブル" ZIP パッケージがあります。 ZIP パッケージを使用して、PowerShell を依存関係と見なすアプリケーションに対して局所的なものを含む、ディスクの任意の位置に任意の数のバージョンをインストールすることができます。
サイド バイ サイド インストールでは、新しいバージョンの PowerShell のテストと既存のスクリプトの段階的な移行がより容易になります。 また、サイド バイ サイドでは、スクリプトを必要な特定のバージョンにピン留めできるため、後方互換性が有効になります。

> [!NOTE]
> 既定では、Windows 上の MSI ベースのインストーラーはインプレース アップデート インストールを行います。

## <a name="renamed-powershellexe-to-pwshexe"></a>`powershell(.exe)` から `pwsh(.exe)` への名称変更

PowerShell Core のバイナリ名が `powershell(.exe)` から `pwsh(.exe)` に変更されました。 この変更により、Windows PowerShell と PowerShell Core のサイド バイ サイドのインストールをサポートするために、ユーザーがコンピューター上で PowerShell Core を実行する決定論的な方法が提供されます。 また、`pwsh` は短いため、入力しやすくなります。

`powershell.exe` から `pwsh(.exe)` への追加の変更点は次のとおりです。

- 最初の位置指定パラメーターが `-Command` から `-File` に変更されました。 この変更により、Windows 以外のプラットフォームの PowerShell 以外のシェルから実行されている PowerShell スクリプトの `#!` (シバンともいいます) の使用方法が修正されます。 これは、`-File` を指定せずに `pwsh foo.ps1` または `pwsh fooScript` のようなコマンドを実行できることも意味します。 ただし、この変更によって、`pwsh.exe -Command Get-Command` などのコマンドを実行しようとする場合は `-c` または `-Command` を明示的に指定する必要があります。
  (#4019)
- PowerShell Core は `-i` (または `-Interactive`) スイッチを受け入れ、対話型シェルを示します。
  (# 3558) これにより、Unix プラットフォームで既定のシェルとして PowerShell を使用できるようになります。
- パラメーターの `-importsystemmodules` と `-psconsoleFile` を `pwsh.exe` から削除しました。 (#4995)
- 他のネイティブ ツールに合わせて `pwsh -version` と `pwsh.exe` の組み込みヘルプを変更しました。 (#4958 および #4931) ([@iSazonov](https://github.com/iSazonov) に感謝)
- `-File` と `-Command` の無効な引数エラー メッセージと終了コードの Unix 標準への準拠 (#4573)
- Windows の `-WindowStyle` パラメーターを追加しました。 (#4573) 同様に、Windows 以外のプラットフォームでのパッケージ ベースのインストールはインプレースアップデートとなります。

## <a name="backwards-compatibility-with-windows-powershell"></a>Windows PowerShell との後方互換性

PowerShell Core の目的は、Windows PowerShell との互換性をできる限り維持することです。
PowerShell Core では [.NET Standard][] 2.0 を使用して、既存の .NET アセンブリとのバイナリの互換性を提供します。 多くの PowerShell モジュールはこれらのアセンブリ (多くの場合、Dll) に依存するため、.NET Standard では .NET Core を引き続き使用できます。 PowerShell Core には、.NET Framework DLL の依存関係を検索するための既知のフォルダー (通常、ディスク上のグローバル アセンブリ キャッシュが存在する場所など) を調べるヒューリスティックも含まれます。

.NET Standard の詳細については、[.NET ブログ][]、この [YouTube][] ビデオ、および GitHub のこの [FAQ][] を参照してください。

PowerShell 言語と "組み込み" モジュール (`Microsoft.PowerShell.Management`、`Microsoft.PowerShell.Utility` など) が Windows PowerShell の場合と同じように機能するように最善を尽くしました。 多くの場合、コミュニティの助力とともに、これらのコマンドレットに新しい機能とバグの修正が追加されています。 場合によっては、基になる .NET レイヤーとの依存関係がないため、機能が削除されているか、使用できなくなっています。

Windows の一部として出荷されるモジュールのほとんど (`DnsClient`、`Hyper-V`、`NetTCPIP`、`Storage` など) と、Azure と Office を含むその他の Microsoft 製品はまだ .NET Core には *明示的に* 移植されていません。 PowerShell チームはこれらの製品グループとチームと協力して既存のモジュールを検証し、PowerShell Core に移植しています。 .NET Standard と [CDXML][] を使用すると、これらの従来の Windows PowerShell モジュールの多くが PowerShell Core で機能するようですが、正式には検証されておらず、また、正式にサポートされていません。

[`WindowsPSModulePath`][windowspsmodulepath] モジュールをインストールして、Windows PowerShell `PSModulePath` をご利用の PowerShell Core `PSModulePath` に追加することで、Windows PowerShell モジュールを使用できます。

最初に、PowerShell ギャラリーから `WindowsPSModulePath` モジュールをインストールします。

```powershell
# Add `-Scope CurrentUser` if you're installing as non-admin
Install-Module WindowsPSModulePath -Force
```

このモジュールをインストールしたら、次のように `Add-WindowsPSModulePath` コマンドレットを実行して、Windows PowerShell `PSModulePath` を PowerShell Core に追加します。

```powershell
# Add this line to your profile if you always want Windows PowerShell PSModulePath
Add-WindowsPSModulePath
```

## <a name="docker-support"></a>Docker サポート

PowerShell Core には、弊社がサポートする主なすべてのプラットフォーム (複数の Linux ディストリビューション、Windows Server Core、Nano Server を含む) 用の Docker コンテナーのサポートが追加されています。

完全なリストについては、[Docker Hub の `microsoft/powershell`][docker-hub] のタグを確認してください。 Docker と PowerShell Core の詳細については、GitHub の「[Docker][]」を参照してください。

## <a name="ssh-based-powershell-remoting"></a>SSH ベースの PowerShell リモート処理

PowerShell リモート処理プロトコル (PSRP) が、従来の WinRM ベースの PSRP に加えて、Secure Shell (SSH) でも機能するようになりました。

これは、`Enter-PSSession` や `New-PSSession` などのコマンドレットを使用し、SSH を使用して認証できることを意味します。 OpenSSH ベースの SSH サーバーにサブシステムとして PowerShell を登録するだけで、従来の `PSSession` セマンティクスで既存の SSH ベースの認証メカニズム (パスワードや秘密キーなど) を使用できます。

SSH ベースのリモート処理の構成と使用の詳細については、「[SSH 経由の PowerShell リモート処理][ssh-remoting]」を参照してください。

## <a name="default-encoding-is-utf-8-without-a-bom-except-for-new-modulemanifest"></a>New-ModuleManifest を除き、既定のエンコードは BOM なしの UTF-8 です

これまでは、`Get-Content`、`Set-Content` のような Windows PowerShell コマンドレットでは、ASCII や UTF-16 などの異なるエンコードを使用していました。 エンコードを指定せずにコマンドレットを混在させると、エンコード既定値の差異により問題が生じていました。

従来、Windows 以外のプラットフォームでは、テキスト ファイルの既定のエンコードとして バイト オーダー マーク (BOM) なしで UTF-8 を使用します。 より多くの Windows アプリケーションとツールが UTF-16 から BOM なしの UTF-8 エンコードに移行しつつあります。 PowerShell Core では、より広範囲のエコシステムに準拠するために既定のエンコードが変更されています。

つまり、`-Encoding` パラメーターを使用するすべての組み込みコマンドレットでは、既定で `UTF8NoBOM` 値が使用されます。 この変更の影響を受けるコマンドレットは次のとおりです。

- Add-Content
- Export-Clixml
- Export-Csv
- Export-PSSession
- Format-Hex
- Get-Content
- Import-Csv
- Out-File
- Select-String
- Send-MailMessage
- Set-Content

これらのコマンドレットも更新され、`-Encoding` パラメーターで `System.Text.Encoding` が一般に受け入れられるようになりました。

`$OutputEncoding` の既定値も UTF-8 に変更されました。

ベスト プラクティスとして、`-Encoding` パラメーターを使用してスクリプトに明示的にエンコードを設定し、プラットフォーム間の動作が決定的なものになるようにする必要があります。

`New-ModuleManifest` コマンドレットには **Encoding** パラメーターがありません。 `New-ModuleManifest` コマンドレットで作成されたモジュール マニフェスト (.psd1) ファイルのエンコーディングは、環境によって異なります。Linux で動作する PowerShell Core の場合、エンコーディングは UTF-8 (BOM なし) です。それ以外の場合、エンコーディングは UTF-16 (BOM あり) です。 (#3940)

## <a name="support-backgrounding-of-pipelines-with-ampersand--3360"></a>アンパサンド (`&`) を使用するパイプラインのバックグラウンドでの実行サポート (#3360)

パイプラインの最後に `&` を配置すると、パイプラインが PowerShell ジョブとして実行されます。 パイプラインをバックグラウンドで実行すると、ジョブ オブジェクトが返されます。 パイプラインがジョブとして実行されている場合は、標準の `*-Job` コマンドレットをすべてジョブの管理のために使用できます。 パイプラインで使用される変数 (プロセス固有の変数は無視) は、`Copy-Item $foo $bar &` のみが機能するようにジョブに自動的にコピーされます。 また、ジョブは、ユーザーのホーム ディレクトリではなく、現在のディレクトリで実行されます。 PowerShell ジョブの詳細については、「[about_Jobs](/powershell/module/microsoft.powershell.core/about/about_jobs)」を参照してください。

## <a name="semantic-versioning"></a>セマンティック バージョニング

- `SemanticVersion` に `SemVer 2.0` との互換性を持たせました。 (#5037) ([@iSazonov](https://github.com/iSazonov) に感謝)
- SemVer に合わせて `New-ModuleManifest` の既定の `ModuleVersion` を `0.0.1` に変更しました。 (#4842) ([@LDSpits](https://github.com/LDSpits) に感謝)
- `System.Management.Automation.SemanticVersion` の型のアクセラレータとして `semver` を追加しました。 (#4142) ([@oising](https://github.com/oising) に感謝)
- `SemanticVersion` インスタンスと、`Major` と `Minor` のバージョン値のみで構築される `Version` インスタンスを比較できるようになりました。

## <a name="language-updates"></a>言語の更新

- ユーザーが Unicode 文字を引数、文字列、または変数名として使用できるように、Unicode エスケープ解析を実装します。 (#3958) ([@rkeithhill](https://github.com/rkeithhill) に感謝)
- ESC の新しいエスケープ文字 (`` `e``) を追加しました。
- 列挙型を文字列に変換するサポートを追加しました。(#4318) ([@KirkMunro](https://github.com/KirkMunro) に感謝)
- ジェネリック コレクションへの単一の要素配列のキャストを修正しました。 (#3170)
- `'a'..'z'` が 'a' から 'z' までの文字を返すように、`..` 演算子に文字範囲のオーバーロードを追加しました。 (#5026) ([@IISResetMe](https://github.com/IISResetMe) に感謝)
- 読み取り専用の変数を上書きしないように変数代入を修正しました。
- スクリプト コマンドレットをドットで表す場合に、'DottedScopes' に自動変数のローカルをプッシュします。(#4709)
- 分割演算子で 'Singleline, Multiline' オプションを使用できるようにします。(#4721) ([@iSazonov](https://github.com/iSazonov) に感謝)

## <a name="engine-updates"></a>エンジンの更新

- `$PSVersionTable` には、次の 4 つの新しいプロパティがあります。
  - `PSEdition`:これは、PowerShell Core では `Core` に、Windows PowerShell では `Desktop` に設定されます。
  - `GitCommitId`:これは、PowerShell が構築された Git ブランチまたはタグの Git コミット ID です。
    リリースされたビルドでは、`PSVersion` と同じになる可能性があります。
  - `OS`:これは、`[System.Runtime.InteropServices.RuntimeInformation]::OSDescription` によって返される OS バージョンの文字列です。
  - `Platform`:これは `[System.Environment]::OSVersion.Platform` によって返されます。Windows では `Win32NT` に、macOs では `Unix` に、Linux では `Unix` に設定されます。
- `$PSVersionTable` から `BuildVersion` プロパティを削除しました。 このプロパティは、Windows のビルド バージョンに強く関連付けられていました。 代わりに、`GitCommitId` を使用して、PowerShell Core の正確なビルド バージョンを取得することをお勧めします。 (#3877) ([@iSazonov](https://github.com/iSazonov) に感謝)
- `$PSVersionTable` から `ClrVersion` プロパティを削除しました。 このプロパティは .NET Core には適しておらず、PowerShell に適用できない特定のレガシー用にのみ .NET Core で保持されました。
- PowerShell が特定の OS で実行されているかどうかを判断するために、新しく 3 つの自動変数 (`$IsWindows`、`$IsMacOs`、`$IsLinux`) を追加しました。
- PowerShell Core のバナーに `GitCommitId` を追加します。 バージョンを取得するために PowerShell を起動してすぐに `$PSVersionTable` を実行する必要がなくなりました。 (#3916) ([@iSazonov](https://github.com/iSazonov) に感謝)
- 起動前に必要ないくつかの設定 (`ExecutionPolicy` など) を格納するために、`$PSHome` に `powershell.config.json` という JSON 構成ファイルを追加します。
- Windows EXE の実行中はパイプラインをブロックしません。
- COM コレクションの列挙が有効になりました。 (#4553)

## <a name="cmdlet-updates"></a>コマンドレットの更新

### <a name="new-cmdlets"></a>新しいコマンドレット

- `Get-Uptime` を `Microsoft.PowerShell.Utility` に追加します。
- `Remove-Alias` コマンドを追加します。 (#5143) ([@PowershellNinja](https://github.com/PowershellNinja) に感謝)
- `Remove-Service` を管理モジュールに追加します。 (#4858) ([@joandrsn](https://github.com/joandrsn) に感謝)

### <a name="web-cmdlets"></a>Web コマンドレット

- Web コマンドレットの証明書認証サポートを追加します。 (#4646) ([@markekraus](https://github.com/markekraus) に感謝)
- Web コマンドレットにコンテンツ ヘッダーのサポートを追加します。 (#4494 および #4640) ([@markekraus](https://github.com/markekraus) に感謝)
- Web コマンドレットに複数のリンク ヘッダー サポートを追加します。 (#5265) ([@markekraus](https://github.com/markekraus) に感謝)
- Web コマンドレットでのリンク ヘッダーの改ページのサポート (#3828)
  - `Invoke-WebRequest` では、応答にリンク ヘッダーが含まれている場合、URL と `rel` 属性を表す Dictionary として RelationLink プロパティを作成し、開発者が使いやすいように URL を絶対 URL にします。
  - `Invoke-RestMethod` では、レスポンスに Link ヘッダーが含まれている場合、`-FollowRelLink` スイッチを公開し、Link ヘッダーが存在しなくなるか、省略可能な `-MaximumFollowRelLink` パラメーター値に到達するまで、`next` `rel` リンクに自動的に従うようにします。
- 標準以外のメソッドの動詞で使用できるように、Web コマンドレットに `-CustomMethod` パラメーターを追加します。 (#3142) (@Lee303 に感謝)
- Web コマンドレットに `SslProtocol` サポートを追加します。 (#5329) ([@markekraus](https://github.com/markekraus) に感謝)
- Web コマンドレットにマルチパートのサポートを追加します。 (#4782) ([@markekraus](https://github.com/markekraus) に感謝)
- システム全体のプロキシ設定が無視されるように、Web コマンドレットに `-NoProxy` を追加します。 (#3447) ([@TheFlyingCorpse](https://github.com/TheFlyingCorpse) に感謝)
- Web コマンドレットのユーザー エージェントで OS プラットフォームがレポートされるようになりました。(#4937) ([@LDSpits](https://github.com/LDSpits) に感謝)
- ヘッダー値を検証せずにヘッダーを追加できるように、Web コマンドレットに `-SkipHeaderValidation` スイッチを追加します。 (#4085)
- 必要に応じて、Web コマンドレットでサーバーの HTTPS 証明書を検証しないようにすることができます。
- Web コマンドレットに認証パラメーターを追加します。 (#5052) ([@markekraus](https://github.com/markekraus) に感謝)
  - 次の 3 つのオプションがある `-Authentication` を追加します。Basic、OAuth、Bearer。
  - OAuth および Bearer オプションのベアラー トークンを取得するために、`-Token` を追加します。
  - HTTPS ではなく、任意のトランスポート スキームに対して指定される認証をバイパスするために、`-AllowUnencryptedAuthentication` を追加します。
- 応答ヘッダーのキャプチャを有効にするために、`Invoke-RestMethod` に `-ResponseHeadersVariable` を追加します。
  (#4888) ([@markekraus](https://github.com/markekraus) に感謝)
- 応答ステータス コードが成功でない場合は例外に HTTP 応答を含めるように、Web コマンドレットを修正します。 (#3201)
- Web コマンドレットの `UserAgent` を `WindowsPowerShell` から `PowerShell` に変更します。 (#4914) ([@markekraus](https://github.com/markekraus) に感謝)
- 明示的な `ContentType` 検出を `Invoke-RestMethod` に追加します。(#4692)
- 標準以外の User-Agent ヘッダーを扱うために、Web コマンドレットの `-SkipHeaderValidation` を修正します。 (#4479 &
  #<a name="4512-thanks-markekraus"></a>4512) ([@markekraus](https://github.com/markekraus) に感謝します)

### <a name="json-cmdlets"></a>JSON コマンドレット

- 代わりに `Hashtable` を返すように `ConvertFrom-Json` に `-AsHashtable` を追加します。 (#5043) ([@bergmeister](https://github.com/bergmeister) に感謝)
- `ConvertTo-Json` 出力でよりわかりやすいフォーマッタを使用します。 (#2787) (@kittholland に感謝)
- `ConvertTo-Json` に `Jobject` のシリアル化サポートを追加します。 (#5141)
- 完全な JSON 文字列を一緒に構築する、パイプラインからの文字列配列の逆シリアル化を行うために `ConvertFrom-Json` を修正します。 これにより、改行で JSON 解析が中断される場合があるのを修正します。
  (#3823)
- `System.Array` に定義されている `AliasProperty "Count"` を削除します。 これにより、いくつかの `ConvertFrom-Json` の出力で無関係な `Count` プロパティが削除されます。 (#3231) ([@PetSerAl](https://github.com/PetSerAl) に感謝)

### <a name="csv-cmdlets"></a>CSV のコマンドレット

- `Import-Csv` では W3C 拡張ログ ファイル形式をサポートするようになりました (#2482) ([@iSazonov](https://github.com/iSazonov) に感謝)
- `Import-Csv` と `ConvertFrom-Csv` の `PSTypeName` サポートを追加します。 (#5389) ([@markekraus](https://github.com/markekraus) に感謝)
- `Import-Csv` で行区切り文字として `CR`、`LF`、`CRLF` を使用できるようにします。 (#5363) ([@iSazonov](https://github.com/iSazonov) に感謝)
- `Export-Csv` と `ConvertTo-Csv` で `-NoTypeInformation` を既定値にします。 (#5164) ([@markekraus](https://github.com/markekraus) に感謝)

### <a name="service-cmdlets"></a>サービスのコマンドレット

- `Get-Service` によって返される `ServiceController` オブジェクトに `UserName`、`Description`、`DelayedAutoStart`、`BinaryPathName`、`StartupType` プロパティを追加します。 (#4907) ([@joandrsn](https://github.com/joandrsn) に感謝)
- `Set-Service` コマンドで資格情報を設定するための機能を追加します。 (#4844) ([@joandrsn](https://github.com/joandrsn) に感謝)

### <a name="other-cmdlets"></a>その他のコマンドレット

- リンク ループを確認し、必要に応じて symlink をトラバースする `-FollowSymlink` というパラメーターを `Get-ChildItem` に追加します。 (#4020)
- `CSharpVersion7` をサポートするために `Add-Type` を更新します。 (#3933) ([@iSazonov](https://github.com/iSazonov) に感謝)
- より優れたソリューションが見つかるまでサポートされていない API を使用するため、`Microsoft.PowerShell.LocalAccounts` モジュールを削除します。 (#4302)
- より優れたソリューションが見つかるまでサポートされていない API を使用するため、`Microsoft.PowerShell.Diagnostics` の `*-Counter` コマンドレットを削除します。 (#4303)
- `Invoke-Item -Path <folder>` のサポートが追加されます。 (#4262)
- ファイル名拡張子とファイル名の残りの部分の間のパスを分割できるように、`Split-Path` に `-Extension` および `-LeafBase` スイッチを追加します。 (#2721) ([@powercode](https://github.com/powercode) に感謝)
- Top/Bottom N Sort のために `Sort-Object` にパラメーターの `-Top` と `-Bottom` を追加します。
- `CodeProperty "Parent"` を `System.Diagnostics.Process` に追加して、プロセスの親プロセスを公開します。 (#2850) ([@powercode](https://github.com/powercode) に感謝)
- `Get-Process` のメモリ列で、KB ではなく MB を使用します。
- `Out-String` の `-NoNewLine` スイッチを追加します。 (#5056) ([@raghav710](https://github.com/raghav710) に感謝)
- `Move-Item` コマンドレットで `-Include`、`-Exclude`、`-Filter` のパラメーターを受け入れます。 (#3878)
- `Remove-Item` のレジストリ パスで `*` を使用できるようにします。 (#4866)
- `-Title` を `Get-Credential` に追加して、プラットフォーム間のプロンプト エクスペリエンスを統合します。
- `-TimeOut` パラメーターを `Test-Connection` に追加します。 (#2492)
- `Get-AuthenticodeSignature` コマンドレットで、ファイルの署名タイムスタンプを取得できるようになりました。 (#4061)
- `Get-Help` からサポートされていない `-ShowWindow` スイッチを削除します。 (#4903)
- 返される配列要素に区切り文字が含まれないように `Get-Content -Delimiter` を修正します。(#3706) ([@mklement0](https://github.com/mklement0) に感謝)
- `Meta`、`Charset`、`Transitional` パラメーターを `ConvertTo-HTML` に追加します。(#4184) ([@ergo3114](https://github.com/ergo3114) に感謝)
- `Get-ComputerInfo` の結果に `WindowsUBR` および `WindowsVersion` プロパティを追加します。
- `-Group` パラメーターを `Get-Verb` に追加します。
- `ShouldProcess` のサポートを `New-FileCatalog` と `Test-FileCatalog` に追加します (`-WhatIf` と `-Confirm` を修正)。 (#3074) ([@iSazonov](https://github.com/iSazonov) に感謝)
- `-WhatIf` スイッチを `Start-Process` コマンドレットに追加します。(#4735) ([@sarithsutha](https://github.com/sarithsutha) に感謝)
- `ValidateNotNullOrEmpty` を多くの既存のパラメーターに追加します。

## <a name="tab-completion"></a>Tab 補完機能

- ランタイム変数の値に基づいて、Tab 補完機能の型推論を改善しました。 (#2744) ([@powercode](https://github.com/powercode) に感謝)これにより、次のような場合に Tab 補完機能が有効になります。

  ```powershell
  $p = Get-Process
  $p | Foreach-Object Prio<tab>
  ```

- `Select-Object` の `-Property` にハッシュテーブルの Tab 補完機能を追加します。 (#3625) ([@powercode](https://github.com/powercode) に感謝)
- `Select-Object` の `-ExcludeProperty` と `-ExpandProperty` で引数のオートコンプリートを有効にします。
  (#3443) ([@iSazonov](https://github.com/iSazonov) に感謝)
- ネイティブ コンプリーターに対する `native.exe --<tab>` 呼び出しを行うために、Tab 補完機能のバグを修正します。 (#3633) ([@powercode](https://github.com/powercode) に感謝)

## <a name="breaking-changes"></a>重大な変更

PowerShell Core 6.0 には多くの破壊的変更が導入されています。
その詳細については、[PowerShell Core 6.0 の破壊的変更][breaking-changes]に関するページを参照してください。

## <a name="debugging"></a>デバッグ

- `Invoke-Command -ComputerName` でのリモート ステップイン デバッグをサポートします。 (#3015)
- PowerShell Core でのバインダー デバッグ ログを有効にします。

## <a name="filesystem-updates"></a>ファイルシステムの更新

- UNC パスからの FileSystem プロバイダーの使用を有効にします。 ($4998)
- UNC ルートで `Split-Path` が使用できるようになりました。
- 引数なしの `cd` が `cd ~` として動作するようになりました。
- 260 文字を超えるパスを使用できるように PowerShell Core を修正しました。 (#3960)

## <a name="bug-fixes-and-performance-improvements"></a>バグの修正とパフォーマンスの改善

起動時刻、さまざまな組み込みコマンドレット、ネイティブ バイナリの操作など、PowerShell 全体のパフォーマンスについて *多く* の改善を行いました。

PowerShell Core 内の多くのバグの修正も行いました。 修正と変更の完全なリストについては、GitHub の[変更ログ][]に関するページを確認してください。

## <a name="telemetry"></a>テレメトリ

- PowerShell Core 6.0 では、次の 2 つの値をレポートするためにコンソール ホストにテレメトリが追加されました。(#3620)
  - OS のプラットフォーム (`$PSVersionTable.OSDescription`)
  - PowerShell の正確なバージョン (`$PSVersionTable.GitCommitId`)

このテレメトリをオプトアウトする場合は、単に `POWERSHELL_TELEMETRY_OPTOUT` 環境変数を作成して値を `true`、`1`、または `yes` のいずれかに設定します。 この変数を作成すると、PowerShell を初めて実行する前であっても、すべてのテレメトリがバイパスされます。 [コミュニティ ダッシュボード][community-dashboard]のテレメトリから収集される、このテレメトリ データと分析情報を公開することも予定しています。 このデータの使用方法の詳細については、こちらの[ブログの投稿][telemetry-blog]を参照してください。

[.NET ブログ]: https://devblogs.microsoft.com/dotnet/introducing-net-standard/
[.NET Core 2.0]: /dotnet/core/
[.NET Standard]: /dotnet/standard/net-standard
[breaking-changes]: breaking-changes-ps6.md
[CDXML]: /previous-versions/windows/desktop/wmi_v2/getting-started-with-cdxml
[変更ログ]: https://github.com/PowerShell/PowerShell/tree/master/CHANGELOG.md
[community-dashboard]: https://aka.ms/PSGitHubBI
[docker-hub]: https://hub.docker.com/r/microsoft/powershell/
[docker]: https://github.com/PowerShell/PowerShell/tree/master/docker
[FAQ]: https://github.com/dotnet/standard/blob/master/docs/faq.md
[github]: https://github.com/PowerShell/PowerShell
[os_log]: https://developer.apple.com/documentation/os/logging
[semi-annual]: /windows-server/get-started/semi-annual-channel-overview
[ssh-remoting]: /powershell/scripting/learn/remoting/SSH-Remoting-in-PowerShell-Core
[Syslog]: https://en.wikipedia.org/wiki/Syslog
[telemetry-blog]: https://devblogs.microsoft.com/powershell/powershell-open-source-community-dashboard/
[windowspsmodulepath]: https://www.powershellgallery.com/packages/WindowsPSModulePath/
[YouTube]: https://www.youtube.com/watch?v=YI4MurjfMn8&list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY
