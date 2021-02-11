---
description: スイッチを使用して複数のステートメントを処理する方法について説明 `If` します。
Locale: en-US
ms.date: 05/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_switch?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Switch
ms.openlocfilehash: 5ca12fe1d5052c1589c5dfecca8cf08cf68901e8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603239"
---
# <a name="about-switch"></a>スイッチの概要

## <a name="short-description"></a>簡単な説明
スイッチを使用して複数のステートメントを処理する方法について説明 `If` します。

## <a name="long-description"></a>長い説明

スクリプトまたは関数で条件を確認するには、ステートメントを使用し `If` ます。 ステートメントでは `If` 、変数の値やオブジェクトのプロパティなど、さまざまな種類の条件を確認できます。

複数の条件を確認するには、ステートメントを使用し `Switch` ます。 `Switch`ステートメントは一連のステートメントに相当しますが、より `If` 簡単です。 ステートメントでは、 `Switch` 各条件とオプションのアクションが一覧表示されます。 条件がを取得すると、アクションが実行されます。

ステートメントでは、 `Switch` `$_` および自動変数を使用でき `$switch` ます。 詳細については、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

基本 `Switch` ステートメントの形式は次のとおりです。

```powershell
Switch (<test-value>)
{
    <condition> {<action>}
    <condition> {<action>}
}
```

たとえば、次のステートメントでは、 `Switch` テスト値3を各条件に比較しています。 テスト値が条件に一致した場合、アクションが実行されます。

```powershell
switch (3)
{
    1 {"It is one."}
    2 {"It is two."}
    3 {"It is three."}
    4 {"It is four."}
}
```

```Output
It is three.
```

この簡単な例では、値3が一致する場合でも、値はリストの各条件と比較されます。 次の `Switch` ステートメントには、値が3の2つの条件があります。 既定では、すべての条件がテストされることを示しています。

```powershell
switch (3)
{
    1 {"It is one."}
    2 {"It is two."}
    3 {"It is three."}
    4 {"It is four."}
    3 {"Three again."}
}
```

```Output
It is three.
Three again.
```

一致後にを比較しないようにに指示するには、 `Switch` ステートメントを使用し `Break` ます。 ステートメントは、 `Break` ステートメントを終了し `Switch` ます。

```powershell
switch (3)
{
    1 {"It is one."}
    2 {"It is two."}
    3 {"It is three."; Break}
    4 {"It is four."}
    3 {"Three again."}
}
```

```Output
It is three.
```

テスト値が配列などのコレクションである場合、コレクション内の各項目は、出現する順序で評価されます。 次の例では、4と2が評価されます。

```powershell
switch (4, 2)
{
    1 {"It is one." }
    2 {"It is two." }
    3 {"It is three." }
    4 {"It is four." }
    3 {"Three again."}
}
```

```Output
It is four.
It is two.
```

次の例に示すように、すべてのステートメントは、 `Break` 各値ではなく、コレクションに適用されます。 ステートメントは、 `Switch` `Break` 値4の条件でステートメントによって終了されます。

```powershell
switch (4, 2)
{
    1 {"It is one."; Break}
    2 {"It is two." ; Break }
    3 {"It is three." ; Break }
    4 {"It is four." ; Break }
    3 {"Three again."}
}
```

```Output
It is four.
```

### <a name="syntax"></a>構文

ステートメントの完全な `Switch` 構文は次のとおりです。

```
switch [-regex|-wildcard|-exact][-casesensitive] (<value>)
{
    "string"|number|variable|{ expression } { statementlist }
    default { statementlist }
}
```

または

```
switch [-regex|-wildcard|-exact][-casesensitive] -file filename
{
    "string"|number|variable|{ expression } { statementlist }
    default { statementlist }
}
```

パラメーターを使用しない場合、は `Switch` **正確** なパラメーターを使用するのと同じように動作します。 値に対して大文字と小文字を区別しない一致を実行します。 値がコレクションの場合は、各要素が出現する順序で評価されます。

ステートメントには、 `Switch` 少なくとも1つの condition ステートメントを含める必要があります。

`Default`値がどの条件にも一致しない場合、句がトリガーされます。 これは、 `Else` ステートメントの句に相当 `If` します。 `Default`各ステートメントで使用できる句は1つだけです `Switch` 。

`Switch` には次のパラメーターがあります。

