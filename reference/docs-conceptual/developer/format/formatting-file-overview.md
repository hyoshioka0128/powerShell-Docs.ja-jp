---
ms.date: 09/13/2016
ms.topic: reference
title: 書式設定ファイルの概要
description: 書式設定ファイルの概要
ms.openlocfilehash: 769a86d274ef3b35a322d76e2f03cb551d9204a1
ms.sourcegitcommit: 04faa7dc1122bce839295d4891bd8b2f0ecb06ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879138"
---
# <a name="formatting-file-overview"></a>書式設定ファイルの概要

コマンドによって返されるオブジェクトの表示形式 (コマンドレット、関数、およびスクリプト) は、書式設定ファイル (format.ps1xml ファイル) を使用して定義されます。 これらのファイルのいくつかは、powershell が提供するコマンドによって返されるオブジェクトの表示形式を定義するために PowerShell によって提供され [ます。たとえば](/dotnet/api/System.Diagnostics.Process) 、コマンドレットによって返される system.object オブジェクトなど `Get-Process` です。 ただし、独自のカスタム書式設定ファイルを作成して既定の表示形式を上書きすることも、独自のコマンドによって返されるオブジェクトの表示を定義するカスタム書式設定ファイルを記述することもできます。

> [!IMPORTANT]
> 書式設定ファイルでは、パイプラインに返されるオブジェクトの要素は決定されません。 オブジェクトがパイプラインに返されると、一部のメンバーが表示されていない場合でも、そのオブジェクトのすべてのメンバーが使用できるようになります。

PowerShell では、これらの書式設定ファイルのデータを使用して、表示される内容と、表示されるデータの書式設定を決定します。 表示されるデータには、オブジェクトのプロパティやスクリプトの値を含めることができます。 スクリプトは、オブジェクトのプロパティから直接使用できない値を表示する場合に使用します。たとえば、オブジェクトの2つのプロパティの値を加算し、その合計をデータの一部として表示します。 表示されるデータの書式設定を行うには、表示するオブジェクトのビューを定義します。 各オブジェクトに対して1つのビューを定義したり、複数のオブジェクトに対して1つのビューを定義したり、同じオブジェクトに対して複数のビューを定義したりできます。 定義できるビューの数に制限はありません。

## <a name="common-features-of-formatting-files"></a>フォーマットファイルの一般的な機能

各フォーマットファイルでは、ファイルで定義されているすべてのビュー間で共有できる次のコンポーネントを定義できます。

- 既定の構成設定 (テーブルの行に表示されるデータを、列の幅よりも長い場合に、次の行に表示するかどうかなど)。 これらの設定の詳細については、「 [共通の構成設定の定義](./defining-common-configuration-features.md)」を参照してください。

- 書式設定ファイルのいずれかのビューで表示できるオブジェクトのセット。 これらのセット ( *選択セット* と呼ばれます) の詳細については、「 [オブジェクトのセットの定義](./defining-selection-sets.md)」を参照してください。

- 書式設定ファイルのすべてのビューで使用できるコモンコントロール。 コントロールを使用すると、データの表示方法をより細かく制御できます。 コントロールの詳細については、「 [カスタムコントロールの定義](./creating-custom-controls.md)」を参照してください。

## <a name="formatting-views"></a>ビューの書式設定

書式設定ビューでは、オブジェクトをテーブル形式、リスト形式、ワイド形式、およびカスタム書式で表示できます。 ほとんどの場合、各書式設定定義は、ビューを記述する XML タグのセットによって記述されます。 各ビューには、ビューの名前、ビューを使用するオブジェクト、およびテーブルビューの列と行の情報など、ビューの要素が含まれます。

テーブルビューには、1つまたは複数の列にオブジェクトまたはスクリプトブロックの値のプロパティが一覧表示されます。 各列は、オブジェクトまたはスクリプト値の単一のプロパティを表します。 オブジェクトのすべてのプロパティ、オブジェクトのプロパティのサブセット、またはプロパティとスクリプト値の組み合わせを表示するテーブルビューを定義できます。 テーブルの各行は、返されたオブジェクトを表します。 テーブルビューの作成は、オブジェクトをコマンドレットにパイプする場合とよく似てい `Format-Table` ます。 このビューの詳細については、「 [テーブルビュー](./creating-a-table-view.md)」を参照してください。

リストビューには、1つの列にオブジェクトまたはスクリプトの値のプロパティが一覧表示されます。 一覧の各行には、省略可能なラベルまたはプロパティ名の後にプロパティまたはスクリプトの値が表示されます。 リストビューを作成することは、オブジェクトをコマンドレットにパイプすることとよく似てい `Format-List` ます。 このビューの詳細については、「 [リストビュー](./creating-a-list-view.md)」を参照してください。

[ワイドビュー] オブジェクトの1つのプロパティまたは1つ以上の列のスクリプト値を一覧表示します。 このビューにはラベルまたはヘッダーがありません。 ワイドビューを作成することは、オブジェクトをコマンドレットにパイプすることとよく似てい `Format-Wide` ます。 このビューの詳細については、「 [Wide ビュー](./creating-a-wide-view.md)」を参照してください。

