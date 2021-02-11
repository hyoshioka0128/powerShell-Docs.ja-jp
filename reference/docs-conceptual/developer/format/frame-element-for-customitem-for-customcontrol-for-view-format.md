---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の CustomItem の Frame 要素 (書式)
description: View の CustomControl の CustomItem の Frame 要素 (書式)
ms.openlocfilehash: 1ffe071bb6c4f590e4c79803a3faff2108c7b636
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652195"
---
# <a name="frame-element-for-customitem-for-customcontrol-for-view-format"></a>View の CustomControl の CustomItem の Frame 要素 (書式)

データを左右に移動するなど、データの表示方法を定義します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) CustomControl 要素 (形式) CustomControl for view (format) の customentries 要素を表示するための CustomEntries for CustomControl for CustomControlView (Format) Frame Element for for View (Format) の Customentries 要素

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
|[FirstLineHanging 要素](./firstlinehanging-element-for-frame-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> データの最初の行を左にシフトする文字数を指定します。|
|[FirstLineIndent 要素](./firstlineindent-element-for-frame-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> データの最初の行を右にシフトする文字数を指定します。|
|[左インデント要素](./leftindent-element-for-frame-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> データを左余白から移動する文字数を指定します。|
|[右インデント要素](./rightindent-element-for-frame-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> データを右余白から移動する文字数を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[CustomEntry for ビューの CustomItem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md)|コントロールによって表示されるデータとその表示方法を定義します。|

## <a name="remarks"></a>解説

同じ要素で [Firstlinehanging](./firstlinehanging-element-for-frame-for-customcontrol-for-view-format.md) と [firstlinehanging](./firstlineindent-element-for-frame-for-customcontrol-for-view-format.md) 要素を指定することはできません `Frame` 。

## <a name="see-also"></a>参照

[FirstLineHanging 要素](./firstlinehanging-element-for-frame-for-customcontrol-for-view-format.md)

[FirstLineIndent 要素](./firstlineindent-element-for-frame-for-customcontrol-for-view-format.md)

[左インデント要素](./leftindent-element-for-frame-for-customcontrol-for-view-format.md)

[右インデント要素](./rightindent-element-for-frame-for-customcontrol-for-view-format.md)

[CustomEntry for ビューの CustomItem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
