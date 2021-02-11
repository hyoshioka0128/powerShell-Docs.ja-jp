---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の CustomItem の ExpressionBinding 要素 (書式)
description: View の Controls の CustomItem の ExpressionBinding 要素 (書式)
ms.openlocfilehash: da87bb26d21dcb051871e67997cc3fba7ce73c74
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649893"
---
# <a name="expressionbinding-element-for-customitem-for-controls-for-view-format"></a>View の Controls の CustomItem の ExpressionBinding 要素 (書式)

コントロールによって表示されるデータを定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (書式) コントロールのコントロール要素を表示するためのコントロールの要素 (形式) コントロール要素 (format) の CustomControl の CustomControl 要素 view (Format) CustomEntry 要素を使用して、ビュー (形式) のコントロールの Customentries の View (format) 式のバインド要素のビュー (形式) の Customentries 要素を表示します。

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
|[View の Controls の ExpressionBinding の CustomControlName 要素 (書式)](./customcontrolname-element-for-expressionbinding-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コモンコントロールまたはビューコントロールの名前を指定します。|
|[View の Controls の ExpressionBinding の EnumerateCollection 要素 (書式)](./enumeratecollection-element-for-expressionbinding-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コレクションの要素を表示することを指定します。|
|[ビューのコントロール (Format) の式のバインドの ItemSelectionCondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> このコントロールを使用するために必要な条件を定義します。|
|[View の Controls の ExpressionBinding の PropertyName 要素 (書式)](./propertyname-element-for-expressionbinding-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールによって表示される値を持つ .NET プロパティを指定します。|
|[View の Controls の ExpressionBinding の ScriptBlock 要素 (書式)](./scriptblock-element-for-expressionbinding-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> コントロールによって値が表示されるスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-controls-for-view-format.md)|コントロールによって表示されるデータとその表示方法を定義します。|

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[View の Controls の CustomEntry の CustomItem 要素 (書式)](./customitem-element-for-customentry-for-controls-for-view-format.md)

[View の Controls の ExpressionBinding の CustomControlName 要素 (書式)](./customcontrolname-element-for-expressionbinding-for-controls-for-view-format.md)

[View の Controls の ExpressionBinding の EnumerateCollection 要素 (書式)](./enumeratecollection-element-for-expressionbinding-for-controls-for-view-format.md)

[ビューのコントロール (Format) の式のバインドの ItemSelectionCondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)

[View の Controls の ExpressionBinding の PropertyName 要素 (書式)](./propertyname-element-for-expressionbinding-for-controls-for-view-format.md)

[View の Controls の ExpressionBinding の ScriptBlock 要素 (書式)](./scriptblock-element-for-expressionbinding-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
