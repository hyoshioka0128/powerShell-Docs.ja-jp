---
title: PowerShell Core 6.2 の新機能
description: PowerShell Core 6.2 でリリースされた新機能と変更
ms.date: 03/28/2019
ms.openlocfilehash: 98dd97b064e11509bf97e68e0a312e6b34b5d2bc
ms.sourcegitcommit: f9d855dd73b916559a22e337672dea3fbb11c634
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96833814"
---
# <a name="whats-new-in-powershell-core-62"></a>PowerShell Core 6.2 の新機能

PowerShell Core 6.2 リリースでは、パフォーマンスの向上、バグ修正、品質を向上するためのコマンドレットと言語の軽微な強化に焦点を置いています。 すべての機能強化一覧については、GitHub で詳細な[変更ログ](https://github.com/PowerShell/PowerShell/releases)を参照してください。

## <a name="experimental-features"></a>試験的な機能

以前のバージョンで[試験的機能][]のサポートが有効になりました。 6\.2 リリースでは、4 つの試験的機能を試すことができます。その機能を改善し、主流の機能に昇格させる価値があるかどうかを判断できるように、フィードバックを提供してください。

使用できる試験的機能の一覧を取得するには、`Get-ExperimentalFeature` を使います。 これらの機能は、`Enable-ExperimentalFeature` および `Disable-ExperimentalFeature` で有効または無効にできます。

### <a name="command-not-found-suggestions"></a>コマンドが見つからない場合の候補

この機能では、あいまい一致を使用して、誤入力された可能性があるコマンドやコマンドレットの候補を見つけます。

```powershell
Enable-ExperimentalFeature -Name PSCommandNotFoundSuggestion
```

#### <a name="example"></a>例

この例では、スペルが正しくないコマンドレット名に対して、あいまい一致で可能性が高い候補から低い候補の順にいくつかの候補が検出されます。

```powershell
Get-Commnd
```

```Output
Get-Commnd : The term 'Get-Commnd' is not recognized as the name of a cmdlet, function, script file, or operable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ Get-Commnd
+ ~~~~~~~~~~
+ CategoryInfo          : ObjectNotFound: (Get-Commnd:String) [], CommandNotFoundException
+ FullyQualifiedErrorId : CommandNotFoundException


Suggestion [4,General]: The most similar commands are: Get-Command, Get-Content, Get-Job, Get-Module, Get-Event, Get-Host, Get-Member, Get-Item, Set-Content.
```

### <a name="implicit-remoting-batching"></a>暗黙的なリモート バッチ処理

パイプラインで[暗黙的なリモート処理](https://devblogs.microsoft.com/scripting/remoting-the-implicit-way/)を使用すると、PowerShell ではパイプライン内の各コマンドが個別に処理されます。 パイプラインの実行中、クライアントとリモート システムの間でオブジェクトのシリアル化と `de-serialized` が繰り返し行われます。

この機能により、PowerShell ではパイプラインが分析され、コマンドを実行しても安全であり、コマンドがターゲット システム上に存在するかどうかが判断されます。 true の場合、PowerShell ではパイプライン全体がリモートで実行され、クライアントに対して結果のシリアル化と `de-serializes` のみが行われます。

```powershell
Enable-ExperimentalFeature -Name PSImplicitRemotingBatching
```

localhost 上での `Get-Process | Sort-Object` の実際のテストは、10 - 15 秒から 20 - 30 **ミリ秒** に短縮されます。 この機能は、クライアント上でのみ有効にする必要があります。 サーバー上で変更の必要はありません。

### <a name="temp-drive"></a>一時ドライブ

```powershell
Enable-ExperimentalFeature -Name PSTempDrive
```

PowerShell Core をさまざまなオペレーティング システムで使用している場合、一時ディレクトリを見つけるための環境変数が Windows、macOS、Linux で異なることがわかります。 この機能を使用すると、`Temp:` という [PSDrive][] が自動的に使用しているオペレーティング システムの一時フォルダーにマップされます。

#### <a name="example"></a>例

```powershell
PS> "Hello World!" > Temp:/hello.txt
PS> Get-Content Temp:/hello.txt
Hello World!
```

ネイティブ ファイル コマンド (Linux 上の `ls` など) では PSDrive が認識されないため、この `Temp:` ドライブが表示されない点に注意してください。

### <a name="abbreviation-expansion"></a>短縮形の展開

PowerShell コマンドレットには、わかりやすい名詞が用意されています。 その結果、長い名前は入力がより困難になっています。 この機能を使用すると、コマンドレットの大文字を入力し、タブ補完を使用して一致を見つけることができます。

```powershell
Enable-ExperimentalFeature -Name PSUseAbbreviationExpansion
```

#### <a name="example"></a>例

```powershell
PS> i-arsavsf
```

Tab キーを押し、Azure PowerShell [Az](https://www.powershellgallery.com/packages/Az) モジュールをインストールしている場合は、次のようにオートコンプリートされます。

```Output
PS> Import-AzRecoveryServicesAsrVaultSettingsFile
```

> [!NOTE]
> この機能は、対話的に使用するためのものです。 短縮形のコマンドレットは実行できません。
> この機能はエイリアスに代わるものではありません。

## <a name="breaking-changes"></a>重大な変更

- Windows PowerShell に合わせて `Write-Output` の `-NoEnumerate` の動作を修正します。 (#9069)
- `Join-String -InputObject 1,2,3` の結果を `1,2,3 | Join-String` の結果と同じにします (#8611) (@sethvs に感謝)
- `-Stable` を `Sort-Object` と関連するテストに追加します (#7862) (@KirkMunro に感謝)
- 小数部の秒を受け入れるように `Start-Sleep` コマンドレットを改善します (#8537) (@Prototyyppi に感謝)
- すべてのカルチャで `case-insensitive` の OrdinalIgnoreCase を使用するようにハッシュテーブルを変更します (#8566)
- `Import-Csv` の **LiteralPath** を `Get-ChildItem` 出力にバインドするように修正します (#8277) (@iSazonov に感謝)
- `Import-Csv` で二重引用符の区切り記号が使用されている場合に、名前なしの列をスキップしなくなります (#7899) (@Topping に感謝)
- `Get-ExperimentalFeature` に `-ListAvailable` スイッチがなくなります (#8318)
- デバッグ パラメーターで、`$DebugPreference` が **Inquire** ではなく **Continue** に設定されるようになります (#8195) (@KirkMunro に感謝)
- pwsh で使用される非対話型のリダイレクトされ、エンコードされたコマンドで指定されている場合は、`-OutputFormat` を適用します (#8115)
- GAC から読み込む前に、モジュール ベース パスからアセンブリを読み込みます (#8073)
- Linux プレビュー パッケージからチルダを削除します (#8244)
- プロファイルの処理の前に `-WorkingDirectory` の処理を移動します (#8079)
- Unix 上では `PATHEXT` 環境変数を追加しません (#7697) (@iSazonov に感謝)

## <a name="known-issues"></a>既知の問題

- Windows IOT ARM プラットフォーム上のリモート処理には、モジュールの読み込みに関する問題があります。 (#8053) を参照してください

## <a name="general-updates-and-fixes"></a>一般的な更新と修正

- 大文字と小文字を区別するファイルシステム上で、ファイルとフォルダーの大文字と小文字を区別しないタブ補完を有効にします (#8128)
- PSVersionInfo.PSVersion と PSVersionInfo.PSEdition を公開します (#8054) (@KirkMunro に感謝)
- `catch{ }` ブロック内の `$_` / `$PSItem` に型の推定を追加します (#8020) (@vexx32 に感謝)
- 静的メソッド呼び出しの型の推定を修正します (#8018) (@SeeminglyScience に感謝)
- `Select-Object`、`Group-Object`、**PSObject**、**ハッシュテーブル** に推定された型を作成します (#7231) (@powercode に感謝)
- `ByRef-like` 型パラメーターを使用した呼び出し方法をサポートします (#7721)
- Windows PowerShell モジュールのパスが環境の PSModulePath に既に含まれている場合を処理します (#7727)
- プレーン テキストを格納することで非 Windows 向けに `SecureString` コマンドレットを有効にします (#9199)
- securestring を使用して clixml をインポートするときの非 Windows 上のエラー メッセージを改善します (#7997)
- パラメーター ReplyTo を `Send-MailMessage` に追加します (#8727) (@replicaJunction に感謝)
- 現在不使用のメッセージを `Send-MailMessage` に追加します (#9178)
- WinRM が存在しないときに `localhost` 上で動作するよう `Restart-Computer` を修正します (#9160)
- PowerShell のホスト中に `Start-Job` から終了エラーをスローするようにします (#9128)
- ushort、uint、ulong、short リテラルに C# スタイルの型のアクセラレータとサフィックスを追加します (#7813) (@vexx32 に感謝)
- 数値リテラルに新しいサフィックスを追加します - [about_Numeric_Literals][] を参照してください (#7901) (@vexx32 に感謝)
- SupportsShouldProcess が 'true' に設定されていない場合の影響レベルを正しく報告します (#8209) (@vexx32 に感謝)
- Web コマンドレットの要求文字セットの問題を修正しました (#8742) (@markekraus に感謝)
- Web コマンドレットの期待値 `100-continue` の問題を修正します (#8679) (@markekraus に感謝)
- Web コマンドレットに関するファイル ブロックの問題を修正します (#7676) (@Claustn に感謝)
- `Invoke-RestMethod` のコード ページ解析の問題を修正します (#8694) (@markekraus に感謝)
- JsonObject.ConvertToJson をパブリック API として公開するように `ConvertTo-Json` をリファクタリングします (#8682)
- -Depth を使用して構成できる最大深度を `ConvertFrom-Json` に追加します (#8199) (@louistio に感謝)
- `ConvertTo-Json` コマンドレットに EscapeHandling パラメーターを追加します (#7775) (@iSazonov に感謝)
- pwsh と `Enter-PSHostProcess` に `-CustomPipeName` を追加します (#8889)
- Windows 上で `New-Item` を使用して相対シンボリック リンクを作成できるようにします (#8783)
- 開発者モードの Windows ユーザーが昇格なしでシンボリック リンクを作成できるようにします (#8534)
- `Write-Information` で `$null` を受け付けるようにします (#8774)
- MAML ヘルプ コンテンツを含め、高度な関数の `Get-Help` を修正します (#8353)
- 1 つのパラメーターのみが宣言されいる場合の -Parameter に関する `Get-Help` PSTypeName の問題を修正します (#8754) (@pougetat に感謝)
- コメントのヘルプとして ScriptBlock で実行された `Get-Help` のトークン計算の修正。 (#8238) (@hubuk に感謝)
- 文字列配列を受け付けるように `Get-Help` コマンドレットの -Parameter パラメーターを変更します (#8454) (@sethvs に感謝)
- パスにスペースが含まれている場合は PAGER を解決します (#8571) (@pougetat に感謝)。
- 関数 'help' の `less` の使用に、ユーザーに終了方法を指示するプロンプトを追加します (#7998)
- `Format-Hex` コマンドレットに enum 型と char 型のサポートを追加します (#8191) (@iSazonov に感謝)
- `Format-Hex` から ShouldProcess を削除します (#8178)
- 新しい Offset および Count パラメーターを `Format-Hex` に追加し、コマンドレットをリファクターリングします (#7877) (@iSazonov に感謝)
- `ConvertTo-Html` で 'label' のエイリアス キーとして 'name' を許可し、'width' エントリを整数にすることを許可します (#8426) (@mklement0 に感謝)
- `ConvertTo-Html` でスクリプトブロック ベースの計算プロパティを再び機能するようにします (#8427) (@mklement0 に感謝)
- パイプライン入力からテキストを作成するコマンドレット `Join-String` を追加します (#7660) (@powercode に感謝)
- `Join-String` コマンドレットの FormatString パラメーターのロジックを修正します (#8449) (@sethvs に感謝)
- `$RAWUI` を使用するように `Clear-Host` を変更し、リモート処理のやり直しを許可します (#8609)
- `Clear-Host` を単に `[console]::clear` という名前に変更し、Unix から clear エイリアスを削除します (#8603)
- `Import-Csv` の LiteralPath を `Get-ChildItem` 出力にバインドするように修正します (#8277) (@iSazonov に感謝)
- AliasHelpInfo のヘルプ関数でポケットベルを使用しないようにします (#8552)
- トランスクリプト ヘッダーを最小化する `-UseMinimalHeader` を `Start-Transcript` に追加します (#8402) (@lukexjeremy に感謝)
- `Enable-ExperimentalFeature` および `Disable-ExperimentalFeature` コマンドレットを追加します (#8318)
- logman.exe を使用できる場合、**PSDiagnostics** のすべてのコマンドレットを公開します (#8366)
- `non-Windows` プラットフォームの `New-PSDrive` から **Persist** パラメーターを削除します (#8291) (@lukexjeremy に感謝)
- `cd +` のサポートを追加します (#7206) (@bergmeister に感謝)
- `Set-Location -LiteralPath` を - および+という名前のフォルダーで機能するようにします (#8089)
- 空または `$null` のパス値を指定すると、`Test-Path` は `$false` を返します (#8080) (@vexx32 に感謝)
- パスがどのプロバイダーと一致しない場合でも、動的パラメーターが返されるようにします (#7957)
- Unix プラットフォーム上で `Get-PSHostProcessInfo` と `Enter-PSHostProcess` をサポートします (#8232)
- `Get-Content` コマンドレットの割り当てを減らします (#8103) (@iSazonov に感謝)
- コンテンツの書き込み中に `Add-Content` が他のツールと読み取りアクセスを共有できるようにします (#8091)
- コンテナーをターゲットにするときに `Get/Add-Content` から返されるエラーを改善します (#7823) (@kvprasoon に感謝)
- `-Name`、`-NoUserOverrides`、`-ListAvailable` パラメーターを `Get-Culture` コマンドレットに追加します (#7702) (@iSazonov に感謝)
- **Encoding** パラメーターの補完用に統一された属性を追加します。 (#7732) (@ThreeFive-O に感謝)
- 数値 ID と登録済みコード ページの名前を **Encoding** パラメーターで許可します (#7636) (@iSazonov に感謝)
- ワイルドカード文字を使用する `Rename-Item -Path` を修正します (#7398) (@kwkam に感謝)
- `Start-Transcript` を使用してファイルが存在する場合、ファイルを削除するのではなく、ファイルを空にします (#8131) (@paalbra に感謝)
- 明示的に **FileAccess.Read** と **FileShare.Read** を指定して `Add-Type` をオープン ソース ファイルにします (#7915) (@IISResetMe に感謝)
- 最新の Windows に合わせて `Enter-PSSession -ContainerId` を修正します (#7883)
- **NestedModules** プロパティに `Test-ModuleManifest` が設定されるようにします (#7859)
- `Get-Date` -UFormat に `%F` のケースを追加します (#7630) (@britishben に感謝)
- 依存関係のあるサービスを停止するように `Set-Service -Status Stopped` を修正します (#5525) (@zhenggu に感謝)

<!-- Link references -->
[about_Numeric_Literals]: /powershell/module/Microsoft.PowerShell.Core/About/about_numeric_literals
[試験的機能]: /powershell/module/Microsoft.PowerShell.Core/About/about_Experimental_Features
[PSDrive]: /powershell/module/microsoft.powershell.management/new-psdrive
