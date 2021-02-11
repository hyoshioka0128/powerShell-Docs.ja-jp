---
ms.date: 09/13/2016
ms.topic: reference
title: EnumerableExpansion の EntrySelectedBy の SelectionSetName 要素 (書式)
description: EnumerableExpansion の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: 074f810f5a3cbad61ee8be69408967758f0591df
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651875"
---
# <a name="selectionsetname-element-for-entryselectedby-for-enumerableexpansion-format"></a>EnumerableExpansion の EntrySelectedBy の SelectionSetName 要素 (書式)

この定義によって展開される .NET 型のセットを指定します。

Configuration 要素 (Format) DefaultSettings 要素 (format) enumerableexpansions 要素 (format) 列挙 able膨張拡張要素 (format) の entryselectedby (format) SelectionSetName 要素 (format Able膨張の場合は Format) のエントリを列挙します。

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
|[EnumerableExpansion の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-enumerableexpansion-format.md)|この定義によって展開される .NET コレクションオブジェクトを定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各定義には、1つ以上の型名、選択セット、または選択条件を指定する必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 たとえば、同じオブジェクトのセットに対してテーブルビューとリストビューを作成することができます。 選択セットの定義の詳細については、「 [ビューのオブジェクトセットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="see-also"></a>参照

[選択セットを定義する](./defining-selection-sets.md)

[EnumerableExpansion の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-enumerableexpansion-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
