---
ms.date: 02/03/2020
keywords: powershell、core
title: PowerShell Core 6.0 の重要な変更
description: この記事では、Windows PowerShell 5.1 と PowerShell 6.0 の違いについてまとめます。
ms.openlocfilehash: b53365ee72e6a6e85faa56125a8aa7961d3ecc7f
ms.sourcegitcommit: f9d855dd73b916559a22e337672dea3fbb11c634
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96833778"
---
# <a name="breaking-changes-for-powershell-6x"></a>PowerShell 6.x の破壊的変更

## <a name="features-no-longer-available-in-powershell-core"></a>PowerShell Core で使用できなくなった機能

### <a name="modules-not-shipped-for-powershell-6x"></a>PowerShell 6.x に付属しないモジュール

互換性に関するさまざまな理由から、次のモジュールは PowerShell 6 には含まれません。

- ISE
- Microsoft.PowerShell.LocalAccounts
- Microsoft.PowerShell.ODataUtils
- Microsoft.PowerShell.Operation.Validation
- PSScheduledJob
- PSWorkflow
- PSWorkflowUtility

### <a name="powershell-workflow"></a>PowerShell ワークフロー

[PowerShell ワークフロー][workflow]は、実行時間の長い、または並列化されたタスクための堅牢な Runbook の作成を可能にする [Windows Workflow Foundation (WF)][workflow-foundation] の上に構築される Windows PowerShell の機能です。

.NET Core では Windows Workflow Foundation がサポートされていないため、PowerShell Core では PowerShell ワークフローはサポートされていません。

今後、PowerShell ワークフローを必要としない、PowerShell 言語でのネイティブの並列処理とコンカレンシーが可能になる予定です。

OS の再起動後にチェックポイントを使ってスクリプトを再開する必要がある場合は、タスク スケジューラを使って OS の起動時にスクリプトを実行することをお勧めします。ただし、そのスクリプトでそれ自体の状態を維持する必要があります (ファイルに保持するなど)。

[workflow]: /previous-versions/powershell/scripting/components/workflows-guide
[workflow-foundation]: /dotnet/framework/windows-workflow-foundation/

### <a name="custom-snap-ins"></a>カスタム スナップイン

[PowerShell スナップイン][snapin]は、PowerShell モジュールの前身ですが、PowerShell コミュニティではあまり使用されていません。

スナップインのサポートは複雑であり、コミュニティ内での普及率が低いことから、PowerShell Core でのカスタム スナップインのサポートを終了します。

現時点では、Windows および Windows Server 内の `ActiveDirectory` および `DnsClient` モジュールに影響します。

[snapin]: /powershell/module/microsoft.powershell.core/about/about_pssnapins

### <a name="wmi-v1-cmdlets"></a>WMI v1 コマンドレット

WMI ベースの 2 つのモジュール セットをサポートすることは複雑になるため、PowerShell Core から WMI v1 コマンドレットが削除されました。

- `Register-WmiEvent`
- `Set-WmiInstance`
- `Invoke-WmiMethod`
- `Get-WmiObject`
- `Remove-WmiObject`

これに代わり、新しい機能や再設計された構文を含み、同じ機能を提供する CIM (別名 WMI v2) コマンドレットを使用することをお勧めします。

- `Get-CimAssociatedInstance`
- `Get-CimClass`
- `Get-CimInstance`
- `Get-CimSession`
- `Invoke-CimMethod`
- `New-CimInstance`
- `New-CimSession`
- `New-CimSessionOption`
- `Register-CimIndicationEvent`
- `Remove-CimInstance`
- `Remove-CimSession`
- `Set-CimInstance`

### <a name="microsoftpowershelllocalaccounts"></a>Microsoft.PowerShell.LocalAccounts

サポートされていない API を使用していることから、より優れたソリューションが提供されるまで、PowerShell Core から `Microsoft.PowerShell.LocalAccounts` コマンドレットを削除します。

### <a name="new-webserviceproxy-cmdlet-removed"></a>`New-WebServiceProxy` コマンドレットが削除された

.NET Core では、SOAP プロトコルを使用するためのサービスを提供する Windows Communication Framework はサポートされていません。 このコマンドレットは、SOAP を必要とするため削除されました。

### <a name="-transaction-cmdlets-removed"></a>`*-Transaction` コマンドレットの削除

