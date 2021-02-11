---
description: 結合演算子 (-join) が複数の文字列を1つの文字列に結合する方法について説明します。
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_join?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Join
ms.openlocfilehash: d76e95aaeca1b5b94bb1a2216576a985ffb209c0
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601196"
---
# <a name="about-join"></a>結合について

## <a name="short-description"></a>概要
結合演算子 (-join) が複数の文字列を1つの文字列に結合する方法について説明します。

## <a name="long-description"></a>詳細説明

Join 操作は、一連の文字列を連結して1つの文字列にします。 文字列は、コマンドに出現する順序で、結果の文字列に追加されます。

### <a name="syntax"></a>構文

次の図は、結合演算子の構文を示しています。

```powershell
-Join <String[]>
<String[]> -Join <Delimiter>
```

#### <a name="parameters"></a>パラメーター

String []-結合する1つ以上の文字列を指定します。

Delimiter-連結された文字列の間に配置される1つ以上の文字を指定します。 既定値は区切り記号 ("") ではありません。

解説

単項結合演算子 (-join <string [] >) は、コンマよりも優先順位が高くなります。 その結果、コンマ区切りの文字列リストを単項結合演算子に送信した場合、最初の文字列 (最初のコンマの前) だけが結合演算子に送信されます。

単項結合演算子を使用するには、文字列をかっこで囲みます。または、変数に文字列を格納し、結合する変数を送信します。

次に例を示します。

```powershell
-join "a", "b", "c"
a
b
c

-join ("a", "b", "c")
abc

$z = "a", "b", "c"
-join $z
abc
```

### <a name="examples"></a>例

次のステートメントは、3つの文字列を結合します。

```powershell
-join ("Windows", "PowerShell", "2.0")
WindowsPowerShell2.0
```

次のステートメントは、スペースで区切られた3つの文字列を結合します。

```powershell
"Windows", "PowerShell", "2.0" -join " "
Windows PowerShell 2.0
```

次のステートメントでは、3つの文字列を結合するために複数の文字の区切り記号を使用しています。

```powershell
$a = "WIND", "S P", "ERSHELL"
$a -join "OW"
WINDOWS POWERSHELL
```

次のステートメントは、ここで指定した文字列内の行を1つの文字列に結合します。 ここでは文字列が1つの文字列であるため、この文字列内の行は、結合する前に分割する必要があります。 このメソッドを使用すると、次の文字列に保存されている XML ファイル内の文字列を再度参加させることができます。

```powershell
$a = @'
a
b
c
'@

(-split $a) -join " "
a b c
```

## <a name="see-also"></a>関連項目

[about_Operators](about_Operators.md)

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Split](about_Split.md)

