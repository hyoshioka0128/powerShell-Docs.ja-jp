---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ItemSelectionCondition の ScriptBlock 要素 (書式)
description: ListControl の ItemSelectionCondition の ScriptBlock 要素 (書式)
ms.openlocfilehash: 1e518f898e0e1e62ca64f9897b1323cc6dd89ae9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92665062"
---
# <a name="scriptblock-element-for-itemselectioncondition-for-listcontrol-format"></a>ListControl の ItemSelectionCondition の ScriptBlock 要素 (書式)

条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、リスト項目が使用されます。 この要素は、リストビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl 要素 (Format) ListControl (format) ListEntry 要素の listentries for ListControl (Format) ListItems 要素の ListEntries 要素 ListEntry for ListControl (Format) の ListItem 要素 for ListItems for List コントロール (format) ItemSelectionCondition の ItemSelectionCondition for ListControl (Format) ScriptBlock 要素 for ListControl (Format)

## <a name="syntax"></a>構文

```xml
<ScriptBlock>ScriptToEvaluate</ScriptBlock>
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
|[ListControl の ListItem の ItemSelectionCondition 要素 (書式)](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md)|このリスト項目を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

評価されるスクリプトを指定します。

## <a name="remarks"></a>解説

この要素が使用されている場合、 `PropertyName` 選択条件を定義するときに要素を指定することはできません。

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