以下のコマンドレットの使用方法は非常に限られていました。 これらのサポートを中止することが決定されました。

- `Complete-Transaction`
- `Get-Transaction`
- `Start-Transaction`
- `Undo-Transaction`
- `Use-Transaction`

### <a name="security-cmdlets-not-available-on-non-windows-platforms"></a>Windows 以外のプラットフォームで使用できないセキュリティ コマンドレット

- `Get-Acl`
- `Set-Acl`
- `Get-AuthenticodeSignature`
- `Set-AuthenticodeSignature`
- `Get-CmsMessage`
- `Protect-CmsMessage`
- `Unprotect-CmsMessage`
- `New-FileCatalog`
- `Test-FileCatalog`

### <a name="-computerand-other-windows-specific-cmdlets"></a>`*-Computer` およびその他の Windows 専用コマンドレット

以下のコマンドレットは、サポートされていない API が使われているため、より優れたソリューションが提供されるまで PowerShell Core から削除されます。

- `Get-Clipboard`
- `Set-Clipboard`
- `Add-Computer`
- `Checkpoint-Computer`
- `Remove-Computer`
- `Restore-Computer`
- `Reset-ComputerMachinePassword`
- `Disable-ComputerRestore`
- `Enable-ComputerRestore`
- `Get-ComputerRestorePoint`
- `Test-ComputerSecureChannel`
- `Get-ControlPanelItem`
- `Show-ControlPanelItem`
- `Get-HotFix`
- `Clear-RecycleBin`
- `Update-List`
- `Out-Printer`
- `ConvertFrom-String`
- `Convert-String`

### <a name="-counter-cmdlets"></a>`*-Counter` コマンドレット

サポートされていない API を使用していることから、より優れたソリューションが提供されるまで、PowerShell Core から `*-Counter` コマンドレットを削除します。

### <a name="-eventlog-cmdlets"></a>`*-EventLog` コマンドレット

サポートされていない API を使用しているため、より優れたソリューションが提供されるまで、PowerShell Core から `*-EventLog` コマンドレットを削除しています 。 `Get-WinEvent` と `New-WinEvent` を使用すると、Windows でイベントを取得および作成することができます。

### <a name="cmdlets-that-use-wpf-removed"></a>WPF を使用するコマンドレットの削除

Windows Presentation Framework は、CoreCLR ではサポートされていません。 以下のコマンドレットが影響を受けます。

- `Show-Command`
- `Out-GridView`
- `Get-Help` の **showwindow** パラメーター

### <a name="some-dsc-cmdlets-removed"></a>一部の DSC コマンドレットの削除

- `Get-DscConfiguration`
- `Publish-DscConfiguration`
- `Restore-DscConfiguration`
- `Start-DscConfiguration`
- `Stop-DscConfiguration`
- `Test-DscConfiguration`
- `Update-DscConfiguration`
- `Remove-DscConfigurationDocument`
- `Get-DscConfigurationStatus`
- `Disable-DscDebug`
- `Enable-DscDebug`
- `Get-DscLocalConfigurationManager`
- `Set-DscLocalConfigurationManager`
- `Invoke-DscResource`

## <a name="enginelanguage-changes"></a>エンジンおよび言語の変更

### <a name="rename-powershellexe-to-pwshexe-5101"></a>`powershell.exe` から  への名前変更 `pwsh.exe` [#5101](https://github.com/PowerShell/PowerShell/issues/5101)

ユーザーが (Windows PowerShell ではなく) Windows 上で PowerShell Core を確実な方法で呼び出せるよう、PowerShell Core のバイナリが Windows では `pwsh.exe` に、Windows 以外のプラットフォームでは `pwsh` に変更されました。

短い形式の名前も、Windows 以外のプラットフォーム上のシェルの名前形式と同じになるように設定されています。

### <a name="dont-insert-line-breaks-to-output-except-for-tables-5193"></a>出力 (テーブル以外) に改行を挿入できない。[#5193](https://github.com/PowerShell/PowerShell/issues/5193)

以前は、コンソールの幅に合わせて出力が調整され、コンソールの幅の終端に改行が追加されていたため、ターミナルのサイズが変更されると、想定外の形式になることがありました。 今回の変更は、列の整列に改行が必要なため、テーブルには適用されていませんでした。

