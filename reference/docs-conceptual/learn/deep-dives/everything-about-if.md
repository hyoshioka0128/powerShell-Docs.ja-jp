---
title: if ステートメントについて知りたかったことのすべて
description: 他の多くの言語と同様に、PowerShell には、スクリプト内でコードを条件付き実行するためのステートメントがあります。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: b6bafb99bfb8ecd0152bae841e5c58d4c27ccd3e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "86469754"
---
# <a name="everything-you-wanted-to-know-about-the-if-statement"></a>`if` ステートメントについて知りたかったことのすべて

他の多くの言語と同様に、PowerShell には、スクリプト内でコードを条件付き実行するためのステートメントがあります。 それらのステートメントの 1 つが [If][] ステートメントです。 本日は、PowerShell の最も基本的なコマンドの 1 つについて詳しく説明します。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログに掲載されました。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="conditional-execution"></a>条件付き実行

多くの場合、スクリプトでは決定を下し、それらの決定に基づいて異なるロジックを実行する必要があります。
これが、条件付き実行の意味です。 評価するステートメントまたは値が 1 つあると、その評価に基づいてコードの別のセクションを実行します。 これがまさに、`if` ステートメントで行われることです。

## <a name="the-if-statement"></a>`if` ステートメント

次に、`if` ステートメントの基本的な例を示します。

```powershell
$condition = $true
if ( $condition )
{
    Write-Output "The condition was true"
}
```

最初に `if` ステートメントが実行することは、かっこ内の式を評価することです。 `$true` に評価されると、中かっこ内の `scriptblock` が実行されます。 値が `$false` であった場合、そのスクリプト ブロックはスキップされます。

前の例では、`if` ステートメントは `$condition` 変数を評価するだけでした。 `$true` であったため、スクリプト ブロック内の `Write-Output` コマンドが実行されたでしょう。

一部の言語では、`if` ステートメントの後に単一行のコードを配置し、それを実行することができます。 それは PowerShell には当てはまりません。 正常に動作するには、中かっこで囲んだ完全な `scriptblock` を指定する必要があります。

## <a name="comparison-operators"></a>比較演算子

`if` ステートメントの最も一般的な用途は、2 つの項目を互いに比較することです。 PowerShell には、さまざまな比較シナリオのために特別な演算子が用意されています。 比較演算子を使用する場合、左側の値が右側の値と比較されます。

### <a name="-eq-for-equality"></a>-eq (等価性)

`-eq` は、値が互いに等しいことを確認するため、2 つの値の間で等価性のチェックを行います。

```powershell
$value = Get-MysteryValue
if ( 5 -eq $value )
{
    # do something
}
```

この例では、既知の値 `5` を取得し、それを `$value` と比較して、それらが一致しているかどうかを確認します。

考えられるユース ケースの 1 つは、値に対してアクションを実行する前に、値の状態をチェックすることです。 サービスを取得し、その状態が実行中であることを確認してから、それに対して `Restart-Service` を呼び出すことができます。

C# のような他の言語では、等価性のチェックに `==` を使用する (例: `5 == $value`) のが一般的ですが、これは PowerShell では機能しません。 別のよくある誤りは、変数に値を代入するために予約されている等号 (例: `5 = $value`) を使用することです。 既知の値を左側に配置することで、その誤りを発生しにくくします。

この演算子 (およびその他) には、いくつかのバリエーションがあります。

- `-eq` (大文字と小文字が区別されない等価性)
- `-ieq` (大文字と小文字が区別されない等価性)
- `-ceq` (大文字と小文字が区別される等価性)

### <a name="-ne-not-equal"></a>-ne (等しくない)

多くの演算子には、逆の結果をチェックする、関連する演算子があります。 `-ne` は、値が互いに等しくないことを確認します。

```powershell
if ( 5 -ne $value )
{
    # do something
}
```

値が `5` でない場合にのみアクションが実行されるようにするには、これを使用します。 適切なユース ケースとしては、サービスが実行中の状態かどうかを確認してからその起動を試みる場合が挙げられます。

**バリエーション:**

