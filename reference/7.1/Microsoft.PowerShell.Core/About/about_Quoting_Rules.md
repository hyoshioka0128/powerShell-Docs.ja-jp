---
description: PowerShell で単一引用符と二重引用符を使用するための規則について説明します。
Locale: en-US
ms.date: 12/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_quoting_rules?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Quoting_Rules
ms.openlocfilehash: d8cc6bb875f6d0ec29ae79eb6350edabe493c8f5
ms.sourcegitcommit: 9a86cac80402d8193147058d4ba50e07b26059dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97490545"
---
# <a name="about-quoting-rules"></a>引用符の規則について

## <a name="short-description"></a>簡単な説明
PowerShell で単一引用符と二重引用符を使用するための規則について説明します。

## <a name="long-description"></a>長い説明

リテラル文字列を指定するには、引用符を使用します。 文字列は単一引用符 ( `'` ) または二重引用符 () で囲むことができ `"` ます。

引用符は、ここで指定した _文字列_ を作成するためにも使用されます。 ここでは、引用符が文字どおりに解釈される一重引用符または二重引用符で囲まれた文字列です。 ここでは、複数の行にわたって文字列を指定できます。 ここで指定した文字列のすべての行は、引用符で囲まれていない場合でも、文字列として解釈されます。

リモートコンピューターへのコマンドでは、引用符によって、リモートコンピューター上で実行されるコマンドの部分が定義されます。 リモートセッションでは、引用符は、コマンド内の変数がローカルコンピューターとリモートコンピューターのどちらで最初に解釈されるかを決定します。

## <a name="single-and-double-quoted-strings"></a>単一引用符と二重引用符で囲まれた文字列

二重引用符で囲まれた文字列は、 _展開_ 可能な文字列です。 変数名の前にドル記号 ( `$` ) を付けると、文字列が処理のためにコマンドに渡される前に、変数の値に置き換えられます。

以下に例を示します。

```powershell
$i = 5
"The value of $i is $i."
```

このコマンドの出力は次のとおりです。

```Output
The value of 5 is 5.
```

また、二重引用符で囲まれた文字列では、式が評価され、結果が文字列に挿入されます。 以下に例を示します。

```powershell
"The value of $(2+3) is 5."
```

このコマンドの出力は次のとおりです。

```Output
The value of 5 is 5.
```

単一引用符で囲まれた文字列は _逐語_ 的文字列です。 文字列は、入力したとおりにコマンドに渡されます。 置換は実行されません。
以下に例を示します。

```powershell
$i = 5
'The value of $i is $i.'
```

このコマンドの出力は次のとおりです。

```Output
The value $i is $i.
```

同様に、単一引用符で囲まれた文字列の式は評価されません。 これらはリテラルとして解釈されます。 以下に例を示します。

```powershell
'The value of $(2+3) is 5.'
```

このコマンドの出力は次のとおりです。

```Output
The value of $(2+3) is 5.
```

