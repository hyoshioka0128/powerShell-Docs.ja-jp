---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl 要素 (書式)
description: View の CustomControl 要素 (書式)
ms.openlocfilehash: 41352be55f0c03b2eaca0dbe2d7345e7cf804a7c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655461"
---
# <a name="customcontrol-element-for-view-format"></a>View の CustomControl 要素 (書式)

ビューのカスタムコントロール形式を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) CustomControl 要素 (形式)

## <a name="syntax"></a>構文

```xml
<CustomControl>
  <CustomEntries>...</CustomEntries>
</CustomControl>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomControl` ます。 1つの子要素を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[View の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-view-format.md)|必須の要素です。<br /><br /> カスタムコントロールビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上の .NET オブジェクトを表示するために使用されるビューを定義します。|

## <a name="remarks"></a>解説

ほとんどの場合、各コントロールビューに必要な定義は1つだけですが、同じビューを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を指定できます。 このような場合は、オブジェクトまたはオブジェクトのセットごとに個別の定義を指定できます。

## <a name="see-also"></a>参照

[View の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-view-format.md)

[View 要素 (書式)](./view-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
