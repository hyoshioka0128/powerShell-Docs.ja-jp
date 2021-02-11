---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableRowEntry の TableColumnItems 要素 (書式)
description: TableControl の TableRowEntry の TableColumnItems 要素 (書式)
ms.openlocfilehash: 4d600a366d2be1c453f05b301bdf575351dd51c1
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667761"
---
# <a name="tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format"></a>TableControl の TableRowEntry の TableColumnItems 要素 (書式)

値が行内に表示されるプロパティまたはスクリプトを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries 要素の tablecontrol (format) TableRowEntry 要素の TableRowEntries for TableControl (Format) TableColumnItems Element for TableControl (Format)

## <a name="syntax"></a>構文

```xml
TableColumnItems>
  <TableColumnItem>...</TableColumnItem>
</TableColumnItems>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TableColumnItems` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableColumnItems の TableColumnItem 要素 (書式)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)|必須の要素です。<br /><br /> 行の列に値が表示されるプロパティまたはスクリプトを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableRowEntries の TableRowEntry 要素 (書式)](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md)|テーブルの行に表示されるデータを定義します。|

## <a name="remarks"></a>解説

`TableColumnItem`行の各列には要素が必要です。 最初のエントリは最初の列に、2番目の列には2番目のエントリが表示されます。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `TableColumnItems` system.object オブジェクトの3つのプロパティ[](/dotnet/api/System.Diagnostics.Process)を定義する要素を示しています。

```xml
<TableColumnItems>
  <TableColumnItem>
    <PropertyName>Status</PropertyName>
  </TableColumnItem>
  <TableColumnItem>
    <PropertyName>Name</PropertyName>
  </TableColumnItem>
  <TableColumnItem>
    <PropertyName>DisplayName</PropertyName>
  </TableColumnItem>
</TableColumnItems>

```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableColumnItem 要素 (Format)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)

[TableRowEntry 要素 (Format)](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
