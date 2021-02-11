---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の AutoSize 要素 (書式)
description: WideControl の AutoSize 要素 (書式)
ms.openlocfilehash: 42dfae337ba8805e1ddcff4269401afdb3e281f6
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92650006"
---
# <a name="autosize-element-for-widecontrol-format"></a>WideControl の AutoSize 要素 (書式)

データのサイズに基づいて列のサイズと列の数を調整するかどうかを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl 要素 (Format) WideControl の Autosize 要素 (Format)

## <a name="syntax"></a>構文

```xml
<AutoSize/>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `AutoSize` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideControl 要素 (書式)](./widecontrol-element-format.md)|ビューのワイド (単一値) リスト形式を定義します。|

## <a name="remarks"></a>解説

ワイドビューを定義する場合は、 `AutoSize` 要素または [columnnumber](./columnnumber-element-for-widecontrol-format.md) 要素を追加できますが、両方を追加することはできません。

ワイドビューのコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

ワイドビューの例については、「 [Wide ビュー (Basic)](./wide-view-basic.md)」を参照してください。

## <a name="see-also"></a>参照

[WideControl の ColumnNumber 要素 (書式)](./columnnumber-element-for-widecontrol-format.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[WideControl 要素 (書式)](./widecontrol-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
