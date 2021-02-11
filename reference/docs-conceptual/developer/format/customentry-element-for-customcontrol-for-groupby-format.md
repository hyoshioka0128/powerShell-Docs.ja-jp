---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomControl の CustomEntry 要素 (書式)
description: GroupBy の CustomControl の CustomEntry 要素 (書式)
ms.openlocfilehash: 0df2ff9c15308939e6d2552f51e2961bdabc59fb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646066"
---
# <a name="customentry-element-for-customcontrol-for-groupby-format"></a>GroupBy の CustomControl の CustomEntry 要素 (書式)

コントロールの定義を提供します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素を表示 (format) の CustomControl 要素を groupby (形式) の CustomControl の CustomControl の CustomEntry 要素の groupby (形式)

## <a name="syntax"></a>構文

```xml
<CustomEntry>
  <EntrySelectedBy>...</EntrySelectedBy>
  <CustomItem>...</CustomItem>
</CustomEntry>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomEntry` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-groupby-format.md)|省略可能な要素です。<br /><br /> このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|
|[GroupBy の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-groupby-format.md)|必須の要素です。<br /><br /> コントロールがデータを表示する方法を定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-groupby-format.md)|コントロールの定義を提供します。|

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-groupby-format.md)

[GroupBy の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-groupby-format.md)

[GroupBy の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
