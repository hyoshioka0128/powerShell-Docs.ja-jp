---
title: ハッシュテーブルについて知りたかったことのすべて
description: ハッシュテーブルは PowerShell で非常に重要であるため、十分に理解しておくことをお勧めします。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: e386e2aa2f7b85bee4bf622fd9251ef7642cf16a
ms.sourcegitcommit: 57e577097085dc621bd797ef4a7e2854ea7d4e29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2021
ms.locfileid: "97980503"
---
# <a name="everything-you-wanted-to-know-about-hashtables"></a>ハッシュテーブルについて知りたかったことのすべて

ここでは、一歩下がって、[ハッシュテーブル][]について説明します。 私は現在は常に使用しています。 昨晩ユーザー グループミーティングの後、これに関してあるユーザーに説明していたとき、私にも同じ混乱が生じていたことに気付きました。 ハッシュテーブルは PowerShell で非常に重要であるため、十分に理解しておくことをお勧めします。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="hashtable-as-a-collection-of-things"></a>もののコレクションとしてのハッシュテーブル

まず、ハッシュテーブルの従来の定義どおり、**ハッシュテーブル** をコレクションとして見てください。 この定義により、後でより高度なことに使用する場合の動作に関する基本的な理解が得られます。 この理解をスキップすることは、多くの場合、混乱の原因になります。

## <a name="what-is-an-array"></a>配列とは

**ハッシュテーブル** とは何かを説明する前に、まず [配列][]について説明する必要があります。 このディスカッションでは、配列は値またはオブジェクトのリストまたはコレクションです。

```powershell
$array = @(1,2,3,5,7,11)
```

項目を配列に格納したら、`foreach` を使用してリストを反復処理するか、インデックスを使用して配列内の個々の要素にアクセスすることができます。

```powershell
foreach($item in $array)
{
    Write-Output $item
}

Write-Output $array[3]
```

また、同じ方法でインデックスを使用して値を更新することもできます。

```powershell
$array[2] = 13
```

配列のほんの一部に触れただけですが、ハッシュテーブルに進むときに、配列を適切なコンテキストに組み込んでくれるはずです。

## <a name="what-is-a-hashtable"></a>ハッシュテーブルとは

まず、一般的な意味でハッシュテーブルとは何かについて基本的な技術説明を行った後、PowerShell でのその他の使用方法に移ります。

ハッシュテーブルは配列によく似たデータ構造ですが、キーを使用して各値 (オブジェクト) を格納する点が異なります。 これは基本的なキー/値ストアです。 まず、空のハッシュテーブルを作成します。

```powershell
$ageList = @{}
```

ハッシュテーブルを定義するには、かっこではなく中かっこを使用することに注意してください。 その後、次のようなキーを使用して項目を追加します。

```powershell
$key = 'Kevin'
$value = 36
$ageList.add( $key, $value )

$ageList.add( 'Alex', 9 )
```

人の名前がキーで、その年齢が保存する値です。

## <a name="using-the-brackets-for-access"></a>アクセスに角かっこを使用する

ハッシュテーブルに値を追加したら、同じキーを使用してその値を取り出すことができます (配列の場合のように数値インデックスを使用するのではなく)。

```powershell
$ageList['Kevin']
$ageList['Alex']
```

Kevin の年齢を取得したい場合は、彼の名前を使用してアクセスします。 このアプローチを使用して、ハッシュテーブルの値の追加または更新もできます。 これは、上記の `add()` 関数を使用する場合と似ています。

```powershell
$ageList = @{}

$key = 'Kevin'
$value = 36
$ageList[$key] = $value

$ageList['Alex'] = 9
```

値にアクセスして更新するために使用できるもう 1 つの構文があり、それについては後のセクションで説明します。 別の言語から PowerShell に移ってきたユーザーにとって、これらの例は、これまでハッシュテーブルを使用してきた方法に合致しているでしょう。

### <a name="creating-hashtables-with-values"></a>値を含むハッシュテーブルの作成

ここまでの例では、空のハッシュテーブルを作成しました。 作成時にキーと値をあらかじめ設定できます。

```powershell
$ageList = @{
    Kevin = 36
    Alex  = 9
}
```

### <a name="as-a-lookup-table"></a>ルックアップ テーブルとして

この種類のハッシュテーブルは、ルックアップ テーブルとして使用できることに真の価値があります。 単純な例を次に示します。

```powershell
$environments = @{
    Prod = 'SrvProd05'
    QA   = 'SrvQA02'
    Dev  = 'SrvDev12'
}

$server = $environments[$env]
```

この例では、`$env` 変数に環境を指定することで、適切なサーバーが選択されます。 このような選択には `switch($env){...}` を使用することもできますが、ハッシュテーブルは便利なオプションです。

これは、後で使用するためにルックアップ テーブルを動的に構築する場合に、さらに優れています。 したがって、何かを相互参照する必要がある場合は、このアプローチの使用を検討してください。 PowerShell での `Where-Object` によるパイプのフィルター処理がそれほど優れていなければ、これをさらに多く目にするだろうと思います。 パフォーマンスが重要になる状況では、このアプローチを検討する必要があります。

