---
description: レジストリ構成を置き換える PowerShell Core の構成ファイル。
Locale: en-US
ms.date: 11/02/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_config?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PowerShell_Config
ms.openlocfilehash: 7190484832958e778a21e6d2cc91771587bffdbf
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605457"
---
# <a name="about-powershell-config"></a>PowerShell の構成について

## <a name="short-description"></a>概要
レジストリ構成を置き換える PowerShell Core の構成ファイル。

## <a name="long-description"></a>詳細説明

このファイルには、 `powershell.config.json` PowerShell Core の構成設定が含まれています。 PowerShell は起動時にこの構成を読み込みます。 設定は、実行時に変更することもできます。 以前は、これらの設定は PowerShell 用の Windows レジストリに格納されていましたが、macOS と Linux での構成を可能にするためにファイルに含まれるようになりました。

> [!WARNING]
> ファイルに `powershell.config.json` 無効な JSON が含まれている場合は、対話型セッションを開始できません。
> この問題が発生した場合は、構成ファイルを修正する必要があります。

> [!NOTE]
> 認識されないキーまたは構成ファイル内の無効な値は、警告なしで無視されます。

### <a name="allusers-shared-configuration"></a>AllUsers (共有) 構成

`powershell.config.json`ディレクトリ内のファイルは、 `$PSHOME` その powershell コアインストールから実行されているすべての powershell コアセッションの構成を定義します。

> [!NOTE]
> この `$PSHOME` 場所は、実行中の System.Management.Automation.dll アセンブリと同じディレクトリとして定義されます。 これは、ホストされている PowerShell SDK インスタンスにも当てはまります。

### <a name="currentuser-per-user-configurations"></a>CurrentUser (ユーザー単位) の構成

ユーザースコープの構成ディレクトリにファイルを配置することで、ユーザーごとに PowerShell を構成することもできます。 ユーザー構成ディレクトリは、コマンドを使用してプラットフォーム間で検出でき `Split-Path $PROFILE.CurrentUserCurrentHost` ます。

## <a name="general-configuration-settings"></a>全般構成設定

### <a name="executionpolicy"></a>ExecutionPolicy

> [!IMPORTANT]
> この構成は、Windows プラットフォームにのみ適用されます。

PowerShell セッションの実行ポリシーを構成し、実行できるスクリプトを決定します。 既定では、PowerShell は既存の実行ポリシーを使用します。

AllUsers 構成の場合、これによって **LocalMachine** 実行ポリシーが設定されます。
CurrentUser 構成の場合、 **currentuser** 実行ポリシーが設定されます。

> [!NOTE]
> [`Set-ExecutionPolicy`](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)コマンドレットは、を使用して呼び出されたときに AllUsers 構成ファイル内のこの設定を変更 `-Scope LocalMachine` し、で呼び出されたときに CurrentUser 構成ファイルでこの設定を変更し `-Scope CurrentUser` ます。

```Schema
"<shell-id>:ExecutionPolicy": "<execution-policy>"
```

条件:

- `<shell-id>` 現在の PowerShell ホストの ID を参照します。
  通常の PowerShell Core の場合、これは `Microsoft.PowerShell` です。
  任意の PowerShell セッションで、を使用して検出でき `$ShellId` ます。
- `<execution-policy>` 有効な実行ポリシー名を参照します。

次の例では、PowerShell の実行ポリシーをに設定し `RemoteSigned` ます。

```json
{
  "Microsoft.PowerShell.ExecutionPolicy": "RemoteSigned"
}
```

Windows では、同等のレジストリキーがおよびの `\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell` 下にあり `HKEY_LOCAL_MACHINE` `HKEY_CURRENT_USER` ます。

### <a name="psmodulepath"></a>PSModulePath

この PowerShell セッションの PSModulePath コンポーネントをオーバーライドします。 現在のユーザーの構成である場合は、CurrentUser モジュールのパスを設定します。 構成がすべてのユーザーを対象としている場合は、AllUser モジュールパスを設定します。

> [!WARNING]
> ここで AllUsers または CurrentUser モジュールパスを構成しても、 [インストールモジュール](/powershell/module/powershellget/install-module)のような PowerShellGet モジュールの対象となるインストール場所は変更されません。
> これらのコマンドレットは、常に *既定* のモジュールパスを使用します。

