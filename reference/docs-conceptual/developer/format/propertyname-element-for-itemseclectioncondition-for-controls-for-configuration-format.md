---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の ItemSeclectionCondition の PropertyName 要素 (書式)
description: Configuration の Controls の ItemSeclectionCondition の PropertyName 要素 (書式)
ms.openlocfilehash: 860683eb54b2a3579767640c1d3f0937897b8f8e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655127"
---
# <a name="propertyname-element-for-itemseclectioncondition-for-controls-for-configuration-format"></a>Configuration の Controls の ItemSeclectionCondition の PropertyName 要素 (書式)

条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、コントロールが使用されます。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (形式) の CustomControl 要素の構成 (書式設定) の CustomControl の構成 (形式) の CustomEntries 要素を構成するためのコントロールの構成 (形式) の CustomEntries 要素 CustomControl の構成 (形式) のコントロール要素) に対する CustomEntry コントロールの構成式のバインド要素の構成のバインド要素の構成のバインド要素の構成のためのコントロールの構成 (format) 要素のバインド要素の構成用コントロールの ItemSeclectionCondition の PropertyName 要素の構成 (形式)

## <a name="syntax"></a>構文

```xml
<PropertyName>.NetTypeProperty</PropertyName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `PropertyName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md)|このコントロールを使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

条件をトリガーする .NET プロパティの名前を指定します。

## <a name="remarks"></a>解説

この要素が使用されている場合、選択条件を定義するときに [ScriptBlock](./scriptblock-element-for-itemseclectioncondition-for-controls-for-configuration-format.md) 要素を指定することはできません。

## <a name="see-also"></a>参照

[Configuration の Controls の ItemSeclectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-itemseclectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
