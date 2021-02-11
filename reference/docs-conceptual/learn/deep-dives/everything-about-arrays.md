---
title: 配列について知りたかったことのすべて
description: 配列は、ほとんどのプログラミング言語の基本的な言語機能の 1 つです。
ms.date: 10/08/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: b26aa11aadbeea1984b2754cfcad061c7fa3ff1e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "91852563"
---
# <a name="everything-you-wanted-to-know-about-arrays"></a>配列について知りたかったことのすべて

[配列][] は、ほとんどのプログラミング言語の基本的な言語機能の 1 つです。 それらは、避けることが困難な値またはオブジェクトのコレクションです。 配列と配列が提供するすべての機能について詳しく見ていきましょう。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="what-is-an-array"></a>配列とは

まず、配列の概要と、ほとんどのプログラミング言語でのその使用方法に関する基本的な技術説明を行ってから、PowerShell でのもう 1 つの使用方法について説明します。

配列とは、複数の項目のコレクションとして機能するデータ構造です。 配列を反復処理したり、インデックスを使用して個々の項目にアクセスしたりすることができます。 配列は、各値が他の値のすぐ隣に格納される連続したメモリ チャンクとして作成されます。

それぞれの詳細については、説明を進めていく中で取り上げます。

## <a name="basic-usage"></a>基本的な使用方法

配列はこのような PowerShell の基本機能であるため、PowerShell で使用するための簡単な構文があります。

### <a name="create-an-array"></a>配列を作成する

空の配列を作成するには、`@()` を使用します

```powershell
PS> $data = @()
PS> $data.count
0
```

配列を作成して、値をシードするには、`@()` のかっこ内に値を配置するだけです。

```powershell
PS> $data = @('Zero','One','Two','Three')
PS> $data.count
4

PS> $data
Zero
One
Two
Three
```

この配列には項目が 4 つあります。 `$data` 変数を呼び出すと、それらの項目の一覧が表示されます。 文字列の配列の場合は、文字列ごとに 1 行返されます。

配列を複数の行で宣言することもできます。 この場合、コンマは省略可能であり、通常は省略されます。

```powershell
$data = @(
    'Zero'
    'One'
    'Two'
    'Three'
)
```

そのように複数の行で配列を宣言することをお勧めします。 複数の項目がある場合に読みやすくなるだけでなく、ソース コード管理を使用する場合に以前のバージョンと比較しやすくなります。

#### <a name="other-syntax"></a>その他の構文

`@()` が配列を作成するための構文であることは普通にわかりますが、ほとんどの場合はコンマ区切りリストが使用されます。

```powershell
$data = 'Zero','One','Two','Three'
```

#### <a name="write-output-to-create-arrays"></a>Write-Output で配列を作成する

注目すべき優れた小技の 1 つは、`Write-Output` を使用してコンソールですばやく文字列を作成できることです。

```powershell
$data = Write-Output Zero One Two Three
```

これが便利なのは、パラメーターで文字列を受け入れるときに文字列を引用符で囲む必要がないからです。 スクリプトでこれを行うことはありませんが、コンソールでは格好の技法です。

### <a name="accessing-items"></a>項目へのアクセス

項目が含まれている配列の用意ができたので、それらの項目にアクセスして更新してみてください。

#### <a name="offset"></a>Offset

個々の項目にアクセスするには、0 から始まるオフセット値と角かっこ `[]` を使用します。 配列内の最初の項目を取得する方法を次に示します。

```powershell
PS> $data = 'Zero','One','Two','Three'
PS> $data[0]
Zero
```

ここで 0 を使用する理由は、最初の項目がリストの先頭にあるため、オフセット 0 の項目を使用してそれを取得するためです。 2 番目の項目を取得するには、オフセット 1 を使用して最初の項目をスキップする必要があります。

```powershell
PS> $data[1]
One
```

つまり、最後の項目はオフセット 3 にあります。

```powershell
PS> $data[3]
Three
```

#### <a name="index"></a>インデックス

この例に向いている値を選択した理由がおわかりいただけるでしょう。 これをオフセットとして紹介したのはそれが実際の値であるためですが、通常こうしたオフセットはインデックスと呼ばれます。 インデックスは `0` から始まります。 この記事の残りの部分では、このオフセットをインデックスと呼びます。

#### <a name="special-index-tricks"></a>特殊なインデックスの技法

ほとんどの言語では、インデックスとして指定できるのは 1 つの数値のみで、返される項目も 1 つです。
それに比べて PowerShell にはかなり高い柔軟性があります。 一度に複数のインデックスを使用できます。 インデックスの一覧を指定することで、いくつかの項目を選択できます。