変数値が二重引用符で囲まれた文字列に代入されないようにするには、PowerShell のエスケープ文字であるバックティック character ( `` ` `` ) (ASCII 96) を使用します。

次の例では、最初の変数の前にあるバックティック文字によって、 `$i` PowerShell が変数名をその値に置き換えることができなくなります。
以下に例を示します。

```powershell
$i = 5
"The value of `$i is $i."
```

このコマンドの出力は次のとおりです。

```Output
The value $i is 5.
```

文字列内に二重引用符を表示するには、文字列全体を単一引用符で囲みます。 以下に例を示します。

```powershell
'As they say, "live and learn."'
```

このコマンドの出力は次のとおりです。

```Output
As they say, "live and learn."
```

また、単一引用符で囲まれた文字列を二重引用符で囲むこともできます。 以下に例を示します。

```powershell
"As they say, 'live and learn.'"
```

このコマンドの出力は次のとおりです。

```Output
As they say, 'live and learn.'
```

または、二重引用符で囲まれた語句を二重引用符で囲みます。 以下に例を示します。

```powershell
"As they say, ""live and learn."""
```

このコマンドの出力は次のとおりです。

```Output
As they say, "live and learn."
```

単一引用符で囲まれた文字列に単一引用符を含めるには、2つ目の連続する単一引用符を使用します。 以下に例を示します。

```powershell
'don''t'
```

このコマンドの出力は次のとおりです。

```Output
don't
```

PowerShell によって二重引用符が文字どおりに解釈されるようにするには、バックティック文字を使用します。 これにより、PowerShell によって引用符が文字列の区切り記号として解釈されるのを防ぐことができます。 以下に例を示します。

```powershell
PS> "Use a quotation mark (`") to begin a string."
Use a quotation mark (") to begin a string.
PS> 'Use a quotation mark (`") to begin a string.'
Use a quotation mark (`") to begin a string.
```

単一引用符で囲まれた文字列の内容は文字どおりに解釈されるため、バックティック文字はリテラル文字として扱われ、出力に表示されます。

## <a name="here-strings"></a>ここ-文字列

ここでは、文字列の引用規則が少し異なります。

ここでは、引用符が文字どおりに解釈される一重引用符または二重引用符で囲まれた文字列です。 ここでは、複数の行にわたって文字列を指定できます。 ここで指定した文字列のすべての行は、引用符で囲まれていなくても文字列として解釈されます。

通常の文字列と同様に、変数は、二重引用符で囲まれた文字列の値に置き換えられます。 ここでは、単一引用符で囲まれた文字列では、変数は値に置き換えられません。

ここでは任意のテキストを使用できますが、次の種類のテキストには特に便利です。

- リテラル引用符を含むテキスト
- HTML または XML 内のテキストなどの複数行のテキスト
- スクリプトまたは関数ドキュメントのヘルプテキスト

ここでは、次のいずれかの形式を使用できます。は、 `<Enter>` <kbd>enter</kbd> キーを押したときに追加される改行または改行の非表示文字を表します。

二重引用符:

```
@"<Enter>
<string> [string] ...<Enter>
"@
```

単一引用符:

```
@'<Enter>
<string> [string] ...<Enter>
'@
```

どちらの形式でも、終わりの引用符は、行の最初の文字である必要があります。

ここでは、2つの非表示文字間のすべてのテキストが含まれています。 この文字列では、すべての引用符が文字どおりに解釈されます。 以下に例を示します。

```powershell
@"
For help, type "get-help"
"@
```

このコマンドの出力は次のとおりです。

```Output
For help, type "get-help"
```

ここで説明した文字列を使用すると、コマンド内での文字列の使用が簡単になります。 以下に例を示します。

```powershell
@"
Use a quotation mark (') to begin a string.
"@
```

このコマンドの出力は次のとおりです。

```Output
Use a quotation mark (') to begin a string.
```

ここでは、単一引用符で囲まれた文字列では、変数は文字どおりに解釈され、正確に再現されます。 以下に例を示します。

```powershell
@'
The $profile variable contains the path
of your PowerShell profile.
'@
```

このコマンドの出力は次のとおりです。

```Output
The $profile variable contains the path
of your PowerShell profile.
```

ここで二重引用符で囲まれた文字列は、変数の値に置き換えられます。 以下に例を示します。

```powershell
@"
Even if you have not created a profile,
the path of the profile file is:
$profile.
"@
```

このコマンドの出力は次のとおりです。

```Output
Even if you have not created a profile,
the path of the profile file is:
C:\Users\User1\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1.
```

ここでは、通常、複数行を変数に割り当てるために文字列を使用します。 たとえば、次の文字列では、XML のページが $page 変数に代入されます。

```powershell
$page = [XML] @"
<command:command xmlns:maml="http://schemas.microsoft.com/maml/2004/10"
xmlns:command="http://schemas.microsoft.com/maml/dev/command/2004/10"
xmlns:dev="http://schemas.microsoft.com/maml/dev/2004/10">
<command:details>
        <command:name>
               Format-Table
        </command:name>
        <maml:description>
            <maml:para>Formats the output as a table.</maml:para>
        </maml:description>
        <command:verb>format</command:verb>
        <command:noun>table</command:noun>
        <dev:version></dev:version>
</command:details>
...
</command:command>
"@
```

ここでは、コマンドレットへの入力に便利な形式として、ここで指定した `ConvertFrom-StringData` 文字列をハッシュテーブルに変換します。
詳細については、「`ConvertFrom-StringData`」を参照してください。

## <a name="see-also"></a>関連項目

[about_Special_Characters](about_Special_Characters.md)

[ConvertFrom-StringData](xref:Microsoft.PowerShell.Utility.ConvertFrom-StringData)
