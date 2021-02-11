---
title: switch ステートメントについて知りたかったことのすべて
description: PowerShell の switch ステートメントには、他の言語では見つからない機能が用意されています。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: c2e77aa5fb36d04fec1bc86f751291205120c729
ms.sourcegitcommit: 39c2a697228276d5dae39e540995fa479c2b5f39
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93355121"
---
# <a name="everything-you-ever-wanted-to-know-about-the-switch-statement"></a>switch ステートメントについて知りたかったことのすべて

他の多くの言語と同様に、PowerShell には、スクリプト内の実行フローを制御するためのコマンドが用意されています。 このようなステートメントの 1 つが [switch][] ステートメントであり、PowerShell 内では他の言語にはない機能を提供します。 本日は、PowerShell `switch` の使用について詳しく説明します。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="the-if-statement"></a>`if` ステートメント

最初に学習するステートメントの 1 つは、`if` ステートメントです。 ステートメントが `$true` の場合にスクリプト ブロックが実行されるようにできます。

``` powershell
if ( Test-Path $Path )
{
    Remove-Item $Path
}
```

`elseif` および `else` ステートメントを使用すると、より複雑なロジックを作成できます。 ここでは、曜日の数値を使用して、名前を文字列として取得する例を示します。

``` powershell
$day = 3

if ( $day -eq 0 ) { $result = 'Sunday'        }
elseif ( $day -eq 1 ) { $result = 'Monday'    }
elseif ( $day -eq 2 ) { $result = 'Tuesday'   }
elseif ( $day -eq 3 ) { $result = 'Wednesday' }
elseif ( $day -eq 4 ) { $result = 'Thursday'  }
elseif ( $day -eq 5 ) { $result = 'Friday'    }
elseif ( $day -eq 6 ) { $result = 'Saturday'  }

$result
```

```Output
Wednesday
```

これは一般的なパターンであり、これを処理する方法は多数あります。 その 1 つに `switch` を使用する方法があります。

## <a name="switch-statement"></a>Switch ステートメント

`switch` ステートメントを使用すると、変数と有効な値の一覧を指定できます。 値が変数に一致すると、そのスクリプト ブロックが実行されます。

``` powershell
$day = 3

switch ( $day )
{
    0 { $result = 'Sunday'    }
    1 { $result = 'Monday'    }
    2 { $result = 'Tuesday'   }
    3 { $result = 'Wednesday' }
    4 { $result = 'Thursday'  }
    5 { $result = 'Friday'    }
    6 { $result = 'Saturday'  }
}

$result
```

```Output
'Wednesday'
```

この例では、`$day` の値が数値のいずれかに一致すると、正しい名前が `$result` に代入されます。 この例では変数代入のみを行っていますが、これらのスクリプト ブロック内で任意の PowerShell を実行できます。

### <a name="assign-to-a-variable"></a>変数に代入する

この最後の例は、別の方法で記述できます。

``` powershell
$result = switch ( $day )
{
    0 { 'Sunday'    }
    1 { 'Monday'    }
    2 { 'Tuesday'   }
    3 { 'Wednesday' }
    4 { 'Thursday'  }
    5 { 'Friday'    }
    6 { 'Saturday'  }
}
```

PowerShell パイプラインに値を入れ、それを `$result` に代入しています。 これと同じことを、`if` および `foreach` ステートメントでも行うことができます。

### <a name="default"></a>Default

`default` キーワードを使用すると、一致するものがない場合にどうするかを指定できます。

``` powershell
$result = switch ( $day )
{
    0 { 'Sunday' }
    # ...
    6 { 'Saturday' }
    default { 'Unknown' }
}
```

ここでは、既定のケースで `Unknown` 値を返します。

### <a name="strings"></a>文字列

これらの最後の例では数値を照合しましたが、文字列を照合することもできます。

``` powershell
$item = 'Role'

switch ( $item )
{
    Component
    {
        'is a component'
    }
    Role
    {
        'is a role'
    }
    Location
    {
        'is a location'
    }
}
```

