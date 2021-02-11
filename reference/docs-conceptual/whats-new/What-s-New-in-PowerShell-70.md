---
title: PowerShell 7.0 の新機能
description: PowerShell 7.0 でリリースされた新機能と変更
ms.date: 03/04/2020
ms.openlocfilehash: 3a5a1aaa0bd9dd1e0df7d6f5e6021678ed21dce4
ms.sourcegitcommit: 39c2a697228276d5dae39e540995fa479c2b5f39
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93355274"
---
# <a name="whats-new-in-powershell-70"></a>PowerShell 7.0 の新機能

PowerShell 7.0 は、異種環境とハイブリッド クラウドを管理するために構築された、PowerShell のオープン ソースのクロスプラットフォーム (Windows、macOS、Linux) エディションです。

このリリースでは、次のような新機能がいくつか導入されました。

- パイプラインの並列化 (`ForEach-Object -Parallel` を使用)
- 新しい演算子:
  - 三項演算子: `a ? b : c`
  - パイプライン チェーン演算子: `||` と `&&`
  - null 条件演算: `??` と `??=`
- 簡略化された動的エラー ビューと `Get-Error` コマンドレット。エラーの調査が容易になります
- 暗黙的な Windows PowerShell セッションでのユーザーによるモジュール インポートを可能にする互換性レイヤー
- 新しいバージョンの自動通知
- PowerShell 7 からの DSC リソースの自動呼び出し機能 (試験段階)

