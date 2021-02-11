---
description: PowerShell の値を比較する演算子について説明します。
Locale: en-US
ms.date: 01/20/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Comparison_Operators
ms.openlocfilehash: 2bd2aa825d09078f37dba1f99fa64584dacd324d
ms.sourcegitcommit: 94d597c4fb38793bc49ca7610e2c9973b1e577c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "99599725"
---
# <a name="about-comparison-operators"></a>比較演算子について

## <a name="short-description"></a>簡単な説明

PowerShell の比較演算子は、2つの値を比較するか、コレクションの要素を入力値に対してフィルター処理することができます。

## <a name="long-description"></a>長い説明

比較演算子を使用すると、値を比較したり、指定したパターンに一致する値を検索したりできます。 PowerShell には、次の比較演算子が含まれています。

|    型     |   演算子   |              比較テスト              |
| ----------- | ------------ | ----------------------------------------- |
| 等価比較    | -eq          | equals                                    |
|             | -ne          | 等しくない                                |
|             | -gt          | より大きい                              |
|             | -ge          | 以上                     |
|             | -lt          | 次の値未満                                 |
|             | -le          | 以下                        |
| Matching    | -like        | ワイルドカードパターンに一致する文字列           |
|             | -notlike     | 文字列がワイルドカードパターンと一致しません    |
|             | -match       | 文字列が regex パターンと一致します              |
|             | -notmatch    | 文字列が regex パターンと一致しません       |
| Replacement | -replace     | 正規表現パターンに一致する文字列を置換します |
| Containment | -contains    | コレクションに値が含まれています               |
|             | -notcontains | コレクションに値が含まれていません       |
|             | -in          | 値がコレクション内にあります                  |
|             | -notin       | 値がコレクション内にありません              |
| 型        | -が          | 両方のオブジェクトが同じ型です。            |
|             | -isnot       | オブジェクトが同じ型ではありません         |

## <a name="common-features"></a>共通機能

既定では、すべての比較演算子で大文字と小文字が区別されます。 大文字と小文字を区別する比較演算子を作成するには、の後にを追加し `c` `-` ます。 たとえば、 `-ceq` は、の大文字と小文字を区別するバージョンです `-eq` 。 大文字と小文字を区別しないようにするには、の前にを追加し `i` `-` ます。 たとえば、 `-ieq` は、の大文字と小文字を区別しない明示的なバージョンです `-eq` 。

演算子の入力がスカラー値の場合、演算子は **ブール** 値を返します。 入力がコレクションの場合、演算子は、式の右辺の値に一致するコレクションの要素を返します。
コレクション内に一致するものがない場合、比較演算子は空の配列を返します。 次に例を示します。

```powershell
$a = (1, 2 -eq 3)
$a.GetType().Name
$a.Count
```

```output
Object[]
0
```

次のようにいくつかの例外があります。

- 含有演算子と型演算子は、常に **ブール** 値を返します。
- 演算子は、 `-replace` 置換結果を返します。
- `-match`And `-notmatch` 演算子も `$Matches` 自動変数を設定します。

## <a name="equality-operators"></a>等値演算子

### <a name="-eq-and--ne"></a>-eq と -ne

左辺がスカラーの場合、 `-eq` 右辺が完全に一致する場合は **True** を返し、それ以外の場合は False を返し `-eq` ます。  `-ne` は逆になります。両辺が一致する場合は **False** を返します。それ以外の場合は `-ne` True を返します。

例:

```powershell
2 -eq 2                 # Output: True
2 -eq 3                 # Output: False
"abc" -eq "abc"         # Output: True
"abc" -eq "abc", "def"  # Output: False
"abc" -ne "def"         # Output: True
"abc" -ne "abc"         # Output: False
"abc" -ne "abc", "def"  # Output: True
```

左側がコレクションの場合は、 `-eq` 右側に一致するメンバーを返し、 `-ne` フィルターで除外します。

例:

```powershell
1,2,3 -eq 2             # Output: 2
"abc", "def" -eq "abc"  # Output: abc
"abc", "def" -ne "abc"  # Output: def
```

これらの演算子は、コレクションのすべての要素を処理します。 例:

```powershell
"zzz", "def", "zzz" -eq "zzz"
```

```output
zzz
zzz
```

等値演算子は、スカラーまたはコレクションだけでなく、任意の2つのオブジェクトを受け入れます。
ただし、比較結果はエンドユーザーにとって意味があるとは限りません。
次の例は、この問題を示しています。

