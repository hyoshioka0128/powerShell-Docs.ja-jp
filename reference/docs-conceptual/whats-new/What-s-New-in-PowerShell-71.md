---
title: PowerShell 7.1 の新機能
description: PowerShell 7.1 でリリースされた新機能と変更
ms.date: 12/15/2020
ms.openlocfilehash: 6bbeccd07dd696ee019828bdcd68ce3f6ee627e3
ms.sourcegitcommit: 1628fd2a1f50aec2f31ffb1c451a3ce77c08983c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577209"
---
# <a name="whats-new-in-powershell-71"></a>PowerShell 7.1 の新機能

2020 年 11 月 11 日に、PowerShell 7.1 の一般提供を[発表しました](https://devblogs.microsoft.com/powershell/announcing-powershell-7-1/)。 PowerShell 7.0 において確立された基盤が基になっており、コミュニティの問題に関する作業が重点的に行われ、さまざまな改善と修正が含まれています。 PowerShell が安定した高性能なプラットフォームとして維持されるよう取り組んでいます。

PowerShell 7.1 には、次の機能、更新プログラム、破壊的変更が含まれています。

- 予測 IntelliSense を含む PSReadLine 2.1.0
- PowerShell 7.1 が Microsoft Store に公開されています
- ARM64 をサポートする新しい OS バージョン用に更新されたインストーラー パッケージ
- メインストリームに昇格した 4 つの新しい試験的機能と 2 つの試験的機能
- 使いやすさ向上のためのいくつかの破壊的変更

詳細な変更一覧については、GitHub リポジトリの[変更ログ](https://github.com/PowerShell/PowerShell/blob/master/CHANGELOG/7.1.md)を参照してください。

## <a name="psreadline-210"></a>PSReadLine 2.1.0

PowerShell 7.1 には、PSReadLine 2.1.0 も組み込まれています。 このバージョンには、Predictive IntelliSense が含まれています。 Predictive IntelliSense 機能の詳細については、PowerShell ブログの[お知らせ](https://devblogs.microsoft.com/powershell/announcing-psreadline-2-1-with-predictive-intellisense/)を参照してください。

## <a name="microsoft-store-installer-package"></a>Microsoft Store インストーラー パッケージ

PowerShell 7.1 が Microsoft Store に公開されています。 PowerShell リリースは [Microsoft Store](https://www.microsoft.com/store/apps/9MZ1SNWT0N5D) Web サイトまたは Windows の Store アプリケーションで見つけることができます。

Microsoft Store パッケージの利点:

- Windows 10 に直接組み込まれた自動更新
- Intune や SCCM など、他のソフトウェア配布メカニズムとの統合

> [!NOTE]
> `$PSHOME` に格納されているシステムレベルの構成設定は変更できません。 これには WSMAN 構成が含まれます。 これにより、リモート セッションが PowerShell のストアベース インストールに接続できなくなります。 ユーザーレベル構成と SSH リモート処理がサポートされていません。

## <a name="other-installers"></a>その他のインストーラー

サポートされているオペレーティング システムとサポート ライフサイクルの最新情報については、[PowerShell サポート ライフサイクル](/powershell/scripting/powershell-support-lifecycle)に関するページをご覧ください。

任意のオペレーティング システムのインストール手順を確認してください。

- [Windows](/powershell/scripting/install/installing-powershell-core-on-windows)
- [macOS](/powershell/scripting/install/installing-powershell-core-on-macos)
- [Linux](/powershell/scripting/install/installing-powershell-core-on-linux)

さらに、PowerShell 7.1 では、Debian、Ubuntu、ARM64 Alpine Linux の ARM32 および ARM64 フレーバーもサポートされています。

公式にはサポートされていませんが、コミュニティでは、[Arch](https://aur.archlinux.org/packages/powershell/) および Kali Linux 用のパッケージも提供しています。

> [!NOTE]
> Debian 10 以降、CentOS 8 以降、Ubuntu 20.04、Alpine、Arm では、WinRM リモート処理はサポートされていません。 SSH ベースのリモート処理設定の詳細については、「[SSH 経由の PowerShell リモート処理](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)」を参照してください。

## <a name="experimental-features"></a>試験的な機能

試験的な機能の詳細については、[試験的機能の使用](../learn/experimental-features.md)に関するページを参照してください。

このリリースでは、次の試験的機能がメインストリーム機能になりました。

- [PSNullConditionalOperators](../learn/experimental-features.md#psnullconditionaloperators)
- [PSUnixFileStat](../learn/experimental-features.md#psunixfilestat)

このリリースでは、次の試験的機能が追加されました。

- [Microsoft.PowerShell.Utility.PSManageBreakpointsInRunspace](../learn/experimental-features.md#microsoftpowershellutilitypsmanagebreakpointsinrunspace)
  - PowerShell 7.1 では、この試験的機能が拡張され、すべての `*-PSBreakpoint` コマンドレットに **Runspace** パラメーターが追加されます。 **Runspace** パラメーターによって、**Runspace** オブジェクトを指定し、指定した実行空間内のブレークポイントと対話できます。

- [PSNativePSPathResolution](../learn/experimental-features.md#psnativepspathresolution) - この機能を使用すると、Powershell のパス構文をサポートしていないネイティブ コマンドに PowerShell プロバイダーのパスを渡すことができます。

- [PSCultureInvariantReplaceOperator](../learn/experimental-features.md#pscultureinvariantreplaceoperator) - `-replace` 演算子ステートメントの左側のオペランドが文字列でない場合、そのオペランドは文字列に変換されます。 機能が有効になっている場合、この変換によって、文字列変換にカルチャ設定が使用されることはありません。

- [PSSubsystemPluginModel](../learn/experimental-features.md#pssubsystempluginmodel) は、今後の予測 IntelliSense プラグインをサポートするための基礎となります。

## <a name="breaking-changes-and-improvements"></a>破壊的変更と機能強化

- ネイティブ コマンドによって `stderr` に書き込まれるときに、`$?` が `$false` にならないように修正されました ([#13395](https://github.com/PowerShell/PowerShell/pull/13395))

  ネイティブ コマンドは、一般的に、エラーを示すことなく `stderr` に書き込みます。
  この変更によって `$?` が `$false` に設定されるのは、ネイティブ コマンドに 0 以外の終了コードが含まれている場合のみです。 この変更は、試験的機能 `PSNotApplyErrorActionToStderr` とは関係ありません。

- `$ErrorActionPreference` がネイティブ コマンドの `stderr` 出力に影響を与えないようになりました ([#13361](https://github.com/PowerShell/PowerShell/pull/13361))

  ネイティブ コマンドは、一般的に、エラーを示すことなく `stderr` に書き込みます。
  この変更があっても、`stderr` の出力は **ErrorRecord** オブジェクトで引き続きキャプチャされますが、**ErrorRecord** がネイティブ コマンドから取得された場合、ランタイムによって `$ErrorActionPreference` が適用されなくなります。

- Unix 時間を入力できるように、`Get-Date` で `-FromUnixTime` の名前が `-UnixTimeSeconds` に変更されました ([#13084](https://github.com/PowerShell/PowerShell/pull/13084)) (@aetos382 に感謝します)

  `-FromUnixTime` パラメーターが 7.1-preview.2 の間に追加されました。 パラメーターの名前が、よりデータ型と一致するように変更されました。 このパラメーターは、1970 年 1 月 1 日 0 時 00 分 00 秒以降の秒数を表す整数値を取ります。

  この例では、Unix 時間 (1970-01-01 0:00:00 以降の秒数で表されます) を DateTime に変換します。

  ```powershell
  Get-Date -UnixTimeSeconds 1577836800

  Wednesday, January 01, 2020 12:00:00 AM
  ```

- 明示的に指定された名前付きパラメーターで、ハッシュテーブル スプラッティングからの同じものを置き換えることができるようになりました (#13162)

  この変更により、スプラッティングからの名前付きパラメーターは、明示的に指定されたすべての名前付きパラメーターがバインドされた後にバインドされるように、パラメーター リストの末尾に移動されます。 単純な関数のパラメーター バインドでは、指定された名前付きパラメーターが見つからない場合のエラーはスローされません。 不明な名前付きパラメーターは、単純な関数の `$args` パラメーターにバインドされます。 スプラッティングを引数リストの末尾に移動すると、パラメーターが `$args` に表示される順序が変わります。

  以下に例を示します。

  ```powershell
  function SimpleTest {
      param(
          $Name,
          $Path
      )
      "Name: $Name; Path: $Path; Args: $args"
  }
  ```

  以前の動作では、**MyPath** が `-Path` にバインドされていません。これは、引数リストの 3 番目の引数であるためです。 ## そのため、`Blah = "World"` と共に '$args' に埋め込まれて終了します

  ```powershell
  PS> $hash = @{ Name = "Hello"; Blah = "World" }
  PS> SimpleTest @hash "MyPath"
  Name: Hello; Path: ; Args: -Blah: World MyPath
  ```

  この変更により、`@hash` の引数は引数リストの末尾に移動します。 **MyPath** がリストの最初の引数になるため、`-Path` にバインドされます。

  ```powershell
  PS> SimpleTest @hash "MyPath"
  Name: Hello; Path: MyPath; Args: -Blah: World
  ```

- `Split-Path` でスイッチ パラメーター `-Qualifier` が位置指定ではなくなりました ([#12960](https://github.com/PowerShell/PowerShell/pull/12960)) (@yecril71pl に感謝します)

- `Start-Process` で、作業ディレクトリが指定されていない場合はリテラル パスとして解決されるようになりました ([#11946](https://github.com/PowerShell/PowerShell/pull/11946)) (@NoMoreFood に感謝します)

- Web コマンドレットの `-OutFile` パラメーターが `-LiteralPath` のように機能するようになりました ([#11701](https://github.com/PowerShell/PowerShell/pull/11701)) (@iSazonov に感謝します)

- `BigInteger` 数値リテラルに対する文字列パラメーターのバインドが修正されました ([#11634](https://github.com/PowerShell/PowerShell/pull/11634)) (@vexx32 に感謝します)

- Windows では、`Start-Process` により現在のセッションからのすべての環境変数を使用してプロセス環境が作成され、`-UseNewEnvironment` を使用すると新しい既定のプロセス環境が作成されるようになりました ([#10830](https://github.com/PowerShell/PowerShell/pull/10830)) (@iSazonov に感謝します)

- `ScriptBlock` をデリゲートに変換するときに、`PSObject` の戻り値の結果が折り返されなくなりました ([#10619](https://github.com/PowerShell/PowerShell/pull/10619))

  `ScriptBlock` が C# コンテキストで使用されるデリゲート型に変換されるときに `PSObject` の結果を折り返すと、次のような望まない問題が生じます。

  - 値がデリゲートの戻り値の型に変換されると、基本的に `PSObject` の折り返しが解除されます。 そのため、`PSObject` は不要です。
  - デリゲートの戻り値の型が `object` の場合、`PSObject` で折り返され、C# コード内での操作が困難になります。

  この変更後、返されるオブジェクトが基になるオブジェクトになります。
