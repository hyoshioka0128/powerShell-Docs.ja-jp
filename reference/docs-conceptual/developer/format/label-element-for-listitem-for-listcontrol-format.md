---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListItem の Label 要素 (書式)
description: ListControl の ListItem の Label 要素 (書式)
ms.openlocfilehash: 01ff34c4129abe2c76a0a21794e756b77bff12bf
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666656"
---
# <a name="label-element-for-listitem-for-listcontrol-format"></a>ListControl の ListItem の Label 要素 (書式)

行内のプロパティまたはスクリプト値の左側に表示されるラベルを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (format) ListEntries 要素の ListControl (format) ListEntry 要素の ListControl (format) ListItems の ListEntry 要素 (format) の ListItem 要素 ListControl for ListItems (format) の ListItem 要素を ListControl (format) に設定します。

## <a name="syntax"></a>構文

```xml
<Label>Label for displayed value</Label>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Label` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ListControl の ListItems の ListItem 要素 (書式)](./listitem-element-for-listitems-for-listcontrol-format.md)|リストビューの行に表示される値を持つプロパティまたはスクリプトを定義します。|

## <a name="text-value"></a>テキスト値

プロパティまたはスクリプト値の左側に表示するラベルを指定します。

## <a name="remarks"></a>解説

ラベルが指定されていない場合は、プロパティまたはスクリプトの名前が表示されます。 リストビューでのラベルの使用の詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

次の例では、ラベルを行に追加する方法を示します。

```xml
<ListItem>
  <Label>Property1: </Label>
  <PropertyName>DotNetProperty1</PropertyName>
</ListItem>

```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[ListItem 要素 (形式)](./listitem-element-for-listitems-for-listcontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
