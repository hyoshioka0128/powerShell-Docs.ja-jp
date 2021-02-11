---
description: 項目のコレクションを格納するように設計されたデータ構造体である配列について説明します。
Locale: en-US
ms.date: 08/26/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Arrays
ms.openlocfilehash: 2e7cf9c8f7d4e6f1d5bc66310f56d3de9461e592
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599317"
---
# <a name="about-arrays"></a>配列について

## <a name="short-description"></a>簡単な説明
項目のコレクションを格納するように設計されたデータ構造体である配列について説明します。

## <a name="long-description"></a>長い説明

配列は、項目のコレクションを格納するように設計されたデータ構造体です。
項目は、同じ型または異なる型にすることができます。

Windows PowerShell 3.0 以降では、0個または1個のオブジェクトのコレクションに配列のプロパティがいくつか含まれています。

## <a name="creating-and-initializing-an-array"></a>配列の作成と初期化

配列を作成して初期化するには、変数に複数の値を割り当てます。 配列に格納されている値は、コンマで区切られ、代入演算子 () によって変数名から区切られ `=` ます。

たとえば、 `$A` 7 つの数値 (int) 値22、5、10、8、12、9、および80を含むという名前の配列を作成するには、次のように入力します。

```powershell
$A = 22,5,10,8,12,9,80
```

コンマは、1つの項目の前にコンマを配置することで、1つの項目配列を初期化するためにも使用できます。

たとえば、単一の値を持つという名前の単一の item 配列を作成するには、次のように `$B` 入力します。

```powershell
$B = ,7
```

範囲演算子 () を使用して、配列を作成および初期化することもでき `..` ます。
次の例では、5 ~ 8 の値を含む配列を作成します。

```powershell
$C = 5..8
```

結果として、 `$C` 5、6、7、8の4つの値が含まれます。

データ型が指定されていない場合、PowerShell は各配列をオブジェクト配列 (**system.object []**) として作成します。 配列のデータ型を特定するには、 **GetType ()** メソッドを使用します。 たとえば、配列のデータ型を確認するには、次のように `$A` 入力します。

```powershell
$A.GetType()
```

厳密に型指定された配列 (特定の型の値のみを格納できる配列) を作成するには、 **string []**、 **long []**、 **int32 []** などの配列型として変数をキャストします。 配列をキャストするには、変数名の前に、角かっこで囲まれた配列型を使用します。 たとえば、 `$ia` 4 つの整数 (1500、2230、3350、および 4000) を含むという名前の32ビット整数配列を作成するには、次のように入力します。

```powershell
[int32[]]$ia = 1500,2230,3350,4000
```

そのため、配列には `$ia` 整数のみを含めることができます。

Microsoft .NET Framework では、サポートされている任意の型にキャストされる配列を作成できます。 たとえば、が `Get-Process` プロセスを表すためにを取得するオブジェクトの種類は **、に** なります。 厳密に型指定されたプロセスオブジェクトの配列を作成するには、次のコマンドを入力します。

```powershell
[Diagnostics.Process[]]$zz = Get-Process
```

## <a name="the-array-sub-expression-operator"></a>配列のサブ式演算子

配列のサブ式演算子は、その内部のステートメントから配列を作成します。 演算子内のステートメントによって生成される場合は、演算子によって配列に格納されます。 0個または1個のオブジェクトがある場合でも同様です。

配列演算子の構文は次のとおりです。

```syntax
@( ... )
```

配列演算子を使用すると、0個または1個のオブジェクトの配列を作成できます。 次に例を示します。

```powershell
$a = @("Hello World")
$a.Count
```

```Output
1
```

```powershell
$b = @()
$b.Count
```

```Output
0
```

配列演算子は、オブジェクトを取得するときにスクリプトで役に立ちますが、取得するオブジェクトの数を把握していません。 次に例を示します。

```powershell
$p = @(Get-Process Notepad)
```

配列のサブ式演算子の詳細については、「 [about_Operators](about_Operators.md)」を参照してください。

