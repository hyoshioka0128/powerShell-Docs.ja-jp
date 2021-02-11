---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の EntrySelectedBy の SelectionSetName 要素 (書式)
description: GroupBy の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: 7ebe5d884061243c8b4af196788187d84c15a92e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651846"
---
# <a name="selectionsetname-element-for-entryselectedby-for-groupby-format"></a>GroupBy の EntrySelectedBy の SelectionSetName 要素 (書式)

リストエントリの一連の .NET オブジェクトを指定します。 エントリに指定できる選択セットの数に制限はありません。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素を使用して、groupby (形式) の CustomControl 要素に対して groupby (format) CustomEntries 要素の CustomControl の CustomControl for groupby (形式) の CustomEntry 要素を使用して、groupby (形式) の EntrySelectedBy の SelectionSetName 要素。

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
|[GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-groupby-format.md)|このカスタムエントリを使用する .NET 型、またはこのエントリが使用されるために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各カスタムコントロール定義には、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 たとえば、同じオブジェクトのセットに対してテーブルビューとリストビューを作成することができます。 選択セットの定義の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

カスタムコントロールビューのコンポーネントの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

## <a name="see-also"></a>参照

[GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-groupby-format.md)

[カスタム コントロールを作成する](./creating-custom-controls.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
