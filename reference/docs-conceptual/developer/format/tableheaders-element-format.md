---
ms.date: 09/13/2016
ms.topic: reference
title: TableHeaders 要素 (Format)
description: TableHeaders 要素 (Format)
ms.openlocfilehash: 5ac4dccae746c167ebf95add9f3d18030a2b3a99
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659819"
---
# <a name="tableheaders-element-format"></a>TableHeaders 要素 (Format)

テーブルの列のヘッダーを定義します。

ViewDefinitions 要素 (Format) ビュー要素 (Format) TableControl Element (Format) TableHeaders 要素 (Format)

## <a name="syntax"></a>構文

```xml
<TableHeaders>
  <TableColumnHeader>...</TableColumnHeader>
</TableHeaders>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TableHeaders` ます。 表示されるオブジェクトの各プロパティには、子要素が必要です。 列ヘッダー情報は、子要素が指定されている順序で表示されます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[TableColumnHeader 要素 (書式)](./tablecolumnheader-element-format.md)|省略可能な要素です。<br /><br /> テーブルビューの列のデータのラベル、幅、および配置を定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl 要素 (書式)](./tablecontrol-element-format.md)|ビューのテーブル形式を定義します。|

## <a name="remarks"></a>解説

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `TableHeaders` 2 つの列ヘッダーを定義する要素を示しています。

```xml
<TableHeaders>
  <TableColumnHeader>
    <Label>Column 1</Label)
    <Width>16</Width>
    <Alignment>Left</Alignment>
  </TableColumnHeader>
  <TableColumnHeader>
    <Label>Column 2</Label)
    <Width>10</Width>
    <Alignment>Centered</Alignment>
  </TableColumnHeader>
</TableHeaders>
```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableColumnHeader 要素 (書式)](./tablecolumnheader-element-format.md)

[TableControl 要素 (書式)](./tablecontrol-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
