---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の ExpressionBinding の CustomControlName 要素 (書式)
description: View の CustomControl の ExpressionBinding の CustomControlName 要素 (書式)
ms.openlocfilehash: 24b27428c07d7178f0069f6d0e5b7ffc555efe34
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666809"
---
# <a name="customcontrolname-element-for-expressionbinding-for-customcontrol-for-view-format"></a>View の CustomControl の ExpressionBinding の CustomControlName 要素 (書式)

コモンコントロールまたはビューコントロールの名前を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) ビュー要素 (形式) の CustomControl 要素のビュー (format) CustomEntries 要素の CustomControl for view (format) customentries 要素に対して、CustomEntries for view (format) CustomControlName 要素の customentries (format) 式バインドのバインド要素 (形式) を指定します。

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
|[CustomItem の式のバインド要素 (Format)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)|コントロールによって表示されるデータを定義します。|

## <a name="text-value"></a>テキスト値

コントロールの名前を指定します。

## <a name="remarks"></a>解説

書式設定ファイルのすべてのビューで使用できるコモンコントロールを作成したり、特定のビューで使用できるビューコントロールを作成したりできます。 これらのコントロールの名前は、次の要素によって指定されます。

- [Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

- [View の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-view-format.md)

## <a name="see-also"></a>参照

[Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

[View の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-view-format.md)

[CustomItem の式のバインド要素 (Format)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
