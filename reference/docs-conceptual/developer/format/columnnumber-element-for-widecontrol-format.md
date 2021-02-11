---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の ColumnNumber 要素 (書式)
description: WideControl の ColumnNumber 要素 (書式)
ms.openlocfilehash: 1ddbbfbd5b53065afcc6c1326d6abf1fadedc67b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648403"
---
# <a name="columnnumber-element-for-widecontrol-format"></a>WideControl の ColumnNumber 要素 (書式)

ワイドビューに表示される列の数を指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl 要素 (format) WideControl の ColumnNumber 要素 (Format)

## <a name="syntax"></a>構文

```xml
<ColumnNumber>PositiveInteger</ColumnNumber>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ColumnNumber` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideControl 要素 (書式)](./widecontrol-element-format.md)|ビューのワイド (単一値) リスト形式を定義します。|

## <a name="text-value"></a>テキスト値

正の整数値を指定してください。

## <a name="remarks"></a>解説

ワイドビューを定義する場合は、 `AutoSize` 要素または `ColumnNumber` 要素を追加できますが、両方を追加することはできません。

ワイドビューのコンポーネントの詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

ワイドビューの例については、「 [Wide ビュー (Basic)](./wide-view-basic.md)」を参照してください。

## <a name="see-also"></a>参照

[WideControl の Autosize 要素 (形式)](./autosize-element-for-widecontrol-format.md)

[ワイド ビューを作成する](./creating-a-wide-view.md)

[ワイド ビュー (基本)](./wide-view-basic.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
