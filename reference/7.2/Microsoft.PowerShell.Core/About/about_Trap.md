---
description: 終了エラーを処理するキーワードについて説明します。
Locale: en-US
ms.date: 06/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_trap?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Trap
ms.openlocfilehash: 30b03235cbc2338de3786e37416d1c1ce1d660e3
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602186"
---
# <a name="about-trap"></a>トラップについて

## <a name="short-description"></a>簡単な説明

終了エラーを処理するキーワードについて説明します。

## <a name="long-description"></a>長い説明

終了エラーにより、ステートメントの実行が停止します。 PowerShell が何らかの方法で終了エラーを処理しない場合、PowerShell は現在のパイプラインでの関数またはスクリプトの実行も停止します。 C# などの他の言語では、終了エラーは例外として知られています。

キーワードは、 `trap` 終了エラーが発生したときに実行するステートメントのリストを指定します。 `trap` ステートメントは、次の方法で終了エラーを処理します。

- ステートメントブロックを処理し、を `trap` 含むスクリプトまたは関数の実行を続行した後に、エラーを表示し `trap` ます。 これが既定の動作です。

- ステートメントでを使用して、エラーを表示し、を含むスクリプトまたは関数の実行を中止し `trap` `break` `trap` ます。

- エラーを無音にしますが、 `trap` ステートメントでを使用して、を含むスクリプトまたは関数の実行を続行し `continue` `trap` ます。

のステートメントの一覧には、 `trap` 複数の条件または関数呼び出しを含めることができます。 では、 `trap` ログやテスト条件を書き込んだり、別のプログラムを実行したりできます。

### <a name="syntax"></a>構文

`trap` ステートメントの構文は次のとおりです。

```powershell
trap [[<error type>]] {<statement list>}
```

ステートメントには、 `trap` 終了エラーが発生したときに実行するステートメントのリストが含まれています。 ステートメントは、 `trap` キーワード、 `trap` オプションで型式、およびエラーがトラップされたときに実行するステートメントのリストを含むステートメントブロックで構成されます。 型式は、キャッチするエラーの種類を指定し `trap` ます。

スクリプトまたはコマンドには、複数のステートメントを含めることができ `trap` ます。 `trap` ステートメントは、スクリプトまたはコマンド内の任意の場所に置くことができます。

### <a name="trapping-all-terminating-errors"></a>すべての終了エラーのトラップ

スクリプトまたはコマンドで別の方法で処理されない終了エラーが発生した場合、PowerShell は `trap` エラーを処理するステートメントを確認します。 `trap`ステートメントが存在する場合、PowerShell はステートメントでスクリプトまたはコマンドの実行を続行し `trap` ます。

次の例は、非常に単純な `trap` ステートメントです。

```powershell
trap {"Error found."}
```

この `trap` ステートメントは、終了エラーをトラップします。

次の例では、関数に、実行時エラーの原因となる文字列が含まれています。

```powershell
function TrapTest {
    trap {"Error found."}
    nonsenseString
}

TrapTest
```

この関数を実行すると、次の値が返されます。

```Output
Error found.
nonsenseString:
Line |
   3 |      nonsenseString
     |      ~~~~~~~~~~~~~~
     | The term 'nonsenseString' is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
```

次の例には、 `trap` 自動変数を使用してエラーを表示するステートメントが含まれてい `$_` ます。

```powershell
function TrapTest {
    trap {"Error found: $_"}
    nonsenseString
}

TrapTest
```

このバージョンの関数を実行すると、次の値が返されます。

```Output
Error found: The term 'nonsenseString' is not recognized as the name of a
cmdlet, function, script file, or operable program. Check the spelling of the
name, or if a path was included, verify that the path is correct and try again.
nonsenseString:
Line |
   3 |      nonsenseString
     |      ~~~~~~~~~~~~~~
     | The term 'nonsenseString' is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
```

