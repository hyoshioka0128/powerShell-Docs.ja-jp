---
ms.date: 09/13/2016
ms.topic: reference
title: ViewSelectedBy 要素 (書式)
description: ViewSelectedBy 要素 (書式)
ms.openlocfilehash: ac3c7de299b3009a067a476a024c6a6fcb5dce02
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667710"
---
# <a name="viewselectedby-element-format"></a>ViewSelectedBy 要素 (書式)

ビューに表示される .NET オブジェクトを定義します。 各ビューには、少なくとも1つの .NET オブジェクトを指定する必要があります。

ViewDefinitions 要素 (Format) ビュー要素 (形式) ViewSelectedBy 要素 (形式)

## <a name="syntax"></a>構文

```xml
<ViewSelectedBy>
  <TypeName>Nameof.NetType</TypeName>
  <SelectionSetName>SelectionSet</SelectionSetName>
</ViewSelectedBy>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ViewSelectedBy` ます。 この要素に `TypeName` は、少なくとも1つの要素または子要素を含める必要があり `SelectionSetName` ます。 指定できる子要素の数に制限はありません。また、順序が重要でもありません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ViewSelectedBy の TypeName 要素 (書式)](./typename-element-for-viewselectedby-format.md)|省略可能な要素です。<br /><br /> ビューに表示される .NET オブジェクトを指定します。|
|[ViewSelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-viewselectedby-format.md)|省略可能な要素です。<br /><br /> ビューに表示される一連の .NET オブジェクトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上の .NET オブジェクトを表示するビューを定義します。|

## <a name="remarks"></a>解説

この要素をさまざまなビューで使用する方法の詳細については、「 [テーブルビューコンポーネント](./creating-a-table-view.md)」、「 [リストビュー](./creating-a-list-view.md)コンポーネント」、「 [ワイドビューコンポーネント](./creating-a-wide-view.md)」、および「 [カスタムコントロールコンポーネント](./creating-custom-controls.md)」を参照してください。

要素は、 `SelectionSetName` 書式設定ファイルが複数のビューによって表示されるオブジェクトのセットを定義する場合に使用されます。 選択セットを定義および参照する方法の詳細については、「 [オブジェクトのセットの定義](./defining-selection-sets.md)」を参照してください。

## <a name="example"></a>例

リストビューの [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトを指定する方法を次の例に示します。 テーブル、ワイド、およびカスタムビューでも同じスキーマが使用されます。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <ListControl>...</ListControl>
</View>
```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[テーブル ビューを作成する](./creating-a-table-view.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[カスタム コントロールを作成する](./creating-custom-controls.md)

[選択セットを定義する](./defining-selection-sets.md)

[ViewSelectedBy の SelectionSetName 要素 (書式)](./selectionsetname-element-for-viewselectedby-format.md)

[TypeName 要素 (Format)](./typename-element-for-viewselectedby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
