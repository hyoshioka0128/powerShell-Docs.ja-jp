---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)
description: GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 5af4abe081ca268699d281a1b586a584107b9a83
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652350"
---
# <a name="entryselectedby-element-for-customentry-for-groupby-format"></a>GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)

このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素を表示 (format) の CustomControl 要素を使用して、groupby (形式) の CustomControl の CustomControl の groupby (format) の CustomEntry 要素を使用して CustomEntry for GroupBy (形式)

## <a name="syntax"></a>構文

```xml
<EntrySelectedBy>
  <TypeName>Nameof.NetType</TypeName>
  <SelectionSetName>NameofSelectionSet</SelectionSetName>
  <SelectionCondition>...</SelectionCondition>
</EntrySelectedBy>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `EntrySelectedBy` ます。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要があります。 使用できる子要素の数に上限はありません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)|省略可能な要素です。<br /><br /> この定義を使用するために必要な条件を定義します。|
|[GroupBy の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-groupby-format.md)|省略可能な要素です。<br /><br /> コントロールのこの定義を使用する .NET 型のセットを指定します。|
|[GroupBy の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-groupby-format.md)|省略可能な要素です。<br /><br /> コントロールのこの定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-groupby-format.md)|コントロールの定義を提供します。|

## <a name="remarks"></a>解説

選択条件は、オブジェクトが特定のプロパティを持っている場合や、特定のプロパティ値またはスクリプトがに評価された場合など、使用する定義のために存在する必要がある条件を定義するために使用され `true` ます。 選択条件の詳細については、「 [ビューエントリまたは項目が使用される場合の条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[GroupBy の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)

[GroupBy の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-groupby-format.md)

[GroupBy の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-groupby-format.md)

[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
