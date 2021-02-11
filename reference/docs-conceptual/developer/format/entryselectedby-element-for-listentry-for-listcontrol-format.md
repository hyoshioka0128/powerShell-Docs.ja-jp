---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListEntry の EntrySelectedBy 要素 (書式)
description: ListControl の ListEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 1981c8fae65f494504d6cdd9f59337d555350b07
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652289"
---
# <a name="entryselectedby-element-for-listentry-for-listcontrol-format"></a>ListControl の ListEntry の EntrySelectedBy 要素 (書式)

このリストビュー定義を使用する .NET 型、またはこの定義を使用するために存在する必要がある条件を定義します。 ほとんどの場合、リストビューに必要な定義は1つだけです。 ただし、同じリストビューを使用して異なるオブジェクトの異なるデータを表示する場合は、リストビューに複数の定義を指定できます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (Format) ListEntries 要素の ListControl (format) ListEntry 要素の ListEntry for ListControl (Format) の要素を ListEntry for ListControl (format) に渡します。

## <a name="syntax"></a>構文

```xml
<EntrySelectedBy>
  <TypeName>Nameof.NetType</TypeName>
  <SelectionSetName>NameofSelectionSet</SelectionSetName>
  <SelectionCondition>...</SelectionCondition>
</EntrySelectedBy>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `EntrySelectedBy` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ListControl (Format) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> このリストビュー定義を使用するために必要な条件を定義します。|
|[ListControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> このリストビュー定義を使用する一連の .NET 型を指定します。|
|[ListControl の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> このリストビュー定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ListControl の ListEntry 要素 (書式)](./listentry-element-for-listcontrol-format.md)|リストの行をどのように表示するかを定義します。|

## <a name="remarks"></a>解説

リストビュー定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要があります。 使用できる子要素の数に上限はありません。

選択条件は、オブジェクトが特定のプロパティを持っている場合や、特定のプロパティ値またはスクリプトがに評価される場合など、使用する定義のために存在する必要がある条件を定義するために使用され `true` ます。 選択条件の詳細については、「 [データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

リストビューのコンポーネントの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、.NET 型名を使用してリストビューのオブジェクトを定義する方法を示しています。

```xml
<ListEntry>
  <EntrySelectedBy>
    <TypeName>NameofDotNetType</TypeName>>
  </EntrySelectedBy>
</ListEntry>
```

## <a name="see-also"></a>参照

[ListControl の ListEntry 要素 (書式)](./listentry-element-for-listcontrol-format.md)

[ListControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-listcontrol-format.md)

[ListControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md)

[ListControl の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-listcontrol-format.md)

[リスト ビューを作成する](./creating-a-list-view.md)

[データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
