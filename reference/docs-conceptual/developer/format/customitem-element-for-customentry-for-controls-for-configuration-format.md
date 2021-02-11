---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の CustomEntry の CustomItem 要素 (書式)
description: Configuration の Controls の CustomEntry の CustomItem 要素 (書式)
ms.openlocfilehash: 06c399e982b6ac0fba9c11e20c468fe8bef6f694
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666775"
---
# <a name="customitem-element-for-customentry-for-controls-for-configuration-format"></a>Configuration の Controls の CustomEntry の CustomItem 要素 (書式)

コントロールによって表示されるデータとその表示方法を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (format) コントロールの要素の構成 (形式) の CustomControl 要素の構成のためのコントロールの構成 (形式) の CustomEntries 要素を構成するための CustomControl の構成 (形式) の Custommentry 要素を構成するための CustomEntry の構成 (形式) Customentries 要素

## <a name="syntax"></a>構文

```xml
<CustomItem>
  <ExpressionBinding>...</ExpressionBinding>
  <NewLine/>
  <Text>TextToDisplay</Text>
  <Frame>...</Frame>
</CustomItem>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomItem` ます。 詳細については、「解説」を参照してください。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コントロールによって表示されるデータを定義します。|
|[Configuration の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> データを左右に移動するなど、データの表示方法を定義します。|
|[Configuration の Controls の CustomItem の NewLine 要素 (書式)](./newline-element-for-customitem-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コントロールの表示に空白行を追加します。|
|[Configuration の Controls の CustomItem の Text 要素 (書式)](./text-element-for-customitem-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> かっこや角かっこなどのテキストをコントロールの表示に追加します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-controls-for-configuration-format.md)|コントロールの定義を提供します。|

## <a name="remarks"></a>解説

要素の子要素を指定する場合 `CustomItem` は、次の点に注意してください。

- 子要素は、、、、およびの順に追加する必要があり `ExpressionBinding` `NewLine` `Text` `Frame` ます。

- 指定できるシーケンスの数に上限はありません。

- 各シーケンスには、使用できる要素の数に上限がありません `ExpressionBinding` 。

## <a name="see-also"></a>参照

[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomItem の NewLine 要素 (書式)](./newline-element-for-customitem-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomItem の Text 要素 (書式)](./text-element-for-customitem-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
