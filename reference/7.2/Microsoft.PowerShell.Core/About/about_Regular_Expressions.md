---
description: PowerShell の正規表現について説明します。
Locale: en-US
ms.date: 03/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_regular_expressions?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Regular_Expressions
ms.openlocfilehash: 4dc6ac670a31cbea4c35ee6540ce3368ad9b02ca
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599510"
---
# <a name="about-regular-expressions"></a>正規表現について

## <a name="short-description"></a>簡単な説明
PowerShell の正規表現について説明します。

## <a name="long-description"></a>長い説明

> [!NOTE]
> この記事では、すべての構文について説明するのではなく、PowerShell で正規表現を使用するための構文とメソッドについて説明します。 詳細なリファレンスについては、「 [正規表現言語-クイックリファレンス](/dotnet/standard/base-types/regular-expression-language-quick-reference)」を参照してください。

正規表現は、テキストの照合に使用されるパターンです。 これは、リテラル文字、演算子、およびその他の構成体で構成できます。

この記事では、PowerShell での正規表現の構文について説明します。 PowerShell には、正規表現を使用する複数の演算子とコマンドレットがあります。 構文と使用法の詳細については、以下のリンクを参照してください。

- [Select-String](xref:Microsoft.PowerShell.Utility.Select-String)
- [-match および-replace 演算子](about_Comparison_Operators.md)
- [-分割](about_Split.md)
- [switch ステートメントと-regex オプション](about_Switch.md)

PowerShell の正規表現では、既定では大文字と小文字が区別されません。 上に示した各メソッドは、大文字と小文字の区別を強制する方法が異なります。

|       メソッド       |                      大文字と小文字の区別                      |
| ------------------ | ---------------------------------------------------------- |
| `Select-String`    | スイッチの使用 `-CaseSensitive`                                |
| `switch` ステートメント | オプションを使用する `-casesensitive`                            |
| 演算子          | **' c '** で始まるプレフィックス ( `-cmatch` 、、 `-csplit` または `-creplace` ) |

### <a name="character-literals"></a>文字リテラル

正規表現には、リテラル文字または文字列を指定できます。 式を指定すると、エンジンは、指定されたテキストを正確に照合します。

```powershell
# This statement returns true because book contains the string "oo"
'book' -match 'oo'
```

### <a name="character-classes"></a>文字クラス

文字リテラルは、正確なパターンがわかっている場合には機能しますが、文字クラスを使用すると、より明確にすることができます。

#### <a name="character-groups"></a>文字グループ

`[character group]` では、任意の数の文字を1回だけ照合でき `[^character group]` ます。一方、はグループに含まれていない文字にのみ一致します。

```powershell
# This expression returns true if the pattern matches big, bog, or bug.
'big' -match 'b[iou]g'
```

一致する文字の一覧にハイフン文字 () が含まれている場合は、 `-` 文字範囲式と区別するために、リストの先頭または末尾にある必要があります。

#### <a name="character-ranges"></a>文字範囲

パターンは、文字の範囲にすることもできます。 文字には、英字 `[A-Z]` 、数字 `[0-9]` 、または ASCII ベース `[ -~]` (印刷可能なすべての文字) を使用できます。

```powershell
# This expression returns true if the pattern matches any 2 digit number.
42 -match '[0-9][0-9]'
```

#### <a name="numbers"></a>数値

`\d`文字クラスは、任意の10進数字と一致します。 反対に、 `\D` は10進数以外の数字と一致します。

```powershell
# This expression returns true if it matches a server name.
# (Server-01 - Server-99).
'Server-01' -match 'Server-\d\d'
```

#### <a name="word-characters"></a>単語の文字

`\w`文字クラスは、任意の単語文字と一致し `[a-zA-Z_0-9]` ます。 単語以外の文字と一致させるには、を使用し `\W` ます。

```powershell
# This expression returns true.
# The pattern matches the first word character 'B'.
'Book' -match '\w'
```

#### <a name="wildcards"></a>ワイルドカード

ピリオド ( `.` ) は、正規表現でのワイルドカード文字です。 改行文字 () を除く任意の文字と一致し `\n` ます。

```powershell
# This expression returns true.
# The pattern matches any 4 characters except the newline.
'a1\ ' -match '....'
```

#### <a name="whitespace"></a>空白文字

空白は、文字クラスを使用して照合され `\s` ます。 空白以外の文字は、を使用して照合され `\S` ます。 リテラルスペース文字 `' '` を使用することもできます。

```powershell
# This expression returns true.
# The pattern uses both methods to match the space.
' - ' -match '\s- '
```

### <a name="quantifiers"></a>量指定子

量指定子は、入力文字列に含まれる各要素のインスタンスの数を制御します。

PowerShell で使用可能な量指定子のいくつかを次に示します。

| 量指定子 |                説明                |
| ---------- | ----------------------------------------- |
| `*`        | 0回以上。                       |
| `+`        | 1回以上。                        |
| `?`        | 0回または1回。                         |
| `{n,m}`    | 少なくとも、少なくとも 1 `n` `m` 回。 |