## <a name="accessing-and-using-array-elements"></a>配列要素へのアクセスと使用

### <a name="reading-an-array"></a>配列の読み取り

配列は、変数名を使用して参照できます。 配列内のすべての要素を表示するには、配列名を入力します。 たとえば、は、 `$a` 整数0、1、2、9までの整数を含む配列で、次のように入力します。

```powershell
$a
```

```Output
0
1
2
3
4
5
6
7
8
9
```

位置0から始まるインデックスを使用して、配列内の要素を参照できます。 インデックス番号を角かっこで囲みます。 たとえば、配列内の最初の要素を表示するには、次のように `$a` 入力します。

```powershell
$a[0]
```

```Output
0
```

配列の3番目の要素を表示するには `$a` 、次のように入力します。

```powershell
$a[2]
```

```Output
2
```

配列の一部を取得するには、インデックスの範囲演算子を使用します。 たとえば、配列の2番目から5番目の要素を取得するには、次のように入力します。

```powershell
$a[1..4]
```

```Output
1
2
3
4
```

配列の末尾からの負の数。 たとえば、"-1" は配列の最後の要素を参照します。 配列の最後の3つの要素を表示するには、インデックスの昇順で、次のように入力します。

```powershell
$a = 0 .. 9
$a[-3..-1]
```

```Output
7
8
9
```

負のインデックスを降順で入力すると、出力が変わります。

```powershell
$a = 0 .. 9
$a[-1..-3]
```

```Output
9
8
7
```

ただし、この表記法を使用する場合は注意が必要です。 記法は、終了境界から配列の先頭までの間に循環します。

```powershell
$a = 0 .. 9
$a[2..-2]
```

```Output
2
1
0
9
8
```

また、1つの一般的な誤りは、 `$a[0..-2]` 最後の要素を除き、配列のすべての要素を参照するということです。 これは、配列内の最初、最後、および最後から2番目の要素を参照します。

プラス演算子 () を使用し `+` て、範囲を配列内の要素のリストと結合することができます。 たとえば、インデックス位置0、2、および 4 ~ 6 の要素を表示するには、次のように入力します。

```powershell
$a = 0 .. 9
$a[0,2+4..6]
```

```Output
0
2
4
5
6
```

また、複数の範囲と個々の要素を一覧表示するには、プラス演算子を使用します。 たとえば、要素を0から2、4 ~ 6、要素を8番目の位置の型で一覧表示するには、次のようにします。

```powershell
$a = 0..9
$a[+0..2+4..6+8]
```

```Output
0
1
2
4
5
6
8
```

### <a name="iterations-over-array-elements"></a>配列要素の反復処理

ForEach、For、While ループなどのループ構造を使用して、配列内の要素を参照することもできます。 たとえば、ForEach ループを使用して配列内の要素を表示するには、次のように `$a` 入力します。

```powershell
$a = 0..9
foreach ($element in $a) {
  $element
}
```

```Output
0
1
2
3
4
5
6
7
8
9
```

Foreach ループは配列を反復処理し、配列の末尾に到達するまで配列内の各値を返します。

For ループは、配列内の要素の検査中にカウンターをインクリメントする場合に便利です。 たとえば、For ループを使用して配列内の他のすべての値を返すには、次のように入力します。

```powershell
$a = 0..9
for ($i = 0; $i -le ($a.length - 1); $i += 2) {
  $a[$i]
}
```

```Output
0
2
4
6
8
```

While ループを使用すると、定義された条件が満たされなくなるまで、配列内の要素を表示できます。 たとえば、配列インデックスが4未満のときに配列の要素を表示するには `$a` 、次のように入力します。

```powershell
$a = 0..9
$i=0
while($i -lt 4) {
  $a[$i];
  $i++
}
```

```Output
0
1
2
3
```

## <a name="properties-of-arrays"></a>配列のプロパティ

### <a name="count-or-length-or-longlength"></a>Count または Length または LongLength

配列内の項目の数を確認するには、 `Length` プロパティまたはそのエイリアスを使用し `Count` ます。 `Longlength` は、配列に2147483647個を超える要素が含まれている場合に便利です。

