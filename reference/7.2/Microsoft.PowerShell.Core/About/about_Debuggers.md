---
description: PowerShell デバッガーについて説明します。
Locale: en-US
ms.date: 08/06/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_debuggers?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Debuggers
ms.openlocfilehash: 46914937cebdf8cf3559371f354bf14cd6706115
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605460"
---
# <a name="about-debuggers"></a>デバッガーについて

## <a name="short-description"></a>概要
PowerShell デバッガーについて説明します。

## <a name="long-description"></a>詳細説明

デバッグは、スクリプトの実行中にスクリプトを調べて、スクリプトの指示に従ってエラーを特定して修正するプロセスです。 PowerShell デバッガーを使用すると、スクリプト、関数、コマンド、PowerShell Desired State Configuration (DSC) 構成、または式のエラーや非効率性を確認し、識別することができます。

PowerShell 5.0 以降では、PowerShell デバッガーは、リモートコンピューター上のコンソールまたは Windows PowerShell ISE で実行されているスクリプト、関数、コマンド、構成、または式をデバッグするように更新されました。 を実行すると、 `Enter-PSSession` リモートコンピューター上でブレークポイントを設定したり、スクリプトファイルやコマンドをデバッグしたりできる対話型リモート PowerShell セッションを開始できます。 `Enter-PSSession` リモートコンピューターでスクリプトまたはコマンドを実行している切断されたセッションに再接続して入力できるように、機能が更新されました。 実行中のスクリプトがブレークポイントにヒットすると、クライアントセッションによってデバッガーが自動的に開始されます。 スクリプトを実行している切断されたセッションが既にブレークポイントにヒットし、ブレークポイントで停止した場合、は `Enter-PSSession` セッションに再接続した後、コマンドラインデバッガーを自動的に開始します。

PowerShell デバッガーの機能を使用して、PowerShell スクリプト、関数、コマンド、または式が実行中であるかどうかを調べることができます。 PowerShell デバッガーには、ブレークポイントの設定、ブレークポイントの管理、および呼び出し履歴の表示を行うためのコマンドレットのセットが含まれています。

## <a name="debugger-cmdlets"></a>デバッガーのコマンドレット

PowerShell デバッガーには、次の一連のコマンドレットが含まれています。

- `Set-PSBreakpoint`: 行、変数、およびコマンドにブレークポイントを設定します。
- `Get-PSBreakpoint`: 現在のセッションのブレークポイントを取得します。
- `Disable-PSBreakpoint`: 現在のセッションのブレークポイントをオフにします。
- `Enable-PSBreakpoint`: 現在のセッションのブレークポイントを再度有効にします。
- `Remove-PSBreakpoint`: 現在のセッションからブレークポイントを削除します。
- `Get-PSCallStack`: 現在の呼び出し履歴を表示します。

## <a name="starting-and-stopping-the-debugger"></a>デバッガーの起動と停止

デバッガーを起動するには、1つまたは複数のブレークポイントを設定します。 次に、デバッグするスクリプト、コマンド、または関数を実行します。

ブレークポイントに到着すると、実行が停止し、制御がデバッガーに対して有効になります。

デバッガーを停止するには、スクリプト、コマンド、または関数を完了するまで実行します。 または、 `stop` またはと入力 `t` します。

### <a name="debugger-commands"></a>デバッガー コマンド

PowerShell コンソールでデバッガーを使用する場合は、次のコマンドを使用して実行を制御します。 Windows PowerShell ISE では、[デバッグ] メニューのコマンドを使用します。

注: 他のホストアプリケーションでデバッガーを使用する方法については、ホストアプリケーションのドキュメントを参照してください。

- `s`、 `StepInto` : 次のステートメントを実行してから停止します。

- `v`、 `StepOver` : 次のステートメントを実行しますが、関数と呼び出しをスキップします。 スキップしたステートメントは実行されますが、ステップ スルーされることはありません。

- `Ctrl+Break`: (ISE 内のすべてを中断) は、PowerShell コンソールまたは Windows PowerShell ISE 内で実行中のスクリプトに分割されます。 <kbd></kbd> + Windows PowerShell 2.0、3.0、および4.0 で Ctrl<kbd>Break</kbd>を押すと、プログラムが閉じられることに注意してください。 Break All は、ローカルとリモートの対話形式で実行されるスクリプトの両方で機能します。

- `o`、 `StepOut` : 現在の関数からステップアウトします。入れ子になっている場合は、1つ上のレベルになります。 メインボディの場合は、最後または次のブレークポイントに進みます。 スキップしたステートメントは実行されますが、ステップ スルーされることはありません。

