---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の EntrySelectedBy の TypeName 要素 (書式)
description: ListControl の EntrySelectedBy の TypeName 要素 (書式)
ms.openlocfilehash: 6fc5a2385fde3140abbc984e3da6a4dda483b2a8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645666"
---
# <a name="typename-element-for-entryselectedby-for-listcontrol-format"></a>ListControl の EntrySelectedBy の TypeName 要素 (書式)

リストビューのこのエントリを使用する .NET 型を指定します。 リストエントリに指定できる型の数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (format) ListEntries Element (format) ListEntry Element (format) ListEntry (Format) の (format) TypeName 要素の EntrySelectedBy for ListControl (Format)

## <a name="syntax"></a>構文

```xml
<TypeName>Nameof.NetType</TypeName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TypeName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ListEntry の EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-listentry-for-listcontrol-format.md)|このリストエントリを使用する .NET 型、またはこのエントリが使用されるために存在する必要がある条件を定義します。|

## <a name="text-value"></a>テキスト値

.NET 型の完全修飾名を指定します (例 `System.IO.DirectoryInfo` :)。

## <a name="remarks"></a>解説

各リストエントリには、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

リストビューでのこの要素の使用方法の詳細については、「 [リストビュー](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

次の例では、リストビューのエントリの選択セットを指定する方法を示します。

```xml
<ListEntry>
  <EntrySelectedBy>
    <TypeName>Nameof.NetType</TypeName>
  </EntrySelectedBy>
  <ListItems>...</ListItems>
</ListEntry>
```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[ListEntry の EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-listentry-for-listcontrol-format.md)

[ListEntry (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