```powershell
PS> $data[0,2,3]
Zero
Two
Three
```

それらの項目は、指定されたインデックスの順序に基づいて返されます。 インデックスを重複させると、どちらのときもその項目が返されます。

```powershell
PS> $data[3,0,3]
Three
Zero
Three
```

組み込みの `..` 演算子を使用すると、一連の数値を指定できます。

```powershell
PS> $data[1..3]
One
Two
Three
```

これは逆順でも機能します。

```powershell
PS> $data[3..1]
Three
Two
One
```

負のインデックス値を使用すると、末尾からオフセットされます。 したがって、リストの最後の項目が必要な場合は、`-1` を使用できます。

```powershell
PS> $data[-1]
Three
```

ここで、`..` 演算子を使用する場合の注意事項があります。 `0..-1` と `-1..0` の各シーケンス は、それぞれ `0,-1` と `-1,0` の値に評価されます。 この詳細を忘れた場合は、`$data[0..-1]` を見て、それがすべての項目を列挙するものだと考えると簡単です。 `$data[0..-1]` では、配列内の最初と最後の項目を返すことによって `$data[0,-1]` と同じ値が返されます (他の値は返されません)。

#### <a name="out-of-bounds"></a>範囲外

ほとんどの言語では、配列の末尾を越える項目のインデックスにアクセスしようとすると、ある種のエラーまたは例外が発生します。 PowerShell では何も返されず、通知もありません。

```powershell
PS> $null -eq $data[9000]
True
```

#### <a name="cannot-index-into-a-null-array"></a>null 配列にインデックスを作成できない

変数が `$null` であるときに、配列のようにそれにインデックスを作成しようとすると、`System.Management.Automation.RuntimeException` 例外が発生してメッセージ `Cannot index into a null array` が表示されます。

```powershell
PS> $empty = $null
SP> $empty[0]
Error: Cannot index into a null array.
```

そのため、配列内の要素にアクセスしようとする前に、配列が `$null` でないことを確認してください。

#### <a name="count"></a>Count

配列とその他のコレクションには、配列内の項目の数を通知する count プロパティがあります。

```powershell
PS> $data.count
4
```

PowerShell 3.0 では、ほとんどのオブジェクトに count プロパティが追加されました。 単一のオブジェクトを配置すると、カウント `1` が返されるはずです。

```powershell
PS> $date = Get-Date
PS> $date.count
1
```

`0` が返されることを除き、`$null` にも count プロパティがあります。

```powershell
PS> $null.count
0
```

ここにはいくつかのトラップがあります。これについては、この記事の後半で `$null` または空の配列がないかの確認方法について説明するときに再度取り上げます。

#### <a name="off-by-one-errors"></a>off-by-one エラー

配列はインデックス 0 から始まるため、一般的なプログラミング エラーが発生します。 off-by-one エラーが発生する可能性のある状況は 2 つあります。

1 つ目は、2 番目の項目が必要であると心の中で思っているときに、インデックス `2` を使用して、実際には 3 番目の項目を取得することによるものです。 または、項目が 4 つあり、最後の項目が必要なので、カウントを使用して最後の項目にアクセスしようと考えることによるものです。

```powershell
$data[ $data.count ]
```

PowerShell では幸いにも、この操作が可能であり、インデックス 4 に存在する項目 (`$null`) が正確に返されます。 `$data.count - 1`、または上記で説明した `-1` を使用する必要があります。

```powershell
PS> $data[ $data.count - 1 ]
Three
```

ここでは、`-1` インデックスを使用して最後の要素を取得できます。

```powershell
PS> $data[ -1 ]
Three
```

Lee Dailey 氏は、`$data.GetUpperBound(0)` を使用して最大インデックス番号を取得できることも指摘しました。

```powershell
PS> $data.GetUpperBound(0)
3
PS> $data[ $data.GetUpperBound(0) ]
Three
```

2 つ目の最も一般的な状況は、リストを反復処理していて、適切なタイミングで停止しない場合です。 これについては、`for` ループの使用方法について説明するときに再度取り上げます。

### <a name="updating-items"></a>項目の更新

同じインデックスを使用して、配列内の既存の項目を更新できます。 これにより、直接アクセスによって個々の項目を更新できるようになります。

```powershell
$data[2] = 'dos'
$data[3] = 'tres'
```

最後の要素を越える項目を更新しようとすると、`Index was outside the bounds of the array.` エラーが発生します。

```powershell
PS> $data[4] = 'four'
Index was outside the bounds of the array.
At line:1 char:1
+ $data[4] = 'four'
+ ~~~~~~~~~~~~~
+ CategoryInfo          : OperationStopped: (:) [], IndexOutOfRangeException
+ FullyQualifiedErrorId : System.IndexOutOfRangeException
```

