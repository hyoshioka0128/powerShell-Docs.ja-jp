---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の TableColumnItem の Alignment 要素 (書式)
description: TableControl の TableColumnItem の Alignment 要素 (書式)
ms.openlocfilehash: d2bb81ff894cad44e16212891faffd22ee627383
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646120"
---
# <a name="alignment-element-for-tablecolumnitem-for-tablecontrol-format"></a>TableControl の TableColumnItem の Alignment 要素 (書式)

行の列のデータをどのように表示するかを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (Format) TableRowEntries Element (format) TableRowEntry Element (format) TableColumnItems Element (format) TableColumnItem Element (format) Element (format) Element (format) Alignment 要素 TableColumnItem (Format)

## <a name="syntax"></a>構文

```xml
<Alignment>AlignmentType</Alignment>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Alignment` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[TableColumnItem 要素 (Format)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)|テーブルの列のデータのラベル、幅、および配置を定義します。|

## <a name="text-value"></a>テキスト値

次のいずれかの値を指定します。 (これらの値は大文字と小文字が区別されません)。

列に表示されているデータを左にシフトします。 (この要素が指定されていない場合の既定値です)。

列に表示されているデータを右にシフトします。

中央には、列に表示されるデータが中央に配置されます。

## <a name="remarks"></a>解説

テーブルビューのコンポーネントの詳細については、「 [テーブルビュー](./creating-a-table-view.md)」を参照してください。

## <a name="see-also"></a>参照

[テーブルビュー](./creating-a-table-view.md)

[TableColumnItem 要素 (Format)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)
