---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListEntry の ListItems 要素 (書式)
description: ListControl の ListEntry の ListItems 要素 (書式)
ms.openlocfilehash: 1553c81bc559020223a3c1fea10336baf017c9a0
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666537"
---
# <a name="listitems-element-for-listentry-for-listcontrol-format"></a>ListControl の ListEntry の ListItems 要素 (書式)

リストビューの行に値が表示されるプロパティとスクリプトを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl 要素 (format) ListControl (format) ListItems 要素のリストコントロール (format) ListEntry 要素の ListEntries 要素を ListControl (Format) に設定します。

## <a name="syntax"></a>構文

```xml
<ListItems>
  <ListItem>...</ListItem>
</ListItems>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ListItems` ます。 指定できる子要素の数に制限はありません。 子要素の順序は、リストビューに表示される値の順序を定義します。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ListControl の ListItem 要素 (形式)](./listitem-element-for-listitems-for-listcontrol-format.md)|必須の要素です。<br /><br /> リストビューによって表示される値を持つプロパティまたはスクリプトを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ListControl の ListEntry 要素 (書式)](./listentry-element-for-listcontrol-format.md)|リストビューの定義を提供します。|

## <a name="remarks"></a>解説

この種類のビューの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

この例は、リストビューの3つの行を定義する XML 要素を示しています。

```xml
<ListEntry>
    <ListItems>
      <ListItem>
        <Label>Property1: </Label>
        <PropertyName>.NetTypeProperty1</PropertyName>
      </ListItem>
      <ListItem>
        <PropertyName>.NetTypeProperty2</PropertyName>
      </ListItem>
      <ListItem>
        <ScriptBlock>$_.ProcessName + ":" $_.Id</ScriptBlock>
      </ListItem>
  </ListEntry>
```

## <a name="see-also"></a>参照

[ListControl の ListEntry 要素 (書式)](./listentry-element-for-listcontrol-format.md)

[ListControl の ListItem 要素 (形式)](./listitem-element-for-listitems-for-listcontrol-format.md)

[リスト ビューを作成する](./creating-a-list-view.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