- `c`、 `Continue` : スクリプトが完了するまで、または次のブレークポイントに到達するまで実行を続けます。 スキップしたステートメントは実行されますが、ステップ スルーされることはありません。

- `l`、 `List` : 実行中のスクリプトの一部を表示します。 既定では、現在の行、前の5行、および10個の後続の行が表示されます。
  スクリプトの一覧を表示するには、enter キーを押します。

- `l <m>`、 `List` : によって指定された行番号で始まる16行のスクリプトが表示されます `<m>` 。

- `l <m> <n>`、 `List` : `<n>` によって指定された行番号で始まるスクリプトの行を表示 `<m>` します。

- `q`、 `Stop` 、 `Exit` : スクリプトの実行を停止し、デバッガーを終了します。 コマンドレットを実行してジョブをデバッグしている場合は、コマンドによって `Debug-Job` `Exit` デバッガーがデタッチされ、ジョブの実行を継続できるようになります。

- `k`、 `Get-PsCallStack` : 現在の呼び出し履歴を表示します。

- `<Enter>`: 最後のコマンドがステップ、Xy ピッチ (v)、またはリスト (l) である場合、それを繰り返します。 それ以外の場合は、送信アクションを表します。

- `?`、 `h` : デバッガーコマンドのヘルプを表示します。

デバッガーを終了するには、Stop (q) を使用します。

PowerShell 5.0 以降では、Exit コマンドを実行して、またはのいずれかを実行して開始した入れ子になったデバッグセッションを終了でき `Debug-Job` `Debug-Runspace` ます。

これらのデバッガーコマンドを使用して、スクリプトを実行し、問題点について停止し、変数の値とシステムの状態を調べて、問題が特定されるまでスクリプトの実行を続行することができます。

注: リダイレクト演算子 (">" など) を使用してステートメントにステップインすると、PowerShell デバッガーはスクリプト内の残りのすべてのステートメントをステップオーバーします。

スクリプト変数の値の表示

デバッガーでは、コマンドを入力したり、変数の値を表示したり、コマンドレットを使用したり、コマンドラインでスクリプトを実行したりすることもできます。

次の自動変数を除き、デバッグ対象のスクリプト内のすべての変数の現在の値を表示できます。

```powershell
$_
$Args
$Input
$MyInvocation
$PSBoundParameters
```

これらの変数のいずれかの値を表示しようとする場合は、スクリプト内の変数の値ではなく、デバッガーが使う内部パイプラインでその変数の値を取得します。

デバッグされているスクリプトのこれらの変数の値を表示するには、スクリプトで自動変数の値を新しい変数に割り当てます。 その後、新しい変数の値を表示できます。

たとえば、次のように入力します。

```powershell
$scriptArgs = $Args
$scriptArgs
```

このトピックの例では、変数の値が次のように再 `$MyInvocation` 割り当てされます。

```powershell
$scriptname = $MyInvocation.MyCommand.Path
```

## <a name="the-debugger-environment"></a>デバッガー環境

ブレークポイントにヒットしたら、デバッガー環境に入ります。 コマンドプロンプトが "[DBG]:" で始まるように変更されます。

プロンプトのカスタマイズの詳細については、「 [about_Prompts](about_prompts.md)」を参照してください。

また、PowerShell コンソールなどの一部のホストアプリケーション (Windows PowerShell Integrated Scripting Environment [ISE] ではない) では、デバッグのために入れ子になったプロンプトが開きます。 入れ子になったプロンプトを検出するには、コマンドプロンプトに表示される大なり記号 (ASCII 62) を繰り返します。

たとえば、PowerShell コンソールの既定のデバッグプロンプトは次のようになります。

```
[DBG]: PS (get-location)>>>
```

入れ子レベルは、自動変数を使用して見つけることができ `$NestedPromptLevel` ます。

また、自動変数は、 `$PSDebugContext` ローカルスコープで定義されます。 変数の存在を使用して、 `$PsDebugContext` デバッガーで実行されているかどうかを判断できます。

次に例を示します。

```powershell
if ($PSDebugContext) {"Debugging"} else {"Not Debugging"}
```

デバッグで変数の値を使用でき `$PSDebugContext` ます。

```
[DBG]: PS>>> $PSDebugContext.InvocationInfo

Name   CommandLineParameters  UnboundArguments  Location
----   ---------------------  ----------------  --------
=      {}                     {}                C:\ps-test\vote.ps1 (1)
```

