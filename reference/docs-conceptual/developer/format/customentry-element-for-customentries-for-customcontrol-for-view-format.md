---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の CustomEntries の CustomEntry 要素 (書式)
description: View の CustomControl の CustomEntries の CustomEntry 要素 (書式)
ms.openlocfilehash: ff481f13e6f16267bdda4c3053faebc96d9a6d3a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646048"
---
# <a name="customentry-element-for-customentries-for-customcontrol-for-view-format"></a>View の CustomControl の CustomEntries の CustomEntry 要素 (書式)

カスタムコントロールビューの定義を提供します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) CustomControl 要素 (形式) の CustomEntries 要素の CustomControl for view (Format) ビューの Custommentry 要素 for View (Format)

## <a name="syntax"></a>構文

```xml
<CustomEntry>
  <EntrySelectedBy>...</EntrySelectedBy>
  <CustomItem>...</CustomItem>
</CustomEntry>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomEntry` ます。 定義によって表示される項目を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> カスタムコントロールビューの定義、またはこの定義を使用するために存在する必要がある条件を使用する .NET 型を定義します。|
|[CustomEntry for ビューの CustomItem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md)|カスタムコントロール定義のコントロールを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-view-format.md)|カスタムコントロールビューの定義を提供します。 カスタムコントロールビューでは、1つまたは複数の定義を指定する必要があります。|

## <a name="remarks"></a>解説

ほとんどの場合、カスタムコントロールビューごとに1つの定義のみが必要ですが、同じビューを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を指定できます。 このような場合は、オブジェクトまたはオブジェクトのセットごとに個別の定義を指定できます。

## <a name="see-also"></a>参照

[View の CustomControl 要素 (書式)](./customcontrol-element-for-view-format.md)

[CustomEntry for ビューの CustomItem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md)

[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
