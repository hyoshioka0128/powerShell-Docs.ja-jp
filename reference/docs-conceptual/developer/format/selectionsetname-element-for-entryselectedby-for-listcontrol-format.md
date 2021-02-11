---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の EntrySelectedBy の SelectionSetName 要素 (書式)
description: ListControl の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: 413a77f7ba06fe952e574061e58d0b5d80c5b3c4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651830"
---
# <a name="selectionsetname-element-for-entryselectedby-for-listcontrol-format"></a>ListControl の EntrySelectedBy の SelectionSetName 要素 (書式)

リストエントリの一連の .NET オブジェクトを指定します。 エントリに指定できる選択セットの数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (format) ListEntries Element (format) ListEntry Element (format) SelectionSetName Element for ListEntry (format) Element for ListEntry (Format) の EntrySelectedBy

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
|[ListEntry の EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-listentry-for-listcontrol-format.md)|このリストエントリを使用する .NET 型、またはこのエントリが使用されるために存在する必要がある条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各リストエントリには、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 たとえば、同じオブジェクトのセットに対してテーブルビューとリストビューを作成することができます。 選択セットの定義の詳細については、「 [ビューのオブジェクトセットの定義](./defining-selection-sets.md)」を参照してください。

リストビューのコンポーネントの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

次の例では、リストビューのエントリの選択セットを指定する方法を示します。

```xml
<ListEntry>
  <EntrySelectedBy>
    <SelectionSetName>NameofSelectionSet</SelectionSetName>
  </EntrySelectedBy>
  <ListItems>...</ListItems>
</ListEntry>
```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[ListEntry の EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-listentry-for-listcontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
