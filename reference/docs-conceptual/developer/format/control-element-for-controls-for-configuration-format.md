---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の Control 要素 (書式)
description: Configuration の Controls の Control 要素 (書式)
ms.openlocfilehash: 43424de025cb89d81533e973abd7c39c09533747
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655654"
---
# <a name="control-element-for-controls-for-configuration-format"></a>Configuration の Controls の Control 要素 (書式)

書式設定ファイルのすべてのビューおよびコントロールを参照するために使用される名前によって使用できるコモンコントロールを定義します。

Configuration 要素 (Format) コントロールの構成 (書式) のコントロール要素の Configuration (書式) コントロール要素

## <a name="syntax"></a>構文

```xml
<Control>
  <Name>NameOfControl</Name>
  <CustomControl>...</CustomControl>
</Control>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Control` ます。 各子要素のうち1つだけを指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の Control の CustomControl 要素 (書式)](./customcontrol-element-for-control-for-controls-for-configuration-format.md)|必須の要素です。<br /><br /> コントロールを定義します。|
|[Configuration のコントロールの Name 要素 (Format)](./name-element-for-control-for-controls-for-configuration-format.md)|必須の要素です。<br /><br /> コントロールを参照するために使用する名前を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls 要素 (Format)](./controls-element-for-configuration-format.md)|書式設定ファイルのすべてのビューまたはその他のコントロールで使用できるコモンコントロールを定義します。|

## <a name="remarks"></a>解説

このコントロールに指定される名前は、次の要素で参照できます。

- [CustomItem の式のバインド要素 (Format)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

- [ビューの GroupBy 要素 (Format)](./groupby-element-for-view-format.md)

## <a name="see-also"></a>参照

[Configuration の Controls 要素 (Format)](./controls-element-for-configuration-format.md)

[構成用のコントロールの CustomControl 要素 (形式)](./customcontrol-element-for-control-for-controls-for-configuration-format.md)

[CustomItem の式のバインド要素 (Format)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

[ビューの GroupBy 要素 (Format)](./groupby-element-for-view-format.md)

[Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
