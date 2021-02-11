---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomItem の ExpressionBinding 要素 (書式)
description: GroupBy の CustomItem の ExpressionBinding 要素 (書式)
ms.openlocfilehash: 742d9f081a674dc3ee4c84d600933aaf57b2aa6b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655305"
---
# <a name="expressionbinding-element-for-customitem-for-groupby-format"></a>GroupBy の CustomItem の ExpressionBinding 要素 (書式)

コントロールによって表示されるデータを定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素を使用して、groupby (形式) の CustomControl 要素に対して、groupby (format) CustomEntries 要素の CustomControl の CustomControl for groupby (format) の CustomEntry 要素を指定します。 groupby (形式) の Customentries のバインド要素。

## <a name="syntax"></a>構文

```xml
<ExpressionBinding>
  <CustomControl>...</CustomControl>
  <CustomControlName>NameofCommonCustomControl</CustomControlName>
  <EnumerateCollection/>
  <ItemSelectionCondition>...</ItemSelectionCondition>
  <PropertyName>Nameof.NetTypeProperty</PropertyName>
  <ScriptBlock>ScriptToEvaluate></ScriptBlock>
</ExpressionBinding>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ExpressionBinding` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|`CustomControl Element`|省略可能な要素です。<br /><br /> このコントロールによって使用されるコントロールを定義します。|
|[GroupBy の ExpressionBinding の CustomControlName 要素 (書式)](./customcontrolname-element-for-expressionbinding-for-groupby-format.md)|省略可能な要素です。<br /><br /> コモンコントロールまたはビューコントロールの名前を指定します。|
|[GroupBy (Format) の式のバインドの列挙 Atecollection 要素](./enumeratecollection-element-for-expressionbinding-for-groupby-format.md)GroupBy (Format) の式のバインドの列挙 Atecollection 要素|省略可能な要素です。<br /><br /> コレクションの要素が表示されることを指定します。|
|[GroupBy の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-groupby-format.md)|省略可能な要素です。<br /><br /> このコントロールを使用するために必要な条件を定義します。|
|[GroupBy の ExpressionBinding の PropertyName 要素 (書式)](./propertyname-element-for-expressionbinding-for-groupby-format.md)|省略可能な要素です。<br /><br /> コントロールによって表示される値を持つ .NET プロパティを指定します。|
|[GroupBy の ExpressionBinding の ScriptBlock 要素 (書式)](./scriptblock-element-for-expressionbinding-for-groupby-format.md)|省略可能な要素です。<br /><br /> コントロールによって値が表示されるスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-groupby-format.md)|カスタムコントロールビューとその表示方法によって表示されるデータを定義します。|

## <a name="see-also"></a>参照

[GroupBy の ExpressionBinding の CustomControlName 要素 (書式)](./customcontrolname-element-for-expressionbinding-for-groupby-format.md)

[GroupBy の ExpressionBinding の EnumerateCollection 要素 (書式)](./enumeratecollection-element-for-expressionbinding-for-groupby-format.md)

[GroupBy の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-groupby-format.md)

[GroupBy の ExpressionBinding の PropertyName 要素 (書式)](./propertyname-element-for-expressionbinding-for-groupby-format.md)

[GroupBy の ExpressionBinding の ScriptBlock 要素 (書式)](./scriptblock-element-for-expressionbinding-for-groupby-format.md)

[GroupBy の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
