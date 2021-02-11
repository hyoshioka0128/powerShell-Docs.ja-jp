---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の WideEntry 要素 (書式)
description: WideControl の WideEntry 要素 (書式)
ms.openlocfilehash: 3faaf767d11914792effd6765beed956a502c642
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92664555"
---
# <a name="wideentry-element-for-widecontrol-format"></a>WideControl の WideEntry 要素 (書式)

ワイドビューの定義を提供します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element (Format)

## <a name="syntax"></a>構文

```xml
<WideEntry>
  <EntrySelectedBy>...</EntrySelectedBy>
  <WideItem>...</WideItem>
</WideEntry>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `WideEntry` ます。 1つの子要素を指定する必要があり `WideItem` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[WideEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-wideentry-format.md)|省略可能な要素です。<br /><br /> このワイドエントリ定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|
|[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md)|必須の要素です。<br /><br /> 値が表示されるプロパティまたはスクリプトを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideEntries 要素 (Format)](./wideentries-element-for-widecontrol-format.md)|ワイドビューの定義を提供します。|

## <a name="remarks"></a>解説

ワイドビューは、オブジェクトごとに1つのプロパティ値またはスクリプト値を表示するリスト形式です。 他の種類のビューとは異なり、各ビュー定義に指定できる項目要素は1つだけです。 ワイドビューのその他のコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、 `WideEntry` 1 つの要素を定義する要素を示して `WideItem` います。 要素は、 `WideItem` ビューに値が表示されるプロパティを定義します。

```xml
<WideEntries>
  <WideEntry>
    <WideItem>
      <PropertyName>ProcessName</PropertyName>
    </WideItem>
  </WideEntry>
</WideEntries>

```

ワイドビューの完全な例については、「 [Wide ビュー (Basic)](./wide-view-basic.md)」を参照してください。

## <a name="see-also"></a>参照

[ワイド ビューを作成する](./creating-a-wide-view.md)

[WideEntry の SelectionCondition 要素 (形式)](./selectioncondition-element-for-entryselectedby-for-widecontrol-format.md)

[WideEntry の SelectionSetName 要素 (形式)](./selectionsetname-element-for-entryselectedby-for-widecontrol-format.md)

[WideEntry の TypeName 要素 (Format)](./typename-element-for-entryselectedby-for-wideentry-format.md)

[WideEntries 要素 (Format)](./wideentries-element-for-widecontrol-format.md)

[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
