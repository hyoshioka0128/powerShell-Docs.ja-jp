---
description: PowerShell の状態情報を格納する変数について説明します。 これらの変数は、PowerShell によって作成および管理されます。
Locale: en-US
ms.date: 12/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Automatic_Variables
ms.openlocfilehash: 8a2410dd2adcc1679ab203293b4c4e712b960278
ms.sourcegitcommit: 3a1d80e27438976101f216b8c3d623c61b868db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2021
ms.locfileid: "99975143"
---
# <a name="about-automatic-variables"></a>自動変数について

## <a name="short-description"></a>簡単な説明

PowerShell の状態情報を格納する変数について説明します。 これらの変数は、PowerShell によって作成および管理されます。

## <a name="long-description"></a>長い説明

概念的には、これらの変数は読み取り専用と見なされます。 これらをに書き込む **ことはできますが** 、旧バージョンとの互換性のために書き込ま **ない** でください。

PowerShell の自動変数の一覧を次に示します。

### <a name=""></a>$$

セッションによって受信された最後の行の最後のトークンを格納します。

### <a name=""></a>$?

最後のコマンドの実行状態を格納します。 最後のコマンドが成功した場合は **True** 、失敗した場合は **False** が格納されます。

パイプライン内の複数の段階で実行されるコマンドレットおよび拡張関数の場合 (およびブロックの両方で) は、とのように、またはの各ポイントでを `process` `end` 呼び出すと、 `this.WriteError()` `$PSCmdlet.WriteError()` `$?` **False** に設定され `this.ThrowTerminatingError()` `$PSCmdlet.ThrowTerminatingError()` ます。

`Write-Error`コマンドレットは、 `$?` 実行された直後に **false** に設定されますが、 `$?` それを呼び出す関数の場合は **false** に設定されません。

```powershell
function Test-WriteError
{
    Write-Error "Bad"
    $? # $false
}

Test-WriteError
$? # $true
```

後者の場合は、 `$PSCmdlet.WriteError()` 代わりにを使用する必要があります。

ネイティブコマンド (実行可能ファイル) の場合、が `$?` 0 の場合は **True** に設定され、 `$LASTEXITCODE` が他の値の場合は **False** に設定され `$LASTEXITCODE` ます。

> [!NOTE]
> PowerShell 7 までは、かっこ内にステートメントが含ま `(...)` れている限り、部分式構文 `$(...)` または配列式は常に true にリセットされるので、は `@(...)` `$?` true になり `(Write-Error)` `$?` ます。 
> これは PowerShell 7 で変更されているため、 `$?` は常に、これらの式で最後に実行されたコマンドの実際の成功を反映します。

### <a name=""></a>$^

セッションによって受信された最後の行の最初のトークンを格納します。

### <a name="_"></a>$_

`$PSItem` と同じ。 パイプラインオブジェクトの現在のオブジェクトを格納します。 この変数は、パイプライン内のすべてのオブジェクトまたは選択したオブジェクトに対してアクションを実行するコマンドで使用できます。

### <a name="args"></a>$args

関数、スクリプト、またはスクリプトブロックに渡される、宣言されていないパラメーターの値の配列を格納します。 関数を作成するときに、キーワードを使用する `param` か、関数名の後にパラメーターのコンマ区切りリストをかっこで追加することによって、パラメーターを宣言できます。

イベントアクションでは、変数には、 `$args` 処理中のイベントのイベント引数を表すオブジェクトが格納されます。 この変数は、 `Action` イベント登録コマンドのブロック内でのみ設定されます。
この変数の値は、を返す **PSEventArgs** オブジェクトの **sourceargs** プロパティにもあり `Get-Event` ます。

### <a name="consolefilename"></a>$ConsoleFileName

セッションで最近使用されたコンソールファイル () のパスを格納し `.psc1` ます。 この変数は、 **Psconsolefile** パラメーターを使用して PowerShell を起動したとき、またはコマンドレットを使用して `Export-Console` スナップイン名をコンソールファイルにエクスポートしたときに設定されます。

`Export-Console`パラメーターを指定せずにコマンドレットを使用すると、セッションで最後に使用されたコンソールファイルが自動的に更新されます。 この自動変数を使用すると、どのファイルを更新するかを決定できます。

