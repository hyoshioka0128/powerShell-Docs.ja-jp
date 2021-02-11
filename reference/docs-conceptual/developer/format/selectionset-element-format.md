---
ms.date: 09/13/2016
ms.topic: reference
title: SelectionSet 要素 (書式)
description: SelectionSet 要素 (書式)
ms.openlocfilehash: 944aa83569ad8ca789746a71f60e5da5c19fbf01
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647877"
---
# <a name="selectionset-element-format"></a>SelectionSet 要素 (書式)

セットの名前で参照できる .NET オブジェクトのセットを定義します。

Configuration 要素 (Format) SelectionSets 要素 (書式) Selectionsets 要素 (形式)

## <a name="syntax"></a>構文

```xml
<SelectionSet>
  <Name>SelectionSetName</Name>
  <Types>...</Types>
</SelectionSet>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `SelectionSet` ます。 各選択セットには名前が必要であり、セットの .NET オブジェクトを指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[SelectionSet の Name 要素 (書式)](./name-element-for-selectionset-format.md)|必須の要素です。<br /><br /> 選択セットを参照するために使用する名前を指定します。|
|[Types 要素 (Format)](./types-element-for-selectionset-format.md)|必須の要素です。<br /><br /> 選択セット内の .NET オブジェクトを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[SelectionSets 要素の形式](./selectionsets-element-format.md)|書式設定ファイルのすべてのビューで使用できる .NET オブジェクトの共通セットを定義します。|

## <a name="remarks"></a>解説

継承によって関連付けられたオブジェクトのセットなど、1つの名前を使用して参照する関連オブジェクトのセットがある場合は、選択セットを使用できます。 ビューを定義するときに、各ビュー内のすべてのオブジェクトを一覧表示するのではなく、選択したセットの名前を使用してオブジェクトのセットを指定できます。

共通選択セットは、書式設定ファイルのビューまたはビューの定義を定義するときに、名前によって指定されます。 このような場合、 `SelectionSetName` 要素と要素の子要素は、 `ViewSelectedBy` `EntrySelectedBy` 使用するセットを指定します。 選択セットの詳細については、「 [オブジェクトのセットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `SelectionSet` 4 つの .net 型を定義する要素を示しています。

```xml
<SelectionSets>
  <SelectionSet>
    <Name>FileSystemTypes</Name>
    <Types>
     <TypeName>System.IO.DirectoryInfo</TypeName>
     <TypeName>System.IO.FileInfo</TypeName>
     <TypeName>Deserialized.System.IO.DirectoryInfo</TypeName>
     <TypeName>Deserialized.System.IO.FileInfo</TypeName>
    </Types>
  </SelectionSet>
</SelectionSets>
```

## <a name="see-also"></a>参照

[選択セットを定義する](./defining-selection-sets.md)

[SelectionSet の Name 要素 (Format)](./name-element-for-selectionset-format.md)

[SelectionSets 要素 (書式)](./selectionsets-element-format.md)

[Types 要素 (Format)](./types-element-for-selectionset-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
