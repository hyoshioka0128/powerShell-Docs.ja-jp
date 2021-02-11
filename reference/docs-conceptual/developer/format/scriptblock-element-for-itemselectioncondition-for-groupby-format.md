---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の ItemSelectionCondition の ScriptBlock 要素 (書式)
description: GroupBy の ItemSelectionCondition の ScriptBlock 要素 (書式)
ms.openlocfilehash: fe366fa31b93e8d69409cc49c3fe2c350d4d06d9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665089"
---
# <a name="scriptblock-element-for-itemselectioncondition-for-groupby-format"></a>GroupBy の ItemSelectionCondition の ScriptBlock 要素 (書式)

条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、コントロールが使用されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素の groupby (format) CustomControl 要素の groupby (形式) CustomControl の CustomControl の CustomEntry 要素に対して、groupby (Format) CustomEntry for groupby (形式) の式のバインド要素 groupby (format) ItemSelectionCondition 要素の場合は、groupby (format) 要素のバインド要素を指定します。 groupby (形式) の ItemSelectionCondition の ScriptBlock (format) ScriptBlock 要素

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
|[GroupBy の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-groupby-format.md)|このコントロールを使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

評価されるスクリプトを指定します。

## <a name="remarks"></a>解説

この要素が使用されている場合、選択条件を定義するときに [PropertyName](./propertyname-element-for-itemselectioncondition-for-groupby-format.md) 要素を指定することはできません。

## <a name="see-also"></a>参照

[GroupBy の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-groupby-format.md)

[GroupBy の ItemSelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-itemselectioncondition-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
