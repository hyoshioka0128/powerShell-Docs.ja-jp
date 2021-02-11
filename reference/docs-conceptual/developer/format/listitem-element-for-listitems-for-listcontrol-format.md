---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListItems の ListItem 要素 (書式)
description: ListControl の ListItems の ListItem 要素 (書式)
ms.openlocfilehash: 999491b7851aa4fa21667ad376d7e9853500ca08
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666554"
---
# <a name="listitem-element-for-listitems-for-listcontrol-format"></a>ListControl の ListItems の ListItem 要素 (書式)

リストビューの行に表示される値を持つプロパティまたはスクリプトを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (format) ListEntries 要素 for ListItems (format) ListControl element for ListControl (format) ListEntry Element for Element (format)。

## <a name="syntax"></a>構文

```xml
<ListItem>
  <PropertyName>PropertyToDisplay</PropertyName>
  <ScriptBlock>ScriptToExecute</ScriptBlock>
  <Label>LabelToDisplay</Label>
  <FormatString>FormatPattern</FormatString>
  <ItemSelectionCondition>...</ItemSelectionCondition>
</ListItem>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ListItem` ます。 1つのプロパティまたはスクリプトだけを指定できます。

### <a name="attributes"></a>属性

なし

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ListControl の ListItem の FormatString 要素 (形式)](./formatstring-element-for-listitem-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> プロパティまたはスクリプトの値の表示方法を定義する書式設定文字列を指定します。|
|[ListControl の ListItem の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> このリスト項目を使用するために必要な条件を定義します。|
|[ListControl の ListItem の Label 要素 (書式)](./label-element-for-listitem-for-listcontrol-format.md)|省略可能な要素<br /><br /> 行内のプロパティまたはスクリプト値の左側に表示されるラベルを指定します。|
|[ListControl の ListItem の PropertyName 要素 (書式)](./propertyname-element-for-listitem-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> 行に値が表示される .NET プロパティを指定します。|
|[ListControl の ListItem の ScriptBlock 要素 (書式)](./scriptblock-element-for-listitem-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> 行に値が表示されるスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[リストコントロールの ListItems 要素 (Format)](./listitems-element-for-listentry-for-listcontrol-format.md)|リストビューに値が表示されるプロパティとスクリプトを定義します。|

## <a name="remarks"></a>解説

リストビューのコンポーネントの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

この例は、リストビューの3つの行を定義する XML 要素を示しています。 最初の2行は .NET プロパティの値を表示し、最後の行にはスクリプトによって生成された値が表示されます。

```xml
<ListEntry>
    <ListItems>
      <ListItem>
        <Label>Property1: </Label>
        <PropertyName>DotNetProperty1</PropertyName>
      </ListItem>
      <ListItem>
        <PropertyName>DotNetProperty2</PropertyName>
      </ListItem>
      <ListItem>
        <ScriptBlock>$_.ProcessName + ":" $_.Id</ScriptBlock>
      </ListItem>
    </ListItems>
</ListEntry>

```

## <a name="see-also"></a>参照

[ListItems 要素 (Format)](./listitems-element-for-listentry-for-listcontrol-format.md)

[ListItem の FormatString 要素 (形式)](./formatstring-element-for-listitem-for-listcontrol-format.md)

[ListItem の Label 要素 (形式)](./label-element-for-listitem-for-listcontrol-format.md)

[ListItem の PropertyName 要素 (Format)](./propertyname-element-for-listitem-for-listcontrol-format.md)

[ListItem の ScriptBlock 要素 (形式)](./scriptblock-element-for-listitem-for-listcontrol-format.md)

[リスト ビューを作成する](./creating-a-list-view.md)

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
