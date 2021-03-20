---
description: コマンドラインインターフェイスの使用方法について説明 `powershell.exe` します。 コマンドラインパラメーターを表示し、構文について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 10/05/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_exe?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PowerShell_exe
ms.openlocfilehash: 4025630ebb3abe4c0598c85940cfce383e9c7890
ms.sourcegitcommit: 16a02ae47d1a85b01692101aa0aa6e91e1ba398e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2021
ms.locfileid: "104726657"
---
# <a name="about-powershellexe"></a>PowerShell.exe について

## <a name="short-description"></a>簡単な説明
コマンドラインインターフェイスの使用方法について説明 `powershell.exe` します。 コマンドラインパラメーターを表示し、構文について説明します。

## <a name="long-description"></a>長い説明

### <a name="syntax"></a>SYNTAX

```
PowerShell[.exe]
    [-PSConsoleFile <file> | -Version <version>]
    [-NoLogo]
    [-NoExit]
    [-Sta]
    [-Mta]
    [-NoProfile]
    [-NonInteractive]
    [-InputFormat {Text | XML}]
    [-OutputFormat {Text | XML}]
    [-WindowStyle <style>]
    [-EncodedCommand <Base64EncodedCommand>]
    [-ConfigurationName <string>]
    [-File - | <filePath> <args>]
    [-ExecutionPolicy <ExecutionPolicy>]
    [-Command - | { <script-block> [-args <arg-array>] }
                | { <string> [<CommandParameters>] } ]

PowerShell[.exe] -Help | -? | /?
```

### <a name="parameters"></a>パラメーター

#### <a name="-psconsolefile-filepath"></a>-PSConsoleFile \<FilePath\>

指定された PowerShell コンソール ファイルを読み込みます。 コンソール ファイルの名前とパスを入力します。 コンソール ファイルを作成するには、PowerShell で Export-Console コマンドレットを使用します。

#### <a name="-version-powershell-version"></a>-Version \<PowerShell Version\>

指定したバージョンの PowerShell を起動します。 有効な値は2.0 および3.0 です。 指定するバージョンがシステムにインストールされている必要があります。 コンピューターに Windows PowerShell 3.0 がインストールされている場合は、"3.0" が既定のバージョンです。
それ以外の場合は、"2.0" が既定のバージョンになります。 詳細については、「 [PowerShell のインストール](/powershell/scripting/install/installing-windows-powershell)」を参照してください。

#### <a name="-nologo"></a>-NoLogo

スタートアップ時に著作権の見出しを非表示にします。

#### <a name="-noexit"></a>-NoExit

スタートアップ コマンドを実行後、終了しません。

#### <a name="-sta"></a>-Sta

シングルスレッド アパートメントを使用して、PowerShell を起動します。 Windows PowerShell 2.0 では、マルチスレッド アパートメント (MTA) が既定値です。 Windows PowerShell 3.0 では、シングルスレッド アパートメント (STA) が既定値です。

#### <a name="-mta"></a>-Mta

マルチスレッド アパートメントを使用して、PowerShell を起動します。 このパラメーターは、PowerShell 3.0 で導入されました。 PowerShell 2.0 では、マルチスレッド アパートメント (MTA) が既定値です。 PowerShell 3.0 では、シングルスレッド アパートメント (STA) が既定値です。

#### <a name="-noprofile"></a>-NoProfile

PowerShell プロファイルを読み込みません。

#### <a name="-noninteractive"></a>-NonInteractive

ユーザーに対話的なプロンプトを表示しません。

#### <a name="-inputformat-text--xml"></a>-InputFormat {Text | XML}

PowerShell に送信されるデータの形式を記述します。 使用できる値は、"Text" (テキスト文字列) と "XML" (シリアル化された CLIXML 形式) です。

#### <a name="-outputformat-text--xml"></a>-OutputFormat {Text | XML}

PowerShell からの出力の形式を決定します。 使用できる値は、"Text" (テキスト文字列) と "XML" (シリアル化された CLIXML 形式) です。

#### <a name="-windowstyle-window-style"></a>-WindowStyle \<Window style\>

セッションのウィンドウ スタイルを設定します。 使用できる値は、Normal、Minimized、Maximized、Hidden です。

#### <a name="-encodedcommand-base64encodedcommand"></a>-EncodedCommand \<Base64EncodedCommand\>

