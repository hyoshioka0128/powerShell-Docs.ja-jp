---
ms.date: 09/13/2016
ms.topic: reference
title: テーブル ビューを作成する
description: テーブル ビューを作成する
ms.openlocfilehash: 035d42f7968a9e8babec692a7a5873e24b36cd97
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660289"
---
# <a name="creating-a-table-view"></a>テーブル ビューを作成する

テーブルビューでは、1つまたは複数の列にデータが表示されます。 テーブルの各行は .NET オブジェクトを表し、テーブルの各列はオブジェクトのプロパティまたはスクリプト値を表します。 オブジェクトのすべてのプロパティ、またはオブジェクトのプロパティのサブセットを表示するテーブルビューを定義できます。

## <a name="a-table-view-display"></a>テーブルビューの表示

次の例で[は、Servicecontroller コマンドレット](/powershell/module/microsoft.powershell.management/get-service)によって返される[](/dotnet/api/System.ServiceProcess.ServiceController)オブジェクトが Windows PowerShell によってどのように表示されるかを示します。 このオブジェクトの場合、Windows PowerShell では、プロパティ、プロパティ `Status` (このプロパティ `Name` はプロパティのエイリアスプロパティ)、およびプロパティを表示するテーブルビューが定義されてい `ServiceName` `DisplayName` ます。 テーブルの各行は、コマンドレットによって返されるオブジェクトを表します。

```output
Status   Name               DisplayName
------   ----               -----------
Stopped  AJRouter           AllJoyn Router Service
Stopped  ALG                Application Layer Gateway Service
Stopped  AppIDSvc           Application Identity
Running  Appinfo            Application Information
```

## <a name="defining-the-table-view"></a>テーブルビューの定義