```powershell
$a = 0..9
$a.Count
$a.Length
```

```Output
10
10
```

### <a name="rank"></a>順位

配列内の次元数を返します。 PowerShell のほとんどの配列には、ディメンションが1つだけあります。 多次元配列を構築していると思われる場合でも、次の例のようになります。

```powershell
$a = @(
  @(0,1),
  @("b", "c"),
  @(Get-Process)
)

[int]$r = $a.Rank
"`$a rank: $r"
```

```Output
$a rank: 1
```

次の例は、.Net Framework を使用して真の多次元配列を作成する方法を示しています。

```powershell
[int[,]]$rank2 = [int[,]]::new(5,5)
$rank2.rank
```

```Output
2
```

## <a name="methods-of-arrays"></a>配列のメソッド

### <a name="clear"></a>クリア

すべての要素の値を、配列の要素型の _既定値_ に設定します。
Clear () メソッドでは、配列のサイズはリセットされません。

次の例で `$a` は、オブジェクトの配列です。

```powershell
$a = 1, 2, 3
$a.Clear()
$a | % { $null -eq $_ }
```

```Output
True
True
True
```

この例で `$intA` は、は整数を含むように明示的に型指定されています。

```powershell
[int[]] $intA = 1, 2, 3
$intA.Clear()
$intA
```

```Output
0
0
0
```

### <a name="foreach"></a>ForEach

が配列内のすべての要素を反復処理し、配列の各要素に対して特定の操作を実行できるようにします。

ForEach メソッドには、さまざまな操作を実行するいくつかのオーバーロードがあります。

```
ForEach(scriptblock expression)
ForEach(scriptblock expression, object[] arguments)
ForEach(type convertToType)
ForEach(string propertyName)
ForEach(string propertyName, object[] newValue)
ForEach(string methodName)
ForEach(string methodName, object[] arguments)
```

#### <a name="foreachscriptblock-expression"></a>ForEach (scriptblock 式)

#### <a name="foreachscriptblock-expression-object-arguments"></a>ForEach (scriptblock 式、object [] 引数)

このメソッドは、PowerShell v4 で追加されました。

> [!NOTE]
> 構文では、スクリプトブロックを使用する必要があります。 Scriptblock が唯一のパラメーターの場合、かっこは省略可能です。 また、メソッドと始めかっこまたは中かっこの間にスペースを配置することはできません。

次の例は、foreach メソッドの使用方法を示しています。 この場合、目的は配列内の要素の二乗値を生成することです。

```powershell
$a = @(0 .. 3)
$a.ForEach({ $_ * $_})
```

```Output
0
1
4
9
```

のパラメーターと同じように、パラメーターを使用すると、 `-ArgumentList` `ForEach-Object` `arguments` 引数の配列を、それを受け入れるように構成されたスクリプトブロックに渡すことができます。

**ArgumentList** の動作の詳細については、「 [about_Splatting](about_Splatting.md#splatting-with-arrays)」を参照してください。

#### <a name="foreachtype-converttotype"></a>ForEach (型 convertToType)

メソッドを使用すると、 `ForEach` 要素を別の型に迅速にキャストできます。次の例では、文字列の日付のリストを型に変換する方法を示します `[DateTime]` 。

```powershell
@("1/1/2017", "2/1/2017", "3/1/2017").ForEach([datetime])
```

```Output