Base 64 エンコード文字列版のコマンドを許可します。 複雑な引用符や中かっこを必要とするコマンドを PowerShell に渡す場合にこのパラメーターを使用します。 文字列は、16LE 文字エンコーディングを使用して書式設定する必要があります。

#### <a name="-configurationname-string"></a>-ConfigurationName \<string\>

PowerShell を実行する構成エンドポイントを指定します。 これには、ローカルコンピューターに登録されている任意のエンドポイントを指定できます。これには、既定の PowerShell リモート処理エンドポイントや、特定のユーザーロール機能を持つカスタムエンドポイントが含まれます。

#### <a name="-file----filepath-args"></a>-File-|\<filePath\>\<args\>

**File** の値が "-" の場合、コマンドテキストは標準入力から読み取られます。
`powershell -File -`リダイレクトされた標準入力なしで実行すると、通常のセッションが開始されます。 これは、 **File** パラメーターを指定しない場合と同じです。

**ファイル** の値がファイルパスの場合、スクリプトはローカルスコープ ("ドットソース") で実行されるので、スクリプトによって作成される関数と変数を現在のセッションで使用できるようになります。 スクリプト ファイルのパスと (存在する場合) パラメーターを入力します。 **File** は、コマンド内の最後のパラメーターにする必要があります。 **File** パラメーターの後に入力されるすべての値は、スクリプトファイルのパスと、そのスクリプトに渡されるパラメーターとして解釈されます。

スクリプトに渡されるパラメーターは、(現在のシェルによる解釈の後で) リテラル文字列として渡されます。 たとえば、 **cmd.exe** で環境変数の値を渡す必要がある場合は、 **cmd.exe** 構文を使用します。 `powershell.exe -File .\test.ps1 -TestParam %windir%`

これに対し、 `powershell.exe -File .\test.ps1 -TestParam $env:windir` **cmd.exe** で実行すると、 `$env:windir` 現在の **cmd.exe** シェルに特別な意味を持たないため、スクリプトがリテラル文字列を受け取ることになります。 `$env:windir`環境変数参照のスタイルは、PowerShell コードとして解釈されるため、**コマンド** パラメーター内で使用 _でき_ ます。

同様に、 **バッチスクリプト** から同じコマンドを実行する場合 `%~dp0` は、またはの代わりにを使用し `.\` `$PSScriptRoot` て、現在の実行ディレクトリを表し `powershell.exe -File %~dp0test.ps1 -TestParam %windir%` ます。
代わりにを使用すると `.\test.ps1` 、PowerShell はリテラルパスを見つけることができないため、エラーをスローします。 `.\test.ps1`

ファイルの値がファイルパスの場合、 **file パラメーター名** の後に入力された文字はスクリプトファイルのパスとして解釈され、その後にスクリプトパラメーターが続くため、**ファイル****はコマンド** の最後のパラメーターである _必要があり_ ます。

**ファイル** パラメーターの値には、スクリプトのパラメーターと値を含めることができます。 例: `-File .\Get-Script.ps1 -Domain Central`

通常、スクリプトのスイッチ パラメーターは、含めるか省略するかのいずれかです。
たとえば、次のコマンドは、スクリプトファイルの **All** パラメーターを使用し `Get-Script.ps1` ます。 `-File .\Get-Script.ps1 -All`

まれに、パラメーターの **ブール** 値の指定が必要になる場合があります。
この方法でスクリプトを実行するときに、スイッチパラメーターに明示的なブール値を渡すことはできません。 この制限は、PowerShell 6 () で削除されました `pwsh.exe` 。

#### <a name="-executionpolicy-executionpolicy"></a>-ExecutionPolicy \<ExecutionPolicy\>

現在のセッションの既定の実行ポリシーを設定し、環境変数に保存し `$env:PSExecutionPolicyPreference` ます。 このパラメーターを指定しても、レジストリで設定された PowerShell の実行ポリシーが変更されるわけではありません。 有効な値の一覧など、PowerShell 実行ポリシーについては、「[about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

#### <a name="-command"></a>-Command

指定されたコマンド (および任意のパラメーター) を PowerShell コマンドプロンプトで入力した場合と同じように実行し `NoExit` ます。パラメーターが指定されていない場合は、を終了します。

**Command** の値には `-` 、、スクリプトブロック、または文字列を指定できます。 **Command** の値がの場合 `-` 、コマンドテキストは標準入力から読み取られます。

**コマンド** パラメーターでは、**コマンド** に **ScriptBlock** 型として渡された値を認識できる場合に限り、スクリプトブロックを実行できます。 これは `powershell.exe` 、別の PowerShell ホストから実行する場合にのみ可能です。 **ScriptBlock** 型は、に渡される前に、既存の変数に格納されるか、式から返されるか、または PowerShell ホストによって、中かっこ () で囲まれたリテラルスクリプトブロックとして解析されることがあり `{}` `powershell.exe` ます。

```powershell
powershell -Command {Get-WinEvent -LogName security}
```

で `cmd.exe` は、スクリプトブロック (または **ScriptBlock** 型) のようなものは存在しないため、 **コマンド** に渡される値は _常に_ 文字列になります。 文字列の中にスクリプト ブロックを記述することはできますが、実行されるのではなく、通常の PowerShell プロンプトで入力した場合とまったく同じように動作し、スクリプト ブロックの内容が出力されます。

**コマンド** に渡された文字列は PowerShell コードとして実行されます。そのため、からの実行時には、スクリプトブロックの中かっこを最初に記述する必要はありません `cmd.exe` 。 文字列内で定義されているインライン スクリプト ブロックを実行するには、次の[呼び出し演算子](about_operators.md#special-operators) `&` を使用できます。

```cmd
powershell.exe -Command "& {Get-WinEvent -LogName security}"
```

**Command** の値が文字列の場合、 **command** は pwsh の最後のパラメーターである必要があります。これは、後続のすべての引数が、実行するコマンドの一部として解釈されるためです。

既存の PowerShell セッション内から呼び出された場合、結果は、ライブオブジェクトではなく、逆シリアル化された XML オブジェクトとして親シェルに返されます。 他のシェルの場合、結果は文字列として返されます。

**Command** の値がの場合 `-` 、コマンドテキストは標準入力から読み取られます。 標準入力で **Command** パラメーターを使用する場合は、標準入力をリダイレクトする必要があります。 次に例を示します。

```powershell
@'
"in"

