---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の ExpressionBinding の CustomControlName 要素 (書式)
description: Configuration の Controls の ExpressionBinding の CustomControlName 要素 (書式)
ms.openlocfilehash: 3815956f59f19c0215aaf26b94dede656b9453cb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648314"
---
# <a name="customcontrolname-element-for-expressionbinding-for-controls-for-configuration-format"></a>Configuration の Controls の ExpressionBinding の CustomControlName 要素 (書式)

コモンコントロールの名前を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの configuration (format) コントロールの要素の構成 (format) の CustomControl 要素の構成 (形式) の CustomControl の構成 (書式) の CustomEntries 要素のコントロール要素 CustomControl for Controls の構成 (書式) 用の Customentries 要素のコントロールの構成式のバインド要素の構成のためのコントロールの構成 (format) CustomControlName 要素の構成用コントロールの構成 (形式)

## <a name="syntax"></a>構文

```xml
<CustomControlName>NameofCustomControl</CustomControlName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomControlName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)|コントロールによって表示されるデータを定義します。|

## <a name="text-value"></a>テキスト値

コントロールの名前を指定します。

## <a name="remarks"></a>解説

書式設定ファイルのすべてのビューで使用できるコモンコントロールを作成できます。また、特定のビューで使用できるビューコントロールを作成することもできます。 次の要素は、これらのコントロールの名前を指定します。

- [Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

- [View の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-view-format.md)

## <a name="see-also"></a>参照

[Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

[View の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-view-format.md)

[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
