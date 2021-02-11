---
description: 、、、、 `foreach` `for` `while` `do` `switch` 、またはステートメントをすぐに終了するために使用できるステートメントについて説明し `trap` ます。
Locale: en-US
ms.date: 06/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_break?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Break
ms.openlocfilehash: 3c418f5bae11f957880c8b53ee0bcf2fb44515ee
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602422"
---
# <a name="about-break"></a>中断について

## <a name="short-description"></a>簡単な説明

、、、、 `foreach` `for` `while` `do` `switch` 、またはステートメントをすぐに終了するために使用できるステートメントについて説明し `trap` ます。

## <a name="long-description"></a>長い説明

ステートメントは、 `break` 現在のコントロールブロックを終了する方法を提供します。
実行は、コントロールブロックの後の次のステートメントから続行されます。 ステートメントでは、ラベルがサポートされています。 ラベルは、スクリプト内のステートメントに割り当てる名前です。

## <a name="using-break-in-loops"></a>Break in ループの使用

`break`ループ内にステートメントが含まれている場合 ( `foreach` 、 `for` 、 `do` 、またはループなど) `while` 、PowerShell はすぐにループを終了します。

ステートメントには、 `break` 埋め込みループを終了できるラベルを含めることができます。 ラベルには `foreach` 、スクリプト内で任意の loop キーワード (、、など) を指定でき `for` `while` ます。

ステートメントを使用してステートメントを終了する方法を次の例に示し `break` `for` ます。

```powershell
for($i=1; $i -le 10; $i++) {
   Write-Host $i
   break
}
```

この例では、 `break` `for` 変数が1と等しい場合、ステートメントはループを終了し `$i` ます。 `for`が10を超えるまでステートメントが **True** と評価された場合でも `$i` 、最初に `for` ループが実行されたときに PowerShell は break ステートメントに到達します。

`break`内部条件を満たす必要があるループでは、ステートメントを使用するのが一般的です。 次のステートメントの例を考えてみましょう `foreach` 。

```powershell
$i=0
$varB = 10,20,30,40
foreach ($val in $varB) {
  if ($val -eq 30) {
    break
  }
  $i++
}
Write-Host "30 was found in array index $i"
```

この例では、 `foreach` ステートメントは配列を反復処理し `$varB` ます。 ステートメントは、 `if` ループの最初の2回の実行時に False と評価され、変数 `$i` は1ずつインクリメントされます。 ループが3回実行された場合、は `$i` 2 に `$val` なり、変数は30と等しくなります。 この時点で、ステートメントが実行され、 `break` `foreach` ループが終了します。

### <a name="using-a-labeled-break-in-a-loop"></a>ループ内でラベル付きの改行を使用する

ステートメントには `break` 、ラベルを含めることができます。 `break`キーワードをラベルと共に使用すると、PowerShell は、現在のループを終了するのではなく、ラベルが付けられたループを終了します。
ラベルは、コロンの後に割り当てた名前を付けたものです。 ラベルは、ステートメント内の最初のトークンである必要があります。また、のようなループキーワードが続く必要があり `while` ます。

`break` 実行を、ラベルが付けられたループの外に移動します。 埋め込みループでは、 `break` キーワードが単独で使用される場合とは異なる結果になります。 この例では、ステートメントにステートメントが含まれ `while` てい `for` ます。

```powershell
:myLabel while (<condition 1>) {
  for ($item in $items) {
    if (<condition 2>) {
      break myLabel
    }
    $item = $x   # A statement inside the For-loop
  }
}
$a = $c  # A statement after the labeled While-loop
```

Condition 2 が **True** と評価された場合、スクリプトの実行は、ラベル付けされたループの後のステートメントまでスキップされます。 この例では、ステートメントを使用して実行を再び開始し `$a = $c` ます。

次の例に示すように、多数のラベル付きループを入れ子にすることができます。

```powershell
:red while (<condition1>) {
  :yellow while (<condition2>) {
    while (<condition3>) {
      if ($a) {break}
      if ($b) {break red}
      if ($c) {break yellow}
    }
    Write-Host "After innermost loop"
  }
  Write-Host "After yellow loop"
}
Write-Host "After red loop"
```

