---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の WideItem の PropertyName 要素 (書式)
description: WideControl の WideItem の PropertyName 要素 (書式)
ms.openlocfilehash: 1d4d5eaf7708dfbd7997122fac156a36487538ea
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665619"
---
# <a name="propertyname-element-for-wideitem-for-widecontrol-format"></a>WideControl の WideItem の PropertyName 要素 (書式)

ワイドビューに表示される値を持つオブジェクトのプロパティを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element (format) WideItem Element (format) PropertyName Element for WideItem (Format)

## <a name="syntax"></a>構文

```xml
<PropertyName>.NetTypeProperty</PropertyName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `PropertyName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md)|ワイドビューに表示される値を持つプロパティまたはスクリプトを定義します。|

## <a name="text-value"></a>テキスト値

値が表示されるプロパティの名前を指定します。

## <a name="remarks"></a>解説

ワイドビューのコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

この例 [は、ProcessName オブジェクトの](/dotnet/api/System.Diagnostics.Process) プロパティの値を表示するワイドビューを示しています。

```xml
View>
  <Name>process</Name>
  <ViewSelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </ViewSelectedBy>
  <WideControl>
    <WideEntries>
      <WideEntry>
        <WideItem>
          <PropertyName>ProcessName</PropertyName>
        </WideItem>
      </WideEntry>
    </WideEntries>
  </WideControl>
</View>

```

## <a name="see-also"></a>参照

[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
