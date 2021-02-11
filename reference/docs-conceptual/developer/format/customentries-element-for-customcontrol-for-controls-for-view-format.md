---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の CustomControl の CustomEntries 要素 (書式)
description: View の Controls の CustomControl の CustomEntries 要素 (書式)
ms.openlocfilehash: 43187294a407d08f765f8c42aba25d13dba6d901
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652377"
---
# <a name="customentries-element-for-customcontrol-for-controls-for-view-format"></a>View の Controls の CustomControl の CustomEntries 要素 (書式)

コントロールの定義を提供します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (形式) コントロールのコントロールの要素 (書式) のコントロール要素 (format) のコントロールの CustomControl 要素 (format) のコントロールのコントロール要素 (形式) のコントロールの CustomControl

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
|[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)|必須の要素です。<br /><br /> コントロールの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の Control の CustomControl 要素 (書式)](./customcontrol-element-for-control-for-controls-for-view-format.md)|ビューによって使用されるコントロールを定義します。|

## <a name="remarks"></a>解説

ほとんどの場合、コントロールには定義が1つだけあります。定義は1つの要素で指定され `CustomEntry` ます。 ただし、同じコントロールを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を指定できます。 そのような場合は、 `CustomEntry` オブジェクトまたはオブジェクトのセットごとに要素を定義できます。

## <a name="see-also"></a>参照

[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)

[View の Controls の Control の CustomControl 要素 (書式)](./customcontrol-element-for-control-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