値が設定されていない場合は、それぞれのモジュールパスコンポーネントの既定値が使用されます。 これらの既定の詳細については、「 [about_Modules](./about_Modules.md#module-and-dsc-resource-locations-and-psmodulepath) 」を参照してください。

この設定により、CMD で許可され `%` `"%HOME%\Documents\PowerShell\Modules"` ているのと同じように、環境変数を文字間に埋め込むことによって使用できます。 この構文は、Linux と macOS でも適用されます。 次の例を参照してください。

```Schema
"PSModulePath": "<ps-module-path>"
```

条件:

- `<ps-module-path>` は、モジュールディレクトリへの絶対パスです。 すべてのユーザー構成について、これは AllUsers 共有モジュールディレクトリです。 現在のユーザー構成では、CurrentUser モジュールディレクトリになります。

次の例は、Windows 環境の PSModulePath 構成を示しています。

```json
{
  "PSModulePath": "C:\\Program Files\\PowerShell\\6\\Modules"
}
```

次の例は、macOS または Linux 環境の PSModulePath 構成を示しています。

```json
{
  "PSModulePath": "/opt/powershell/6/Modules"
}
```

この例では、PSModulePath 構成に環境変数を埋め込む方法を示します。 `HOME`環境変数とディレクトリの区切り記号を使用すると `/` 、Windows、MacOS、Linux で動作することに注意してください。

```json
{
  "PSModulePath": "%HOME%/Documents/PowerShell/Modules"
}
```

この例では、macOS と Linux でのみ機能する PSModulePath 構成に環境変数を埋め込む方法を示します。

```json
{
  "PSModulePath": "%XDG_CONFIG_HOME%/powershell/Modules"
}
```

> [!NOTE]
> PowerShell 変数は、PSModulePath 構成に埋め込むことはできません。
> Linux と macOS での PSModulePath の構成では、大文字と小文字が区別されます。 PSModulePath 構成では、プラットフォームに有効なディレクトリ区切り記号を使用する必要があります。 MacOS と Linux では、これは `/` です。 Windows では、 `/` との両方 `\` が動作します。

### <a name="experimentalfeatures"></a>ExperimentalFeatures

PowerShell で有効にする試験的な機能の名前。
既定では、試験的な機能は有効になっていません。
既定値は空の配列です。

```Schema
"ExperimentalFeatures": ["<experimental-feature-name>", ...]
```

条件:

- `<experimental-feature-name>` 有効にする試験的な機能の名前を指定します。

次の例では、PowerShell の起動時に **PSImplicitRemoting** と **PSUseAbbreviationExpansion** の試験的な機能を有効にします。

```json
{
  "ExperimentalFeatures": [
    "PSImplicitRemotingBatching",
    "PSUseAbbreviationExpansion"
  ]
}
```

試験的な機能の詳細については、「 [POWERSHELL RFC 29][RFC0029]」を参照してください。

## <a name="non-windows-logging-configuration"></a>Windows 以外のログの構成

> [!IMPORTANT]
> このセクションの構成オプションは、macOS と Linux にのみ適用されます。
> Windows のログは、Windows イベントビューアーによって管理されます。

Powershell の macOS と Linux でのログ記録は、PowerShell 構成ファイルで構成できます。 Windows 以外のシステムの PowerShell ログの詳細については、「 [ログ記録につい](./about_Logging_Non-Windows.md)て」を参照してください。

### <a name="logidentity"></a>LogIdentity

> [!IMPORTANT]
> この設定は、macOS と Linux でのみ使用できます。

システムログへの書き込みに使用する id 名を設定します。 既定値は "powershell" です。

```Schema
"LogIdentity": "<log-identity>"
```

条件:

- `<log-identity>` は、PowerShell が syslog に書き込むために使用する文字列 id です。

> [!NOTE]
> インストールした PowerShell のインスタンスごとに異なる **Logidentity** 値を使用することもできます。

この例では、PowerShell のプレビューリリースの **Logidentity** を構成しています。

```json
{
  "LogIdentity": "powershell-preview"
}
```

### <a name="loglevel"></a>LogLevel

> [!IMPORTANT]
> この設定は、macOS と Linux でのみ使用できます。

PowerShell がログに記録する必要がある最小重大度レベルを指定します。

```Schema
"LogLevel": "<log-level>|default"
```

条件:

- `<log-level>`は次のいずれかです:
  - Always
  - Critical
  - エラー
  - 警告
  - Informational
  - "詳細"
  - デバッグ

> [!NOTE]
> ログレベルを設定すると、その上にあるすべてのログレベルが有効になります。

この設定を **default** に設定すると、既定値として解釈されます。
既定値は **情報** です。

次の例では、値を **Verbose** に設定します。

```json
{
  "LogLevel": "Verbose"
}
```

### <a name="logchannels"></a>LogChannels

> [!IMPORTANT]
> この設定は、macOS と Linux でのみ使用できます。

有効にするログチャネルを指定します。

```Schema
"LogChannels": "<log-channel>,..."
```

条件:

- `<log-channel>`は次のいずれかです:
  - 操作ログ PowerShell アクティビティに関する基本情報
  - 分析-より詳細な診断情報をログに記録します

既定値は **Operational** です。 両方のチャネルを有効にするには、両方の値を1つのコンマ区切りの文字列として指定します。 次に例を示します。

```json
{
  "LogChannels": "Operational,Analytic"
}
```

### <a name="logkeywords"></a>LogKeywords

> [!IMPORTANT]
> この設定は、macOS と Linux でのみ使用できます。

PowerShell のどの部分がログに記録されるかを決定します。 既定では、すべてのログキーワードが有効になっています。 複数のキーワードを有効にするには、1つのコンマ区切りの文字列で値を一覧表示します。

```Schema
"LogKeywords": "<log-keyword>,..."
```

条件:

- `<log-keyword>`は次のいずれかです:
  - 実行空間-実行空間の管理
  - パイプラインパイプライン操作
  - プロトコル通信プロトコルの処理 (PSRP など)
  - トランスポート-トランスポート層のサポート (SSH など)
  - ホスト-PowerShell ホストの機能 (コンソールの相互作用など)
  - コマンドレット-PowerShell の組み込みコマンドレット
  - シリアライザー-シリアル化ロジック
  - セッション-PowerShell セッションの状態
  - ManagedPlugin-WSMan プラグイン

> [!NOTE]
> 一般的に、PowerShell の既知の部分で特定の動作を診断する場合を除き、この値を設定しないことをお勧めします。
> この値を変更すると、ログに記録される情報の量が減少します。

この例では、実行空間操作、パイプラインロジック、およびコマンドレットの使用に対するログ記録を制限します。 その他のすべてのログは省略されます。

```json
"LogKeywords": "Runspace,Pipeline,Cmdlets"
```

<!--

## Group policy configuration

### ScriptExecution

### ScriptBlockLogging

### ProtectedEventLogging

### Transcription

### UpdateableHelp

### ConsoleSessionConfiguration

-->

## <a name="more-example-configurations"></a>その他の構成の例

### <a name="example-windows-configuration"></a>Windows の構成例

この構成では、より多くの設定が明示的に設定されています。

- この PowerShell インストールの実行ポリシーは次のようになります。 `AllSigned`
- CurrentUser モジュールパスは、共有ドライブ上のモジュールディレクトリに設定されています
- `PSImplicitRemotingBatching`試験的な機能が有効になっている

> [!NOTE]
> `ExecutionPolicy`ここでの設定との設定は、 `PSModulePath` Windows プラットフォームでのみ機能します。モジュールパスで `\` 区切り文字が使用され `;` ており、実行ポリシーが `Unrestricted` (UNIX に似たプラットフォームでのみ許可されている) であるためです。

```json
{
  "Microsoft.PowerShell.ExecutionPolicy": "AllSigned",
  "PSModulePath": "Z:\\Marisol's Shared Drive\\Modules",
  "ExperimentalFeatures": ["PSImplicitRemotingBatching"],
}
```

### <a name="example-non-windows-configuration"></a>Windows 以外の構成の例

この構成では、macOS または Linux でのみ機能するいくつかのオプションを設定します。

- CurrentUser モジュールパスは、のカスタムモジュールディレクトリに設定されます。 `$HOME`
- **PSImplicitRemotingBatching** の試験的な機能が有効になっている
- ログ記録の詳細については、PowerShell のログ記録レベルを **Verbose** に設定します。
- この PowerShell インストールでは、 **ホーム-PowerShell** id を使用してログに書き込みます。

```json
{
  "PSModulePath": "%HOME%/.powershell/Modules",
  "ExperimentalFeatures": ["PSImplicitRemotingBatching"],
  "LogLevel": "Verbose",
  "LogIdentity": "home-powershell"
}
```

## <a name="see-also"></a>関連項目

[実行ポリシーについて](./about_Execution_Policies.md)

[自動変数について](./about_Automatic_Variables.md)

[RFC0029]: https://github.com/PowerShell/PowerShell-RFC/blob/master/5-Final/RFC0029-Support-Experimental-Features.md