より高速であるとは言えませんが、[パフォーマンスが重要ならテストせよ][]という規則に適合します。

#### <a name="multiselection"></a>複数選択

一般に、ハッシュテーブルはキーと値のペアと考えることができます。そこでは、1 つのキーを指定し、1 つの値を取得します。 PowerShell では、複数の値を取得するためにキーの配列を指定できます。

```powershell
$environments[@('QA','DEV')]
$environments[('QA','DEV')]
$environments['QA','DEV']
```

この例では、上記と同じルックアップ ハッシュテーブルを使用し、3 つの異なる配列スタイルを指定して一致項目を取得します。 これは、ほとんどの方が気付いていない PowerShell の隠された宝石です。

## <a name="iterating-hashtables"></a>ハッシュテーブルの反復処理

ハッシュテーブルはキーと値のペアのコレクションであるため、配列または通常の項目リストとは異なる方法で反復処理します。

まず注目すべき点は、ハッシュテーブルをパイプ処理するとき、ハッシュテーブルは 1 つのオブジェクトのように扱われるということです。

```powershell
PS> $ageList | Measure-Object
count : 1
```

ただし、それに含まれる値の数は `.count` プロパティでわかります。

```powershell
PS> $ageList.count
2
```

値だけが必要な場合は、`.values` プロパティを使用してこの問題を回避します。

```powershell
PS> $ageList.values | Measure-Object -Average
Count   : 2
Average : 22.5
```

多くの場合、キーを列挙し、それらを使用して値にアクセスする方が便利です。

```powershell
PS> $ageList.keys | ForEach-Object{
    $message = '{0} is {1} years old!' -f $_, $ageList[$_]
    Write-Output $message
}
Kevin is 36 years old
Alex is 9 years old
```

`foreach(){...}` ループを使用した同じ例を次に示します。

```powershell
foreach($key in $ageList.keys)
{
    $message = '{0} is {1} years old' -f $key, $ageList[$key]
    Write-Output $message
}
```

ハッシュテーブルの各キーを順に調べ、それを使用して値にアクセスします。 これは、ハッシュテーブルをコレクションとして使用する場合の一般的なパターンです。

### <a name="getenumerator"></a>GetEnumerator()

ここで、ハッシュテーブルを反復処理するために `GetEnumerator()` を使用します。

```powershell
$ageList.GetEnumerator() | ForEach-Object{
    $message = '{0} is {1} years old!' -f $_.key, $_.value
    Write-Output $message
}
```