機能と修正プログラムの完全な一覧については、[変更ログ](https://github.com/PowerShell/PowerShell/blob/master/CHANGELOG/7.0.md)をご覧ください。

## <a name="where-can-i-install-powershell"></a>PowerShell をインストールできる場所

現在 PowerShell 7 では、x64 上で次のオペレーティング システムがサポートされています。

- Windows 8.1、10
- Windows Server 2012、2012 R2、2016、2019
- macOS 10.13 以降
- Red Hat Enterprise Linux (RHEL)/CentOS 7
- Fedora 30 以降
- Debian 9
- Ubuntu LTS 16.04 以降
- Alpine Linux 3.8 以降

さらに、PowerShell 7.0 では、Debian、Ubuntu、ARM64 Alpine Linux の ARM32 および ARM64 フレーバーもサポートされています。

お使いのオペレーティング システム [Windows](/powershell/scripting/install/installing-powershell-core-on-windows)、[macOS](/powershell/scripting/install/installing-powershell-core-on-macos)、または [Linux](/powershell/scripting/install/installing-powershell-core-on-linux) のインストール手順を確認してください。

公式にはサポートされていませんが、コミュニティでは、[Arch](https://aur.archlinux.org/packages/powershell/) および Kali Linux 用のパッケージも提供しています。

> [!NOTE]
> 現在、Debian 10 および CentOS 8 では、WinRM リモート処理はサポートされません。 SSH ベースのリモート処理設定の詳細については、「[SSH 経由の PowerShell リモート処理](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)」を参照してください。

サポートされているオペレーティング システムとサポート ライフサイクルの最新情報については、[PowerShell サポート ライフサイクル](/powershell/scripting/powershell-support-lifecycle)に関するページをご覧ください。

## <a name="running-powershell-7"></a>PowerShell 7 の実行

PowerShell 7 は、Windows PowerShell とは別のディレクトリにインストールされます。
これにより、PowerShell 7 を Windows PowerShell 5.1 と共存させて実行することができます。 PowerShell Core 6.x がインストールされている場合、PowerShell 7 にインプレース アップグレードされ、PowerShell Core 6.x は削除されます。

- PowerShell 7 は `%programfiles%\PowerShell\7` にインストールされます
- `%programfiles%\PowerShell\7` フォルダーは `$env:PATH` に追加されます

PowerShell 7 のインストーラー パッケージは、以前のバージョンの PowerShell Core 6.x をアップグレードします。

- Windows 上の PowerShell Core 6.x: `%programfiles%\PowerShell\6` が `%programfiles%\PowerShell\7` に置き換えられます
- Linux: `/opt/microsoft/powershell/6` が `/opt/microsoft/powershell/7` に置き換えられます
- macOS: `/usr/local/microsoft/powershell/6` が `/usr/local/microsoft/powershell/7` に置き換えられます

> [!NOTE]
> Windows PowerShell では、PowerShell を起動する実行可能ファイルには `powershell.exe` という名前が付けられます。 この実行可能ファイルは、バージョン 6 以降では、side-by-side 実行をサポートするように名前変更されています。 PowerShell 7 を起動する新しい実行可能ファイルの名前は `pwsh.exe` です。 プレビュー ビルドは、7-preview ディレクトリに、`pwsh` ではなく `pwsh-preview` としてそのまま存在します。

## <a name="improved-backwards-compatibility-with-windows-powershell"></a>Windows PowerShell との下位互換性の向上

PowerShell 7.0 では .NET Core 3.1 への移行がマークされ、既存の Windows PowerShell モジュールとの下位互換性が大幅に向上しています。 これには、`Out-GridView`、`Show-Command` などの GUI 機能を必要とする Windows 上の多くのモジュール、および Windows の一部として出荷される多くの役割管理モジュールが含まれます。

Windows の場合、新しいスイッチ パラメーター **UseWindowsPowerShell** が `Import-Module` に追加されています。 このスイッチにより、PowerShell 7 にプロキシ モジュールが作成されます。このモジュールは、ローカルの Windows PowerShell プロセスを使用して、そのモジュールに含まれる任意のコマンドレットを暗黙的に実行します。 詳細については、「[Import-Module](/powershell/module/microsoft.powershell.core/import-module?view=powershell-7&preserve-view=true)」を参照してください。

PowerShell 7.0 で動作する Microsoft モジュールの詳細については、[モジュールの互換性に関する表](https://aka.ms/PSModuleCompat)をご覧ください。

## <a name="parallel-execution-added-to-foreach-object"></a>ForEach-Object に追加された並列実行

コレクション内の項目を反復する `ForEach-Object` コマンドレットに、新しい **Parallel** パラメーターを持つ組み込みの並列処理が追加されました。

既定では、並列スクリプト ブロックは、並列タスクを開始した呼び出し元の現在の作業ディレクトリを使用します。

この例は、ローカルの Windows マシンで、5 つのシステム ログから 50,000 個のログ エントリを取得しています。

```powershell
$logNames = 'Security','Application','System','Windows PowerShell','Microsoft-Windows-Store/Operational'

$logEntries = $logNames | ForEach-Object -Parallel {
    Get-WinEvent -LogName $_ -MaxEvents 10000
} -ThrottleLimit 5

$logEntries.Count

50000
```

**Parallel** パラメーターは、入力ログ名ごとに並列実行されるスクリプト ブロックを指定します。

新しい **ThrottleLimit** パラメーターによって、指定した時間に並列実行されるスクリプト ブロックの数が制限されます。 既定値は 5 です。

`$_` 変数を使用して、スクリプト ブロックで現在の入力オブジェクトを表すします。 `$using:` スコープを使用して、変数参照を実行中のスクリプト ブロックに渡します。

詳細については、「[ForEach-Object](/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7&preserve-view=true)」を参照してください。

## <a name="ternary-operator"></a>三項演算子

PowerShell 7.0 には、簡略化された `if-else` ステートメントのように動作する三項演算子が導入されています。
PowerShell の三項演算子は、C# 三項演算子構文に基づく類似のモデルです。

```
<condition> ? <if-true> : <if-false>
```

常に条件式が評価され、その結果は、次の評価対象となる分岐を判断するために **ブール値** に変換されます。

- `<if-true>` 式は、`<condition>` 式が true の場合に実行されます
- `<if-false>` 式は、`<condition>` 式が false の場合に実行されます

次に例を示します。

```powershell
$message = (Test-Path $path) ? "Path exists" : "Path not found"
```

この例では、パスが存在すると、 **Path exists** が表示されます。 パスが存在しない場合は、 **Path not found** が表示されます。

詳細については、「[About If](/powershell/module/microsoft.powershell.core/about/about_if)」を参照してください。

## <a name="pipeline-chain-operators"></a>パイプライン チェーン演算子

PowerShell 7 では、`&&` 演算子と `||` 演算子が実装され、条件に応じてパイプラインがチェーンされます。 これらの演算子は、PowerShell では "パイプライン チェーン演算子" として知られ、 **Bash** 、 **Zsh** などのシェルにある AND リストと OR リスト、および Windows コマンド シェル ( **cmd.exe** ) の条件付き処理シンボルと似ています。

`&&` 演算子では、左側のパイプラインが成功した場合に、右側のパイプラインが実行されます。 反対に、`||` 演算子では、左側のパイプラインが失敗した場合に、右側のパイプラインが実行されます。

> [!NOTE]
> これらの演算子では `$?` 変数と `$LASTEXITCODE` の変数を使用して、パイプラインが失敗したかどうかが判断されます。 これにより、コマンドレットや関数だけでなく、ネイティブ コマンドでも使用できます。

次の場合、最初のコマンドは成功し、2 番目のコマンドは実行されます。

```powershell
Write-Output 'First' && Write-Output 'Second'
```

```Output
First
Second
```

次の場合、最初のコマンドは失敗し、2 番目のコマンドは実行されません。

```powershell
Write-Error 'Bad' && Write-Output 'Second'
```

```Output
Write-Error: Bad
```

次の場合、最初のコマンドは成功し、2 番目のコマンドは実行されません。

```powershell
Write-Output 'First' || Write-Output 'Second'
```

```Output
First
```

次の場合、最初のコマンドは失敗し、2 番目のコマンドは実行されます。

```powershell
Write-Error 'Bad' || Write-Output 'Second'
```

```Output
Write-Error 'Bad'
Second
```

詳細については、「[パイプライン チェーン演算子の概要](/powershell/module/microsoft.powershell.core/about/about_pipeline_chain_operators?view=powershell-7&preserve-view=true)」を参照してください。

## <a name="null-coalescing-assignment-and-conditional-operators"></a>null 合体演算子、代入演算子、および条件演算子

PowerShell 7 には、null 合体演算子 `??`、null 条件付き代入演算子 `??=`、および null 条件メンバー アクセス演算子 `?.` および `?[]` があります。

### <a name="null-coalescing-operator-"></a>null 合体演算子 ??

null 合体演算子 `??` は、左側のオペランドが null でない場合に、左側のオペランドの値を返します。
それ以外の場合は、右側のオペランドを評価し、その結果を返します。 左側のオペランドが null 以外に評価された場合、`??` 演算子は右側のオペランドを評価しません。

```powershell
$x = $null
$x ?? 100
100
```

次の例では、右側のオペランドは評価されません。

```powershell
[string] $todaysDate = '1/10/2020'
$todaysDate ?? (Get-Date).ToShortDateString()
1/10/2020
```

### <a name="null-conditional-assignment-operator-"></a>null 条件付き代入演算子 ??=

null 条件付き代入演算子 `??=` は、左側のオペランドが null と評価された場合にのみ、右側のオペランドの値を左側のオペランドに代入します。 左側のオペランドが null 以外に評価された場合、`??=` 演算子は右側のオペランドを評価しません。

```powershell
$x = $null
$x ??= 100
$x
100
```

次の例では、右側のオペランドは評価されません。

```powershell
[string] $todaysDate = '1/10/2020'
$todaysDate ??= (Get-Date).ToShortDateString()
1/10/2020
```

### <a name="null-conditional-member-access-operators--and--experimental"></a>null 条件付きメンバー アクセス演算子 ?. および ?[] (試験段階)

> [!NOTE]
> これは **PSNullConditionalOperators** という試験的な機能です。 詳細については、「[試験的な機能の使用](/powershell/scripting/learn/experimental-features)」を参照してください。

null 条件演算子は、そのオペランドが null 以外に評価される場合にのみ、メンバー アクセス `?.`、または要素アクセス `?[]` をそのオペランドに許可します。それ以外の場合は、null を返します。

> [!NOTE]
> PowerShell では、変数名の一部として `?` が許可されるため、これらの演算子を使用するには、変数名の正式な仕様が必要です。 したがって、変数名は、`${a}` や、`?` が変数名 `${a?}` に含まれる場合のように、`{}` で囲む必要があります。

次の例では、メンバー プロパティ **Status** の値が返されます。

```powershell
$Service = Get-Service -Name 'bits'
${Service}?.status
Stopped
```

次の例では、メンバー名 **Status** へのアクセスを試みずに、null が返されます。

```powershell
$service = $Null
${Service}?.status
```

同様に、`?[]` を使用すると、要素の値が返されます。

```powershell
$a = 1..10
${a}?[0]
1
```

オペランドが null の場合、要素にはアクセスされず、null が返されます。

```powershell
$a = $null
${a}?[0]
```

詳細については、「[About_Operators](/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7&preserve-view=true)」を参照してください。

## <a name="new-view-conciseview-and-cmdlet-get-error"></a>新しい ConciseView ビューと Get-Error コマンドレット

PowerShell 7.0 には、対話型エラーとスクリプト エラーを見やすく表示する、エラー メッセージの表示が改善された新しい既定のビューの **ConciseView** があります。 ビューは、ユーザー設定変数 `$ErrorView` を使用してユーザーが選択できます。

**ConciseView** を使用すると、スクリプトのエラーまたはパーサー エラーでない場合、エラー メッセージは 1 行になります。

```powershell
Get-Childitem -Path c:\NotReal
```

```Output
Get-ChildItem: Cannot find path 'C:\NotReal' because it does not exist
```

スクリプトの実行中にエラーが発生した場合、または解析エラーの場合は、PowerShell によって複数行のエラー メッセージが返されます。そのエラー メッセージには、エラー、ポインター、およびエラーの場所を示すエラー メッセージが含まれています。 ターミナルで ANSI カラー エスケープ シーケンス (VT100) がサポートされていない場合、色は表示されません。

![スクリプトからのエラー表示](./media/What-s-New-in-PowerShell-70/myscript-error.png)

PowerShell 7 の既定のビューは **ConciseView** です。 以前の既定のビューの **NormalView** は、ユーザー設定変数 `$ErrorView` を設定して選択できます。

```powershell
$ErrorView = 'NormalView' # Sets the error view to NormalView
$ErrorView = 'ConciseView' # Sets the error view to ConciseView
```

> [!NOTE]
> エラー メッセージのアクセント カラーの変更をサポートするために、新しいプロパティ **ErrorAccentColor** が `$Host.PrivateData` に追加されています。

新しいコマンドレットである `Get-Error` では、必要に応じてエラーの詳細を完全に表示できます。
既定では、このコマンドレットによって、最後に発生したエラーの詳細 (内部例外を含む) が表示されます。

![Get-Error からの表示](./media/What-s-New-in-PowerShell-70/myscript-geterror.png)

`Get-Error` コマンドレットは、組み込み変数 `$Error` を使用して、パイプラインからの入力をサポートします。
`Get-Error` によって、パイプ処理されたすべてのエラーが表示されます。

```powershell
$Error | Get-Error
```

`Get-Error` コマンドレットでは、 **最新** のパラメーターがサポートされているため、現在のセッションから表示するエラーの数を指定できます。

```powershell
Get-Error -Newest 3 # Displays the lst three errors that occurred in the session
```

詳細については、「[Get-Error](/powershell/module/microsoft.powershell.utility/get-error?view=powershell-7&preserve-view=true)」を参照してください。

## <a name="new-version-notification"></a>新しいバージョンの通知

PowerShell 7 では、更新通知を使用して、PowerShell に更新プログラムがあることをユーザーに警告します。
PowerShell は、新しいバージョンが利用可能かどうかを、1 日に 1 回オンライン サービスに問い合わせます。

> [!NOTE]
> 更新チェックは、指定した 24 時間の期間の最初のセッション中に実行されます。 パフォーマンス上の理由から、更新チェックはセッションが開始してから 3 秒後に開始されます。 通知は、以降のセッションの開始時にのみ表示されます。

既定では、PowerShell は、そのバージョン/分岐に応じて、2 つの異なる通知チャネルのいずれかをサブスクライブします。 サポートされている一般公開 (GA) バージョンの PowerShell では、更新された GA リリースの通知のみが返されます。 プレビューおよびリリース候補 (RC) リリースでは、プレビュー、RC、GA リリースに対する更新プログラムが通知されます。

更新通知動作は、`$Env:POWERSHELL_UPDATECHECK` 環境変数を使用して変更できます。 サポートされている値を次に示します。

- **Default** は、`$Env:POWERSHELL_UPDATECHECK` を定義しない場合と同じです
  - GA リリースでは、GA リリースに対する更新プログラムが通知されます
  - プレビュー/RC リリースでは、GA およびプレビュー リリースに対する更新プログラムが通知されます
- **Off** により、更新通知機能が無効になります
- **LTS** は、長期サービス (LTS) GA リリースの更新についてのみ通知します

> [!NOTE]
> 環境変数 `$Env:POWERSHELL_UPDATECHECK` は、最初に設定されるまで存在しません。

`LTS` リリースについてのみバージョン通知を設定するには:

```powershell
$Env:POWERSHELL_UPDATECHECK = 'LTS'
```

バージョン通知を `Default` 動作に設定するには:

```powershell
$Env:POWERSHELL_UPDATECHECK = 'Default'
```

詳細については、「[更新通知](/powershell/module/microsoft.powershell.core/about/about_update_notifications)」を参照してください。

## <a name="new-dsc-resource-support-with-invoke-dscresource-experimental"></a>Invoke-DSCResource を使用した新しい DSC リソースのサポート (試験段階)

> [!NOTE]
> これは **PSDesiredStateConfiguration.InvokeDscResource** という試験的な機能です。 詳細については、「[試験的な機能の使用](/powershell/scripting/learn/experimental-features)」を参照してください。

`Invoke-DscResource` コマンドレットは、指定された PowerShell Desired State Configuration (DSC) リソースのメソッドを実行します。

このコマンドレットは、構成ドキュメントを作成せずに、DSC リソースを直接呼び出します。 このコマンドレットを使用すると、構成管理製品で DSC リソースを使って Windows または Linux を管理できます。 DSC エンジンの実行中、デバッグが有効になっている場合は、このコマンドレットでリソースのデバッグを有効にすることもできます。

このコマンドは、 **WindowsProcess** という名前のリソースの **Set** メソッドを呼び出し、指定された Windows プロセスを開始するために必須の **Path** プロパティと **Arguments** プロパティを指定します。

```powershell
Invoke-DscResource -Name WindowsProcess -Method Set -ModuleName PSDesiredStateConfiguration -Property @{
  Path = 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe'
  Arguments = ''
}
```

詳細については、「[Invoke-DSCResource](/powershell/module/psdesiredstateconfiguration/invoke-dscresource?view=powershell-7&preserve-view=true)」を参照してください。

## <a name="breaking-changes-and-improvements"></a>破壊的変更と機能強化

### <a name="breaking-changes"></a>重大な変更

- 更新通知での LTS と既定のチャネルをサポートを追加 (#11132)
- Windows PowerShell の Test-Connection と同じように動作するよう Test-Connection を更新 (#10697) (@vexx32 に感謝)
- 次の式に対して $? を保持: ParenExpression、SubExpression、および ArrayExpression (#11040)
- 作業ディレクトリを Start-Job の現在のディレクトリに設定 (#10920) (@iSazonov に感謝)
- $PSCulture にセッション内カルチャの変更を一貫して反映 (#10138) (@iSazonovに感謝)

### <a name="engine-updates-and-fixes"></a>エンジンの更新と修正

- リモート シナリオのブレークポイント API の機能強化 (#11312)
- 別の実行空間にリークしている PowerShell クラス定義を修正 (#11273)
- 7\.0.0-Preview1 に追加された FirstOrDefault プリミティブによって発生する書式設定の回帰を修正 (#11258)
- PS7 テレメトリで追跡する追加の Microsoft モジュール (#10751)
- 承認済み機能を試験的ではない機能として指定 (#11303)
- TargetObject を使用するように ConciseView を更新 (該当する場合) (#11075)
- CompletionCompleters パブリック メソッドの NullReferenceException を修正 (#11274)
- Windows 以外のプラットフォームでのアパートメント スレッド状態チェックを修正 (#11301)
- プロセスとマシンの環境変数を連結するように設定 PSModulePath を更新 (#11276)
- .NET Core を 3.1.0 にバージョンアップ (#11260)
- $env:PATH の前にある $PSHOME の検出を修正 (#11141)
- pwsh による $env:PSModulePath の継承を許可し、powershell.exe の適切な起動を実現 (#11057)
- .NET Core 3.1 プレビュー 1 への移行 (#10798)
- ファイル システムプロバイダーでの再解析タグ チェックをリファクター (#10431) (@iSazonov に感謝)
- スクリプト ログ記録で CR と改行を 0x23CE 文字に置換 (#10616)
- AppDomain.CurrentDomain.ProcessExit から イベントハンドラーの登録を解除して、リソース リークを修正 (#10626)
- ActionPreference.Break にサポートを追加して、デバッグ、エラー、情報、進行状況、詳細、または警告メッセージの生成時にデバッガーを起動 (#8205) (@KirkMunroに感謝)
- CPL 拡張機能を指定せずに、PowerShell Core 内でコントロール パネルのアドイン起動を有効化 (#9828)
- -split 演算子で負の数値をサポート (#8960) (@ece-jacob-scott に感謝)

### <a name="general-cmdlet-updates-and-fixes"></a>一般的なコマンドレットの更新と修正

- UnixStat の試験的な機能のファイル変更日を設定するための Raspbian に関する問題を修正(#11313)
- -AsPlainText を ConvertFrom-SecureString に追加 (#11142)
- WinCompat の WindowsPS バージョン チェックを追加 (#11148)
- 一部の WinCompat シナリオでのエラー報告を修正 (#11259)
- ネイティブ バイナリ リゾルバー (#11032) を追加 (@iSazonov に感謝)
- CJK 文字が正しく考慮されるよう文字幅の計算を更新 (#11262)
- macOS 用の Unblock-File を追加 (#11137)
- Get-PSCallStack の回帰を修正 (#11210) (@iSazonov に感謝)
- Job コマンドレットを使用しているときの ScheduledJob モジュールの自動読み込みを削除 (#11194)
- OutputType を Get-Error コマンドレットに追加し、元の typenames を保持 (#10856)
- SupportsVirtualTerminal プロパティの null 参照を修正 (#11105)
- Get-WinEvent (#10648) で制限チェックを追加 (@iSazonov に感謝)
- StopUpstreamCommandsException が -ErrorVariable に設定されないようにコマンド ランタイムを修正 (#10840)
- 出力エンコードを、ネイティブ コマンド用の [Console]:: OutputEncoding に設定 (#10824)
- 例で使用されている複数行のコード ブロックをサポート (#10776) (@Greg-Smulko に感謝)
- Culture パラメーターを Select-String コマンドレットに追加 (#10943) (@iSazonov に感謝)
- 末尾に円記号が付いている Start-Job 作業ディレクトリ パスを修正 (#11041)
- ConvertFrom-Json: 既定でコレクションの折り返しを解除 (#10861) (@danstur に感謝)
- 大文字と小文字を区別するハッシュテーブルを、CaseSensitive スイッチおよび -AsHashtable スイッチと共に Group-Object コマンドレットに対して使用 (#11030) (@vexx32 に感謝)
- 大文字と小文字が正しく区別されるようにパスを再構築するとき、ファイル列挙が失敗した場合に例外を処理 (#11014)
- myCommand ではなく Activity が表示されるように ConciseView を修正 (#11007)
- Web コマンドレットが HTTP エラー状態を無視することを許可 (#10466) (@vdamewood に感謝)
- Get-Command に対する複数の CommandInfo のパイプ処理を修正 (#10929)
- Windows 用 Get Counter コマンドレットを再度追加 (#10933)
- ConvertTo-Json が [AutomationNull]::Value と [NullString]::Value を $null として処理するように指定 (#10957)
- SSH リモート処理の ipv6 アドレスから角かっこを削除 (#10968)
- pwsh に送信されたコマンドが空白の場合に発生するクラッシュを修正 (#10977)
- クロスプラットフォームの Get-Clipboard と Set-Clipboard を追加 (#10340)
- ファイルシステム オブジェクトの元のパスに、余分な末尾のスラッシュが付かないように修正 (#10959)
- ConvertTo-Json で $null をサポート (#10947)
- Windows で Out-Printer コマンドを再度追加 (#10906)
- 空白が含まれる Start-Job -WorkingDirectory の開始を修正 (#10951)
- PSConfiguration.cs の設定に対して null を取得するときに既定値を返す (#10963) (@iSazonov に感謝)
- IO 例外を終了しない例外として処理 (#10950)
- GraphicalHost アセンブリを追加して、Out-GridView、Show-Command、および Get-Help -ShowWindow を有効化 (#10899)
- Get-HotFix のパイプラインを使用して ComputerName を取得 (#10852) (@kvprasoon に感謝)
- 共通パラメーターが使用可能として表示されるようにパラメーターのタブ補完を修正 (#10850)
- First() を呼び出す前に、システム ファイル エントリが返されていないかどうかを最初に確認するように GetCorrectCasedPath() を修正 (#10930)
- 作業ディレクトリを Start-Job の現在のディレクトリに設定 (#10920) (@iSazonov に感謝)
- TabExpansion2 を、-CursorColumn を必要としないように変更し、$InputScript.Length として処理 (#10849)
- ホストが画面の行または列を返さないかもしれないケースを処理 (#10938)
- アクセント カラーをサポートしていないホストについて、そのアクセント カラーの使用を修正 (#10937)
- Update-List コマンドを再度追加 (#10922)
- Clear-RecycleBin の FWLink ID を更新 (#10925)
- タブ補完中、ファイル属性を読み取ることができない場合はファイルをスキップ (#10910)
- Windows 用 Clear-RecycleBin を再度追加 (#10909)
- `$env:__SuppressAnsiEscapeSequences` を追加して VT エスケープ シーケンスを出力に含めるかどうかを制御 (#10814)
- -NoEmphasize パラメーターを追加して、Select-String 出力を色分け (#8963) (@derek-xia に感謝)
- Get-HotFix コマンドレットを再度追加 (#10740)
- PowerShell をホストするアプリケーションで Add-Type を使用可能に指定 (#10587)
- LanguagePrimitives.IsNullLike() でより効果的な評価順序を使用 (#10781) (@vexx32 に感謝)
- パイプ処理された mixed-collection 入力と Format-Hex のパイプ処理された入力ストリームの処理を改善 (#8674) (@vexx32 に感謝)
- 想定されている型と値が一致しないときに SSHConnection ハッシュテーブルの型変換を使用 (#10720) (@SeeminglyScience に感謝)
- -TotalCount が設定されているときに Get-Content -ReadCount 0 動作を修正 (#10749) (@eugenesmlv に感謝)
- Get-WinEvent でアクセス拒否エラー メッセージを言い換え (#10639) (@iSazonov に感謝)
- 列挙型または型制約の変数代入に対してタブ補完を有効化 (#10646)
- 書式設定の問題を引き起こしている未使用の SourceLength リモート処理プロパティを削除 (#10765)
- -Delimiter パラメーターを ConvertFrom-StringData に追加 (#10665) (@steviecoaster に感謝)
- SSH で Invoke-Command を使用しているときに ScriptBlock の位置指定パラメーターを追加 (#10721) (@machgo に感謝)
- ConciseView で複数行なのにスクリプト名がない場合に行のコンテキスト情報を表示 (#10746)
- \\wsl$\ パスのサポートを ファイル システム プロバイダーに追加 (#10674)
- パーサーで TokenKind.QuestionMark に不足しているトークン テキストを追加 (#10706)
- 呼び出しスクリプトの結果として同じ場所へのスクリプトを実行している各 ForEach-Object -Parallel に現在の作業ディレクトリを設定 (#10672)
- FindFirstStreamW と FindNextStreamW API について api-ms-win-core-file-l1-2-2.dll を Kernell32.dll に置換 (#10680) (@iSazonov に感謝)
- StrictMode 耐性を高めるためにヘルプの書式設定スクリプトを調整 (#10563)
- -SecurityDescriptorSDDL パラメーターを New-Service に追加 (#10483) (@kvprasoon に感謝)
- 情報出力を削除し、Test-Connection で ping の使用を統合 (#10478) (@vexx32 に感謝)
- 再解析ポイントをアクセスしないで読み取る (#10662) (@iSazonovに感謝)
- Clear-Host をターミナルに直接出力 (#10681) (@iSazonov に感謝)
- Format-Table と -Property を使用してグループ化のために改行を再度追加 (#10653)
- Get-Random で -InputObject から [ValidateNotNullOrEmpty] を削除して空の文字列を許可 (#10644)
- 提案システム文字列の距離アルゴリズムで大文字と小文字を区別しない (#10549) (@iSazonovに感謝)
- ForEach-Object -Parallel 入力処理の null 参照例外を修正 (#10577)
- PowerShell Core グループ ポリシー定義を追加 (#10468)
- 構成可能性のシナリオで使用される XTPUSHSGR/XTPOPSGR VT 制御シーケンスをサポートするようにコンソール ホストを更新 (#10208)
- WorkingDirectory パラメーターを Start-Job に追加 (#10324) (@davinci26 に感謝)
- ブレークポイントの変更がホスト実行空間デバッガーに誤ってレプリケートされる原因となっていたイベント ハンドラーを削除 (#10503) (@KirkMunroに感謝)
- Microsoft.PowerShell.Commands.NativeMethods P/Invoke API で api-ms-win-core-job-12-1-0.dll を Kernell32.dll に置換 (#10417) (@iSazonov に感謝)
- 変数代入および -OutVariable で New-Service の誤った出力を修正 (#10444) (@kvprasoon に感謝)
- 終了コード、コマンド ライン パラメーター、およびスペースを含むパスに関するグローバル ツールの問題を修正 (#10461)
- OneDrive への再帰を修正 - SafeFindHandle 型 を使用するように FindFirstFileEx() を変更(#10405)
- NVDA スクリーン リーダーがアクティブの場合、Windows で PSReadLine の自動読み込みをスキップ (#10385)
- 組み込み PowerShell モジュールを 7.0.0.0 にバージョンアップ (#10356)
- 同じ名前の型が既に存在する場合の Add-Type でのエラーのスローを追加 (#9609) (@iSazonov に感謝)

### <a name="performance"></a>パフォーマンス

- Parser.SaveError でのクロージャの使用を回避 (#11006)
- 新しい Regex インスタンス作成時のキャッシュを改善 (#10657) (@iSazonov に感謝)
- types.ps1xml、typesV3.ps1xml、および GetEvent.types.ps1xml からの PowerShell 組み込み型データの処理を改善 (#10898)
- PSConfiguration.ReadValueFromFile を更新して、高速化とメモリ効率の改善を実現 (#10839)
- 実行空間の初期化のパフォーマンスを若干改善 (#10569) (@iSazonov に感謝)
- 一般的に使用されるシナリオの ForEach-Object を高速化 (#10454)。また、多数の実行空間によって ForEach-Object -Parallel のパフォーマンスに関する問題を修正 (#10455)

### <a name="code-cleanup"></a>コードのクリーンアップ

- Microsoft 標準に合わせてコメントおよび要素テキストを変更 (#11304)
- Compiler.cs のスタイルに関する問題をクリーンアップ (#10368) (@iSazonov に感謝)
- CommaDelimitedStringCollection の未使用の型コンバーターを削除 (#11000) (@iSazonov に感謝)
- InitialSessionState.cs のスタイルをクリーンアップ (#10865) (@iSazonov に感謝)
- PSSession クラスのコードをクリーンアップ (#11001)
- 動作していない "Get-Help の初回実行時に Get-Help から Update-Help を実行する" 機能を削除 (#10974)
- スタイルの問題を修正 (#10998) (@iSazonov に感謝)
- クリーンアップ: 組み込み型エイリアスを使用 (#10882) (@iSazonov に感謝)
- 使用されていない設定キー ConsolePrompting を削除し、ExecutionPolicy 設定にクエリを実行するときの不要な文字列作成を回避 (#10985)
- デイリー ビルドに対する更新通知チェックを無効化 (#10903) (@bergmeister に感謝)
- #10338 で失われたデバッグ API を復帰 (#10808)
- 使用されなくなった WorkflowJobSourceAdapter 参照を削除 (#10326) (@KirkMunro に感謝)
- PreserveSig 属性を修正して、ジャンプ リスト コードの COM インターフェイスをクリーンアップ (#9899) (@weltkante に感謝)
- Add a comment describing why -ia が -InformationAction 共通パラメーターのエイリアスではない理由を説明するコメントを追加 (#10703) (@KirkMunro に感謝)
- InvokeCommandCmdlet.cs を InvokeExpressionCommand.cs に名前変更(#10659) (@kilasuit に感謝)
- 更新通知に関連する小さなコード クリーンアップを追加 (#10698)
- 非推奨のワークフロー ロジックをリモート処理設定スクリプトから削除 (#10320) (@KirkMunro に感謝)
- 適切なケースを使用するようにヘルプ形式を更新 (#10678) (@tnieto88 に感謝)
- 先月のコミットで発生する CodeFactor スタイルの問題をクリーンアップ (#10591) (@iSazonov に感謝)
- 試験的な PSTernaryOperator 機能の説明の入力ミスを修正 (#10586) (@bergmeister に感謝)
- ActionPreference.Suspend 列挙値をサポートされていない予約済み状態に変換し、ユーザー設定変数で ActionPreference.Ignore を使用するときの制限を削除 (#10317) (@KirkMunro に感謝)
- 機能を変更することなくコードの読みやすさと信頼性を高めるために ArrayList を List\<T> に置換 (#10333) (@iSazonov に感謝)
- TestConnectionCommand へのコード スタイルの修正 (#10439) (@vexx32 に感謝)
- AutomationEngine をクリーンアップし、余分な SetSessionStateDrive メソッド呼び出しを削除 (#10416) (@iSazonov に感謝)
- 既定の ParameterSetName の名前を ConvertTo-Csv および ConvertFrom-Csv の Delimiter に再度変更 (#10425)

### <a name="tools"></a>ツール

- VS でビルドが行われるように SDKToUse プロパティの既定の設定を追加 (#11085)
- Install-Powershell.ps1: MSI インストールが使用されるようにパラメーターを追加 (#10921) (@MJECloud に感謝)
- install-powershell.ps1 の基本的な例を追加 (#10914) (@kilasuit に感謝)
- Install-PowerShellRemoting.ps1 による PowerShellHome パラメーターの空の文字列の処理を実現 (#10526) (@Orca88 に感謝)
- Install-powershell.sh で /etc/lsb-release から /etc/os-release に切り替え (#10773) (@Himura2la に感謝)
- Windows で pwsh.exe と pwsh のデイリー バージョンを確認 (#10738) (@centreboard に感謝)
- installpsh-osx.sh で不要なタップを削除 (#10752)
- 既にインストールされているデイリー ビルド チェックのために install-powershell.ps1 を更新 (#10489)

### <a name="tests"></a>テスト

- 信頼性の低い DSC テストを保留 (#11131)
- ハッシュテーブルのキーが正しく検証されるように stringdata テストを修正 (#10810)
- テスト モジュールをアンロード (#11061) (@iSazonov に感謝)
- URL テストの再試行間隔を長くする (#11015)
- テスト アクションが正確に記述されるようにテストを更新 (#10928) (@romero126 に感謝)
- 不安定なテスト TestAppDomainProcessExitEvenHandlerNotLeaking を一時的にスキップ (#10827)
- イベント ハンドラー リーク テストの安定化 (#10790)
- CI YAML の大文字と小文字の区別を同期 (#10767) (@RDIL に感謝)
- イベント ハンドラー リーク修正のテストを追加 (#10768)
- Get-ChildItem テストを追加 (#10507) (@iSazonov に感謝)
- 正確性を高めるためにテストのあいまいな言語をスイッチからパラメーターに置換 (#10666) (@romero126 に感謝)
- 試験的なチェックを ForEach-Object -Parallel テストに追加 (#10354) (@KirkMunro に感謝)
- Alpine 検証のテストを更新 (#10428)

### <a name="build-and-package-improvements"></a>ビルドとパッケージの機能強化

- 調整済みパッケージ ビルドの Nuget パッケージ署名を修正 (#11316)
- PowerShell ギャラリーと NuGet からの依存関係を更新 (#11323)
- Microsoft.ApplicationInsights を 2.11.0 から 2.12.0 にバージョンアップ (#11305)
- Microsoft.CodeAnalysis.CSharp を 3.3.1 から 3.4.0 にバージョンアップ (#11265)
- Debian 10 および 11 のパッケージを更新 (#11236)
- RC より前の試験的な機能のみを有効化 (#11162)
- macOS の最小バージョンを更新 (#11163)
- NJsonSchema を 10.0.27 から 10.0.28 にバージョンアップ (#11170)
- Preview.5 の README.md および metadata.json のリンクを更新 (#10854)
- PowerShell 所有のコンプライアンス テストのファイルを選択 (#10837)
- win7x86 msix パッケージによるビルドを許可 (内部 10515)
- セマンティク バージョンを NormalizeVersion 関数に渡すことを許可 (#11087)
- .NET Core Framework を 3.1-preview.3 にバージョンアップ (#11079)
- /src/Modules で PSReadLine を 2.0.0-beta5 から 2.0.0-beta6 にバージョンアップ (#11078)
- Newtonsoft.Json を 12.0.2 から 12.0.3 にバージョンアップ (#11037) (#11038)
- Debian 10、11、および CentOS 8 パッケージを追加 (#11028)
- ReleaseDate フィールドを含む Build-Info Json ファイルをアップロード (#10986)
- .NET Core Framework を 3.1-preview.2 にバージョンアップ (#10993)
- x86 MSIX パッケージのビルドを有効化 (#10934)
- build.psm1 の dotnet SDK インストール スクリプト URL を更新 (#10927)
- Markdig.Signed を 0.17.1 から 0.18.0 にバージョンアップ (#10887)
- ThreadJob を 2.0.1 から 2.0.2 にバージョンアップ (#10886)
- MS Store の要件を満たすように AppX マニフェストおよびパッケージ モジュールを更新 (#10878)
- PowerShell SDK のパッケージ参照を preview.5 に更新 (内部 10295)
- ThirdPartyNotices.txt を更新 (#10834)
- Microsoft.PowerShell.Native を 7.0.0-preview.3 にバージョンアップ (#10826)
- Microsoft.ApplicationInsights を 2.10.0 から 2.11.0 にバージョンアップ (#10608)
- NJsonSchema を 10.0.24 から 10.0.27 にバージョンアップ (#10756)
- MacPorts サポートをビルド システムに追加 (#10736) (@Lucius-Q-User に感謝)
- PackageManagement を 1.4.4 から 1.4.5 にバージョンアップ (#10728)
- NJsonSchema を 10.0.23 から 10.0.24 にバージョンアップ (#10635)
- MSI でクライアント/サーバーのテレメトリを区別できるように環境変数を追加 (#10612)
- PSDesiredStateConfiguration を 2.0.3 から 2.0.4 にバージョンアップ (#10603)
- Microsoft.CodeAnalysis.CSharp を 3.2.1 から 3.3.1 にバージョンアップ (#10607)
- .Net Core 3.0 RTM に更新 (#10604) (@bergmeister に感謝)
- Windows ストアの要件に対応するように MSIX パッケージを更新 (#10588)
- PowerShellGet を 2.2 から 2.2.1 にバージョンアップ (#10382)
- PackageManagement を 1.4.3 から 1.4.4 にバージョンアップ (#10383)
- 7\.0.0-preview.4 の README.md および metadata.json を更新 (内部 10011)
- .Net Core 3.0 バージョンを Preview 9 から RC1 にアップグレード (#10552) (@bergmeister に感謝)
- ExperimentalFeature リスト生成を修正 (内部 9996)
- PSReadLine を 2.0.0-beta4 から 2.0.0-beta5 にバージョンアップ (#10536)
- リリース タグが設定されるようにリリース ビルド スクリプトを修正
- Microsoft.PowerShell.Native のバージョンを 7.0.0-preview.2 に更新 (#10519)
- Netcoreapp3.0 preview9 をアップグレード (#10484) (@bergmeister に感謝)
- 調整済みデイリー ビルドがデイリー ビルドであることを確実に認識 (#10464)
- デイリー ビルドをリリースするように結合パッケージ ビルドを更新 (#10449)
- appveyor 参照を削除 (#10445) (@RDIL に感謝)
- NJsonSchema を 10.0.22 から 10.0.23 にバージョンアップ (#10421)
- Alpine の依存関係の一部で必要とされるため linux-x64 ビルド フォルダーの削除を削除 (#10407)

### <a name="documentation-and-help-content"></a>ドキュメントとヘルプ コンテンツ

- 変更ログをリリースごとに 1 つのログにリファクター (#11165)
- PowerShell 7 オンライン ヘルプ ドキュメントの FWLink を修正 (#11071)
- CONTRIBUTING.md を更新 (#11096) (@mklement0 に感謝)
- README.md のインストール ドキュメントのリンクを修正 (#11083)
- install-powershell.ps1 スクリプトに例を追加 (#11024) (@kilasuit に感謝)
- CHANGELOG.md の Select-String 強調および Import-DscResource への修正 (#10890)
- powershell-beginners-guide.md から古いリンクを削除 (#10926)
- 安定したサービス変更ログの統合 (#10527)
- ビルド ドキュメントの使用されている .NET バージョンを更新 (#10775) (@Greg-Smulko に感謝)
- powershell-beginners-guide.md で MSDN のリンクを docs.microsoft.com に置換 (#10778) (@iSazonov に感謝)
- 破損した DSC 概要リンクを修正 (#10702)
- Support_Question.md を更新して、Stack Overflow に別のコミュニティ リソースとしてリンク (#10638) (@mklement0 に感謝)
- プロセッサ アーキテクチャをディストリビューション要求テンプレートに追加 (#10661)
- 新しい PowerShell MoL ブックを学習用の PowerShell ドキュメントに追加 (#10602)
- v6.1.6 および v6.2.3 リリースの README.md およびメタデータを更新 (#10523)
- README.md の入力ミスを修正 (#10465) (@vedhasp に感謝)
- PSKoans モジュールへの参照を学習用のリソース ドキュメントに追加 (#10369) (@vexx32 に感謝)
- 7\.0.0-preview.3 の README.md および metadata.json を更新 (#10393)
