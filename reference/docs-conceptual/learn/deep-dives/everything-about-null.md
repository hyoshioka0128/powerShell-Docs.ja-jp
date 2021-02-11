---
title: $null について知りたかったことのすべて
description: PowerShell の $null は、単純なものと見なされがちですが、さまざまなニュアンスの違いがあります。 予期しない null 値が発生した場合に何が起きているのかを理解できるように、$null について詳しく見ていきましょう。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: e0553a5e17450d8044f548792649369e99903850
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "84149475"
---
# <a name="everything-you-wanted-to-know-about-null"></a>$null について知りたかったことのすべて

PowerShell の `$null` は、単純なものと見なされがちですが、さまざまなニュアンスの違いがあります。 予期しない `$null` 値が発生した場合に何が起きているのかを理解できるように、`$null` について詳しく見ていきましょう。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="what-is-null"></a>null 値とは

null 値を、不明な値や空の値を指すものとお考えかもしれません。 変数は、値やオブジェクトを割り当てるまでは null 値です。 このことを理解しておくことは重要です。なぜなら、コマンドの中には、値が必要で、その値が null 値の場合はエラーを生成するものがあるからです。

### <a name="powershell-null"></a>PowerShell の $null

`$null` は、null 値を表すために PowerShell で使用される自動変数です。 これは、変数に割り当てること、比較で使用すること、コレクション内で null 値のプレース ホルダーとして使用することができます。

PowerShell では、`$null` は、null 値を値として持つオブジェクトとして扱われます。 これは、他の言語をこれまで使用してきた開発者にとって、想定と異なる場合があります。

## <a name="examples-of-null"></a>$null の例

初期化していない変数を使用しようとする場合、その値は常に `$null` です。 これは、`$null` 値がコードに忍び込む最もよくあるケースの 1 つです。

```powershell
PS> $null -eq $undefinedVariable
True
```

開発者が変数名を誤って入力してしまうと、PowerShell はそれを別の変数と見なし、その値は `$null` になります。

`$null` 値が発生するもう 1 つのケースは、結果を何も返さない別のコマンドに由来している場合です。

```powershell
PS> function Get-Nothing {}
PS> $value = Get-Nothing
PS> $null -eq $value
True
```

## <a name="impact-of-null"></a>$null の影響

`$null` は、どこで発生するかによって、コードに異なる影響を与えます。

### <a name="in-strings"></a>文字列の場合

`$null` を文字列で使用する場合、これは空白の値 (または空の文字列) です。

```powershell
PS> $value = $null
PS> Write-Output "The value is $value"
The value is
```

これは、ログ メッセージ内で変数を使用する場合に、その変数を角かっこで囲むことを私が好む理由の 1 つです。 さらに重要なのは、変数の値が文字列の末尾に来る場合に、変数の値の両端がはっきりわかるようにできることです。

```powershell
PS> $value = $null
PS> Write-Output "The value is [$value]"
The value is []
```

こうすることにより、空の文字列と `$null` の値を見つけやすくなります。

### <a name="in-numeric-equation"></a>数値式の場合

`$null` 値が数値式で使用される場合、エラーにならない限り、その結果は無効になります。 `$null` は `0` と評価される場合もあれば、それにより結果全体が `$null` になる場合もあります。
値の順序によって、結果が 0 または `$null` になる乗算の例を次に示します。

```powershell
PS> $null * 5
PS> $null -eq ( $null * 5 )
True

PS> 5 * $null
0
PS> $null -eq ( 5 * $null )
False
```

### <a name="in-place-of-a-collection"></a>コレクション内の場合

コレクションを使用すると、インデックスを使って値にアクセスできます。 実際には `null` であるコレクションにインデックスを付けようとすると、エラー `Cannot index into a null array` が返されます。

```powershell
PS> $value = $null
PS> $value[10]
Cannot index into a null array.
At line:1 char:1
+ $value[10]
+ ~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [], RuntimeException
    + FullyQualifiedErrorId : NullArray
```

コレクションは存在するものの、そのコレクションに含まれていない要素にアクセスしようとすると、結果として `$null` が返されます。

```powershell
$array = @( 'one','two','three' )
$null -eq $array[100]
True
```

### <a name="in-place-of-an-object"></a>オブジェクト内の場合

指定されたプロパティを持たないオブジェクトのプロパティやサブプロパティにアクセスしようとすると、未定義の変数の場合と同様に `$null` 値が返されます。 この場合、その変数が `$null` であるか実際のオブジェクトであるかは関係ありません。

