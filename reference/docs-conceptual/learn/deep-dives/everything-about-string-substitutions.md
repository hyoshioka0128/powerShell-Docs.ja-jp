---
title: 文字列での変数の代入について知りたかったことのすべて
description: 文字列で変数を使用して、書式設定されたテキストを作成するには、多くの方法があります。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: 786526fb98dbf1b3ec7c5c6c985ac95b85a96259
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "86301320"
---
# <a name="everything-you-wanted-to-know-about-variable-substitution-in-strings"></a>文字列での変数の代入について知りたかったことのすべて

文字列で変数を使用する方法は数多くあります。 この変数の代入を呼び出そうとしていますが、変数の値を含むように文字列を書式設定する場合はいつでも参照することになります。 これは、スクリプト作成の初心者にはたいてい説明していることです。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログに掲載されました。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="concatenation"></a>連結

メソッドの最初のクラスは、連結として参照できます。 これは基本的に、いくつかの文字列を取得し、それらを一緒に結合することです。 書式設定された文字列を、連結を使用して構築することには長い歴史があります。

```powershell
$name = 'Kevin Marquette'
$message = 'Hello, ' + $name
```

追加する値が少しの値のみである場合、連結は正しく機能します。 しかし、これはすぐに複雑になる可能性があります。

```powershell
$first = 'Kevin'
$last = 'Marquette'
```

```powershell
$message = 'Hello, ' + $first + ' ' + $last + '.'
```

この簡単な例でさえ、読みにくさが増しています。

## <a name="variable-substitution"></a>変数の置換

PowerShell には、より簡単な別のオプションがあります。 文字列内で変数を直接指定できます。

```powershell
$message = "Hello, $first $last."
```

文字列を囲むのに使用する引用符の種類によって、違いが生じます。 二重引用符で囲まれた文字列では代入が可能ですが、単一引用符で囲まれた文字列ではできません。 一方が必要なときも他方が必要なときもあるため、選択肢があるのです。

## <a name="command-substitution"></a>コマンドの代入

プロパティの値を文字列で取得しようとしているときには、少し注意が必要になります。 ここで多くの初心者がつまずきます。 まず彼らが、何が機能するはずだと考えているかを説明しましょう (一見した限りはそうなるはずのように思えます)。

```powershell
$directory = Get-Item 'c:\windows'
$message = "Time: $directory.CreationTime"
```

`$directory` から `CreationTime` を取得することを期待していますが、その代わり、値としてこの `Time: c:\windows.CreationTime` を取得します。 この種類の代入では、基本変数のみが参照されることが理由です。 ピリオドは文字列の一部と見なされるため、より深く値を解決する処理は停止されます。

この処理だけが発生し、このオブジェクトは、文字列内に配置されるときの既定値として文字列を提供します。
一部のオブジェクトでは、代わりに `System.Collections.Hashtable` のような型名を提供します。 注意だけしておいてください。

PowerShell では、特殊な構文を使用すると、文字列内でのコマンドの実行が可能です。 これにより、これらのオブジェクトのプロパティを取得し、その他の任意のコマンドを実行して値を取得できます。

```powershell
$message = "Time: $($directory.CreationTime)"
```

これは、一部の状況では適切に機能しますが、数個の変数があるだけでも連結と同様に複雑になります。

### <a name="command-execution"></a>コマンド実行

文字列内でコマンドを実行できます。 この選択肢があっても、私はそれが好きではありません。 すぐに乱雑になり、デバッグが難しくなります。 私なら、コマンドを実行して変数に保存するか、書式設定文字列を使用するかのいずれかです。

```powershell
$message = "Date: $(Get-Date)"
```

## <a name="format-string"></a>[書式設定文字列]

.NET には、扱いがかなり容易だと思われる、文字列を書式設定する方法が用意されています。 最初に、そのための静的メソッドについて説明してから、同じことを行う PowerShell ショートカットを示します。

```powershell
# .NET string format string
[string]::Format('Hello, {0} {1}.',$first,$last)

# PowerShell format string
'Hello, {0} {1}.' -f $first, $last
```