### <a name="skip-null-element-check-for-collections-with-a-value-type-element-type-5432"></a>要素の型が値型のコレクションの null 要素チェックを省略 [#5432](https://github.com/PowerShell/PowerShell/issues/5432)

`Mandatory`パラメーターと `ValidateNotNull` および `ValidateNotNullOrEmpty` 属性について、コレクションの要素の型が値型の場合に null 要素チェックを省略します。

### <a name="change-outputencoding-to-use-utf-8-nobom-encoding-rather-than-ascii-5369"></a>ASCIIではなく `UTF-8 NoBOM` エンコードを使用するよう `$OutputEncoding` を変更 [#5369](https://github.com/PowerShell/PowerShell/issues/5369)

以前のエンコード、ASCII (7 ビット) を使用すると、出力が不適切に変更される場合があります。 今回の変更では、`UTF-8 NoBOM` が既定値となり、ほとんどのツールおよびオペレーティング システムでサポートされているエンコードを使用して Unicode 出力を保持します。

### <a name="remove-allscope-from-most-default-aliases-5268"></a>大部分の既定エイリアスから `AllScope` を削除 [#5268](https://github.com/PowerShell/PowerShell/issues/5268)

スコープの作成を高速化するために、大部分の既定エイリアスから `AllScope` が削除されました。 検索が高速化できる使用頻度の高いいくつかのエイリアスでは `AllScope` のままになっています。

### <a name="-verbose-and--debug-no-longer-overrides-erroractionpreference-5113"></a>`-Verbose` および `-Debug` による  のオーバーライドの終了 `$ErrorActionPreference` [#5113](https://github.com/PowerShell/PowerShell/issues/5113)

以前は、`-Verbose` または `-Debug` が指定された場合、それにより `$ErrorActionPreference` の動作がオーバーライドされていました。 今回の変更により、`-Verbose` および `-Debug` が `$ErrorActionPreference` の動作に影響を与えることはなくなります。

## <a name="cmdlet-changes"></a>コマンドレットの変更

### <a name="invoke-restmethod-doesnt-return-useful-info-when-no-data-is-returned-5320"></a>データが返されない場合、Invoke-RestMethod により、有益な情報が返されない [#5320](https://github.com/PowerShell/PowerShell/issues/5320)

API が `null` のみを返す場合、Invoke-restmethod は、これを `$null` ではなく、文字列 `"null"` としてシリアル化していました。 今回の変更により、有効なシングル値 JSON `null` がリテラル `$null` として正しくシリアル化されるよう、`Invoke-RestMethod` のロジックが修正されました。

### <a name="remove--protocol-from--computer-cmdlets-5277"></a>`*-Computer` コマンドレットから `-Protocol` を削除 [#5277](https://github.com/PowerShell/PowerShell/issues/5277)

CoreFX の RPC リモート処理 (特に Windows 以外のプラットフォームで) の問題と、PowerShell で一貫性のあるリモート処理エクスペリエンスを保証するため、`\*-Computer` コマンドレットから `-Protocol` パラメーターが削除されました。 リモート処理では DCOM がサポートされなくなりました。 次のコマンドレットでは、WSMAN リモート処理のみをサポートします。

- Rename-Computer
- Restart-Computer
- Stop-Computer

### <a name="remove--computername-from--service-cmdlets-5090"></a>`*-Service` コマンドレットから `-ComputerName` を削除 [#5090](https://github.com/PowerShell/PowerShell/issues/5094)

一貫性のある PSRP の使用を促進するために、`*-Service` コマンドレットから `-ComputerName` パラメーターが削除されました。

### <a name="fix-get-item--literalpath-ab-if-ab-doesnt-actually-exist-to-return-error-5197"></a>`a*b` が実際に存在しない場合は、エラーを返すよう `Get-Item -LiteralPath a*b` を修正 [#5197](https://github.com/PowerShell/PowerShell/issues/5197)

以前は、ワイルドカードを渡された `-LiteralPath` は `-Path` と同様に処理され、ワイルドカードを使ってファイルが検出されない場合は、何も返さず終了していました。 正しい動作として、ファイルが存在しない場合はエラーを返すには、`-LiteralPath` はリテラルである必要があります。 `-Literal` にワイルドカードを使用した場合、リテラルとして処理するよう変更されました。

### <a name="import-csv-should-apply-pstypenames-upon-import-when-type-information-is-present-in-the-csv-5134"></a>インポート時に CSV に型情報が存在する場合、`Import-Csv` が `PSTypeNames` を適用 [#5134](https://github.com/PowerShell/PowerShell/issues/5134)

