---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の CustomEntries 要素 (書式)
description: View の CustomControl の CustomEntries 要素 (書式)
ms.openlocfilehash: 6e757bccdface2503667f8786462a2b43134a07d
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649981"
---
# <a name="customentries-element-for-customcontrol-for-view-format"></a>View の CustomControl の CustomEntries 要素 (書式)

カスタムコントロールビューの定義を提供します。 カスタムコントロールビューでは、1つまたは複数の定義を指定する必要があります。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) CustomControl Element (Format) CustomControl for View (Format) の CustomEntries 要素

## <a name="syntax"></a>構文

```xml
<CustomEntries>
  <CustomEntry>...</CustomEntry>
</CustomEntries>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomControlEntries` ます。 1つまたは複数の子要素を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[View (Format) の CustomEntries の CustomEntry 要素](./customentry-element-for-customentries-for-customcontrol-for-view-format.md)|必須の要素です。<br /><br /> カスタムコントロールビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の CustomControl 要素 (書式)](./customcontrol-element-for-view-format.md)|必須の要素です。<br /><br /> ビューのカスタムコントロール形式を定義します。|

## <a name="remarks"></a>解説

ほとんどの場合、コントロールには定義が1つだけあります。定義は1つの要素で定義されてい `CustomEntry` ます。 ただし、同じコントロールを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を含めることができます。 そのような場合は、 `CustomEntry` オブジェクトまたはオブジェクトのセットごとに要素を定義できます。

## <a name="see-also"></a>参照

[View の CustomControl 要素 (書式)](./customcontrol-element-for-view-format.md)

[View (Format) の CustomEntries の CustomEntry 要素](./customentry-element-for-customentries-for-customcontrol-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
