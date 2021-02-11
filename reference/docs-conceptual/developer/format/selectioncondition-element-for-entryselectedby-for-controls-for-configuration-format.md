---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)
description: Configuration の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)
ms.openlocfilehash: ce00cdf868a3075875043aeb59f2cb8a17d049a9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645782"
---
# <a name="selectioncondition-element-for-entryselectedby-for-controls-for-configuration-format"></a>Configuration の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)

共通のコントロール定義を使用するために必要な条件を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの configuration (format) コントロールの要素の構成 (format) CustomControl 要素の構成のためのコントロールの CustomControl の構成 (形式) の CustomEntries 要素の構成 (書式設定用コントロール) CustomControl のためのコントロールのためのコントロールのための構成 (形式) の構成 (形式) のために、customentry for Configuration (形式) の SelectionCondition 要素を指定します。

## <a name="syntax"></a>構文

```xml
<SelectionCondition>
  <TypeName>Nameof.NetType</TypeName>
  <SelectionSetName>NameofSelectionSet</SelectionSetName>
  <PropertyName>.NetTypeProperty</PropertyName>
  <ScriptBlock>ScriptToEvaluate</ScriptBlock>
</SelectionCondition>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `SelectionCondition` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET プロパティを指定します。|
|[Configuration の Controls の SelectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-selectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーするスクリプトを指定します。|
|[Configuration の Controls の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型のセットを指定します。|
|[Configuration の Controls の SelectionCondition の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-configuration-format.md)|コモンコントロール定義のこのエントリを使用する .NET 型を定義します。|

## <a name="remarks"></a>解説

選択条件を定義するときは、次のガイドラインに従う必要があります。

- 選択条件には、少なくとも1つのプロパティ名またはスクリプトブロックを指定する必要がありますが、両方を指定することはできません。

- 選択条件では、任意の数の .NET 型または選択セットを指定できますが、両方を指定することはできません。

選択条件を使用する方法の詳細については、「 [データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[Configuration の Controls の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の SelectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-selectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の SelectionCondition の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-controls-for-configuration-format.md)

[Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-configuration-format.md)

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