- `-ne` (大文字と小文字が区別されない、等しくない)
- `-ine` (大文字と小文字が区別されない、等しくない)
- `-cne` (大文字と小文字が区別される、等しくない)

これらは、`-eq` の逆のバリエーションです。 私は、他の演算子のバリエーションをリストにするときには、これらの型を一緒にグループ化します。

### <a name="-gt--ge--lt--le-for-greater-than-or-less-than"></a>-gt -ge -lt -le (より大きい、より小さい)

これらの演算子は、ある値が別の値より大きいか小さいかを調べて確認するときに使用されます。
`-gt -ge -lt -le` は、GreaterThan、GreaterThanOrEqual、LessThan、および LessThanOrEqual を表しています。

```powershell
if ( $value -gt 5 )
{
    # do something
}
```

**バリエーション:**

- `-gt` (より大きい)
- `-igt` (より大きい、大文字と小文字が区別されない)
- `-cgt` (より大きい、大文字と小文字が区別される)
- `-ge` (以上)
- `-ige` (以上、大文字と小文字が区別されない)
- `-cge` (以上、大文字と小文字が区別される)
- `-lt` (より小さい)
- `-ilt` (より小さい、大文字と小文字が区別されない)
- `-clt` (より小さい、大文字と小文字が区別される)
- `-le` (以下)
- `-ile` (以下、大文字と小文字が区別されない)
- `-cle` (以下、大文字と小文字が区別される)

私には、これらの演算子で、大文字と小文字を区別するオプション、区別しないオプションを使用する理由がわかりません。

### <a name="-like-wildcard-matches"></a>-like (ワイルドカードの一致)

PowerShell には、ワイルドカード ベースの独自のパターン一致構文があり、`-like` 演算子と共に使用できます。 これらのワイルドカード パターンは、かなり基本的なものです。

- `?` は任意の 1 文字と一致します。
- `*` は任意の数の文字と一致します。

```powershell
$value = 'S-ATX-SQL01'
if ( $value -like 'S-*-SQL??')
{
    # do something
}
```

指摘しておくことが重要なのは、パターンは文字列全体に一致するという点です。 文字列の途中にある何かと一致させる必要がある場合は、文字列の両端に `*` を付ける必要があります。

```powershell
$value = 'S-ATX-SQL02'
if ( $value -like '*SQL*')
{
    # do something
}
```

**バリエーション:**

- `-like` (大文字と小文字が区別されないワイルドカード)
- `-ilike` (大文字と小文字が区別されないワイルドカード)
- `-clike` (大文字と小文字が区別されるワイルドカード)
- `-notlike` (大文字と小文字が区別されないワイルドカード不一致)
- `-inotlike` (大文字と小文字が区別されないワイルドカード不一致)
- `-cnotlike` (大文字と小文字が区別されるワイルドカード不一致)

### <a name="-match-regular-expression"></a>-match (正規表現)

`-match` 演算子を使用すると、正規表現に基づく一致があるか、文字列を調べられます。 これは、ワイルドカード パターンでは十分な柔軟性が得られない場合に使用します。

```powershell
$value = 'S-ATX-SQL01'
if ( $value -match 'S-\w\w\w-SQL\d\d')
{
    # do something
}
```

正規表現パターンは、既定では文字列内の任意の場所に一致します。 そのため次のように、一致させる部分文字列を指定できます。

```powershell
$value = 'S-ATX-SQL01'
if ( $value -match 'SQL')
{
    # do something
}
```

正規表現は独自の複雑な言語であり、詳しく調べる価値があります。 私は別の記事で、`-match` の詳細と[正規表現を使用するさまざまな方法][]について説明しています。

**バリエーション:**

- `-match` (大文字と小文字が区別されない正規表現)
- `-imatch` (大文字と小文字が区別されない正規表現)
- `-cmatch` (大文字と小文字が区別される正規表現)
- `-notmatch` (大文字と小文字が区別されない正規表現不一致)
- `-inotmatch` (大文字と小文字が区別されない正規表現不一致)
- `-cnotmatch` (大文字と小文字が区別される正規表現不一致)

