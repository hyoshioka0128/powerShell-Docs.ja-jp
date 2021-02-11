---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableRowEntries 要素 (書式)
description: TableControl の TableRowEntries 要素 (書式)
ms.openlocfilehash: 1df63e645234da8276c7ccc5af34e81a56475e43
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651481"
---
# <a name="tablerowentries-element-for-tablecontrol-format"></a>TableControl の TableRowEntries 要素 (書式)

テーブルの行を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries 要素 (format)

## <a name="syntax"></a>構文

```xml
<TableRowEntries>
  <TableRowEntry>...</TableRowEntry>
</TableRowEntries>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TableRowEntries` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableRowEntries の TableRowEntry 要素 (書式)](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md)|必須の要素です。<br /><br /> テーブルの行に表示されるデータを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl 要素 (書式)](./tablecontrol-element-format.md)|ビューのテーブル形式を定義します。|

## <a name="remarks"></a>解説

テーブルビューには1つ以上の要素を指定する必要があり `TableRowEntry` ます。 追加できる要素の数に上限はありません `TableRowEntry` 。また、順序が重要でもありません。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、system.string `TableRowEntries` オブジェクトの2つのプロパティの値を表示する行を[](/dotnet/api/System.Diagnostics.Process)定義する要素を示しています。

```xml
<TableRowEntries>
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
</TableRowEntries>

```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableControl 要素 (書式)](./tablecontrol-element-format.md)

[TableRowEntry 要素 (Format)](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