アスタリスク () は、 `*` 直前の要素の0回以上の繰り返しに一致します。 結果として、要素のない入力文字列も一致と見なされます。

```powershell
# This returns true for all account name strings even if the name is absent.
'ACCOUNT NAME:    Administrator' -match 'ACCOUNT NAME:\s*\w*'
```

正符号 () は、 `+` 直前の要素と1回以上一致します。

```powershell
# This returns true if it matches any server name.
'DC-01' -match '[A-Z]+-\d\d'
```

疑問符は、 `?` 直前の要素と0回または1回一致します。 アスタリスクと同様 `*` に、要素が存在しない文字列も一致します。

```powershell
# This returns true for any server name, even server names without dashes.
'SERVER01' -match '[A-Z]+-?\d\d'
```

量 `{n, m}` 指定子は、量指定子をきめ細かく制御できるように、いくつかの異なる方法で使用できます。 2番目の要素 `m` とコンマは `,` 省略可能です。

| 量指定子 |                説明                 |
| ---------- | ------------------------------------------ |
| `{n}`      | 正確 `n` な回数に一致します。         |
| `{n,}`     | 少なくとも回数に一致 `n` します。        |
| `{n,m}`    | `n`と `m` の回数の間で一致します。 |

```powershell
# This returns true if it matches any phone number.
'111-222-3333' -match '\d{3}-\d{3}-\d{4}'
```

### <a name="anchors"></a>アンカー

アンカーを使用すると、入力文字列内で一致する位置に基づいて、一致と見なされるようにすることができます。

一般的に使用される2つのアンカーは `^` と `$` です。 カレットは文字列の `^` 先頭に一致し、は `$` 文字列の末尾に一致します。 アンカーを使用すると、特定の位置にあるテキストを一致させながら、不要な文字を破棄することができます。

```powershell
# The pattern expects the string 'fish' to be the only thing on the line.
# This returns FALSE.
'fishing' -match '^fish$'
```

> [!NOTE]
> アンカーを含む regex を定義する場合 `$` は、二重引用符 () ではなく単一引用符 () を使用して regex を囲む必要があります。そうしないと、 `'` PowerShell に `"` よって式が変数として拡張されます。

PowerShell でアンカーを使用する場合は、 **regexoptions.singleline** と **複数行** の正規表現オプションの違いを理解しておく必要があります。

- **Multiline**: 複数行モードでは `^` 、 `$` 入力文字列の先頭と末尾ではなく、すべての行の先頭の末尾とが一致します。
- **Regexoptions.singleline**: regexoptions.singleline モードでは、入力文字列が **regexoptions.singleline** として扱われます。
  `.`改行を除くすべての文字に一致するのではなく、すべての文字が (改行を含めて) 一致するように強制し `\n` ます。

これらのオプションの詳細と使用方法については、「 [正規表現言語-クイックリファレンス](/dotnet/standard/base-types/regular-expression-language-quick-reference)」を参照してください。

### <a name="escaping-characters"></a>文字のエスケープ