### <a name="-is-of-type"></a>-is (型)

`-is` 演算子を使用して値の型を調べられます。

```powershell
if ( $value -is [string] )
{
    # do something
}
```

これは、複数のクラスを使用している場合や、パイプライン上でさまざまなオブジェクトを受け入れている場合に使用できます。 入力として、サービスまたはサービス名のいずれかを使用できます。 次に、サービスがあるかどうかを調べて、名前しかない場合はサービスをフェッチします。

```powershell
if ( $Service -isnot [System.ServiceProcess.ServiceController] )
{
    $Service = Get-Service -Name $Service
}
```

**バリエーション:**

- `-is` (型)
- `-isnot` (型でない)

## <a name="collection-operators"></a>コレクション演算子

1 つの値を指定して前出の演算子を使用すると、結果は `$true` または `$false` になります。 これは、コレクションの操作時には少し異なる方法で処理されます。 コレクション内の各項目が評価されて、演算子からは、`$true` に評価されるすべての値が返されます。

```powershell
PS> 1,2,3,4 -eq 3
3
```

これは、`if` ステートメントでも正しく機能します。 そのため、演算子によって値が返されると、ステートメント全体は `$true` になります。

```powershell
$array = 1..6
if ( $array -gt 3 )
{
    # do something
}
```

これについては、指摘する必要がある小さな罠が細部に潜んでいます。このように `-ne` 演算子を使用すると、誤ってロジックを逆に考えてしまいやすくなります。 コレクションで `-ne` を使用すると、コレクション内のいずれかの項目が値と一致しない場合に、`$true` が返されます。

```powershell
PS> 1,2,3 -ne 4
1
2
3
```

これは巧みな技法のようですが、これをより効率的に処理する演算子 `-contains` および `-in` が用意されています。 そして、期待したことは `-notcontains` によって行われます。

### <a name="-contains"></a>-contains

`-contains` 演算子は、値があるか、コレクションを調べます。 一致が見つかるとすぐに `$true` が返されます。

```powershell
$array = 1..6
if ( $array -contains 3 )
{
    # do something
}
```

これは、コレクションに値が含まれているかどうかを確認する場合の推奨される方法です。 `Where-Object` (または `-eq`) を使用すると、毎回リスト全体が処理され、大幅に低速になります。

**バリエーション:**

- `-contains` (大文字と小文字が区別されない一致)
- `-icontains` (大文字と小文字が区別されない一致)
- `-ccontains` (大文字と小文字が区別される一致)
- `-notcontains` (大文字と小文字が区別されない不一致)
- `-inotcontains` (大文字と小文字が区別されない不一致)
- `-cnotcontains` (大文字と小文字が区別される不一致)

### <a name="-in"></a>-in

`-in` 演算子は、右側にコレクションがあること以外は、`-contains` 演算子とまったく同じです。

```powershell
$array = 1..6
if ( 3 -in $array )
{
    # do something
}
```

**バリエーション:**

- `-in` (大文字と小文字が区別されない一致)
- `-iin` (大文字と小文字が区別されない一致)
- `-cin` (大文字と小文字が区別される一致)
- `-notin` (大文字と小文字が区別されない不一致)
- `-inotin` (大文字と小文字が区別されない不一致)
- `-cnotin` (大文字と小文字が区別される不一致)

## <a name="logical-operators"></a>論理演算子

論理演算子は、他の式を反転または結合するために使用されます。

### <a name="-not"></a>-not

`-not` 演算子は、式を `$false` から `$true` に、または `$true` から `$false` に反転させます。 次に、`Test-Path` が `$false` であるときにアクションを実行する例を示します。

```powershell
if ( -not ( Test-Path -Path $path ) )
```

説明してきたほとんどの演算子には、`-not` 演算子を使用する必要がないバリエーションがあります。 しかし、それでも役に立つ場面があります。

### <a name="-operator"></a>! operator

`-not` の別名として `!` を使用できます。

```powershell
if ( -not $value ){}
if ( !$value ){}
```

