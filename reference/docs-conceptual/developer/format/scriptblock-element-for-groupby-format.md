---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の ScriptBlock 要素 (書式)
description: GroupBy の ScriptBlock 要素 (書式)
ms.openlocfilehash: 117cbef93889046626741449886a1caaa39815cb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665245"
---
# <a name="scriptblock-element-for-groupby-format"></a>GroupBy の ScriptBlock 要素 (書式)

値が変更されるたびに新しいグループを開始するスクリプトを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビューの要素 (形式) GroupBy 要素のビュー (形式) の groupby 要素

## <a name="syntax"></a>構文

```xml
<ScriptBlock>ScriptToEvaluate</ScriptBlock>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ScriptBlock` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)|.NET オブジェクトのグループを表示する方法を定義します。|

## <a name="text-value"></a>テキスト値

評価されるスクリプトを指定します。

## <a name="remarks"></a>解説

このスクリプトの値が変更されるたびに、PowerShell によって新しいグループが開始されます。

この要素が指定されている場合、 [PropertyName](propertyname-element-for-groupby-format.md) 要素を指定して新しいグループを開始することはできません。

## <a name="see-also"></a>参照

[GroupBy の PropertyName 要素 (書式)](propertyname-element-for-groupby-format.md)

[View の GroupBy 要素 (書式)](groupby-element-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](writing-a-powershell-formatting-file.md)