```powershell
PS> $null -eq $undefined.some.fake.property
True

PS> $date = Get-Date
PS> $null -eq $date.some.fake.property
True
```

### <a name="method-on-a-null-valued-expression"></a>null 値式のメソッド

`$null` オブジェクトでメソッドを呼び出すと、`RuntimeException` がスローされます。

```powershell
PS> $value = $null
PS> $value.toString()
You cannot call a method on a null-valued expression.
At line:1 char:1
+ $value.tostring()
+ ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [], RuntimeException
    + FullyQualifiedErrorId : InvokeMethodOnNull
```

`You cannot call a method on a null-valued expression` が表示された場合、最初に `$null` がないかどうかメソッドを確認するのではなく、まず、変数でメソッドを呼び出している場所を探します。

## <a name="checking-for-null"></a>$null の確認

これまでの例で、`$null` を確認するときに、`$null` が常に左側に置かれていることにお気付きになったかもしれません。 これは意図的なもので、PowerShell ではベスト プラクティスとして受け入れられる方法です。 右側に置くと期待どおりの結果が得られないシナリオがあるからです。

次の例を見て、結果を予想してみてください。

```powershell
if ( $value -eq $null )
{
    'The array is $null'
}
if ( $value -ne $null )
{
    'The array is not $null'
}
```

`$value` を定義しない場合、最初のものは `$true` と評価され、`The array is $null` というメッセージが表示されます。 落とし穴になるのは、どちらも `$false` となるような `$value` を作成できる点です。

```powershell
$value = @( $null )
```

この場合、`$value` は `$null` を含む配列です。 `-eq` は、配列内のすべての値をチェックし、一致する `$null` を返します。 これは、`$false` と評価されます。 `-ne` は、`$null` と一致しないものすべてを返します。この例の場合、結果はありません (こちらも `$false` と評価されます)。 一方は `$true` になりそうに思えますが、どちらもそうはならないのです。

どちらも `$false` と評価される値を作成できるだけでなく、どちらも `$true` と評価される値を作成することもできます。 Mathias Jessen (@IISResetMe) 氏が、このシナリオを詳しく説明する[優れた記事][]を投稿しています。

### <a name="psscriptanalyzer-and-vscode"></a>PSScriptAnalyzer と VSCode

[PSScriptAnalyzer][] モジュールには、この問題を確認する、`PSPossibleIncorrectComparisonWithNull` という名前のルールが用意されています。

```powershell
PS> Invoke-ScriptAnalyzer ./myscript.ps1

RuleName                              Message
--------                              -------
PSPossibleIncorrectComparisonWithNull $null should be on the left side of equality comparisons.
```

VS Code でも PSScriptAnalyser ルールが使用されるので、それによってもこれがスクリプト内の問題として強調表示または識別されます。

### <a name="simple-if-check"></a>単純な if によるチェック

$null 以外の値を確認する一般的な方法としては、比較を含まない単純な `if()` ステートメントを使用する方法があります。

```powershell
if ( $value )
{
    Do-Something
}
```

値が `$null` の場合、これは `$false` と評価されます。 簡単そうに見えますが、このコードの実際の評価内容と、このコードが評価していると思っているものとが全く同じであるかに注意する必要があります。 このコード行は、次のように見えます。

> `$value` に値があるかどうか。

しかし、それだけではありません。 この行が実際に意味しているのは、次のことです。

> `$value` が、`$null` でも `0` でも `$false` でも `empty string` でもないかどうか。

このステートメントをより完全に記述したサンプルを以下に示します。

```powershell
if ( $null -ne $value -and
        $value -ne 0 -and
        $value -ne '' -and
        $value -ne $false )
{
    Do-Something
}
```

これらの他の値も `$false` としてカウントされるのであって、変数に値があることをただチェックしているわけではないことを意識している限り、基本的な `if` チェックを使用しても全く問題ありません。

数日前にコードをリファクタリングしていたとき、私はこの問題に出くわしました。 そのコードには、次のような基本的なプロパティ チェックが含まれていました。

```powershell
if ( $object.property )
{
    $object.property = $value
}
```