```Output
is a role
```

ここでは、一致項目 `Component`、`Role`、および `Location` を引用符で囲まないことにしました。引用符は省略可能であることを示すためです。 `switch` では、ほとんどの場合、これらは文字列として扱われます。

## <a name="arrays"></a>配列

PowerShell `switch` の優れた機能の 1 つは、配列の処理方法です。 `switch` に配列 を与えると、そのコレクション内の各要素が処理されます。

``` powershell
$roles = @('WEB','Database')

switch ( $roles ) {
    'Database'   { 'Configure SQL' }
    'WEB'        { 'Configure IIS' }
    'FileServer' { 'Configure Share' }
}
```

```Output
Configure IIS
Configure SQL
```

配列内に繰り返されている項目がある場合、それらは該当するセクションに複数回一致します。

### <a name="psitem"></a>PSItem

`$PSItem` または `$_` を使用すると、処理された現在の項目を参照できます。 単純な照合を行う場合、`$PSItem` は照合する値です。 次のセクションでは、この変数を使用して高度な照合をいくつか実行します。

## <a name="parameters"></a>パラメーター

PowerShell `switch` の固有の機能は、その実行方法を変更する[スイッチ パラメーター][]がいくつかあることです。

### <a name="-casesensitive"></a>-CaseSensitive

既定では、照合時に大文字と小文字は区別されません。 大文字と小文字を区別する必要がある場合は、`-CaseSensitive` を使用できます。 これは、他のスイッチ パラメーターと組み合わせて使用できます。

### <a name="-wildcard"></a>-Wildcard

`-wildcard` スイッチを使用すると、ワイルドカードのサポートを有効にできます。 これは、`-like` 演算子と同じワイルドカード ロジックを使用して、各照合を実行します。

``` powershell
$Message = 'Warning, out of disk space'

switch -Wildcard ( $message )
{
    'Error*'
    {
        Write-Error -Message $Message
    }
    'Warning*'
    {
        Write-Warning -Message $Message
    }
    default
    {
        Write-Information $message
    }
}
```

```Output
WARNING: Warning, out of disk space
```

ここでは、メッセージを処理し、コンテンツに基づいてさまざまなストリームに出力しています。

### <a name="-regex"></a>-Regex

switch ステートメントでは、ワイルドカードと同様に、正規表現照合がサポートされています。

``` powershell
switch -Regex ( $message )
{
    '^Error'
    {
        Write-Error -Message $Message
    }
    '^Warning'
    {
        Write-Warning -Message $Message
    }
    default
    {
        Write-Information $message
    }
}
```

次の記事では、正規表現を使用するその他の例を記載しています。[正規表現を使用するさまざまな方法][]。

### <a name="-file"></a>-File

switch ステートメントには、`-File` パラメーターを使用してファイルを処理できるという、あまり知られていない機能があります。 変数式を指定する代わりに、`-file` をファイルへのパスと共に使用します。

``` powershell
switch -Wildcard -File $path
{
    'Error*'
    {
        Write-Error -Message $PSItem
    }
    'Warning*'
    {
        Write-Warning -Message $PSItem
    }
    default
    {
        Write-Output $PSItem
    }
}
```

配列の処理と同じように動作します。 この例では、これをワイルドカード照合と組み合わせ、`$PSItem` を使用します。 これにより、ログ ファイルが処理され、正規表現の一致に応じて警告メッセージとエラーメッセージに変換されます。

## <a name="advanced-details"></a>詳細情報

ドキュメントに記載されているこれらすべての機能について理解したので、さらに高度な処理のコンテキストでそれらを使用してみましょう。

### <a name="expressions"></a>式

`switch` は、変数の代わりに式を対象にすることができます。

``` powershell
switch ( ( Get-Service | Where status -eq 'running' ).name ) {...}
```

式の評価結果が、照合に使用される値です。

### <a name="multiple-matches"></a>複数一致