## <a name="debugging-and-scope"></a>デバッグとスコープ

デバッガーを中断しても、操作中のスコープは変更されませんが、スクリプト内のブレークポイントに到着すると、スクリプトのスコープに移動します。 スクリプトスコープは、デバッガーを実行したスコープの子です。

スクリプトスコープで定義されている変数とエイリアスを検索するに `Get-Alias` は、またはコマンドレットの scope パラメーターを使用し `Get-Variable` ます。

たとえば、次のコマンドは、ローカル (スクリプト) スコープ内の変数を取得します。

```powershell
Get-Variable -scope 0
```

コマンドは次のように省略できます。

```powershell
gv -s 0
```

これは、スクリプトで定義した変数と、デバッグ中に定義した変数のみを表示するのに便利な方法です。

コマンドラインでのデバッグ

変数のブレークポイントまたはコマンドのブレークポイントを設定した場合は、スクリプトファイル内でのみブレークポイントを設定できます。 ただし、既定では、現在のセッションで実行されるすべてのものにブレークポイントが設定されます。

たとえば、変数にブレークポイントを設定した場合、 `$name` デバッガーは、 `$name` ブレークポイントを無効にするか削除するまで実行する任意のスクリプト、コマンド、関数、スクリプトコマンドレット、または式で、任意の変数を中断します。

これにより、セッションとユーザーのプロファイルの関数、変数、およびその他のスクリプトによって影響を受ける可能性のある、より現実的なコンテキストでスクリプトをデバッグすることができます。

行のブレークポイントはスクリプトファイルに固有であるため、スクリプトファイルでのみ設定されます。

## <a name="debugging-functions"></a>デバッグ関数

、、およびの各セクションを含む関数にブレークポイントを設定すると、 `Begin` `Process` `End` デバッガーは各セクションの最初の行で中断します。

次に例を示します。

```powershell
function test-cmdlet {
    begin {
        write-output "Begin"
    }
    process {
        write-output "Process"
    }
    end {
        write-output "End"
    }
}

C:\PS> Set-PSBreakpoint -command test-cmdlet

C:\PS> test-cmdlet

Begin
Entering debug mode. Use h or ? for help.

Hit Command breakpoint on 'prompt:test-cmdlet'

test-cmdlet

[DBG]: C:\PS> c
Process
Entering debug mode. Use h or ? for help.

Hit Command breakpoint on 'prompt:test-cmdlet'

test-cmdlet

[DBG]: C:\PS> c
End
Entering debug mode. Use h or ? for help.

Hit Command breakpoint on 'prompt:test-cmdlet'

test-cmdlet

# [DBG]: C:\PS>
```

## <a name="debugging-remote-scripts"></a>リモートスクリプトのデバッグ

PowerShell 5.0 以降では、コンソールまたは Windows PowerShell ISE で、リモートセッションで PowerShell デバッガーを実行できます。
`Enter-PSSession` リモートコンピューターで実行中の切断されたセッションに再接続して、現在スクリプトを実行できるように、機能が更新されました。 実行中のスクリプトがブレークポイントにヒットすると、クライアントセッションによってデバッガーが自動的に開始されます。

次に示すのは、行6、11、22、および25のスクリプトにブレークポイントを設定して、このしくみがどのように機能するかを示す例です。 この例では、デバッガーの起動時に2つの識別プロンプトがあります。セッションが実行されているコンピューターの名前と、デバッグモードであることを知らせる DBG プロンプトがあります。

```powershell
Enter-Pssession -Cn localhost
[localhost]: PS C:\psscripts> Set-PSBreakpoint .\ttest19.ps1 6,11,22,25

ID Script          Line     Command          Variable          Action
-- ------          ----     -------          --------          ------
0 ttest19.ps1          6
1 ttest19.ps1          11
2 ttest19.ps1          22
3 ttest19.ps1          25

[localhost]: PS C:\psscripts> .\ttest19.ps1
Hit Line breakpoint on 'C:\psscripts\ttest19.ps1:11'

At C:\psscripts\ttest19.ps1:11 char:1
+ $winRMName = "WinRM"
# + ~

[localhost]: [DBG]: PS C:\psscripts>> list

6:      1..5 | foreach { sleep 1; Write-Output "hello2day $_" }
7:  }
# 8:

9:  $count = 10
10:  $psName = "PowerShell"
11:* $winRMName = "WinRM"
12:  $myVar = 102
# 13:

14:  for ($i=0; $i -lt $count; $i++)
15:  {
16:      sleep 1
17:      Write-Output "Loop iteration is: $i"
18:      Write-Output "MyVar is $myVar"
# 19:

20:      hello2day
# 21:


[localhost]: [DBG]: PS C:\psscripts>> stepover
At C:\psscripts\ttest19.ps1:12 char:1
+ $myVar = 102
# + ~

[localhost]: [DBG]: PS C:\psscripts>> quit
[localhost]: PS C:\psscripts> Exit-PSSession
PS C:\psscripts>
```

