---
ms.date: 12/14/2020
title: PowerShell の試験的機能の使用
description: 現在使用できる試験的機能とその使用方法を示します。
ms.openlocfilehash: be02829c27ff5d8babaf173d2ee7ebbfc7614773
ms.sourcegitcommit: 04faa7dc1122bce839295d4891bd8b2f0ecb06ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879356"
---
# <a name="using-experimental-features-in-powershell"></a>PowerShell の試験的機能の使用

PowerShell の試験的機能のサポートは、試験的機能を PowerShell または PowerShell モジュールの既存の安定した機能と共存させるためのメカニズムを提供します。

試験的機能は、設計が未完成です。 この機能は、ユーザーがテストしてフィードバックを提供するために使用できます。 試験的機能が完成すると、設計の変更が破壊的変更になります。

> [!CAUTION]
> 試験的機能は、変更が破壊的になる可能性があるため、運用環境で使用することは想定されていません。 試験的機能は正式にはサポートされていません。 しかし、フィードバックとバグ レポートは歓迎いたします。 [GitHub ソース リポジトリ](https://github.com/PowerShell/PowerShell/issues/new/choose)でイシューを送信できます。

これらの機能を有効または無効にする方法の詳細については、「[about_Experimental_Features](/powershell/module/microsoft.powershell.core/about/about_experimental_features)」を参照してください。

## <a name="available-features"></a>利用可能な機能

この記事では、使用可能な試験的機能と、その機能の使用方法について説明します。

|                            名前                            |   6.2   |   7.0   |   7.1   |   7.2   |
| ---------------------------------------------------------- | :-----: | :-----: | :-----: | :-----: |
| PSTempDrive (PS 7.0 以降のメインストリーム)                        | &check; |         |         |         |
| PSUseAbbreviationExpansion (PS 7.0 以降のメインストリーム)         | &check; |         |         |         |
| PSNullConditionalOperators (PS 7.1 以降のメインストリーム)         |         | &check; |         |         |
| PSUnixFileStat (Windows 以外のみ - PS 7.1 以降のメインストリーム)  |         | &check; |         |         |
| PSCommandNotFoundSuggestion                                | &check; | &check; | &check; | &check; |
| PSImplicitRemotingBatching                                 | &check; | &check; | &check; | &check; |
| Microsoft.PowerShell.Utility.PSManageBreakpointsInRunspace |         | &check; | &check; | &check; |
| PSDesiredStateConfiguration.InvokeDscResource              |         | &check; | &check; | &check; |
| PSNativePSPathResolution                                   |         |         | &check; | &check; |
| PSCultureInvariantReplaceOperator                          |         |         | &check; | &check; |
| PSNotApplyErrorActionToStderr                              |         |         | &check; | &check; |
| PSSubsystemPluginModel                                     |         |         | &check; | &check; |
| PSAnsiRendering                                            |         |         |         | &check; |

## <a name="microsoftpowershellutilitypsmanagebreakpointsinrunspace"></a>Microsoft.PowerShell.Utility.PSManageBreakpointsInRunspace

PowerShell 7.0 では、この試験的機能によって、`Debug-Runspace` および `Debug-Job` コマンドレットの **BreakAll** パラメーターが有効になり、デバッガーをアタッチするときに PowerShell を現在の場所ですぐに中断するかどうかをユーザーが決定できます。

PowerShell 7.1 では、この試験的機能によって、`*-PSBreakpoint` コマンドレットに **Runspace** パラメーターも追加されます。

- `Disable-PSBreakpoint`
- `Enable-PSBreakpoint`
- `Get-PSBreakpoint`
- `Remove-PSBreakpoint`
- `Set-PSBreakpoint`

**Runspace** パラメーターによって、**Runspace** オブジェクトを指定し、指定した実行空間内のブレークポイントと対話できます。

```powershell
Start-Job -ScriptBlock {
    Set-PSBreakpoint -Command Start-Sleep
    Start-Sleep -Seconds 10
}

$runspace = Get-Runspace -Id 1

$breakpoint = Get-PSBreakPoint -Runspace $runspace
```

この例では、ジョブが開始され、`Set-PSBreakPoint` の実行時に中断するようにブレークポイントが設定されます。 実行空間は変数に格納され、**Runspace** パラメーターを使用して `Get-PSBreakPoint` コマンドに渡されます。 その後、`$breakpoint` 変数内のブレークポイントを調べることができます。

## <a name="psansirendering"></a>PSAnsiRendering

この実験は、PowerShell 7.2 で追加されました。 この機能により、PowerShell エンジンがテキストを出力し、`$PSStyle` 自動変数を追加し、文字列出力の ANSI 表示を制御する方法が変更されます。

```powershell
PS> $PSStyle

Name            MemberType Definition
----            ---------- ----------
Reset           Property   string AttributesOff {get;set;}
Background      Property   System.Management.Automation.PSStyle+BackgroundColor Background {get;set;}
Blink           Property   string Blink {get;set;}
BlinkOff        Property   string BlinkOff {get;set;}
Bold            Property   string Bold {get;set;}
BoldOff         Property   string BoldOff {get;set;}
Foreground      Property   System.Management.Automation.PSStyle+ForegroundColor Foreground {get;set;}
Formatting      Property   System.Management.Automation.PSStyle+FormattingData Formatting {get;set;}
Hidden          Property   string Hidden {get;set;}
HiddenOff       Property   string HiddenOff {get;set;}
OutputRendering Property   System.Management.Automation.OutputRendering OutputRendering {get;set;}
Reverse         Property   string Reverse {get;set;}
ReverseOff      Property   string ReverseOff {get;set;}
Italic          Property   string Standout {get;set;}
ItalicOff       Property   string StandoutOff {get;set;}
Underline       Property   string Underlined {get;set;}
Underline Off   Property   string UnderlinedOff {get;set;}
```

基本メンバーは、名前にマップされた ANSI エスケープ シーケンスの文字列を返します。 値は、カスタマイズできるように設定できます。

詳細については、「[about_Automatic_Variables](/reference/7.2/Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください

> [!NOTE]
> C# の開発者は、シングルトンとして `PSStyle` にアクセスできます。 このようにして使用します。
>
> ```csharp
> string output = $"{PSStyle.Instance.Foreground.Red}{PSStyle.Instance.Bold}Hello{PSStyle.Instance.Reset}";
> ```
>
> `PSStyle` は、System.Management.Automation 名前空間に存在します。

これにより、`$PSStyle` ヘのアクセスと共に、PowerShell エンジンに対する変更が導入されます。 PowerShell の書式設定システムが、`$PSStyle.OutputRendering` を考慮するように更新されます。

- `StringDecorated` 型が、ANSI エスケープ文字列を処理するために追加されます。
- 文字列に ESC または C1 CSI が含まれているかどうかに基づいて、文字列に ANSI エスケープ シーケンスが含まれているかどうかを返すために、`string IsDecorated` ブール型プロパティが追加されます。
- `Length` プロパティは、ANSI エスケープ シーケンスを使用せずにテキストの長さ "_のみ_" を返します。
- `StringDecorated Substring(int contentLength)` メソッドは、ANSI エスケープ シーケンスの一部ではないコンテンツの長さまで、インデックス 0 から始まる substring を返します。 これは、テーブルの書式設定が文字列を切り捨て、印刷可能な文字領域を占有しない ANSI エスケープ シーケンスを保持するために必要です。
- `string ToString()` メソッドは同じままで、文字列のプレーンテキスト バージョンを返します。
- `Ansi` パラメーターが true の場合、`string ToString(bool Ansi)` メソッドは生の ANSI 埋め込み文字列を返します。 それ以外の場合は、ANSI エスケープ シーケンスが削除されたプレーンテキスト バージョンが返されます。

## <a name="pscommandnotfoundsuggestion"></a>PSCommandNotFoundSuggestion

**CommandNotFoundException** 後にあいまい一致検索に基づいて、可能性のあるコマンドを推奨します。

```powershell
PS> get
```

```Output
get: The term 'get' is not recognized as the name of a cmdlet, function, script file, or operable
program. Check the spelling of the name, or if a path was included, verify that the path is correct
and try again.

Suggestion [4,General]: The most similar commands are: set, del, ft, gal, gbp, gc, gci, gcm, gdr,
gcs.
```

## <a name="pscultureinvariantreplaceoperator"></a>PSCultureInvariantReplaceOperator

`-replace` 演算子ステートメントの左側のオペランドが文字列でない場合、そのオペランドは文字列に変換されます。

この機能が無効になっている場合、`-replace` 演算子は、カルチャに依存した文字列変換を行います。
たとえば、カルチャがフランス語 (fr) に設定されている場合、`1.2` の値は文字列 `1,2` に変換されます。

```powershell
PS> [cultureinfo]::CurrentCulture = 'fr'
PS> 1.2 -replace ','
12
PS> [cultureinfo]::CurrentCulture = 'en'
PS> 1.2 -replace ','
1.2
```

機能が有効になっている場合:

```powershell
PS> [cultureinfo]::CurrentCulture = 'fr'
PS> 1.2 -replace ','
1.2
```

## <a name="psdesiredstateconfigurationinvokedscresource"></a>PSDesiredStateConfiguration.InvokeDscResource

Windows 以外のシステムで MOF へのコンパイルを有効にし、LCM を使用せずに `Invoke-DSCResource` を使用できるようにします。

## <a name="psimplicitremotingbatching"></a>PSImplicitRemotingBatching

この機能は、シェルで入力されたコマンドを検査します。すべてのコマンドが、単純なパイプラインを形成する暗黙のリモート処理プロキシ コマンドである場合は、コマンドがまとめられ、1 つのリモート パイプラインとして呼び出されます。

例:

```powershell
# Create remote session and import TestIMod module
$s = nsn -host remoteMachine
icm $s { ipmo 'C:\Users\user\Documents\WindowsPowerShell\Modules\TestIMod\TestIMod.psd1' }
Import-PSSession $s -mod testimod

$maxProcs = 1000
$filter = 'pwsh','powershell*','wmi*'

# Without batching, this pipeline takes approximately 12 seconds to run
Measure-Command { Get-AllProcesses -MaxCount $maxProcs | Select-Custom $filter | Group-Stuff $filter }
Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 12
Milliseconds      : 463

# But with the batching experimental feature enabled, it takes approximately 0.20 seconds
Measure-Command { Get-AllProcesses -MaxCount $maxProcs | Select-Custom $filter | Group-Stuff $filter }
Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 0
Milliseconds      : 209
```

前述のように、バッチ処理機能を有効にすると、3 つの暗黙的なリモート処理プロキシ コマンド (`Get-AllProcesses`、`Select-Custom`、`Group-Stuff`) がすべてリモート セッションで実行され、パイプラインからの結果がクライアントに返される唯一のデータになります。 これにより、クライアントとリモート セッションの間で送受信されるデータの量が減少し、オブジェクトのシリアル化と逆シリアル化の量も少なくなります。

## <a name="psnativepspathresolution"></a>PSNativePSPathResolution

FileSystem プロバイダーを使用する PSDrive パスがネイティブ コマンドに渡されると、解決されたファイル パスがネイティブ コマンドに渡されます。 これは、`code temp:/test.txt` のようなコマンドが期待どおりに動作することを意味します。

また、Windows の `~` で始まるパスは、完全なパスに解決され、ネイティブ コマンドに渡されます。 どちらの場合も、パスは、関連するオペレーティング システムのディレクトリ区切り記号に正規化されます。

- パスが PSDrive または `~` (Windows) でない場合、パスの正規化は行われません
- パスが単一引用符で囲まれている場合、そのパスは解決されず、リテラルとして扱われます

## <a name="psnotapplyerroractiontostderr"></a>PSNotApplyErrorActionToStderr

この試験的機能を有効にすると、ネイティブ コマンドからリダイレクトされるエラー レコード (リダイレクト演算子 (`2>&1`) を使用する場合のように) が、`$Error` 変数に書き込まれず、ユーザー設定変数 `$ErrorActionPreference` はリダイレクトされた出力に影響しません。

多くのネイティブ コマンドでは、追加情報の代替ストリームとして `stderr` に書き込みが行われます。 この動作により、エラーを探すときに混乱が生じる可能性があります。または、`$ErrorActionPreference` が出力をミュートする状態に設定されている場合は、ユーザーに対する追加の出力情報が失われる可能性があります。

ネイティブ コマンドの終了コードがゼロ以外の場合、`$?` は `$false` に設定されます。 終了コードがゼロの場合、`$?` は `$true` に設定されます。

## <a name="psnullconditionaloperators"></a>PSNullConditionalOperators

Null 条件付きメンバー アクセス演算子 (`?.` および `?[]`) 用の新しい演算子が導入されています。 Null メンバー アクセス演算子は、スカラー型および配列型で使用できます。 変数が null でない場合は、アクセスされるメンバーの値を返します。 変数の値が null の場合は、null を返します。

```powershell
$x = $null
${x}?.propname
${x?}?.propname

${x}?[0]
${x?}?[0]

${x}?.MyMethod()
```

プロパティ `propname` にアクセスし、`$x` が null でない場合にのみ値が返されます。 同様に、インデクサーは `$x` が null でない場合にのみ使用されます。 `$x` が null の場合、null が返されます。

`?.` 演算子と `?[]` 演算子はメンバー アクセス演算子であり、変数名と演算子の間にスペースを使用することはできません。

PowerShell では変数名の一部として `?` が許可されるため、変数名と演算子の間にスペースを使用せずに演算子を使用する場合はあいまいさを解消する必要があります。 あいまいさを解消するには、変数が `${x?}?.propertyName` や `${y}?[0]` などの変数名を囲む `{}` を使用する必要があります。

> [!NOTE]
> この機能は試験段階から移行され、PowerShell 7.1 以降のメインストリーム機能になりました。

## <a name="pstempdrive"></a>PSTempDrive

ユーザーの一時ディレクトリ パスにマップされた `TEMP:` PSDrive を作成します。

> [!NOTE]
> この機能は試験段階から移行され、PowerShell 7 以降のメインストリーム機能になりました。

## <a name="psunixfilestat"></a>PSUnixFileStat

この機能では、Unix の **stat** API のデータをファイル システム プロバイダーの出力に含めて、より Unix に近いファイル リストを容易に作成できる新しい動作が提供されます。 これにより、基になる Unix 型システムの `stat(2)` API の共通式を含む **UnixStat** という名前のファイル システム プロバイダーに新しいメモ プロパティが追加されます。

`Get-ChildItem` からの出力は次のようになります。

```powershell
dir | select -first 4 -skip 5


    Directory: /Users/jimtru/src/github/forks/JamesWTruher/PowerShell-1

UnixMode   User      Group           LastWriteTime        Size Name
--------   ----      -----           -------------        ---- ----
drwxr-xr-x jimtru    staff        10/23/2019 13:16         416 test
drwxr-xr-x jimtru    staff         11/8/2019 10:37         896 tools
-rw-r--r-- jimtru    staff         11/8/2019 10:37      112858 build.psm1
-rw-r--r-- jimtru    staff         11/8/2019 10:37      201297 CHANGELOG.md
```

> [!NOTE]
> この機能は試験段階から移行され、PowerShell 7.1 以降のメインストリーム機能になりました。

## <a name="psuseabbreviationexpansion"></a>PSUseAbbreviationExpansion

この機能により、省略されたコマンドレットと関数のタブ補完が可能になります。

次に例を示します。

- `i-psdf<tab>` は `Import-PowerShellDataFile` を返します。
- `u-akvmssdr<tab>` は `Undo-AzKeyVaultManagedStorageSasDefinitionRemoval` を返します。

これは、タブ補完 (対話型使用) に対してのみ機能するため、`i-psdf` はスクリプト内で有効なコマンドレット名ではありません。

> [!NOTE]
> この機能は試験段階から移行され、PowerShell 7 以降のメインストリーム機能になりました。

## <a name="pssubsystempluginmodel"></a>PSSubsystemPluginModel

この機能により、PowerShell でサブシステム プラグイン モデルが有効になります。 この機能により、`System.Management.Automation.dll` のコンポーネントを、独自のアセンブリに存在する個々のサブシステムに分けることができます。 この分割により、コア PowerShell エンジンのディスク占有領域が削減され、これらのコンポーネントを最小限の PowerShell インストールに対するオプション機能にすることができます。

現時点では、**CommandPredictor** サブシステムのみがサポートされています。 このサブシステムは、カスタム予測プラグインを提供するために、PSReadLine モジュールと共に使用されます。 今後、**Job**、**CommandCompleter**、**Remoting** などのコンポーネントは、`System.Management.Automation.dll` 外のサブシステム アセンブリに分割される可能性があります。

この試験的機能には、新しいコマンドレット [Get-PSSubsystem](xref:Microsoft.PowerShell.Core.Get-PSSubsystem) が含まれています。 このコマンドレットは、機能が有効になっている場合にのみ使用できます。 このコマンドレットを使用すると、システムで使用可能なサブシステムに関する情報が返されます。