以前は、`ConvertFrom-Csv` を使用してインポートされた `TypeInformation` を含む `Export-CSV` を使用してオブジェクトをエクスポートすると、型情報が保持されませんでした。 今回の変更により、CSV ファイルから使用可能な場合は `PSTypeNames` メンバーに型情報が追加されます。

### <a name="-notypeinformation-should-be-default-on-export-csv-5131"></a>`-NoTypeInformation` の既定値を  に設定 `Export-Csv` [#5131](https://github.com/PowerShell/PowerShell/issues/5131)

お客様からの声を反映して、`Export-CSV` の既定の動作として型情報を含めるよう変更されました。

以前は、コマンドレットから、オブジェクトの型名を含む最初の行としてコメントが出力されていました。 ほとんどのツールでは認識されないため、既定ではこれを抑制されるよう変更されています。 以前の動作のままにするには、`-IncludeTypeInformation` を使用してください。

### <a name="web-cmdlets-should-warn-when--credential-is-sent-over-unencrypted-connections-5112"></a>暗号化されていない接続経由で `-Credential` が送信された場合に、Web コマンドレットが警告 [#5112](https://github.com/PowerShell/PowerShell/issues/5112)

HTTP を使用する場合、パスワードなどのコンテンツはクリア テキストとして送信されます。 今回の変更は、これを既定で許可しないようにするもので、資格情報が安全でない方法で渡されると、エラーを返します。 `-AllowUnencryptedAuthentication` スイッチを使用すると、この動作を無効にできます。

## <a name="api-changes"></a>API の変更

### <a name="remove-addtypecommandbase-class-5407"></a>`AddTypeCommandBase` クラスを削除 [#5407](https://github.com/PowerShell/PowerShell/issues/5407)

パフォーマンス向上のため、`Add-Type` から `AddTypeCommandBase` クラスが削除されました。 このクラスは、Add-Type コマンドレットでのみ使用されており、ユーザーに影響はありません。

### <a name="unify-cmdlets-with-parameter--encoding-to-be-of-type-systemtextencoding-5080"></a>コマンドレットと `-Encoding` パラメーターを  型に統合 `System.Text.Encoding` [#5080](https://github.com/PowerShell/PowerShell/issues/5080)

`-Encoding` の値 `Byte`はファイルシステム プロバイダーのコマンドレットから削除されました。 新しいパラメーター `-AsByteStream` を使用して、入力としてバイト ストリームが必要なこと、あるいは出力がバイト ストリームであることを指定してください。

### <a name="add-better-error-message-for-empty-and-null--uformat-parameter-5055"></a>`-UFormat` パラメーターが空または null の場合に、改善されたエラー メッセージを追加 [#5055](https://github.com/PowerShell/PowerShell/issues/5055)

以前は、`-UFormat` に空の形式の文字列が渡されると、有益でないエラー メッセージが表示されていました。 よりわかりやすいエラーが追加されました。

### <a name="clean-up-console-code-4995"></a>コンソール コードをクリーンアップ [#4995](https://github.com/PowerShell/PowerShell/issues/4995)

`-psconsolefile` スイッチおよびコード、`-importsystemmodules` スイッチおよびコード、およびフォントの変更コードは、レガシーの Windows PowerShell のために存在し、PowerShell Core でサポートされておらず、今後もサポートを追加する予定がないため、削除されました。

### <a name="removed-runspaceconfiguration-support-4942"></a>`RunspaceConfiguration` のサポートを終了 [#4942](https://github.com/PowerShell/PowerShell/issues/4942)

以前は、API を使用してプログラムで PowerShell 実行空間を作成する場合、従来の [`RunspaceConfiguration`][runspaceconfig] か新しい [`InitialSessionState`][iss] を使用できました。 今回の変更により `RunspaceConfiguration` のサポートが終了し、`InitialSessionState` のみがサポートされます。

[runspaceconfig]: /dotnet/api/system.management.automation.runspaces.runspaceconfiguration
[iss]: /dotnet/api/system.management.automation.runspaces.initialsessionstate

### <a name="commandinvocationintrinsicsinvokescript-bind-arguments-to-input-instead-of-args-4923"></a>`CommandInvocationIntrinsics.InvokeScript` が引数を  ではなく `$input` にバインド `$args` [#4923](https://github.com/PowerShell/PowerShell/issues/4923)

