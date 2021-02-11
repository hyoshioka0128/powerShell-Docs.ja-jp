---
ms.date: 09/13/2016
ms.topic: reference
title: EnumerableExpansion の EntrySelectedBy の SelectionCondition 要素 (書式)
description: EnumerableExpansion の EntrySelectedBy の SelectionCondition 要素 (書式)
ms.openlocfilehash: 3ecf7fde99b9ee66a153eea416792f351ff62af3
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647915"
---
# <a name="selectioncondition-element-for-entryselectedby-for-enumerableexpansion-format"></a>EnumerableExpansion の EntrySelectedBy の SelectionCondition 要素 (書式)

この定義のコレクションオブジェクトを展開するために必要な条件を定義します。

Configuration 要素 (Format) DefaultSettings Element (format) 列挙 able膨張要素 (format) 列挙 able膨張拡張要素 (format) EntrySelectedBy (format) の Entryselectedby の SelectionCondition 要素を列挙します。

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

次のセクションでは、要素の属性、子要素、および親要素について説明し `SelectionCondition` ます。 1つまたは複数の要素を指定する必要があり `PropertyName` `ScriptBlock` ます。 `SelectionSetName`要素と `TypeName` 要素は省略可能です。 いずれかの要素を指定できます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[EnumerableExpansion の EntrySelectedBy の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET プロパティを指定します。|
|[EnumerableExpansion の EntrySelectedBy の SelectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーするスクリプトを指定します。|
|[EnumerableExpansion の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型のセットを指定します。|
|[EnumerableExpansion の EntrySelectedBy の SelectionCondition の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[EnumerableExpansion の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-enumerableexpansion-format.md)|この定義によって展開される .NET コレクションオブジェクトを定義します。|

## <a name="remarks"></a>解説

各定義には、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択条件を定義する場合は、次の要件が適用されます。

- 選択条件には、少なくとも1つのプロパティ名またはスクリプトブロックを指定する必要がありますが、両方を指定することはできません。

- 選択条件では、任意の数の .NET 型または選択セットを指定できますが、両方を指定することはできません。

選択条件の使用方法の詳細については、「 [データの Diplaying の条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

ワイドビューのその他のコンポーネントの詳細については、「 [Wide ビュー](./creating-a-wide-view.md)」を参照してください。

## <a name="see-also"></a>参照

[データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
