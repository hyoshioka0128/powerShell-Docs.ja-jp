---
ms.date: 09/13/2016
ms.topic: reference
title: ファイルにカスタム書式設定を適用する
description: ファイルにカスタム書式設定を適用する
ms.openlocfilehash: 16e1c1046e25b35ff346585a5fd930c449c04bf8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653247"
---
# <a name="custom-formatting-files"></a>ファイルにカスタム書式設定を適用する

コマンドレット、関数、およびスクリプトによって返されるオブジェクトの表示形式は、書式設定ファイル (format.ps1xml ファイル) を使用して定義されます。 これらのファイルのいくつかは、windows powershell コマンドレットによって返されるオブジェクトの既定の表示形式を定義するために、Windows PowerShell によって提供されます。 ただし、独自のカスタム書式設定ファイルを作成して、既定の表示形式を上書きしたり、独自のコマンドによって返されるオブジェクトの表示を定義したりすることもできます。

Windows PowerShell は、これらの書式設定ファイルのデータを使用して、表示される内容と、データの書式設定を決定します。 表示されるデータには、オブジェクトのプロパティまたはスクリプトブロックの値を含めることができます。  スクリプトブロックは、オブジェクトのプロパティから直接使用できない値を表示する場合に使用します。 たとえば、オブジェクトの2つのプロパティの値を加算し、その合計を個別のデータとして表示することができます。 独自の書式設定ファイルを作成する場合は、表示するオブジェクトの *ビュー* を定義する必要があります。 各オブジェクトに対して1つのビューを定義したり、複数のオブジェクトに対して1つのビューを定義したり、同じオブジェクトに対して複数のビューを定義したりできます。 定義できるビューの数に制限はありません。

> [!IMPORTANT]
> 書式設定ファイルでは、パイプラインに返されるオブジェクトの要素は決定されません。 オブジェクトがパイプラインに返されると、そのオブジェクトのすべてのメンバーが使用できるようになります。

## <a name="format-views"></a>ビューの書式設定

書式設定ビューでは、テーブル形式、リスト形式、ワイド形式、およびカスタム書式でオブジェクトを表示できます。 ほとんどの場合、各書式設定定義は、ビューを記述する XML タグのセットによって記述されます。 各ビューには、ビューの名前、ビューを使用するオブジェクト、およびテーブルビューの列と行の情報など、ビューの要素が含まれます。

次のビューを使用できます。

テーブルビューには、1つまたは複数の列にオブジェクトまたはスクリプトブロックの値のプロパティが一覧表示されます。 各列は、オブジェクトのプロパティまたはスクリプトブロック値を表します。 オブジェクトのすべてのプロパティ、オブジェクトのプロパティのサブセット、またはプロパティとスクリプトブロック値の組み合わせを表示するテーブルビューを定義できます。 テーブルの各行は、返されたオブジェクトを表します。 このビューの詳細については、「 [テーブルビュー](../format/creating-a-table-view.md)」を参照してください。

リストビューには、1つの列にオブジェクトまたはスクリプトブロックの値のプロパティが一覧表示されます。 一覧の各行には、省略可能なラベルまたはプロパティ名の後にプロパティまたはスクリプトブロックの値が表示されます。 このビューの詳細については、「 [リストビュー](../format/creating-a-list-view.md)」を参照してください。

ワイドビュー1つまたは複数の列にオブジェクトまたはスクリプトブロックの値の1つのプロパティを一覧表示します。 このビューにはラベルまたはヘッダーがありません。 このビューの詳細については、「 [Wide ビュー](../format/creating-a-wide-view.md)」を参照してください。

カスタムビューでは、テーブルビュー、リストビュー、またはワイドビューの固定構造に準拠していないオブジェクトプロパティまたはスクリプトブロック値のカスタマイズ可能なビューが表示されます。 スタンドアロンのカスタムビューを定義することも、テーブルビューやリストビューなどの別のビューで使用されるカスタムビューを定義することもできます。 このビューの詳細については、「 [カスタムビュー](../format/creating-custom-controls.md)」を参照してください。

## <a name="view-xml-elements"></a>XML 要素の表示

次の例は、2つの列を含むテーブルビューを定義するために使用される XML タグを示しています。 [Viewdefinitions](../format/viewdefinitions-element-format.md)要素は、書式設定ファイルで定義されているすべてのビューのコンテナー要素です。 [View](../format/view-element-format.md)要素は、特定のテーブル、リスト、ワイド、またはカスタムビューを定義します。 各ビュー内では、 [name](../format/name-element-for-view-format.md) 要素はビューの名前を指定し、 [viewselectedby](../format/viewselectedby-element-format.md) 要素はビューを使用するオブジェクトを定義し、さまざまなコントロール要素 ( `TableControl` 要素など) がビューの形式を定義します。

```xml
ViewDefinitions
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
    </TableControl)
  </View>
</ViewDefinitions>

```

## <a name="see-also"></a>参照

[テーブルビュー](../format/creating-a-table-view.md)

[リストビュー](../format/creating-a-list-view.md)

[ワイドビュー](../format/creating-a-wide-view.md)

[カスタムビュー](../format/creating-custom-controls.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
