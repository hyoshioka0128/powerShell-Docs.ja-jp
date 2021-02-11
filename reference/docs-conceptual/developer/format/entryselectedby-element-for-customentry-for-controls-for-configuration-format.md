---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)
description: Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: fadcdb69ac71269ba2f2f80baf139bb363d4acba
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666673"
---
# <a name="entryselectedby-element-for-customentry-for-controls-for-configuration-format"></a>Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)

コモンコントロールの定義、またはこのコントロールを使用するために存在する必要がある条件を使用する .NET 型を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの構成 (format) コントロールの要素の構成 (形式) の CustomControl 要素の構成のためのコントロール要素の構成 (形式) の CustomEntries 要素を構成するための CustomControl for configuration (形式) の CustomEntries 要素の構成用コントロール (書式) のための構成 (形式) のコントロールの設定 (形式)

## <a name="syntax"></a>構文

```xml
<EntrySelectedBy>
  <TypeName>Nameof.NetType</TypeName>
  <SelectionSetName>SelectionSet</SelectionSetName>
  <SelectionCondition>...</SelectionCondition>
</EntrySelectedBy>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `EntrySelectedBy` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 共通のコントロール定義を使用するために必要な条件を定義します。|
|[Configuration の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コモンコントロールのこの定義を使用する一連の .NET 型を指定します。|
|[Configuration の Controls の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コモンコントロールのこの定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-controls-for-configuration-format.md)|コモンコントロールの定義を提供します。|

## <a name="remarks"></a>解説

少なくとも、各定義には、少なくとも1つの .NET 型、選択セット、または選択条件が指定されている必要があります。 指定できる型、選択セット、または選択条件の数に上限はありません。

## <a name="see-also"></a>参照

[Configuration の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-controls-for-configuration-format.md)

[Configuration の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomControl の CustomEntry 要素 (書式)](./customentry-element-for-customcontrol-for-controls-for-configuration-format.md)

[Configuration の Controls の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
