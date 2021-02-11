---
ms.date: 09/13/2016
ms.topic: reference
title: CustomControl の ExpressionBinding の ItemSelectionCondition 要素 (書式)
description: CustomControl の ExpressionBinding の ItemSelectionCondition 要素 (書式)
ms.openlocfilehash: 38c88ad47e57cd20902c6b8aabdb0b8e8b682ba4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648040"
---
# <a name="itemselectioncondition-element-for-expressionbinding-for-customcontrol-format"></a>CustomControl の ExpressionBinding の ItemSelectionCondition 要素 (書式)

このコントロールを使用するために必要な条件を定義します。 コントロール項目に指定できる選択条件の数に制限はありません。 この要素は、カスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) view Element (Format) CustomControl 要素 (Format) ビューの CustomEntries の CustomEntry 要素の CustomControl for view (format) の CustomEntries 要素を表示します。) CustomControl for View (Format) の式のバインドのための CustomControl for view (format) ItemSelectionCondition 要素の customentries 要素に対する CustomEntry for view (format) 式のバインド要素のための Customentries 要素

## <a name="syntax"></a>構文

```xml
<ItemSelectionCondition>
  <PropertyName>.NetTypeProperty</PropertyName>
  <ScriptBlock>ScriptToEvaluate</ScriptBlock>
</ItemSelectionCondition>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ItemSelectionCondition` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ビューの CustomControl の ItemSelectionCondition の PropertyName 要素 (形式)](./propertyname-element-for-itemselectioncondition-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET プロパティを指定します。|
|[View の CustomControl の ItemSelectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-itemselectioncondition-for-customcontrol-for-view-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーするスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の CustomControl の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-customcontrol-for-view-format.md)|コントロールによって表示されるデータを定義します。|

## <a name="remarks"></a>解説

この条件には、1つのプロパティ名またはスクリプトを指定できますが、両方を指定することはできません。

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)

[View の CustomControl の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-customcontrol-for-view-format.md)
