---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListItem の PropertyName 要素 (書式)
description: ListControl の ListItem の PropertyName 要素 (書式)
ms.openlocfilehash: 30cd48f9549e1a091776cd5f8395e1a71314ea1b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665976"
---
# <a name="propertyname-element-for-listitem-for-listcontrol-format"></a>ListControl の ListItem の PropertyName 要素 (書式)

値が一覧に表示される .NET プロパティを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (format) ListEntries 要素 (format) ListEntry Element (Format) ListItems Element (Format) ListItem 要素 (format) ListItem (format) の PropertyName 要素

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
|[ListItem 要素 (形式)](./listitem-element-for-listitems-for-listcontrol-format.md)|リストビューの行に表示される値を持つプロパティまたはスクリプトを定義します。|

## <a name="text-value"></a>テキスト値

値が表示されるプロパティの名前を指定します。

## <a name="remarks"></a>解説

この要素が指定されている場合、 [ScriptBlock](./scriptblock-element-for-listitem-for-listcontrol-format.md) 要素を指定することはできません。

プロパティ値を表示するだけでなく、値のラベルや、値の表示を変更するために使用できる書式設定文字列を指定することもできます。 リストビューでのデータの指定の詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、値が表示されるラベルとプロパティを指定する方法を示しています。

```xml
ListItem>
  <Label>NameOfProperty</Label>
  <PropertyName>.NetTypeProperty</PropertyName>
</ListItem>

```

## <a name="see-also"></a>参照

[ListControl の ListItem の ScriptBlock 要素 (書式)](./scriptblock-element-for-listitem-for-listcontrol-format.md)

[リスト ビューを作成する](./creating-a-list-view.md)

[ListControl の ListItem 要素 (形式)](./listitem-element-for-listitems-for-listcontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
