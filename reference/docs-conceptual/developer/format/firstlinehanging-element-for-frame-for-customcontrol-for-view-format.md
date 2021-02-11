---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の Frame の FirstLineHanging 要素 (書式)
description: View の CustomControl の Frame の FirstLineHanging 要素 (書式)
ms.openlocfilehash: 8b9601b2afd7ab5523e339fb45119f5cf9f4a535
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648143"
---
# <a name="firstlinehanging-element-for-frame-for-customcontrol-for-view-format"></a>View の CustomControl の Frame の FirstLineHanging 要素 (書式)

データの最初の行を左にシフトする文字数を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) view Element (Format) CustomControl 要素 (Format) ビューの CustomEntries の CustomEntry 要素の CustomControl for view (format) の CustomEntries 要素を表示します。) CustomControl for View (Format) の Frame の CustomControl for ビュー (Format) FirstLineHanging 要素の customentries 要素の CustomEntry for CustomControlView (format) Frame 要素の Customentries 要素

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
|[View の CustomControl の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-customcontrol-for-view-format.md)|データを左右に移動するなど、データの表示方法を定義します。|

## <a name="text-value"></a>テキスト値

データの最初の行をシフトする文字数を指定します。

## <a name="remarks"></a>解説

この要素が指定されている場合、 [Firstlineindent](./firstlineindent-element-for-frame-for-customcontrol-for-view-format.md) 要素を指定することはできません。

## <a name="see-also"></a>参照

[View の CustomControl の Frame の FirstLineIndent 要素 (書式)](./firstlineindent-element-for-frame-for-customcontrol-for-view-format.md)

[View の CustomControl の CustomItem の Frame 要素 (書式)](./frame-element-for-customitem-for-customcontrol-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
