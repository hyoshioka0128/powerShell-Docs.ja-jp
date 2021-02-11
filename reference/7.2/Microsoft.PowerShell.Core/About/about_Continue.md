---
description: ステートメントがプログラム `continue` フロー、 `switch` ステートメント、またはステートメントの先頭にプログラムフローを直ちに返す方法について説明し `trap` ます。
ms.date: 06/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_continue?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Continue
ms.openlocfilehash: 36c14dc0489345083e8938c066cefa77e3f5ef8d
ms.sourcegitcommit: 0c31814bed14ff715dc7d4aace07cbdc6df2438e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "99601215"
---
# <a name="about-continue"></a>続行について

## <a name="short-description"></a>簡単な説明

ステートメントがプログラム `continue` フロー、 `switch` ステートメント、またはステートメントの先頭にプログラムフローを直ちに返す方法について説明し `trap` ます。

## <a name="long-description"></a>長い説明

ステートメントは、を `continue` 完全に終了するのではなく、現在のコントロールブロックを終了し、実行を継続する方法を提供します。 ステートメントでは、ラベルがサポートされています。
ラベルは、スクリプト内のステートメントに割り当てる名前です。

## <a name="using-continue-in-loops"></a>Continue in ループの使用

ラベル `continue` が付けられていないステートメントは `for` 、、、 `foreach` `do` 、またはステートメントによって制御される最も内側のループの一番上にプログラムフローを直ちに返し `while` ます。 ループの現在の反復処理が終了し、ループは次の反復処理で続行されます。

次の例では、 `while` `$ctr` 変数が5に等しい場合、プログラムフローがループの先頭に戻ります。 結果として、1から10までのすべての数値が表示されます (5 を除く)。

```powershell
while ($ctr -lt 10)
{
    $ctr += 1
    if ($ctr -eq 5)
    {
        continue
    }

    Write-Host -Object $ctr
}
```

ループを使用すると、ステートメントで実行が続行され、 `for` `<Repeat>` その後にテストが続き `<Condition>` ます。 次の例では、キーワードの後にのデクリメントが発生するため、無限ループは発生しません `$i` `continue` 。

```powershell
#   <Init>  <Condition> <Repeat>
for ($i = 0; $i -lt 10; $i++)
{
    Write-Host -Object $i
    if ($i -eq 5)
    {
        continue
        # Will not result in an infinite loop.
        $i--
    }
}
```

### <a name="using-a-labeled-continue-in-a-loop"></a>ループ内でのラベル付き continue の使用

ラベルが付けられたステートメントは、 `continue` イテレーションの実行を終了し、対象となる対象のイテレーションまたはステートメントのラベルに制御を転送し `switch` ます。

次の例では、が True の場合、最も内側のが終了し、の `for` `$condition` 2 番目のループで反復処理が続行され `for` `labelB` ます。

```powershell
:labelA for ($i = 1; $i -le 10; $i++) {
    :labelB for ($j = 1; $j -le 10; $j++) {
        :labelC for ($k = 1; $k -le 10; $k++) {
            if ($condition) {
                continue labelB
            } else {
                $condition = Update-Condition
            }
        }
    }
}
```

## <a name="using-continue-in-a-switch-statement"></a>Switch ステートメントで continue を使用する

内のラベル付けされ `continue` ていないステートメントは、 `switch` 現在の反復の実行 `switch` を終了し、の先頭に制御を移して `switch` 次の入力項目を取得します。

1つの入力項目がある場合 `continue` 、ステートメント全体が終了し `switch` ます。
入力がコレクションの場合は、 `switch` によって `switch` コレクションの各要素がテストされます。 は `continue` 現在の反復処理を終了し、は `switch` 次の要素に進みます。

```powershell
switch (1,2,3) {
  2 { continue }  # moves on to the next element, 3
  default { $_ }
}
```

```Output
1
3
```

## <a name="using-continue-in-a-trap-statement"></a>Trap ステートメントで continue を使用する

本体で実行された最後のステートメント `trap` がの場合 `continue` 、トラップされたエラーは警告なしで無視され、実行が発生したステートメントの直後にあるステートメントから実行が続行され `trap` ます。

## <a name="do-not-use-continue-outside-of-a-loop-switch-or-trap"></a>ループ、スイッチ、またはトラップの外側で [続行] を使用しない

`continue`を直接サポートするコンストラクト (ループ、、) の外部でを使用した場合 `switch` `trap` 、PowerShell は外側の構造体の _呼び出し履歴_ を検索します。 外側のコンストラクトが見つからない場合、現在の実行空間は自動的に終了します。

これは、それをサポートする外側のコンストラクトの外部でを使用する関数とスクリプトが `continue` 誤って _呼び出し元_ を終了する可能性があることを意味します。

パイプライン内でを使用する `continue` と、 `ForEach-Object` パイプラインが終了するだけでなく、実行空間全体が終了する可能性があります。

## <a name="see-also"></a>関連項目

[about_Break](about_Break.md)

[about_For](about_For.md)

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Throw](about_Throw.md)

[about_Trap](about_Trap.md)

[about_Try_Catch_Finally](about_Try_Catch_Finally.md)
