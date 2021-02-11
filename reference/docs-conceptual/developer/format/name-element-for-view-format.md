---
ms.date: 09/13/2016
ms.topic: reference
title: View の Name 要素 (書式)
description: View の Name 要素 (書式)
ms.openlocfilehash: 5781bcdf7a0e1eb5e9c7e97bb6acc0a383dc0262
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666461"
---
# <a name="name-element-for-view-format"></a>View の Name 要素 (書式)

ビューを識別するために使用される名前を指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) Name 要素 (形式)

## <a name="syntax"></a>構文

```xml
<Name>ViewName</Name>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Name` ます。 `Name`各ビューで許可される要素は1つだけです。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上の .NET オブジェクトのメンバーを表示するために使用されるビューを定義します。|

## <a name="text-value"></a>テキスト値

ビューの一意の表示名を指定します。 この名前には、ビューの種類 (テーブルビューやリストビューなど) への参照、ビューを使用するオブジェクトまたはオブジェクトのセット、オブジェクトを返すコマンド、またはこれらの組み合わせを含めることができます。

## <a name="remarks"></a>解説

さまざまな種類のビューの詳細については、次のトピックを参照してください。 [テーブルビュー](./creating-a-table-view.md)、 [リストビュー](./creating-a-list-view.md)、 [ワイドビュー](./creating-a-wide-view.md)、および [カスタムビュー](./creating-custom-controls.md)です。

## <a name="example"></a>例

次の例は、 `View` [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトのテーブルビューを定義する要素を示しています。 ビューの名前は "service" です。

```xml
<View>
  <Name>service</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <TableControl>...</TableControl>
</View>

```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[テーブル ビューを作成する](./creating-a-table-view.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[カスタム コントロールを作成する](./creating-custom-controls.md)

[View 要素 (書式)](./view-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
