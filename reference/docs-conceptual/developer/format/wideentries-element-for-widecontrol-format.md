---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の WideEntries 要素 (書式)
description: WideControl の WideEntries 要素 (書式)
ms.openlocfilehash: 567b8acdd0d2e8d5daaef2c31b4fe4ce38c23a47
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651236"
---
# <a name="wideentries-element-for-widecontrol-format"></a>WideControl の WideEntries 要素 (書式)

ワイドビューの定義を提供します。 ワイドビューでは、1つまたは複数の定義を指定する必要があります。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl 要素 (format) WideEntries 要素 (形式)

## <a name="syntax"></a>構文

```xml
<WideEntries>
  <WideEntry>...</WideEntry>
</WideEntries>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `WideEntries` ます。 少なくとも1つの子要素を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md)|ワイドビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideControl 要素 (書式)](./widecontrol-element-format.md)|ビューのワイド (単一値) リスト形式を定義します。|

## <a name="remarks"></a>解説

ワイドビューは、オブジェクトごとに1つのプロパティ値またはスクリプト値を表示するリスト形式です。 ワイドビューのコンポーネントの詳細については、「 [ワイドビューコンポーネント](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `WideEntries` 1 つの要素を定義する要素を示して `WideEntry` います。 要素には、 `WideEntry` `WideItem` ビューに表示されるプロパティまたはスクリプトの値を定義する1つの要素が含まれます。

```xml
<WideControl>
  <WideEntries>
    <WideEntry>
      <WideItem>...</WideItem>
    <WideEntry>
  </WideEntries>
</WideControl>
```

ワイドビューの完全な例については、「 [Wide ビュー (Basic)](./wide-view-basic.md)」を参照してください。

## <a name="see-also"></a>参照

[ワイド ビューを作成する](./creating-a-wide-view.md)

[WideControl 要素 (書式)](./widecontrol-element-format.md)

[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