値が存在していた場合にのみ、オブジェクト プロパティに値を割り当てるのが目的でした。 ほとんどの場合、基になるオブジェクトには `if` ステートメントで `$true` と評価される値がありました。 しかし、値が設定されていないものがあるという問題が発生していました。 コードをデバッグしてみると、そのオブジェクトにプロパティはあったのですが、そのプロパティは空の文字列値だったのです。 そのため、上の例のロジックでは値が更新されませんでした。 そこで、適切な `$null` チェックを追加したところ、問題はすべて解決しました。

```powershell
if ( $null -ne $object.property )
{
    $object.property = $value
}
```

このような細かなバグは非常に見つけにくいので、`$null` の値は積極的にチェックするようにしています。

## <a name="nullcount"></a>$null.Count

`$null` 値のプロパティにアクセスしようとする場合、そのプロパティも `$null` です。 `count` プロパティは、このルールの例外です。

```powershell
PS> $value = $null
PS> $value.count
0
```

`$null` 値がある場合、その `count` は `0` になります。 この特別なプロパティは、PowerShell によって追加されます。

### <a name="pscustomobject-count"></a>[PSCustomObject] Count

PowerShell のほとんどすべてのオブジェクトに、count プロパティがあります。 重要な例外の 1 つは、Windows PowerShell 5.1 の `[PSCustomObject]` です (PowerShell 6.0 では修正済み)。 これには count プロパティがないため、使用しようとすると `$null` 値が返されます。 `$null` チェックの代わりに `.Count` を使用しようとすることがないようにご注意ください。

このサンプルを Windows PowerShell 5.1 と PowerShell 6.0 とで実行すると、異なる結果が得られます。

```powershell
$value = [PSCustomObject]@{Name='MyObject'}
if ( $value.count -eq 1 )
{
    "We have a value"
}
```

## <a name="empty-null"></a>空の null 値

他と異なる動作をする、特殊な `$null` が 1 つあります。 これを、空の `$null` と呼ぶことにします (実際には [System.Management.Automation.Internal.AutomationNull][] です)。 この空の `$null` は、何も返さない (無効な結果を返す) 関数またはスクリプト ブロックの結果として返されるものです。

```powershell
PS> function Get-Nothing {}
PS> $nothing = Get-Nothing
PS> $null -eq $nothing
True
```

これを `$null` と比較すると、`$null` 値が得られます。 値が必要な評価に使用される場合、その値は常に `$null` になります。 しかし、これを配列内に配置すると、空の配列と同じように扱われます。

```powershell
PS> $containempty = @( @() )
PS> $containnothing = @($nothing)
PS> $containnull = @($null)

PS> $containempty.count
0
PS> $containnothing.count
0
PS> $containnull.count
1
```

`$null` 値を 1 つ含む配列を作成できます。その `count` は `1` です。 しかし、配列内に空の結果を配置すると、それは項目としてカウントされません。 そのカウントは `0` です。

空の `$null` をコレクションのように扱うと、それは空になります。

### <a name="pipeline"></a>パイプライン

この違いが最もよく見られるのは、パイプラインを使用する場合です。 `$null` 値はパイプを使用して渡し、空の `$null` 値は渡さないようにすることができます。

```powershell
PS> $null | ForEach-Object{ Write-Output 'NULL Value' }
'NULL Value'
PS> $nothing | ForEach-Object{ Write-Output 'No Value' }
```

コードによっては、ロジック内の `$null` を説明する必要があります。

まず `$null` をチェックするか、

- パイプラインで null 値をフィルター処理して除外します (`... | Where {$null -ne $_} | ...`)
- パイプライン関数で処理します

## <a name="foreach"></a>foreach

`foreach` で私が気に入っている特徴の 1 つは、それが `$null` コレクションを列挙しないことです。

```powershell
foreach ( $node in $null )
{
    #skipped
}
```

そのため、コレクションを列挙する前にそのコレクションに対して `$null` チェックをする必要はありません。 `$null` 値のコレクションがある場合でも、`$node` はやはり `$null` になります。

foreach がこのように動作するようになったのは、PowerShell 3.0 からです。 これより古いバージョンを使用している場合は、動作が異なります。 これは、2.0 との互換性のためにコードをバックポートする際に意識する必要のある、重要な変更点の 1 つです。

## <a name="value-types"></a>値の型

技術的には、参照型のみが `$null` になり得ます。 しかし、PowerShell は非常に寛容で、変数がどのような型であっても許容します。 値の型を厳密に型指定する場合は、`$null` にはできません。
PowerShell では、多くの型で `$null` を既定値に変換します。