C# などの別の言語が専門のユーザーは、`!` をより多用することに気付くかも知れません。 私は、スクリプトをすばやく調べるときに見にくいと感じるため、これを文字で入力することを好んでいます。

### <a name="-and"></a>-and

式を `-and` 演算子と組み合わせることができます。 それを行うときには、式全体が `$true` になるには、両方の側が `$true` になる必要があります。

```powershell
if ( ($age -gt 13) -and ($age -lt 55) )
```

この例では、`$age` は左側で 13 以上、右側で 55 未満である必要があります。 その例で、より明確にするために私がかっこを余分に追加しましたが、式が単純である限り、これらのかっこは省略可能です。 それらがない同じ例を示します。

```powershell
if ( $age -gt 13 -and $age -lt 55 )
```

評価は左から右に実行されます。 最初の項目が `$false` に評価された場合は早期に終了し、右側の比較は実行されません。 これは、使用する前に値が存在することを確認する必要がある場合に便利です。 たとえば、`$null` パスを指定した場合、`Test-Path` からエラーがスローされます。

```powershell
if ( $null -ne $path -and (Test-Path -Path $path) )
```

### <a name="-or"></a>-or

`-or` を使用すると、2 つの式を指定し、その 1 つが `$true` である場合に `$true` を返すことができます。

```powershell
if ( $age -le 13 -or $age -ge 55 )
```

`-and` 演算子と同様に、評価は左から右に実行されます。 最初の部分が `$true` である場合は、ステートメント全体が `$true` になるので、式の残りの部分は処理されません。

これらの演算子の構文がどのように機能するかもメモしておいてください。 2 つの別個の式が必要です。 私は、自分のミスに気付かずに、このようなこと (`$value -eq 5 -or 6`) を実行しようとしたユーザーを見てきました。

### <a name="-xor-exclusive-or"></a>-xor (排他的 OR)

これは少し変わっています。 `-xor` では、1 つの式だけが `$true` に評価されることができます。 そのため、両方の項目が `$false` か、両方の項目が `$true` である場合に、式全体が `$false` になります。 式の結果が異なるときにだけ、式が `$true` になる、というのがこれの別の見方です。

この論理演算子を使用することはめったになく、私はこれを使用する理由について、適切な例を思い付くことができません。

## <a name="bitwise-operators"></a>ビットごとの演算子

ビットごとの演算子は、値内のビットに対して演算を実行し、結果として新しい値を生成します。 [ビットごとの演算子][]について教えることは、この記事の範囲外ですが、次にそれらのリストを示します。

- `-band` (2 項 AND)
- `-bor` (2 項 OR)
- `-bxor` (2 項排他的 OR)
- `-bnot` (2 項 NOT)
- `-shl` (左シフト)
- `-shr` (右シフト)

## <a name="powershell-expressions"></a>PowerShell の式

条件ステートメント内で通常の PowerShell を使用できます。

```powershell
if ( Test-Path -Path $Path )
```

`Test-Path` は、実行されると `$true` または `$false` を返します。 これは、他の値を返すコマンドにも適用されます。

```powershell
if ( Get-Process Notepad* )
```

返されたプロセスがある場合は `$true` に、何もない場合は `$false` に評価されます。 次のようにパイプライン式やその他の PowerShell ステートメントを使用することは、完全に有効です。

```powershell
if ( Get-Process | Where Name -eq Notepad )
```

これらの式は、`-and` 演算子と `-or` 演算子を使用して相互に結合できますが、かっこを使用して部分式に分割することが必要な場合があります。

```powershell
if ( (Get-Process) -and (Get-Service) )
```

### <a name="checking-for-null"></a>$null の確認

結果がなかったり、`if` ステートメントで `$null` 値が `$false` と評価されます。 `$null` を特に調べる場合は、左側に `$null` を配置することがベスト プラクティスです。

```powershell
if ( $null -eq $value )
```

PowerShell で `$null` 値を扱う際には、かなり多くの微妙な差異があります。 詳細についてお知りになりたい場合は、[$null について知りたかったことのすべて][]に関する記事があります。

### <a name="variable-assignment"></a>変数の代入

