---
title: PSCustomObject について知りたかったことのすべて
description: PSCustomObject は、構造化データを作成するためのシンプルな方法です。
ms.date: 10/05/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: 8086541bf93b4d3f878c9a3f7ca4300dc452a80b
ms.sourcegitcommit: 08c63095f54916013634d6703f2523779815f4b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2021
ms.locfileid: "99424471"
---
# <a name="everything-you-wanted-to-know-about-pscustomobject"></a>PSCustomObject について知りたかったことのすべて

`PSCustomObject` は、PowerShell のツール ベルトに追加できる優れたツールです。 基本事項から始めて、より高度な機能に進みましょう。 `PSCustomObject` を使用する背景にあるアイデアは、構造化データを簡単に作成することです。 最初の例を見てみると、その意味がよく理解できるでしょう。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="creating-a-pscustomobject"></a>PSCustomObject の作成

私は PowerShell で `[PSCustomObject]` を使うことが気に入っています。 これ以上簡単に有用なオブジェクトを作成する方法はありません。
そのため、オブジェクトを作成するその他の方法はすべてスキップします。ただし、これらの例のほとんどは PowerShell v3.0 以降であることに言及しておく必要があります。

```powershell
$myObject = [PSCustomObject]@{
    Name     = 'Kevin'
    Language = 'PowerShell'
    State    = 'Texas'
}
```

私はほとんどすべてのものにハッシュテーブルを使用するため、この方法はうまく機能します。 しかし、PowerShell でハッシュテーブルをよりオブジェクトに近い方法で処理したい場合もあります。 その違いに初めて気付くのは、`Format-Table` または `Export-CSV` を使用するときに、ハッシュテーブルがキーと値のペアのコレクションに過ぎないことを認識したときです。

その後、通常のオブジェクトの場合と同様に値にアクセスして使用することができます。

```powershell
$myObject.Name
```

### <a name="converting-a-hashtable"></a>ハッシュテーブルの変換

本筋から外れないうちに、次のようにすることもできたことにお気付きでしょうか。

```powershell
$myHashtable = @{
    Name     = 'Kevin'
    Language = 'PowerShell'
    State    = 'Texas'
}
$myObject = [pscustomobject]$myHashtable
```

私は初めからオブジェクトを作成する方が好きですが、最初にハッシュテーブルを操作することが必要になる場合もあります。 コンストラクターがオブジェクト プロパティに対してハッシュテーブルを受け取るため、この例は適切です。 重要な注意事項の 1 つは、この方法は機能しますが、まったく同等ではないことです。 最大の違いは、プロパティの順序が維持されないことです。

### <a name="legacy-approach"></a>従来の手法

カスタム オブジェクトを作成するために `New-Object` が使用されるのを見たことがあるかもしれません。

```powershell
$myHashtable = @{
    Name     = 'Kevin'
    Language = 'PowerShell'
    State    = 'Texas'
}

$myObject = New-Object -TypeName PSObject -Property $myHashtable
```

この方法はかなり遅くなりますが、初期バージョンの PowerShell では最良の選択肢である可能性があります。

### <a name="saving-to-a-file"></a>ファイルへの保存

ハッシュテーブルをファイルに保存する最善の方法は、JSON として保存することだと考えられます。 これをもう一度 `[PSCustomObject]` にインポートすることができます

```powershell
$myObject | ConvertTo-Json -depth 1 | Set-Content -Path $Path
$myObject = Get-Content -Path $Path | ConvertFrom-Json
```

[ファイルの読み取りと書き込みを行うさまざまな方法][]に関する記事では、オブジェクトをファイルに保存するその他の方法が説明されています。

## <a name="working-with-properties"></a>プロパティの操作

### <a name="adding-properties"></a>プロパティの追加

`Add-Member` を使用して `PSCustomObject` に新しいプロパティを追加することもできます。

```powershell
$myObject | Add-Member -MemberType NoteProperty -Name 'ID' -Value 'KevinMarquette'

$myObject.ID
```

