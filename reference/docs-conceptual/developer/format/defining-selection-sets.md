---
ms.date: 09/13/2016
ms.topic: reference
title: 選択セットを定義する
description: 選択セットを定義する
ms.openlocfilehash: d709a368a45623d56fdbf4e98a11a5e5f8a193fa
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648260"
---
# <a name="defining-selection-sets"></a>選択セットを定義する

複数のビューおよびコントロールを作成する場合は、選択セットと呼ばれるオブジェクトのセットを定義できます。 選択セットを使用すると、各ビューまたはコントロールに対して個別に定義しなくても、オブジェクトを一度定義することができます。 通常、選択セットは、関連する一連の .NET オブジェクトがある場合に使用されます。 たとえば、 `FileSystem` 書式設定ファイル (FileSystem.format.ps1xml) では、複数のビューで使用するファイルシステムの種類の選択セットが定義されています。

## <a name="where-selection-sets-are-defined-and-referenced"></a>選択セットが定義され参照されている場所

選択セットは、書式設定ファイルで定義されているすべてのビューとコントロールで使用できる共通データの一部として定義します。 次の例では、3つの選択セットを定義する方法を示します。

```xml
<Configuration>
  <SelectionSets>
    <SelectionSet>...</SelectionSet>
    <SelectionSet>...</SelectionSet>
    <SelectionSet>...</SelectionSet>
  </SelectionSets>
</Configuration>
```

選択セットは、次の方法で参照できます。

- 各ビューには `ViewSelectedBy` 、ビューを使用して表示されるオブジェクトを定義する要素があります。 `ViewSelectedBy`要素には、 `SelectionSetName` ビューのすべての定義で使用される選択セットを指定する子要素があります。 ビューから参照できる選択セットの数に制限はありません。

- ビューまたはコントロールの各定義では、 `EntrySelectedBy` 要素によって、その定義を使用して表示されるオブジェクトが定義されます。 通常、ビューまたはコントロールには定義が1つしかないため、オブジェクトは要素によって定義され `ViewSelectedBy` ます。 `EntrySelectedBy`定義の要素には、 `SelectionSetName` 選択セットを指定する子要素があります。 定義の選択セットを指定する場合、要素の他の子要素を指定することはできません `EntrySelectedBy` 。

- ビューまたはコントロールの各定義では、要素を使用して、 `SelectionCondition` 定義を使用するときの条件を指定できます。 `SelectionCondition`要素には、 `SelectionSetName` 条件をトリガーする選択セットを指定する子要素があります。 この条件は、選択セットで定義されたオブジェクトのいずれかが表示されたときにトリガーされます。 これらの条件を設定する方法の詳細については、「 [データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="selection-set-example"></a>選択セットの例

次の例は、Windows PowerShell によって提供される書式設定ファイルから直接取得される選択セットを示して `FileSystem` います。 その他の Windows PowerShell フォーマットファイルの詳細については、「 [Windows powershell の書式設定ファイル](./powershell-formatting-files.md)」を参照してください。

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

前の選択セットは、 `ViewSelectedBy` テーブルビューの要素で参照されます。

```xml
<ViewDefinitions>
  <View>
    <Name>Files</Name>
    <ViewSelectedBy>
      <SelectionSetName>FileSystemTypes</SelectionSetName>
    </ViewSelectedBy>
    <TableControl>...</TableControl>
  </View>
</ViewDefinitions>

```

## <a name="xml-elements"></a>XML 要素

 定義できる選択セットの数に制限はありません。 次の XML 要素を使用して、選択セットを作成します。

- [Selectionsets](./selectionsets-element-format.md)要素は、書式設定ファイルのビューおよびコントロールによって参照される .net オブジェクトのセットを定義します。

- [Selectionset](./selectionset-element-format.md)要素は、.net オブジェクトの1つのセットを定義します。

- [Name](./name-element-for-selectionset-format.md)要素は、選択セットを参照するために使用される名前を指定します。

- [Types](./types-element-for-selectionset-format.md)要素は、選択セットのオブジェクトの .net 型を指定します。 (書式設定ファイル内では、オブジェクトは .NET 型によって指定されます)。

 次の XML 要素を使用して、選択セットを指定します。

- 次の要素は、ビューのすべての定義で使用する選択セットを指定します。

  - [ViewSelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-viewselectedby-format.md)

  - [GroupBy の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-groupby-format.md)

- 次の要素は、1つのビュー定義で使用される選択セットを指定します。

  - [ListControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md)

  - [TableControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-tablecontrol-format.md)

  - [WideControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-widecontrol-format.md)

  - [View の CustomControl の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-customcontrol-for-view-format.md)

- 次の要素は、コモンコントロール定義と view コントロール定義で使用される選択セットを指定します。

  - [View の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-controls-for-view-format.md)

  - [Configuration の Controls の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-controls-for-configuration-format.md)

- 次の要素では、拡張するオブジェクトを定義するときに使用する選択セットを指定します。

  - [EnumerableExpansion の EntrySelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-entryselectedby-for-enumerableexpansion-format.md)

- 次の要素は、選択条件で使用される選択セットを指定します。

  - [Configuration の Controls の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md)

  - [View の Controls の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-controls-for-view-format.md)

  - [View の CustomControl の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-customcontrol-for-view-format.md)

  - [EnumerableExpansion の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md)

  - [ListEntry の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-listentry-format.md)

  - [TableControl の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-tablecontrol-format.md)

  - [WideEntry の EntrySelectedBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md)

  - [GroupBy の SelectionCondition の SelectionSetName 要素 (書式)](./selectionsetname-element-for-selectioncondition-for-groupby-format.md)

## <a name="see-also"></a>参照

[SelectionSets](./selectionsets-element-format.md)

[SelectionSet](./selectionset-element-format.md)

[名前](./name-element-for-selectionset-format.md)

[型](./types-element-for-selectionset-format.md)

[PowerShell 書式設定ファイル](./powershell-formatting-files.md)

[データを表示するときの条件の定義](./defining-conditions-for-displaying-data.md)

[PowerShell の形式と種類のファイルの作成](./writing-a-powershell-formatting-file.md)