既にお気付きかもしれませんが、`switch` は複数の条件に一致する場合があります。 これは、`-wildcard` または `-regex` 照合を使用する場合に特に当てはまります。 同じ条件を複数回追加すれば、それらすべてがトリガーされます。

``` powershell
switch ( 'Word' )
{
    'word' { 'lower case word match' }
    'Word' { 'mixed case word match' }
    'WORD' { 'upper case word match' }
}
```

```Output
lower case word match
mixed case word match
upper case word match
```

これら 3 つのステートメントがすべてトリガーされています。 これは、すべての条件が (順に) チェックされることを示しています。 これは配列を処理する場合にも当てはまり、各項目で各条件がチェックされます。

### <a name="continue"></a>Continue

通常は、ここで `break` ステートメントを紹介しますが、まず `continue` を使用する方法を学習する方がよいでしょう。 `foreach` ループと同様に、`continue` はコレクション内の次の項目に進み続け、それ以上項目がなくなると `switch` から抜けます。 ステートメントが 1 つだけ実行されるように、continue ステートメントを使用して最後の例を書き換えることができます。

``` powershell
switch ( 'Word' )
{
    'word'
    {
        'lower case word match'
        continue
    }
    'Word'
    {
        'mixed case word match'
        continue
    }
    'WORD'
    {
        'upper case word match'
        continue
    }
}
```

```Output
lower case word match
```

3 つの項目をすべて照合するのではなく、最初の項目が一致したところで switch は次の値に進みます。 処理する値が残っていないため、switch は終了します。 次の例は、ワイルドカードが複数の項目とどのように一致するかを示しています。

``` powershell
switch -Wildcard -File $path
{
    '*Error*'
    {
        Write-Error -Message $PSItem
        continue
    }
    '*Warning*'
    {
        Write-Warning -Message $PSItem
        continue
    }
    default
    {
        Write-Output $PSItem
    }
}
```

入力ファイル内の行に `Error` と `Warning` の両方の語が含まれている可能性があるため、最初のものだけを実行してから、ファイルの処理を続行します。

### <a name="break"></a>Break

`break` ステートメントは、switch を終了します。 これは、個々の値に対する `continue` の動作と同じです。 配列を処理するときに違いが示されます。 `break` は switch のすべての処理を停止し、`continue` は次の項目に移ります。

``` powershell
$Messages = @(
    'Downloading update'
    'Ran into errors downloading file'
    'Error: out of disk space'
    'Sending email'
    '...'
)

switch -Wildcard ($Messages)
{
    'Error*'
    {
        Write-Error -Message $PSItem
        break
    }
    '*Error*'
    {
        Write-Warning -Message $PSItem
        continue
    }
    '*Warning*'
    {
        Write-Warning -Message $PSItem
        continue
    }
    default
    {
        Write-Output $PSItem
    }
}
```

```Output
Downloading update
WARNING: Ran into errors downloading file
write-error -message $PSItem : Error: out of disk space
+ CategoryInfo          : NotSpecified: (:) [Write-Error], WriteErrorException
+ FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException
```

この場合、`Error` で始まる行に到達すると、エラーが表示され、switch が停止します。
これは、`break` ステートメントが行う処理です。 `Error` が文字列の内側または先頭に見つかった場合は、警告として記述します。 `Warning` についても同じことを行います。 行には `Error` と `Warning` の両方の語が含まれている可能性ありますが、処理に必要なのは 1 つだけです。 これは、`continue` ステートメントが行う処理です。

### <a name="break-labels"></a>break ラベル

`foreach` と同様に、`switch` ステートメントでは `break/continue` ラベルがサポートされています。

``` powershell
:filelist foreach($path in $logs)
{
    :logFile switch -Wildcard -File $path
    {
        'Error*'
        {
            Write-Error -Message $PSItem
            break filelist
        }
        'Warning*'
        {
            Write-Error -Message $PSItem
            break logFile
        }
        default
        {
            Write-Output $PSItem
        }
    }
}
```

