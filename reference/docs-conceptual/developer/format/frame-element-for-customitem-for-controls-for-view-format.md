---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の CustomItem の Frame 要素 (書式)
description: View の Controls の CustomItem の Frame 要素 (書式)
ms.openlocfilehash: 6f26e19a6894ac213b924108a56cb80f9ffd1143
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652213"
---
# <a name="frame-element-for-customitem-for-controls-for-view-format"></a>View の Controls の CustomItem の Frame 要素 (書式)

データを左右に移動するなど、データの表示方法を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (形式) コントロールのコントロール要素を表示するためのコントロールの CustomControl 要素 (format) CustomControl のビュー (書式) の CustomEntries 要素 for view (format) CustomEntry 要素を使用して、コントロールのカスタム項目のビュー (形式) の Customentries 要素を表示するためのコントロールの Customentries の Frame 要素を表示するためのコントロール (形式)

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
|[ビューのコントロールのフレームの FirstLineHanging 要素 (形式)](./firstlinehanging-element-for-frame-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> 最初の行を左にシフトする文字数を指定します。|
|[ビューのコントロールのフレームの FirstLineIndent 要素 (Format)](./firstlineindent-element-for-frame-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> 最初の行を右にシフトする文字数を指定します。|
|[ビューのコントロールのフレームの左インデント要素 (書式)](./leftindent-element-for-frame-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> データを左余白から移動する文字数を指定します。|
|[ビューのコントロールのフレームの右インデント要素 (書式)](./rightindent-element-for-frame-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> データを右余白から移動する文字数を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-controls-for-view-format.md)|コントロールによって表示されるデータとその表示方法を定義します。|

## <a name="remarks"></a>解説

同じ要素で [Firstlinehanging](./firstlinehanging-element-for-frame-for-controls-for-view-format.md) と [firstlinehanging](./firstlineindent-element-for-frame-for-controls-for-view-format.md) 要素を指定することはできません `Frame` 。

## <a name="see-also"></a>参照

[ビューのコントロールのフレームの FirstLineHanging 要素 (形式)](./firstlinehanging-element-for-frame-for-controls-for-view-format.md)

[ビューのコントロールのフレームの FirstLineIndent 要素 (Format)](./firstlineindent-element-for-frame-for-controls-for-view-format.md)

[ビューのコントロールのフレームの左インデント要素 (書式)](./leftindent-element-for-frame-for-controls-for-view-format.md)

[ビューのコントロールのフレームの右インデント要素 (書式)](./rightindent-element-for-frame-for-controls-for-view-format.md)

[View の Controls の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