これについては、後で配列のサイズを大きくする方法について説明するときに再度取り上げます。

### <a name="iteration"></a>反復

ある時点で、リスト全体を調べたり反復処理したりして、配列内の項目ごとに何らかのアクションを実行することが必要な場合があります。

#### <a name="pipeline"></a>パイプライン

配列と PowerShell パイプラインは最高の組み合わせです。 これは、それらの値を処理する最も簡単な方法の 1 つです。 配列をパイプラインに渡すと、配列内の各項目が個別に処理されます。

```powershell
PS> $data = 'Zero','One','Two','Three'
PS> $data | ForEach-Object {"Item: [$PSItem]"}
Item: [Zero]
Item: [One]
Item: [Two]
Item: [Three]
```

`$PSItem` を今までに見たことがない場合は、それが `$_` と同じものであることがわかります。 両方ともパイプライン内の現在のオブジェクトを表しているため、どちらを使用してもかまいません。

#### <a name="foreach-loop"></a>ForEach ループ

`ForEach` ループは、コレクションで適切に機能します。 使用する構文は `foreach ( <variable> in <collection> )` です。

```powershell
foreach ( $node in $data )
{
    "Item: [$node]"
}
```

#### <a name="foreach-method"></a>ForEach メソッド

これについては忘れがちですが、単純な操作に適しています。 PowerShell では、コレクションに対して `.ForEach()` を呼び出すことができます。

```powershell
PS> $data.foreach({"Item [$PSItem]"})
Item [Zero]
Item [One]
Item [Two]
Item [Three]
```

`.foreach()` は、スクリプト ブロックであるパラメーターを受け取ります。 かっこを削除して、スクリプト ブロックのみを指定することもできます。

```powershell
$data.foreach{"Item [$PSItem]"}
```

これは、あまり知られていない構文ですが、まったく同じように機能します。 この `foreach` メソッドは、PowerShell 4.0 で追加されました。

#### <a name="for-loop"></a>For ループ

`for` ループは、他のほとんどの言語では頻繁に使用されますが、PowerShell ではあまり見かけません。 これを見かけるとしたら、多くの場合、配列を調べるコンテキストにおいてです。

```powershell
for ( $index = 0; $index -lt $data.count; $index++)
{
    "Item: [{0}]" -f $data[$index]
}
```

最初に、`$index` を `0` に初期化します。 次に、`$index` が `$data.count` より小さくなければならないという条件を追加します。 最後に、ループするたびにインデックスを `1` ずつ増やす必要があることを指定します。 このケースでは、`$index++` は `$index = $index + 1` の短縮形です。

`for` ループを使用する場合は常に、条件に特別な注意を払ってください。 ここでは `$index -lt $data.count` を使用しました。 条件を少し取り違えて、ロジック内に off-by-one エラーを発生させるのはたやすいことです。 `$index -le $data.count` または `$index -lt ($data.count - 1)` の使用には、若干の誤解があります。 結果として、処理される項目の数が多すぎたり少なすぎたりすることがあります。 これは、よくある off-by-one エラーです。

#### <a name="switch-loop"></a>switch ループ

これは見落としやすい技法です。 配列を [switch ステートメント][]に指定すると、配列内の各項目がチェックされます。

```powershell
$data = 'Zero','One','Two','Three'
switch( $data )
{
    'One'
    {
        'Tock'
    }
    'Three'
    {
        'Tock'
    }
    Default
    {
        'Tick'
    }
}
```

```Output
Tick
Tock
Tick
Tock
```

switch ステートメントを使用して実行できるすばらしい処理がたくさんあります。 これに関する専用の記事をご覧ください。

- [switch ステートメントについて知りたかったことのすべて][switch ステートメント]

#### <a name="updating-values"></a>値の更新

配列が文字列または整数 (値型) のコレクションである場合、ループするときに配列内の値の更新が必要になることがあります。 上記のほとんどのループでは、値のコピーを保持する変数をループ内で使用します。 その変数を更新しても、配列内の元の値は更新されません。

そのステートメントの例外は、`for` ループです。 配列を調べてその内部の値を更新する場合、お探しのものは `for` ループです。

```powershell
for ( $index = 0; $index -lt $data.count; $index++ )
{
    $data[$index] = "Item: [{0}]" -f $data[$index]
}
```

この例では、インデックスによって値を受け取り、いくつかの変更を行ってから、同じインデックスを使用して値を割り当て直します。

## <a name="arrays-of-objects"></a>オブジェクトの配列

これまでは、値型だけを配列内に配置してきましたが、配列にはオブジェクトを含めることもできます。