### <a name="error"></a>$Error

最新のエラーを表すエラーオブジェクトの配列が含まれています。
最新のエラーは、配列内の最初のエラーオブジェクトです `$Error[0]` 。

エラーが配列に追加されないようにするには `$Error` 、 **erroraction** 共通パラメーターに値 **Ignore** を指定します。 詳細については、「[about_CommonParameters](about_CommonParameters.md)」を参照してください。

### <a name="event"></a>$Event

処理中のイベントを表す **PSEventArgs** オブジェクトを格納します。 この変数は、などの `Action` イベント登録コマンドのブロック内でのみ設定され `Register-ObjectEvent` ます。 この変数の値は、コマンドレットが返すオブジェクトと同じです `Get-Event` 。 そのため、などの変数のプロパティを `Event` `$Event.TimeGenerated` スクリプトブロックで使用でき `Action` ます。

### <a name="eventargs"></a>$EventArgs

処理中のイベントの **EventArgs** から派生した最初のイベント引数を表すオブジェクトを格納します。 この変数は、 `Action` イベント登録コマンドのブロック内でのみ設定されます。
この変数の値は、を返す **PSEventArgs** オブジェクトの **sourceeventargs** プロパティにもあり `Get-Event` ます。

### <a name="eventsubscriber"></a>$EventSubscriber

処理中のイベントのイベントサブスクライバーを表す **PSEventSubscriber** オブジェクトを格納します。 この変数は、 `Action` イベント登録コマンドのブロック内でのみ設定されます。 この変数の値は、コマンドレットが返すオブジェクトと同じです `Get-EventSubscriber` 。

### <a name="executioncontext"></a>$ExecutionContext

PowerShell ホストの実行コンテキストを表す **EngineIntrinsics** オブジェクトを格納します。 この変数を使用すると、コマンドレットで使用できる実行オブジェクトを見つけることができます。

### <a name="false"></a>$false

**False** が含まれています。 この変数を使用すると、文字列 "false" を使用する代わりに、コマンドとスクリプトで **false** を表すことができます。 文字列は、空でない文字列または0以外の整数に変換される場合、 **True** として解釈できます。

### <a name="foreach"></a>$foreach

[ForEach](about_ForEach.md)ループの列挙子 (結果の値ではない) が含まれます。 変数は、 `$ForEach` ループの実行中にのみ存在します。ループ `ForEach` が完了すると削除されます。