[Prasoon Karunan V][] に思い出させてもらうまで、この部分を追加するのをほとんど忘れていました。

```powershell
if ($process=Get-Process notepad -ErrorAction ignore) {$process} else {$false}
```

通常、変数に値を代入するときに、値はパイプラインやコンソールに渡されません。 部分式で変数の代入を行うと、確かにパイプラインに渡されます。

```powershell
PS> $first = 1
PS> ($second = 2)
2
```

`$first` の代入には出力がなく、`$second` の代入には出力がある様子を確認してください。 `if` ステートメントで代入が行われると、上の `$second` の代入と同様に実行されます。 次に、使用方法のはっきりした例を示します。

```powershell
if ( $process = Get-Process Notepad* )
{
    $process | Stop-Process
}
```

`$process` が代入された値を取得すると、ステートメントは `$true` になり、`$process` は停止されます。

これは等価性チェックではないため、`-eq` と混同しないでください。 これは、ほとんどの人がこのように動作することを認識していない、わかりにくい機能です。

## <a name="alternate-execution-path"></a>代替実行パス

`if` ステートメントを使用すると、ステートメントが `$true` のときだけでなく、`$false` のときのためにもアクションを指定できます。 ここで `else` ステートメントが登場します。

### <a name="else"></a>else

`else` ステートメントは、使用されるときには常に `if` ステートメントの最後の部分です。

```powershell
if ( Test-Path -Path $Path -PathType Leaf )
{
    Move-Item -Path $Path -Destination $archivePath
}
else
{
    Write-Warning "$path doesn't exist or isn't a file."
}
```

この例では、`$path` を調べて、それがファイルであることを確認します。 ファイルが見つかった場合は、それを移動します。 そうでない場合は、警告を書き出します。 この種類の分岐ロジックは非常に一般的です。

### <a name="nested-if"></a>入れ子の if

`if` ステートメントと `else` ステートメントではスクリプト ブロックを取るため、その他の `if` ステートメントを含め、任意の PowerShell コマンドをブロック内に配置できます。 これにより、はるかに複雑なロジックを使用できるようになります。

```powershell
if ( Test-Path -Path $Path -PathType Leaf )
{
    Move-Item -Path $Path -Destination $archivePath
}
else
{
    if ( Test-Path -Path $Path )
    {
        Write-Warning "A file was required but a directory was found instead."
    }
    else
    {
        Write-Warning "$path could not be found."
    }
}
```

この例では、まずハッピー パスをテストしてから、それに対してアクションを実行します。 失敗した場合は別のチェックを行い、より詳細な情報を、ユーザーに提供します。

### <a name="elseif"></a>elseif

単一の条件付きチェックのみに限定されているわけではありません。 `elseif` ステートメントを使用してステートメントを入れ子にするのではなく、`if` ステートメントと `else` ステートメントをまとめて連結できます。

```powershell
if ( Test-Path -Path $Path -PathType Leaf )
{
    Move-Item -Path $Path -Destination $archivePath
}
elseif ( Test-Path -Path $Path )
{
    Write-Warning "A file was required but a directory was found instead."
}
else
{
    Write-Warning "$path could not be found."
}
```

実行は、最上部から最下部へと行われます。 最上位の `if` ステートメントが最初に評価されます。 それが `$false` の場合は、リスト内で次にある `elseif` または `else` まで移動します。 最後の `else` は、他のどれからも `$true` が返されない場合に実行する既定のアクションです。

### <a name="switch"></a>スイッチ

この時点で、`switch` ステートメントに触れる必要があります。 1 つの値で複数の比較を行うための代替構文を提供します。 `switch` を使用して、式を指定し、その結果を異なるいくつかの値と比較します。 それらの値のいずれかが一致すると、一致するコード ブロックが実行されます。 次の例を見てください。

```powershell
$itemType = 'Role'
switch ( $itemType )
{
    'Component'
    {
        'is a component'
    }
    'Role'
    {
        'is a role'
    }
    'Location'
    {
        'is a location'
    }
}
```

