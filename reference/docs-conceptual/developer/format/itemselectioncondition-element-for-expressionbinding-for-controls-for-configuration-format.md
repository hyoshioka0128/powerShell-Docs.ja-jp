---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)
description: Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)
ms.openlocfilehash: 6bd3d386ba64b33a1744fcc9e602cf13404765a0
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648097"
---
# <a name="itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format"></a>Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)

このコントロールを使用するために必要な条件を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (format) コントロールの要素の構成 (format) の CustomControl 要素の構成のためのコントロールの要素の構成 (形式) CustomControl の構成 (書式) の CustomEntries 要素 CustomControl の CustomEntry 要素コントロールのために、CustomEntry 用のカスタム Customentries 要素を構成するためのコントロールに対して、構成のためのコントロールのための構成 (Format) ItemSelectionCondition 要素の構成 (形式) のコントロールのための式のバインド要素のバインド要素

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
|[構成用のコントロールの ItemSelectionCondition の PropertyName 要素 (形式)](./propertyname-element-for-itemseclectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET プロパティを指定します。|
|[構成用のコントロールの ItemSelectionCondition の ScriptBlock 要素 (形式)](./scriptblock-element-for-itemseclectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーするスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)|コントロールによって表示されるデータを定義します。|

## <a name="remarks"></a>解説

この条件には、1つのプロパティ名またはスクリプトを指定できますが、両方を指定することはできません。

## <a name="see-also"></a>参照

[構成用のコントロールの ItemSelectionCondition の PropertyName 要素 (形式)](./propertyname-element-for-itemseclectioncondition-for-controls-for-configuration-format.md)

[構成用のコントロールの ItemSelectionCondition の ScriptBlock 要素 (形式)](./scriptblock-element-for-itemseclectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
