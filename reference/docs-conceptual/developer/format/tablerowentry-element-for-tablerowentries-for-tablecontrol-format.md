---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableRowEntries の TableRowEntry 要素 (書式)
description: TableControl の TableRowEntries の TableRowEntry 要素 (書式)
ms.openlocfilehash: 60d64b7c14b40e87825ada36e19f52a66fe8b6cb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659769"
---
# <a name="tablerowentry-element-for-tablerowentries-for-tablecontrol-format"></a>TableControl の TableRowEntries の TableRowEntry 要素 (書式)

テーブルの行に表示されるデータを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries の TableControl (format) TableRowEntry 要素の TableRowEntries for TableControl (Format)

## <a name="syntax"></a>構文

```xml
<TableRowEntry>
  <Wrap/>
  <EntrySelectedBy>...</EntrySelectedBy>
  <TableColumnItems>...</TableColumnItems>
</TableRowEntry>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TableRowEntry` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[TableControl (Format) の TableRowEntry の EntrySelectedBy 要素](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md)|必須の要素です。<br /><br /> プロパティ値が行内に表示されるオブジェクトを定義します。|
|[TableControl の TableRowEntry の TableColumnItems 要素 (書式)](./tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format.md)|必須の要素です。<br /><br /> 値が表示されるプロパティまたはスクリプトを定義します。|
|[TableControl (Format) の TableRowEntry の Wrap 要素](./wrap-element-for-tablerowentry-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> 列幅を超えるテキストを次の行に表示するように指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableRowEntries 要素 (書式)](./tablerowentries-element-for-tablecontrol-format.md)|テーブルの行を定義します。|

## <a name="remarks"></a>解説

1つ `TableColumnItems` の要素と1つの `EntrySelectedBy` 要素を指定する必要があります。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、system.string `TableRowEntry` オブジェクトの2つのプロパティの値を表示する行を[](/dotnet/api/System.Diagnostics.Process)定義する要素を示しています。

```xml
<TableRowEntry>
  <EntrySelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </EntrySelectedBy>
  <TableColumnItems>
    <TableColumnItem>
      <PropertyName> Property for first column</PropertyName>
    </TableColumnItem>
    <TableColumnItem>
      <PropertyName> Property for second column</PropertyName>
    </TableColumnItem>
  </TableColumnItems>
</TableRowEntry>
```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableControl (Format) の TableRowEntry の EntrySelectedBy 要素](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md)

[TableControl の TableRowEntry の TableColumnItems 要素 (書式)](./tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format.md)

[TableControl の TableRowEntries 要素 (書式)](./tablerowentries-element-for-tablecontrol-format.md)

[TableControl (Format) の TableRowEntry の Wrap 要素](./wrap-element-for-tablerowentry-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