列挙子には、ループ値を取得したり、現在のループの反復を変更したりするために使用できるプロパティとメソッドが含まれています。 詳細については、「 [列挙子の使用](#using-enumerators)」を参照してください。

### <a name="home"></a>$HOME

ユーザーのホームディレクトリの完全なパスを含みます。 この変数は、 `"$env:homedrive$env:homepath"` 通常、Windows 環境変数に相当し `C:\Users\<UserName>` ます。

### <a name="host"></a>$Host

PowerShell の現在のホストアプリケーションを表すオブジェクトを格納します。 この変数を使用すると、コマンドで現在のホストを表すことや、ホストのプロパティを表示または変更することができ `$Host.version` ます。たとえば、やなど `$Host.CurrentCulture` `$host.ui.rawui.setbackgroundcolor("Red")` です。

### <a name="input"></a>$input

関数に渡されるすべての入力を列挙する列挙子を格納します。 変数は、 `$input` 関数とスクリプトブロック (名前のない関数) でのみ使用できます。

- 、、またはブロックのない関数では、 `Begin` `Process` `End` 変数は、 `$input` 関数へのすべての入力のコレクションを列挙します。

- ブロックでは、 `Begin` `$input` 変数にデータが含まれていません。

- ブロック内の変数には、 `Process` `$input` 現在パイプライン内にあるオブジェクトが含まれています。

- ブロックでは、 `End` 変数は、 `$input` 関数へのすべての入力のコレクションを列挙します。

  > [!NOTE]
  > `$input`同じ関数またはスクリプトブロックの中で、プロセスブロックと End ブロックの両方で変数を使用することはできません。

`$input`は列挙子であるため、そのプロパティのいずれかにアクセスすると、が使用できなくなり `$input` ます。 `$input`別の変数に格納して、プロパティを再利用することができ `$input` ます。

列挙子には、ループ値を取得したり、現在のループの反復を変更したりするために使用できるプロパティとメソッドが含まれています。 詳細については、「 [列挙子の使用](#using-enumerators)」を参照してください。

変数は、 `$input` `-Command` `pwsh` コマンドラインから呼び出されたときに、のパラメーターで指定したコマンドにも使用できます。 次の例は、Windows コマンドシェルから実行されます。

```CMD
echo Hello | pwsh -Command """$input World!"""
```

### <a name="iscoreclr"></a>$IsCoreCLR

`$True`現在のセッションが .Net Core ランタイム (CoreCLR) で実行されているかどうかを示します。 それ以外 `$False` の場合は。

### <a name="islinux"></a>$IsLinux

`$True`現在のセッションが Linux オペレーティングシステムで実行されているかどうかを示します。
それ以外 `$False` の場合は。

### <a name="ismacos"></a>$IsMacOS

`$True`現在のセッションが MacOS オペレーティングシステムで実行されているかどうかを示します。
それ以外 `$False` の場合は。

### <a name="iswindows"></a>$IsWindows

`$TRUE`現在のセッションが Windows オペレーティングシステムで実行されているかどうかを示します。 それ以外 `$FALSE` の場合は。

### <a name="lastexitcode"></a>$LastExitCode

最後に実行された Windows ベースのプログラムの終了コードを格納します。

### <a name="matches"></a>$Matches

変数は、 `Matches` `-match` 演算子と演算子と連携し `-notmatch` ます。
スカラー入力を or 演算子に送信 `-match` `-notmatch` し、一方が一致を検出した場合は、ブール値を返し、 `$Matches` 一致した文字列値のハッシュテーブルで自動変数を設定します。 `$Matches`演算子で正規表現を使用する場合、ハッシュテーブルにキャプチャを設定することもでき `-match` ます。

オペレーターの詳細については `-match` 、「 [about_Comparison_Operators](about_comparison_operators.md)」を参照してください。 正規表現の詳細については、「 [about_Regular_Expressions](about_Regular_Expressions.md)」を参照してください。

### <a name="myinvocation"></a>$MyInvocation

現在のコマンドに関する情報 (名前、パラメーター、パラメーター値など)、コマンドの開始、呼び出し、または呼び出しの方法 (現在のコマンドを呼び出したスクリプトの名前など) に関する情報を格納します。

`$MyInvocation` は、スクリプト、関数、およびスクリプトブロックに対してのみ設定されます。 現在のスクリプトの `$MyInvocation` パスとファイル名 ( `$MyInvocation.MyCommand.Path` )、または関数の名前 () を使用して現在のコマンドを `$MyInvocation.MyCommand.Name` 識別することで、InvocationInfo オブジェクトの情報を使用することができます。 これは、現在のスクリプトの名前を検索する場合に特に便利です。

PowerShell 3.0 以降では、に `MyInvocation` 次の新しいプロパティが追加されています。

| プロパティ      | Description                                         |
| ------------- | --------------------------------------------------- |
| **PSScriptRoot**  | 呼び出されたスクリプトへの完全なパスを格納します。   |
|               | 現在のコマンド。 このプロパティの値は次のようになります。  |
|               | 呼び出し元がスクリプトの場合にのみ設定されます。         |
| **PSCommandPath** | スクリプトの完全なパスとファイル名が含まれます。  |
|               | 現在のコマンドを呼び出した。 このの値 |
|               | プロパティは、呼び出し元がである場合にのみ設定されます。     |
|               |  スクリプトを入手してください。                                             |

`$PSScriptRoot`および自動変数とは異なり `$PSCommandPath` 、自動変数の **psscriptroot** および **psscriptroot** プロパティには、現在のスクリプトでは `$MyInvocation` なく、呼び出し元または呼び出し元のスクリプトに関する情報が含まれます。

### <a name="nestedpromptlevel"></a>$NestedPromptLevel

現在のプロンプトレベルを格納します。 値が0の場合は、元のプロンプトレベルを示します。 この値は、入れ子になったレベルを入力するとインクリメントされ、終了時にはデクリメントされます。

たとえば、PowerShell では、メソッドを使用すると、入れ子になったコマンドプロンプトが表示さ `$Host.EnterNestedPrompt` れます。 Powershell デバッガーでブレークポイントに到着すると、入れ子になったコマンドプロンプトも表示されます。

入れ子になったプロンプトを入力すると、PowerShell は現在のコマンドを一時停止し、実行コンテキストを保存して、変数の値をインクリメントし `$NestedPromptLevel` ます。 入れ子になったコマンドプロンプト (最大128レベル) を作成するか、元のコマンドプロンプトに戻るには、コマンドを実行するか、「」と入力 `exit` します。

変数は、 `$NestedPromptLevel` プロンプトレベルを追跡するのに役立ちます。 この値を含む別の PowerShell コマンドプロンプトを作成して、常に表示されるようにすることができます。

### <a name="null"></a>$null

`$null`**null** または空の値を含む自動変数を指定します。 この変数を使用すると、コマンドやスクリプトに存在しない値または未定義の値を表すことができます。

PowerShell `$null` は、値を持つオブジェクト (つまり、明示的なプレースホルダー) としてを処理します。これにより、を使用して `$null` 一連の値で空の値を表すことができます。

たとえば、 `$null` コレクションにが含まれている場合、オブジェクトの1つとしてカウントされます。

```powershell
$a = "one", $null, "three"
$a.count
```

```Output
3
```

パイプを使用して変数をコマンドレットに渡した場合、 `$null` 他の `ForEach-Object` `$null` オブジェクトの場合と同様に、の値が生成されます。

```powershell
"one", $null, "three" | ForEach-Object { "Hello " + $_}
```

```Output
Hello one
Hello
Hello three
```

このため、を使用して `$null` **パラメーター値を指定** することはできません。 パラメーター値がの場合、 `$null` 既定のパラメーター値よりも優先されます。

ただし、PowerShell では `$null` 変数がプレースホルダーとして扱われるため、次のようなスクリプトで使用できます。これは、が無視された場合には機能し `$null` ません。

```powershell
$calendar = @($null, $null, "Meeting", $null, $null, "Team Lunch", $null)
$days = "Sunday","Monday","Tuesday","Wednesday","Thursday",
        "Friday","Saturday"
$currentDay = 0
foreach($day in $calendar)
{
    if($day -ne $null)
    {
        "Appointment on $($days[$currentDay]): $day"
    }

    $currentDay++
}
```

```Output
Appointment on Tuesday: Meeting
Appointment on Friday: Team lunch
```

### <a name="pid"></a>$PID

現在の PowerShell セッションをホストしているプロセスのプロセス識別子 (PID) を格納します。

### <a name="profile"></a>$PROFILE

現在のユーザーと現在のホストアプリケーションの PowerShell プロファイルの完全パスを含みます。 この変数を使用すると、コマンドでプロファイルを表すことができます。 たとえば、コマンドで使用して、プロファイルが作成されているかどうかを確認できます。

```powershell
Test-Path $PROFILE
```

または、コマンドで使用してプロファイルを作成することもできます。

```powershell
New-Item -ItemType file -Path $PROFILE -Force
```

これをコマンドで使用すると、 **notepad.exe** でプロファイルを開くことができます。

```powershell
notepad.exe $PROFILE
```

### <a name="psboundparameters"></a>$PSBoundParameters

スクリプトまたは関数に渡されるパラメーターのディクショナリと、その現在の値を格納します。 この変数の値は、スクリプトや関数など、パラメーターが宣言されているスコープ内にのみ存在します。 これを使用すると、パラメーターの現在の値を表示または変更したり、別のスクリプトまたは関数にパラメーター値を渡したりすることができます。

この例では、 **Test2** 関数はを `$PSBoundParameters` **Test1** 関数に渡します。 は、 `$PSBoundParameters` **キー** と **値** の形式で表示されます。

```powershell
function Test1 {
   param($a, $b)

   # Display the parameters in dictionary format.
   $PSBoundParameters
}

function Test2 {
   param($a, $b)

   # Run the Test1 function with $a and $b.
   Test1 @PSBoundParameters
}
```

```powershell
Test2 -a Power -b Shell
```

```Output
Key   Value
---   -----
a     Power
b     Shell
```

### <a name="pscmdlet"></a>$PSCmdlet

実行されているコマンドレットまたは高度な関数を表すオブジェクトを格納します。

コマンドレットまたは関数コードでオブジェクトのプロパティとメソッドを使用して、使用条件に応答できます。 たとえば、 **ParameterSetName** プロパティには、使用されているパラメーターセットの名前が含まれます **。このメソッドは** 、 **WhatIf** および **Confirm** パラメーターをコマンドレットに動的に追加します。

自動変数の詳細については `$PSCmdlet` 、「 [about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md) 」および「 [about_Functions_Advanced](about_Functions_Advanced.md)」を参照してください。

### <a name="pscommandpath"></a>$PSCommandPath

実行されているスクリプトの完全なパスとファイル名が含まれます。 この変数は、すべてのスクリプトで有効です。

### <a name="psculture"></a>$PSCulture

PowerShell 7 以降では、は `$PSCulture` 現在の powershell 実行空間 (セッション) のカルチャを反映しています。 PowerShell 実行空間でカルチャが変更された場合、 `$PSCulture` その実行空間の値が更新されます。

カルチャによって、数値、通貨、日付などの項目の表示形式が決定され、 **システムの CultureInfo** オブジェクトに格納されます。 `Get-Culture`コンピューターのカルチャを表示するには、を使用します。 `$PSCulture`**Name** プロパティの値を格納します。

### <a name="psdebugcontext"></a>$PSDebugContext

デバッグ中に、この変数にはデバッグ環境に関する情報が含まれます。 それ以外の場合は、 **null** 値が含まれます。 その結果、デバッガーにコントロールがあるかどうかを示すために使用できます。 値が設定されると、**ブレークポイント** と **InvocationInfo** プロパティを持つ **psdebugcontext** オブジェクトが格納されます。 **InvocationInfo** プロパティには、 **Location** プロパティなど、いくつかの便利なプロパティがあります。 **Location** プロパティは、デバッグされているスクリプトのパスを示します。

### <a name="pshome"></a>$PSHOME

PowerShell のインストールディレクトリ (通常は Windows システム) の完全なパスが含まれ `$env:windir\System32\PowerShell\v1.0` ます。 この変数は、PowerShell ファイルのパスで使用できます。 たとえば、次のコマンドは、概念説明ヘルプトピックで word **変数** を検索します。

```powershell
Select-String -Pattern Variable -Path $pshome\*.txt
```

### <a name="psitem"></a>$PSItem

`$_` と同じ。 パイプラインオブジェクトの現在のオブジェクトを格納します。 この変数は、パイプライン内のすべてのオブジェクトまたは選択したオブジェクトに対してアクションを実行するコマンドで使用できます。

### <a name="psscriptroot"></a>$PSScriptRoot

スクリプトの実行元のディレクトリが含まれます。

PowerShell 2.0 では、この変数はスクリプトモジュール () でのみ有効です `.psm1` 。
PowerShell 3.0 以降、すべてのスクリプトで有効になります。

### <a name="pssenderinfo"></a>$PSSenderInfo

PSSession を開始したユーザーに関する情報が含まれます。これには、元のコンピューターのユーザー id とタイムゾーンが含まれます。 この変数は、PSSessions でのみ使用できます。

変数には、 `$PSSenderInfo` ユーザーが構成できるプロパティ **applicationarguments** が含まれています。既定では、元のセッションからのみが含まれ `$PSVersionTable` ます。 **Applicationarguments** プロパティにデータを追加するには、コマンドレットの **applicationarguments** パラメーターを使用し `New-PSSessionOption` ます。

### <a name="psuiculture"></a>$PSUICulture

オペレーティングシステムで現在使用されているユーザーインターフェイス (UI) カルチャの名前を格納します。 UI カルチャは、どのテキスト文字列がメニューやメッセージなどのユーザー インターフェイス要素に使用されるかを決定します。 これは、システムの **System.Globalization.CultureInfo.CurrentUICulture.Name** プロパティの値です。 システムの **システムのグローバリゼーション** オブジェクトを取得するには、コマンドレットを使用し `Get-UICulture` ます。

### <a name="psversiontable"></a>$PSVersionTable

現在のセッションで実行されている PowerShell のバージョンに関する詳細を表示する読み取り専用のハッシュテーブルが含まれています。 この表には、次の項目が含まれています。

| プロパティ                  | Description                                   |
| ------------------------- | --------------------------------------------- |
| **PSVersion**             | PowerShell のバージョン番号                 |
| **PSEdition**             | このプロパティの値は、  |
|                           | Powershell 4 以降、PowerShell  |
|                           | 5.1 は、すべての機能を備えた Windows エディションに対応しています。        |
|                           | このプロパティの値は、     |
|                           | Powershell と powershell 6 以降  |
|                           | フットプリントが縮小されたエディションでの PowerShell 5.1  |
|                           | Windows Nano Server や Windows IoT と同様です。      |
| **GitCommitId**           | ソースファイルのコミット Id (GitHub) |
| **OS**                    | オペレーティングシステムの説明      |
|                           | PowerShell はで実行されています。                     |
| **プラットフォーム**              | オペレーティングシステムが実行されているプラットフォーム |
|                           | 。 Linux と macOS の値は **Unix** です。 |
|                           | `$IsMacOs` と `$IsLinux` を参照してください。                |
| **Ps互換バージョン**  | 互換性のある PowerShell のバージョン    |
|                           | 現在のバージョンで                      |
| **PSRemotingProtocolVersion** | PowerShell リモートのバージョン      |
|                           | 管理プロトコル。                          |
| **SerializationVersion**  | シリアル化メソッドのバージョン       |
| **WSManStackVersion**     | WS-Management スタックのバージョン番号 |

### <a name="pwd"></a>$PWD

現在のディレクトリの完全パスを表す path オブジェクトを格納します。

### <a name="sender"></a>$Sender

このイベントを生成したオブジェクトを格納します。 この変数は、イベント登録コマンドのアクションブロック内でのみ設定されます。 この変数の値は、を返す **PSEventArgs** オブジェクトの Sender プロパティにもあり `Get-Event` ます。

### <a name="shellid"></a>$ShellId

現在のシェルの識別子を格納します。

### <a name="stacktrace"></a>$StackTrace

最新のエラーのスタックトレースが含まれています。

### <a name="switch"></a>$switch

ステートメントの結果の値ではない列挙子を格納し `Switch` ます。 変数は、 `$switch` ステートメントの実行中にのみ存在し、 `Switch` ステートメントの実行が完了すると削除され `switch` ます。 詳細については、「 [about_Switch](about_Switch.md)」を参照してください。

列挙子には、ループ値を取得したり、現在のループの反復を変更したりするために使用できるプロパティとメソッドが含まれています。 詳細については、「 [列挙子の使用](#using-enumerators)」を参照してください。

### <a name="this"></a>$this

スクリプトプロパティまたはスクリプトメソッドを定義するスクリプトブロックでは、 `$this` 変数は拡張されるオブジェクトを参照します。

カスタムクラスでは、変数はクラス `$this` オブジェクト自体を参照し、クラスで定義されているプロパティとメソッドにアクセスできるようにします。

### <a name="true"></a>$true

**True** を格納します。 この変数を使用すると、コマンドとスクリプトで **True** を表すことができます。

## <a name="using-enumerators"></a>列挙子の使用

`$input`、 `$foreach` 、およびの `$switch` 各変数は、含まれているコードブロックによって処理される値を反復処理するために使用されるすべての列挙子です。

列挙子には、反復処理やイテレーション値の取得に使用できるプロパティとメソッドが含まれています。 列挙子を直接操作することは、ベストプラクティスとは見なされません。

- ループ内では、フロー制御キーワード [break](about_Break.md) と [continue](about_Continue.md) を優先する必要があります。
- パイプライン入力を受け入れる関数内では、 **Valuefrompipeline** 属性または **Valuefrompipelinebypropertyname** 属性でパラメーターを使用することをお勧めします。

  詳細については、「 [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)」を参照してください。

### <a name="movenext"></a>MoveNext

[MoveNext](/dotnet/api/system.collections.ienumerator.movenext)メソッドは、列挙子をコレクションの次の要素に進めます。 列挙子が正常に拡張された場合、 **MoveNext** は **True** を返します。列挙子がコレクションの末尾を越えた場合は **False** を返します。

> [!NOTE]
> 返さ **れた戻り値は** 、出力ストリームに **送信され** ます。
> 出力を抑制するには、出力を typecasting する `[void]` か、 [Out-Null](xref:Microsoft.PowerShell.Core.Out-Null)にパイプします。
>
> ```powershell
> $input.MoveNext() | Out-Null
> ```
>
> ```powershell
> [void]$input.MoveNext()
> ```

### <a name="reset"></a>Reset

[Reset](/dotnet/api/system.collections.ienumerator.reset)メソッドは、列挙子を初期位置、つまりコレクション内の最初の要素の **前** に設定します。

### <a name="current"></a>Current

[現在](/dotnet/api/system.collections.ienumerator.current)のプロパティは、列挙子の現在位置にある、コレクションまたはパイプライン内の要素を取得します。

**現在** のプロパティは、 **MoveNext** が呼び出されるまで、同じプロパティを返し続けます。

## <a name="examples"></a>例

### <a name="example-1-using-the-input-variable"></a>例 1: $input 変数の使用

次の例では、変数にアクセスすると、 `$input` 次にプロセスブロックが実行されるまで変数がクリアされます。 **Reset** メソッドを使用すると、 `$input` 変数が現在のパイプライン値にリセットされます。

```powershell
function Test
{
    begin
    {
        $i = 0
    }

    process
    {
        "Iteration: $i"
        $i++
        "`tInput: $input"
        "`tAccess Again: $input"
        $input.Reset()
        "`tAfter Reset: $input"
    }
}

"one","two" | Test
```

```Output
Iteration: 0
    Input: one
    Access Again:
    After Reset: one
Iteration: 1
    Input: two
    Access Again:
    After Reset: two
```

プロセスブロックは、 `$input` アクセスしない場合でも変数を自動的に進めます。

```powershell
$skip = $true
function Skip
{
    begin
    {
        $i = 0
    }

    process
    {
        "Iteration: $i"
        $i++
        if ($skip)
        {
            "`tSkipping"
            $skip = $false
        }
        else
        {
            "`tInput: $input"
        }
    }
}

"one","two" | Skip
```

```Output
Iteration: 0
    Skipping
Iteration: 1
    Input: two
```

### <a name="example-2-using-input-outside-the-process-block"></a>例 2: プロセスブロックの外部での $input の使用

プロセスブロックの外部では、 `$input` 変数は関数にパイプされたすべての値を表します。

- 変数にアクセスすると、 `$input` すべての値がクリアされます。
- **Reset** メソッドは、コレクション全体をリセットします。
- **現在** のプロパティは設定されません。
- コレクションを高度にすることはできないため、 **MoveNext** メソッドは false を返します。
  - **MoveNext** を呼び出すと、変数がクリアさ `$input` れます。

```powershell
Function All
{
    "All Values: $input"
    "Access Again: $input"
    $input.Reset()
    "After Reset: $input"
    $input.MoveNext() | Out-Null
    "After MoveNext: $input"
}

"one","two","three" | All
```

```Output
All Values: one two three
Access Again:
After Reset: one two three
After MoveNext:
```

### <a name="example-3-using-the-inputcurrent-property"></a>例 3: $input の使用現在のプロパティ

**現在** のプロパティを使用して、 **Reset** メソッドを使用せずに、現在のパイプライン値に複数回アクセスできます。 プロセスブロックは自動的に **MoveNext** メソッドを呼び出しません。

明示的に **MoveNext** を呼び出さない限り、**現在** のプロパティには値が設定されません。 **現在** のプロパティには、その値をクリアせずに、プロセスブロック内で複数回アクセスできます。

```powershell
function Current
{
    begin
    {
        $i = 0
    }

    process
    {
        "Iteration: $i"
        $i++
        "`tBefore MoveNext: $($input.Current)"
        $input.MoveNext() | Out-Null
        "`tAfter MoveNext: $($input.Current)"
        "`tAccess Again: $($input.Current)"
    }
}

"one","two" | Current
```

```Output
Iteration: 0
    Before MoveNext:
    After MoveNext: one
    Access Again: one
Iteration: 1
    Before MoveNext:
    After MoveNext: two
    Access Again: two
```

### <a name="example-4-using-the-foreach-variable"></a>例 4: $foreach 変数の使用

変数とは異なり、 `$input` 変数は、 `$foreach` 直接アクセスされるときに常にコレクション内のすべての項目を表します。 **現在のプロパティを** 使用して現在のコレクション要素にアクセスし、 **Reset** メソッドと **MoveNext** メソッドを使用してその値を変更します。

> [!NOTE]
> ループの各反復処理で `foreach` は、 **MoveNext** メソッドが自動的に呼び出されます。

次のループは2回だけ実行されます。 2番目の反復処理では、反復処理が完了する前にコレクションが3番目の要素に移動されます。 2番目の反復処理の後、反復処理する値がなくなり、ループが終了します。

**MoveNext** プロパティは、コレクション () を反復処理するために選択された変数には影響しません `$Num` 。

```powershell
$i = 0
foreach ($num in ("one","two","three"))
{
    "Iteration: $i"
    $i++
    "`tNum: $num"
    "`tCurrent: $($foreach.Current)"

    if ($foreach.Current -eq "two")
    {
        "Before MoveNext (Current): $($foreach.Current)"
        $foreach.MoveNext() | Out-Null
        "After MoveNext (Current): $($foreach.Current)"
        "Num has not changed: $num"
    }
}
```

```Output
Iteration: 0
        Num: one
        Current: one
Iteration: 1
        Num: two
        Current: two
Before MoveNext (Current): two
After MoveNext (Current): three
Num has not changed: two
```

**Reset** メソッドを使用すると、コレクション内の現在の要素がリセットされます。 次の例では、 **Reset** メソッドが呼び出されるため、最初の2つの要素を **2 回** ループします。 最初の2つのループの後、 `if` ステートメントは失敗し、ループは3つのすべての要素を通常どおり反復処理します。

> [!IMPORTANT]
> これにより、無限ループが発生する可能性があります。

```powershell
$stopLoop = 0
foreach ($num in ("one","two", "three"))
{
    ("`t" * $stopLoop) + "Current: $($foreach.Current)"

    if ($num -eq "two" -and $stopLoop -lt 2)
    {
        $foreach.Reset() | Out-Null
        ("`t" * $stopLoop) + "Reset Loop: $stopLoop"
        $stopLoop++
    }
}
```

```Output
Current: one
Current: two
Reset Loop: 0
        Current: one
        Current: two
        Reset Loop: 1
                Current: one
                Current: two
                Current: three
```

### <a name="example-5-using-the-switch-variable"></a>例 5: $switch 変数を使用する

変数には、 `$switch` 変数とまったく同じ規則があり `$foreach` ます。
次の例は、すべての列挙子の概念を示しています。

> [!NOTE]
> MoveNext メソッドの後にステートメントが存在しない場合でも、Note のよう **な大文字** と小文字の区別が実行されないことに注意して `break` ください。 

```powershell
$values = "Start", "MoveNext", "NotEvaluated", "Reset", "End"
$stopInfinite = $false
switch ($values)
{
    "MoveNext" {
        "`tMoveNext"
        $switch.MoveNext() | Out-Null
        "`tAfter MoveNext: $($switch.Current)"
    }
    # This case is never evaluated.
    "NotEvaluated" {
        "`tAfterMoveNext: $($switch.Current)"
    }

    "Reset" {
        if (!$stopInfinite)
        {
            "`tReset"
            $switch.Reset()
            $stopInfinite = $true
        }
    }

    default {
        "Default (Current): $($switch.Current)"
    }
}
```

```Output
Default (Current): Start
    MoveNext
    After MoveNext: NotEvaluated
    Reset
Default (Current): Start
    MoveNext
    After MoveNext: NotEvaluated
Default (Current): End
```

## <a name="see-also"></a>関連項目

[about_Functions](about_Functions.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Functions_OutputTypeAttribute](about_Functions_OutputTypeAttribute.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

[about_Hash_Tables](about_Hash_Tables.md)

[about_Preference_Variables](about_Preference_Variables.md)

[about_Splatting](about_Splatting.md)

[about_Variables](about_Variables.md)

