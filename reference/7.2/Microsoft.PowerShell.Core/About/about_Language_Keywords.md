---
description: PowerShell スクリプト言語のキーワードについて説明します。
Locale: en-US
ms.date: 10/06/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_language_keywords?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Language_Keywords
ms.openlocfilehash: 4136e5fe6b0e3ad2ba1ec41d2dc9a8fba7b88f56
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600974"
---
# <a name="about-language-keywords"></a>言語のキーワードについて

## <a name="short-description"></a>概要
PowerShell スクリプト言語のキーワードについて説明します。

## <a name="long-description"></a>詳細説明

PowerShell には、次の言語キーワードがあります。 詳細については、キーワードの about トピックと、表の後にある情報を参照してください。

キーワード     | 関連項目
---         | ---
開始       | [about_Functions](about_Functions.md)、 [about_Functions_Advanced](about_Functions_Advanced.md)
分解       | [about_Break](about_Break.md)、 [about_Trap](about_Trap.md)
キャッチ       | [about_Try_Catch_Finally](about_Try_Catch_Finally.md)
インスタンス       | [about_Classes](about_Classes.md)
続行    | [about_Continue](about_Continue.md)、 [about_Trap](about_Trap.md)
データ        | [about_Data_Sections](about_Data_Sections.md)
定義      | 予約済み
推奨          | [about_Do](about_Do.md)、 [about_While](about_While.md)
DynamicParam| [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)
Else        | [about_If](about_If.md)
Elseif      | [about_If](about_If.md)
End         | [about_Functions](about_Functions.md)、 [about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)
列挙型        | [about_Enum](about_Enum.md)
終了        | [このトピックで説明します。](#exit)
Assert      | [about_Functions](about_Functions.md)
Finally     | [about_Try_Catch_Finally](about_Try_Catch_Finally.md)
For         | [about_For](about_For.md)
ForEach     | [about_ForEach](about_ForEach.md)
差出人        | 予約済み
Function    | [about_Functions](about_Functions.md)、 [about_Functions_Advanced](about_Functions_Advanced.md)
[非表示]      | [about_Hidden](about_Hidden.md)
If          | [about_If](about_If.md)
/          | [about_ForEach](about_ForEach.md)
Param       | [about_Functions](about_Functions.md)
プロセス     | [about_Functions](about_Functions.md)、 [about_Functions_Advanced](about_Functions_Advanced.md)
戻り値      | [about_Return](about_Return.md)
静的      | [about_Classes](about_Classes.md)
Switch      | [about_Switch](about_Switch.md)
Throw       | [about_Throw](about_Throw.md)、 [about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)
Trap        | [about_Trap](about_Trap.md)、 [about_Break](about_Break.md)、 [about_Try_Catch_Finally](about_Try_Catch_Finally.md)
試す         | [about_Try_Catch_Finally](about_Try_Catch_Finally.md)
Until       | [about_Do](about_Do.md)
使用       | [about_Using](about_Using.md)、 [about_Classes](about_Classes.md)
Var         | 予約済み
While       | [about_While](about_While.md)、 [about_Do](about_Do.md)

言語キーワード

### <a name="begin"></a>開始

、、およびの各キーワードと共に、関数本体の1つの部分を指定し `DynamicParam` `Process` `End` ます。 `Begin`ステートメントリストは、パイプラインからオブジェクトを受信する前に1回実行されます。

構文:

```Syntax
function <name> {
    DynamicParam {<statement list>}
    begin {<statement list>}
    process {<statement list>}
    end {<statement list>}
}
```

### <a name="break"></a>分解

スクリプトでループを終了させます。

構文:

```Syntax
while (<condition>) {
   <statements>
   ...

   break
   ...

   <statements>
}
```

### <a name="catch"></a>キャッチ

付随する Try ステートメントの一覧でエラーが発生した場合に実行するステートメントリストを指定します。 エラーの種類には角かっこが必要です。 2番目のかっこのペアは、エラーの種類が省略可能であることを示します。

構文:

```Syntax
try {<statement list>}
catch [[<error type>]] {<statement list>}
```

### <a name="class"></a>インスタンス

PowerShell の新しいクラスを指定します。

構文:

```Syntax
class <class-name> {
    [[hidden] [static] <property-definition> ...]
    [<class-name>([argument-list>]) {<constructor-statement-list>} ...]
    [[hidden] [static] <method-definition> ...]
}
```

### <a name="continue"></a>続行

スクリプトがループの実行を停止し、条件に戻るようにします。 条件が満たされると、スクリプトはループを再び開始します。

構文:

```Syntax
while (<condition>) {
   <statements>
   ...

   continue
   ...

   <statements>
}
```

### <a name="data"></a>データ

スクリプトでは、は、スクリプトロジックからデータを分離するセクションを定義します。 `If`ステートメントおよびいくつかの制限されたコマンドを含めることもできます。

構文:

```Syntax
data <variable> [-supportedCommand <cmdlet-name>] {<permitted content>}
```

### <a name="do"></a>推奨

`While`ループ構造として、キーワードまたはキーワードと共に使用され `Until` ます。 を使用するループとは異なり、PowerShell では、ステートメントの一覧が少なくとも1回実行され `While` ます。

`While` の構文:

```Syntax
do {<statement list>} while (<condition>)
```

`Until` の構文:

```Syntax
do {<statement list>} until (<condition>)
```

### <a name="dynamicparam"></a>DynamicParam

、、およびの各キーワードと共に、関数本体の1つの部分を指定し `Begin` `Process` `End` ます。 動的パラメーターは、実行時に追加されます。

構文:

```Syntax
function <name> {
   DynamicParam {<statement list>}
   begin {<statement list>}
   process {<statement list>}
   end {<statement list>}
}
```

### <a name="else"></a>Else

`If`既定のステートメントリストを指定するためにキーワードと共に使用されます。

構文:

```Syntax
if (<condition>) {<statement list>}
else {<statement list>}
```

### <a name="elseif"></a>Elseif

`If`追加の条件を `Else` 指定するために、キーワードおよびキーワードと共に使用します。 `Else`キーワードは省略可能です。

構文:

```Syntax
if (<condition>) {<statement list>}
elseif (<condition>) {<statement list>}
else {<statement list>}
```

### <a name="end"></a>End

、、およびの各キーワードと共に、関数本体の1つの部分を指定し `DynamicParam` `Begin` `End` ます。 ステートメントの一覧は、 `End` すべてのオブジェクトがパイプラインから受信された後に1回実行されます。

構文:

```Syntax
function <name> {
   DynamicParam {<statement list>}
   begin {<statement list>}
   process {<statement list>}
   end {<statement list>}
}
```

### <a name="enum"></a>列挙型

`enum` は、列挙型を宣言するために使用されます。列挙子リストと呼ばれる一連の名前付きラベルで構成される個別の型。

構文:

```Syntax
enum <enum-name> {
    <label> [= <int-value>]
    ...
}
```

### <a name="exit"></a>終了

PowerShell でスクリプトまたは PowerShell インスタンスを終了させます。

構文:

```Syntax
exit
exit <exitcode>
```

ファイルパラメーターを指定してを使用する場合、 `pwsh`  `.ps1` (スクリプト) ファイル自体には、スクリプトの実行中に発生したエラーや例外を処理するための命令を含める必要があります。 ステートメントは、 `exit` スクリプトの実行後の状態を示すためにのみ使用してください。

Windows では、との間に任意の数の数値を `[int]::MinValue` `[int]::MaxValue` 指定できます。

Unix では、との間に正の数値のみ `[byte]::MinValue` `[byte]::MaxValue` が許可されます。 からまでの範囲の負の `-1` 数値 `-255` は、256を加算することによって自動的に正の数値に変換されます。 たとえば、 `-2` はに変換され `254` ます。

PowerShell では、ステートメントによって `exit` 変数の値が設定され `$LASTEXITCODE` ます。 Windows コマンドシェル (cmd.exe) では、exit ステートメントによって環境変数の値が設定され `%ERRORLEVEL%` ます。

数値以外、またはプラットフォーム固有の範囲外の引数は、の値に変換され `0` ます。

次の例では、ユーザーがスクリプトファイルにを追加して、エラーレベル変数の値を4に設定し `exit 4` `test.ps1` ます。

```cmd
C:\scripts\test>type test.ps1
1
2
3
exit 4

C:\scripts\test>pwsh -file ./test.ps1
1
2
3

C:\scripts\test>echo %ERRORLEVEL%
4
```

を実行し、スクリプトファイルがコマンドで終了すると、 `pwsh.exe -File <path to a script>` `exit` 終了コードはコマンドで使用される数値引数に設定され `exit` ます。 スクリプトにステートメントがない場合 `exit` 、終了コードは常に `0` スクリプトがエラーなしで完了したとき、またはスクリプトがハンドルされない例外から終了したときに発生し `1` ます。

### <a name="filter"></a>Assert

各入力オブジェクトに対してステートメントリストを1回実行する関数を指定します。 これは、プロセスブロックだけを含む関数と同じ効果があります。

構文:

```Syntax
filter <name> {<statement list>}
```

### <a name="finally"></a>Finally

Try および Catch に関連付けられたステートメントの後に実行されるステートメントリストを定義します。 ステートメントリストは、を押してスクリプトを終了した `Finally` 場合 `CTRL+C` や、スクリプトで Exit キーワードを使用した場合にも実行されます。

構文:

```Syntax
try {<statement list>}
catch [<error type>] {<statement list>}
finally {<statement list>}
```

### <a name="for"></a>For

条件を使用してループを定義します。

構文:

```Syntax
for (<initialize>; <condition>; <iterate>) { <statement list> }
```

### <a name="foreach"></a>ForEach

コレクションの各メンバーを使用してループを定義します。

構文:

```Syntax
ForEach (<item> in <collection>) { <statement list> }
```

### <a name="from"></a>差出人

将来使用するために予約されています。

### <a name="function"></a>Function

再利用可能なコードの名前付きステートメントリストを作成します。 関数が属するスコープに名前を指定できます。 また、キーワードを使用して、1つまたは複数の名前付きパラメーターを指定することもでき `Param` ます。 Function ステートメントリスト内で、、、 `DynamicParam` `Begin` `Process` 、およびの各ステートメントリストを含めることができ `End` ます。

構文:

```Syntax
function [<scope:>]<name> {
   param ([type]<$pname1> [, [type]<$pname2>])
   DynamicParam {<statement list>}
   begin {<statement list>}
   process {<statement list>}
   end {<statement list>}
}
```

また、関数名の後に1つ以上のパラメーターを定義するオプションもあります。

構文:

```Syntax
function [<scope:>]<name> [([type]<$pname1>, [[type]<$pname2>])] {
   DynamicParam {<statement list>}
   begin {<statement list>}
   process {<statement list>}
   end {<statement list>}
}
```

### <a name="if"></a>If

条件付きを定義します。

構文:

```Syntax
if (<condition>) {<statement list>}
```

### <a name="hidden"></a>[非表示]

コマンドレットの既定の結果、 `Get-Member` および IntelliSense とタブ補完の結果からクラスメンバーを非表示にします。

構文:

```Syntax
Hidden [data type] $member_name
```

### <a name="in"></a>/

`ForEach`コレクションの各メンバーを使用するループを作成するために、ステートメントで使用されます。

構文:

```Syntax
ForEach (<item> in <collection>){<statement list>}
```

### <a name="inlinescript"></a>InlineScript

共有 PowerShell セッションでワークフローコマンドを実行します。 このキーワードは、PowerShell ワークフローでのみ有効です。

構文:

```Syntax
workflow <verb>-<noun>
{
   InlineScript
   {
      <Command/Expression>
      ...

   }
}
```

キーワードは、 `InlineScript` `InlineScript` 共有された標準 (非ワークフロー) セッションでコマンドを実行するアクティビティを示します。 キーワードを使用して、 `InlineScript` ワークフローでは有効でないコマンドを実行したり、データを共有するコマンドを実行したりできます。 既定では、InlineScript スクリプトブロック内のコマンドは別のプロセスで実行されます。

詳細については、「 [ワークフローでの PowerShell コマンドの実行](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574197(v=ws.11))」を参照してください。

### <a name="param"></a>Param

関数のパラメーターを定義します。

構文:

```Syntax
function [<scope:>]<name> {
   param ([type]<$pname1>[, [[type]<$pname2>]])
   <statement list>
}
```

### <a name="process"></a>プロセス

、、およびの各キーワードと共に、関数本体の一部を指定し `DynamicParam` `Begin` `End` ます。 `Process`ステートメントリストがパイプラインからの入力を受け取ると、 `Process` パイプラインの各要素に対してステートメントリストが1回実行されます。 パイプラインがオブジェクトを提供しない場合、 `Process` ステートメントの一覧は実行されません。 コマンドがパイプラインの最初のコマンドである場合、 `Process` ステートメントリストは1回だけ実行されます。

構文:

```Syntax
function <name> {
   DynamicParam {<statement list>}
   begin {<statement list>}
   process {<statement list>}
   end {<statement list>}
}
```

### <a name="return"></a>戻り値

PowerShell を使用して、スクリプトや関数などの現在のスコープをそのままにし、省略可能な式を出力に書き込みます。

構文:

```Syntax
return [<expression>]
```

### <a name="static"></a>静的

が定義されているクラスのすべてのインスタンスに共通するプロパティまたはメソッドを指定します。

`Class`使用例については、「」を参照してください。

### <a name="switch"></a>Switch

複数の条件を確認するには、ステートメントを使用し `Switch` ます。 `Switch`ステートメントは一連のステートメントに相当しますが、より `If` 簡単です。

ステートメントでは、 `Switch` 各条件とオプションのアクションが一覧表示されます。 条件がを取得すると、アクションが実行されます。

構文 1:

```Syntax
switch [-regex|-wildcard|-exact][-casesensitive] ( <value> )
{
   <string>|<number>|<variable>|{ <expression> } {<statement list>}
   <string>|<number>|<variable>|{ <expression> } {<statement list>}
   ...

   default {<statement list>}
}
```

構文 2:

```Syntax
switch [-regex|-wildcard|-exact][-casesensitive] -file <filename>
{
   <string>|<number>|<variable>|{ <expression> } {<statement list>}
   <string>|<number>|<variable>|{ <expression> } {<statement list>}
   ...

   default {<statement list>}
}
```

### <a name="throw"></a>Throw

オブジェクトをエラーとしてスローします。

構文:

```Syntax
throw [<object>]
```

### <a name="trap"></a>Trap

エラーが発生した場合に実行されるステートメントリストを定義します。 エラーの種類には角かっこが必要です。 2番目のかっこのペアは、エラーの種類が省略可能であることを示します。

構文:

```Syntax
trap [[<error type>]] {<statement list>}
```

### <a name="try"></a>試す

ステートメントの実行中にエラーをチェックするステートメントリストを定義します。 エラーが発生した場合、PowerShell `Catch` はまたはステートメントで実行を継続 `Finally` します。 エラーの種類には角かっこが必要です。 2番目のかっこのペアは、エラーの種類が省略可能であることを示します。

構文:

```Syntax
try {<statement list>}
catch [[<error type>]] {<statement list>}
finally {<statement list>}
```

### <a name="until"></a>Until

ステートメント `Do` リストが少なくとも1回実行されるループ構造として、ステートメントで使用されます。

構文:

```Syntax
do {<statement list>} until (<condition>)
```

### <a name="using"></a>使用

セッションで使用されている名前空間を示すことができます。 クラスとメンバーは、それを説明するために、少ない入力を必要とします。 モジュールのクラスを含めることもできます。

構文 #1:

```Syntax
using namespace <.Net-framework-namespace>
```

構文 #2:

```Syntax
using module <module-name>
```

### <a name="while"></a>While

ステートメントは、 `while` ステートメントが実行される前に条件がテストされるループ構造です。 条件が FALSE の場合、ステートメントは実行されません。

ステートメントの構文:

```Syntax
while (<condition>) {
   <statements>
 }
```

ステートメントで使用される場合 `Do` 、 `while` は、ステートメントリストが少なくとも1回実行されるループ構造の一部です。

Do ループ構文:

```Syntax
do {<statement list>} while (<condition>)
```

## <a name="see-also"></a>関連項目

- [about_Special_Characters](about_Special_Characters.md)
- [about_Wildcards](about_Wildcards.md)