Sunday, January 1, 2017 12:00:00 AM
Wednesday, February 1, 2017 12:00:00 AM
Wednesday, March 1, 2017 12:00:00 AM
```

#### <a name="foreachstring-propertyname"></a>ForEach (文字列 propertyName)

#### <a name="foreachstring-propertyname-object-newvalue"></a>ForEach (string propertyName, object [] newValue)

メソッドを使用して、 `ForEach` コレクション内のすべての項目のプロパティ値をすばやく取得したり、設定したりすることもできます。

```powershell
# Set all LastAccessTime properties of files to the current date.
(dir 'C:\Temp').ForEach('LastAccessTime', (Get-Date))
# View the newly set LastAccessTime of all items, and find Unique entries.
(dir 'C:\Temp').ForEach('LastAccessTime') | Get-Unique
```

```Output
Wednesday, June 20, 2018 9:21:57 AM
```

#### <a name="foreachstring-methodname"></a>ForEach (文字列 methodName)

#### <a name="foreachstring-methodname-object-arguments"></a>ForEach (文字列 methodName、object [] 引数)

最後に、メソッドを使用して、 `ForEach` コレクション内のすべての項目に対してメソッドを実行できます。

```powershell
("one", "two", "three").ForEach("ToUpper")
```

```Output
ONE
TWO
THREE
```

のパラメーターと同じように、パラメーターを使用すると、 `-ArgumentList` `ForEach-Object` `arguments` 引数の配列を、それを受け入れるように構成されたスクリプトブロックに渡すことができます。

> [!NOTE]
> Windows PowerShell 3.0 以降では、コレクション内の各項目のプロパティを取得し、メソッドを実行することもできます。詳細については、「スカラーオブジェクトとコレクションのメソッド」を参照してください [about_methods](about_methods.md)を参照してください。

### <a name="where"></a>Where

配列の要素をフィルター処理または選択できるようにします。 スクリプトは、ゼロ (0)、空の文字列、 `$false` または `$null` の後に表示する要素に対して、 `Where`

メソッドには1つの定義があり `Where` ます。

```
Where(scriptblock expression[, WhereOperatorSelectionMode mode
                            [, int numberToReturn]])
