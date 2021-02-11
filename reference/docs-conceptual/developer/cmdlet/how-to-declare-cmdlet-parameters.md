---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット パラメーターを宣言する方法
description: コマンドレット パラメーターを宣言する方法
ms.openlocfilehash: ed53f9788c9afb142b137e08966dff33551b9d0f
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667098"
---
# <a name="how-to-declare-cmdlet-parameters"></a>コマンドレット パラメーターを宣言する方法

これらの例では、名前付き、位置指定、必須、省略可能、およびスイッチのパラメーターを宣言する方法を示します。 これらの例では、パラメーターの別名を定義する方法も示しています。

## <a name="how-to-declare-a-named-parameter"></a>名前付きパラメーターを宣言する方法

- 次のコードに示すように、パブリックプロパティを定義します。 パラメーター属性を追加するときは、 `Position` 属性からキーワードを省略します。

    ```csharp
    [Parameter()]
    public string UserName
    {
      get { return userName; }
      set { userName = value; }
    }
    private string userName;
    ```

Parameter 属性の詳細については、「 [Parameter 属性の宣言](./parameter-attribute-declaration.md)」を参照してください。

## <a name="how-to-declare-a-positional-parameter"></a>位置指定パラメーターを宣言する方法

- 次のコードに示すように、パブリックプロパティを定義します。 パラメーター属性を追加するときに、 `Position` キーワードを引数の位置に設定します。 値が0の場合は、最初の位置を示します。

    ```csharp
    [Parameter(Position = 0)]
    public string UserName
    {
      get { return userName; }
      set { userName = value; }
    }
    private string userName;
    ```

Parameter 属性の詳細については、「 [Parameter 属性の宣言](./parameter-attribute-declaration.md)」を参照してください。

## <a name="how-to-declare-a-mandatory-parameter"></a>必須パラメーターを宣言する方法

- 次のコードに示すように、パブリックプロパティを定義します。 パラメーター属性を追加するときに、 `Mandatory` キーワードをに設定 `true` します。

    ```csharp
    [Parameter(Position = 0, Mandatory = true)]
    public string UserName
    {
      get { return userName; }
      set { userName = value; }
    }
    private string userName;
    ```

Parameter 属性の詳細については、「 [Parameter 属性の宣言](./parameter-attribute-declaration.md)」を参照してください。

## <a name="how-to-declare-an-optional-parameter"></a>省略可能なパラメーターを宣言する方法

- 次のコードに示すように、パブリックプロパティを定義します。 パラメーター属性を追加する場合は、キーワードを省略し `Mandatory` ます。

    ```csharp
    [Parameter(Position = 0)]
    public string UserName
    {
      get { return userName; }
      set { userName = value; }
    }
    private string userName;
    ```

## <a name="how-to-declare-a-switch-parameter"></a>スイッチパラメーターを宣言する方法

- パブリックプロパティを型 system.string として定義 [し、パラメーター](/dotnet/api/System.Management.Automation.SwitchParameter)属性を宣言します。

    ```csharp
    [Parameter(Position = 1)]
    public SwitchParameter GoodBye
    {
      get { return goodbye; }
      set { goodbye = value; }
    }
    private bool goodbye;
    ```

Parameter 属性の詳細については、「 [Parameter 属性の宣言](./parameter-attribute-declaration.md)」を参照してください。

## <a name="how-to-declare-a-parameter-with-aliases"></a>エイリアスを使用してパラメーターを宣言する方法

- 次のコードに示すように、パブリックプロパティを定義します。 パラメーターのエイリアスを一覧表示するエイリアス属性を追加します。 この例では、同じパラメーターに対して3つのエイリアスが定義されています。 最初のエイリアスは、ショートカットを提供します。 2番目と3番目のエイリアスには、さまざまなシナリオで使用できる名前が用意されています。

    ```csharp
    [Alias("UN","Writer","Editor")]
    [Parameter()]
    public string UserName
    {
      get { return userName; }
      set { userName = value; }
    }
    private string userName;
    ```

Alias 属性の詳細については、「 [Alias 属性の宣言](./alias-attribute-declaration.md)」を参照してください。

## <a name="see-also"></a>参照

[System.Management.Automation.SwitchParameter](/dotnet/api/System.Management.Automation.SwitchParameter)

[パラメーター属性の宣言](./parameter-attribute-declaration.md)

[エイリアス属性の宣言](./alias-attribute-declaration.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
