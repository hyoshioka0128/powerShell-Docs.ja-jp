---
ms.date: 09/13/2016
ms.topic: reference
title: SelectionSet の Name 要素 (書式)
description: SelectionSet の Name 要素 (書式)
ms.openlocfilehash: 98c13be6ea352055231fbdb3a60f0eb508f661b8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666462"
---
# <a name="name-element-for-selectionset-format"></a>SelectionSet の Name 要素 (書式)

選択セットを参照するために使用する名前を指定します。

Configuration 要素 (Format) SelectionSets 要素 (形式) Selectionsets 要素 (書式) Selectionsets (形式) の名前要素

## <a name="syntax"></a>構文

```xml
<Name>Name of selection set</Name>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Name` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[SelectionSet 要素 (書式)](./selectionset-element-format.md)|セットの名前で参照できる .NET オブジェクトの1つのセットを定義します。|

## <a name="text-value"></a>テキスト値

選択セットを参照する名前を指定します。 使用できる文字に関する制限はありません。

## <a name="remarks"></a>解説

ここで指定された名前は、要素で使用され `SelectionSetName` ます。 ビューで使用できる選択セット (ビューには複数の定義を含めることができます)、または選択条件を指定する場合に使用できます。 選択セットの詳細については、「 [オブジェクトのセットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="example"></a>例

この例は、 `SelectionSet` 4 つの .net 型を定義する要素を示しています。 選択セットの名前は "FileSystemTypes" です。

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

[SelectionSet 要素 (書式)](./selectionset-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
