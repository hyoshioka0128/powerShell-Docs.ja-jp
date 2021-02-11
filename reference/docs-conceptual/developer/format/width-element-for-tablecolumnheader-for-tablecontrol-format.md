---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableColumnHeader の Width 要素 (書式)
description: TableControl の TableColumnHeader の Width 要素 (書式)
ms.openlocfilehash: bde84f1d33b3d6b3b8c4462f870f978611cb434b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92658249"
---
# <a name="width-element-for-tablecolumnheader-for-tablecontrol-format"></a>TableControl の TableColumnHeader の Width 要素 (書式)

列の幅 (文字数) を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableHeaders 要素 tablecontrol (format) TableColumnHeader 要素 TableHeaders for tablecontrol (format) TableColumnHeader for TableControl (Format)

## <a name="syntax"></a>構文

```xml
<Width>NumberOfCharacters</Width>
```

## <a name="attributes-and-elements"></a>属性および要素

以下のセクションでは、 `Width` 列ヘッダーを定義するときに使用する要素の属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableHeaders の TableColumnHeader 要素 (Format)](./tablecolumnheader-element-format.md)|テーブルの列のデータのラベル、幅、および配置を定義します。|

## <a name="text-value"></a>テキスト値

可能な限り、表示されるプロパティ値の長さを超える幅 (文字数) を指定します。

## <a name="remarks"></a>解説

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `TableColumnHeader` 幅が16文字の要素を示しています。

```xml
<TableColumnHeader>
  <Label>Column 1</Label)
  <Width>16</Width>
  <Alignment>Left</Alignment>
</TableColumnHeader>
```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableControl の TableHeader の TableColumnHeader 要素 (Format)](./tablecolumnheader-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