```powershell
$data = @(
    [pscustomobject]@{FirstName='Kevin';LastName='Marquette'}
    [pscustomobject]@{FirstName='John'; LastName='Doe'}
)
```

多くのコマンドレットでは、変数にオブジェクトが割り当てられると、それらのオブジェクトのコレクションを配列として返します。

```powershell
$processList = Get-Process
```

既に説明したすべての基本機能はオブジェクトの配列にも適用されますが、指摘しておくべき点がいくつかあります。

### <a name="accessing-properties"></a>プロパティへのアクセス

値型と同様に、インデックスを使用してコレクション内の個々の項目にアクセスできます。

```powershell
PS> $data[0]

FirstName LastName
-----     ----
Kevin     Marquette
```

プロパティは、直接アクセスして更新できます。

```powershell
PS> $data[0].FirstName

Kevin

PS> $data[0].FirstName = 'Jay'
PS> $data[0]

FirstName LastName
-----     ----
Jay       Marquette
```

#### <a name="array-properties"></a>配列のプロパティ

通常、すべてのプロパティにアクセスするには、次のようにリスト全体を列挙する必要があります。

```powershell
PS> $data | ForEach-Object {$_.LastName}

Marquette
Doe
```

または、`Select-Object -ExpandProperty` コマンドレットを使用します。

```powershell
PS> $data | Select-Object -ExpandProperty LastName

Marquette
Doe
```

ただし、PowerShell では `LastName` を直接要求することができます。 PowerShell では、ユーザーに代わってそれらをすべて列挙し、クリーンなリストを返します。

```powershell
PS> $data.LastName

Marquette
Doe
```

列挙は今までどおり行われますが、その背後にある複雑な操作は表示されません。

### <a name="where-object-filtering"></a>Where-Object のフィルタリング

ここでは `Where-Object` が使用されるため、オブジェクトのプロパティに基づいて、配列から必要なものをフィルター処理して選択できます。

```powershell
PS> $data | Where-Object {$_.FirstName -eq 'Kevin'}

FirstName LastName
-----     ----
Kevin     Marquette
```

これと同じクエリを作成して、探している `FirstName` を取得できます。

```powershell
$data | Where FirstName -eq Kevin
```

#### <a name="where"></a>Where()

配列には、フィルターとして `scriptblock` を指定できる `Where()` メソッドが用意されています。

```powershell
$data.Where({$_.FirstName -eq 'Kevin'})
```

この機能は、PowerShell 4.0 で追加されました。

### <a name="updating-objects-in-loops"></a>ループ内のオブジェクトの更新

値型の場合、値を置き換えるインデックスを把握しておく必要があるため、配列を更新する唯一の方法は for ループを使用することです。 オブジェクトは参照型であるため、さらに多くの選択肢があります。 以下に簡単な例を示します。

```powershell
foreach($person in $data)
{
    $person.FirstName = 'Kevin'
}
```

このループでは、`$data` 配列内のすべてのオブジェクトを調べています。 オブジェクトは参照型であるため、`$person` 変数は配列内のまったく同じオブジェクトを参照します。 そのため、そのオブジェクトのプロパティを更新すると、元のオブジェクトが更新されます。

オブジェクト全体をこのように置き換えることはまだできません。 `$person` 変数に新しいオブジェクトを割り当てようとする場合は、配列内の元のオブジェクトをもう指さなくなった別のものに変数の参照を更新します。 これは期待どおりに機能しません。

```powershell
foreach($person in $data)
{
    $person = [pscustomobject]@{
        FirstName='Kevin'
        LastName='Marquette'
    }
}
```

## <a name="operators"></a>オペレーター

PowerShell の演算子は、配列でも機能します。 それらの一部の動作は若干異なります。

### <a name="-join"></a>-join

`-join` 演算子が最もわかりやすいので、最初にそれを見てみましょう。 `-join` 演算子は気に入っているため、頻繁に使用しています。 これは、配列内のすべての要素を、指定した文字または文字列と結合します。

```powershell
PS> $data = @(1,2,3,4)
PS> $data -join '-'
1-2-3-4
PS> $data -join ','
1,2,3,4
```

`-join` 演算子に関して気に入っている機能の 1 つに、単一の項目を処理することが挙げられます。

```powershell
PS> 1 -join '-'
1
```

これは、ログ記録や詳細メッセージの内部で使用します。

```powershell
PS> $data = @(1,2,3,4)
PS> "Data is $($data -join ',')."
Data is 1,2,3,4.
```

#### <a name="-join-array"></a>-join $array

Lee Dailey 氏が指摘した巧みな技法の 1 つを次に示します。 区切り記号を使用せずにすべてを結合する場合は、次のようにするのではなく、

