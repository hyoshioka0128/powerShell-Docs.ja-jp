---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableColumnItem の FormatString 要素 (書式)
description: TableControl の TableColumnItem の FormatString 要素 (書式)
ms.openlocfilehash: 3d386e61ac321c05e0b298019c2298f76b391b21
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667897"
---
# <a name="formatstring-element-for-tablecolumnitem-for-tablecontrol-format"></a>TableControl の TableColumnItem の FormatString 要素 (書式)

テーブルのプロパティまたはスクリプトの値の表示方法を定義する形式パターンを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries Element (format) TableRowEntry Element (format) TableColumnItems Element (format) TableColumnItem Element (format) TableColumnItem (format) の FormatString 要素

## <a name="syntax"></a>構文

```xml
<FormatString>FormatPattern</FormatString>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `FormatString` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableColumnItem 要素 (Format)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)|行の列に値が表示されるプロパティまたはスクリプトを定義します。|

## <a name="text-value"></a>テキスト値

データの書式設定に使用するパターンを指定します。 たとえば、このパターンを使用して、型が system.string のプロパティの値を書式設定することができ [ます。 Timespan](/dotnet/api/System.TimeSpan): {0: MMM} {0: dd} {0: HH}: {0: mm}。

## <a name="remarks"></a>解説

書式指定文字列は、テーブルビュー、リストビュー、ワイドビュー、またはカスタムビューを作成するときに使用できます。 ビューに表示される値の書式設定の詳細については、「 [表示されるデータの書式設定](./formatting-displayed-data.md)」を参照してください。

テーブルビューのコンポーネントの詳細については、「 [テーブルビュー](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、プロパティの値の書式設定文字列を定義する方法を示して `StartTime` います。

```xml
<TableColumnItem>
  <PropertyName>StartTime</PropertyName>
  <FormatString>{0:MMM} (0:DD) (0:HH):(0:MM)</FormatString>
</TableColumnItem>
```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[表示されるデータの書式を設定する](./formatting-displayed-data.md)

[TableColumnItem 要素 (Format)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
