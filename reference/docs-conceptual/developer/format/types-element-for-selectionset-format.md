---
ms.date: 09/13/2016
ms.topic: reference
title: SelectionSet の Types 要素 (書式)
description: SelectionSet の Types 要素 (書式)
ms.openlocfilehash: ff3c24e7f52f862dc416b88d50983196ce907012
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645464"
---
# <a name="types-element-for-selectionset-format"></a>SelectionSet の Types 要素 (書式)

選択セット内の .NET オブジェクトを定義します。

Configuration 要素 (Format) SelectionSets 要素 (書式) Selectionsets Element (format) Types 要素 (書式)

## <a name="syntax"></a>構文

```xml
<Types>
  <TypeName>Nameof.NetType</TypeName>
</Types>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Types` ます。 少なくとも1つの子要素が必要ですが、追加できる子要素の数に上限はありません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[型の TypeName 要素 (Format)](./typename-element-for-types-format.md)|必須の要素です。<br /><br /> 選択セットに属する .NET オブジェクトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[SelectionSet 要素 (書式)](./selectionset-element-format.md)|セットの名前で参照できる .NET オブジェクトのセットを定義します。|

## <a name="remarks"></a>解説

この要素によって定義されるオブジェクトは、ビューで使用できる選択セット、ビューの定義 (ビューは複数の定義を持つことができます)、または選択条件を指定するときに使用できます。  選択セットの詳細については、「 [オブジェクトのセットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="example"></a>例

この例は、 `SelectionSet` 4 つの .net 型を定義する要素を示しています。

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

[オブジェクトのセットの定義](./defining-selection-sets.md)

[SelectionSet 要素 (書式)](./selectionset-element-format.md)

[型の TypeName 要素 (Format)](./typename-element-for-types-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
