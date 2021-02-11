---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomItem の Frame 要素 (書式)
description: GroupBy の CustomItem の Frame 要素 (書式)
ms.openlocfilehash: 86b51766974ebfcae06583ade237a77c6db92866
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652168"
---
# <a name="frame-element-for-customitem-for-groupby-format"></a>GroupBy の CustomItem の Frame 要素 (書式)

データを左右に移動するなど、データの表示方法を定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素については、groupby (format) の CustomControl 要素に対して、groupby (形式) の CustomEntries 要素を指定します。 groupby (書式) の customentries 要素については、groupby (format) の Customentries の groupby (format) フレーム要素の CustomControl

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
|[GroupBy の Frame の FirstLineHanging 要素 (書式)](./firstlinehanging-element-for-frame-for-groupby-format.md)|省略可能な要素です。<br /><br /> データの最初の行を左にシフトする文字数を指定します。|
|[GroupBy の Frame の FirstLineIndent 要素 (書式)](./firstlineindent-element-for-frame-for-groupby-format.md)|省略可能な要素です。<br /><br /> データの最初の行を右にシフトする文字数を指定します。|
|[GroupBy の Frame の LeftIndent 要素 (書式)](./leftindent-element-for-frame-for-groupby-format.md)|省略可能な要素です。<br /><br /> データを左余白から移動する文字数を指定します。|
|[GroupBy (Format) の Frame の右インデント要素](./rightindent-element-for-frame-for-groupby-format.md)右インデント要素|省略可能な要素です。<br /><br /> データを右余白から移動する文字数を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-groupby-format.md)|コントロールによって表示されるデータとその表示方法を定義します。|

## <a name="remarks"></a>解説

同じ要素で [Firstlinehanging](./firstlinehanging-element-for-frame-for-groupby-format.md) と [firstlinehanging](./firstlineindent-element-for-frame-for-groupby-format.md) 要素を指定することはできません `Frame` 。

## <a name="see-also"></a>参照

[GroupBy の Frame の FirstLineHanging 要素 (書式)](./firstlinehanging-element-for-frame-for-groupby-format.md)

[GroupBy の Frame の FirstLineIndent 要素 (書式)](./firstlineindent-element-for-frame-for-groupby-format.md)

[GroupBy の Frame の LeftIndent 要素 (書式)](./leftindent-element-for-frame-for-groupby-format.md)

[GroupBy の Frame の RightIndent 要素 (書式)](./rightindent-element-for-frame-for-groupby-format.md)

[GroupBy の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