```powershell
PS> [int]$number = $null
PS> $number
0

PS> [bool]$boolean = $null
PS> $boolean
False

PS> [string]$string = $null
PS> $string -eq ''
True
```

一部、`$null` から有効に変換されない型も存在します。 このような型では、`Cannot convert null to type` エラーが発生します。

```powershell
PS> [datetime]$date = $null
Cannot convert null to type "System.DateTime".
At line:1 char:1
+ [datetime]$date = $null
+ ~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ArgumentTransformationMetadataException
    + FullyQualifiedErrorId : RuntimeException
```

### <a name="function-parameters"></a>関数のパラメーター

関数パラメーターで厳密に型指定された値を使用するのは、非常に一般的な方法です。 スクリプトの他の変数の型を定義しようとは考えないとしても、パラメーターの型は定義することが習慣になっているものです。 関数の中で厳密に型指定された変数を既に使用していても、それに気づいていないことさえあり得ます。

```powershell
function Do-Something
{
    param(
        [String] $Value
    )
}
```

パラメーターの型を `string` として設定した時点で、その値は `$null` にはできなくなります。 値が `$null` かどうかをチェックして、ユーザーが値を指定したかどうかを確認するのは一般的な方法です。

```powershell
if ( $null -ne $Value ){...}
```

値が指定されない場合、`$Value` は空の文字列 `''` になります。 代わりに、自動変数 `$PSBoundParameters.Value` を使用しましょう。

```powershell
if ( $null -ne $PSBoundParameters.Value ){...}
```

`$PSBoundParameters` には、その関数が呼び出された時点で指定済みのパラメーターのみが含まれます。
`ContainsKey` メソッドを使用してプロパティを確認することもできます。

```powershell
if ( $PSBoundParameters.ContainsKey('Value') ){...}
```

### <a name="isnotnullorempty"></a>IsNotNullOrEmpty

値が文字列の場合は、静的な文字列関数を使用して、値が `$null` であるか空の文字列であるかを同時に確認できます。

```powershell
if ( -not [string]::IsNullOrEmpty( $value ) ){...}
```

個人的には、その値の型が文字列でなければならないことがわかっている場合に、これをよく使用しています。

## <a name="when-i-null-check"></a>$null チェックをする場面

私は防御的なスクリプト作成者です。 関数を呼び出して変数に割り当てるときは、それが `$null` でないかをいつも確認します。

```powershell
$userList = Get-ADUser kevmar
if ($null -ne $userList){...}
```

`try/catch` を使用するより、`if` や `foreach` を使用する方が断然好きです。 もちろん、`try/catch` もかなり使用します。 それでも、エラー条件をテストしたり、結果の空のセットをテストしたりできるのであれば、例外処理で真の例外を処理することを許容できます。

値にインデックスを付けたり、オブジェクトでメソッドを呼び出したりする前にも、`$null` を確認するようにしています。 これらの 2 つのアクションは、`$null` オブジェクトの場合は失敗します。そのため、まずそれらを検証するのは重要なことだと考えています。 これらのシナリオについては、この記事で既に説明しました。

### <a name="no-results-scenario"></a>結果が存在しないシナリオ

関数やコマンドが異なると、結果が存在しないシナリオの処理方法が異なることを理解していることは重要です。 多くの PowerShell コマンドでは、空の `$null` とエラーがエラー ストリームで返されます。 しかし、例外をスローするものや、ステータス オブジェクトを返すものもあります。 使用するコマンドが、結果が存在しないシナリオやエラー シナリオをどのように処理するかを理解しておく必要があります。

## <a name="initializing-to-null"></a>$null への初期化

私は、変数を使用する前に、使用する変数すべてを初期化することを習慣にしています。 これは、他の言語でも行う必要があります。 関数の先頭か、foreach ループに入った時点で、使用する予定のすべての値を定義するようにします。

注意してご覧いただきたいシナリオを次に示します。 これは、私が以前に見つけ出す必要があったバグの例です。

```powershell
function Do-Something
{
    foreach ( $node in 1..6 )
    {
        try
        {
            $result = Get-Something -ID $node
        }
        catch
        {
            Write-Verbose "[$result] not valid"
        }

        if ( $null -ne $result )
        {
            Update-Something $result
        }
    }
}
```

ここでは、`Get-Something` が結果か空の `$null` のどちらかを返すものと想定されています。 エラーが発生すると、ログに記録されます。 次に、有効な結果が得られたことを確認してから、それを処理します。

