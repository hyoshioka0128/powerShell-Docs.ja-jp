---
ms.date: 09/13/2016
ms.topic: reference
title: View の GroupBy 要素 (書式)
description: View の GroupBy 要素 (書式)
ms.openlocfilehash: d8ca93a3b2c1490928885579919c07f5eb274cd8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652097"
---
# <a name="groupby-element-for-view-format"></a>View の GroupBy 要素 (書式)

オブジェクトの新しいグループをどのように表示するかを定義します。 この要素は、テーブル、リスト、ワイド、またはカスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビューの要素 (形式) GroupBy 要素ビュー (形式)

## <a name="syntax"></a>構文

```xml
<GroupBy>
  <PropertyName>.NetTypeProperty</PropertyName>
  <ScriptBlock>ScriptToEvaluate</ScriptBlock>
  <Label>TextToDisplay</Label>
  <CustomControl>...</CustomControl>
  <CustomControlName>NameOfControl</CustomControlName>
</GroupBy>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomControl 要素 (書式)](./customcontrol-element-for-groupby-format.md)|省略可能な要素です。<br /><br /> 新しいグループを表示するカスタムコントロールを定義します。|
|[GroupBy の CustomControlName 要素 (書式)](./customcontrolname-element-for-groupby-format.md)|省略可能な要素です。<br /><br /> 新しいグループを表示するために使用するコントロールの名前を指定します。|
|[GroupBy の Label 要素 (書式)](./label-element-for-groupby-format.md)|省略可能な要素です。<br /><br /> 新しいグループが検出されたときに表示されるラベルを指定します。|
|[GroupBy の PropertyName 要素 (書式)](./propertyname-element-for-groupby-format.md)|省略可能な要素です。<br /><br /> .NET プロパティの値が変更されるたびに、によって新しいグループが開始されることを指定します。|
|[GroupBy の ScriptBlock 要素 (書式)](./scriptblock-element-for-groupby-format.md)|省略可能な要素です。<br /><br /> 値が変更されるたびに新しいグループを開始するスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上の .NET オブジェクトを表示するビューを定義します。|

## <a name="remarks"></a>解説

新しいオブジェクトのグループをどのように表示するかを定義するときには、新しいグループを開始するプロパティまたはスクリプトを指定する必要があります。ただし、両方を指定することはできません。

## <a name="see-also"></a>参照

[GroupBy の CustomControlName 要素 (書式)](./customcontrolname-element-for-groupby-format.md)

[GroupBy の Label 要素 (書式)](./label-element-for-groupby-format.md)

[GroupBy の PropertyName 要素 (書式)](./propertyname-element-for-groupby-format.md)

[GroupBy の ScriptBlock 要素 (書式)](./scriptblock-element-for-groupby-format.md)

[View 要素 (書式)](./view-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
