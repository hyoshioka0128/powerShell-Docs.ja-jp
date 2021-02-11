---
ms.date: 09/13/2016
ms.topic: reference
title: TableControl 要素 (書式)
description: TableControl 要素 (書式)
ms.openlocfilehash: 93e05e22d61504d7781b6f3c07f9a3fd0b3c9e8a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651464"
---
# <a name="tablecontrol-element-format"></a>TableControl 要素 (書式)

ビューのテーブル形式を定義します。

ViewDefinitions 要素 (Format) ビュー要素 (Format) TableControl 要素 (Format)

## <a name="syntax"></a>構文

```xml
<TableControl>
  <AutoSize/>
  <HideTableHeaders/>
  <TableHeaders>...</TableHeaders>
  <TableRowEntries>...</TableRowEntries>
</TableControl>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TableControl` ます。 テーブルの行を指定する必要があります。 その他のすべての子要素は省略可能です。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[TableControl の AutoSize 要素 (書式)](./autosize-element-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> データのサイズに基づいて列のサイズと列の数を調整するかどうかを指定します。|
|[TableControl (Format) の HideTableHeaders 要素](./hidetableheaders-element-format.md)|省略可能な要素です。<br /><br /> テーブルのヘッダーが表示されていないかどうかを示します。|
|[TableControl の TableHeaders 要素 (Format)](./tableheaders-element-format.md)|必須の要素です。<br /><br /> テーブルビューの列のデータのラベル、幅、および配置を定義します。|
|[TableControl の TableRowEntries 要素 (書式)](./tablerowentries-element-for-tablecontrol-format.md)|省略可能な要素です。<br /><br /> テーブルビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上のオブジェクトのメンバーを表示するために使用されるビューを定義します。|

## <a name="remarks"></a>解説

テーブルビューのコンポーネントの詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」を参照してください。

## <a name="example"></a>例

この例は、 `TableControl` [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトのプロパティを表示するために使用される要素を示しています。

```xml
<View>
  <Name>service</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <TableControl>
    <TableHeaders>...</TableHeaders>
    <TableRowEntries>...</TableRowEntries>
  </TableControl>
</View>

```

## <a name="see-also"></a>参照

[テーブル ビューを作成する](./creating-a-table-view.md)

[View 要素 (書式)](./view-element-format.md)

[TableControl の AutoSize 要素 (書式)](./autosize-element-for-tablecontrol-format.md)

[HideTableHeaders 要素 (Format)](./hidetableheaders-element-format.md)

[TableHeaders 要素 (Format)](./tableheaders-element-format.md)

[TableRowEntries 要素 (Format)](./tablerowentries-element-for-tablecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
