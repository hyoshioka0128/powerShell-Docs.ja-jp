---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の EntrySelectedBy の SelectionSetName 要素 (書式)
description: WideControl の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: bf6a44bb733421f36a9140d0ec10e61aedfbda6a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651698"
---
# <a name="selectionsetname-element-for-entryselectedby-for-widecontrol-format"></a>WideControl の EntrySelectedBy の SelectionSetName 要素 (書式)

定義の .NET オブジェクトのセットを指定します。 これらのオブジェクトのいずれかが表示されるたびに、定義が使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element (format) WideEntry (Format) SelectionSetName Element for WideEntry (format) の EntrySelectedBy 要素

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
|[WideEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-wideentry-format.md)|このワイドエントリを使用する .NET 型、またはこのエントリが使用されるために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各定義には、1つの型名、選択セット、または選択条件を指定する必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 たとえば、同じオブジェクトのセットに対してテーブルビューとリストビューを作成することができます。 選択セットの定義の詳細については、「 [ビューのオブジェクトセットの定義](./defining-selection-sets.md)」を参照してください。

ワイドビューのその他のコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="see-also"></a>参照

[ワイド ビューを作成する](./creating-a-wide-view.md)

[選択セットを定義する](./defining-selection-sets.md)

[WideEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-wideentry-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