### <a name="remove-properties"></a>プロパティの削除

オブジェクトからプロパティを削除することもできます。

```powershell
$myObject.psobject.properties.remove('ID')
```

`psobject` は、ベース オブジェクトのメタデータへのアクセスを提供する非表示のプロパティです。

### <a name="enumerating-property-names"></a>プロパティ名の列挙

オブジェクトのすべてのプロパティ名の一覧が必要になることがあります。

```powershell
$myObject | Get-Member -MemberType NoteProperty | Select -ExpandProperty Name
```

この同じ一覧を `psobject` プロパティからも取得できます。

```powershell
$myobject.psobject.properties.name
```

### <a name="dynamically-accessing-properties"></a>プロパティへの動的なアクセス

プロパティ値に直接アクセスできることは既に説明しました。

```powershell
$myObject.Name
```

プロパティ名に文字列を使用しても、やはり機能します。

```powershell
$myObject.'Name'
```

これをもう一歩進めて、プロパティ名に変数を使用することができます。

```powershell
$property = 'Name'
$myObject.$property
```

これは奇妙に見えますが、うまく機能します。

### <a name="convert-pscustomobject-into-a-hashtable"></a>PSCustomObject をハッシュテーブルに変換する

最後のセクションから作業を続行する場合、プロパティを動的にウォークして、そこからハッシュテーブルを作成することができます。

```powershell
$hashtable = @{}
foreach( $property in $myobject.psobject.properties.name )
{
    $hashtable[$property] = $myObject.$property
}
```

### <a name="testing-for-properties"></a>プロパティのテスト

プロパティが存在するかどうかを知る必要がある場合は、そのプロパティに値があるかどうかを確認するだけで済みます。

```powershell
if( $null -ne $myObject.ID )
```

ただし、値が `$null` である可能性がある場合は、`psobject.properties` をチェックすることで、これが存在するかどうかを確認できます。

```powershell
if( $myobject.psobject.properties.match('ID').Count )
```

## <a name="adding-object-methods"></a>オブジェクト メソッドの追加

スクリプト メソッドをオブジェクトに追加する必要がある場合は、`Add-Member` と `ScriptBlock` を使用して実行できます。 現在のオブジェクトを参照するには、`this` 自動変数を使用する必要があります。 オブジェクトをハッシュテーブルに変換する `scriptblock` を次に示します。 (最後の例と同じコード形式)

```powershell
$ScriptBlock = {
    $hashtable = @{}
    foreach( $property in $this.psobject.properties.name )
    {
        $hashtable[$property] = $this.$property
    }
    return $hashtable
}
```

次に、それをスクリプト プロパティとしてオブジェクトに追加します。

```powershell
$memberParam = @{
    MemberType = "ScriptMethod"
    InputObject = $myobject
    Name = "ToHashtable"
    Value = $scriptBlock
}
Add-Member @memberParam
```

その後、次のように関数を呼び出すことができます。

```powershell
$myObject.ToHashtable()
```

### <a name="objects-vs-value-types"></a>オブジェクトと値の型

オブジェクトと値の型では、変数の割り当ての処理方法が異なります。 値の型を相互に割り当てた場合、値だけが新しい変数にコピーされます。

```powershell
$first = 1
$second = $first
$second = 2
```

この場合、`$first` は 1、`$second` は 2 です。

オブジェクト変数では、実際のオブジェクトへの参照が保持されます。 新しい変数に 1 つのオブジェクトを割り当てても、それらは引き続き同じオブジェクトを参照します。

```powershell
$third = [PSCustomObject]@{Key=3}
$fourth = $third
$fourth.Key = 4
```

`$third` と `$fourth` はオブジェクトの同じインスタンスを参照するため、`$third.key` と `$fourth.Key` の両方が 4 になります。

### <a name="psobjectcopy"></a>psobject.copy()

オブジェクトを本当にコピーする必要がある場合は、複製することができます。

```powershell
$third = [PSCustomObject]@{Key=3}
$fourth = $third.psobject.copy()
$fourth.Key = 4
```