次の XML は、Servicecontroller を表示するためのテーブルビュースキーマを示しています。 [Displayproperty = Fullname](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクト。 テーブルビューに表示する各プロパティを指定する必要があります。

```xml
<View>
  <Name>service</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <TableControl>
    <TableHeaders>
      <TableColumnHeader>
        <Width>8</Width>
      </TableColumnHeader>
      <TableColumnHeader>
        <Width>18</Width>
      </TableColumnHeader>
      <TableColumnHeader>
        <Width>38</Width>
      </TableColumnHeader>
    </TableHeaders>
    <TableRowEntries>
      <TableRowEntry>
        <TableColumnItems>
          <TableColumnItem>
           <PropertyName>Status</PropertyName>
          </TableColumnItem>
          <TableColumnItem>
            <PropertyName>Name</PropertyName>
          </TableColumnItem>
          <TableColumnItem>
            <PropertyName>DisplayName</PropertyName>
          </TableColumnItem>
        </TableColumnItems>
      </TableRowEntry>
    </TableRowEntries>
  </TableControl>
</View>
```

リストビューを定義するには、次の XML 要素を使用します。

- [View](./view-element-format.md)要素は、テーブルビューの親要素です。 (これは、リスト、ワイド、およびカスタムコントロールビューの親要素と同じです)。

- [Name](./name-element-for-view-format.md)要素は、ビューの名前を指定します。 この要素は、すべてのビューに必要です。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、ビューを使用するオブジェクトを定義します。 この要素は必須です。

- [GroupBy](./groupby-element-for-view-format.md)要素 (この例では示されていません) は、オブジェクトの新しいグループが表示されるタイミングを定義します。 特定のプロパティまたはスクリプトの値が変更されるたびに、新しいグループが開始されます。 この要素は省略可能です。

- [Controls](./controls-element-for-view-format.md)要素 (この例では示されていません) は、テーブルビューで定義されるカスタムコントロールを定義します。 コントロールを使用すると、データの表示方法をさらに指定することができます。 この要素は省略可能です。 ビューでは、独自のカスタムコントロールを定義することも、書式設定ファイルの任意のビューで使用できるコモンコントロールを使用することもできます。 カスタムコントロールの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

- [Hidetableheaders](./hidetableheaders-element-format.md)要素 (この例では示されていません) は、テーブルの先頭にラベルが表示されないように指定します。 この要素は省略可能です。

- テーブルのヘッダーと行の情報を定義する [Tablecontrol](./tablecontrol-element-format.md) 要素。 他のすべてのビューと同様に、テーブルビューでは、オブジェクトのプロパティの値や、スクリプトによって生成される値を表示できます。

## <a name="defining-column-headers"></a>列ヘッダーの定義

1. [Tableheaders](./tableheaders-element-format.md)要素とその子要素は、テーブルの上部に表示される内容を定義します。

2. [TableColumnHeader](./tablecolumnheader-element-format.md)要素は、テーブルの列の上部に表示される内容を定義します。 これらの要素は、ヘッダーを表示する順序で指定します。

   使用できる要素の数に制限はありませんが、テーブルビュー内の [TableColumnHeader](./tablecolumnheader-element-format.md) 要素の数は、使用する [TableRowEntry](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md) 要素の数と同じである必要があります。

3. [Label](./label-element-for-tablecolumnheader-for-tablecontrol-format.md)要素は、表示されるテキストを指定します。 この要素は省略可能です。

4. [Width](./width-element-for-tablecolumnheader-for-tablecontrol-format.md)要素は、列の幅 (文字数) を指定します。 この要素は省略可能です。

5. [Alignment](./alignment-element-for-tablecolumnheader-for-tablecontrol-format.md)要素は、ラベルの表示方法を指定します。 ラベルは左、右、または中央に配置できます。 この要素は省略可能です。

## <a name="defining-the-table-rows"></a>テーブル行の定義

テーブルビューでは、 [TableRowEntries](./tablerowentries-element-for-tablecontrol-format.md) 要素の子要素を使用して、テーブルの行に表示されるデータを指定する1つ以上の定義を指定できます。 テーブルの行に対して複数の定義を指定できますが、使用されている行の定義に関係なく、行のヘッダーは同じままです。 通常、テーブルの定義は1つだけです。

次の例では、ビューには、システムの複数のプロパティの値を表示する1つの定義が用意されています。 [Displayproperty = Fullname](/dotnet/api/System.Diagnostics.Process) オブジェクト。 テーブルビューでは、プロパティの値やスクリプトの値 (例では示されていません) を行に表示できます。

```xml
<TableRowEntries>
      <TableRowEntry>
        <TableColumnItems>
          <TableColumnItem>
           <PropertyName>Status</PropertyName>
          </TableColumnItem>
          <TableColumnItem>
            <PropertyName>Name</PropertyName>
          </TableColumnItem>
          <TableColumnItem>
            <PropertyName>DisplayName</PropertyName>
          </TableColumnItem>
        </TableColumnItems>
      </TableRowEntry>
    </TableRowEntries>

```

次の XML 要素を使用して、行の定義を指定できます。

- [TableRowEntries](./tablerowentries-element-for-tablecontrol-format.md)要素とその子要素は、テーブルの行に表示される内容を定義します。

- [TableRowEntry](./listentry-element-for-listcontrol-format.md)要素は、行の定義を提供します。 少なくとも1つの [TableRowEntry](./listentry-element-for-listcontrol-format.md) が必要です。ただし、追加できる要素の数に上限はありません。 ほとんどの場合、ビューには定義が1つだけ含まれます。

- [Entryselectedby](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md)要素は、特定の定義によって表示されるオブジェクトを指定します。 この要素は省略可能であり、異なるオブジェクトを表示する複数の [TableRowEntry](./listentry-element-for-listcontrol-format.md) 要素を定義する場合にのみ必要です。

- [Wrap](./wrap-element-for-tablerowentry-for-tablecontrol-format.md)要素は、列幅を超えるテキストを次の行に表示するように指定します。 既定では、列幅を超えるテキストは切り捨てられます。

- [TableColumnItems](./tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format.md)要素は、値が行に表示されるプロパティまたはスクリプトを定義します。

- [TableColumnItem](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)要素は、値が行の列に表示されるプロパティまたはスクリプトを定義します。 行の各列には [TableColumnItem](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md) 要素が必要です。 最初のエントリは最初の列に、2番目の列には2番目のエントリが表示されます。

- [PropertyName](./propertyname-element-for-tablecolumnitem-for-tablecontrol-format.md)要素は、値が行に表示されるプロパティを指定します。 プロパティまたはスクリプトのどちらかを指定する必要がありますが、両方を指定することはできません。

- [ScriptBlock](./scriptblock-element-for-tablecolumnitem-for-tablecontrol-format.md)要素は、値が行に表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

- [FormatString](./label-element-for-listitem-for-listcontrol-format.md)要素は、プロパティまたはスクリプトの値の表示方法を定義する形式パターンを指定します。 この要素は省略可能です。

- [Alignment](./alignment-element-for-tablecolumnitem-for-tablecontrol-format.md)要素は、プロパティまたはスクリプトの値の表示方法を指定します。 値は、左、右、または中央揃えに配置できます。 この要素は省略可能です。

## <a name="defining-the-objects-that-use-the-table-view"></a>テーブルビューを使用するオブジェクトの定義

テーブルビューを使用する .NET オブジェクトを定義するには、次の2つの方法があります。 [Viewselectedby](./viewselectedby-element-format.md)要素を使用すると、ビューのすべての定義で表示できるオブジェクトを定義できます。また、 [entryselectedby](./entryselectedby-element-for-listentry-for-listcontrol-format.md)要素を使用して、ビューの特定の定義によって表示されるオブジェクトを定義することもできます。 ほとんどの場合、ビューには定義が1つしかないため、オブジェクトは通常、 [Viewselectedby](./viewselectedby-element-format.md) 要素によって定義されます。

次の例は、 [Viewselectedby](./viewselectedby-element-format.md) および [TypeName](./typename-element-for-viewselectedby-format.md) 要素を使用してテーブルビューに表示されるオブジェクトを定義する方法を示しています。 指定できる [TypeName](./typename-element-for-viewselectedby-format.md) 要素の数に制限はありません。これらの要素の順序は重要ではありません。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <TableControl>...</TableControl>
</View>
```

次の XML 要素を使用して、テーブルビューで使用されるオブジェクトを指定できます。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、リストビューによって表示されるオブジェクトを定義します。

- [TypeName](./typename-element-for-viewselectedby-format.md)要素は、ビューによって表示される .net オブジェクトを指定します。 完全修飾 .NET 型名が必要です。 ビューには少なくとも1つの種類または選択セットを指定する必要がありますが、指定できる要素の最大数はありません。

次の例では、 [Viewselectedby](./viewselectedby-element-format.md) 要素と [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md) 要素を使用します。 複数のビューを使用して表示される関連オブジェクトのセットがある場合、たとえば、同じオブジェクトのリストビューやテーブルビューを定義するときに、選択セットを使用します。 選択セットを作成する方法の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <SelectionSetName>.NET Type Set</SelectionSetName>
  </ViewSelectedBy>
  <TableControl>...</TableControl>
</View>
```

次の XML 要素を使用して、リストビューで使用されるオブジェクトを指定できます。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、リストビューによって表示されるオブジェクトを定義します。

- [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md)要素は、ビューで表示できるオブジェクトのセットを指定します。 ビューには少なくとも1つの選択セットまたは型を指定する必要がありますが、指定できる要素の最大数はありません。

次の例では、 [Entryselectedby](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md) 要素を使用して、テーブルビューの特定の定義によって表示されるオブジェクトを定義する方法を示します。 この要素を使用して、オブジェクトの .NET 型名、選択したオブジェクトのセット、または定義を使用するタイミングを指定する選択条件を指定できます。 選択条件を作成する方法の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

> [!NOTE]
> テーブルビューの複数の定義を作成する場合は、異なる列ヘッダーを指定することはできません。 表示するオブジェクトなど、テーブルの行に表示されるもののみを指定できます。

```xml
<TableRowEntry>
  <EntrySelectedBy>
    <TypeName>.NET Type</TypeName>
  </EntrySelectedBy>
</TableRowEntry>
```

次の XML 要素を使用して、リストビューの特定の定義によって使用されるオブジェクトを指定できます。

- [Entryselectedby](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md)要素は、定義によって表示されるオブジェクトを定義します。

- [TypeName](./typename-element-for-entryselectedby-for-listcontrol-format.md)要素は、定義によって表示される .net オブジェクトを指定します。 この要素を使用する場合は、完全修飾された .NET 型名が必要です。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。

- [SelectionSetName](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md)要素 (表示されません) は、この定義で表示できるオブジェクトのセットを指定します。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。

- [Selectioncondition](./selectioncondition-element-for-entryselectedby-for-listcontrol-format.md)要素 (表示されません) は、この定義を使用するために存在する必要がある条件を指定します。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。 選択条件の定義の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="using-format-strings"></a>書式指定文字列の使用

書式設定文字列をビューに追加して、データの表示方法をさらに定義できます。 次の例は、プロパティの値の書式設定文字列を定義する方法を示して `StartTime` います。

```xml
<TableColumnItem>
  <PropertyName>StartTime</PropertyName>
  <FormatString>{0:MMM} (0:DD) (0:HH):(0:MM)</FormatString>
</TableColumnItem>
```

次の XML 要素を使用して、書式パターンを指定できます。

- [TableColumnItem](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)要素は、値が行の列に表示されるプロパティまたはスクリプトを定義します。 行の各列には [TableColumnItem](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md) 要素が必要です。 最初のエントリは最初の列に、2番目の列には2番目のエントリが表示されます。

- [PropertyName](./propertyname-element-for-tablecolumnitem-for-tablecontrol-format.md)要素は、値が行に表示されるプロパティを指定します。 プロパティまたはスクリプトのどちらかを指定する必要がありますが、両方を指定することはできません。

- [FormatString](./label-element-for-listitem-for-listcontrol-format.md)要素は、プロパティまたはスクリプトの値の表示方法を定義する形式パターンを指定します。

次の例では、 `ToString` スクリプトの値の書式を設定するためにメソッドが呼び出されています。 スクリプトは、オブジェクトの任意のメソッドを呼び出すことができます。 したがって、オブジェクトに、書式設定パラメーターを持つなどのメソッドがある場合、 `ToString` スクリプトはそのメソッドを呼び出してスクリプトの出力値を書式設定できます。

```xml
<ListItem>
  <ScriptBlock>
    [String]::Format("(0,10) (1,8)", .LastWriteTime.ToString("d"),
    $_.LastWriteTime.Totring("t"))
  </ScriptBlock>
</ListItem>
```

次の XML 要素を使用して、メソッドを呼び出すことができ `ToString` ます。

- [TableColumnItem](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md)要素は、値が行の列に表示されるプロパティまたはスクリプトを定義します。 行の各列には [TableColumnItem](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md) 要素が必要です。 最初のエントリは最初の列に、2番目の列には2番目のエントリが表示されます。

- [ScriptBlock](./scriptblock-element-for-tablecolumnitem-for-tablecontrol-format.md)要素は、値が行に表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
