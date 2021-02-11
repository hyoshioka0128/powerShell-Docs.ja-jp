---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の SelectionCondition の SelectionSetName 要素 (書式)
description: GroupBy の SelectionCondition の SelectionSetName 要素 (書式)
ms.openlocfilehash: a4f28c1caba3790718b99f63659cb0cbed8def16
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92654990"
---
# <a name="selectionsetname-element-for-selectioncondition-for-groupby-format"></a>GroupBy の SelectionCondition の SelectionSetName 要素 (書式)

条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、このコントロールを使用してオブジェクトが表示されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) GroupBy 要素の groupby (format) CustomControl 要素を groupby (format) CustomEntry 要素の CustomControl の CustomEntries 要素 CustomControl for groupby (Format) の CustomEntry for groupby (format) Selectionselectedby 要素の EntrySelectedBy を使用して groupby (format) SelectionSetName 要素を指定して、GroupBy (形式) の SelectionCondition 要素

## <a name="syntax"></a>構文

```xml
<SelectionSetName>NameofSelectionSet</SelectionSetName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `SelectionSetName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)|コントロール定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

選択セットは、書式設定ファイルによって定義される任意のビューで使用できる .NET オブジェクトの一般的なグループです。 選択セットの作成と参照の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

この要素が指定されている場合、 [TypeName](./typename-element-for-selectioncondition-for-groupby-format.md) 要素を指定することはできません。 選択条件の定義の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[GroupBy の SelectionCondition の TypeName 要素 (書式)](./typename-element-for-selectioncondition-for-groupby-format.md)

[GroupBy の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)

[データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)

[選択セットを定義する](./defining-selection-sets.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
