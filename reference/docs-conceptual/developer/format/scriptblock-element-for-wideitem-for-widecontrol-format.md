---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の WideItem の ScriptBlock 要素 (書式)
description: WideControl の WideItem の ScriptBlock 要素 (書式)
ms.openlocfilehash: 68e47926e5e6b846c8a0a3dbc16d1f0d59f11dee
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659877"
---
# <a name="scriptblock-element-for-wideitem-for-widecontrol-format"></a>WideControl の WideItem の ScriptBlock 要素 (書式)

ワイドビューに値が表示されるスクリプトを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element (format) WideItem Element (format) Element for WideItem (Format)

## <a name="syntax"></a>構文

```xml
<ScriptBlock>ScriptToExecute</ScriptBlock>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ScriptBlock` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md)|ワイドビューに表示される値を持つプロパティまたはスクリプトブロックを定義します。|

## <a name="text-value"></a>テキスト値

値が表示されるスクリプトを指定します。

## <a name="remarks"></a>解説

ワイドビューのコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

この例では、 `WideItem` ビューに値が表示されるスクリプトを定義する要素を示します。

```xml
<WideItem>
  <ScriptBlock>ScriptToExecute</ScriptBlock>
</WideItem>
```

## <a name="see-also"></a>参照

[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
