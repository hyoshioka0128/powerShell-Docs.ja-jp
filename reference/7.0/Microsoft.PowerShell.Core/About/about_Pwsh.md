---
description: コマンドラインインターフェイスの使用方法について説明 `pwsh` します。 コマンドラインパラメーターを表示し、構文について説明します。
ms.date: 10/05/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pwsh?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Pwsh
ms.openlocfilehash: 25ccb20a4c19a9519bf9d2a518ef6187c2327323
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "97692701"
---
# <a name="about-pwsh"></a>Pwsh の概要

## <a name="short-description"></a>簡単な説明
コマンドラインインターフェイスの使用方法について説明 `pwsh` します。 コマンドラインパラメーターを表示し、構文について説明します。

## <a name="long-description"></a>長い説明

## <a name="syntax"></a>構文

```
pwsh[.exe]
   [[-File] <filePath> [args]]
   [-Command { - | <script-block> [-args <arg-array>]
                 | <string> [<CommandParameters>] } ]
   [-ConfigurationName <string>]
   [-CustomPipeName <string>]
   [-EncodedCommand <Base64EncodedCommand>]
   [-ExecutionPolicy <ExecutionPolicy>]
   [-InputFormat {Text | XML}]
   [-Interactive]
   [-Login]
   [-MTA]
   [-NoExit]
   [-NoLogo]
   [-NonInteractive]
   [-NoProfile]
   [-OutputFormat {Text | XML}]
   [-SettingsFile <SettingsFilePath>]
   [-SSHServerMode]
   [-STA]
   [-Version]
   [-WindowStyle <style>]
   [-WorkingDirectory <directoryPath>]

pwsh[.exe] -h | -Help | -? | /?
```

## <a name="parameters"></a>パラメーター

すべてのパラメーターで大文字と小文字が区別されます。

### <a name="-file---f"></a>-File |-f

の値がの `File` 場合 `-` 、コマンドテキストは標準入力から読み取られます。
`pwsh -File -`リダイレクトされた標準入力なしで実行すると、通常のセッションが開始されます。 これは、パラメーターをまったく指定しない場合と同じです `File` 。

パラメーターが存在せず、値がコマンドラインに存在する場合は、これが既定のパラメーターになります。 指定されたスクリプトはローカルスコープ ("ドットソース") で実行されるため、スクリプトによって作成される関数と変数を現在のセッションで使用できます。 スクリプト ファイルのパスと (存在する場合) パラメーターを入力します。 File パラメーター名の後に入力されたすべての文字は、スクリプトファイルのパスとその後に続くスクリプトパラメーターとして解釈されるため、ファイルはコマンドの最後のパラメーターである必要があります。

通常、スクリプトのスイッチ パラメーターは、含めるか省略するかのいずれかです。
たとえば、次のコマンドは、Get-Script.ps1 スクリプトファイルの All パラメーターを使用します。 `-File .\Get-Script.ps1 -All`

まれに、スイッチパラメーターに **ブール** 値を指定しなければならない場合があります。 **File** パラメーターの値にスイッチパラメーターの **ブール** 値を指定するには、次のように、通常、コロンとブール値の直後に続くパラメーターを使用し `-File .\Get-Script.ps1 -All:$False` ます。

スクリプトに渡されるパラメーターは、(現在のシェルによる解釈の後で) リテラル文字列として渡されます。 たとえば、で環境変数の値を渡す場合は、 `cmd.exe` 次の構文を使用し `cmd.exe` ます。 `pwsh -File .\test.ps1 -TestParam %windir%`

これに対し、でを実行すると、 `pwsh -File .\test.ps1 -TestParam $env:windir` `cmd.exe` 現在の `$env:windir` シェルに特別な意味を持たないため、スクリプトがリテラル文字列を受け取ることになり `cmd.exe` ます。 `$env:windir`環境変数参照のスタイルは、PowerShell コードとして解釈されるため、**コマンド** パラメーター内で使用 _でき_ ます。