```powershell
PS> $data = @(1,2,3,4)
PS> $data -join $null
1234
```

`-join` をプレフィックスなしのパラメーターとして配列で使用できます。 次の例を見て、私が説明していることを確認してください。

```powershell
PS> $data = @(1,2,3,4)
PS> -join $data
1234
```

### <a name="-replace-and--split"></a>-replace と -split

`-replace` や `-split` などの他の演算子は、配列内の各項目に対して実行されます。 それらをこのように使用したことはありませんが、次に例を示します。

```powershell
PS> $data = @('ATX-SQL-01','ATX-SQL-02','ATX-SQL-03')
PS> $data -replace 'ATX','LAX'
LAX-SQL-01
LAX-SQL-02
LAX-SQL-03
```

### <a name="-contains"></a>-contains

`-contains` 演算子を使用すると、値の配列を調べて、指定した値が含まれているかどうかを確認できます。

```powershell
PS> $data = @('red','green','blue')
PS> $data -contains 'green'
True
```

### <a name="-in"></a>-in

1 つの値が複数の値のいずれかに一致するかどうかを確認する場合は、`-in` 演算子を使用できます。 演算子の左側に値が置かれ、右側に配列が置かれます。

```powershell
PS> $data = @('red','green','blue')
PS> 'green' -in $data
True
```

この方法は、リストが大きい場合にコストが高くなる可能性があります。 多くの値を確認する場合は、正規表現パターンがよく使用されます。

```powershell
PS> $data = @('red','green','blue')
PS> $pattern = "^({0})$" -f ($data -join '|')
PS> $pattern
^(red|green|blue)$

PS> 'green' -match $pattern
True
```

### <a name="-eq-and--ne"></a>-eq と -ne

等価と配列は複雑になる可能性があります。 配列が左側にある場合は、すべての項目が比較されます。 `True` が返されるのではなく、一致するオブジェクトが返されます。

```powershell
PS> $data = @('red','green','blue')
PS> $data -eq 'green'
green
```

`-ne` 演算子を使用する場合は、指定した値と等しくないすべての値が返されます。

```powershell
PS> $data = @('red','green','blue')
PS> $data -ne 'green'
red
blue
```

`if()` ステートメントでこれを使用する場合、返される値は `True` 値になります。 返される値がない場合は、`False` 値になります。 次に示すこれらのステートメントはどちらも `True` に評価されます。

```powershell
$data = @('red','green','blue')
if ( $data -eq 'green' )
{
    'Green was found'
}
if ( $data -ne 'green' )
{
    'And green was not found'
}
```

これについては、`$null` のテスト方法について説明するときに再度取り上げます。

### <a name="-match"></a>-match

`-match` 演算子は、コレクション内の各項目を一致させようとします。

```powershell
PS> $servers = @(
    'LAX-SQL-01'
    'LAX-API-01'
    'ATX-SQL-01'
    'ATX-API-01'
)
PS> $servers -match 'SQL'
LAX-SQL-01
ATX-SQL-01
```

1 つの値で `-match` を使用する場合は、特殊変数 `$Matches` に一致情報が入力されます。 配列がこのように処理される場合、これは当てはまりません。

`Select-String` を使用して同様の手法をとることができます。

```powershell
$servers | Select-String SQL
```

`Select-String`、`-match`、および `$matches` 変数については、[正規表現を使用するさまざまな方法][]という別の投稿で詳しく説明しています。

### <a name="null-or-empty"></a>$null または空

`$null` または空の配列かどうかのテストは、難しい場合があります。 配列を使用した一般的なトラップを次に示します。

一見すると、このステートメントは正しく機能するように見えます。

```powershell
if ( $array -eq $null)
{
    'Array is $null'
}
```

しかし、`-eq` が配列内の各項目を確認する方法について説明したばかりです。 そのため、1 つの $null 値を含む、複数の項目の配列を作成することができ、それは `$true` に評価されます

```powershell
$array = @('one',$null,'three')
if ( $array -eq $null)
{
    'I think Array is $null, but I would be wrong'
}
```

演算子の左側に `$null` を配置することがベスト プラクティスであるのはそのためです。 これにより、このシナリオが問題になることはありません。

```powershell
if ( $null -eq $array )
{
    'Array actually is $null'
}
```

`$null` 配列は、空の配列と同じではありません。 配列があることがわかっている場合は、その中のオブジェクトの数を確認します。 配列が `$null` の場合、カウントは `0` です。

```powershell
if ( $array.count -gt 0 )
{
    "Array isn't empty"
}
```

