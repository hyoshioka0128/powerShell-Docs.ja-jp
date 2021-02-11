---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ItemSelectionCondition の PropertyName 要素 (書式)
description: ListControl の ItemSelectionCondition の PropertyName 要素 (書式)
ms.openlocfilehash: c515efe70afdb1c1186c0a07fe1f52dc49ad57b9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665993"
---
# <a name="propertyname-element-for-itemselectioncondition-for-listcontrol-format"></a>ListControl の ItemSelectionCondition の PropertyName 要素 (書式)

条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、ビューが使用されます。 この要素は、リストビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (format) ListControl Element (format) ListEntries Element (format) ListEntry 要素 for ListEntry (format) ListControl (format) ListItems 要素の ListControl for ListItems (format) 項目の (format) 項目の listitem 要素の ListItem 要素の Listentries の (形式)

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
|[ListControl の ListItem の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md)||

## <a name="text-value"></a>テキスト値

値が表示されるプロパティの名前を指定します。

## <a name="remarks"></a>解説

この要素が使用されている場合、選択条件を定義するときに [ScriptBlock](./scriptblock-element-for-itemselectioncondition-for-listcontrol-format.md) 要素を指定することはできません。

## <a name="see-also"></a>参照

[ListIControl の ItemSelectionCondition の ScriptBlock 要素 (形式)](./scriptblock-element-for-itemselectioncondition-for-listcontrol-format.md)

[ListControl の ListItem の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
