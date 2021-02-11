---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の CustomControl の CustomEntries 要素 (書式)
description: Configuration の Controls の CustomControl の CustomEntries 要素 (書式)
ms.openlocfilehash: 86c2b517d0d7075a355a3190a14e25d9dd4fe374
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655395"
---
# <a name="customentries-element-for-customcontrol-for-controls-for-configuration-format"></a>Configuration の Controls の CustomControl の CustomEntries 要素 (書式)

コモンコントロールの定義を提供します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの configuration (format) CustomControl 要素のコントロール要素を構成するためのコントロールの構成 (形式) のコントロール要素を構成するための CustomControl の構成 (format) の CustomEntries 要素の構成 (形式)

## <a name="syntax"></a>構文

```xml
<CustomEntries>
  <CustomEntry>...</CustomEntry>
</CustomEntries>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomEntries` ます。 1つまたは複数の子要素を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-controls-for-configuration-format.md)|コモンコントロールの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[構成用のコントロールの CustomControl 要素 (形式)](./customcontrol-element-for-control-for-controls-for-configuration-format.md)|共通コントロールを定義します。|

## <a name="remarks"></a>解説

ほとんどの場合、コントロールには定義が1つだけあります。定義は1つの要素で定義されてい `CustomEntry` ます。 ただし、同じコントロールを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を含めることができます。 そのような場合は、 `CustomEntry` オブジェクトまたはオブジェクトのセットごとに要素を定義できます。

## <a name="see-also"></a>参照

[構成用のコントロールの CustomControl 要素 (形式)](./customcontrol-element-for-control-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
