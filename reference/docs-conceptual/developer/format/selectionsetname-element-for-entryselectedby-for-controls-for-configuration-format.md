---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)
description: Configuration の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: b775aa8a3184aa3ebcbda17a8e3191c69d67a700
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645730"
---
# <a name="selectionsetname-element-for-entryselectedby-for-controls-for-configuration-format"></a>Configuration の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)

コントロールのこの定義を使用する .NET 型のセットを指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの configuration (format) CustomControl 要素のコントロール要素を構成するためのコントロールの要素の構成 (形式) CustomControl の構成 (形式) の CustomEntries 要素の構成 (形式) CustoCustomControl 用の Mentry 要素は、構成 (format) のコントロールに対して EntrySelectedBy の Configuration (format) SelectionSetName 要素を構成するためのコントロール用の構成 (format) 要素を構成 (format) します。

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
|[Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-configuration-format.md)|このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

選択セットの名前を指定します。

## <a name="remarks"></a>解説

各コントロール定義には、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

選択セットは、通常、複数のビューで使用されるオブジェクトのグループを定義する場合に使用します。 選択セットの定義の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="see-also"></a>参照

[Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