## <a name="examples"></a>例

このテストスクリプトは、オペレーティングシステムのバージョンを検出し、システムに適切なメッセージを表示します。 これには、関数、関数呼び出し、および変数が含まれます。

次のコマンドを実行すると、テストスクリプトファイルの内容が表示されます。

```powershell
PS C:\PS-test>  Get-Content test.ps1

function psversion {
  "PowerShell " + $PSVersionTable.PSVersion
  if ($PSVersionTable.PSVersion.Major -lt 6) {
    "Upgrade to PowerShell 6.0!"
  }
  else {
    "Have you run a background job today (start-job)?"
  }
}

$scriptName = $MyInvocation.MyCommand.Path
psversion
"Done $scriptName."
```

まず、行、コマンド、変数、関数など、スクリプト内の目的の位置にブレークポイントを設定します。

まず、現在のディレクトリの Test.ps1 スクリプトの最初の行に行のブレークポイントを作成します。

```powershell
PS C:\ps-test> Set-PSBreakpoint -line 1 -script test.ps1
```

このコマンドは、省略形

```powershell
PS C:\ps-test> spb 1 -s test.ps1
```

このコマンドは、行ブレークポイントオブジェクト (system.string **ブレークポイント**) を返します。

```
Column     : 0
Line       : 1
Action     :
Enabled    : True
HitCount   : 0
Id         : 0
Script     : C:\ps-test\test.ps1
ScriptName : C:\ps-test\test.ps1
```

次に、スクリプトを開始します。

```powershell
PS C:\ps-test> .\test.ps1
```

スクリプトが最初のブレークポイントに到達すると、ブレークポイントメッセージはデバッガーがアクティブであることを示します。 ここでは、ブレークポイントについて説明し、関数宣言であるスクリプトの最初の行をプレビューします。 また、コマンドプロンプトは、デバッガーにコントロールがあることを示すためにも変更されます。

プレビュー行には、プレビューされたコマンドのスクリプト名と行番号が含まれています。

```powershell
Entering debug mode. Use h or ? for help.

Hit Line breakpoint on 'C:\ps-test\test.ps1:1'

test.ps1:1   function psversion {
# DBG>
```

ステップコマンドを使用して、スクリプト内の最初のステートメントを実行し、次のステートメントをプレビューします。 次のステートメントでは、自動変数を使用して、 `$MyInvocation` 変数の値を `$scriptName` スクリプトファイルのパスとファイル名に設定します。

```powershell
DBG> s
test.ps1:11  $scriptName = $MyInvocation.MyCommand.Path
```

この時点で、 `$scriptName` 変数には値が設定されませんが、変数の値を表示して変数の値を確認できます。 この場合、値は `$null` です。

```powershell
DBG> $scriptname
# DBG>
```

別のステップコマンドを使用して、現在のステートメントを実行し、スクリプト内の次のステートメントをプレビューします。 次のステートメントは、PsVersion 関数を呼び出します。

```powershell
DBG> s
test.ps1:12  psversion
```

この時点で、 `$scriptName` 変数に値が設定されていますが、変数の値を表示することによって変数の値を確認します。 この場合、値はスクリプトパスに設定されます。

```powershell
DBG> $scriptName
C:\ps-test\test.ps1
```

別のステップコマンドを使用して関数呼び出しを実行します。 ENTER キーを押すか、ステップに「s」と入力します。

```powershell
DBG> s
test.ps1:2       "PowerShell " + $PSVersionTable.PSVersion
```

デバッグメッセージには、関数内のステートメントのプレビューが含まれています。 このステートメントを実行し、関数の次のステートメントをプレビューするには、コマンドを使用し `Step` ます。 ただし、この場合は、StepOut コマンド (o) を使用します。 (ブレークポイントに到達しない限り) 関数の実行を完了し、スクリプト内の次のステートメントにステップインします。

```powershell
DBG> o
Windows PowerShell 2.0
Have you run a background job today (start-job)?
test.ps1:13  "Done $scriptName"
```

