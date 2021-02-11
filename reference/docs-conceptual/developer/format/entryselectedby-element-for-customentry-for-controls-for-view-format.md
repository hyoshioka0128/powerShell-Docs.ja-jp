---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)
description: View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 29b0574a95d81962fb3f72a526f89273baeea647
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660265"
---
# <a name="entryselectedby-element-for-customentry-for-controls-for-view-format"></a>View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)

このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (書式) コントロールのコントロール要素 (format) を使用して、ビューのコントロール (format) の CustomControl 要素のコントロール要素 view (format) の CustomControl のための Entries 要素を表示するためのコントロールのためのカスタムエントリを表示するためのコントロール (形式) の CustomEntry 要素を指定します。

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
|[View の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> この定義を使用するために必要な条件を定義します。|
|[View の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールのこの定義を使用する .NET 型のセットを指定します。|
|[View の Controls の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールのこの定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)|コントロールの定義を提供します。|

## <a name="remarks"></a>解説

選択条件は、オブジェクトが特定のプロパティを持っている場合や、特定のプロパティ値またはスクリプトがに評価された場合など、使用する定義のために存在する必要がある条件を定義するために使用され `true` ます。 選択条件の詳細については、「 [ビューエントリまたは項目が使用される場合の条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
