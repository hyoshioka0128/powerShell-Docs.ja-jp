---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableRowEntry の EntrySelectedBy 要素 (書式)
description: TableControl の TableRowEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 1b7fc60b6fa9864b66e9edfebb3e4a86e287f3f8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645894"
---
# <a name="entryselectedby-element-for-tablerowentry--for-tablecontrol-format"></a>TableControl の TableRowEntry の EntrySelectedBy 要素 (書式)

テーブルビューのこの定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries Element (format) TableRowEntry Element (format) 要素 (format) のエントリ

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
|[TableControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> このテーブルビュー定義を使用するために必要な条件を定義します。|
|[TableControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> このテーブルビュー定義を使用する一連の .NET 型を指定します。|
|[TableControl の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> このテーブルビュー定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl (Format) の TableRowEntry 要素](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md)|テーブルの行に表示されるデータを定義します。|

## <a name="remarks"></a>解説

テーブルビュー定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要があります。 使用できる子要素の数に上限はありません。

選択条件は、オブジェクトが特定のプロパティを持っている場合や、特定のプロパティ値またはスクリプトがに評価される場合など、使用する定義のために存在する必要がある条件を定義するために使用され `true` ます。 選択条件の詳細については、「 [ビューエントリまたは項目が使用される場合の条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `TableRowEntry` system.object オブジェクトのプロパティを表示するために使用[](/dotnet/api/System.Diagnostics.Process)される要素を示しています。

```xml
<TableRowEntry>
  <EntrySelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </EntrySelectedBy>
  <TableColumnItems>
    <TableColumnItem>
      <PropertyName>PropertyForFirstColumn</PropertyName>
    </TableColumnItem>
    <TableColumnItem>
      <PropertyName>PropertyForSecondColumn</PropertyName>
    </TableColumnItem>
  </TableColumnItems>
</TableRowEntry>
```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-tablecontrol-format.md)

[TableControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-tablecontrol-format.md)

[TableControl (Format) の TableRowEntry 要素](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md)

[TableControl の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