"hi" |
  % { "$_ there" }

"out"
'@ | powershell -NoProfile -Command -
```

この例を実行すると、次の出力が生成されます。

```Output
in
hi there
out
```

プロセス終了コードは、スクリプトブロック内の最後の (実行された) コマンドの状態によって決定されます。 がの場合、 `0` `$?` または `$true` `1` `$?` がの場合 `$false` 、終了コードはです。 最後のコマンドが、または以外の終了コードを明示的に設定する外部プログラムまたは PowerShell スクリプトの場合 `0` `1` 、終了コードは `1` プロセス終了コード用にに変換されます。 特定の終了コードを保持するに `exit $LASTEXITCODE` は、をコマンド文字列またはスクリプトブロックに追加します。

同様に、やなどのスクリプト終了 (実行空間終了) エラーが発生したとき、または `throw` `-ErrorAction Stop` <kbd>Ctrl C キーを押し</kbd>て実行が中断されたときに、値1が返され - <kbd></kbd>ます。

#### <a name="-help---"></a>-Help、-?、/?

**PowerShell.exe** のヘルプを表示します。 PowerShell セッションで **PowerShell.exe** コマンドを入力する場合は、コマンドパラメーターにスラッシュ (/) ではなくハイフン (-) を付加します。 **cmd.exe** では、ハイフンとスラッシュのいずれかを使用できます。

### <a name="remarks"></a>REMARKS

トラブルシューティング上の注意: PowerShell 2.0 では、PowerShell コンソールから一部のプログラムを開始すると、 **LastExitCode** 0xc0000142 で失敗します。

### <a name="examples"></a>例

```powershell
# Create a new PowerShell session and load a saved console file
PowerShell -PSConsoleFile sqlsnapin.psc1

# Create a new PowerShell V2 session with text input, XML output, and no logo
PowerShell -Version 2.0 -NoLogo -InputFormat text -OutputFormat XML

# Execute a PowerShell Command in a session
PowerShell -Command "Get-EventLog -LogName security"

# Run a script block in a session
PowerShell -Command {Get-EventLog -LogName security}

# An alternate way to run a command in a new session
PowerShell -Command "& {Get-EventLog -LogName security}"

# To use the -EncodedCommand parameter:
$command = "dir 'c:\program files' "
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encodedCommand = [Convert]::ToBase64String($bytes)
powershell.exe -encodedCommand $encodedCommand
```
