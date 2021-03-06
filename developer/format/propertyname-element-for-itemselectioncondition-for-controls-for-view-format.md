---
title: コントロールが表示されます (形式) の ItemSelectionCondition PropertyName 要素 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: ba3955bc-f3a1-4ef6-86ac-80ffc133ad1b
caps.latest.revision: 6
ms.openlocfilehash: 28ad31be4be7be20f1f43ea1b69ad5d294de86f6
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62075853"
---
# <a name="propertyname-element-for-itemselectioncondition-for-controls-for-view-format"></a>View の Controls の ItemSelectionCondition の PropertyName 要素 (書式)

条件をトリガーする、.NET プロパティを指定します。 このプロパティが存在するかを評価すると`true`条件が満たされると、およびコントロールを使用します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

要素 (形式) ViewDefinitions 要素 (形式) 表示要素 (形式) コントロール要素 (形式) コントロールの構成要素のビュー (形式) CustomEntries 要素のコントロールのコントロールのビュー (形式) のカスタム コントロール要素のコントロールCustomEntries CustomEntry CustomItem ビュー (形式) のコントロールのビュー (形式) ExpressionBinding 要素のコントロールのビュー (形式) CustomItem 要素のコントロールのビュー (形式) CustomEntry 要素のカスタム コントロールExpressionBinding ItemSelectionCondition ビュー (形式) のコントロールのビュー (形式) PropertyName 要素のコントロールの ItemSelectionCondition 要素

## <a name="syntax"></a>構文

```xml
<PropertyName>.NetTypeProperty</PropertyName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、属性、子要素、およびの親要素について説明します、`PropertyName`要素。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[コントロールが表示されます (形式) の ExpressionBinding の ItemSelectionCondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)|このコントロールを使用するのに必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

条件をトリガーする、.NET プロパティの名前を指定します。

## <a name="remarks"></a>コメント

指定できないかどうかは、この要素が使用される、 [ScriptBlock](./scriptblock-element-for-itemselectioncondition-for-controls-for-view-format.md)要素の選択条件を定義するときにします。

## <a name="see-also"></a>参照

[コントロールが表示されます (形式) の ItemSelectionCondition の ScriptBlock 要素](./scriptblock-element-for-itemselectioncondition-for-controls-for-view-format.md)

[コントロールが表示されます (形式) の ExpressionBinding の ItemSelectionCondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)

[PowerShell のファイルを書式設定の書き込み](./writing-a-powershell-formatting-file.md)
