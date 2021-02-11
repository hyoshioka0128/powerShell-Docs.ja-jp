---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)
description: View の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: 3211b7cacd7e57770b48b03f4aade33da506f180
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92664731"
---
# <a name="selectionsetname-element-for-entryselectedby-for-controls-for-view-format"></a>View の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)

コントロールのこの定義を使用する .NET 型のセットを指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールのコントロールの要素 (書式) コントロールのコントロール要素 (format) コントロールの要素 (書式) コントロールの要素 (書式) をコントロールに対して、ビュー (Format) の CustomControl の CustomControl 要素 for view (format) CustomEntry 要素に対して、CustomEntry の EntrySelectedBy 要素を使用して、ビュー (Format) のコントロールに対して EntrySelectedBy の SelectionSetName 要素を表示 (format)

## <a name="syntax"></a>構文

```xml
<SelectionSetName>NameofSelectionSet</SelectionSetName>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `SelectionSetName` ます。

### <a name="attributes"></a>属性

なし

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md)|このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各コントロール定義には、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 選択セットの定義の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="see-also"></a>参照

[View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
