---
description: PowerShell のステートメントを接続する演算子について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_logical_operators?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Logical_Operators
ms.openlocfilehash: 8602551f3ecf74027b59715e1f47aab1b10bea92
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605167"
---
# <a name="about_logical_operators"></a>about_Logical_Operators

## <a name="short-description"></a>概要
PowerShell のステートメントを接続する演算子について説明します。

## <a name="long-description"></a>詳細説明

PowerShell 論理演算子は、式とステートメントを接続します。これにより、1つの式を使用して複数の条件をテストできます。

たとえば、次のステートメントでは、and 演算子と or 演算子を使用して、3つの条件付きステートメントを接続しています。 $A の値が $b の値より大きく、$a または $b がより小さい場合にのみ、ステートメントが true になります。
20.

```powershell
($a -gt $b) -and (($a -lt 20) -or ($b -lt 20))
```

PowerShell では、次の論理演算子がサポートされています。

|演算子|説明                        |例                   |
|--------|-----------------------------------|--------------------------|
|`-and`  |論理 AND。 両方の場合に TRUE        |`(1 -eq 1) -and (1 -eq 2)`|
|        |ステートメントは TRUE です。               |`False`                   |
|`-or`   |論理 OR。 どちらかの場合は TRUE       |`(1 -eq 1) -or (1 -eq 2)` |
|        |ステートメントが TRUE です。                 |`True`                    |
|`-xor`  |論理排他的 OR。 TRUE の場合    |`(1 -eq 1) -xor (2 -eq 2)`|
|        |1つのステートメントのみ TRUE         |`False`                   |
|`-not`  |論理 not。 ステートメントを否定します。 |`-not (1 -eq 1)`          |
|        |次のようになります。                      |`False`                   |
|`!`     |`-not` と同じ                     |`!(1 -eq 1)`              |
|        |                                   |`False`                   |

 注:

前の例でも、等値比較演算子が使用されて `-eq` います。 詳細については、「 [about_Comparison_Operators](about_Comparison_Operators.md)」を参照してください。 この例では、整数のブール値も使用します。 整数0の値は FALSE です。 その他のすべての整数の値は TRUE です。

論理演算子の構文は次のとおりです。

```
<statement> {-AND | -OR | -XOR} <statement>
{! | -NOT} <statement>
```

論理演算子を使用するステートメントは、ブール値 (TRUE または FALSE) を返します。

PowerShell 論理演算子は、ステートメントの実際の値を決定するために必要なステートメントのみを評価します。 And 演算子を含むステートメントの左オペランドが FALSE の場合、右オペランドは評価されません。
ステートメントまたはステートメントが含まれているステートメントの左オペランドが TRUE の場合、右オペランドは評価されません。 このため、ステートメントを使用する場合と同じ方法でこれらのステートメントを使用でき `If` ます。

## <a name="see-also"></a>関連項目

[about_Operators](about_Operators.md)

[Compare-Object](xref:Microsoft.PowerShell.Utility.Compare-Object)

[about_Comparison_operators](about_Comparison_Operators.md)

[about_If](about_If.md)