スクリプトの最後のステートメントが実行されているので、Step、StepOut、Continue の各コマンドは同じ効果を持ちます。 この場合は、StepOut (o) を使用します。

```powershell
Done C:\ps-test\test.ps1
PS C:\ps-test>
```

StepOut コマンドは、最後のコマンドを実行します。 標準のコマンドプロンプトは、デバッガーが終了し、コマンドプロセッサに制御を返したことを示します。

次に、デバッガーを再度実行します。 まず、現在のブレークポイントを削除するには、 `Get-PsBreakpoint` `Remove-PsBreakpoint` コマンドレットとコマンドレットを使用します。 (ブレークポイントを再利用する可能性があると思われる場合は、 `Disable-PsBreakpoint` の代わりにコマンドレットを使用 `Remove-PsBreakpoint` してください)。

```powershell
PS C:\ps-test> Get-PSBreakpoint| Remove-PSBreakpoint
```

このコマンドは、省略形

```powershell
PS C:\ps-test> gbp | rbp
```

または、次のような関数を記述してコマンドを実行します。

```powershell
function delbr { gbp | rbp }
```

次に、変数にブレークポイントを作成し `$scriptname` ます。

```powershell
PS C:\ps-test> Set-PSBreakpoint -variable scriptname -script test.ps1
```

コマンドは次のように省略できます。

```powershell
PS C:\ps-test> sbp -v scriptname -s test.ps1
```

次に、スクリプトを開始します。 スクリプトが変数のブレークポイントに到達します。 既定のモードは Write であるため、変数の値を変更するステートメントの直前に実行が停止します。

```powershell
PS C:\ps-test> .\test.ps1
Hit Variable breakpoint on 'C:\ps-test\test.ps1:$scriptName'
(Write access)

test.ps1:11  $scriptName = $MyInvocation.MyCommand.Path
# DBG>
```

変数の現在の値を表示 `$scriptName` `$null` します。

```powershell
DBG> $scriptName
# DBG>
```

ステップコマンドを使用して、変数を設定するステートメントを実行します。
次に、変数の新しい値を表示し `$scriptName` ます。

```powershell
DBG> $scriptName
C:\ps-test\test.ps1
```powershell

Use a Step command (s) to preview the next statement in the script.

```powershell
DBG> s
test.ps1:12  psversion
```

次のステートメントは、PsVersion 関数の呼び出しです。 関数をスキップした後も実行するには、Xy ピッチコマンドを使用します。 ピッチを使用するときに関数を既に使用している場合は、効果はありません。 関数呼び出しが表示されますが、実行されません。

```powershell
DBG> v
Windows PowerShell 2.0
Have you run a background job today (start-job)?
test.ps1:13  "Done $scriptName"
```

Xy ピッチコマンドは関数を実行し、スクリプト内の次のステートメントをプレビューします。最後の行が出力されます。

停止コマンド (t) を使用してデバッガーを終了します。 コマンドプロンプトが標準のコマンドプロンプトに戻ります。

```powershell
C:\ps-test>
```

ブレークポイントを削除するには `Get-PsBreakpoint` 、 `Remove-PsBreakpoint` コマンドレットとコマンドレットを使用します。

```powershell
PS C:\ps-test> Get-PSBreakpoint| Remove-PSBreakpoint
```

PsVersion 関数に新しいコマンドのブレークポイントを作成します。

```powershell
PS C:\ps-test> Set-PSBreakpoint -command psversion -script test.ps1
```

このコマンドは、次のように省略できます。

```powershell
PS C:\ps-test> sbp -c psversion -s test.ps1
```

次に、スクリプトを実行します。

```powershell
PS C:\ps-test> .\test.ps1
Hit Command breakpoint on 'C:\ps-test\test.ps1:psversion'

test.ps1:12  psversion
# DBG>
```

スクリプトは、関数呼び出しのブレークポイントに到達します。 この時点で、関数はまだ呼び出されていません。 これにより、の Action パラメーターを使用して、 `Set-PSBreakpoint` ブレークポイントの実行条件を設定したり、ログの開始や診断やセキュリティスクリプトの呼び出しなどの準備タスクや診断タスクを実行したりすることができます。

アクションを設定するには、Continue コマンド (c) を使用してスクリプトを終了し、コマンドを使用して `Remove-PsBreakpoint` 現在のブレークポイントを削除します。 (ブレークポイントは読み取り専用であるため、現在のブレークポイントにアクションを追加することはできません)。

