---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の ItemSelectionCondition の ScriptBlock 要素 (書式)
description: View の Controls の ItemSelectionCondition の ScriptBlock 要素 (書式)
ms.openlocfilehash: c005215f7b7984541806d2f5de47372d536787ff
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665185"
---
# <a name="scriptblock-element-for-itemselectioncondition-for-controls-for-view-format"></a>View の Controls の ItemSelectionCondition の ScriptBlock 要素 (書式)

条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、コントロールが使用されます。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (形式) コントロールのコントロールの要素 (書式) のコントロールの要素 (形式) コントロールのコントロール要素を表示するためのコントロールの CustomControl 要素 (形式) の CustomControl for View (書式) の Custommentry 要素コントロールの CustomEntries for view (format) Customentries 要素を使用して、ビュー (形式) の式のバインド要素を表示するためのコントロールの、ビュー (format) ItemSelectionCondition 要素の要素を表示するためのコントロールのビュー (format) ScriptBlock 要素のバインド要素を表示するコントロールの ItemSelectionCondition の要素を表示 (Format)

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
|[ビューのコントロール (Format) の式のバインドの ItemSelectionCondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)|このコントロールを使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

評価されるスクリプトを指定します。

## <a name="remarks"></a>解説

この要素が使用されている場合、選択条件を定義するときに [PropertyName](./propertyname-element-for-itemselectioncondition-for-controls-for-view-format.md) 要素を指定することはできません。

## <a name="see-also"></a>参照

[View の Controls の ItemSelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-itemselectioncondition-for-controls-for-view-format.md)

[ビューのコントロール (Format) の式のバインドの ItemSelectionCondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
