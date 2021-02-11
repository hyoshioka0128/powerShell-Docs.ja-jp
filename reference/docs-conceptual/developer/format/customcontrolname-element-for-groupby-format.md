---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomControlName 要素 (書式)
description: GroupBy の CustomControlName 要素 (書式)
ms.openlocfilehash: 03664fe4d5559312e2720a3892493c90c15f7501
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655420"
---
# <a name="customcontrolname-element-for-groupby-format"></a>GroupBy の CustomControlName 要素 (書式)

新しいグループを表示するために使用されるカスタムコントロールの名前を指定します。 この要素は、テーブル、リスト、ワイド、またはカスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) GroupBy 要素 (format) の CustomControlName 要素を指定します。

## <a name="syntax"></a>構文

```xml
<CustomControlName>ControlName</CustomControlName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomControlName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)|Windows PowerShell がオブジェクトの新しいグループを表示する方法を定義します。|

## <a name="text-value"></a>テキスト値

新しいグループの表示に使用されるカスタムコントロールの名前を指定します。

## <a name="remarks"></a>解説

書式設定ファイルのすべてのビューで使用できるコモンコントロールを作成できます。また、特定のビューで使用できるビューコントロールを作成することもできます。 次の要素は、これらのカスタムコントロールの名前を指定します。

- [Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

- [View の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-view-format.md)

## <a name="see-also"></a>参照

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[Configuration の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-configuration-format.md)

[View の Controls の Control の Name 要素 (書式)](./name-element-for-control-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