```powershell
class MyFileInfoSet {
    [String]$File
    [Int64]$Size
}
$a = [MyFileInfoSet]@{File = "C:\Windows\explorer.exe"; Size = 4651032}
$b = [MyFileInfoSet]@{File = "C:\Windows\explorer.exe"; Size = 4651032}
$a -eq $b
```

```Output
False
```

この例では、同一のプロパティを持つ2つのオブジェクトを作成しました。 ただし、等価テストの結果は、異なるオブジェクトであるため **False** になります。 同等のクラスを作成するには、クラスに[IEquatable \<T> ][2]を実装する必要があります。 次の例は、 [IEquatable \<T>][2]を実装し、**ファイル** と **サイズ** の2つのプロパティを持つ、 **myfileinfoset** クラスの部分実装を示しています。 `Equals()`2 つの **Myfileinfoset** オブジェクトの File プロパティと Size プロパティが同じである場合、メソッドは True を返します。

```powershell
class MyFileInfoSet : System.IEquatable[Object] {
    [String]$File
    [Int64]$Size

    [bool] Equals([Object] $obj) {
        return ($this.File -eq $obj.File) -and ($this.Size -eq $obj.Size)
    }
}
$a = [MyFileInfoSet]@{File = "C:\Windows\explorer.exe"; Size = 4651032}
$b = [MyFileInfoSet]@{File = "C:\Windows\explorer.exe"; Size = 4651032}
$a -eq $b
```

```Output
True
```

任意のオブジェクトを比較するための例として、null かどうかを確認することがあります。 ただし、変数がであるかどうかを判断する必要がある場合は `$null` 、 `$null` 等値演算子の左側に配置する必要があります。 これを右側に配置しても、期待どおりの処理は行われません。

たとえば、次の `$a` ように null 要素を含む配列を使用します。

```powershell
$a = 1, 2, $null, 4, $null, 6
```

次のテストは `$a` null ではありません。

```powershell
$null -ne $a
```

```output
False
```

ただし、次の例では、からすべての null 要素を除外してい `$a` ます。

```powershell
$a -ne $null # Output: 1, 2, 4, 6
```

```output
1
2
4
6
```

### <a name="-gt--ge--lt-and--le"></a>-gt、-ge、-lt、および-le

`-gt`、 `-ge` 、 `-lt` 、およびは `-le` 同様に動作します。 両方の側がスカラーの場合は、2つの辺の比較に応じて、 **True** または **False** を返します。

| 演算子 | True を返します。                   |
| -------- | -------------------------------------- |
| -gt      | 左側が大きくなっています          |
| -ge      | 左辺が大きいか等しいです。 |
| -lt      | 左側が小さくなっています          |
| -le      | 左側が小さいか等しい |

次の例では、すべてのステートメントが True を返します。

```powershell
8 -gt 6  # Output: True
8 -ge 8  # Output: True
6 -lt 8  # Output: True
8 -le 8  # Output: True
```

> [!NOTE]
> ほとんどのプログラミング言語では、大なり演算子は `>` です。 PowerShell では、この文字がリダイレクトに使用されます。 詳細については、「 [about_Redirection][3]」を参照してください。

左側がコレクションの場合、これらの演算子は、コレクションの各メンバーを右側と比較します。 ロジックに応じて、メンバーを保持するか破棄します。

例:

```powershell
$a=5, 6, 7, 8, 9

Write-Output "Test collection:"
$a

Write-Output "`nMembers greater than 7"
$a -gt 7

Write-Output "`nMembers greater than or equal to 7"
$a -ge 7

Write-Output "`nMembers smaller than 7"
$a -lt 7

Write-Output "`nMembers smaller than or equal to 7"
$a -le 7
```

```output
Test collection:
5
6
7
8
9

Members greater than 7
8
9

Members greater than or equal to 7
7
8
9

Members smaller than 7
5
6

Members smaller than or equal to 7
5
6
7
```

これらの演算子は、 [system.icomparable][1]を実装するすべてのクラスで機能します。

次に例を示します。

```powershell
# Date comparison
[DateTime]'2001-11-12' -lt [DateTime]'2020-08-01' # True

