---
description: Split 演算子を使用して1つ以上の文字列を部分文字列に分割する方法について説明します。
Locale: en-US
ms.date: 03/24/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_split?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Split
ms.openlocfilehash: c7944c710ae3b6803772de77f50b639de4953340
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602792"
---
# <a name="about-split"></a>分割について

## <a name="short-description"></a>概要
Split 演算子を使用して1つ以上の文字列を部分文字列に分割する方法について説明します。

## <a name="long-description"></a>詳細説明

Split 操作は、1つまたは複数の文字列を部分文字列に分割します。 Split 操作の次の要素を変更できます。

- 区切り記号. 既定値は空白ですが、区切り記号を指定する文字、文字列、パターン、またはスクリプトブロックを指定できます。 PowerShell の Split 演算子は、単純な文字ではなく、区切り記号に正規表現を使用します。
- 部分文字列の最大数。 既定では、すべての部分文字列が返されます。 部分文字列の数よりも小さい数値を指定した場合、残りの部分文字列は最後の部分文字列で連結されます。
- 区切り記号が一致する条件を指定するオプション (SimpleMatch や Multiline など)。

## <a name="syntax"></a>SYNTAX

次の図は、-split 演算子の構文を示しています。

パラメーター名がコマンドに含まれていません。 パラメーター値のみを含めます。 値は、構文図に指定されている順序で表示される必要があります。

```
-Split <String>
-Split (<String[]>)
<String> -Split <Delimiter>[,<Max-substrings>[,"<Options>"]]
<String> -Split {<ScriptBlock>} [,<Max-substrings>]
```

`-iSplit` `-cSplit` `-split` 任意のバイナリ split ステートメント (区切り記号またはスクリプトブロックを含む Split ステートメント) では、またはをに置き換えることができます。 `-iSplit`演算子と `-split` 演算子では大文字と小文字が区別されません。 演算子では大文字と小文字が `-cSplit` 区別されます。つまり、区切り記号の規則が適用されるときに、大文字と小文字が区別されます。

## <a name="parameters"></a>PARAMETERS

### <a name="string-or-string"></a>\<String\> または \<String[]\>

分割する1つ以上の文字列を指定します。 複数の文字列を送信する場合、すべての文字列は同じ区切り記号の規則を使用して分割されます。

例:

```
-split "red yellow blue green"
red
yellow
blue
green
```

### \<Delimiter\>