引数内のパラメーターの位置が正しくないため、引数ではなく入力として渡されていました。

### <a name="remove-unsupported--showwindow-switch-from-get-help-4903"></a>サポートされていなかった `-showwindow` スイッチを  から削除 `Get-Help` [#4903](https://github.com/PowerShell/PowerShell/issues/4903)

`-showwindow` では、CoreCLR でサポートされていない、WPF を使用します。

### <a name="allow--to-be-used-in-registry-path-for-remove-item-4866"></a> のレジストリ パスで * が使用可能に `Remove-Item` [#4866](https://github.com/PowerShell/PowerShell/issues/4866)

以前は、ワイルドカードを渡された `-LiteralPath` は `-Path` と同様に処理され、ワイルドカードを使ってファイルが検出されない場合は、何も返さず終了していました。 正しい動作として、ファイルが存在しない場合はエラーを返すには、`-LiteralPath` はリテラルである必要があります。 `-Literal` にワイルドカードを使用した場合、リテラルとして処理するよう変更されました。

### <a name="fix-set-service-failing-test-4802"></a>テストで失敗していた `Set-Service` を修正 [#4802](https://github.com/PowerShell/PowerShell/issues/4802)

以前は、`New-Service -StartupType foo` を使用している場合、`foo` は無視され、既定のスタートアップ タイプを使用して、サービスが作成されていました。 今回の変更により、無効なスタートアップ タイプの場合は、エラーが明示的にスローされます。

### <a name="rename-isosx-to-ismacos-4700"></a>`$IsOSX` の名前を  に変更 `$IsMacOS` [#4700](https://github.com/PowerShell/PowerShell/issues/4700)

PowerShell の名前付けが Microsoft の名前規則に準拠し、OSX ではなく Apple の macOS が使用する名前付け規則に準拠している必要があります。 ただし、読みやすさと整合性の目的で、引き続き Pascal の形式に従っています。

### <a name="make-error-message-consistent-when-invalid-script-is-passed-to--file-better-error-when-passed-ambiguous-argument-4573"></a>無効なスクリプトがファイルに渡されたときに一貫性のあるエラー メッセージを生成。あいまいな引数 が渡されたときのエラー表示を改善 [#4573](https://github.com/PowerShell/PowerShell/issues/4573)

`pwsh.exe` の終了コードを Unix の規則と一致するよう変更

### <a name="removal-of-localaccount-and-cmdlets-from--diagnostics-modules-4302-4303"></a>`Diagnostics` モジュールから `LocalAccount` およびコマンドレットを削除 [#4302](https://github.com/PowerShell/PowerShell/issues/4302) [#4303](https://github.com/PowerShell/PowerShell/issues/4303)

API がサポートされていないため、改善されたソリューションが発表されるまで、`LocalAccounts` モジュールと `Diagnostics` モジュールの `Counter`コマンドレットが削除されました。

### <a name="executing-powershell-script-with-bool-parameter-does-not-work-4036"></a>ブール値のパラメーターを使用する PowerShell スクリプトが正しく動作しない [#4036](https://github.com/PowerShell/PowerShell/issues/4036)

以前は、**powershell.exe** (現在は **pwsh.exe**) で `-File` を使用して PowerShell スクリプトを実行する場合、`$true`/`$false` をパラメーター値として渡すことができませんでした。 解析された値としての `$true`/`$false` パラメーターのサポートが追加されました。 現在、ドキュメントに記載されている構文が機能しないため、スイッチの値もサポートされます。

### <a name="remove-clrversion-property-from-psversiontable-4027"></a> から `ClrVersion` プロパティを削除 `$PSVersionTable` [#4027](https://github.com/PowerShell/PowerShell/issues/4027)

`$PSVersionTable` の `ClrVersion` プロパティは CoreCLR では有用でないため、エンドユーザーは、互換性を判定するためにこの値を使用しないでください。

### <a name="change-positional-parameter-for-powershellexe-from--command-to--file-4019"></a>`powershell.exe` の位置指定パラメーターを `-Command` から  に変更 `-File` [#4019](https://github.com/PowerShell/PowerShell/issues/4019)

