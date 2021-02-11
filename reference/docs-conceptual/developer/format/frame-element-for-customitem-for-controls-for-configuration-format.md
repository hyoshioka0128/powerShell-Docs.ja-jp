---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の CustomItem の Frame 要素 (書式)
description: Configuration の Controls の CustomItem の Frame 要素 (書式)
ms.openlocfilehash: 85d095b9b0c25b68b2353bce56b85333aff91b98
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652232"
---
# <a name="frame-element-for-customitem-for-controls-for-configuration-format"></a>Configuration の Controls の CustomItem の Frame 要素 (書式)

データを左右に移動するなど、データの表示方法を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (format) コントロールの要素の構成 (形式) の CustomControl 要素の構成のためのコントロールの構成 (書式) の CustomEntries 要素の構成のための CustomControl の構成 (書式) のカスタム Customentries 要素を構成するためのコントロールの configuration (形式) customentries 要素を構成するためのコントロールの構成 (形式)

## <a name="syntax"></a>構文

```xml
<Frame>
  <LeftIndent>NumberOfCharactersToShift</LeftIndent>
  <RightIndent>NumberOfCharactersToShift</RightIndent>
  <FirstLineHanging>NumberOfCharactersToShift</FirstLineHanging>
  <FirstLineIndent>NumberOfCharactersToShift</FirstLineIndent>
  <CustomItem>...</CustomItem>
</Frame>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Frame` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|`CustomItem Element`|必須の要素|
|[Configuration の Controls の Frame の FirstLineHanging 要素 (書式)](./firstlinehanging-element-for-frame-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> データの最初の行を左にシフトする文字数を指定します。|
|[Configuration の Controls の Frame の FirstLineIndent 要素 (書式)](./firstlineindent-element-for-frame-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> データの最初の行を右にシフトする文字数を指定します。|
|[Configuration の Controls の Frame の LeftIndent 要素 (書式)](./leftindent-element-for-frame-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> データを左余白から移動する文字数を指定します。|
|[Configuration の Controls の Frame の RightIndent 要素 (書式)](./rightindent-element-for-frame-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> データを右余白から移動する文字数を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[構成のための CustomEntry コントロールの CustomItem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md)|コントロールによって表示されるデータとその表示方法を定義します。|

## <a name="remarks"></a>解説

同じ要素で [Firstlinehanging](./firstlinehanging-element-for-frame-for-controls-for-configuration-format.md) と [firstlinehanging](./firstlineindent-element-for-frame-for-controls-for-configuration-format.md) 要素を指定することはできません `Frame` 。

## <a name="see-also"></a>参照

[Configuration の Controls の Frame の FirstLineHanging 要素 (書式)](./firstlinehanging-element-for-frame-for-controls-for-configuration-format.md)

[Configuration の Controls の Frame の FirstLineIndent 要素 (書式)](./firstlineindent-element-for-frame-for-controls-for-configuration-format.md)

[Configuration の Controls の Frame の LeftIndent 要素 (書式)](./leftindent-element-for-frame-for-controls-for-configuration-format.md)

[Configuration の Controls の Frame の RightIndent 要素 (書式)](./rightindent-element-for-frame-for-controls-for-configuration-format.md)

[構成のための CustomEntry コントロールの CustomItem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
