---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls 要素 (書式)
description: Configuration の Controls 要素 (書式)
ms.openlocfilehash: 53f874ddccf3b4f1f0a23aad608e786524bde830
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668101"
---
# <a name="controls-element-for-configuration-format"></a>Configuration の Controls 要素 (書式)

書式設定ファイルのすべてのビューで使用できる共通コントロールを定義します。

Configuration 要素 (Format) コントロールの構成要素 (形式)

## <a name="syntax"></a>構文

```xml
<Controls>
  <Control>...</Control>
</Controls>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Controls` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の Control 要素 (書式)](./control-element-for-controls-for-configuration-format.md)|必須の要素です。<br /><br /> 書式設定ファイルのすべてのビューで使用できるコモンコントロールを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration 要素 (書式)](./configuration-element-format.md)|書式設定ファイルのトップレベルの要素を表します。|

## <a name="remarks"></a>解説

任意の数のコモンコントロールを作成できます。 コントロールごとに、コントロールとそのコンポーネントを参照するために使用する名前を指定する必要があります。

## <a name="see-also"></a>参照

[Configuration 要素 (書式)](./configuration-element-format.md)

[Configuration の Controls の Control 要素 (書式)](./control-element-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