ここで注意すべきトラップがもう 1 つあります。 オブジェクトが `PSCustomObject` である場合を除き、オブジェクトが 1 つしかない場合でも `count` を使用できます。 これは、PowerShell 6.1 で修正されたバグです。
これはよいニュースですが、多くのユーザーがまだ 5.1 を使用しているため、注意が必要です。

```powershell
PS> $object = [PSCustomObject]@{Name='TestObject'}
PS> $object.count
$null
```

PowerShell 5.1 をまだ使用している場合は、配列内でそのオブジェクトをラップしてから、カウントを確認して正確な数を取得できます。

```powershell
if ( @($array).count -gt 0 )
{
    "Array isn't empty"
}
```

十分に安全な方法をとるには、`$null` がないか確認してから、カウントを調べます。

```powershell
if ( $null -ne $array -and @($array).count -gt 0 )
{
    "Array isn't empty"
}
```

### <a name="all--eq"></a>All -eq

[配列内のすべての値が指定された値と一致するかどうかを確認する方法][]について、だれかが最近質問していました。
Reddit ユーザーの **/u/bis** には、不適切な値がないかどうかを確認してから結果を反転させるという巧妙な [解決方法][]が載っていました。

```powershell
$results = Test-Something
if ( -not ( $results -ne 'Passed') )
{
    'All results a Passed'
}
```

## <a name="adding-to-arrays"></a>配列への追加

ここにきて、配列に項目を追加する方法が気になり始めています。 簡単に答えると、それはできません。 配列は、メモリ内で固定サイズです。 それを拡張したり、それに単一の項目を追加したりする必要がある場合は、新しい配列を作成して、古い配列からすべての値をコピーする必要があります。 これには多大な労力がかかるように思われますが、PowerShell では新しい配列の作成の複雑さは見えません。 PowerShell は、配列の加算演算子 (`+`) を実装します。

