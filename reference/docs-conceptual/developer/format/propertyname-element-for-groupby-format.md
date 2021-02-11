---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の PropertyName 要素 (書式)
description: GroupBy の PropertyName 要素 (書式)
ms.openlocfilehash: 44351c46ff2386f967644fef4f423b3858dc1619
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666146"
---
# <a name="propertyname-element-for-groupby-format"></a>GroupBy の PropertyName 要素 (書式)

値が変更されるたびに新しいグループを開始する .NET プロパティを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビューの要素 (書式) GroupBy 要素 (形式) の PropertyName 要素

## <a name="syntax"></a>構文

```xml
<PropertyName>.NetTypeProperty</PropertyName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `PropertyName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)|.NET オブジェクトのグループを表示する方法を定義します。|

## <a name="text-value"></a>テキスト値

.NET プロパティ名を指定します。

## <a name="remarks"></a>解説

このプロパティの値が変更されるたびに、Windows PowerShell によって新しいグループが開始されます。

この要素が指定されている場合、 [ScriptBlock](./scriptblock-element-for-groupby-format.md) 要素を指定して新しいグループを開始することはできません。

## <a name="example"></a>例

次の例は、プロパティの値が変更されたときに新しいグループを開始する方法を示しています。

```xml
<GroupBy>
  <Label>Service Type</Label>
  <PropertyName>ServiceType</PropertyName>
</GroupBy>

```

この要素を含む完全な書式設定ファイルの例については、「 [Wide ビュー (GroupBy)](./wide-view-groupby.md)」を参照してください。

## <a name="see-also"></a>参照

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[GroupBy の ScriptBlock 要素 (書式)](./scriptblock-element-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
