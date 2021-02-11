---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の Control の Name 要素 (書式)
description: Configuration の Controls の Control の Name 要素 (書式)
ms.openlocfilehash: 0c1c83f827482886ca742f2c0174e8383f87fb52
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666503"
---
# <a name="name-element-for-control-for-controls-for-configuration-format"></a>Configuration の Controls の Control の Name 要素 (書式)

コントロールの名前を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (書式) のコントロール要素の構成 (format) コントロールの要素の構成 (形式) コントロールの要素

## <a name="syntax"></a>構文

```xml
<Name>NameOfControl</Name>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Name` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の Control 要素 (書式)](./control-element-for-controls-for-configuration-format.md)|書式設定ファイルのすべてのビューおよびコントロールを参照するために使用される名前によって使用できるコモンコントロールを定義します。|

## <a name="text-value"></a>テキスト値

このコントロールを参照するために使用する名前を指定します。

## <a name="remarks"></a>解説

ここで指定した名前は、このコントロールを参照するために次の要素で使用できます。

- テーブル、リスト、ワイドコントロール、またはカスタムコントロールビューを作成するときに、コントロールを指定するには、次の要素を使用します。 [GroupBy 要素 (Format)](./groupby-element-for-view-format.md)

- 別のコモンコントロールを作成するときに、このコントロールを指定するには、次の要素を使用します。[構成するコントロールの CustomItem の式のバインド要素](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)です。

- ビューで使用できるコントロールを作成する場合、このコントロールは、[ビュー (Format) のコントロールの CustomItem の式のバインド要素](./expressionbinding-element-for-customitem-for-controls-for-view-format.md)によって指定できます。

## <a name="see-also"></a>参照

[Configuration の Controls の Control 要素 (書式)](./control-element-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

[View の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-view-format.md)

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
