---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の Frame の FirstLineHanging 要素 (書式)
description: View の Controls の Frame の FirstLineHanging 要素 (書式)
ms.openlocfilehash: a7a2aa533a74bd3c347307ab49a467d1f9844fc3
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655206"
---
# <a name="firstlinehanging-element-for-frame-for-controls-for-view-format"></a>View の Controls の Frame の FirstLineHanging 要素 (書式)

データの最初の行を左にシフトする文字数を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロール要素 (形式) コントロールのコントロールの要素 (形式) コントロールの要素 (書式) を制御するためのコントロールの CustomControl 要素 (format) CustomControl for ビュー (形式) の CustomEntries 要素ビュー (format) のコントロールの CustomEntries のカスタムコントロールのビュー (format) Frame 要素のコントロールのビュー (形式) のフレーム要素を表示するためのコントロールの Customentries 要素を使用します。ビューのコントロールのフレームのビュー (Format) の Frame 要素を指定します。

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
|[View の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-view-format.md)|データを左右に移動するなど、データの表示方法を定義します。|

## <a name="text-value"></a>テキスト値

データの最初の行をシフトする文字数を指定します。

## <a name="remarks"></a>解説

この要素が指定されている場合、 [Firstlineindent](./firstlineindent-element-for-frame-for-controls-for-view-format.md) 要素を指定することはできません。

## <a name="see-also"></a>参照

[View の Controls の Frame の FirstLineIndent 要素 (書式)](./firstlineindent-element-for-frame-for-controls-for-view-format.md)

[View の Controls の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
