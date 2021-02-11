---
ms.date: 09/13/2016
ms.topic: reference
title: ViewSelectedBy の TypeName 要素 (書式)
description: ViewSelectedBy の TypeName 要素 (書式)
ms.openlocfilehash: 62edc2fe4b4c1c5f1b17dd2f8b0943f28ff5dfb7
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667727"
---
# <a name="typename-element-for-viewselectedby-format"></a>ViewSelectedBy の TypeName 要素 (書式)

ビューに表示される .NET オブジェクトを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) ViewSelectedBy 要素 (Format) ViewSelectedBy (形式) の TypeName 要素

## <a name="syntax"></a>構文

```xml
<TypeName>FullyQualifiedTypeName</TypeName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TypeName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ViewSelectedBy 要素 (書式)](./viewselectedby-element-format.md)|ビューに表示される .NET オブジェクトを定義します。|

## <a name="text-value"></a>テキスト値

.NET 型の完全修飾名 (など) を指定し `System.IO.DirectoryInfo` ます。

## <a name="remarks"></a>解説

この要素をさまざまなビューで使用する方法の詳細については、「 [テーブルビューの作成](./creating-a-table-view.md)」、「 [リストビュー](./creating-a-list-view.md)の作成」、「 [ワイドビューの作成](./creating-a-wide-view.md)」、および「 [カスタムビューコンポーネント](./creating-custom-controls.md)」を参照してください。

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

[ViewSelectedBy 要素 (書式)](./viewselectedby-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
