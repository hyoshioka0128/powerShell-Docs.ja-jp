---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls 要素 (書式)
description: View の Controls 要素 (書式)
ms.openlocfilehash: 0e41f9ad35a0c45b615251417198a47bc7feb760
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668067"
---
# <a name="controls-element-for-view-format"></a>View の Controls 要素 (書式)

特定のビューで使用できるビューコントロールを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビューの要素 (書式) コントロール要素ビュー (形式)

## <a name="syntax"></a>構文

```xml
<Controls>
  <Control>...</Control>
</Controls>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Controls` ます。 この要素には、少なくとも1つの子要素が必要です。 子要素の最大数はなく、順序は重要でもありません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ビューのコントロールの Control 要素 (Format)](./control-element-for-controls-for-view-format.md)|ビューで使用できるコントロールを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上の .NET オブジェクトのメンバーを表示するために使用されるビューを定義します。|

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[Control 要素 (Format)](./control-element-for-controls-for-view-format.md)

[View 要素 (書式)](./view-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
