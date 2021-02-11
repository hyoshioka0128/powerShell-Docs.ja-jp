---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の EntrySelectedBy の SelectionSetName 要素 (書式)
description: View の CustomControl の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: a158c5476fb3a168a146ce67565c0ed6f7859519
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651924"
---
# <a name="selectionsetname-element-for-entryselectedby-for-customcontrol-for-view-format"></a>View の CustomControl の EntrySelectedBy の SelectionSetName 要素 (書式)

リストエントリの一連の .NET オブジェクトを指定します。 エントリに指定できる選択セットの数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) ビュー要素 (Format) CustomControl 要素 (形式) の CustomEntries for CustomControl for view (format) SelectionSetName Element for customentry for の EntrySelectedBy 要素を使用して、customentry 用の entryselectedby の要素を指定します。

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
|[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)|このカスタムエントリを使用する .NET 型、またはこのエントリが使用されるために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各カスタムコントロールエントリには、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 たとえば、同じオブジェクトのセットに対してテーブルビューとリストビューを作成することができます。 選択セットの定義の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

カスタムコントロールビューのコンポーネントの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

## <a name="see-also"></a>参照

[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)

[カスタムコントロールビュー](./creating-custom-controls.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
