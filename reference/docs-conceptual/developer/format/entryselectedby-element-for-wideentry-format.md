---
ms.date: 09/13/2016
ms.topic: reference
title: WideEntry の EntrySelectedBy 要素 (書式)
description: WideEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 246a1967300ab0551f376c4799deac275068308c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660239"
---
# <a name="entryselectedby-element-for-wideentry-format"></a>WideEntry の EntrySelectedBy 要素 (書式)

ワイドビューのこの定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element (format) WideEntry (Format) の要素 (format)

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
|[WideEntry (Format) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> このワイドビュー定義を使用するために必要な条件を定義します。|
|[WideEntry (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> このワイドビュー定義を使用する一連の .NET 型を指定します。|
|[WideEntry の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-wideentry-format.md)|省略可能な要素です。<br /><br /> このワイドビュー定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md)|ワイドビューの定義を提供します。|

## <a name="remarks"></a>解説

ワイドビュー定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要があります。 使用できる子要素の数に上限はありません。

選択条件は、オブジェクトに特定のプロパティがある場合や、特定のプロパティ値またはスクリプト値がに評価される場合など、使用する定義のために存在する必要がある条件を定義するために使用され `true` ます。 選択条件の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

ワイドビューのその他のコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="see-also"></a>参照

[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md)

[WideEntry (Format) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-widecontrol-format.md)

[WideEntry (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-widecontrol-format.md)

[WideEntry の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-wideentry-format.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[データの表示条件を定義する](./defining-conditions-for-displaying-data.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
