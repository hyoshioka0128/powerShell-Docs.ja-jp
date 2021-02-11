---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomControl の CustomEntries 要素 (書式)
description: GroupBy の CustomControl の CustomEntries 要素 (書式)
ms.openlocfilehash: cde59d38b83930cb64a3a0040891783e4ab96723
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666792"
---
# <a name="customentries-element-for-customcontrol-for-groupby-format"></a>GroupBy の CustomControl の CustomEntries 要素 (書式)

コントロールの定義を提供します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) ビュー Element (format) GroupBy 要素の groupby (format) CustomControl 要素を groupby (形式) の CustomControl の CustomEntries 要素に設定します。

## <a name="syntax"></a>構文

```xml
<CustomEntries>
  <CustomEntry>...</CustomEntry>
</CustomEntries>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomEntries` ます。 指定できる子要素の数に上限はありません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-groupby-format.md)|必須の要素です。<br /><br /> コントロールの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomControl 要素 (書式)](./customcontrol-element-for-groupby-format.md)|新しいグループを表示するカスタムコントロールを定義します。|

## <a name="remarks"></a>解説

ほとんどの場合、コントロールには定義が1つだけあります。定義は1つの要素で指定され `CustomEntry` ます。 ただし、同じコントロールを使用して異なるグループを表示する場合は、複数の定義を指定できます。 そのような場合は、 `CustomEntry` グループの要素を定義できます。

## <a name="see-also"></a>参照

[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)

[GroupBy の CustomControl 要素 (書式)](./customcontrol-element-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
