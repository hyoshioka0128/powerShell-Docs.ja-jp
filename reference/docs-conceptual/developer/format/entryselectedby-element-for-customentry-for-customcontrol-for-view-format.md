---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の CustomEntry の EntrySelectedBy 要素 (書式)
description: View の CustomControl の CustomEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 4821f22560f35034f90d018e5a109004f331441f
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655342"
---
# <a name="entryselectedby-element-for-customentry-for-customcontrol-for-view-format"></a>View の CustomControl の CustomEntry の EntrySelectedBy 要素 (書式)

このカスタムエントリを使用する .NET 型、またはこのエントリが使用されるために必要な条件を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) CustomControl 要素 (形式) の CustomEntries for CustomControl for view (format) の CustomEntry 要素を使用して、CustomEntry for ビューの EntrySelectedBy 要素を表示 (形式) します。

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
|[CustomEntry (形式) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-customcontrol-format.md)|省略可能な要素です。<br /><br /> この定義を使用するために必要な条件を定義します。|
|[CustomEntry (形式) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールビューのこの定義を使用する .NET 型のセットを指定します。|
|[CustomEntry (形式) の EntrySelectedBy の TypeName 要素](./typename-element-for-selectioncondition-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールビューのこの定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View (Format) の CustomEntries の CustomEntry 要素](./customentry-element-for-customentries-for-customcontrol-for-view-format.md)|特定の .NET オブジェクトで使用されるコントロールを定義します。|

## <a name="remarks"></a>解説

エントリの種類、選択セット、または選択条件を少なくとも1つ指定する必要があります。 使用できる子要素の数に上限はありません。

選択条件は、オブジェクトに特定のプロパティがある場合や、特定のプロパティ値またはスクリプトがに評価される場合など、エントリの使用に必要な条件を定義するために使用し `true` ます。 選択条件の詳細については、「 [ビューエントリまたは項目が使用される場合の条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

カスタムコントロールビューのコンポーネントの詳細については、「 [カスタムコントロールビュー](./creating-custom-controls.md)」を参照してください。

## <a name="see-also"></a>参照

[CustomEntry (形式) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-customcontrol-format.md)

[CustomEntry (形式) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-customcontrol-for-view-format.md)

[CustomEntry (形式) の EntrySelectedBy の TypeName 要素](./typename-element-for-selectioncondition-for-customcontrol-for-view-format.md)

[View (Format) の CustomEntries の CustomEntry 要素](./customentry-element-for-customentries-for-customcontrol-for-view-format.md)

[カスタムコントロールビュー](./creating-custom-controls.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