# Sorting order comparison
'a' -lt 'z'           # True; 'a' comes before 'z'
'macOS' -ilt 'MacOS'  # False
'MacOS' -ilt 'macOS'  # False
'macOS' -clt 'MacOS'  # True; 'm' comes before 'M'
```

次の例は、"a" の後に並べ替えられる、アメリカ QWERTY キーボードにシンボルがないことを示しています。 このようなすべての記号を含むセットを演算子にフィードして、 `-gt` ' a ' と比較します。 出力は空の配列です。

```powershell
$a=' ','`','~','!','@','#','$','%','^','&','*','(',')','_','+','-','=',
   '{','}','[',']',':',';','"','''','\','|','/','?','.','>',',','<'
$a -gt 'a'
# Output: Nothing
```

演算子の両側が適度に比較できない場合、これらの演算子は終了しないエラーを発生させます。

## <a name="matching-operators"></a>照合演算子

一致する演算子 ( `-like` 、 `-notlike` 、 `-match` 、および `-notmatch` ) は、指定したパターンに一致するか一致しない要素を検索します。 とのパターン `-like` `-notlike` は、ワイルドカード式 (、、およびを含む `*` ) であり、とは `?` `[ ]` `-match` `-notmatch` 正規表現 (Regex) を受け入れます。

の構文は次のとおりです。

```
<string[]> -like    <wildcard-expression>
<string[]> -notlike <wildcard-expression>
<string[]> -match    <regular-expression>
<string[]> -notmatch <regular-expression>
```

これらの演算子の入力がスカラー値の場合は、 **ブール** 値が返されます。 入力が値のコレクションである場合、演算子は一致するメンバーを返します。 コレクション内に一致するものがない場合、これらの演算子は空の配列を返します。

### <a name="-like-and--notlike"></a>-like および-notlike

`-like` およびは `-notlike` と同様に動作し `-eq` ますが、右側には `-ne` [ワイルドカード](about_Wildcards.md)を含む文字列を指定できます。

例:

```powershell
"PowerShell" -like    "*shell"           # Output: True
"PowerShell" -notlike "*shell"           # Output: False
"PowerShell" -like    "Power?hell"       # Output: True
"PowerShell" -notlike "Power?hell"       # Output: False
"PowerShell" -like    "Power[p-w]hell"   # Output: True
"PowerShell" -notlike "Power[p-w]hell"   # Output: False

"PowerShell", "Server" -like "*shell"    # Output: PowerShell
"PowerShell", "Server" -notlike "*shell" # Output: Server
```

### <a name="-match-and--notmatch"></a>-match と-notmatch

`-match` と `-notmatch` は、正規表現を使用して、左側の値のパターンを検索します。 正規表現は、電子メールアドレス、UNC パス、書式設定された電話番号などの複雑なパターンに一致する場合があります。 右側の文字列は [正規表現](about_Regular_Expressions.md) の規則に従う必要があります。

スカラーの例:

```powershell
# Partial match test, showing how differently -match and -like behave
"PowerShell" -match 'shell'        # Output: True
"PowerShell" -like  'shell'        # Output: False

# Regex syntax test
"PowerShell" -match    '^Power\w+' # Output: True
'bag'        -notmatch 'b[iou]g'   # Output: True
```

入力がコレクションの場合、演算子はそのコレクションの一致するメンバーを返します。

コレクションの例:

```powershell
"PowerShell", "Super PowerShell", "Power's hell" -match '^Power\w+'
# Output: PowerShell

"Rhell", "Chell", "Mel", "Smell", "Shell" -match "hell"
# Output: Rhell, Chell, Shell

"Bag", "Beg", "Big", "Bog", "Bug"  -match 'b[iou]g'
#Output: Big, Bog, Bug

"Bag", "Beg", "Big", "Bog", "Bug"  -notmatch 'b[iou]g'
#Output: Bag, Beg
```

`-match` およびでは、 `-notmatch` regex キャプチャグループがサポートされています。 実行するたびに、自動変数が上書きさ `$Matches` れます。 `<input>`がコレクションの場合、 `$Matches` 変数は `$null` です。 `$Matches` は、常に ' 0 ' という名前のキーを持つ **ハッシュテーブル** であり、一致した文字列全体が格納されます。 正規表現にキャプチャグループが含まれている場合、には、 `$Matches` 各グループの追加のキーが含まれます。

例:

```powershell
$string = 'The last logged on user was CONTOSO\jsmith'
$string -match 'was (?<domain>.+)\\(?<user>.+)'

$Matches

Write-Output "`nDomain name:"
$Matches.domain

Write-Output "`nUser name:"
$Matches.user
```

```output
True

Name                           Value
----                           -----
domain                         CONTOSO
user                           jsmith
0                              was CONTOSO\jsmith

Domain name:
CONTOSO

User name:
jsmith
```

詳細については、「 [about_Regular_Expressions](about_Regular_Expressions.md)」を参照してください。

## <a name="replacement-operator"></a>置換演算子

### <a name="replacement-with-regular-expressions"></a>正規表現での置換

と同様に `-match` 、 `-replace` 演算子は正規表現を使用して、指定されたパターンを検索します。 ただし、とは異なり、 `-match` 一致を別の指定された値に置き換えます。

構文:

```
<input> -replace <regular-expression>, <substitute>
```

演算子は、値のすべてまたは一部を、正規表現を使用して、指定された値に置き換えます。 演算子は、ファイル名の変更など、多くの管理タスクに使用できます。 たとえば、次のコマンドは、すべてのファイルのファイル名拡張子 `.txt` をに変更し `.log` ます。

```powershell
Get-ChildItem *.txt | Rename-Item -NewName { $_.name -replace '\.txt$','.log' }
```

既定では、 `-replace` 演算子は大文字と小文字を区別しません。 大文字と小文字を区別するには、を使用 `-creplace` します。 大文字と小文字を区別しないようにするには、を使用 `-ireplace` します。

次に例を示します。

```powershell
"book" -ireplace "B", "C" # Case insensitive
"book" -creplace "B", "C" # Case-sensitive; hence, nothing to replace
```

```Output
Cook
book
```

### <a name="regular-expressions-substitutions"></a>正規表現の置換

また、正規表現を使用して、キャプチャグループと置換を使用してテキストを動的に置換することもできます。 キャプチャグループは、 `<substitute>` `$` グループ識別子の前にドル記号 () を使用して、文字列内で参照できます。

次の例では、 `-replace` 演算子は、の形式でユーザー名を受け取り、 `DomainName\Username` 形式に変換し `Username@DomainName` ます。

```powershell
$SearchExp = '^(?<Username>[\w-.]+)\\(?<DomainName>[\w-.]+)$'
$ReplaceExp = '${Username}@${DomainName}'

'Contoso.local\John.Doe' -replace $SearchExp,$ReplaceExp
```

```output
John.Doe@Contoso.local
```

> [!WARNING]
> この `$` 文字には、PowerShell と正規表現の両方で syntatic ロールがあります。
>
> - PowerShell では、2つの二重引用符の間に変数を指定し、部分式演算子として機能します。
> - 正規表現の検索文字列では、行の終わりを示します。
> - Regex の置換文字列では、キャプチャされたグループを表します。したがって、単一引用符の間に正規表現を配置するか、バックティック () 文字を挿入するようにしてください `` ` `` 。

次に例を示します。

```powershell
$1 = 'Goodbye'

'Hello World' -replace '(\w+) \w+', "$1 Universe"
# Output: Goodbye Universe

'Hello World' -replace '(\w+) \w+', '$1 Universe'
# Output: Hello Universe
```

`$$` Regex のはリテラルを表し `$` ます。 置換 `$$` 文字列内のこの値によって、置換後のリテラルが含まれ `$` ます。 次に例を示します。

```powershell
'5.72' -replace '(.+)', '$ $1' # Output: $ 5.72
'5.72' -replace '(.+)', '$$$1' # Output: $5.72
'5.72' -replace '(.+)', '$$1'  # Output: $1
```

詳細については、「[正規表現での][4] [about_Regular_Expressions](about_Regular_Expressions.md)と置換」を参照してください。

### <a name="substituting-in-a-collection"></a>コレクション内での置換

`<input>`演算子へのが `-replace` コレクションである場合、PowerShell はコレクションのすべての値に置換を適用します。 次に例を示します。

```powershell
"B1","B2","B3","B4","B5" -replace "B", 'a'
a1
a2
a3
a4
a5
```

### <a name="replacement-with-a-script-block"></a>スクリプトブロックを使用した置換

PowerShell 6 以降では、演算子は、 `-replace` 置換を実行するスクリプトブロックも受け入れます。 スクリプトブロックは、一致するたびに1回実行されます。

構文:

```powershell
<String> -replace <regular-expression>, {<Script-block>}
```

スクリプトブロック内では、自動変数を使用し `$_` て、置き換えられる入力テキストやその他の有用な情報にアクセスします。 この変数のクラス型は [system.text.regularexpressions.regexoptions][2]です。

次の例では、3桁の各シーケンスを、対応する文字に置き換えます。 スクリプトブロックは、置換する必要がある3桁のセットごとに実行されます。

```powershell
"072101108108111" -replace "\d{3}", {return [char][int]$_.Value}
```

```output
Hello
```

## <a name="containment-operators"></a>含有演算子

含有演算子 ( `-contains` 、、 `-notcontains` `-in` 、および `-notin` ) は、等値演算子に似ていますが、入力がコレクションの場合でも、常に **ブール** 値を返す点が異なります。 これらの演算子は、最初の一致を検出するとすぐに比較を停止します。一方、等値演算子はすべての入力メンバーを評価します。 非常に大きなコレクションでは、これらの演算子は等値演算子よりも高速に戻ります。

構文:

```
<Collection> -contains <Test-object>
<Collection> -notcontains <Test-object>
<Test-object> -in <Collection>
<Test-object> -notin <Collection>
```

### <a name="-contains-and--notcontains"></a>-contains と-notcontains

これらの演算子は、セットに特定の要素が含まれているかどうかを判断します。 `-contains` 右側 (テストオブジェクト) がセット内のいずれかの要素と一致する場合に True を返します。 `-notcontains` 代わりに False を返します。 テストオブジェクトがコレクションの場合、これらの演算子は参照の等価性を使用します。つまり、セットの要素の1つがテストオブジェクトの同じインスタンスであるかどうかを確認します。

次に例を示します。

```powershell
"abc", "def" -contains "def"                  # Output: True
"abc", "def" -notcontains "def"               # Output: False
"Windows", "PowerShell" -contains "Shell"     # Output: False
"Windows", "PowerShell" -notcontains "Shell"  # Output: True
"abc", "def", "ghi" -contains "abc", "def"    # Output: False
"abc", "def", "ghi" -notcontains "abc", "def" # Output: True
```

より複雑な例:

```powershell
$DomainServers = "ContosoDC1","ContosoDC2","ContosoFileServer","ContosoDNS",
                 "ContosoDHCP","ContosoWSUS"
$thisComputer  = "ContosoDC2"

$DomainServers -contains $thisComputer
# Output: True

$a = "abc", "def"
"abc", "def", "ghi" -contains $a # Output: False
$a, "ghi" -contains $a           # Output: True
```

### <a name="-in-and--notin"></a>-in および-notin

And 演算子は、 `-in` `notin` と演算子の構文の逆に、PowerShell 3 で導入されました `contains` `-notcontain` 。 `-in`左辺が `<test-object>` set 内のいずれかの要素と一致する場合に True を返します。 `-notin` 代わりに **False** を返します。 テストオブジェクトがセットの場合、これらの演算子は参照の等価性を使用して、セットの要素の1つがテストオブジェクトの同じインスタンスであるかどうかを確認します。

次の例では、との例と同じことを行い `-contain` `-notcontain` ますが、代わりにとを使用して記述されてい `-in` `-notin` ます。

```powershell
"def" -in "abc", "def"                  # Output: True
"def" -notin "abc", "def"               # Output: False
"Shell" -in "Windows", "PowerShell"     # Output: False
"Shell" -notin "Windows", "PowerShell"  # Output: True
"abc", "def" -in "abc", "def", "ghi"    # Output: False
"abc", "def" -notin "abc", "def", "ghi" # Output: True
```

より複雑な例:

```powershell
$DomainServers = "ContosoDC1","ContosoDC2","ContosoFileServer","ContosoDNS",
                 "ContosoDHCP","ContosoWSUS"
$thisComputer  = "ContosoDC2"

$thisComputer -in $DomainServers
# Output: True

$a = "abc", "def"
$a -in "abc", "def", "ghi" # Output: False
$a -in $a, "ghi"           # Output: True
```

## <a name="type-comparison"></a>型の比較

型の比較演算子 ( `-is` と `-isnot` ) は、オブジェクトが特定の型であるかどうかを判断するために使用されます。

構文:

```powershell
<object> -is <type-reference>
<object> -isnot <type-reference>
```

例:

```powershell
$a = 1
$b = "1"
$a -is [int]           # Output: True
$a -is $b.GetType()    # Output: False
$b -isnot [int]        # Output: True
$a -isnot $b.GetType() # Output: True
```

## <a name="see-also"></a>関連項目

- [about_Operators](about_Operators.md)
- [about_Regular_Expressions](about_Regular_Expressions.md)
- [about_Wildcards](about_Wildcards.md)
- [Compare-Object](xref:Microsoft.PowerShell.Utility.Compare-Object)
- [Foreach-オブジェクト](xref:Microsoft.PowerShell.Core.ForEach-Object)
- [Where-Object](xref:Microsoft.PowerShell.Core.Where-Object)

[1]: /dotnet/api/system.icomparable
[2]: /dotnet/api/system.iequatable-1
[3]: /dotnet/api/system.text.regularexpressions.match
[4]: about_Redirection.md#potential-confusion-with-comparison-operators
[5]: /dotnet/standard/base-types/substitutions-in-regular-expressions
