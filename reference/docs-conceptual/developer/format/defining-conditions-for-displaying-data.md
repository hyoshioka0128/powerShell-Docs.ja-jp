---
ms.date: 09/13/2016
ms.topic: reference
title: データの表示条件を定義する
description: データの表示条件を定義する
ms.openlocfilehash: 9a8b7a618da8c64de978c13b527435a2d7793677
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660316"
---
# <a name="defining-conditions-for-displaying-data"></a>データの表示条件を定義する

ビューまたはコントロールによって表示されるデータを定義するときに、表示されるデータに必要な条件を指定できます。 条件は、特定のプロパティによって、またはスクリプトまたはプロパティの値がに評価されるときにトリガーされ `true` ます。 選択条件が満たされると、ビューまたはコントロールの定義が使用されます。

## <a name="specifying-a-selection-condition-for-a-definition"></a>定義の選択条件の指定

ビューまたはコントロールの定義を作成するときに、要素を使用して、定義を `EntrySelectedBy` 使用するオブジェクトまたは定義を使用するために必要な条件を指定します。 条件は、要素によって指定され `SelectionCondition` ます。

次の例では、テーブルビューの定義に対して選択条件が指定されています。 この例では、指定されたスクリプトがに評価されるときにのみ、定義が使用され `true` ます。

```xml
<TableRowEntry>
  <EntrySelectedBy>
    <SelectionCondition>
      <ScriptBlock>ScriptToEvaluate</ScriptBlock>
    </SelectionCondition>
  </EntrySelectedBy>
  <TableColumnItems>
  </TableColumnItems>
</TableRowEntry>

```

ビューまたはコントロールの定義に指定できる選択条件の数に制限はありません。 唯一の要件は次のとおりです。

- 選択条件では、条件をトリガーするプロパティ名またはスクリプトを1つ指定する必要がありますが、両方を指定することはできません。

- 選択条件では、任意の数の .NET 型または選択セットを指定できますが、両方を指定することはできません。

## <a name="specifying-a-selection-condition-for-an-item"></a>項目の選択条件の指定

項目定義に要素を含めることによって、リストビューまたはコントロールの項目をいつ使用するかを指定することもでき `ItemSelectionCondition` ます。 次の例では、リストビューの項目に対して選択条件が指定されています。 この例では、項目は、スクリプトがに評価されるときにのみ使用され `true` ます。

```xml
<ListItem>
  <ItemSelectionCondition>
    <ScriptBlock>ScriptToEvaluate</ScriptBlock>
  </ItemSelectionCondition>
</ListItem>

```

項目の選択条件は1つだけ指定できます。 条件では、条件をトリガーするプロパティ名またはスクリプトを1つ指定する必要がありますが、両方を指定することはできません。

## <a name="xml-elements"></a>XML 要素

 次の XML 要素を使用して、選択条件を作成します。

- 次の要素は、ビュー定義の選択条件を指定します。

  - [TableControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-tablecontrol-format.md)

  - [ListControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-listcontrol-format.md)

  - [WideControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-widecontrol-format.md)

  - [CustomControl の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-customcontrol-format.md)

- 次の要素は、共通およびビューのコントロール定義の選択条件を指定します。

  - [Configuration の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-controls-for-configuration-format.md)

  - [View の Controls の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-controls-for-view-format.md)

- 次の要素は、コレクションオブジェクトを展開するための選択条件を指定します。

  - [EnumerableExpansion の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-enumerableexpansion-format.md)

- 次の要素は、新しいデータグループを表示するための選択条件を指定します。

  - [GroupBy の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)

- 次の要素は、リストビューの項目選択条件を指定します。

  - [ListControl の ListItem の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md)

- 次の要素は、コントロールの項目選択条件を指定します。

  - [Configuration の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md)

  - [View の Controls の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md)

  - [CustomControl の ExpressionBinding の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-expressionbinding-for-customcontrol-format.md)

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
