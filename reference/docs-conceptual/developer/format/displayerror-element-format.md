---
ms.date: 09/13/2016
ms.topic: reference
title: DisplayError 要素 (書式)
description: DisplayError 要素 (書式)
ms.openlocfilehash: fb54df86a3558263687a8c417870495b7066f563
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649934"
---
# <a name="displayerror-element-format"></a>DisplayError 要素 (書式)

データの表示中にエラーが発生した場合に、文字列 #ERR が表示されることを指定します。

Configuration 要素 (Format) DefaultSettings Element (format) DisplayError 要素 (Format)

## <a name="syntax"></a>構文

```xml
<DisplayError/>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `DisplayError` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[DefaultSettings 要素 (書式)](./defaultsettings-element-format.md)|書式設定ファイルのすべてのビューに適用される共通設定を定義します。|

## <a name="remarks"></a>解説

既定では、データの一部を表示しようとしてエラーが発生すると、データの場所は空白のままになります。 この要素が true に設定されている場合、#ERR 文字列が表示されます。

## <a name="see-also"></a>参照

[DefaultSettings 要素 (書式)](./defaultsettings-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
