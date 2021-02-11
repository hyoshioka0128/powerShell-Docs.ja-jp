---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の Control の Name 要素 (書式)
description: View の Controls の Control の Name 要素 (書式)
ms.openlocfilehash: 52b7170777a35596767c34f2d58106dfa6479567
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666486"
---
# <a name="name-element-for-control-for-controls-for-view-format"></a>View の Controls の Control の Name 要素 (書式)

コントロールの名前を指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (書式) コントロールのコントロール要素 (format) を表示するコントロールの要素 (書式)

## <a name="syntax"></a>構文

```xml
<Name>ControlName</Name>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `Name` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ビューのコントロールの Control 要素 (Format)](./control-element-for-controls-for-view-format.md)|ビューで使用できるコントロールと、コントロールを参照するために使用される名前を定義します。|

## <a name="text-value"></a>テキスト値

コントロールを参照するために使用する名前を指定します。

## <a name="remarks"></a>解説

ここで指定した名前は、このコントロールを参照するために次の要素で使用できます。

- テーブル、リスト、ワイドコントロール、またはカスタムコントロールビューを作成するときに、コントロールを指定するには、次の要素を使用します。 [GroupBy 要素 (Format)](./groupby-element-for-view-format.md)

- ビューで使用できる別のコントロールを作成する場合、このコントロールは、[ビュー (Format) のコントロールの CustomItem の式のバインド要素](./expressionbinding-element-for-customitem-for-controls-for-view-format.md)によって指定できます。

## <a name="see-also"></a>参照

[View の GroupBy 要素 (書式)](./groupby-element-for-view-format.md)

[View の Controls の CustomItem の ExpressionBinding 要素 (書式)](./expressionbinding-element-for-customitem-for-controls-for-view-format.md)

[ビューのコントロールの Control 要素 (Format)](./control-element-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
