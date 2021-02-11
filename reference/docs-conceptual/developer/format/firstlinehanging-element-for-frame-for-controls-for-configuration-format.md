---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の Frame の FirstLineHanging 要素 (書式)
description: Configuration の Controls の Frame の FirstLineHanging 要素 (書式)
ms.openlocfilehash: 94d59ef7b54e036f76e38a3b06b769700443b9fb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655236"
---
# <a name="firstlinehanging-element-for-frame-for-controls-for-configuration-format"></a>Configuration の Controls の Frame の FirstLineHanging 要素 (書式)

データの最初の行を左にシフトする文字数を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの configuration (format) コントロールの要素の構成 (format) の CustomControl 要素の構成 (形式) の CustomControl の構成 (書式) の CustomEntries 要素のコントロール要素 CustomControl for Controls の構成 (フォーマット) Customentries 要素を使用した CustomEntry コントロールの構成フレーム要素の構成のためのコントロールの構成 (形式) の FirstLineHanging 要素の構成用コントロールのフレーム (形式)

## <a name="syntax"></a>構文

```xml
<FirstLineHanging>NumberOfCharactersToShift</FirstLineHanging>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `FirstLineHanging` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-configuration-format.md)|データを左右に移動するなど、データの表示方法を定義します。|

## <a name="text-value"></a>テキスト値

データの最初の行をシフトする文字数を指定します。

## <a name="remarks"></a>解説

この要素が指定されている場合、要素を指定することはできません `FirstLineIndent` 。

## <a name="see-also"></a>参照

[Configuration の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