```powershell
DBG> c
Windows PowerShell 2.0
Have you run a background job today (start-job)?
Done C:\ps-test\test.ps1

PS C:\ps-test> Get-PSBreakpoint| Remove-PSBreakpoint
PS C:\ps-test>
```

ここで、アクションを使用して新しいコマンドブレークポイントを作成します。 次のコマンドは、 `$scriptName` 関数が呼び出されたときに変数の値をログに記録するアクションを使用して、コマンドのブレークポイントを設定します。 Break キーワードがアクションで使用されていないため、実行は停止されません。 (バックティック (') は、行連結文字です)。

```powershell
PS C:\ps-test> Set-PSBreakpoint -command psversion -script test.ps1  `
-action { add-content "The value of `$scriptName is $scriptName." `
-path action.log}
```

また、ブレークポイントの条件を設定するアクションを追加することもできます。 次のコマンドでは、実行ポリシーが RemoteSigned に設定されている場合にのみ、コマンドのブレークポイントが実行されます。これは、スクリプトの実行を許可する最も制限の厳しいポリシーです。 (バックティック (') は継続文字です)。

```powershell
PS C:\ps-test> Set-PSBreakpoint -script test.ps1 -command psversion `
-action { if ((Get-ExecutionPolicy) -eq "RemoteSigned") { break }}
```

アクションの Break キーワードは、ブレークポイントを実行するようにデバッガーに指示します。
Continue キーワードを使用して、デバッガーを中断せずに実行するように指定することもできます。 Default キーワードは Continue であるため、実行を停止するには [中断] を指定する必要があります。

次に、スクリプトを実行します。

```powershell
PS C:\ps-test> .\test.ps1
Hit Command breakpoint on 'C:\ps-test\test.ps1:psversion'

test.ps1:12  psversion
```

実行ポリシーは **RemoteSigned** に設定されているため、関数呼び出しで実行が停止します。

この時点で、呼び出し履歴を確認することができます。 コマンド `Get-PsCallStack` レットまたは `Get-PsCallStack` デバッガーコマンド (k) を使用します。 次のコマンドは、現在の呼び出し履歴を取得します。

```powershell
DBG> k
2: prompt
1: .\test.ps1: $args=[]
0: prompt: $args=[]
```

この例では、PowerShell デバッガーを使用するさまざまな方法をいくつか示します。

デバッガーのコマンドレットの詳細については、次のコマンドを入力してください。

```powershell
help <cmdlet-name> -full
```

たとえば、次のように入力します。

```powershell
help Set-PSBreakpoint -full
```

## <a name="other-debugging-features-in-powershell"></a>PowerShell のその他のデバッグ機能

Powershell デバッガーに加えて、PowerShell には、スクリプトや関数のデバッグに使用できる機能が他にもいくつか用意されています。

- Windows PowerShell ISE には、対話型のグラフィカルデバッガーが含まれています。 詳細については、Windows PowerShell ISE を開始し、F1 キーを押してください。

- `Set-PSDebug`コマンドレットには、ステップ実行やトレースなど、基本的なスクリプトデバッグ機能が用意されています。

- コマンドレットを使用して、初期化されて `Set-StrictMode` いない変数への参照、オブジェクトの存在しないプロパティへの参照、および無効な関数構文を検出します。

- 変数の値を表示するステートメント、コマンドラインから入力を読み取るステートメント、現在の命令を報告するステートメントなど、診断ステートメントをスクリプトに追加します。 、、、など、このタスクの書き込み動詞を含むコマンドレットを使用し `Write-Host` `Write-Debug` `Write-Warning` `Write-Verbose` ます。

## <a name="see-also"></a>関連項目

- [無効にする-PsBreakpoint ポイント](xref:Microsoft.PowerShell.Utility.Disable-PSBreakpoint)
- [有効にする-PsBreakpoint ポイント](xref:Microsoft.PowerShell.Utility.Enable-PSBreakpoint)
- [取得-PsBreakpoint ポイント](xref:Microsoft.PowerShell.Utility.Get-PSBreakpoint)
- [Get-PsCallStack](xref:Microsoft.PowerShell.Utility.Get-PSCallStack)
- [-PsBreakpoint ポイントの削除](xref:Microsoft.PowerShell.Utility.Remove-PSBreakpoint)
- [Set-PSBreakpoint](xref:Microsoft.PowerShell.Utility.Set-PSBreakpoint)
- [Write-Debug](xref:Microsoft.PowerShell.Utility.Write-Debug)
- [Write-Verbose](xref:Microsoft.PowerShell.Utility.Write-Verbose)