```

> [!NOTE]
> 構文では、スクリプトブロックを使用する必要があります。 Scriptblock が唯一のパラメーターの場合、かっこは省略可能です。 また、メソッドと始めかっこまたは中かっこの間にスペースを配置することはできません。

`Expression`は、フィルター処理に必要な scriptblock です `mode` 。オプションの引数を使用すると、追加の選択機能を使用でき `numberToReturn` ます。また、省略可能な引数を使用すると、フィルターから返される項目の数を制限することができます。

に使用できる値 `mode` は次のとおりです。

- Default (0)-すべての項目を返す
- 最初の (1)-最初の項目を返します。
- Last (2)-最後の項目を返します。
- 延期 (3)-条件が true になるまで項目をスキップし、残りの項目を返します
- Until (4)-条件が true になるまですべての項目を返す
- Split (5)-2 つの要素の配列を返します。
  - 最初の要素には一致する項目が含まれます
  - 2番目の要素には残りの項目が含まれます。

次の例は、配列からすべての奇数を選択する方法を示しています。

```powershell
(0..9).Where{ $_ % 2 }
```

```Output
1
3
5
7
9
```

この例では、空でない文字列を選択する方法を示します。

```powershell
('hi', '', 'there').Where({$_.Length})
```

```Output
hi
there
```

#### <a name="default"></a>Default

モードでは `Default` 、scriptblock を使用して項目をフィルター処理し `Expression` ます。

が指定されている場合は、 `numberToReturn` 返される項目の最大数を指定します。

```powershell
# Get the zip files in the current users profile, sorted by LastAccessTime.
$Zips = dir $env:userprofile -Recurse '*.zip' | Sort-Object LastAccessTime
# Get the least accessed file over 100MB
$Zips.Where({$_.Length -gt 100MB}, 'Default', 1)
```

> [!NOTE]
> モードとモードは、どちらも `Default` `First` 最初の ( `numberToReturn` ) 項目を返し、同義に使用することができます。

#### <a name="last"></a>Last (最後へ)

```powershell
$h = (Get-Date).AddHours(-1)
$logs = dir 'C:\' -Recurse '*.log' | Sort-Object CreationTime
# Find the last 5 log files created in the past hour.
$logs.Where({$_.CreationTime -gt $h}, 'Last', 5)
```

#### <a name="skipuntil"></a>延期

モードでは、 `SkipUntil` オブジェクトがスクリプトブロック式フィルターに合格するまで、コレクション内のすべてのオブジェクトがスキップされます。 その後、残りの **すべて** のコレクションアイテムをテストせずに返します。 _1 つの渡す項目のみがテストされ_ ます。

つまり、返されるコレクションには、テストされていない、 _渡し_ ている項目と _渡し_ ていない項目の両方が含まれます。

返される項目の数は、引数に値を渡すことによって制限できます `numberToReturn` 。

```powershell
$computers = "Server01", "Server02", "Server03", "localhost", "Server04"
# Find the first available online server.
$computers.Where({ Test-Connection $_ }, 'SkipUntil', 1)
```

```Output
localhost
```

#### <a name="until"></a>Until

モードは、モードを `Until` 反転し `SkipUntil` ます。  コレクション内の **すべて** の項目を返します。これは、項目がスクリプトブロック式に合格するまで続きます。 項目が scriptblock 式を _渡す_ と、 `Where` メソッドは項目の処理を停止します。

これは、メソッドから _渡されない_ 項目の最初のセットを受け取ることを意味し `Where` ます。 1つの項目が成功し _た後_、残りはテストも返されません。

返される項目の数は、引数に値を渡すことによって制限できます `numberToReturn` 。

```powershell
# Retrieve the first set of numbers less than or equal to 10.
(1..50).Where({$_ -gt 10}, 'Until')
# This would perform the same operation.
(1..50).Where({$_ -le 10})
```

```Output
1
2
3
4
5
6
7
8
9
10
```

> [!NOTE]
> とはどちらも、 `Until` `SkipUntil` 項目のバッチをテストしないという前提で動作します。
>
> `Until`最初の _パス_ の **前に** ある項目を返します。
>
> `SkipUntil`最初の _パス_ の **後** にあるすべての項目 (最初の引き渡し項目を含む) を返します。

#### <a name="split"></a>Split

この `Split` モードでは、コレクションアイテムが分割されるか、コレクションアイテムが2つの異なるコレクションにグループ化されます。 Scriptblock 式を渡すこれらの式と、それ以外の式を渡します。

が指定されている場合、 `numberToReturn` 最初のコレクションには、指定された値を超えるものではなく、 _渡す_ 項目が含まれます。

残りのオブジェクトは、式フィルターを **通過** したオブジェクトも、2番目のコレクションで返されます。

```powershell
$running, $stopped = (Get-Service).Where({$_.Status -eq 'Running'}, 'Split')
$running
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  Appinfo            Application Information
Running  AudioEndpointBu... Windows Audio Endpoint Builder
Running  Audiosrv           Windows Audio
...
```

```powershell
$stopped
```

```Output
Status   Name               DisplayName
------   ----               -----------
Stopped  AJRouter           AllJoyn Router Service
Stopped  ALG                Application Layer Gateway Service
Stopped  AppIDSvc           Application Identity
...
```

## <a name="get-the-members-of-an-array"></a>配列のメンバーを取得する

Length プロパティや **SetValue** メソッドなど、配列のプロパティとメソッドを取得するには、コマンドレットの **InputObject** パラメーターを使用し `Get-Member` ます。

パイプを使用して配列をに渡した場合 `Get-Member` 、PowerShell は一度に1つずつ項目を送信し、 `Get-Member` 配列内の各項目の型を返します (重複部分は無視されます)。

**InputObject** パラメーターを使用すると、は `Get-Member` 配列のメンバーを返します。

たとえば、次のコマンドは、配列変数のメンバーを取得し `$a` ます。

```powershell
Get-Member -InputObject $a
```

また、コマンドレットにパイプされる値の前にコンマ (,) を入力して、配列のメンバーを取得することもでき `Get-Member` ます。 コンマを使用すると、配列の配列の2番目の項目が配列になります。 PowerShell は、配列を一度に1つずつパイプし、 `Get-Member` 配列のメンバーを返します。 次の2つの例のようになります。

```powershell
,$a | Get-Member