バックスラッシュ ( `\` ) は、正規表現エンジンによって解析されないように、文字をエスケープするために使用されます。

次の文字が予約されています: `[]().\^$|?*+{}` 。

これらの文字を入力文字列内で照合するには、パターン内でエスケープする必要があります。

```powershell
# This returns true and matches numbers with at least 2 digits of precision.
# The decimal point is escaped using the backslash.
'3.141' -match '3\.\d{2,}'
```

Regex クラスの静的メソッドを使用すると、テキストをエスケープできます。

```powershell
[regex]::escape('3.\d{2,}')
```

```Output
3\.\\d\{2,}
```

> [!NOTE]
> これにより、文字クラスで使用される既存の円記号を含む、予約済みの正規表現文字がすべてエスケープされます。 これは、エスケープする必要があるパターンの部分でのみ使用してください。

#### <a name="other-character-escapes"></a>その他の文字のエスケープ

特殊文字の種類を一致させるために使用できる予約文字エスケープもあります。

一般的に使用される文字エスケープをいくつか次に示します。

|文字エスケープ  |説明  |
|---------|---------|
|`\t`|タブに一致します|
|`\n`|改行に一致します|
|`\r`|キャリッジリターンと一致します|

### <a name="groups-captures-and-substitutions"></a>グループ、キャプチャ、および置換

グループ化構成体は、入力文字列をキャプチャまたは無視できる部分文字列に分割します。 グループ化された部分文字列を部分式と呼びます。 既定では、部分式は番号付きグループにキャプチャされますが、名前を割り当てることもできます。

グループ化構成体は、かっこで囲まれた正規表現です。 囲まれた正規表現に一致するテキストがキャプチャされます。 次の例では、入力テキストを2つのキャプチャグループに分割します。

```powershell
'The last logged on user was CONTOSO\jsmith' -match '(.+was )(.+)'
```

```Output
True
```

`$Matches`キャプチャしたテキストを取得するには、 **Hashtable** 自動変数を使用します。
一致した文字列全体を表すテキストは、キーに格納され `0` ます。

```powershell
$Matches.0
```

```Output
The last logged on user was CONTOSO\jsmith
```

キャプチャは、左から右に増加する数値の **整数** キーに格納されます。 Capture には、 `1` ユーザー名になるまですべてのテキストが含まれており、capture には `2` ユーザー名だけが含まれています。

```powershell
$Matches
```

```Output
Name                           Value
----                           -----
2                              CONTOSO\jsmith
1                              The last logged on user was
0                              The last logged on user was CONTOSO\jsmith
```

> [!IMPORTANT]
> `0`キーは **整数** です。 任意の **ハッシュテーブル** メソッドを使用して、格納されている値にアクセスできます。
>
> ```
> PS> 'Good Dog' -match 'Dog'
> True
>
> PS> $Matches[0]
> Dog
>
> PS> $Matches.Item(0)
> Dog
>
> PS> $Matches.0
> Dog
> ```

#### <a name="named-captures"></a>名前付きキャプチャ

既定では、キャプチャは左から右に昇順で格納されます。
キャプチャグループに **名前** を割り当てることもできます。 この **名前** は、 `$Matches` **ハッシュテーブル** の自動変数のキーになります。

キャプチャグループ内で、を使用して、 `?<keyname>` キャプチャされたデータを名前付きキーの下に格納します。

```
PS> $string = 'The last logged on user was CONTOSO\jsmith'
PS> $string -match 'was (?<domain>.+)\\(?<user>.+)'
True

PS> $Matches

Name                           Value
----                           -----
domain                         CONTOSO
user                           jsmith
0                              was CONTOSO\jsmith

PS> $Matches.domain
CONTOSO

PS> $Matches.user
jsmith
```

次の例では、Windows セキュリティログに最新のログエントリを格納します。
指定された正規表現は、メッセージからユーザー名とドメインを抽出し、キーの下に格納します。名前の場合は **N** 、ドメインの場合は **D** です。

```powershell
$log = (Get-WinEvent -LogName Security -MaxEvents 1).message
$r = '(?s).*Account Name:\s*(?<N>.*).*Account Domain:\s*(?<D>[A-Z,0-9]*)'
$log -match $r
```

```Output
True
```

```powershell
$Matches
```

```Output
Name                           Value
----                           -----
D                              CONTOSO
N                              jsmith
0                              A process has exited....
```

詳細については、「 [正規表現でのグループ化構成体](/dotnet/standard/base-types/grouping-constructs-in-regular-expressions)」を参照してください。

#### <a name="substitutions-in-regular-expressions"></a>正規表現での置換

演算子で正規表現を使用すると、 `-replace` キャプチャしたテキストを使用してテキストを動的に置換できます。

`<input> -replace <original>, <substitute>`

- `<input>`: 検索する文字列
- `<original>`: 入力文字列の検索に使用する正規表現
- `<substitute>`: 入力文字列で見つかった一致項目を置換する正規表現の置換式。

> [!NOTE]
> `<original>`オペランドと `<substitute>` オペランドは、文字エスケープなどの正規表現エンジンの規則に従います。

キャプチャグループは、文字列内で参照でき `<substitute>` ます。 置換を行うには、 `$` グループ識別子の前にある文字を使用します。

キャプチャグループを参照する2つの方法は、数値と **名前** で **指定** します。

- **数値** キャプチャグループには、左から右に番号が付けられます。

  ```powershell
  'John D. Smith' -replace '(\w+) (\w+)\. (\w+)', '$1.$2.$3@contoso.com'
  ```

  ```Output
  John.D.Smith@contoso.com
  ```

- **名前** のキャプチャグループは、名前によって参照することもできます。

  ```powershell
  'CONTOSO\Administrator' -replace '\w+\\(?<user>\w+)', 'FABRIKAM\${user}'
  ```

  ```Output
  FABRIKAM\Administrator
  ```

`$&`式は、一致したすべてのテキストを表します。

```powershell
'Gobble' -replace 'Gobble', '$& $&'
```

```Output
Gobble Gobble
```

> [!WARNING]
> 文字は `$` 文字列の展開で使用されるため、置換でリテラル文字列を使用するか、 `$` 二重引用符を使用する場合は文字をエスケープする必要があります。
>
> ```powershell
> 'Hello World' -replace '(\w+) \w+', '$1 Universe'
> "Hello World" -replace "(\w+) \w+", "`$1 Universe"
> ```
>
> ```Output
> Hello Universe
> Hello Universe
> ```
>
> また、を `$` リテラル文字として使用する場合は、 `$$` 通常のエスケープ文字の代わりにを使用します。 二重引用符を使用する場合でも、 `$` 不適切な置換を避けるために、のすべてのインスタンスをエスケープします。
>
> ```powershell
> '5.72' -replace '(.+)', '$$$1'
> "5.72" -replace "(.+)", "`$`$`$1"
> ```
>
> ```Output
> $5.72
> $5.72
> ```

詳細については、「 [正規表現での置換](/dotnet/standard/base-types/substitutions-in-regular-expressions)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Operators](about_Operators.md)