ここで起きているのは、次のようなことです。文字列がトークン `{0}` および `{1}` に対して解析された後、その数値を使用して、指定された値からの選択を行っています。 文字列内のどこかにある 1 つの値を繰り返す場合は、その値番号を再利用できます。

文字列がより複雑になるほど、このアプローチからより多くの価値が得られます。

### <a name="format-values-as-arrays"></a>値を配列として書式設定する

書式設定行が長くなりすぎる場合は、値をまず配列内に配置できます。

```powershell
$values = @(
    "Kevin"
    "Marquette"
)
'Hello, {0} {1}.' -f $values
```

配列全体を渡すため、記述が長くなりませんが、考え方は同様です。

## <a name="advanced-formatting"></a>高度な書式設定

既に詳細に[文書化済み][]の書式設定オプションが多数存在するため、私は意図的に、これらのオプションは .NET 由来のものであると述べました。 さまざまなデータ型を書式設定するために、組み込みの方法が用意されています。

```powershell
"{0:yyyyMMdd}" -f (Get-Date)
"Population {0:N0}" -f  8175133
```

それらについては説明しませんが、必要な場合には、これは非常に強力な書式設定エンジンであることを指摘させてください。

## <a name="joining-strings"></a>文字列の結合

場合によっては、値のリストを連結することが実際に必要になることがあります。 これを行ってくれる `-join` 演算子があります。 文字列の間をつなぐ文字を指定することもできます。

```powershell
$servers = @(
    'server1'
    'server2'
    'server3'
)

$servers  -join ','
```

区切り記号を使用せずに一部の文字列を `-join` する場合は、空の文字列 `''` を指定する必要があります。
しかし、それが必要なことのすべてであれば、より高速なオプションがあります。

```powershell
[string]::Concat('server1','server2','server3')
[string]::Concat($servers)
```

指摘しておくのに値するのは、文字列の `-split` も可能である点です。

## <a name="join-path"></a>Join-Path

これはよく見過ごされますが、ファイル パスを構築するための優れたコマンドレットです。

```powershell
$folder = 'Temp'
Join-Path -Path 'c:\windows' -ChildPath $folder
```

これの優れた点は、値を 1 つにまとめるときに円記号が正しく処理されることです。 これが特に重要なのは、ユーザーまたは構成ファイルから値を取得しようとしている場合です。

これは、`Split-Path` や `Test-Path` と使用するのにも適しています。 これらについては、[ファイルに対する読み取りと保存][]に関する私の投稿でも取り上げています。

## <a name="strings-are-arrays"></a>文字列は配列である

先に進む前に、ここで文字列の追加について言及する必要があります。 文字列は、文字の配列にすぎないことを思い出してください。 複数の文字列を一緒に追加すると、新しい配列が毎回作成されます。

次の例を見てください。

```powershell
$message = "Numbers: "
foreach($number in 1..10000)
{
    $message += " $number"
}
```

非常に基本的な内容に見えますが、見えていないのは、文字列が `$message` に追加されるたびに、まったく新しい文字列全体が作成されていることです。 メモリが割り当てられ、データがコピーされて、古いものが破棄されます。
数回実行するだけの場合は気にする必要はありませんが、このようなループから実際に問題が明るみに出るでしょう。

### <a name="stringbuilder"></a>StringBuilder

多数の小さな文字列から大きな文字列を構築する場合には、StringBuilder もよく使用されます。 追加したすべての文字列をただ収集し、最後に値を取得するときにすべての文字列を連結するだけであるのが、その理由です。

```powershell
$stringBuilder = New-Object -TypeName "System.Text.StringBuilder"

[void]$stringBuilder.Append("Numbers: ")
foreach($number in 1..10000)
{
    [void]$stringBuilder.Append(" $number")
}
$message = $stringBuilder.ToString()
```

これも、私が .NET の使用を考える処理内容です。 私が頻繁に使用することはもうありませんが、それが存在することを覚えておくとよいでしょう。

## <a name="delineation-with-braces"></a>中かっこを使用した区切り

これは、文字列内でのサフィックス連結に使用されます。 場合によっては、変数に明確なワード境界がないことがあります。

```powershell
$test = "Bet"
$tester = "Better"
Write-Host "$test $tester ${test}ter"
```

