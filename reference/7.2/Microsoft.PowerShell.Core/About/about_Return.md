---
description: 現在の範囲 (関数、スクリプト、スクリプト ブロック) を終了します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_return?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Return
ms.openlocfilehash: 032f3c694096e11e2800be941eb76b1f442b5088
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602646"
---
# <a name="about-return"></a>戻り値

## <a name="short-description"></a>簡単な説明

現在の範囲 (関数、スクリプト、スクリプト ブロック) を終了します。

## <a name="long-description"></a>長い説明

キーワードは、 `return` 関数、スクリプト、またはスクリプトブロックを終了します。 特定のポイントでスコープを終了したり、値を返したり、スコープの末尾に達したことを示すために使用できます。

C や C などの言語に慣れているユーザーは、キーワードを使用して、 \# `return` スコープを明示的にするロジックを作成することをお勧めします。

PowerShell では、Return キーワードを含むステートメントを使用せずに、各ステートメントの結果が出力として返されます。 C や C などの言語では、 \# キーワードによって指定された値だけが返され `return` ます。

> [!NOTE]
> PowerShell 5.0 以降では、正式な構文を使用して、クラスを定義するための言語が追加されました。  PowerShell クラスのコンテキストでは、ステートメントを使用して指定したものを除き、メソッドからの出力は何もありません `return` 。 PowerShell クラスの詳細については、 [about_Classes](about_Classes.md)を参照してください。

### <a name="syntax"></a>構文

キーワードの構文 `return` は次のとおりです。

```
return [<expression>]
```

キーワードは、 `return` 単独で使用することも、次のように値または式の後に指定することもできます。

```powershell
return
return $a
return (2 + $a)
```

### <a name="examples"></a>例

次の例では、キーワードを使用して、 `return` 条件が満たされた場合に特定のポイントで関数を終了します。 ステートメントを実行する前に return ステートメントが終了するため、奇数は乗算されません。

```powershell
function MultiplyEven
{
    param($number)

    if ($number % 2) { return "$number is not even" }
    $number * 2
}

1..10 | ForEach-Object {MultiplyEven -Number $_}
```

```output
1 is not even
4
3 is not even
8
5 is not even
12
7 is not even
16
9 is not even
20
```

PowerShell では、キーワードが使用されていない場合でも値を返すことができ `return` ます。
各ステートメントの結果が返されます。 たとえば、次のステートメントは変数の値を返し `$a` ます。

```powershell
$a
return
```

次のステートメントでも、の値が返され `$a` ます。

```powershell
return $a
```

次の例には、関数が計算を実行していることをユーザーに知らせるためのステートメントが含まれています。

```powershell
function calculation {
    param ($value)

    "Please wait. Working on calculation..."
    $value += 73
    return $value
}

$a = calculation 14
```

"しばらくお待ちください。 計算時に作業しています... "文字列は表示されません。 代わりに、次の例のように、変数に割り当てられ `$a` ます。

```
PS> $a
Please wait. Working on calculation...
87
```

情報文字列と計算結果の両方が関数によって返され、変数に代入され `$a` ます。

PowerShell 5.0 以降で、関数内にメッセージを表示する場合は、ストリームを使用でき `Information` ます。 次のコードでは、コマンドレットと Continue を使用して、上記の例を修正して `Write-Information` `InformationAction` います。 

```powershell
function calculation {
    param ($value)

    Write-Information "Please wait. Working on calculation..." -InformationAction Continue
    $value += 73
    return $value
}

$a = calculation 14
```

```output
Please wait. Working on calculation...
C:\PS> $a
87
```

### <a name="return-values-and-the-pipeline"></a>戻り値とパイプライン

スクリプトブロックまたは関数からコレクションを返すと、PowerShell によって自動的にメンバーのロールが解除され、パイプラインを介して一度に1つずつ渡されます。 これは、PowerShell の1回限りの処理が原因です。 詳細については、「 [about_pipelines](about_pipelines.md)」を参照してください。

この概念は、数値の配列を返す次のサンプル関数によって示されています。 関数からの出力は、 `Measure-Object` パイプライン内のオブジェクトの数をカウントするコマンドレットにパイプ処理されます。

```powershell
function Test-Return
{
    $array = 1,2,3
    return $array
}
Test-Return | Measure-Object
```

```Output
Count    : 3
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

スクリプトブロックまたは関数が、コレクションを1つのオブジェクトとしてパイプラインに返すように強制するには、次の2つの方法のいずれかを使用します。

- 単項配列式

  単項式を使用すると、次の例に示すように、戻り値を1つのオブジェクトとして渡すことができます。

  ```powershell
  function Test-Return
  {
      $array = 1,2,3
      return (, $array)
  }
  Test-Return | Measure-Object
  ```

  ```Output
  Count    : 1
  Average  :
  Sum      :
  Maximum  :
  Minimum  :
  Property :
  ```

- `Write-Output`**Noenumerate** パラメーターを使用します。

  `Write-Output` **Noenumerate** パラメーターを指定してコマンドレットを使用することもできます。 次の例では、コマンドレットを使用し `Measure-Object` て、サンプル関数からパイプラインに送信されたオブジェクトをキーワードでカウントし `return` ます。

  ```powershell
  function Test-Return
  {
      $array = 1, 2, 3
      return Write-Output -NoEnumerate $array
  }

  Test-Return | Measure-Object
  ```

  ```Output
  Count    : 1
  Average  :
  Sum      :
  Maximum  :
  Minimum  :
  Property :
  ```

## <a name="see-also"></a>関連項目

[about_Language_Keywords](about_Language_Keywords.md)

[about_Functions](about_Functions.md)

[about_Scopes](about_Scopes.md)

[about_Classes](about_Classes.md)

[Write-Information](xref:Microsoft.PowerShell.Utility.Write-Information)

[about_Script_Blocks](about_Script_Blocks.md)

