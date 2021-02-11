---
description: While または Until 条件に従って、ステートメントリストを1回以上実行します。
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_do?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Do
ms.openlocfilehash: 412a13669e86df5804dd74d75fcb5b4389192c79
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602788"
---
# <a name="about-do"></a>作業の概要

## <a name="short-description"></a>概要
While または Until 条件に従って、ステートメントリストを1回以上実行します。

## <a name="long-description"></a>詳細説明

Do キーワードは、While キーワードまたは Until キーワードを使用して、スクリプトブロック内でステートメントを実行します。このとき、条件が適用されます。 関連する While ループとは異なり、Do ループ内のスクリプトブロックは常に少なくとも1回は実行されます。

**Do while** ループは、さまざまな while ループです。 **Do while** ループでは、スクリプトブロックの実行後に条件が評価されます。 While ループと同様に、条件が true と評価される限り、スクリプトブロックが繰り返されます。

**Do while** ループと同様に、 **do until** ループは、条件が評価される前に常に少なくとも1回は実行されます。 ただし、スクリプトブロックは、条件が false のときにのみ実行されます。

*Continue* および *Break* flow 制御キーワードは、 **Do While** ループまたは **do until** ループで使用できます。

### <a name="syntax"></a>構文

次に、 **Do While** ステートメントの構文を示します。

```powershell
do {<statement list>} while (<condition>)
```

次に、 **Do Until** ステートメントの構文を示します。

```powershell
do {<statement list>} until (<condition>)
```

ステートメントの一覧には、ループが入力または繰り返されるたびに実行されるステートメントが1つ以上含まれています。

ステートメントの条件部分は、true または false に解決されます。

### <a name="example"></a>例

次の Do ステートメントの例では、値が0の項目に到達するまで配列内の項目をカウントします。

```powershell
C:\PS> $x = 1,2,78,0
C:\PS> do { $count++; $a++; } while ($x[$a] -ne 0)
C:\PS> $count
3
```

次の例では、Until キーワードを使用します。 不等号演算子 ( `-ne` ) が equal to 演算子 () に置き換えられていることに注意して `-eq` ください。

```powershell
C:\PS> $x = 1,2,78,0
C:\PS> do { $count++; $a++; } until ($x[$a] -eq 0)
C:\PS> $count
3
```

次の例では、0未満の値をすべてスキップして、配列のすべての値を書き込みます。

```powershell
do {
  if ($x[$a] -lt 0) { continue }
  Write-Host $x[$a]
}
while (++$a -lt 10)
```

## <a name="see-also"></a>関連項目

[about_While](about_While.md)

[about_Operators](about_Operators.md)

[about_Assignment_Operators](about_Assignment_Operators.md)

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Break](about_Break.md)

[about_Continue](about_Continue.md)