[カスタムビュー] テーブルビュー、リストビュー、またはワイドビューの固定構造に準拠していないオブジェクトプロパティまたはスクリプト値のカスタマイズ可能なビューが表示されます。 スタンドアロンのカスタムビューを定義することも、テーブルビューやリストビューなどの別のビューで使用されるカスタムビューを定義することもできます。 カスタムビューを作成することは、オブジェクトをコマンドレットにパイプすることとよく似てい `Format-Custom` ます。 このビューの詳細については、「 [カスタムビュー](./creating-custom-controls.md)」を参照してください。

## <a name="components-of-a-view"></a>ビューのコンポーネント

次の XML の例は、ビューの基本的な XML コンポーネントを示しています。 個々の XML 要素は、どのビューを作成するかによって異なりますが、ビューの基本コンポーネントはすべて同じです。

最初に、各ビューには、 `Name` ビューを参照するために使用されるユーザーフレンドリ名を指定する要素があります。 `ViewSelectedBy`ビューによって表示される .net オブジェクトと、ビューを定義する *コントロール* 要素を定義する要素。

```xml
<ViewDefinitions>
  <View>
    <Name>NameOfView</Name>
    <ViewSelectedBy>...</ViewSelectedBy>
    <TableControl>...</TableControl>
  </View>
  <View>
    <Name>NameOfView</Name>
    <ViewSelectedBy>...</ViewSelectedBy>
    <ListControl>...</ListControl>
  <View>
  <View>
    <Name>NameOfView</Name>
    <ViewSelectedBy>...</ViewSelectedBy>
    <WideControl>...</WideControl>
  <View>
  <View>
    <Name>NameOfView</Name>
    <ViewSelectedBy>...</ViewSelectedBy>
    <CustomControl>...</CustomControl>
  </View>
</ViewDefinitions>

```

Control 要素内では、1つまたは複数の *エントリ* 要素を定義できます。 複数の定義を使用する場合は、各定義を使用する .NET オブジェクトを指定する必要があります。 通常、各コントロールに必要なエントリは1つだけです。

```xml
<ListControl>
  <ListEntries>
    <ListEntry>
      <EntrySelectedBy>...</EntrySelectedBy>
      <ListItems>...</ListItems>
    <ListEntry>
    <ListEntry>
        <EntrySelectedBy>...</EntrySelectedBy>
      <ListItems>...</ListItems>
    <ListEntry>
    <ListEntry>
        <EntrySelectedBy>...</EntrySelectedBy>
      <ListItems>...</ListItems>
    <ListEntry>
  </ListEntries>
</ListControl>

```

ビューの各エントリ要素内で、そのビューによって表示される .NET プロパティまたはスクリプトを定義する *項目* 要素を指定します。

```xml

<ListItems>
  <ListItem>...</ListItem>
  <ListItem>...</ListItem>
  <ListItem>...</ListItem>
</ListItems>

```

前の例で示したように、書式設定ファイルには複数のビューを含めることができます。ビューには複数の定義を含めることができ、各定義には複数の項目を含めることができます。

## <a name="example-of-a-table-view"></a>テーブルビューの例

次の例は、2つの列を含むテーブルビューを定義するために使用される XML タグを示しています。 [Viewdefinitions](./viewdefinitions-element-format.md)要素は、書式設定ファイルで定義されているすべてのビューのコンテナー要素です。 [View](./view-element-format.md)要素は、特定のテーブル、リスト、ワイド、またはカスタムビューを定義します。 各 [ビュー](./view-element-format.md) 要素内では、 [name](./name-element-for-view-format.md) 要素はビューの名前を指定し、 [viewselectedby](./viewselectedby-element-format.md) 要素はビューを使用するオブジェクトを定義します。また、次の例に示すように、さまざまなコントロール要素が `TableControl` ビューの型を定義します。

```xml
<ViewDefinitions>
  <View>
    <Name>Name of View</Name>
    <ViewSelectedBy>
      <TypeName>Object to display using this view</TypeName>
      <TypeName>Object to display using this view</TypeName>
    </ViewSelectedBy>
    <TableControl>
      <TableHeaders>
        <TableColumnHeader>
          <Width></Width>
        </TableColumnHeader>
        <TableColumnHeader>
          <Width></Width>
        </TableColumnHeader>
      </TableHeaders>
      <TableRowEntries>
        <TableRowEntry>
          <TableColumnItems>
            <TableColumnItem>
              <PropertyName>Header for column 1</PropertyName>
            </TableColumnItem>
            <TableColumnItem>
              <PropertyName>Header for column 2</PropertyName>
            </TableColumnItem>
          </TableColumnItems>
        </TableRowEntry>
      </TableRowEntries>
    </TableControl>
  </View>
</ViewDefinitions>

```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[テーブル ビューを作成する](./creating-a-table-view.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[カスタム コントロールを作成する](./creating-custom-controls.md)

[PowerShell の形式と種類のファイルの作成](./writing-a-powershell-formatting-file.md)