この列挙子は、各キーと値のペアを 1 つずつ渡してくれます。 このユース ケース専用に設計されています。 このことを思い出させてくれた [Mark Kraus](https://get-PowerShellblog.blogspot.com) さんに感謝いたします。

### <a name="badenumeration"></a>BadEnumeration

重要な詳細の 1 つは、列挙中はハッシュテーブルを変更できないことです。 基本的な `$environments` の例から始めます。

```powershell
$environments = @{
    Prod = 'SrvProd05'
    QA   = 'SrvQA02'
    Dev  = 'SrvDev12'
}
```

すべてのキーを同じサーバー値に設定しようとすると失敗します。

```powershell
$environments.Keys | ForEach-Object {
    $environments[$_] = 'SrvDev03'
}

An error occurred while enumerating through a collection: Collection was modified; enumeration operation may not execute.
+ CategoryInfo          : InvalidOperation: tableEnumerator:HashtableEnumerator) [], RuntimeException
+ FullyQualifiedErrorId : BadEnumeration
```

これも問題ないように見えますが、やはり失敗します。

```powershell
foreach($key in $environments.keys) {
    $environments[$key] = 'SrvDev03'
}

Collection was modified; enumeration operation may not execute.
    + CategoryInfo          : OperationStopped: (:) [], InvalidOperationException
    + FullyQualifiedErrorId : System.InvalidOperationException
```

この状況に対処する秘訣は、列挙を行う前にキーを複製することです。

```powershell
$environments.Keys.Clone() | ForEach-Object {
    $environments[$_] = 'SrvDev03'
}
```

## <a name="hashtable-as-a-collection-of-properties"></a>プロパティのコレクションとしてのハッシュテーブル

ここまでは、ハッシュテーブルに配置したオブジェクトの型はすべて同じ型のオブジェクトでした。 これらのすべての例で年齢を使用し、キーは人の名前でした。 これは、それぞれに名前が付いているオブジェクトのコレクションでは、それを確認するための優れた方法です。 PowerShell でハッシュテーブルを使用するもう 1 つの一般的な方法は、プロパティの名前をキーとして持つプロパティのコレクションを保持することです。 次の例では、このことについて説明します。

### <a name="property-based-access"></a>プロパティベースのアクセス

プロパティベースのアクセスを使用すると、ハッシュテーブルのダイナミクスと PowerShell での使用方法が変わります。 上記の例で、キーをプロパティとして扱う例を次に示します。

```powershell
$ageList = @{}
$ageList.Kevin = 35
$ageList.Alex = 9
```

上記の例と同じように、この例では、まだハッシュテーブルに存在しないキーは追加されます。 キーの定義方法と値の種類によって、これは少し奇妙なものか、最高の選択肢になります。 この時点までは、年齢リストの例はうまく機能しています。 納得して先へ進むには、このための新しい例が必要です。

```powershell
$person = @{
    name = 'Kevin'
    age  = 36
}
```

このように `$person` に属性を追加してアクセスすることもできます。

```powershell
$person.city = 'Austin'
$person.state = 'TX'
```

このハッシュテーブルは、突然、オブジェクトのように機能し始めます。 まだもののコレクションであるため、上記のすべての例が引き続き適用されます。 ここでは、別の観点からアプローチします。

### <a name="checking-for-keys-and-values"></a>キーと値の確認

ほとんどの場合、次のような方法で値をテストできます。

```powershell
if( $person.age ){...}
```

これは単純ですが、私はこのロジックで 1 つの重要な詳細を見落とししていたため、多くのバグの原因になっていました。 キーが存在するかどうかをテストするために使用しました。 値が `$false` または 0 の場合、そのステートメントは予期せず `$false` を返しました。

```powershell
if( $person.age -ne $null ){...}
```

これは、0 の値に対してはこの問題を回避しますが、$null と存在しないキーの比較では回避できません。 ほとんどの場合、この区別を行う必要はありませんが、区別が必要な場合にはそのための関数があります。

```powershell
if( $person.ContainsKey('age') ){...}
```

また、キーがわからない場合に値をテストする、またはコレクション全体を反復処理しながら値をテストする必要がある状況では、`ContainsValue()` を使用できます。

### <a name="removing-and-clearing-keys"></a>キーの削除とクリア

`.Remove()` 関数を使用してキーを削除できます。

```powershell
$person.remove('age')
```

`$null` 値を割り当てると、`$null` 値を持つキーが残ります。

ハッシュテーブルをクリアする一般的な方法は、空のハッシュテーブルに初期化することです。

```powershell
$person = @{}
```

それでも機能しますが、代わりに `clear()` 関数を使用してみてください。

```powershell
$person.clear()
```

これは、関数を使用することで自己文書化コードを実現し、コードの意図を明確にできる事例の 1 つです。

## <a name="all-the-fun-stuff"></a>すべての楽しいもの

### <a name="ordered-hashtables"></a>順序付きハッシュテーブル

既定では、ハッシュテーブルは順序付け (または並べ替え) されていません。 従来のコンテキストでは、常にキーを使用して値にアクセスする場合、順序は重要ではありません。 プロパティを定義した順序で維持する必要が生じる場合があります。 幸いにも、`ordered` キーワードを使用してこれを行う方法があります。

```powershell
$person = [ordered]@{
    name = 'Kevin'
    age  = 36
}
```

これで、キーと値を列挙すると、その順序が維持されるようになります。

### <a name="inline-hashtables"></a>インライン ハッシュテーブル

1 行にハッシュテーブルを定義する場合は、キーと値のペアをセミコロンで区切ることができます。

```powershell
$person = @{ name = 'kevin'; age = 36; }
```

これは、パイプで作成する場合に便利です。

### <a name="custom-expressions-in-common-pipeline-commands"></a>一般的なパイプライン コマンドのカスタム式

ハッシュテーブルを使用してカスタム プロパティや計算されるプロパティを作成するためのコマンドレットがいくつかあります。 これは一般的に `Select-Object` と `Format-Table` で使用されます。 ハッシュテーブルには特殊な構文があり、完全に展開すると次のようなものです。

```powershell
$property = @{
    name = 'totalSpaceGB'
    expression = { ($_.used + $_.free) / 1GB }
}
```

`name` は、コマンドレットがその列に付けるラベルです。 `expression` は実行されるスクリプト ブロックで、`$_` はパイプのオブジェクトの値です。 このスクリプトの動作は次のとおりです。

```powershell
$drives = Get-PSDrive | Where Used
$drives | Select-Object -Properties name, $property

Name     totalSpaceGB
----     ------------
C    238.472652435303
```

変数に配置しましたが、インラインで簡単に定義することもできます。また、`name` を `n` に短縮し、`expression` を `e` に短縮できます。

```powershell
$drives | Select-Object -properties name, @{n='totalSpaceGB';e={($_.used + $_.free) / 1GB}}
```

コマンドが長くなるうえ、説明は省きますが問題のある行動を助長させることも多いため、個人的には好きではありません。 私なら、スクリプトでこのアプローチを使用する代わりに、必要なすべてのフィールドとプロパティを含む新しいハッシュテーブルまたは `pscustomobject` を作成するでしょう。 しかし、これを行うコードはたくさんあるため、認識していただくことが目的でした。 `pscustomobject` の作成については後で説明します。

### <a name="custom-sort-expression"></a>カスタムの並べ替え式

並べ替えの対象となるデータがオブジェクトに含まれている場合は、コレクションを簡単に並べ替えることができます。 並べ替える前にオブジェクトにデータを追加するか、`Sort-Object` のカスタム式を作成することができます。

```powershell
Get-ADUser | Sort-Object -Parameter @{ e={ Get-TotalSales $_.Name } }
```

この例では、ユーザーの一覧を取得し、いくつかのカスタム コマンドレットを使用して、並べ替えのためだけに追加情報を取得しています。

#### <a name="sort-a-list-of-hashtables"></a>ハッシュテーブルの一覧の並べ替え

ハッシュテーブルの一覧を並べ替える場合、`Sort-Object` ではキーがプロパティとして扱われないことがわかります。 カスタムの並べ替え式を使用することで、これを回避できます。

```powershell
$data = @(
    @{name='a'}
    @{name='c'}
    @{name='e'}
    @{name='f'}
    @{name='d'}
    @{name='b'}
)

$data | Sort-Object -Property @{e={$_.name}}
```

## <a name="splatting-hashtables-at-cmdlets"></a>コマンドレットでのハッシュテーブルのスプラッティング

これはハッシュテーブルに関する私のお気に入りの 1 つで、早い段階で気付く人は少ないでしょう。
すべてのプロパティを 1 行でコマンドレットに渡すのではなく、まずハッシュテーブルにパッケージ化するというアイデアです。 その後、特別な方法でハッシュテーブルを関数に渡すことができます。
通常の方法で DHCP スコープを作成する例を次に示します。

```powershell
Add-DhcpServerv4Scope -Name 'TestNetwork' -StartRange'10.0.0.2' -EndRange '10.0.0.254' -SubnetMask '255.255.255.0' -Description 'Network for testlab A' -LeaseDuration (New-TimeSpan -Days 8) -Type "Both"
```

[スプラッティング][]を使用しない場合は、これらすべてを 1 行で定義する必要があります。 画面の外にはみ出るか、適当な場所で折り返されます。 ここで、スプラッティングを使用するコマンドと比較します。

```powershell
$DHCPScope = @{
    Name        = 'TestNetwork'
    StartRange  = '10.0.0.2'
    EndRange    = '10.0.0.254'
    SubnetMask  = '255.255.255.0'
    Description = 'Network for testlab A'
    LeaseDuration = (New-TimeSpan -Days 8)
    Type = "Both"
}
Add-DhcpServerv4Scope @DHCPScope
```

`$` ではなく `@` 記号を使用することで、スプラッティング操作が呼び出されます。

少し時間を取って、その例がいかに読みやすいかを確認してください。 これらはすべて同じ値を使用したまったく同じコマンドです。 2 つ目の方法は、理解しやすく、今後も維持しやすくなっています。

コマンドが長くなりすぎる場合、私は常にスプラッティングを使用します。 定義が長すぎると、ウィンドウが右にスクロールします。 関数に 3 つのプロパティを入力する場合は、スプラッティングされたハッシュテーブルを使用して書き直す方がよいでしょう。

### <a name="splatting-for-optional-parameters"></a>省略可能なパラメーターのスプラッティング

私がスプラッティングをよく使用する方法の 1 つは、スクリプト内の他の場所から取得された省略可能なパラメーターを処理することです。 たとえば、省略可能な `$Credential` 引数を持つ `Get-CIMInstance` 呼び出しをラップする関数があるとします。

```powershell
$CIMParams = @{
    ClassName = 'Win32_Bios'
    ComputerName = $ComputerName
}

if($Credential)
{
    $CIMParams.Credential = $Credential
}

Get-CIMInstance @CIMParams
```

まず、共通パラメーターを使用してハッシュテーブルを作成します。 `$Credential` が存在する場合は追加します。
ここではスプラッティングを使用しているので、コード内で `Get-CIMInstance` を 1 回呼び出すだけで済みます。 この設計パターンは非常にクリーンであり、多数の省略可能なパラメーターを簡単に処理できます。

実際には、パラメーターの `$null` 値を許可するようにコマンドを記述できるでしょう。 呼び出している他のコマンドを常に制御できるわけではありません。

### <a name="multiple-splats"></a>複数スプラッティング

複数のハッシュテーブルを同じコマンドレットにスプラッティングできます。 元のスプラッティングの例を見直すと、次のようになります。

```powershell
$Common = @{
    SubnetMask  = '255.255.255.0'
    LeaseDuration = (New-TimeSpan -Days 8)
    Type = "Both"
}

$DHCPScope = @{
    Name        = 'TestNetwork'
    StartRange  = '10.0.0.2'
    EndRange    = '10.0.0.254'
    Description = 'Network for testlab A'
}

Add-DhcpServerv4Scope @DHCPScope @Common
```

多くのコマンドに渡されるパラメーターの共通セットがある場合に、私はこの方法を使用します。

### <a name="splatting-for-clean-code"></a>クリーンなコードのためのスプラッティング

コードをすっきりさせることができるなら、1 つのパラメーターをスプラッティングしてもかまいません。

```powershell
$log = @{Path = '.\logfile.log'}
Add-Content "logging this command" @log
```

### <a name="splatting-executables"></a>実行可能ファイルのスプラッティング

スプラッティングは、`/param:value` 構文を使用する一部の実行可能ファイルでも機能します。 たとえば、`Robocopy.exe` には、次のようないくつかのパラメーターがあります。

```powershell
$robo = @{R=1;W=1;MT=8}
robocopy source destination @robo
```

これは役に立つかどうかはわかりませんが、興味深いと思いました。

## <a name="adding-hashtables"></a>ハッシュテーブルの追加

ハッシュテーブルでは、2 つのハッシュテーブルを結合する加算演算子がサポートされています。

```powershell
$person += @{Zip = '78701'}
```

これは、2 つのハッシュテーブルがキーを共有していない場合にのみ機能します。

## <a name="nested-hashtables"></a>入れ子になったハッシュテーブル

ハッシュテーブルは、ハッシュテーブル内の値として使用できます。

```powershell
$person = @{
    name = 'Kevin'
    age  = 36
}
$person.location = @{}
$person.location.city = 'Austin'
$person.location.state = 'TX'
```

ここでは、2 つのキーを含む基本的なハッシュテーブルから始めました。 空のハッシュテーブルを持つ `location` という名前のキーを追加しました。 次に、その `location` ハッシュテーブルに、最後の 2 つの項目を追加しました。 これをすべてインラインで行うこともできます。

```powershell
$person = @{
    name = 'Kevin'
    age  = 36
    location = @{
        city  = 'Austin'
        state = 'TX'
    }
}
```

これにより、上記と同じハッシュテーブルが作成され、同じ方法でプロパティにアクセスできます。

```powershell
$person.location.city
Austin
```

オブジェクトの構造にはさまざまな方法で対処できます。 入れ子になったハッシュテーブルを確認するもう 1 つの方法です。

```powershell
$people = @{
    Kevin = @{
        age  = 36
        city = 'Austin'
    }
    Alex = @{
        age  = 9
        city = 'Austin'
    }
}
```

ここには、ハッシュテーブルをオブジェクトのコレクションとして使用する概念と、プロパティのコレクションとして使用する概念が混在しています。 これらの値は、入れ子になっている場合でも、任意の方法で簡単にアクセスできます。

```powershell
PS> $people.kevin.age
36
PS> $people.kevin['city']
Austin
PS> $people['Alex'].age
9
PS> $people['Alex']['City']
Austin
```

プロパティのように扱うときは、私はドット プロパティを使用する傾向があります。 これらは一般に、コード内で静的に定義してあり、すぐに思い出せるものです。 一覧を調べたり、キーにプログラムでアクセスしたりする必要がある場合は、角かっこを使用してキー名を指定します。

```powershell
foreach($name in $people.keys)
{
    $person = $people[$name]
    '{0}, age {1}, is in {2}' -f $name, $person.age, $person.city
}
```

ハッシュテーブルを入れ子にできることにより、柔軟性と選択の幅が大きく向上します。

### <a name="looking-at-nested-hashtables"></a>入れ子になったハッシュテーブルの確認

ハッシュテーブルを入れ子にし始めるとすぐに、コンソールから簡単に確認する方法が必要になります。 たとえば最後のハッシュテーブルでは、次のような出力が得られます。

```powershell
PS> $people
Name                           Value
----                           -----
Kevin                          {age, city}
Alex                           {age, city}
```

私がこれらを確認するために使用する go to コマンドは `ConvertTo-JSON` です。これは非常にクリーンであり、私は他の処理で JSON を頻繁に使用するためです。

```powershell
PS> $people | ConvertTo-Json
{
    "Kevin":  {
                "age":  36,
                "city":  "Austin"
            },
    "Alex":  {
                "age":  9,
                "city":  "Austin"
            }
}
```

JSON について知らない場合でも、探しているものを確認できます。 このような構造化データ用には `Format-Custom` コマンドがありますが、私は JSON ビューの方が好きです。

## <a name="creating-objects"></a>オブジェクトの作成

オブジェクトが必要な場合に、ハッシュテーブルを使用してプロパティを保持するだけでは目的を達成できないことがあります。 キーを列名として表示したい場合がよくあります。 `pscustomobject` を使用すると、簡単に実現できます。

```powershell
$person = [pscustomobject]@{
    name = 'Kevin'
    age  = 36
}

$person

name  age
----  ---
Kevin  36
```

最初から `pscustomobject` として作成していない場合でも、必要に応じていつでもキャストできます。

```powershell
$person = @{
    name = 'Kevin'
    age  = 36
}

[pscustomobject]$person

name  age
----  ---
Kevin  36
```

[pscustomobject][] については詳細な記事を既に書いたので、この記事の後で読むことをお勧めします。 ここで学習したさまざまな要素に基づいて書かれています。

## <a name="reading-and-writing-hashtables-to-file"></a>ファイルに対するハッシュテーブルの読み取りと書き込み

### <a name="saving-to-csv"></a>CSV に保存する

ハッシュテーブルを CSV に保存するために苦労することは、上記で言及した問題の 1 つです。 ハッシュテーブルを `pscustomobject` に変換すると、CSV に正しく保存されます。 列の順序が保持されるように `pscustomobject` から始めておけば役立ちます。 ただし、必要な場合はインラインで `pscustomobject` にキャストできます。

```powershell
$person | ForEach-Object{ [pscustomobject]$_ } | Export-CSV -Path $path
```

これについても、[pscustomobject][] の使用に関する記事をご覧ください。

### <a name="saving-a-nested-hashtable-to-file"></a>入れ子になったハッシュテーブルをファイルに保存する

入れ子になったハッシュテーブルをファイルに保存し、再度読み取る必要がある場合は、JSON コマンドレットを使用します。

```powershell
$people | ConvertTo-JSON | Set-Content -Path $path
$people = Get-Content -Path $path -Raw | ConvertFrom-JSON
```

この方法には重要な点が 2 つあります。 1 点目は、JSON は複数行に書き出されるため、`-Raw` オプションを使用して 1 つの文字列に読み込み直す必要があるということです。 2 点目は、インポートされたオブジェクトは `[hashtable]` ではなくなるということです。 これは `[pscustomobject]` であり、予期していない場合は問題が発生する可能性があります。

深い入れ子になったハッシュテーブルを監視します。 JSON に変換した場合、期待どおりの結果が得られないことがあります。

```powershell
@{ a = @{ b = @{ c = @{ d = "e" }}}} | ConvertTo-Json

{
  "a": {
    "b": {
      "c": "System.Collections.Hashtable"
    }
  }
}
```

**Depth** パラメーターを使用して、入れ子になったすべてのハッシュテーブルが展開されていることを確認します。

```powershell
@{ a = @{ b = @{ c = @{ d = "e" }}}} | ConvertTo-Json -Depth 3

{
  "a": {
    "b": {
      "c": {
        "d": "e"
      }
    }
  }
}
```

インポート時に `[hashtable]` であることが必要な場合は、`Export-CliXml` および `Import-CliXml` コマンドを使用する必要があります。

### <a name="converting-json-to-hashtable"></a>JSON をハッシュテーブルに変換する

JSON を `[hashtable]` に変換する必要がある場合に、.NET で [JavaScriptSerializer][] を使用して実行する方法を 1 つ知っています。

```powershell
[Reflection.Assembly]::LoadWithPartialName("System.Web.Script.Serialization")
$JSSerializer = [System.Web.Script.Serialization.JavaScriptSerializer]::new()
$JSSerializer.Deserialize($json,'Hashtable')
```

PowerShell v6 以降では、JSON サポートに NewtonSoft JSON.NET が使用され、ハッシュテーブルのサポートが追加されます。

```powershell
'{ "a": "b" }' | ConvertFrom-Json -AsHashtable

Name      Value
----      -----
a         b
```

PowerShell 6.2 が `ConvertFrom-Json` に **Depth** パラメーターを追加しました。 既定の **Depth** は 1024 です。

### <a name="reading-directly-from-a-file"></a>ファイルから直接読み取る

PowerShell 構文を使用しているハッシュテーブルを含むファイルがある場合は、それを直接インポートする方法があります。

```powershell
$content = Get-Content -Path $Path -Raw -ErrorAction Stop
$scriptBlock = [scriptblock]::Create( $content )
$scriptBlock.CheckRestrictedLanguage( $allowedCommands, $allowedVariables, $true )
$hashtable = ( & $scriptBlock )
```

ファイルの内容を `scriptblock` にインポートした後、それを実行する前に、他の PowerShell コマンドが含まれていないことを確認します。

それに関連して、モジュール マニフェスト (psd1 ファイル) はハッシュテーブルであることを知っていましたか。

## <a name="keys-can-be-any-object"></a>キーには任意のオブジェクトを指定できます。

ほとんどの場合、キーは文字列にすぎません。 したがって、任意のものを引用符で囲んで、キーにすることができます。

```powershell
$person = @{
    'full name' = 'Kevin Marquette'
    '#' = 3978
}
$person['full name']
```

気付いていないかもしれませんが、いくつかの奇妙なことを行うことができます。

```powershell
$person.'full name'

$key = 'full name'
$person.$key
```

何かを行うことができるからといって、推奨されるわけではありません。 最後のものは、バグが発生するのを待っているようです。このコードを読む人によって簡単に誤解されるでしょう。

厳密に言うと、キーは文字列である必要はありませんが、文字列のみを使用する方が考えやすくなります。 ただし、インデックス作成は複雑なキーでは適切に機能しません。

```powershell
$ht = @{ @(1,2,3) = "a" }
$ht

Name                           Value
----                           -----
{1, 2, 3}                      a
```

キーによってハッシュテーブルの値にアクセスするのは、常に機能するわけではありません。 次に例を示します。

```powershell
$key = $ht.keys[0]
$ht.$($key)
a
$ht[$key]
a
```

キーが配列の場合は、メンバー アクセス (`.`) 表記で使用できるように、`$key` 変数を部分式でラップする必要があります。 または、配列インデックス (`[]`) 表記を使用することもできます。

## <a name="use-in-automatic-variables"></a>自動変数での使用

### <a name="psboundparameters"></a>$PSBoundParameters

[$PSBoundParameters][] は、関数のコンテキスト内にのみ存在する自動変数です。
これには、関数の呼び出し時に指定されたすべてのパラメーターが含まれます。 これは厳密にはハッシュテーブルではありませんがよく似ているため、そのように扱うことができます。

これには、キーの削除と他の関数へのスプラッティングが含まれます。 プロキシ関数を作成する場合は、これについて詳細を調べてください。

詳細については、[about_Automatic_Variables][]に関するページを参照してください。

### <a name="psboundparameters-gotcha"></a>PSBoundParameters の注意事項

覚えておくべき重要な点の 1 つは、これにはパラメーターとして渡された値のみが含まれることです。 既定値を持つパラメーターもある場合、それらが呼び出し元から渡されないときは、`$PSBoundParameters` にはこれらの値が含まれていません。 これはよく見落とされます。

### <a name="psdefaultparametervalues"></a>$PSDefaultParameterValues

この自動変数を使用すると、コマンドレットを変更せずに、任意のコマンドレットに既定値を割り当てることができます。
次の例を見てみましょう。

```powershell
$PSDefaultParameterValues["Out-File:Encoding"] = "UTF8"
```

これにより、`UTF8` を `Out-File -Encoding` パラメーターの既定値として設定するエントリが `$PSDefaultParameterValues` ハッシュテーブルに追加されます。 これはセッション固有であるため、自分の `$profile` 内に置く必要があります。

私は頻繁に入力する値を事前に割り当てるために、これをよく使用します。

```powershell
$PSDefaultParameterValues[ "Connect-VIServer:Server" ] = 'VCENTER01.contoso.local'
```

また、ワイルドカードも使用できるため、値を一括して設定できます。 次のような方法で使用できます。

```powershell
$PSDefaultParameterValues[ "Get-*:Verbose" ] = $true
$PSDefaultParameterValues[ "*:Credential" ] = Get-Credential
```

詳細については、[Michael Sorens][] 氏による[自動既定値][]に関するすばらしい記事を参照してください。

## <a name="regex-matches"></a>正規表現の $Matches

`-match` 演算子を使用すると、一致の結果を保持する `$matches` という名前の自動変数が作成されます。 正規表現に部分式がある場合は、それらの部分一致も一覧表示されます。

```powershell
$message = 'My SSN is 123-45-6789.'

$message -match 'My SSN is (.+)\.'
$Matches[0]
$Matches[1]
```

### <a name="named-matches"></a>名前付き一致

これは、ほとんどの人が知らないお気に入りの機能の 1 つです。 名前付き正規表現照合を使用すると、一致項目に名前でアクセスできます。

```powershell
$message = 'My Name is Kevin and my SSN is 123-45-6789.'

if($message -match 'My Name is (?<Name>.+) and my SSN is (?<SSN>.+)\.')
{
    $Matches.Name
    $Matches.SSN
}
```

上記の例では、`(?<Name>.*)` は名前付きの部分式です。 その後、この値は `$Matches.Name` プロパティに格納されます。

## <a name="group-object--ashashtable"></a>Group-Object -AsHashtable

`Group-Object` のあまり知られていない機能の 1 つとして、一部のデータセットをハッシュテーブルに変換できるという点があります。

```powershell
Import-CSV $Path | Group-Object -AsHashtable -Property email
```

これにより、各行がハッシュテーブルに追加され、指定されたプロパティがそれにアクセスするためのキーとして使用されます。

## <a name="copying-hashtables"></a>ハッシュテーブルのコピー

覚えておくべき重要な点の 1 つは、ハッシュテーブルがオブジェクトであることです。 各変数は、オブジェクトへの参照にすぎません。 つまり、ハッシュテーブルの有効なコピーを作成するには、より多くの作業が必要になります。

### <a name="assigning-reference-types"></a>参照型を割り当てる

ハッシュテーブルが 1 つある場合に、それを 2 つ目の変数に割り当てると、両方の変数が同じハッシュテーブルを指します。

```powershell
PS> $orig = @{name='orig'}
PS> $copy = $orig
PS> $copy.name = 'copy'
PS> 'Copy: [{0}]' -f $copy.name
PS> 'Orig: [{0}]' -f $orig.name

Copy: [copy]
Orig: [copy]
```

一方の値を変更するともう一方の値も変更されるため、これらは同じであることがよくわかります。 これは、ハッシュテーブルを他の関数に渡すときにも当てはまります。 これらの関数がハッシュテーブルに変更を加えた場合、元のハッシュテーブルも変更されます。

### <a name="shallow-copies-single-level"></a>シャロー コピー、単一レベル

上記の例のような単純なハッシュテーブルがある場合は、`.Clone()` を使用してシャロー コピーを作成できます。

```powershell
PS> $orig = @{name='orig'}
PS> $copy = $orig.Clone()
PS> $copy.name = 'copy'
PS> 'Copy: [{0}]' -f $copy.name
PS> 'Orig: [{0}]' -f $orig.name

Copy: [copy]
Orig: [orig]
```

これにより、他方に影響を与えずに、一方に対していくつかの基本的な変更を行うことができます。

### <a name="shallow-copies-nested"></a>シャロー コピー、入れ子

シャロー コピーと呼ばれる理由は、基本レベルのプロパティのみがコピーされるためです。 これらのプロパティのいずれかが参照型 (別のハッシュテーブルなど) である場合、入れ子になったオブジェクトは引き続き互いを指します。

```powershell
PS> $orig = @{
        person=@{
            name='orig'
        }
    }
PS> $copy = $orig.Clone()
PS> $copy.person.name = 'copy'
PS> 'Copy: [{0}]' -f $copy.person.name
PS> 'Orig: [{0}]' -f $orig.person.name

Copy: [copy]
Orig: [copy]
```

ハッシュテーブルを複製したにもかかわらず、`person` への参照は複製されていないことがわかります。 最初のハッシュテーブルにリンクされていない、本当の意味で 2 つ目のハッシュテーブルを作成するには、ディープ コピーを作成する必要があります。

### <a name="deep-copies"></a>ディープ コピー

ハッシュテーブルのディープ コピーを作成するには、いくつかの方法があります (ハッシュテーブルは保持されます)。 次に示す関数では、PowerShell を使用してディープ コピーが再帰的に作成されます。

```powershell
function Get-DeepClone
{
    [CmdletBinding()]
    param(
        $InputObject
    )
    process
    {
        if($InputObject -is [hashtable]) {
            $clone = @{}
            foreach($key in $InputObject.keys)
            {
                $clone[$key] = Get-DeepClone $InputObject[$key]
            }
            return $clone
        } else {
            return $InputObject
        }
    }
}
```

他の参照型または配列は処理されませんが、出発点として適しています。

もう 1 つは、.Net を使用して、この関数のように **CliXml** を使用して逆シリアル化する方法です。

```powershell
function Get-DeepClone
{
    param(
        $InputObject
    )
    $TempCliXmlString = [System.Management.Automation.PSSerializer]::Serialize($obj, [int32]::MaxValue)
    return [System.Management.Automation.PSSerializer]::Deserialize($TempCliXmlString)
}
```

非常に大きなハッシュテーブルの場合、逆シリアル化関数の方がスケールアウトされるため高速になります。ただし、この方法を使用する際、考慮すべき点がいくつかあります。 **Clixml** が使用されることで、メモリが集中的に使用されるため、大きなハッシュテーブルを複製する場合に問題になる可能性があります。 **Clixml** には、深さの制限が 48 という制限もあります。 つまり、ハッシュテーブルに 48 層のハッシュテーブルが入れ子にされている場合、複製は失敗し、ハッシュテーブルはまったく出力されません。

## <a name="anything-else"></a>その他

この記事では、多岐にわたって手早く説明しました。 この記事を読むたびに、新しいことを学んだり、理解を深めたりしていただければ幸いです。 この機能の全範囲について説明したので、すぐにはご自身に該当しない側面もあるでしょう。 それはまったく問題ではなく、どの程度 PowerShell を使用するかに応じて予想されることです。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2016-11-06-powershell-hashtable-everything-you-wanted-to-know-about/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[ハッシュテーブル]: /powershell/module/microsoft.powershell.core/about/about_hash_tables
[配列]: /powershell/module/microsoft.powershell.core/about/about_arrays
[パフォーマンスが重要ならテストせよ]: https://github.com/PoshCode/PowerShellPracticeAndStyle/blob/master/Best-Practices/Performance.md
[スプラッティング]: /powershell/module/microsoft.powershell.core/about/about_splatting
[pscustomobject]: everything-about-pscustomobject.md
[JavaScriptSerializer]: /dotnet/api/system.web.script.serialization.javascriptserializer?view=netframework-4.8&preserve-view=true
[PSBoundParameters]: https://tommymaynard.com/the-psboundparameters-automatic-variable-2016/
[about_Automatic_Variables]: /powershell/module/microsoft.powershell.core/about/about_automatic_variables
[自動既定値]: https://www.simple-talk.com/sysadmin/PowerShell/PowerShell-time-saver-automatic-defaults/
[Michael Sorens]: http://cleancode.sourceforge.net/wwwdoc/about.html