複製では、そのオブジェクトのシャロー コピーが作成されます。 この例では、それぞれが異なるインスタンスを持つようになり、`$third.key` が 3、`$fourth.Key` が 4 になります。

これをシャロー コピーと呼んだのは、入れ子になったオブジェクトを使用する場合があるためです。 (プロパティに他のオブジェクトが含まれている場合)。 最上位レベルの値のみがコピーされます。 子オブジェクトはお互いを参照します。

### <a name="pstypename-for-custom-object-types"></a>カスタム オブジェクトの型に対する PSTypeName

これでオブジェクトを作成できたので、これを使用して、まったく明白ではないかもしれないその他の操作をいくつか実行できます。 まず、これに `PSTypeName` を指定する必要があります。 最もよく使用されている一般的な方法を次に示します。

```powershell
$myObject.PSObject.TypeNames.Insert(0,"My.Object")
```

最近、こちらの [/u/markekraus による投稿][]から、これを行う別の方法を発見しました。 私はもう少し詳しく調べて、[Adam Bertram][] と [Mike Shepard][] によるこのアイデアについてのさらなる投稿を見つけました。そこでは、これをインラインで定義できるようにするアプローチが説明されています。

```powershell
$myObject = [PSCustomObject]@{
    PSTypeName = 'My.Object'
    Name       = 'Kevin'
    Language   = 'PowerShell'
    State      = 'Texas'
}
```

これはこの言語にとてもよく合っています。 これで、適切な型名を持つオブジェクトを作成できたので、さらにいくつかの操作を行うことができます。

> [!NOTE]
> PowerShell クラスを使用して、カスタム PowerShell 型を作成することもできます。 詳細については、[PowerShell クラスの概要](/powershell/module/Microsoft.PowerShell.Core/About/about_Classes)に関する記事を参照してください。

## <a name="using-defaultpropertyset-the-long-way"></a>DefaultPropertySet の使用 (長い道のり)

PowerShell では、既定で表示するプロパティが自動的に決定されます。 多くのネイティブ コマンドには、すべての複雑な処理を行う `.ps1xml` [書式設定ファイル][]があります。 こちらの [Boe Prox による投稿][]から、PowerShell のみを使用してカスタム オブジェクトに対してこれを行う別の方法があります。 これに使用する `MemberSet` を指定できます。

```powershell
$defaultDisplaySet = 'Name','Language'
$defaultDisplayPropertySet = New-Object System.Management.Automation.PSPropertySet('DefaultDisplayPropertySet',[string[]]$defaultDisplaySet)
$PSStandardMembers = [System.Management.Automation.PSMemberInfo[]]@($defaultDisplayPropertySet)
$MyObject | Add-Member MemberSet PSStandardMembers $PSStandardMembers
```

これで、オブジェクトがシェルに表示されると、既定ではこれらのプロパティのみが表示されます。

### <a name="update-typedata-with-defaultpropertyset"></a>Update-TypeData と DefaultPropertySet

これは便利ですが、最近、[Jeffrey Snover と Don Jones による PowerShell アンプラグド 2016][psunplugged] を見ていたときに、より良い方法を知りました。 Jeffrey は [Update-TypeData][] を使用して、既定のプロパティを指定していました。

```powershell
$TypeData = @{
    TypeName = 'My.Object'
    DefaultDisplayPropertySet = 'Name','Language'
}
Update-TypeData @TypeData
```

これは簡単なので、もし私がこの投稿をクイック リファレンスとして使えなくても、内容をほとんど覚えていたでしょう。 これで、多くのプロパティを持つオブジェクトを簡単に作成し、さらにシェルから見たときにわかりやすい表示にすることができるようになりました。 その他のプロパティにアクセスしたり表示したりする必要がある場合も、それらはまだ存在しています。

```powershell
$myObject | Format-List *
```

### <a name="update-typedata-with-scriptproperty"></a>Update-TypeData と ScriptProperty