私自身は break ラベルを使用することを好みませんが、見たことがない場合はわかりにくいので、説明したいと思います。 複数の `switch` または `foreach` ステートメントが入れ子になっているときに、最も内側から抜けるだけでなく、さらに外へ抜け出したい場合があります。 `break` のターゲットとなるラベルを `switch` に配置できます。

### <a name="enum"></a>列挙型

PowerShell 5.0 では列挙型が提供されており、switch で使用できます。

``` powershell
enum Context {
    Component
    Role
    Location
}

$item = [Context]::Role

switch ( $item )
{
    Component
    {
        'is a component'
    }
    Role
    {
        'is a role'
    }
    Location
    {
        'is a location'
    }
}
```

```Output
is a role
```

厳密に型指定された列挙型としてすべてを保持する場合は、かっこで囲みます。

``` powershell
switch ($item )
{
    ([Context]::Component)
    {
        'is a component'
    }
    ([Context]::Role)
    {
        'is a role'
    }
    ([Context]::Location)
    {
        'is a location'
    }
}
```

ここでは、switch で値 `[Context]::Location` がリテラル文字列として処理されないようにするために、かっこが必要です。

### <a name="scriptblock"></a>スクリプト ブロック

必要に応じて、スクリプト ブロックを使用して評価を実行して照合に利用できます。

``` powershell
$age = 37

switch ( $age )
{
    {$PSItem -le 18}
    {
        'child'
    }
    {$PSItem -gt 18}
    {
        'adult'
    }
}
```

```Output
'adult'
```

これにより、複雑さが増し、`switch` が読みにくくなる可能性があります。 ほとんどの場合、このようなステートメントを使用するところでは、`if` および `elseif` ステートメントを使用する方が適しています。 既に大規模な switch があり、同じ評価ブロックにヒットさせるために 2 つの項目が必要になるような場合は、これを使用することを検討します。

読みやすくするために、スクリプト ブロックをかっこ内に配置することをお勧めします。

``` powershell
switch ( $age )
{
    ({$PSItem -le 18})
    {
        'child'
    }
    ({$PSItem -gt 18})
    {
        'adult'
    }
}
```

それでも同じように実行され、さっと見るときに目に留まりやすくなります。

### <a name="regex-matches"></a>正規表現の $matches

正規表現を見直して、すぐにはわからない点について説明する必要があります。 正規表現を使用すると、`$matches` 変数が設定されます。 `$matches` の使用については、[正規表現を使用するさまざまな方法][]について説明するときに詳しく説明します。 これを名前付き一致と共に使用する簡単な例を次に示します。

``` powershell
$message = 'my ssn is 123-23-3456 and credit card: 1234-5678-1234-5678'

switch -regex ($message)
{
    '(?<SSN>\d\d\d-\d\d-\d\d\d\d)'
    {
        Write-Warning "message contains a SSN: $($matches.SSN)"
    }
    '(?<CC>\d\d\d\d-\d\d\d\d-\d\d\d\d-\d\d\d\d)'
    {
        Write-Warning "message contains a credit card number: $($matches.CC)"
    }
    '(?<Phone>\d\d\d-\d\d\d-\d\d\d\d)'
    {
        Write-Warning "message contains a phone number: $($matches.Phone)"
    }
}
```

```Output
WARNING: message may contain a SSN: 123-23-3456
WARNING: message may contain a credit card number: 1234-5678-1234-5678
```

### <a name="null"></a>$null

既定値である必要のない `$null` 値と照合できます。

``` powershell
$value = $null

switch ( $value )
{
    $null
    {
        'Value is null'
    }
    default
    {
        'value is not null'
    }
}

```Output
Value is null
```

空の文字列の場合と同様になります。

``` powershell
switch ( '' )
{
    ''
    {
        'Value is empty'
    }
    default
    {
        'value is a empty string'
    }
}

```Output
Value is empty
```

### <a name="constant-expression"></a>定数式