`$itemType` と一致する可能性がある候補の値が 3 つあります。 この場合は `Role` と一致します。 `switch` 演算子に触れる機会となるように、私は単純な例を使用しました。 別の記事で、[switch ステートメントについて知りたかったことのすべて][]について詳しく説明しています。

## <a name="pipeline"></a>パイプライン

パイプラインは、PowerShell の独特で重要な機能です。 抑制されていない、または変数に代入されていない任意の値を、パイプライン内に配置します。 `if` では、常に明白であるとは限らない方法でパイプラインを利用する方法が提供されます。

```powershell
$discount = if ( $age -ge 55 )
{
    Get-SeniorDiscount
}
elseif ( $age -le 13 )
{
    Get-ChildDiscount
}
else
{
    0.00
}
```

各スクリプト ブロックでは、コマンドの結果または値をパイプラインに配置しています。 次に、`if` ステートメントの結果を `$discount` 変数に代入します。 その例では、それらの値を直接、各スクリプト ブロック内の `$discount` 変数に簡単に割り当てることができました。 私は、`if` ステートメントでこれを頻繁に使用しているとは言えませんが、確かにこれを最近使用した例があります。

### <a name="array-inline"></a>インライン配列

いくつかのコマンドライン引数を指定して実行可能ファイルを起動する、[Invoke-SnowSql][] という名前の関数があります。 ここに、引数の配列を構築する関数からのクリップを示します。

```powershell
$snowSqlParam = @(
    '--accountname', $Endpoint
    '--username', $Credential.UserName
    '--option', 'exit_on_error=true'
    '--option', 'output_format=csv'
    '--option', 'friendly=false'
    '--option', 'timing=false'
    if ($Debug)
    {
        '--option', 'log_level=DEBUG'
    }
    if ($Path)
    {
        '--filename', $Path
    }
    else
    {
        '--query', $singleLineQuery
    }
)
```

`$Debug` 変数と `$Path` 変数は、エンド ユーザーによって指定される、関数に対するパラメーターです。
これらは、配列の初期化中にインラインで評価します。 `$Debug` が true の場合、それらの値は正しい場所の `$snowSqlParam` に格納されます。 `$Path` 変数についても、同じことが当てはまります。

## <a name="simplify-complex-operations"></a>複雑な操作を簡略化する

確認する比較が多すぎて `If` ステートメントが画面右側の遠くの方へスクロールされてしまうという状況を避けることはできません。

```powershell
$user = Get-ADUser -Identity $UserName
if ( $null -ne $user -and $user.Department -eq 'Finance' -and $user.Title -match 'Senior' -and $user.HomeDrive -notlike '\\server\*' )
{
    # Do Something
}
```

読むのが難しい場合があり、それが理由でよりミスをしやすくなります。 これについては、できることがいくつかあります。

### <a name="line-continuation"></a>行の連結

PowerShell には、コマンドを次の行にラップするための演算子がいくつかあります。 論理演算子 `-and` および `-or` は、式を複数行に分割する場合に使うのに適した演算子です。

```powershell
if ($null -ne $user -and
    $user.Department -eq 'Finance' -and
    $user.Title -match 'Senior' -and
    $user.HomeDrive -notlike '\\server\*'
)
{
    # Do Something
}
```

それでも続行する作業は多数ありますが、各部分をそれぞれの行に配置することで大きな違いが生じます。
私が通常これを使用するのは、2 つ以上の比較を取得する場合や、ロジックを読むために右にスクロールする必要がある場合です。

### <a name="pre-calculating-results"></a>結果の事前計算

`if` ステートメントから目的のステートメントを取り出して、結果だけを調べることができます。

```powershell
$needsSecureHomeDrive = $null -ne $user -and
    $user.Department -eq 'Finance' -and
    $user.Title -match 'Senior' -and
    $user.HomeDrive -notlike '\\server\*'

if ( $needsSecureHomeDrive )
{
    # Do Something
}
```

これはただ、前の例よりずっとすっきりしているように感じます。 また、実際に調べようとしていることが何であるかを説明する変数名を使用できます。 これは、不要なコメントを節約する自己文書化コードの例でもあります。

