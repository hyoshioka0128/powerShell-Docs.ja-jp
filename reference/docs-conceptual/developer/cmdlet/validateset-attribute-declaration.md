---
ms.date: 09/13/2016
ms.topic: reference
title: ValidateSet 属性の宣言
description: ValidateSet 属性の宣言
ms.openlocfilehash: 7894d00561366ada492911e8147acbd8d3454a55
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660473"
---
# <a name="validateset-attribute-declaration"></a>ValidateSet 属性の宣言

ValidateSetAttribute 属性は、コマンドレットパラメーターの引数として使用可能な値のセットを指定します。 この属性は、Windows PowerShell の関数でも使用できます。

この属性を指定すると、Windows PowerShell ランタイムは、コマンドレットパラメーターの指定された引数が、指定された要素セット内の要素と一致するかどうかを判断します。 コマンドレットは、パラメーター引数が set 内の要素と一致する場合にのみ実行されます。 一致するものが見つからない場合は、Windows PowerShell ランタイムによってエラーがスローされます。

## <a name="syntax"></a>構文

```csharp
[ValidateSetAttribute(params string[] validValues)]
[ValidateSetAttribute(params string[] validValues, Named Parameters)]
```

#### <a name="parameters"></a>パラメーター

`ValidValues` ([System.string](/dotnet/api/System.String)) が必要です。 有効なパラメーター要素の値を指定します。 次のサンプルは、1つまたは複数の要素を指定する方法を示しています。

```csharp
[ValidateSetAttribute("Steve")]
[ValidateSetAttribute("Steve","Mary")]
```

`IgnoreCase` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 の既定値は、 `true` 大文字小文字を区別しないことを示します。 値をにすると `false` 、コマンドレットでは大文字と小文字が区別されます。

## <a name="remarks"></a>解説

- この属性は、パラメーターごとに1回だけ使用できます。

- パラメーター値が配列の場合は、配列のすべての要素が属性セットの要素と一致している必要があります。

- ValidateSetAttribute 属性は、 [ValidateSetAttribute](/dotnet/api/System.Management.Automation.ValidateSetAttribute) クラスによって定義されます。

## <a name="see-also"></a>参照

[Validatesetattribute (システム管理)](/dotnet/api/System.Management.Automation.ValidateSetAttribute)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
