---
ms.date: 09/13/2016
ms.topic: reference
title: ValidateRange 属性の宣言
description: ValidateRange 属性の宣言
ms.openlocfilehash: 1fec9d1bd36cd21b7f0f23bf6d72338d276dce91
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660603"
---
# <a name="validaterange-attribute-declaration"></a>ValidateRange 属性の宣言

ValidateRange 属性は、コマンドレットパラメーター引数の最小値と最大値 (範囲) を指定します。 この属性は、Windows PowerShell の関数でも使用できます。

## <a name="syntax"></a>構文

```csharp
[ValidateRange(object minRange, object maxRange)]
```

#### <a name="parameters"></a>パラメーター

`MinRange` ([System.object](/dotnet/api/system.object)) が必要です。 許容される最小値を指定します。

`MaxRange` ([System.object](/dotnet/api/system.object)) が必要です。 許容される最大値を指定します。

## <a name="remarks"></a>解説

- パラメーターの値 `MinRange` がパラメーターの値よりも大きい場合、Windows PowerShell ランタイムは構築エラーをスローし `MaxRange` ます。

- Windows PowerShell ランタイムは、次の条件下で検証エラーをスローします。

  - 引数の値が制限よりも小さいか、または制限を超えている場合 `MinRange` `MaxRange` 。

  - 引数がおよびパラメーターと同じ型ではない場合 `MinRange` `MaxRange` 。

- ValidateRange 属性は、system.servicemodel [属性](/dotnet/api/System.Management.Automation.ValidateRangeAttribute) クラスによって定義されます。

## <a name="see-also"></a>参照

[System. Automation. Validaterangeattribute](/dotnet/api/System.Management.Automation.ValidateRangeAttribute)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