変数が `$b` True と評価されると、"red" というラベルが付いたループの後にスクリプトの実行が再開されます。 変数が `$c` True と評価された場合、スクリプトコントロールの実行は、"黄色" というラベルが付いたループの後に再開されます。

変数が True と評価されると `$a` 、最も内側のループの後に実行が再開されます。 ラベルは必要ありません。

PowerShell では、ラベルの実行を再開する距離を制限しません。 ラベルは、スクリプトと関数呼び出しの境界を越えて制御を渡すこともできます。

## <a name="using-break-in-a-switch-statement"></a>Switch ステートメントでの break の使用

コンストラクトでは `switch` 、 `break` PowerShell によってコードブロックが終了さ `switch` れます。

キーワードは、 `break` コンストラクトを離れるために使用され `switch` ます。 たとえば、次のステートメントでは、 `switch` ステートメントを使用して、 `break` 最も具体的な条件をテストしています。

```powershell
$var = "word2"
switch -regex ($var) {
    "word2" {
      Write-Host "Exact" $_
      break
    }

    "word.*" {
      Write-Host "Match on the prefix" $_
      break
    }

    "w.*" {
      Write-Host "Match on at least the first letter" $_
      break
    }

    default {
      Write-Host "No match" $_
      break
    }
}
```

この例では、 `$var` 変数が作成され、文字列値に初期化され `word2` ます。 このステートメントでは、Regex クラスを使用して、 `switch` 変数の値を最初に用語と照合し `word2` ます。 変数の値とステートメント内の最初のテストが一致するため `switch` 、ステートメント内の最初のコードブロックが実行され `switch` ます。

PowerShell が最初のステートメントに到達すると、 `break` `switch` ステートメントは終了します。 この例から4つのステートメントを削除すると `break` 、4つの条件がすべて満たされます。 この例では、ステートメントを使用して `break` 、最も具体的な条件が満たされたときに結果を表示します。

## <a name="using-break-in-a-trap-statement"></a>トラップステートメントでの break の使用

ステートメントの本体で最後に実行されたステートメントがの場合は `trap` `break` 、エラーオブジェクトが抑制され、例外が再スローされます。

次の例では、ステートメントを使用してトラップされる **DivideByZeroException** 例外を作成し `trap` ます。

```powershell
function test {
  trap [DivideByZeroException] {
    Write-Host 'divide by zero trapped'
    break
  }

  $i = 3
  'Before loop'
  while ($true) {
     "1 / $i = " + (1 / $i--)
  }
  'After loop'
}
test
```

例外で実行が停止することに注意してください。 に `After loop` 到達していません。
を実行すると、例外が再スローされ `trap` ます。

```Output
Before loop
1 / 3 = 0.333333333333333
1 / 2 = 0.5
1 / 1 = 1
divide by zero trapped
ParentContainsErrorRecordException:
Line |
  10 |       "1 / $i = " + (1 / $i--)
     |       ~~~~~~~~~~~~~~~~~~~~~~~~
     | Attempted to divide by zero.
```

## <a name="do-not-use-break-outside-of-a-loop-switch-or-trap"></a>ループ、スイッチ、またはトラップの外側では中断を使用しない

`break`を直接サポートするコンストラクト (ループ、、) の外部でを使用した場合 `switch` `trap` 、PowerShell は外側の構造体の _呼び出し履歴_ を検索します。 外側のコンストラクトが見つからない場合、現在の実行空間は自動的に終了します。

これは、それをサポートする外側のコンストラクトの外部でを使用する関数とスクリプト `break` が誤って _呼び出し元_ を終了する可能性があることを意味します。

パイプライン内でを使用する `break` `break` と、 `ForEach-Object` パイプラインが終了するだけでなく、実行空間全体が終了する可能性があります。

## <a name="see-also"></a>関連項目

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Continue](about_Continue.md)

[about_For](about_For.md)

[about_Foreach](about_Foreach.md)

[about_Switch](about_Switch.md)

[about_Throw](about_Throw.md)

[about_Trap](about_Trap.md)

[about_Try_Catch_Finally](about_Try_Catch_Finally.md)

[about_While](about_While.md)