このビデオから得たその他の情報は、オブジェクトのスクリプト プロパティを作成することです。 これは既存のオブジェクトに対しても同様に機能することを、ここで指摘しておきましょう。

```powershell
$TypeData = @{
    TypeName = 'My.Object'
    MemberType = 'ScriptProperty'
    MemberName = 'UpperCaseName'
    Value = {$this.Name.toUpper()}
}
Update-TypeData @TypeData
```

これは、オブジェクトが作成される前に実行しても後に実行しても機能します。 これが、スクリプト プロパティで `Add-Member` を使用する場合との相違点です。 以前に言及した方法で `Add-Member` を使用した場合、これはオブジェクトのその特定のインスタンス上にのみ存在します。 これは、この `TypeName` を持つすべてのオブジェクトに適用されます。

## <a name="function-parameters"></a>関数のパラメーター

これで、これらのパラメーターのカスタム型を関数やスクリプト内で使用できるようになりました。 1 つの関数でこれらのカスタム オブジェクトを作成し、他の関数に渡すことができます。

```powershell
param( [PSTypeName('My.Object')]$Data )
```

PowerShell では、オブジェクトが指定した型である必要があります。 型が自動的に一致しない場合は検証エラーがスローされるため、自分のコードでそれをテストする手間が省けます。 これは PowerShell に得意な処理を任せる場合の優れた例です。

### <a name="function-outputtype"></a>関数の OutputType

また、高度な関数の `OutputType` を定義することもできます。

```powershell
function Get-MyObject
{
    [OutputType('My.Object')]
    [CmdletBinding()]
        param
        (
            ...
```

**OutputType** 属性値は、ドキュメントのメモにすぎません。 これを関数のコードから派生させたり、実際の関数の出力と比較したりすることはできません。

出力型を使用する主な理由は、関数に関するメタ情報に自分の意図を反映させることです。 開発環境で利用できる `Get-Command` や `Get-Help` のようなものです。 詳細については、そのヘルプを参照してください: [about_Functions_OutputTypeAttribute][]。

ただし、関数の単体テストを行うために Pester を使用している場合は、出力オブジェクトが **OutputType** と一致することを検証することをお勧めします。 これにより、不適切な場合にパイプに送られた変数をキャッチできます。

## <a name="closing-thoughts"></a>最後に

このコンテキストはすべて `[PSCustomObject]` に関するものでしたが、この情報の大部分はオブジェクト全般に適用されます。

これらの機能のほとんどは以前にどこかで見たことがありましたが、`PSCustomObject` に関する情報のコレクションとして提示されているのは見たことがありませんでした。 つい先週、私はもう一つを偶然見つけて、それを前に見ていなかったことに驚きました。 私は、これらすべてのアイデアをまとめて、皆様が全体像を確認し、使用する機会があればそれらを認識できるようにしたいと思いました。 皆様が何かを学び、ご自分のスクリプトで使用する方法を見つけられれば幸いです。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2016-10-28-powershell-everything-you-wanted-to-know-about-pscustomobject/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[Boe Prox による投稿]: https://learn-PowerShell.net/2013/08/03/quick-hits-set-the-default-property-display-in-PowerShell-on-custom-objects/
[書式設定ファイル]: https://mcpmag.com/articles/2014/05/13/PowerShell-properties-part-3.aspx
[about_Functions_OutputTypeAttribute]: /powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute
[ファイルの読み取りと書き込みを行うさまざまな方法]: https://powershellexplained.com/2017-03-18-Powershell-reading-and-saving-data-to-files
[/u/markekraus による投稿]: https://www.reddit.com/r/PowerShell/comments/590awc/is_it_possible_to_initialize_a_pscustoobject_with/
[Adam Bertram]: http://www.adamtheautomator.com/building-custom-object-types-PowerShell-pstypename/
[Mike Shepard]: https://powershellstation.com/2016/05/22/custom-objects-and-pstypename/
[psunplugged]: https://www.youtube.com/watch?v=Ab46gHXNm8Q
[Update-TypeData]: /powershell/module/microsoft.powershell.utility/update-typedata
