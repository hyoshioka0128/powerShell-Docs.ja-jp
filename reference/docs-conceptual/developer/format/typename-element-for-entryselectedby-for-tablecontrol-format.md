---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl の EntrySelectedBy の TypeName 要素 (書式)
description: TableControl の EntrySelectedBy の TypeName 要素 (書式)
ms.openlocfilehash: 5a9f5cda1810d461d19ffb48a1cfa2d41f87ca96
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651429"
---
# <a name="typename-element-for-entryselectedby-for-tablecontrol-format"></a>TableControl の EntrySelectedBy の TypeName 要素 (書式)

テーブルビューのこのエントリを使用する .NET 型を指定します。 テーブルエントリに指定できる型の数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) TableControl Element (format) TableRowEntries Element (format) TableRowEntry Element (format) TableRowEntry の EntrySelectedBy Element (format) TypeName 要素 (format)

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
|[EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md)|このエントリを使用する .NET 型、またはこのエントリが使用されるために存在する必要がある条件を定義します。|

## <a name="text-value"></a>テキスト値

.NET 型の名前を指定します。

## <a name="remarks"></a>解説

各リストエントリには、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
