---
ms.date: 09/13/2016
ms.topic: reference
title: 引数範囲を検証する方法
description: 引数範囲を検証する方法
ms.openlocfilehash: 1c1c53d43350a38beb2193200de3bd6b689366a4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666928"
---
# <a name="how-to-validate-an-argument-range"></a>引数範囲を検証する方法

この例では、コマンドレットを実行する前に、Windows PowerShell ランタイムがパラメーター引数の最小値と最大値を確認するために使用できる検証規則を指定する方法を示します。 この検証規則を設定するには、ValidateRange 属性を宣言します。

> [!NOTE]
> この属性を定義するクラスの詳細については、「system.object [属性](/dotnet/api/System.Management.Automation.ValidateRangeAttribute)」を参照してください。

### <a name="to-validate-an-argument-range"></a>引数の範囲を検証するには

- 次のコードに示すように、ValidateRange 属性を追加します。 この例では、パラメーターに 0 ~ 5 の範囲を指定し `InputData` ます。

    ```csharp
    [ValidateRange(0, 5)]
    [Parameter(Position = 0, Mandatory = true)]
    public int InputData
    {
      get { return inputData; }
      set { inputData = value; }
    }
    private int inputData;
    ```

この属性を宣言する方法の詳細については、「 [ValidateRange Attribute 宣言](./validaterange-attribute-declaration.md)」を参照してください。

## <a name="see-also"></a>参照

[ValidateRange 属性の宣言](./validaterange-attribute-declaration.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
