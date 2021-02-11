---
ms.date: 09/13/2016
ms.topic: reference
title: 引数の長さを検証する方法
description: 引数の長さを検証する方法
ms.openlocfilehash: 460aedbe6847033f976cb7bf70b6c77ac5a3a3c9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652622"
---
# <a name="how-to-validate-the-argument-length"></a>引数の長さを検証する方法

この例では、コマンドレットを実行する前に、Windows PowerShell ランタイムがパラメーター引数の文字数 (長さ) を確認するために使用できる検証規則を指定する方法を示します。 この検証規則を設定するには、ValidateLength 属性を宣言します。

> [!NOTE]
> この属性を定義するクラスの詳細については、「 [Validatelengthattribute](/dotnet/api/System.Management.Automation.ValidateLengthAttribute)」を参照してください。

## <a name="to-validate-the-argument-length"></a>引数の長さを検証するには

- 次のコードに示すように Validate 属性を追加します。 この例では、引数の長さが 0 ~ 10 文字の長さであることを指定します。

    ```csharp
    [ValidateLength(0, 10)]
    [Parameter(Position = 0, Mandatory = true)]
    public string UserName
    {
      get { return userName; }
      set { userName = value; }
    }
    private string userName;
    ```

この属性を宣言する方法の詳細については、「 [ValidateLength Attribute 宣言](./validatelength-attribute-declaration.md)」を参照してください。

## <a name="see-also"></a>参照

[ValidateLength 属性の宣言](./validatelength-attribute-declaration.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
