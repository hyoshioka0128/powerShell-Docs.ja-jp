---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableColumnHeader の Label 要素 (書式)
description: TableControl の TableColumnHeader の Label 要素 (書式)
ms.openlocfilehash: fe4c209903c04e683b44e2bdcbf381adb6874ace
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667795"
---
# <a name="label-element-for-tablecolumnheader-for-tablecontrol-format"></a>TableControl の TableColumnHeader の Label 要素 (書式)

列の上部に表示されるラベルを定義します。 この要素は、テーブルビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableHeaders 要素の tablecontrol (format) TableColumnHeader for tablecontrol (format) Label Element for tablecontrol (Format) の TableColumnHeader 要素

## <a name="syntax"></a>構文

```xml
<Label>DisplayedLabel</Label>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Label` ます。 各列に対して使用できるラベルは1つだけです。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableControl の TableHeaders の TableColumnHeader 要素 (Format)](./tablecolumnheader-element-format.md)|テーブルの列のデータのラベル、幅、および配置を定義します。|

## <a name="text-value"></a>テキスト値

テーブルの列の上部に表示されるテキストを指定します。 列ラベルに使用できる文字は制限されていません。

## <a name="remarks"></a>解説

ラベルが指定されていない場合は、行に表示される値を持つプロパティの名前が使用されます。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

この例で `TableColumnHeader` は、ラベルが "Column 1" である要素を示しています。

```xml
<TableColumnHeader>
  <Label>Column 1</Label)
  <Width>16</Width>
  <Alignment>Left</Alignment>
</TableColumnHeader>
```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[TableColumnHeader 要素 (書式)](./tablecolumnheader-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
