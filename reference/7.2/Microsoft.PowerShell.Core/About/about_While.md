---
description: 条件テストの結果に基づいてコマンドブロックを実行するために使用できる言語ステートメントについて説明します。
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_while?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_While
ms.openlocfilehash: 37c277a5919ba5fc39f953e05edaf58d159f3e7c
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600591"
---
# <a name="about-while"></a>しばらく

## <a name="short-description"></a>概要
条件テストの結果に基づいてコマンドブロックを実行するために使用できる言語ステートメントについて説明します。

## <a name="long-description"></a>詳細説明

While ステートメント (While ループとも呼ばれます) は、条件テストが true と評価されている限り、コマンドブロックでコマンドを実行するループを作成するための言語構成要素です。 While ステートメントは、構文が複雑になるので、For ステートメントよりも簡単に作成できます。 さらに、While ステートメントで条件テストを指定すると、ループの実行回数を制御できるため、Foreach ステートメントよりも柔軟です。

While ステートメントの構文を次に示します。

```powershell
while (<condition>){<statement list>}
```

While ステートメントを実行すると、 `<condition>` セクションに入る前に、PowerShell によってステートメントのセクションが評価され `<statement list>` ます。 ステートメントの条件部分は、true または false のいずれかに解決されます。 条件が true のままである限り、PowerShell はセクションを再適用し `<statement list>` ます。

`<statement list>`ステートメントのセクションには、ループが入力または繰り返されるたびに実行される1つ以上のコマンドが含まれています。

たとえば、次の While ステートメントでは、$val 変数が作成されていない場合、または $val 変数が作成されて0に初期化された場合に、1 ~ 3 の数値が表示されます。

```powershell
while($val -ne 3)
{
    $val++
    Write-Host $val
}
```

この例では、$val \= 0、1、2の場合、条件 ($val は3と等しくありません) は true です。 ループを実行するたびに、 \+ \+ 単項インクリメント演算子 ($val) を使用して $val が1ずつインクリメントされ \+ \+ ます。 最後にループを実行したときは、$val \= 3 です。 $Val が3の場合、condition ステートメントは false と評価され、ループが終了します。

PowerShell コマンドプロンプトでこのコマンドを簡単に記述するには、次のように入力します。

```powershell
while($val -ne 3){$val++; Write-Host $val}
```

$Val の値をコンソールに書き込む2番目のコマンドから $val に1を追加する最初のコマンドがセミコロンで区切られていることに注意してください。

## <a name="see-also"></a>関連項目

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Do](about_Do.md)

[about_Foreach](about_Foreach.md)

[about_For](about_For.md)

[about_Language_Keywords](about_Language_Keywords.md)

