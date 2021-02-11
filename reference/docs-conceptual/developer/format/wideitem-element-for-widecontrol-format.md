---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の WideItem 要素 (書式)
description: WideControl の WideItem 要素 (書式)
ms.openlocfilehash: b9c416bbe3befcd689b8a2c0b72a8ff1301b9659
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647807"
---
# <a name="wideitem-element-for-widecontrol-format"></a>WideControl の WideItem 要素 (書式)

値が表示されるプロパティまたはスクリプトを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (Format) WideEntries Element (format) WideEntry Element (format) WideItem Element (Format)

## <a name="syntax"></a>構文

```xml
<WideItem>
  <PropertyName>.NetTypeProperty</PropertyName>
  <ScriptBlock>ScriptToExecute</ScriptBlock>
  <FormatString>FormatPattern</FormatString>
</WideItem>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `WideItem` ます。 `FormatString` 要素は省略可能です。 ただし、要素または要素を指定する必要があり `PropertyName` `ScriptBlock` ますが、両方を指定することはできません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[WideControl の WideItem の FormatString 要素 (書式)](./formatstring-element-for-wideitem-for-widecontrol-format.md)|省略可能な要素です。<br /><br /> プロパティまたはスクリプトの値をビューに表示する方法を定義する形式パターンを指定します。|
|[WideItem の PropertyName 要素 (形式)](./propertyname-element-for-wideitem-for-widecontrol-format.md)|ワイドビューに表示される値を持つオブジェクトのプロパティを指定します。|
|[WideItem の ScriptBlock 要素 (形式)](./scriptblock-element-for-wideitem-for-widecontrol-format.md)|ワイドビューに値が表示されるスクリプトを指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md)|ワイドビューの定義を提供します。|

## <a name="remarks"></a>解説

ワイドビューのコンポーネントの詳細については、「 [Wide ビュー](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `WideEntry` 1 つの要素を定義する要素を示して `WideItem` います。 要素は、 `WideItem` ビューに値が表示されるプロパティまたはスクリプトを定義します。

```xml
<WideEntry>
  <WideItem>
    <PropertyName>ProcessName</PropertyName>
  </WideItem>
</WideEntry>
```

ワイドビューの完全な例については、「 [Wide ビュー (Basic)](./wide-view-basic.md)」を参照してください。

## <a name="see-also"></a>参照

[WideControl の WideItem の FormatString 要素 (書式)](./formatstring-element-for-wideitem-for-widecontrol-format.md)

[WideItem の PropertyName 要素 (形式)](./propertyname-element-for-wideitem-for-widecontrol-format.md)

[WideItem の ScriptBlock 要素 (形式)](./scriptblock-element-for-wideitem-for-widecontrol-format.md)

[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
