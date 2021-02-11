---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の CustomControl 要素 (書式)
description: GroupBy の CustomControl 要素 (書式)
ms.openlocfilehash: 633cfcbd10206dc8d7fb4bc1d0092f19aa5bde7c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646102"
---
# <a name="customcontrol-element-for-groupby-format"></a>GroupBy の CustomControl 要素 (書式)

新しいグループを表示するカスタムコントロールを定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) groupby 要素 (format) の CustomControl 要素を指定します。

## <a name="syntax"></a>構文

```xml
<CustomControl>
  <CustomEntries>...</CustomEntries>
<CustomControl>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomControl` ます。 任意の数の子要素を指定し、任意の順序で一覧表示することができます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-groupby-format.md)|必須の要素です。<br /><br /> コントロールの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)|Windows PowerShell がオブジェクトの新しいグループを表示する方法を定義します。|

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[GroupBy の CustomControl の CustomEntries 要素 (書式)](./customentries-element-for-customcontrol-for-groupby-format.md)

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
