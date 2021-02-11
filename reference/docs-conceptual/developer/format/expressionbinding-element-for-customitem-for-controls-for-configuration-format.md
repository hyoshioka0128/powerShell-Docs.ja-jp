---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)
description: Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)
ms.openlocfilehash: 1abcf2173e18d45839161b4c7e59f1ad238f81a1
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655338"
---
# <a name="expressionbinding-element-for-customitem-for-controls-for-configuration-format"></a>Configuration の Controls の CustomItem の ExpressionBinding 要素 (書式)

コントロールによって表示されるデータを定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (format) コントロールの要素の構成 (format) の CustomControl 要素の構成のためのコントロール要素の構成 (書式) の CustomControl の構成 (形式) のカスタム Customentries 要素の構成のためのコントロールの構成式の構成 (書式設定) のカスタム項目のバインド要素の構成 (形式)

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
|[Configuration の Controls の ExpressionBinding の CustomControlName 要素 (書式)](./customcontrolname-element-for-expressionbinding-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コモンコントロールまたはビューコントロールの名前を指定します。|
|[Configuration の Controls の ExpressionBinding の EnumerateCollection 要素 (書式)](./enumeratecollection-element-for-expressionbinding-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コレクションの要素がコントロールによって表示されることを指定します。|
|[Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> このコモンコントロールを使用するために必要な条件を定義します。|
|[Configuration の Controls の ExpressionBinding の PropertyName 要素 (書式)](./propertyname-element-for-expressionbinding-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コモンコントロールによって表示される値を持つ .NET プロパティを指定します。|
|[Configuration の Controls の ExpressionBinding の ScriptBlock 要素 (書式)](./scriptblock-element-for-expressionbinding-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 共通コントロールによって値が表示されるスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[構成のための CustomEntry コントロールの CustomItem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md)|カスタムコントロールビューとその表示方法によって表示されるデータを定義します。|

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[構成のための CustomEntry コントロールの CustomItem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
