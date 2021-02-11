---
ms.date: 09/13/2016
ms.topic: reference
title: ValidateCount 属性の宣言
description: ValidateCount 属性の宣言
ms.openlocfilehash: 6acdd02a10ecc1bc2be0e6be88cf2f42a3673eb8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646264"
---
# <a name="validatecount-attribute-declaration"></a>ValidateCount 属性の宣言

ValidateCount 属性では、コマンドレットパラメーターに許可される引数の最小数と最大数を指定します。

## <a name="syntax"></a>構文

```csharp
[ValidateCount(int minLength, int maxlength)]
```

#### <a name="parameters"></a>パラメーター

`MinLength` ([System.string][]) が必要です。 引数の最小数を指定します。

`MaxLength`([System.string][]) が必要です。 引数の最大数を指定します。

## <a name="remarks"></a>解説

- この属性を宣言する方法の詳細については、「 [引数カウントを検証する方法][]」を参照してください。

- この属性が呼び出されない場合、対応するコマンドレットパラメーターには任意の数の引数を指定できます。

- Windows PowerShell ランタイムは、次の状況でエラーをスローします。

  - `MinLength`属性と `MaxLength` 属性のパラメーターが、system.string [][]型ではありません。

  - 属性パラメーターの値 `MaxLength` が、属性パラメーターの値未満です `MinLength` 。

- ValidateCount 属性は、 [system.string クラスに][] よって定義されています。

## <a name="see-also"></a>参照

[System. Automation. ValidateCountAttribute][]

[引数カウントを検証する方法][]

[Windows PowerShell コマンドレットの記述][]

[引数カウントを検証する方法]: how-to-validate-an-argument-count.md
[Windows PowerShell コマンドレットの記述]: writing-a-windows-powershell-cmdlet.md

[System.Int32]: /dotnet/api/System.Int32
[System. Automation. ValidateCountAttribute]: /dotnet/api/System.Management.Automation.ValidateCountAttribute
