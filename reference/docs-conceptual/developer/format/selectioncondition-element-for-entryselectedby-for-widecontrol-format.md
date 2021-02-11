---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の EntrySelectedBy の SelectionCondition 要素 (書式)
description: WideControl の EntrySelectedBy の SelectionCondition 要素 (書式)
ms.openlocfilehash: 8c51ca72edc6ad174dc289c61a8987e8f9495d19
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655119"
---
# <a name="selectioncondition-element-for-entryselectedby-for-widecontrol-format"></a>WideControl の EntrySelectedBy の SelectionCondition 要素 (書式)

この定義を使用するために必要な条件を定義します。 ワイドエントリの定義に指定できる選択条件の数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element (format) WideEntry (Format) の (Format) SelectionCondition 要素の EntrySelectedBy for WideEntry (Format)

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
|[WideEntry の EntrySelectedBy の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET プロパティを指定します。|
|[WideEntry (Format) の EntrySelectedBy の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーするスクリプトブロックを指定します。|
|[WideEntry の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型のセットを指定します。|
|[WideEntry (Format) の EntrySelectedBy の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-entryselectedby-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-wideentry-format.md)|このワイドエントリを使用する .NET 型、またはこのエントリが使用されるために必要な条件を定義します。|

## <a name="remarks"></a>解説

各ワイドエントリには、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択条件を定義する場合は、次の要件が適用されます。

- 選択条件には、少なくとも1つのプロパティ名またはスクリプトブロックを指定する必要がありますが、両方を指定することはできません。

- 選択条件では、任意の数の .NET 型または選択セットを指定できますが、両方を指定することはできません。

選択条件の使用方法の詳細については、「 [ビューエントリまたは項目が使用される場合の条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

ワイドビューのその他のコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="see-also"></a>参照

[ワイド ビューを作成する](./creating-a-wide-view.md)

[データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)

[WideEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-wideentry-format.md)

[WideEntry の EntrySelectedBy の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md)

[WideEntry (Format) の EntrySelectedBy の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-widecontrol-format.md)

[WideEntry の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md)

[WideEntry (Format) の EntrySelectedBy の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-entryselectedby-for-widecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
