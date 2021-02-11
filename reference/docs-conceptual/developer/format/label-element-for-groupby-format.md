---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の Label 要素 (書式)
description: GroupBy の Label 要素 (書式)
ms.openlocfilehash: ff4b0dec01bb5b472b1813540661791b91568eed
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649784"
---
# <a name="label-element-for-groupby-format"></a>GroupBy の Label 要素 (書式)

新しいグループが検出されたときに表示されるラベルを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) groupby のビュー (format) Label 要素の groupby (形式)

## <a name="syntax"></a>構文

```xml
<Label>DisplayedLabel</Label>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Label` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)|オブジェクトの新しいグループをどのように表示するかを定義します。|

## <a name="text-value"></a>テキスト値

Windows PowerShell で新しいプロパティまたはスクリプト値が検出されたときに表示されるテキストを指定します。

## <a name="remarks"></a>解説

Windows PowerShell では、この要素で指定されたテキストに加えて、グループを開始する新しい値が表示され、グループの前後に空白行が追加されます。

## <a name="example"></a>例

次の例は、新しいグループのラベルを示しています。 表示されるラベルは次のようになります。 `Service Type: NewValueofProperty`

```xml
<GroupBy>
  <Label>Service Type</Label>
  <PropertyName>ServiceType</PropertyName>
</GroupBy>

```

この要素を含む完全な書式設定ファイルの例については、「 [Wide ビュー (GroupBy)](./wide-view-groupby.md)」を参照してください。

## <a name="see-also"></a>参照

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
