---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl 要素 (書式)
description: WideControl 要素 (書式)
ms.openlocfilehash: f88e1ce18f87e5e47de473298b3ecf070b71c192
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651264"
---
# <a name="widecontrol-element-format"></a>WideControl 要素 (書式)

ビューのワイド (単一値) リスト形式を定義します。 このビューには、各オブジェクトの1つのプロパティ値またはスクリプト値が表示されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl 要素 (形式)

## <a name="syntax"></a>構文

```xml
<WideControl>
  <AutoSize/>
  <ColumnNumber>PositiveInteger</ColumnNumber>
  <WideEntries>...</WideEntries>
</WideControl>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `WideControl` ます。 との各 `AutoSize` 要素を同時に指定することはできません `ColumnNumber` 。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[WideControl の AutoSize 要素 (書式)](./autosize-element-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> データのサイズに基づいて列のサイズと列の数を調整するかどうかを指定します。|
|[WideControl の ColumnNumber 要素 (書式)](./columnnumber-element-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> ワイドビューに表示される列の数を指定します。|
|[WideEntries 要素 (Format)](./wideentries-element-for-widecontrol-format.md)|必須の要素です。<br /><br /> ワイドビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上の .NET オブジェクトを表示するために使用されるビューを定義します。|

## <a name="remarks"></a>解説

ワイドビューを定義する場合 `AutoSize` は、要素またはを追加でき `ColumnNumber` ますが、両方を追加することはできません。

ほとんどの場合、ワイドビューごとに1つの定義のみが必要ですが、同じビューを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を指定できます。 このような場合は、オブジェクトまたはオブジェクトのセットごとに個別の定義を指定できます。

ワイドビューのコンポーネントの詳細については、「 [ワイドビューコンポーネント](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `WideControl` system.object オブジェクトのプロパティを表示するために使用[](/dotnet/api/System.Diagnostics.Process)される要素を示しています。

```xml
<View>
  <Name>process</Name>
  <ViewSelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </ViewSelectedBy>
  <WideControl>
    <WideEntries>...</WideEntries>
  </WideControl>
</View>
```

ワイドビューの完全な例については、「 [Wide ビュー (Basic)](./wide-view-basic.md)」を参照してください。

## <a name="see-also"></a>参照

[WideControl の Autosize 要素 (形式)](./autosize-element-for-widecontrol-format.md)

[WideControl の ColumnNumber 要素 (書式)](./columnnumber-element-for-widecontrol-format.md)

[View 要素 (書式)](./view-element-format.md)

[WideEntries 要素 (Format)](./wideentries-element-for-widecontrol-format.md)

[ワイド ビュー (基本)](./wide-view-basic.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