部分文字列の末尾を識別する文字。 既定の区切り記号は空白文字で、スペースや印刷不可能な文字 (改行 ( \` n)、タブ ( \` t) など) が含まれます。 文字列が分割されると、すべての部分文字列の区切り記号が省略されます。 例:

```
"Lastname:FirstName:Address" -split ":"
Lastname
FirstName
Address
```

既定では、区切り記号は結果から除外されます。 区切り記号の全体または一部を保持するには、保持する部分をかっこで囲みます。
パラメーターを \<Max-substrings\> 追加すると、コマンドによってコレクションが分割されるときに、これが優先されます。 出力の一部として区切り記号を含めることを選択した場合、コマンドは出力の一部として区切り記号を返します。ただし、出力の一部として区切り記号を返すように文字列を分割することは、分割としてカウントされません。

次に例を示します。

```
"Lastname:FirstName:Address" -split "(:)"
Lastname
:
FirstName
:
Address

"Lastname/:/FirstName/:/Address" -split "/(:)/"
Lastname
:
FirstName
:
Address
```

次の例で \<Max-substrings\> は、が3に設定されています。 この結果、文字列値は3つの分割が行われますが、結果の出力には合計5つの文字列が含まれます。分割の後に、最大3つの部分文字列に到達するまで、区切り記号が含まれます。 最後の部分文字列の追加の区切り記号は、部分文字列の一部になります。

```powershell
'Chocolate-Vanilla-Strawberry-Blueberry' -split '(-)', 3
```

```Output
Chocolate
-
Vanilla
-
Strawberry-Blueberry
```

### \<Max-substrings\>

文字列が分割される最大回数を指定します。 既定では、区切り記号によって分割されたすべての部分文字列です。 複数の部分文字列がある場合、それらは最後の部分文字列に連結されます。 部分文字列が少数の場合は、すべての部分文字列が返されます。 値が0の場合は、すべての部分文字列が返されます。 負の値は、入力文字列の末尾から要求された部分文字列の量を返します。

> [!NOTE]
> PowerShell 7 では、負の値のサポートが追加されました。

**Max 部分文字列** には、返されるオブジェクトの最大数が指定されていません。 この値は、文字列が分割される最大回数と同じです。
複数の文字列 (文字列の配列) を演算子に送信すると `-split` 、各文字列に対して **最大部分** 文字列の制限が個別に適用されます。

例:

```powershell
$c = "Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune"
$c -split ",", 5
```

```Output
Mercury
Venus
Earth
Mars
Jupiter,Saturn,Uranus,Neptune
```

```powershell
$c = "Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune"
$c -split ",", -5
```

```Output
Mercury,Venus,Earth,Mars
Jupiter
Saturn
Uranus
Neptune
```

### \<ScriptBlock\>

区切り記号を適用するための規則を指定する式。 式は、$true または $false に評価される必要があります。 スクリプトブロックを中かっこで囲みます。

例:

```powershell
$c = "Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune"
$c -split {$_ -eq "e" -or $_ -eq "p"}
```

```Output
M
rcury,V
nus,
arth,Mars,Ju
it
r,Saturn,Uranus,N

tun
```

### \<Options\>

オプション名を引用符で囲みます。 オプションは、 \<Max-substrings\> ステートメントでパラメーターが使用されている場合にのみ有効です。

Options パラメーターの構文は次のとおりです。

```
"SimpleMatch [,IgnoreCase]"

"[RegexMatch] [,IgnoreCase] [,CultureInvariant]
[,IgnorePatternWhitespace] [,ExplicitCapture]
[,Singleline | ,Multiline]"
```

SimpleMatch オプションは次のとおりです。

- **SimpleMatch**: 区切り記号を評価するときに、単純な文字列比較を使用します。 RegexMatch と共に使用することはできません。
- **IgnoreCase**:-csplit 演算子が指定されている場合でも、大文字と小文字を区別しない一致を強制的に実行します。

RegexMatch オプションは次のとおりです。

- **RegexMatch**: 正規表現の照合を使用して、区切り記号を評価します。 これが既定の動作です。 SimpleMatch と共に使用することはできません。
- **IgnoreCase**:-csplit 演算子が指定されている場合でも、大文字と小文字を区別しない一致を強制的に実行します。
- **Regexoptions.cultureinvariant**: 区切り記号を評価するときに、言語のカルチャの違いを無視します。 RegexMatch でのみ有効です。
- **Ignorepattern whitespace**: エスケープされていない空白と、シャープ記号 (#) でマークされたコメントを無視します。 RegexMatch でのみ有効です。
- **Multiline**: 複数行モードでは `^` 、 `$` 入力文字列の先頭と末尾ではなく、すべての行の先頭の末尾とが一致します。
- **Regexoptions.singleline**: regexoptions.singleline モードでは、入力文字列が *regexoptions.singleline* として扱われます。
  `.`改行を除くすべての文字に一致するのではなく、すべての文字が (改行を含めて) 一致するように強制し `\n` ます。
- **Regexoptions.explicitcapture**: 明示的なキャプチャグループのみが結果一覧に返されるように、名前のない一致グループを無視します。 RegexMatch でのみ有効です。

## <a name="unary-and-binary-split-operators"></a>単項分割演算子と二項分割演算子

単項分割演算子 ( `-split <string>` ) は、コンマよりも優先順位が高くなります。 その結果、コンマ区切りの文字列リストを単項 split 演算子に送信した場合、最初の文字列 (最初のコンマの前) のみが分割されます。

次のパターンのいずれかを使用して、複数の文字列を分割します。

- バイナリ split 演算子 ( \<string[]\> -split) を使用する \<delimiter\>
- すべての文字列をかっこで囲みます。
- 変数に文字列を格納し、その変数を split 演算子に送信します。

次に例を示します。

```
PS> -split "1 2", "a b"
1
2
a b
```

```
PS> "1 2", "a b" -split " "
1
2
a
b
```

```
PS> -split ("1 2", "a b")
1
2
a
b
```

```
PS> $a = "1 2", "a b"
PS> -split $a
1
2
a
b
```

## <a name="examples"></a>例

次のステートメントは、文字列を空白文字で分割します。

```powershell
-split "Windows PowerShell 2.0`nWindows PowerShell with remoting"
```

```Output

Windows
PowerShell
2.0
Windows
PowerShell
with
remoting
```

次のステートメントは、文字列を任意のコンマで分割します。

```powershell
"Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune" -split ','
```

```Output
Mercury
Venus
Earth
Mars
Jupiter
Saturn
Uranus
Neptune
```

次のステートメントは、パターン "er" で文字列を分割します。

```powershell
"Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune" -split 'er'
```

```Output
M
cury,Venus,Earth,Mars,Jupit
,Saturn,Uranus,Neptune
```

次のステートメントは、文字 "N" で大文字と小文字を区別する分割を実行します。

```powershell
"Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune" -cSplit 'N'
```

```Output
Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,
eptune
```

次のステートメントは、"e" と "t" の文字列を分割します。

```powershell
"Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune" -split '[et]'
```

```Output
M
rcury,V
nus,
ar
h,Mars,Jupi

r,Sa
urn,Uranus,N
p
un
```

次のステートメントは、"e" と "r" の文字列を分割しますが、結果として得られる部分文字列は6つの部分文字列に限定されます。

```powershell
"Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune" -split '[er]', 6
```

```Output
M

cu
y,V
nus,
arth,Mars,Jupiter,Saturn,Uranus,Neptune
```

次のステートメントは、文字列を3つの部分文字列に分割します。

```powershell
"a,b,c,d,e,f,g,h" -split ",", 3
```

```Output
a
b
c,d,e,f,g,h
```

次のステートメントは、文字列の末尾から始まる3つの部分文字列に文字列を分割します。

```powershell
"a,b,c,d,e,f,g,h" -split ",", -3
```

```Output
a,b,c,d,e,f
g
h
```

次のステートメントは、2つの文字列を3つの部分文字列に分割します。
(この制限は、各文字列に個別に適用されます)。

```powershell
"a,b,c,d", "e,f,g,h" -split ",", 3
```

```Output
a
b
c,d
e
f
g,h
```

次のステートメントは、1つ目の桁にある文字列の各行を分割します。 また、複数行オプションを使用して、各行と文字列の先頭を認識します。

0は、最大部分文字列パラメーターの "return all" 値を表します。 マルチ部分文字列の値が指定されている場合にのみ、複数行のようなオプションを使用できます。

```powershell
$a = @'
1The first line.
2The second line.
3The third of three lines.
'@
$a -split "^\d", 0, "multiline"
```

```Output

The first line.

The second line.

The third of three lines.
```

次のステートメントでは、円記号を使用して、ドット (.) 区切り記号をエスケープします。

既定の RegexMatch では、引用符 (".") で囲まれたドットは、改行文字を除く任意の文字と一致するように解釈されます。 その結果、Split ステートメントは、改行を除くすべての文字に対して空白行を返します。

```powershell
"This.is.a.test" -split "\."
```

```Output
This
is
a
test
```

次のステートメントでは、SimpleMatch オプションを使用して、-split 演算子がドット (.) 区切り記号を文字どおり解釈するように指定しています。

0は、最大部分文字列パラメーターの "return all" 値を表します。 SimpleMatch などのオプションは、最大部分文字列の値が指定されている場合にのみ使用できます。

```powershell
"This.is.a.test" -split ".", 0, "simplematch"
```

```Output
This
is
a
test
```

次のステートメントは、変数の値に応じて、2つの区切り記号のいずれかで文字列を分割します。

```powershell
$i = 1
$c = "LastName, FirstName; Address, City, State, Zip"
$c -split $(if ($i -lt 1) {","} else {";"})
```

```Output
LastName, FirstName
 Address, City, State, Zip
```

## <a name="see-also"></a>関連項目

[Split-Path](xref:Microsoft.PowerShell.Management.Split-Path)

[about_Operators](about_Operators.md)

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Join](about_Join.md)

