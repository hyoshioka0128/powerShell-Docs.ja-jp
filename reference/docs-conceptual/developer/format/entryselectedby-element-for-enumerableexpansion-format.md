---
ms.date: 09/13/2016
ms.topic: reference
title: EnumerableExpansion の EntrySelectedBy 要素 (書式)
description: EnumerableExpansion の EntrySelectedBy 要素 (書式)
ms.openlocfilehash: 8b2fff2d0b14d0622d0be2c5af3a95194c733a73
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652322"
---
# <a name="entryselectedby-element-for-enumerableexpansion-format"></a>EnumerableExpansion の EntrySelectedBy 要素 (書式)

この定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。

Configuration 要素 (Format) DefaultSettings 要素 (format) の列挙 Able膨張要素 (format) 列挙 Able膨張の要素 (形式) には、列挙 Able拡張 (Format) の要素を指定します。

## <a name="syntax"></a>構文

```xml
<EntrySelectedBy>
  <TypeName>Nameof.NetType</TypeName>
  <SelectionSetName>NameofSelectionSet</SelectionSetName>
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
|[EnumerableExpansion の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> この定義のコレクションオブジェクトを展開するために必要な条件を定義します。|
|[EnumerableExpansion の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> コレクションオブジェクトの展開方法のこの定義を使用する一連の .NET 型を指定します。|
|[EnumerableExpansion の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-enumerableexpansion-format.md)|省略可能な要素です。<br /><br /> コレクションオブジェクトの展開方法のこの定義を使用する .NET 型を指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[EnumerableExpansion 要素 (書式)](./enumerableexpansion-element-format.md)|特定の .NET コレクションオブジェクトをビューに表示するときに展開する方法を定義します。|

## <a name="remarks"></a>解説

定義エントリには、少なくとも1つの種類、選択セット、または選択条件を指定する必要があります。 使用できる子要素の数に上限はありません。

選択条件は、オブジェクトが特定のプロパティを持っている場合や、特定のプロパティ値またはスクリプトがに評価される場合など、使用する定義のために存在する必要がある条件を定義するために使用され `true` ます。 選択条件の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[データの表示条件を定義する](./defining-conditions-for-displaying-data.md)

[EnumerableExpansion 要素 (書式)](./enumerableexpansion-element-format.md)

[EnumerableExpansion の EntrySelectedBy の SelectionCondition 要素 (書式)](./selectioncondition-element-for-entryselectedby-for-enumerableexpansion-format.md)

[EnumerableExpansion の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-enumerableexpansion-format.md)

[EnumerableExpansion の EntrySelectedBy の TypeName 要素 (書式)](./typename-element-for-entryselectedby-for-enumerableexpansion-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