- **ワイルドカード** -条件がワイルドカード文字列であることを示します。 Match 句が文字列でない場合、パラメーターは無視されます。 比較では大文字と小文字は区別されません。
- **Exact** -match 句が文字列である場合は、正確に一致する必要があることを示します。 Match 句が文字列でない場合、このパラメーターは無視されます。 比較では大文字と小文字は区別されません。
- **CaseSensitive** -大文字と小文字を区別する一致を実行します。 Match 句が文字列でない場合、このパラメーターは無視されます。
- **File**-値ステートメントではなく、ファイルからの入力を受け取ります。 複数の **ファイル** パラメーターが含まれている場合は、最後のパラメーターのみが使用されます。 ファイルの各行は、ステートメントによって読み取られ、評価され `Switch` ます。 比較では大文字と小文字は区別されません。
- **Regex** -条件に対して値の正規表現の一致を実行します。 Match 句が文字列でない場合、このパラメーターは無視されます。
  比較では大文字と小文字は区別されません。 `$matches`自動変数は、対応するステートメントブロック内で使用できます。

> [!NOTE]
> **Regex** や **ワイルドカード** などの競合する値を指定すると、指定された最後のパラメーターが優先され、競合するすべてのパラメーターは無視されます。 パラメーターの複数のインスタンスを使用することもできます。 ただし、最後に使用されたパラメーターのみが有効です。

この例では、文字列または数値データではないオブジェクトがに渡され `Switch` ます。 は、 `Switch` オブジェクトに対して文字列の強制変換を実行し、結果を評価します。

```powershell
$test = @{
    Test  = 'test'
    Test2 = 'test2'
}

$test.ToString()

switch -Exact ($test)
{
    'System.Collections.Hashtable'
    {
        'Hashtable string coercion'
    }
    'test'
    {
        'Hashtable value'
    }
}
```

```Output
System.Collections.Hashtable
Hashtable string coercion
```

この例では、一致するケースがないため、出力はありません。

```powershell
switch ("fourteen")
{
    1 {"It is one."; Break}
    2 {"It is two."; Break}
    3 {"It is three."; Break}
    4 {"It is four."; Break}
    "fo*" {"That's too many."}
}
```

句を追加することで `Default` 、他の条件が成功しなかった場合にアクションを実行できます。

```powershell
switch ("fourteen")
{
    1 {"It is one."; Break}
    2 {"It is two."; Break}
    3 {"It is three."; Break}
    4 {"It is four."; Break}
    "fo*" {"That's too many."}
    Default {
        "No matches"
    }
}
```

```Output
No matches
```

"14" という語を大文字と小文字に一致させるに `-Wildcard` は、パラメーターまたはパラメーターを使用する必要があり `-Regex` ます。

```powershell
   PS> switch -Wildcard ("fourteen")
       {
           1 {"It is one."; Break}
           2 {"It is two."; Break}
           3 {"It is three."; Break}
           4 {"It is four."; Break}
           "fo*" {"That's too many."}
       }
 ```

```Output
That's too many.
```

次の例では、パラメーターを使用し `-Regex` ます。

```powershell
$target = 'https://bing.com'
switch -Regex ($target)
{
    '^ftp\://.*$' { "$_ is an ftp address"; Break }
    '^\w+@\w+\.com|edu|org$' { "$_ is an email address"; Break }
    '^(http[s]?)\://.*$' { "$_ is a web address that uses $($matches[1])"; Break }
}
```

```Output
https://bing.com is a web address that uses https
```

Switch ステートメントの条件には、次のいずれかを指定できます。

- 値が入力値と比較される式。
- `$true`条件が満たされた場合にを返すスクリプトブロック。

`$_`自動変数には switch ステートメントに渡される値が含まれており、condition ステートメントのスコープ内で評価および使用できます。

各条件のアクションは、他の条件のアクションから独立しています。

次の例では、ステートメントの条件としてスクリプトブロックを使用する方法を示し `Switch` ます。

```powershell
switch ("Test")
{
    {$_ -is [String]} {
        "Found a string"
    }
    "Test" {
        "This $_ executes as well"
    }
}
```

```Output
Found a string
This Test executes as well
```

値が複数の条件に一致する場合は、各条件のアクションが実行されます。 この動作を変更するに `Break` は、キーワードまたはキーワードを使用し `Continue` ます。

`Break`キーワードは処理を停止し、ステートメントを終了し `Switch` ます。

キーワードは、 `Continue` 現在の値の処理を停止しますが、後続の値の処理は続行されます。

次の例では、数値の配列を処理し、奇数または偶数であるかどうかを表示します。 負の数値は、キーワードを使用してスキップされ `Continue` ます。 非数が検出された場合、実行はキーワードを使用して終了し `Break` ます。

```powershell
switch (1,4,-1,3,"Hello",2,1)
{
    {$_ -lt 0} { Continue }
    {$_ -isnot [Int32]} { Break }
    {$_ % 2} {
        "$_ is Odd"
    }
    {-not ($_ % 2)} {
        "$_ is Even"
    }
}
```

```Output
1 is Odd
4 is Even
3 is Odd
```

## <a name="see-also"></a>関連項目

[about_Break](about_Break.md)

[about_Continue](about_Continue.md)

[about_If](about_If.md)

[about_Script_Blocks](about_Script_Blocks.md)