> [!NOTE]
> PowerShell は減算演算を実装していません。 配列に代わる柔軟な方法が必要な場合は、[ジェネリック `List`](#generic-list) オブジェクトを使用する必要があります。

### <a name="array-addition"></a>配列の加算

配列で加算演算子を使用すると、新しい配列を作成できます。 そのため、次の 2 つの配列を指定します。

```powershell
$first = @(
    'Zero'
    'One'
)
$second = @(
    'Two'
    'Three'
)
```

これらを合算して、新しい配列を取得できます。

```powershell
PS> $first + $second

Zero
One
Two
Three
```

### <a name="plus-equals-"></a>プラス イコール (+=)

新しい配列を所定の場所に作成し、次のように項目を追加できます。

```powershell
$data = @(
    'Zero'
    'One'
    'Two'
    'Three'
)
$data += 'four'
```

`+=` を使用するたびに、新しい配列を複製して作成していることを忘れないでください。 これは小規模なデータセットでは問題になりませんが、非常に不適切にスケーリングされます。

### <a name="pipeline-assignment"></a>パイプラインによる代入

どのパイプラインの結果も変数に代入することができます。 複数の項目が含まれている場合、それは配列です。

```powershell
$array = 1..5 | ForEach-Object {
    "ATX-SQL-$PSItem"
}
```

通常、パイプラインの使用を検討しているときは、一般的な PowerShell ワンライナーのことが浮かびます。 パイプラインは、`foreach()` ステートメントやその他のループと共に使用できます。 したがって、ループ内の配列に項目を追加するのではなく、パイプラインに項目をドロップできます。

```powershell
$array = foreach ( $node in (1..5))
{
    "ATX-SQL-$node"
}
```

## <a name="array-types"></a>配列の型

既定では、PowerShell の配列は `[PSObject[]]` 型として作成されます。 これにより、任意の型のオブジェクトまたは値を含めることができます。 これが機能するのは、`PSObject` 型からすべてが継承されるためです。

### <a name="strongly-typed-arrays"></a>厳密に型指定された配列

同様の構文を使用して、任意の型の配列を作成できます。 厳密に型指定された配列を作成すると、指定した型の値またはオブジェクトのみを含めることができます。

```powershell
PS> [int[]] $numbers = 1,2,3
PS> [int[]] $numbers2 = 'one','two','three'
ERROR: Cannot convert value "one" to type "System.Int32". Input string was not in a correct format."

PS> [string[]] $strings = 'one','two','three'
```

### <a name="arraylist"></a>ArrayList

配列への項目の追加は最大の制限事項の 1 つですが、この問題を解決するために使用できる他のコレクションがいくつかあります。

`ArrayList` は通常、迅速に処理できる配列が必要な場合に最初に思い付くものの 1 つです。 それが必要となるすべての場所でオブジェクトの配列のように動作しますが、項目の追加をすばやく処理します。

ここでは、`ArrayList` を作成し、それに項目を追加する方法について説明します。

```powershell
$myarray = [System.Collections.ArrayList]::new()
[void]$myArray.Add('Value')
```

この型を取得するために .NET を呼び出します。 この場合、既定のコンストラクターを使用して作成します。 次に、`Add` メソッドを呼び出して項目を追加します。

行の先頭で `[void]` を使用している理由は、リターン コードが表示されないようにするためです。 一部の .NET 呼び出しではこれを実行して、予期しない出力が作成されることがあります。

配列に格納されているデータが文字列のみの場合は、[StringBuilder][] の使用方法もご確認ください。 これはほぼ同じものですが、文字列を処理するためだけのメソッドがいくつかあります。 `StringBuilder` は、特にパフォーマンスを考慮して設計されています。

ユーザーが配列から `ArrayList` に変えることはよくあります。 しかし、これは C# にジェネリックのサポートがなかったときに作られたものです。 `ArrayList` は、ジェネリックの `List[]` のサポートでは非推奨です

### <a name="generic-list"></a>ジェネリックのリスト

ジェネリック型は、汎用化されたクラスを定義する C# の特殊な型で あり、ユーザーは作成時にそれが使用するデータ型を指定します。 したがって、数値または文字列のリストが必要な場合は、`int` または `string` 型のリストが必要であることを定義します。

ここでは、文字列のリストを作成する方法について説明します。

```powershell
$mylist = [System.Collections.Generic.List[string]]::new()
```

または数値のリストを作成します。

```powershell
$mylist = [System.Collections.Generic.List[int]]::new()
```

オブジェクトを最初に作成せずに、次のように既存の配列をリストにキャストできます。

```powershell
$mylist = [System.Collections.Generic.List[int]]@(1,2,3)
```

PowerShell 5 以降では、`using namespace` ステートメントを使用して構文を短縮できます。 `using` ステートメントは、スクリプトの最初の行である必要があります。 名前空間を宣言することで、PowerShell ではデータ型を参照するときにデータ型からそれを省くことができます。

```powershell
using namespace System.Collections.Generic
$myList = [List[int]]@(1,2,3)
```

これにより、`List` がはるかに使いやすくなります。

同様の `Add` メソッドも用意されています。 ArrayList とは異なり、`Add` メソッドには戻り値がないため、それを `void` する必要はありません。

```powershell
$myList.Add(10)
```

また、他の配列のように要素にアクセスすることもできます。

```powershell
PS> $myList[-1]
10
```

#### <a name="listpsobject"></a>List[PSObject]

任意の型のリストを持つことができますが、オブジェクトの型がわからない場合は、`[List[PSObject]]` を使用してそれらを含めることができます。

```powershell
$list = [List[PSObject]]::new()
```

#### <a name="remove"></a>Remove()

`ArrayList` とジェネリックの `List[]` はどちらも、コレクションからの項目の削除をサポートしています。

```powershell
using namespace System.Collections.Generic
$myList = [List[string]]@('Zero','One','Two','Three')
[void]$myList.Remove("Two")
Zero
One
Three
```

値型を使用する場合は、リストから最初の値が削除されます。 それを何度も繰り返して呼び出すと、その値を削除し続けることができます。 参照型がある場合は、削除するオブジェクトを指定する必要があります。

```powershell
[list[System.Management.Automation.PSDriveInfo]]$drives = Get-PSDrive
$drives.remove($drives[2])
```

```powershell
$delete = $drives[2]
$drives.remove($delete)
```

remove メソッドは、項目を検索してコレクションから削除できる場合は `true` を返します。

### <a name="more-collections"></a>その他のコレクション

使用できるコレクションは他にも多数ありますが、これらは優れたジェネリックの配列の置換です。
これらのオプションに関心をお持ちの場合は、[Mark Kraus 氏](https://get-powershellblog.blogspot.com/2016/11/about-mark-kraus.html)がまとめたこちらの[Gist](https://gist.github.com/kevinblumenfeld/4a698dbc90272a336ed9367b11d91f1c) をご覧ください。

## <a name="other-nuances"></a>その他の微妙な違い

すべての主要な機能について説明したので、これを終わりにする前に触れておきたかったいくつかの点を挙げます。

### <a name="pre-sized-arrays"></a>事前にサイズ調整された配列

配列の作成後にそのサイズを変更できないことについては説明しました。 事前に決められたサイズの配列を作成するには、`new($size)` コンストラクターを使用してそれを呼び出します。

```powershell
$data = [Object[]]::new(4)
$data.count
4
```

### <a name="multiplying-arrays"></a>配列の乗算

興味深い小技の 1 つに、配列と整数を乗算できることがあります。

```powershell
PS> $data = @('red','green','blue')
PS> $data * 3
red
green
blue
red
green
blue
red
green
blue
```

### <a name="initialize-with-0"></a>0 で初期化

一般的なシナリオでは、すべてゼロで配列を作成します。 整数のみを使用する場合は、厳密に型指定された整数の配列の既定値がすべて 0 になります。

```powershell
PS> [int[]]::new(4)
0
0
0
0
```

乗算技法を使用してこれを行うこともできます。

```powershell
PS> $data = @(0) * 4
PS> $data
0
0
0
0
```

乗算技法には、任意の値を使用できるという良い点があります。 したがって、既定値として `255` を使用する場合は、この方法を使用することをお勧めします。

```powershell
PS> $data = @(255) * 4
PS> $data
255
255
255
255
```

### <a name="nested-arrays"></a>入れ子になった配列

配列内の配列は、入れ子になった配列と呼ばれます。 PowerShell ではあまり使用しませんが、他の言語では使用していました。 データがパターンのようなグリッドに収まる場合は、配列の配列を使用することを検討してください。

2 次元配列を作成するには、次の 2 つの方法があります。

```powershell
$data = @(@(1,2,3),@(4,5,6),@(7,8,9))

$data2 = @(
    @(1,2,3),
    @(4,5,6),
    @(7,8,9)
)
```

これらの例では、コンマは非常に重要です。 以前に示した通常の複数行での配列の例では、コンマは省略可能でした。 多次元配列の場合はそうではありません。

インデックス表記を使用する方法は、入れ子になった配列を使用するようになったところで多少変更されています。 上記の `$data` を使用して、値 3 にアクセスする方法を次に示します。

```powershell
PS> $outside = 0
PS> $inside = 2
PS> $data[$outside][$inside]
3
```

配列の入れ子のレベルごとに 1 組の角かっこを追加します。 最初の 1 組の角かっこは、一番外側の配列を対象とし、そこから順番に内側に入っていきます。

### <a name="write-output--noenumerate"></a>Write-Output -NoEnumerate

PowerShell では、配列のラップ解除または列挙が好まれます。 これは、PowerShell がパイプラインを使用する方法の主要な側面ですが、場合によってはそれを発生させたくないことがあります。

通常、オブジェクトの詳細を学習するには、パイプを使用してオブジェクトを `Get-Member` に渡します。 パイプを使用して配列をそれに渡すと、ラップが解除され、Get-Member は実際の配列ではなく配列のメンバーを認識します。

```powershell
PS> $data = @('red','green','blue')
PS> $data | Get-Member
TypeName: System.String
...
```

こうした配列のラップ解除を回避するには、`Write-Object -NoEnumerate` を使用します。

```powershell
PS> Write-Output -NoEnumerate $data | Get-Member
TypeName: System.Object[]
...
```

ハッキングのような 2 つ目の方法もあります (このようなハッキングは避けるようにしています)。 パイプを使用する前に、配列の前にコンマを配置できます。

```powershell
PS> ,$data | Get-Member
TypeName: System.Object[]
...
```

### <a name="return-an-array"></a>配列を返す

こうした配列のラップ解除は、関数から値を出力または返す場合にも発生します。 出力を変数に割り当てた場合でも配列を取得できるため、通常は問題になりません。

新しい配列があることをキャッチします。 これが問題になる場合は、`Write-Output -NoEnumerate $array` または `return ,$array` を使用して回避できます。

## <a name="anything-else"></a>その他

覚えることがたくさんあって大変なことはわかっています。 これから長い期間にわたって、この記事を読むたびにそこから何かを学び、それがご自身にとってよい参考となれば幸いです。 これが役に立つことがわかった場合は、そこから価値を得ることができると思われる他のユーザーと共有してください。

この場から、[ハッシュテーブル][]について記述した同様の投稿を確認することをお勧めします。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2018-10-15-Powershell-arrays-Everything-you-wanted-to-know/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[配列]: /powershell/module/microsoft.powershell.core/about/about_arrays
[switch ステートメント]: everything-about-switch.md
[ハッシュテーブル]: everything-about-hashtable.md
[正規表現を使用するさまざまな方法]: https://powershellexplained.com/2017-07-31-Powershell-regex-regular-expression/
[配列内のすべての値が指定された値と一致するかどうかを確認する方法]: https://www.reddit.com/r/PowerShell/comments/9mzo09/if_statement_multiple_variables_but_1_condition
[解決方法]: https://www.reddit.com/r/PowerShell/comments/9mzo09/if_statement_multiple_variables_but_1_condition/e7iizca
[StringBuilder]: https://powershellexplained.com/2017-11-20-Powershell-StringBuilder/