Windows 以外のプラットフォームでの PowerShell のシバン使用を有効化します。 Unix ベースのシステムでは、`pwsh` を明示的に呼び出すのではなく、自動的に PowerShell を起動するスクリプト実行可能ファイルを作成できます。 また、`-File` を指定せずに `powershell foo.ps1` または `powershell fooScript` のようなコマンドを実行できます。 ただし、今回の変更後、`powershell.exe Get-Command` などのコマンドを実行しようとする場合は `-c` または `-Command` を明示的に指定する必要があります。

### <a name="implement-unicode-escape-parsing-3958"></a>Unicode エスケープ解析の実装 [#3958](https://github.com/PowerShell/PowerShell/issues/3958)

`` `u####`` または `` `u{####}`` は対応する Unicode 文字に変換されます。 `` `u`` リテラルを出力するには、``` ``u``` のように、グラーブ文字をエスケープします。

### <a name="change-new-modulemanifest-encoding-to-utf8nobom-on-non-windows-platforms-3940"></a>Windows 以外のプラットフォームの `New-ModuleManifest` エンコーディングを `UTF8NoBOM` に変更 [#3940](https://github.com/PowerShell/PowerShell/issues/3940)

以前は、`New-ModuleManifest` は、BOM を使用して UTF-16 で psd1 マニフェストを作成しており、Linux ツールで問題が発生していました。 今回の重要な変更により、Windows 以外のプラットフォーム上の エンコーディング `New-ModuleManifest` が UTF (BOM なし) に変更されました。

### <a name="prevent-get-childitem-from-recursing-into-symlinks-1875-3780"></a>`Get-ChildItem` のシンボリック リンクへ反復を防止 (#1875) [#3780](https://github.com/PowerShell/PowerShell/issues/3780)

今回の変更により、`Get-ChildItem` と Unix の `ls -r` や Windows の `dir /s` ネイティブ コマンドとの連携度が向上しました。 上記のコマンドと同様、このコマンドレットでは、反復中に特定されたディレクトリへのシンボリック リンクが表示されますが、反復はされません。

### <a name="fix-get-content--delimiter-to-not-include-the-delimiter-in-the-returned-lines-3706"></a>返された行に区切り文字が含まれないように `Get-Content -Delimiter` を修正 [#3706](https://github.com/PowerShell/PowerShell/issues/3706)

以前は、`Get-Content -Delimiter` を使用したときの出力に一貫性がなく、区切り記号を削除するために追加のデータ処理が必要になり不便でした。 今回の変更により、返される行の区切り記号が削除されます。

### <a name="implement-format-hex-in-c-3320"></a>Format-hex を C# で実装 [#3320](https://github.com/PowerShell/PowerShell/issues/3320)

`-Raw` パラメーターは "no-op" になりました (なにも動作はしません)。 今後は、その種類の数字のすべてのバイトを含め、数字がすべての出力に忠実に表示されます (今回の変更前は、`-Raw` パラメーターが正式にこの動作を実行していました)。

### <a name="powershell-as-a-default-shell-doesnt-work-with-script-command-3319"></a>既定シェルの PowerShell でスクリプト コマンドが正しく動作しない [#3319](https://github.com/PowerShell/PowerShell/issues/3319)

Unix では、対話型のシェルで `-i` の入力を受け付けることが通常で、多くのツールでこの動作が前提とされ、スイッチ `-i` を使ってシェルを呼び出します (たとえば `script` や、PowerShell が既定のシェルに設定されている場合)。 以前は `-inputformat` と一致する便利な方法として `-i` を使用できましたが、この重要な変更後は `-in` を使用する必要があります。

### <a name="typo-fix-in-get-computerinfo-property-name-3167"></a>Get-computerinfo プロパティ名の入力ミスを修正 [#3167](https://github.com/PowerShell/PowerShell/issues/3167)

`BiosSerialNumber` が `BiosSeralNumber` と誤記されていましたが、正しいスペルに変更されました。

### <a name="add-get-stringhash-and-get-filehash-cmdlets-3024"></a>`Get-StringHash` および `Get-FileHash` コマンドレットを追加 [#3024](https://github.com/PowerShell/PowerShell/issues/3024)

CoreFX でサポートされていないハッシュ アルゴリズムがありましたが、今回の変更により、使用できなくなりました。

- `MACTripleDES`
- `RIPEMD160`

### <a name="add-validation-on-get--cmdlets-where-passing-null-returns-all-objects-instead-of-error-2672"></a>$null を渡した場合、エラーではなくすべてのオブジェクトが返されるよう、`Get-*` コマンドレットに検証機能を追加 [#2672](https://github.com/PowerShell/PowerShell/issues/2672)

次のコマンドに `$null` を渡すと、エラーがスローされるよう変更されました。

- `Get-Credential -UserName`
- `Get-Event -SourceIdentifier`
- `Get-EventSubscriber -SourceIdentifier`
- `Get-Help -Name`
- `Get-PSBreakpoint -Script`
- `Get-PSProvider -PSProvider`
- `Get-PSSessionConfiguration -Name`
- `Get-PSSnapin -Name`
- `Get-Runspace -Name`
- `Get-RunspaceDebug -RunspaceName`
- `Get-Service -Name`
- `Get-TraceSource -Name`
- `Get-Variable -Name`
- `Get-WmiObject -Class`
- `Get-WmiObject -Property`

### <a name="add-support-w3c-extended-log-file-format-in-import-csv-2482"></a> に W3C 拡張ログ ファイル形式のサポートを追加 `Import-Csv` [#2482](https://github.com/PowerShell/PowerShell/issues/2482)

以前は、`Import-Csv` コマンドレットを使用して、W3C 拡張ログ形式でログ ファイルを直接インポートできず、追加の操作が必要でした。 今回の変更により、W3C 拡張ログ形式がサポートされます。

### <a name="parameter-binding-problem-with-valuefromremainingarguments-in-ps-functions-2035"></a>PS 関数の `ValueFromRemainingArguments` でのパラメーター バインドの問題 [#2035](https://github.com/PowerShell/PowerShell/issues/2035)

`ValueFromRemainingArguments` は、それ自体が配列である単一の値ではなく、値を配列として返すよう変更されました。

### <a name="buildversion-is-removed-from-psversiontable-1415"></a> を `BuildVersion` から削除 `$PSVersionTable` [#1415](https://github.com/PowerShell/PowerShell/issues/1415)

`$PSVersionTable` から `BuildVersion` プロパティを削除してください。 このプロパティは、Windows のビルド バージョンと関連付けられていました。 代わりに、`GitCommitId` を使用して、PowerShell Core の正確なビルド バージョンを取得することをお勧めします。

### <a name="changes-to-web-cmdlets"></a>Web のコマンドレットへの変更

Web コマンドレットの基となる .NET API が `System.Net.Http.HttpClient` に変更されました。 この変更は、多くのメリットを提供します。 ただし、この変更と、Internet Explorer での相互運用性がないことを理由として、`Invoke-WebRequest` と `Invoke-RestMethod`に重要な変更が加えられました。

- `Invoke-WebRequest` では、基本的な HTML 解析のみがサポートされます。 `Invoke-WebRequest` は常に `BasicHtmlWebResponseObject` オブジェクトを返します。 `ParsedHtml` および `Forms` プロパティが削除されました。
- `BasicHtmlWebResponseObject.Headers` の値は `String` から `String[]` に変更されました。
- `BasicHtmlWebResponseObject.BaseResponse` は、`System.Net.Http.HttpResponseMessage` オブジェクトに変更されました。
- Web コマンドレットの例外の `Response` プロパティが、`System.Net.Http.HttpResponseMessage` オブジェクトに変更されました。
- `-Headers` および `-UserAgent` パラメーターの既定設定が、Strict RFC ヘッダー解析に変更されました。 これは、`-SkipHeaderValidation` を使用して無効にできます。
- `file://` および `ftp://` URI スキームのサポートは終了しました。
- `System.Net.ServicePointManager` 設定は使用できなくなります。
- 現在、macOS では、証明書を使用した認証は使用できません。
- `http://` URI 経由で `-Credential` を使用するとエラーになります。 `https://` URI を使用するか、または `-AllowUnencryptedAuthentication`パラメーターを指定してエラーを回避してください。
- `-MaximumRedirection` では、指定された制限を超えようとすると、最後のリダイレクトの結果を返す代わりに、終了エラーが生成されるようになりました。
- PowerShell 6.2 では、JSON 応答に対して規定値が UTF-8 エンコードになるように変更が加えられました。 JSON 応答に文字セットが指定されていない場合、既定のエンコードは RFC 8259 に従って UTF-8 にする必要があります。
