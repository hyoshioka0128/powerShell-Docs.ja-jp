---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)
description: View の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)
ms.openlocfilehash: 16b048e73195b3d6168724714ff223851dc1b20b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92664852"
---
# <a name="selectioncondition-element-for-entryselectedby-for-controls-for-view-format"></a>View の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)

コントロール定義を使用するために必要な条件を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールのコントロールに対して、ビューのコントロールの要素 (形式) コントロール要素 (format) コントロールの要素 (書式) を制御するためのコントロール要素 (書式) の CustomControl 要素 (形式) に対して、コントロールの CustomControl view (format) CustomEntry 要素を使用して、ビュー (format) のコントロールに対して、参照 (format) の項目を表示するためのコントロールの参照 (format) SelectionCondition 要素を表示するためのコントロールに対して、EntrySelectedBy を使用します。

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
|[View の Controls の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET プロパティを指定します。|
|[View の Controls の SelectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-selectioncondition-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーするスクリプトを指定します。|
|[View の Controls の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型のセットを指定します。|
|[View の Controls の SelectionCondition の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-controls-for-view-format.md)|省略可能な要素です。<br /><br /> 条件をトリガーする .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md)|このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|

## <a name="remarks"></a>解説

選択条件を定義する場合は、次の要件が適用されます。

- 選択条件には、少なくとも1つのプロパティ名またはスクリプトブロックを指定する必要がありますが、両方を指定することはできません。

- 選択条件では、任意の数の .NET 型または選択セットを指定できますが、両方を指定することはできません。

選択条件の使用方法の詳細については、「 [データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[View の Controls の SelectionCondition の PropertyName 要素 (書式)](./propertyname-element-for-selectioncondition-for-controls-for-view-format.md)

[View の Controls の SelectionCondition の ScriptBlock 要素 (書式)](./scriptblock-element-for-selectioncondition-for-controls-for-view-format.md)

[View の Controls の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-view-format.md)

[View の Controls の SelectionCondition の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-controls-for-view-format.md)

[View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
