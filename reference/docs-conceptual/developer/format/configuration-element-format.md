---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration 要素 (書式)
description: Configuration 要素 (書式)
ms.openlocfilehash: 0524736d40dd7a7acb0b6fb61d1438b75672c240
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655689"
---
# <a name="configuration-element-format"></a>Configuration 要素 (書式)

書式設定ファイルのトップレベルの要素を表します。

Configuration 要素

## <a name="syntax"></a>構文

```xml
<Configuration>
  <DefaultSettings>...</DefaultSettings>
  <SelectionSets>...</SelectionSets>
  <Controls>...</Controls>
  <ViewDefinitions>...</ViewDefinitions>
</Configuration>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Configuration` ます。 この要素は、各書式設定ファイルのルート要素である必要があり、この要素には少なくとも1つの子要素が含まれている必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls 要素 (書式)](./controls-element-for-configuration-format.md)|省略可能な要素です。<br /><br /> 書式設定ファイルのすべてのビューで使用できる共通コントロールを定義します。|
|[DefaultSettings 要素 (書式)](./defaultsettings-element-format.md)|省略可能な要素です。<br /><br /> 書式設定ファイルのすべてのビューに適用される共通設定を定義します。|
|[SelectionSets 要素の形式](./selectionsets-element-format.md)|省略可能な要素です。<br /><br /> 書式設定ファイルのすべてのビューで使用できる .NET オブジェクトの共通セットを定義します。|
|[ViewDefinitions 要素 (書式)](./viewdefinitions-element-format.md)|省略可能な要素です。<br /><br /> オブジェクトを表示するために使用するビューを定義します。|

### <a name="parent-elements"></a>親要素

[なし] :

## <a name="remarks"></a>解説

書式設定ファイルは、オブジェクトの表示方法を定義します。 ほとんどの場合、このルート要素には、書式設定ファイルのテーブル、リスト、およびワイドビューを定義する [Viewdefinitions](./viewdefinitions-element-format.md) 要素が含まれています。 ビュー定義に加えて、書式設定ファイルでは、これらのビューで使用できる共通の選択セット、設定、およびコントロールを定義できます。

## <a name="see-also"></a>参照

[Configuration の Controls 要素 (書式)](./controls-element-for-configuration-format.md)

[DefaultSettings 要素 (書式)](./defaultsettings-element-format.md)

[SelectionSets 要素 (書式)](./selectionsets-element-format.md)

[ViewDefinitions 要素 (書式)](./viewdefinitions-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
