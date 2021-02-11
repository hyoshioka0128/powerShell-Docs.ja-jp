---
ms.date: 09/13/2016
ms.topic: reference
title: ViewSelectedBy の SelectionSetName 要素 (書式)
description: ViewSelectedBy の SelectionSetName 要素 (書式)
ms.openlocfilehash: a1a1816761715a138bcb3c2bd4cb9dbbb2f9cb92
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92654900"
---
# <a name="selectionsetname-element-for-viewselectedby-format"></a>ViewSelectedBy の SelectionSetName 要素 (書式)

ビューに表示される一連の .NET オブジェクトを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) ViewSelectedBy Element (Format) SelectionSetName 要素 (形式)

## <a name="syntax"></a>構文

```xml
<SelectionSetName>Name of selection set<SelectionSetName>
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
|[ViewSelectedBy 要素 (書式)](./viewselectedby-element-format.md)|ビューに表示される .NET オブジェクトを定義します。|

## <a name="text-value"></a>テキスト値

選択セットの要素によって定義される選択セットの名前を指定し `Name` ます。

## <a name="remarks"></a>解説

継承によって関連付けられたオブジェクトのセットなど、1つの名前を使用して参照する関連オブジェクトのセットがある場合は、選択セットを使用できます。 選択セットの定義と参照の詳細については、「 [オブジェクトのセットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="example"></a>例

次の例では、リストビューの選択セットを指定する方法を示します。 テーブル、ワイド、およびカスタムビューでも同じスキーマが使用されます。

```xml
<View>
  <Name>Name of View</Name>
  <ViewSelectedBy>
    <SelectionSetName>NameofSelectionSet</SelectionSetName>>
  </ViewSelectedBy>
  <ListControl>...</ListControl>
</View>
```

## <a name="see-also"></a>参照

[選択セットを定義する](./defining-selection-sets.md)

[ViewSelectedBy 要素 (書式)](./viewselectedby-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
