---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の CustomEntry の CustomItem 要素 (書式)
description: View の Controls の CustomEntry の CustomItem 要素 (書式)
ms.openlocfilehash: 67bff97c27cfedf046b17eea438efcd66ae2ee4a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666758"
---
# <a name="customitem-element-for-customentry-for-controls-for-view-format"></a>View の Controls の CustomEntry の CustomItem 要素 (書式)

コントロールによって表示されるデータとその表示方法を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビューのコントロール要素 (format) コントロールの要素 (書式) コントロールのコントロール要素 (format) のコントロール要素 (形式)) の CustomEntries 要素を使用して、CustomControl for view (format) の CustomEntry 要素を使用して、ビュー (Format) のコントロールのためのビュー (format) Customentries 要素を表示します。

## <a name="syntax"></a>構文

```xml
<CustomItem>
  <ExpressionBinding>...</ExpressionBinding>
  <NewLine/>
  <Text>TextToDisplay</Text>
  <Frame>...<Frame>
</CustomItem>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomItem` ます。 詳細については、「解説」を参照してください。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールによって表示されるデータを定義します。|
|[View の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> データを左右に移動するなど、データの表示方法を定義します。|
|[View の Controls の CustomItem の NewLine 要素 (書式)](./newline-element-for-customitem-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールの表示に空白行を追加します。|
|[View の Controls の CustomItem の Text 要素 (書式)](./text-element-for-customitem-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> かっこや角かっこなどのテキストをコントロールの表示に追加します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomEntries の CustomEntry 要素 (書式)](./customentry-element-for-customentries-for-controls-for-view-format.md)|コントロールの定義を提供します。|

## <a name="remarks"></a>解説

要素の子要素を指定する場合 `CustomItem` は、次の点に注意してください。

- 子要素は、、、、およびの順に追加する必要があり `ExpressionBinding` `NewLine` `Text` `Frame` ます。

- 指定できるシーケンスの数に上限はありません。

- 各シーケンスには、使用できる要素の数に上限がありません `ExpressionBinding` 。

## <a name="see-also"></a>参照

[View の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-view-format.md)

[View の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-view-format.md)

[View の Controls の CustomItem の NewLine 要素 (書式)](./newline-element-for-customitem-for-controls-for-view-format.md)

[View の Controls の CustomItem の Text 要素 (書式)](./text-element-for-customitem-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
