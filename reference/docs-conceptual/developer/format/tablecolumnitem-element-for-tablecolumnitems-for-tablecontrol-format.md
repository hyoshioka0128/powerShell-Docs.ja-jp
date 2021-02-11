---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableColumnItems の TableColumnItem 要素 (書式)
description: TableControl の TableColumnItems の TableColumnItem 要素 (書式)
ms.openlocfilehash: 8ef5158c9bb9f074d5c58190d4d3b20c10c83744
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659856"
---
# <a name="tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format"></a>TableControl の TableColumnItems の TableColumnItem 要素 (書式)

行の列に値が表示されるプロパティまたはスクリプトを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries 要素の tablecontrol (format) TableRowEntry 要素の TableRowEntries for tablecontrol (format) TableColumnItems 要素 for TableControlEntry for tablecontrol (format) TableColumnItem for TableControl (format) TableColumnItems 要素

## <a name="syntax"></a>構文

```xml
<TableColumnItem>
  <Alignment>Left, Right, or Center</Alignment>
  <FormatString>FormatPattern</FormatString>
  <PropertyName>Nameof.NetProperty</PropertyName>
  <ScriptBlock>ScriptToEvaluate</ScriptBlock>
</TableColumnItem>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TableColumnItem` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableColumnItem の Alignment 要素 (書式)](./alignment-element-for-tablecolumnitem-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> 行の列のデータをどのように表示するかを定義します。|
|[TableControl の TableColumnItem の FormatString 要素 (書式)](./formatstring-element-for-tablecolumnitem-for-tablecontrol-format.md)|行の列のデータの書式設定に使用する形式パターンを指定します。|
|[TableControl の TableColumnItem の PropertyName 要素 (書式)](./propertyname-element-for-tablecolumnitem-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> 値が表示されるプロパティの名前を指定します。|
|[TableControl の TableColumnItem の ScriptBlock 要素 (書式)](./scriptblock-element-for-tablecolumnitem-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> 行の列に値が表示されるスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl (Format) の TableControlEntry の TableColumnItems 要素](./tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format.md)|値が行内に表示されるプロパティまたはスクリプトを定義します。|

## <a name="remarks"></a>解説

行の各列に、オブジェクトまたはスクリプトのプロパティを指定できます。 子要素が指定されていない場合、項目はプレースホルダーであり、データは表示されません。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

この例は、 `TableColumnItem` `Status` system.object オブジェクトのプロパティの値を表示する要素[](/dotnet/api/System.Diagnostics.Process)を示しています。

```xml
<TableColumnItem>
   <Alignment>Centered</Alignment>
  <PropertyName>Status</PropertyName>
</TableColumnItem>

```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableControl の TableColumnItem の Alignment 要素 (書式)](./alignment-element-for-tablecolumnitem-for-tablecontrol-format.md)

[TableColumnItems 要素 (Format)](./tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format.md)

[TableControl の TableColumnItem の FormatString 要素 (書式)](./formatstring-element-for-tablecolumnitem-for-tablecontrol-format.md)

[TableControl の TableColumnItem の PropertyName 要素 (書式)](./propertyname-element-for-tablecolumnitem-for-tablecontrol-format.md)

[TableControl の TableColumnItem の ScriptBlock 要素 (書式)](./scriptblock-element-for-tablecolumnitem-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