このコードに潜むバグは、`Get-Something` が例外をスローし、`$result` に値を割り当てない場合です。 値を割り当てる前に失敗しているため、`$null` を `$result` 変数に割り当てることさえできていません。 `$result` には、他のイテレーションで取得した、前の有効な `$result` が含まれています。
この例では、`Update-Something` により、同一のオブジェクトに対する処理が複数回実行されることになります。

この問題を解決するため、foreach ループの先頭で `$result` を `$null` に設定してから、これを使用することにしました。

```powershell
foreach ( $node in 1..6 )
{
    $result = $null
    try
    {
        ...
```

### <a name="scope-issues"></a>スコープの問題

これは、スコープの問題を軽減するのにも役立ちます。 上の例では、ループ内で値を何度も `$result` に割り当てています。 しかし、PowerShell では、関数の外部から現在の関数のスコープに変数の値を入れられるため、関数内で変数の値を初期化することにより、そのような方法が原因で生じる可能性のあるバグを軽減できます。

関数内の初期化されていない変数は、親スコープで何らかの値に設定されている場合、`$null` ではありません。
親スコープは、関数を呼び出し、同じ変数名を使用する別の関数である場合も考えられます。

同じ `Do-something` の例を使ってループを削除すると、次の例のようなものが出来上がります。

```powershell
function Invoke-Something
{
    $result = 'ParentScope'
    Do-Something
}

function Do-Something
{
    try
    {
        $result = Get-Something -ID $node
    }
    catch
    {
        Write-Verbose "[$result] not valid"
    }

    if ( $null -ne $result )
    {
        Update-Something $result
    }
}
```

`Get-Something` への呼び出しが例外をスローすると、`$null` チェックで `Invoke-Something` の `$result` が見つかります。 関数内で値を初期化することにより、この問題を軽減できます。

変数に名前を付けるのは大変な作業であり、作成者が複数の関数で同じ変数名を使用するのはよくあることです。 私自身、いつも `$node` や `$result` や `$data` を使っています。 このため、異なるスコープの値が、使用されるべきでない場所で使用されることは十分に起こりうる問題なのです。

## <a name="redirect-output-to-null"></a>$null への出力のリダイレクト

この記事全体で `$null` 値について説明してきましたが、出力を `$null` にリダイレクトする方法に言及せずに説明を終えることはできません。 コマンドが出力する情報やオブジェクトを、非表示にすることが必要な場合があります。 出力を `$null` にリダイレクトすると、この処理を実現できます。

### <a name="out-null"></a>Out-Null

Out-Null コマンドは、パイプライン データを `$null` にリダイレクトするためにあらかじめ用意された方法です。

```powershell
New-Item -Type Directory -Path $path | Out-Null
```

### <a name="assign-to-null"></a>$null への割り当て

`$null` にコマンドの結果を割り当てることにより、`Out-Null` を使用するのと同じ結果が得られます。

```powershell
$null = New-Item -Type Directory -Path $path
```

`$null` は定数値であるため、上書きすることはできません。 個人的にはコードの見た目が好きではないのですが、`Out-Null` よりも処理が速いことがよくあります。

### <a name="redirect-to-null"></a>$null へのリダイレクト

リダイレクト演算子を使用して出力を `$null` に送信することもできます。

```powershell
New-Item -Type Directory -Path $path > $null
```

異なるストリームに出力するコマンドライン実行可能ファイルを処理する場合は、 次のようにすると、すべての出力ストリームを `$null` にリダイレクトできます。

```powershell
git status *> $null
```

## <a name="summary"></a>まとめ

この点についてさまざまな説明を行ってきました。この記事は、私の詳細説明のほとんどより断片的なものです。 それは、`$null` 値が PowerShell のさまざまな場所に現れ、出現箇所によってその意味合いが微妙に異なるからです。 この記事が、`$null` に関する皆さまの理解と、遭遇する可能性のある難解なシナリオの理解のお役に立てば幸いです。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2018-12-23-Powershell-null-everything-you-wanted-to-know/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[優れた記事]: https://blog.iisreset.me/schrodingers-argumentlist
[PSScriptAnalyzer]: https://www.powershellgallery.com/packages/PSScriptAnalyzer
[System.Management.Automation.Internal.AutomationNull]: /dotnet/api/system.management.automation.internal.automationnull