### <a name="multiple-if-statements"></a>複数の if ステートメント

これを複数のステートメントに分割し、一度に 1 つずつ調べることができます。 この場合、結果を結合するためにフラグまたは追跡用の変数を使用します。

```powershell

$skipUser = $false

if( $null -eq $user )
{
    $skipUser = $true
}

if( $user.Department -ne 'Finance' )
{
    Write-Verbose "isn't in Finance department"
    $skipUser = $true
}

if( $user.Title -match 'Senior' )
{
    Write-Verbose "Doesn't have Senior title"
    $skipUser = $true
}

if( $user.HomeDrive -like '\\server\*' )
{
    Write-Verbose "Home drive already configured"
    $skipUser = $true
}

if ( -not $skipUser )
{
    # do something
}
```

実際には、フラグ ロジックが正しく機能するように、ロジックを反転する必要がありました。 各評価は、個々の `if` ステートメントです。 この利点は、デバッグ中に、ロジックで何を実行中であるかを正確に知ることができることです。 同時に、ずっと役立つ詳細を追加できました。

明らかなマイナス面は、記述するコードがずっと多いことです。 コードは、1 行のロジックを取得し、それを 25 行以上に展開するため、より複雑に見えます。

### <a name="using-functions"></a>関数の使用

そうした検証ロジックすべてを、1 つの関数に移すこともできます。 一目で、これがどれほどすっきりしているかがわかります。

```powershell
if ( Test-SecureDriveConfiguration -ADUser $user )
{
    # do something
}
```

まだ、検証を実行する関数を作成する必要はありますが、このコードはずっと扱いやすくなっています。 このコードをテストするのも容易になっています。 テストでは、`Test-ADDriveConfiguration` への呼び出しを模倣できるので、この関数に必要なテストは 2 つだけです。 1 つはそこで `$true` を返し、もう 1 つはそこで `$false` を返します。 その他の関数のテストは、関数が非常に小さいためより簡単です。

その関数の本体は、開始時に使った 1 行バージョンである場合も、最後のセクションで使用した、展開されたロジックと同じである場合もあります。 これは、どちらのシナリオでも正しく機能し、後でその実装を簡単に変更できます。

## <a name="error-handling"></a>エラー処理

`if` ステートメントの重要な用途の 1 つは、エラーが発生する前にエラー条件を調べることです。 フォルダーが既に存在するかどうかを確認してからフォルダーを作成することが、良い例です。

```powershell
if ( -not (Test-Path -Path $folder) )
{
    New-Item -Type Directory -Path $folder
}
```

例外の発生を予期している場合、それは実際には例外ではないとも言えます。 そのため、値を確認し、可能な場合は条件を検証します。

実際の例外処理についてもう少し詳しく知りたい場合は、[例外について知りたかったことのすべて][]に関するページを参照してください。

## <a name="final-words"></a>まとめ

`if` ステートメントはあのように単純なステートメントですが、PowerShell の基本となっている部分です。 これは、自分が記述するほとんどすべてのスクリプトで、複数回使用しているのに気付くことになります。 以前よりも理解を深めていただけたならば幸いです。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2019-08-11-Powershell-if-then-else-equals-operator/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[if]: /powershell/module/microsoft.powershell.core/about/about_if
[ビットごとの演算子]: /powershell/module/microsoft.powershell.core/about/about_arithmetic_operators#bitwise-operators
[正規表現を使用するさまざまな方法]: https://powershellexplained.com/2017-07-31-Powershell-regex-regular-expression/
[例外について知りたかったことのすべて]: everything-about-exceptions.md
[$null について知りたかったことのすべて]: everything-about-null.md
[Prasoon Karunan V]: https://twitter.com/prasoonkarunan
[switch ステートメントについて知りたかったことのすべて]: everything-about-switch.md
[Invoke-SnowSql]: https://github.com/loanDepot/SnowSQL/blob/a3731b52e4ab4ecb503fb81e2d8cb131e8f90410/SnowSQL/public/Invoke-SnowSql.ps1#L90