,(1,2,3) | Get-Member
```

## <a name="manipulating-an-array"></a>配列の操作

配列内の要素を変更し、配列に要素を追加して、2つの配列の値を3番目の配列に結合できます。

配列内の特定の要素の値を変更するには、変更する要素の配列名とインデックスを指定し、代入演算子 () を使用して `=` 要素に新しい値を指定します。 たとえば、配列の2番目の項目 (インデックス位置 1) の値を10に変更するには、次のように `$a` 入力します。

```powershell
$a[1] = 10
```

配列の **SetValue** メソッドを使用して値を変更することもできます。 次の例では、配列の2番目の値 (インデックス位置 1) `$a` を500に変更します。

```powershell
$a.SetValue(500,1)
```

演算子を使用すると、 `+=` 配列に要素を追加できます。 次の例は、要素を配列に追加する方法を示して `$a` います。

```powershell
$a = @(0..4)
$a += 5
```

> [!NOTE]
> 演算子を使用すると `+=` 、PowerShell は実際には、元の配列の値と追加された値を使用して、新しい配列を作成します。 この場合、操作が複数回繰り返されるか、配列のサイズが大きすぎると、パフォーマンスの問題が発生する可能性があります。

配列から要素を削除するのは簡単ではありませんが、既存の配列の選択した要素のみを含む新しい配列を作成できます。 たとえば、 `$t` インデックス位置2の値を除いて、配列内のすべての要素を含む配列を作成するには、 `$a` 次のように入力します。

```powershell
$t = $a[0,1 + 3..($a.length - 1)]
```

2つの配列を1つの配列に結合するには、プラス演算子 () を使用し `+` ます。 次の例では、2つの配列を作成し、それらを結合して、結果として得られる配列を表示します。

```powershell
$x = 1,3
$y = 5,9
$z = $x + $y
```

結果として、 `$z` 配列には1、3、5、および9が含まれます。

配列を削除するには、配列に値を代入し `$null` ます。 次のコマンドは、変数内の配列を削除し `$a` ます。

`$a = $null`

コマンドレットを使用することもできますが、 `Remove-Item` `$null` 特に大きな配列の場合は、の値を割り当てる方が高速です。

## <a name="arrays-of-zero-or-one"></a>0個または1個の配列

Windows PowerShell 3.0 以降では、0個または1個のオブジェクトのコレクションに Count と Length プロパティがあります。 また、1つのオブジェクトの配列にインデックスを作成することもできます。 この機能は、コレクションを必要とするコマンドが2つよりも小さい場合に発生するスクリプトエラーを回避するのに役立ちます。

この機能の例を次に示します。

### <a name="zero-objects"></a>ゼロオブジェクト

```powershell
$a = $null
$a.Count
$a.Length
```

```Output
0
0
```

### <a name="one-object"></a>1つのオブジェクト

```powershell
$a = 4
$a.Count
$a.Length
$a[0]
$a[-1]
```

```Output
1
1
4
4
```

## <a name="indexing-support-for-systemtuple-objects"></a>システムの Tuple オブジェクトのインデックス作成のサポート

PowerShell 6.1 では、配列に似た **タプル** オブジェクトのインデックス付きアクセスのサポートが追加されました。
次に例を示します。

```powershell
PS> $tuple = [Tuple]::Create(1, 'test')
PS> $tuple[0]
1
PS> $tuple[1]
test
PS> $tuple[0..1]
1
test
PS> $tuple[-1]
test
```

配列やその他のコレクションオブジェクトとは異なり、 **組** オブジェクトは、パイプラインまたはオブジェクトの配列をサポートするパラメーターによって渡されるときに、単一のオブジェクトとして扱われます。

詳細については、「 [Tuple](/dotnet/api/system.tuple)」を参照してください。

## <a name="see-also"></a>関連項目

- [about_Assignment_Operators](about_Assignment_Operators.md)
- [about_Hash_Tables](about_Hash_Tables.md)
- [about_Operators](about_Operators.md)
- [about_For](about_For.md)
- [about_Foreach](about_Foreach.md)
- [about_While](about_While.md)