> [!IMPORTANT]
> `trap` ステートメントは、特定のスコープ内の任意の場所で定義できますが、常にそのスコープ内のすべてのステートメントに適用されます。 実行時に、 `trap` ブロック内のステートメントは、他のステートメントが実行される前に定義されます。 JavaScript では、これは [hoisting](https://wikipedia.org/wiki/JavaScript_syntax#hoisting)と呼ばれます。 つまり、ステートメントは、 `trap` 定義された時点を超えて実行が進んでいない場合でも、そのブロック内のすべてのステートメントに適用されます。 たとえば、 `trap` スクリプトの末尾にを定義し、最初のステートメントでエラーをスローした場合でも、そのがトリガーさ `trap` れます。

### <a name="trapping-specific-errors"></a>特定のエラーのトラップ

スクリプトまたはコマンドには、複数のステートメントを含めることができ `trap` ます。 は、 `trap` 特定のエラーを処理するように定義できます。

次の例は、 `trap` 特定のエラー **CommandNotFoundException** をトラップするステートメントです。

```powershell
trap [System.Management.Automation.CommandNotFoundException]
    {"Command error trapped"}
```

関数またはスクリプトで、既知のコマンドと一致しない文字列が検出されると、この `trap` ステートメントは "command error トラップ" という文字列を表示します。
ステートメントの一覧を実行 `trap` すると、PowerShell によってエラーオブジェクトがエラーストリームに書き込まれ、スクリプトが続行されます。

PowerShell は .NET の例外の種類を使用します。 次の例では、 **system.exception** エラーの種類を指定します。

```powershell
trap [System.Exception] {"An error trapped"}
```

**CommandNotFoundException** エラーの種類は、 **system.exception** 型から継承されます。 このステートメントは、不明なコマンドによって作成されたエラーをトラップします。 また、他のエラーの種類をトラップします。

1つのスクリプトに複数のステートメントを含めることができ `trap` ます。 各エラーの種類は、1つのステートメントでのみトラップでき `trap` ます。 終了エラーが発生すると、PowerShell は、 `trap` 現在の実行スコープを開始位置として、最も限定的な一致を持つを検索します。

次のスクリプトの例には、エラーが含まれています。 このスクリプトには、 `trap` 終了エラーをトラップする一般的なステートメントと、 `trap` **CommandNotFoundException** 型を指定する特定のステートメントが含まれています。

```powershell
trap {"Other terminating error trapped" }
trap [System.Management.Automation.CommandNotFoundException] {
  "Command error trapped"
}
nonsenseString
```

このスクリプトを実行すると、次の結果が生成されます。

```Output
Command error trapped
nonsenseString:
Line |
   5 |  nonsenseString
     |  ~~~~~~~~~~~~~~
     | The term 'nonsenseString' is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
```

PowerShell はコマンドレットまたはその他の項目として "nonsenseString" を認識しないため、 **CommandNotFoundException** エラーが返されます。 この終了エラーは、特定のステートメントによってトラップされ `trap` ます。

次のスクリプトの例では、同じステートメントに別のエラーが含まれてい `trap` ます。

```powershell
trap {"Other terminating error trapped" }
trap [System.Management.Automation.CommandNotFoundException]
    {"Command error trapped"}
1/$null
```

このスクリプトを実行すると、次の結果が生成されます。

```Output
Other terminating error trapped
RuntimeException:
Line |
   4 |  1/$null
     |  ~~~~~~~
     | Attempted to divide by zero.
```

0で除算しようとしても、 **CommandNotFoundException** エラーは発生しません。 代わりに、そのエラーは、終了エラーをトラップするもう1つのステートメントによってトラップされ `trap` ます。

### <a name="trapping-errors-and-scope"></a>エラーとスコープのトラップ

ステートメントと同じスコープ内で終了エラーが発生した場合 `trap` 、PowerShell はによって定義されたステートメントの一覧を実行 `trap` します。 実行は、エラーの後のステートメントから続行されます。 `trap`ステートメントがエラーとは別のスコープにある場合、ステートメントと同じスコープ内にある次のステートメントから実行が続行され `trap` ます。

たとえば、関数でエラーが発生し、 `trap` ステートメントが関数内にある場合、スクリプトは次のステートメントで続行されます。 次のスクリプトには、エラーとステートメントが含まれてい `trap` ます。

```powershell
function function1 {
    trap { "An error: " }
    NonsenseString
    "function1 was completed"
}

function1
```

このスクリプトを実行すると、次の結果が生成されます。

```Output
An error:
NonsenseString:
Line |
   3 |      NonsenseString
     |      ~~~~~~~~~~~~~~
     | The term 'NonsenseString' is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
function1 was completed
```

`trap`関数内のステートメントは、エラーをトラップします。 メッセージを表示すると、PowerShell によって関数の実行が再開されます。 が完了したことに注意 `Function1` してください。

これを次の例と比較します。この例では、同じエラーとステートメントが使用されてい `trap` ます。 この例では、 `trap` ステートメントが関数の外側にあります。

```powershell
function function2 {
    NonsenseString
    "function2 was completed"
}

trap { "An error: " }

function2
```

関数を実行する `Function2` と、次の結果が生成されます。

```Output
An error:
NonsenseString:
Line |
   2 |      NonsenseString
     |      ~~~~~~~~~~~~~~
     | The term 'NonsenseString' is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
```

この例では、"function2 was completed" コマンドは実行されませんでした。 どちらの例でも、関数内で終了エラーが発生します。 ただし、この例では、 `trap` ステートメントは関数の外にあります。 PowerShell は、ステートメントの実行後に関数に戻りません `trap` 。

> [!CAUTION]
> 同じエラー条件に対して複数のトラップが定義されている場合、最初に `trap` 定義された構文 (スコープ内で最も高いもの) が使用されます。

次の例では、"そのよう `trap` な ops 1" を持つのみが実行されます。

```powershell
Remove-Item -ErrorAction Stop ThisFileDoesNotExist
trap { "whoops 1"; continue }
trap { "whoops 2"; continue }
```

> [!IMPORTANT]
> トラップステートメントは、コンパイルされる場所にスコープが設定されています。 関数またはドットのソース化されたスクリプト内にステートメントがある場合 `trap` 、関数またはドットのソース化されたスクリプトが終了すると、内のすべての `trap` ステートメントが削除されます。

### <a name="using-the-break-and-continue-keywords"></a>Break および continue キーワードの使用

`break`ステートメントでキーワードとキーワードを使用すると、 `continue` `trap` 終了エラー後もスクリプトまたはコマンドを実行し続けるかどうかを判断できます。

ステートメントを `break` ステートメントリストに含めた場合 `trap` 、PowerShell は関数またはスクリプトを停止します。 次のサンプル関数では、ステートメントでキーワードを使用してい `break` `trap` ます。

```powershell
function break_example {
    trap {
        "Error trapped"
        break
    }
    1/$null
    "Function completed."
}

break_example
```

```Output
Error trapped
ParentContainsErrorRecordException:
Line |
   6 |      1/$null
     |      ~~~~~~~
     | Attempted to divide by zero.
```

ステートメントに `trap` キーワードが含まれているため、 `break` 関数は実行を続行せず、"関数は完了しました" 行は実行されません。

`continue`ステートメントにキーワードを含めると `trap` 、またはを使用しない場合と同様に、エラーの原因となったステートメントの後に PowerShell が再開され `break` `continue` ます。 ただし、キーワードを使用すると、エラー `continue` ストリームにエラーが書き込まれません。

次のサンプル関数では、ステートメントでキーワードを使用してい `continue` `trap` ます。

```powershell
function continue_example {
    trap {
        "Error trapped"
        continue
    }
    1/$null
    "Function completed."
}

continue_example
```

```Output
Error trapped
Function completed.
```

関数は、エラーがトラップされ、"関数が完了しました" ステートメントが実行された後に再開されます。 エラーストリームにはエラーが書き込まれません。

## <a name="notes"></a>メモ

`trap` ステートメントを使用すると、スコープ内のすべての終了エラーを広範囲に処理することができます。 より詳細なエラー処理を行うには、 `try` / `catch` ステートメントを使用してトラップが定義されているブロックを使用し `catch` ます。 ステートメントは、 `catch` 関連付けられているステートメント内のコードにのみ適用され `try` ます。 詳細については、「 [about_Try_Catch_Finally](about_Try_Catch_Finally.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Break](about_Break.md)

[about_Continue](about_Continue.md)

[about_Scopes](about_Scopes.md)

[about_Throw](about_Throw.md)

[about_Try_Catch_Finally](about_Try_Catch_Finally.md)