Lee Dailey は、定数 `$true` 式を使用して `[bool]` 項目を評価できると指摘しています。
いくつかのブール値チェックを実行する必要がある場合を考えてみましょう。

``` powershell
$isVisible = $false
$isEnabled = $true
$isSecure = $true

switch ( $true )
{
    $isEnabled
    {
        'Do-Action'
    }
    $isVisible
    {
        'Show-Animation'
    }
    $isSecure
    {
        'Enable-AdminMenu'
    }
}
```

```Output
Do-Action
Enabled-AdminMenu
```

これは、複数のブール値フィールドのステータスを評価してアクションを実行するためのクリーンな方法です。 これのおもしろい点は、まだ評価されていない値のステータスを 1 つの照合で反転させられることです。

``` powershell
$isVisible = $false
$isEnabled = $true
$isAdmin = $false

switch ( $true )
{
    $isEnabled
    {
        'Do-Action'
        $isVisible = $true
    }
    $isVisible
    {
        'Show-Animation'
    }
    $isAdmin
    {
        'Enable-AdminMenu'
    }
}
```

```Output
Do-Action
Show-Animation
```

この例では、`$isEnabled` を `$true` に設定することで、`$isVisible` も `$true` に設定されます。 その後、`$isVisible` が評価されるときには、そのスクリプト ブロックが呼び出されます。 これは、少し直観に反していますが、このしくみの巧妙な使い方です。

### <a name="switch-automatic-variable"></a>$switch 自動変数

`switch` は、その値を処理するときに、`$switch` という列挙子を作成します。 これは、PowerShell によって作成される自動変数で、ユーザーが直接操作できます。

```powershell
$a = 1, 2, 3, 4

switch($a) {
    1 { [void]$switch.MoveNext(); $switch.Current }
    3 { [void]$switch.MoveNext(); $switch.Current }
}
```

これにより、次の結果が得られます。

```Output
2
4
```

列挙子を前方に移動すると、次の項目は `switch` によって処理されませんが、その値に直接アクセスすることができます。 私には狂気に思えます。

## <a name="other-patterns"></a>その他のパターン

### <a name="hashtables"></a>ハッシュテーブル

私の投稿の中で最も人気のあるものの 1 つは、[ハッシュテーブル][]で行ったものです。 `hashtable` のユース ケースの 1 つはルックアップ テーブルです。 これは、`switch` ステートメントで対処することがよくある一般的なパターンに代わるアプローチです。

``` powershell
$day = 3

$lookup = @{
    0 = 'Sunday'
    1 = 'Monday'
    2 = 'Tuesday'
    3 = 'Wednesday'
    4 = 'Thursday'
    5 = 'Friday'
    6 = 'Saturday'
}

$lookup[$day]
```

```Output
Wednesday
```

`switch` をルックアップとしてのみ使用する場合、私は代わりに `hashtable` を使用することが多いです。

### <a name="enum"></a>列挙型

PowerShell 5.0 では `Enum` が導入されており、この場合にも使用できます。

``` powershell
$day = 3

enum DayOfTheWeek {
    Sunday
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
}

[DayOfTheWeek]$day
```

```Output
Wednesday
```

この問題の解決には他にもさまざまな方法があります。 ここでは、さまざまなオプションがあることを認識していただくことが目的でした。

## <a name="final-words"></a>まとめ

switch ステートメントは表面上は単純ですが、あまり知られていない高度な機能を持っています。 これらの機能を組み合わせることで、強力な機能になります。 これまで気付がなかったことを学んでいただければ幸いです。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2018-01-12-Powershell-switch-statement/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[switch]: /powershell/module/microsoft.powershell.core/about/about_switch
[スイッチ パラメーター]: https://www.powershellmagazine.com/2013/12/20/using-powershell-switch-vs-boolean-parameters-in-sma-runbooks/
[正規表現を使用するさまざまな方法]: https://powershellexplained.com/2017-07-31-Powershell-regex-regular-expression
[ハッシュテーブル]: everything-about-hashtable.md