このテーマについては、[/u/real_parbold][] に感謝です。

次に、このアプローチに代わる方法を示します。

```powershell
Write-Host "$test $tester $($test)ter"
Write-Host "{0} {1} {0}ter" -f $test, $tester
```

私個人としては、これには書式設定文字列を使用しますが、どこかで目にした場合に備えて知っておいてください。

## <a name="find-and-replace-tokens"></a>検索と置換のトークン

これらの多くの機能により、独自のソリューションを書く必要性は限られていますが、内部の文字列を置き換える必要がある大きなテンプレート ファイルがある場合もあります。

ファイルからテンプレートを作成し、それには大量のテキストが含まれるとします。

```powershell
$letter = Get-Content -Path TemplateLetter.txt -RAW
$letter = $letter -replace '#FULL_NAME#', 'Kevin Marquette'
```

多数のトークンを置換することもあります。 うまいやり方は、検索と置換が容易な、非常に明確なトークンを使用することです。 私は得てして、区別しやすいように両端で特殊文字を使用します。

最近、これに取り組む新しい方法を見つけました。 これは一般的に使用されるパターンであるため、この記事ではこのセクションを残すことに決めました。

### <a name="replace-multiple-tokens"></a>複数のトークンを置換する

置換する必要があるトークンのリストがあるときには、より一般的なアプローチにします。 それをハッシュテーブル内に置き、反復処理して置換を実行します。

```powershell
$tokenList = @{
    Full_Name = 'Kevin Marquette'
    Location = 'Orange County'
    State = 'CA'
}

$letter = Get-Content -Path TemplateLetter.txt -RAW
foreach( $token in $tokenList.GetEnumerator() )
{
    $pattern = '#{0}#' -f $token.key
    $letter = $letter -replace $pattern, $token.Value
}
```

これらのトークンは、必要に応じて JSON や CSV から読み込むことができます。

### <a name="executioncontext-expandstring"></a>ExecutionContext の ExpandString

単一引用符で囲んで置換文字列を定義し、後で変数を拡張する巧みな方法があります。 次の例を見てください。

```powershell
$message = 'Hello, $Name!'
$name = 'Kevin Marquette'
$string = $ExecutionContext.InvokeCommand.ExpandString($message)
```

現在の実行コンテキストで `.InvokeCommand.ExpandString` を呼び出すと、置換には、現在のスコープ内の変数が使用されます。 ここで重要なのは、`$message` は、変数が存在する前であっても非常に早い段階で定義できることです。

それを少しだけ拡張すれば、異なる値を使用して、この置換を何度も繰り返し実行できます。

```powershell
$message = 'Hello, $Name!'
$nameList = 'Mark Kraus','Kevin Marquette','Lee Dailey'
foreach($name in $nameList){
    $ExecutionContext.InvokeCommand.ExpandString($message)
}
```

このアイデアをさらに進める場合、テキスト ファイルから大きなメール テンプレートをインポートし、これを行うことができます。 この[提案][]については、[Mark Kraus][] に感謝しなければなりません。

## <a name="whatever-works-the-best-for-you"></a>あなたにとって最適に機能することが重要

私は、書式指定文字列を使用するアプローチを愛好しています。 私は、より複雑な文字列を扱う場合でも、複数の変数がある場合でも、絶対にこれを行います。 非常に短ければ何に対しても、これらのいずれかを使用できるでしょう。

## <a name="anything-else"></a>その他

この記事では多くの範囲を取り上げました。 何か新しいことを学び、この記事を終えていただければ幸いです。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2017-01-13-powershell-variable-substitution-in-strings/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[Mark Kraus]: https://get-powershellblog.blogspot.com/
[提案]: https://www.reddit.com/r/PowerShell/comments/5npf8h/kevmar_everything_you_wanted_to_know_about/dcdfia5/
[/u/real_parbold]: https://www.reddit.com/r/PowerShell/comments/5npf8h/kevmar_everything_you_wanted_to_know_about/dcdfm6p/
[ファイルに対する読み取りと保存]: https://powershellexplained.com/2017-03-18-Powershell-reading-and-saving-data-to-files/
[文書化済み]: /dotnet/api/system.string.format#overloads