同様に、 _バッチスクリプト_ から同じコマンドを実行する場合 `%~dp0` は、またはの代わりにを使用し `.\` `$PSScriptRoot` て、現在の実行ディレクトリを表し `pwsh -File %~dp0test.ps1 -TestParam %windir%` ます。 代わりにを使用すると `.\test.ps1` 、PowerShell はリテラルパスを見つけることができないため、エラーをスローします。 `.\test.ps1`

スクリプトファイルがコマンドで終了すると、 `exit` プロセスの終了コードは、コマンドで使用される数値引数に設定され `exit` ます。 通常の終了では、終了コードは常に `0` です。

と同様に、 `-Command` スクリプトを終了するエラーが発生すると、終了コードはに設定され `1` ます。 ただし、とは異なり、 `-Command` <kbd>Ctrl</kbd>C を使用して実行が中断されると、 - <kbd></kbd>終了コードはに `0` なります。

### <a name="-command---c"></a>-Command |-c

指定されたコマンド (および任意のパラメーター) を PowerShell コマンドプロンプトで入力した場合と同じように実行し `NoExit` ます。パラメーターが指定されていない場合は、を終了します。

**Command** の値には `-` 、、スクリプトブロック、または文字列を指定できます。 **Command** の値がの場合 `-` 、コマンドテキストは標準入力から読み取られます。

**コマンド** パラメーターでは、**コマンド** に **ScriptBlock** 型として渡された値を認識できる場合に限り、スクリプトブロックを実行できます。 これは `pwsh` 、別の PowerShell ホストから実行する場合にのみ可能です。 **ScriptBlock** 型は、に渡される前に、既存の変数に格納されるか、式から返されるか、または PowerShell ホストによって、中かっこ () で囲まれたリテラルスクリプトブロックとして解析されることがあり `{}` `pwsh` ます。

```powershell
pwsh -Command {Get-WinEvent -LogName security}
```

で `cmd.exe` は、スクリプトブロック (または **ScriptBlock** 型) のようなものは存在しないため、 **コマンド** に渡される値は _常に_ 文字列になります。 文字列の中にスクリプト ブロックを記述することはできますが、実行されるのではなく、通常の PowerShell プロンプトで入力した場合とまったく同じように動作し、スクリプト ブロックの内容が出力されます。

**コマンド** に渡された文字列は PowerShell コードとして実行されます。そのため、からの実行時には、スクリプトブロックの中かっこを最初に記述する必要はありません `cmd.exe` 。 文字列内で定義されているインライン スクリプト ブロックを実行するには、次の[呼び出し演算子](about_operators.md#special-operators) `&` を使用できます。

```
pwsh -Command "& {Get-WinEvent -LogName security}"
```

**Command** の値が文字列の場合、 **command** は pwsh の最後のパラメーターである必要があります。これは、後続のすべての引数が、実行するコマンドの一部として解釈されるためです。

既存の PowerShell セッション内から呼び出された場合、結果は、ライブオブジェクトではなく、逆シリアル化された XML オブジェクトとして親シェルに返されます。 他のシェルの場合、結果は文字列として返されます。

**Command** の値がの場合 `-` 、コマンドテキストは標準入力から読み取られます。 標準入力で **Command** パラメーターを使用する場合は、標準入力をリダイレクトする必要があります。 以下に例を示します。

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

### <a name="-configurationname---config"></a>-ConfigurationName |-config

PowerShell を実行する構成エンドポイントを指定します。 これには、ローカルコンピューターに登録されている任意のエンドポイントを指定できます。これには、既定の PowerShell リモート処理エンドポイントや、特定のユーザーロール機能を持つカスタムエンドポイントが含まれます。

例: `pwsh -ConfigurationName AdminRoles`

### <a name="-custompipename"></a>-CustomPipeName

デバッグおよびその他のプロセス間通信に使用する追加の IPC サーバー (名前付きパイプ) に使用する名前を指定します。 これにより、他の PowerShell インスタンスに接続するための予測可能なメカニズムが提供されます。 通常、では **Custompipename** パラメーターと共に使用され `Enter-PSHostProcess` ます。

このパラメーターは、PowerShell 6.2 で導入されました。

以下に例を示します。

```powershell
# PowerShell instance 1
pwsh -CustomPipeName mydebugpipe
# PowerShell instance 2
Enter-PSHostProcess -CustomPipeName mydebugpipe
```

### <a name="-encodedcommand---e---ec"></a>-EncodedCommand |-e |-ec

Base64 でエンコードされた文字列バージョンのコマンドを受け入れます。 このパラメーターを使用して、入れ子になった複雑な引用符を必要とするコマンドを PowerShell に送信します。 Base64 表現は、16LE でエンコードされた文字列である必要があります。

以下に例を示します。

```powershell
$command = 'dir "c:\program files" '
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encodedCommand = [Convert]::ToBase64String($bytes)
pwsh -encodedcommand $encodedCommand
```

### <a name="-executionpolicy---ex---ep"></a>-Set-executionpolicy |-ex |-ep

現在のセッションの既定の実行ポリシーを設定し、環境変数に保存し `$env:PSExecutionPolicyPreference` ます。 このパラメーターでは、永続的に構成された実行ポリシーは変更されません。

このパラメーターは、Windows コンピューターにのみ適用されます。 `$env:PSExecutionPolicyPreference`環境変数は、Windows 以外のプラットフォームには存在しません。

### <a name="-inputformat---inp---if"></a>-InputFormat |-sct.inp |-if

PowerShell に送信されるデータの形式を記述します。 使用できる値は、"Text" (テキスト文字列) と "XML" (シリアル化された CLIXML 形式) です。

### <a name="-interactive---i"></a>-Interactive |-i

対話型プロンプトをユーザーに表示します。 非対話型パラメーターの逆。

### <a name="-login---l"></a>-Login |-l

Linux および macOS では、/bin/sh を使用して/etc/profile や ~/.profile. などのログインプロファイルを実行し、ログインシェルとして PowerShell を起動します。
Windows では、このスイッチは何も行いません。

> [!IMPORTANT]
> このパラメーターは、PowerShell をログインシェルとして起動するために最初に指定する必要があります。
> このパラメーターを別の位置に渡すことは無視されます。

`pwsh`UNIX のようなオペレーティングシステムでログインシェルとしてを設定するには、次の操作を行います。

- の完全な絶対パスがの下に表示されていることを確認します。 `pwsh``/etc/shells`
  - このパスは、通常、 `/usr/bin/pwsh` Linux または `/usr/local/bin/pwsh` macOS では次のようになります。
  - インストールの方法によっては、インストール時にこのエントリが自動的に追加されます。
  - `pwsh`がに存在しない場合は、エディターを使用して、 `/etc/shells` 最後の行にパスを追加し `pwsh` ます。 このためには、管理者特権でを編集する必要があります。
- [Chsh](https://linux.die.net/man/1/chsh)ユーティリティを使用して、現在のユーザーのシェルを次のように設定し `pwsh` ます。

  ```sh
  chsh -s /usr/bin/pwsh
  ```

> [!WARNING]
> `pwsh`現在、ログインシェルとしての設定は、Windows Subsystem For Linux (WSL) ではサポートされていません。また、をログインシェルとして設定しようとすると、 `pwsh` wsl を対話的に開始できなくなる可能性があります。

### <a name="-mta"></a>-MTA

マルチスレッドアパートメントを使用して PowerShell を起動します。 このスイッチは Windows でのみ使用できます。

### <a name="-noexit---noe"></a>-NoExit |-noe

スタートアップ コマンドを実行後、終了しません。

例: `pwsh -NoExit -Command Get-Date`

### <a name="-nologo---nol"></a>-NoLogo |-nol

対話型セッションの起動時に著作権情報のバナーを非表示にします。

### <a name="-noninteractive---noni"></a>-非対話型 |-非 i

ユーザーに対話的なプロンプトを表示しません。 またはの確認プロンプトなどの対話機能を使用しようとすると `Read-Host` 、ステートメントの終了エラーが発生します。

### <a name="-noprofile---nop"></a>-NoProfile |-nop

では、PowerShell プロファイルは読み込まれません。

### <a name="-outputformat---o---of"></a>-OutputFormat |-o |-/

PowerShell からの出力の形式を決定します。 使用できる値は、"Text" (テキスト文字列) と "XML" (シリアル化された CLIXML 形式) です。

例: `pwsh -o XML -c Get-Date`

でを PowerShell セッションとして呼び出すと、逆シリアル化されたオブジェクトが、プレーンテキストではなく出力として取得されます。 他のシェルから呼び出された場合、出力は EXPORT-CLIXML text として書式設定された文字列データになります。

### <a name="-settingsfile---settings"></a>-SettingsFile |-設定

セッションのシステム全体の `powershell.config.json` 設定ファイルをオーバーライドします。 既定では、ディレクトリのからシステム全体の設定が読み取られ `powershell.config.json` `$PSHOME` ます。

これらの設定は、引数で指定されたエンドポイントでは使用されないことに注意 `-ConfigurationName` してください。

例: `pwsh -SettingsFile c:\myproject\powershell.config.json`

### <a name="-sshservermode---sshs"></a>-SSHServerMode |-sshs

SSH サブシステムとして PowerShell を実行するために sshd_config で使用されます。 これは、他の用途のためのものでも、サポートもされていません。

### <a name="-sta"></a>-STA

シングルスレッドアパートメントを使用して PowerShell を起動します。 これは既定値です。 このスイッチは Windows でのみ使用できます。

### <a name="-version---v"></a>-Version |-v

PowerShell のバージョンを表示します。 追加のパラメーターは無視されます。

### <a name="-windowstyle---w"></a>-System.windows.window.windowstyle |-w

セッションのウィンドウ スタイルを設定します。 使用できる値は、Normal、Minimized、Maximized、Hidden です。

### <a name="-workingdirectory---wd"></a>-WorkingDirectory |-wd

起動時にを実行して初期作業ディレクトリを設定します。 有効な PowerShell ファイルパスがサポートされています。

ホームディレクトリで PowerShell を開始するには、次のように使用します。 `pwsh -WorkingDirectory ~`

### <a name="-help---"></a>-Help、-?、/?

のヘルプを表示 `pwsh` します。 PowerShell で pwsh コマンドを入力する場合は、コマンドパラメーターにスラッシュ () ではなくハイフン () を付加し `-` `/` ます。
