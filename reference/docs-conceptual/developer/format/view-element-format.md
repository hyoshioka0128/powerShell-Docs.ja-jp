---
ms.date: 09/13/2016
ms.topic: reference
title: View 要素 (書式)
description: View 要素 (書式)
ms.openlocfilehash: 6fed1304d94339cc90b6ae53e06483c08d937c12
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649743"
---
# <a name="view-element-format"></a>View 要素 (書式)

1つ以上の .NET オブジェクトを表示するビューを定義します。 書式設定ファイルで定義できるビューの数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式)

## <a name="syntax"></a>構文

```xml
<View>
  <Name>Friendly name of view.</Name>
  <ViewSelectedBy>...</ViewSelectedBy>
  <Controls>...</Controls>
  <GroupBy>...</GroupBy>
  <TableControl>...</TableControl>
  <ListControl>...</ListControl>
  <WideControl>...</WideControl>
  <CustomControl>...</CustomControl>
</View>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `View` ます。 コントロールの子要素を1つだけ指定し、ビューの名前とビューを使用するオブジェクトを指定する必要があります。 カスタムコントロールの定義、オブジェクトのグループ化、およびビューが帯域外であるかどうかの指定はオプションです。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[View の Controls 要素 (書式)](./controls-element-for-view-format.md)|省略可能な要素です。<br /><br /> ビュー内から名前で参照できるコントロールのセットを定義します。|
|[CustomControl 要素 (Format)](./customcontrol-element-for-groupby-format.md)|省略可能な要素です。<br /><br /> ビューのカスタムコントロール形式を定義します。|
|[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)|省略可能な要素です。<br /><br /> .NET オブジェクトのメンバーをグループ化する方法を定義します。|
|[ListControl 要素 (書式)](./listcontrol-element-format.md)|省略可能な要素です。<br /><br /> ビューのリスト形式を定義します。|
|[View の Name 要素 (書式)](./name-element-for-view-format.md)|必須の要素です。<br /><br /> ビューを参照するために使用する名前を指定します。|
|[TableControl 要素 (書式)](./tablecontrol-element-format.md)|省略可能な要素です。<br /><br /> ビューのテーブル形式を定義します。|
|[View (Format) の ViewSelectedBy 要素](./viewselectedby-element-format.md)|必須の要素です。<br /><br /> このビューに表示される .NET オブジェクトを定義します。|
|[WideControl 要素 (書式)](./widecontrol-element-format.md)|省略可能な要素です。<br /><br /> ビューのワイド (単一値) リスト形式を定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ViewDefinitions 要素 (書式)](./viewdefinitions-element-format.md)|オブジェクトを表示するために使用するビューを定義します。|

## <a name="remarks"></a>解説

さまざまなビューとカスタムコントロールのコンポーネントの詳細については、次のトピックを参照してください。

- [テーブルビューのコンポーネント](./creating-a-table-view.md)

- [リストビューのコンポーネント](./creating-a-list-view.md)

- [ワイドビューコンポーネント](./creating-a-wide-view.md)

- [カスタムコントロール](./creating-custom-controls.md)

## <a name="example"></a>例

次の例は、 `View` [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトのテーブルビューを定義する要素を示しています。

```xml
<ViewDefinitions>
  <View>
    <Name>service</Name>
    <ViewSelectedBy>
      <TypeName>System.ServiceProcess.ServiceController</TypeName>
    </ViewSelectedBy>
    <TableControl>...</TableControl>
  </View>
</ViewDefinitions>

```

## <a name="see-also"></a>参照

[ViewDefinitions 要素 (書式)](./viewdefinitions-element-format.md)

[View の Name 要素 (書式)](./name-element-for-view-format.md)

[ViewSelectedBy 要素 (書式)](./viewselectedby-element-format.md)

[View の Controls 要素 (書式)](./controls-element-for-view-format.md)

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[TableControl 要素 (書式)](./tablecontrol-element-format.md)

[ListControl 要素 (書式)](./listcontrol-element-format.md)

[WideControl 要素 (書式)](./widecontrol-element-format.md)

[CustomControl 要素 (Format)](./customcontrol-element-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
