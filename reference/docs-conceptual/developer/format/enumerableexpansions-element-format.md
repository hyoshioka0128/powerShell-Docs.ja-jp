---
ms.date: 09/13/2016
ms.topic: reference
title: EnumerableExpansions 要素 (書式)
description: EnumerableExpansions 要素 (書式)
ms.openlocfilehash: 789599e6ff368b4685c7937d5b6eb38618752f9e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660178"
---
# <a name="enumerableexpansions-element-format"></a>EnumerableExpansions 要素 (書式)

.NET コレクションオブジェクトがビューに表示されるときの展開方法を定義します。

Configuration 要素 (Format) DefaultSettings 要素 (Format) 列挙 Able展開要素 (形式)

## <a name="syntax"></a>構文

```xml
<EnumerableExpansions>
  <EnumerableExpansion>...</EnumerableExpansion>
</EnumerableExpansions>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `EnumerableExpansions` ます。 使用できる子要素の数に制限はありません。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[EnumerableExpansion 要素 (書式)](./enumerableexpansion-element-format.md)|省略可能な要素です。<br /><br /> ビューに表示されるときに展開される特定の .NET コレクションオブジェクトを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[DefaultSettings 要素 (書式)](./defaultsettings-element-format.md)|書式設定ファイルのすべてのビューに適用される共通設定を定義します。|

## <a name="remarks"></a>解説

この要素は、コレクションオブジェクトとコレクション内のオブジェクトをどのように表示するかを定義するために使用されます。 この場合、コレクションオブジェクト  **は、system.string インターフェイスを** サポートする任意のオブジェクトを参照します。

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
