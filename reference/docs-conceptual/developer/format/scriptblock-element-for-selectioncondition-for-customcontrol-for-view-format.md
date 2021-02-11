---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomControl の SelectionCondition の ScriptBlock 要素 (書式)
description: View の CustomControl の SelectionCondition の ScriptBlock 要素 (書式)
ms.openlocfilehash: 78b977548243b6f3a658f15a0249d8cad12e2f1b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92664922"
---
# <a name="scriptblock-element-for-selectioncondition-for-customcontrol-for-view-format"></a>View の CustomControl の SelectionCondition の ScriptBlock 要素 (書式)

条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、定義が使用されます。 この要素は、カスタムコントロールビューを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (形式) ビュー要素 (format) CustomControl のビュー (形式) の CustomEntries 要素の CustomControl for view (format) の CustomEntries の要素の CustomControl for ビューの CustomEntries に使用します。 (フォーマット) CustomControl for view (Format) の SelectionCondition の CustomControl for ビュー (format) ScriptBlock 要素の EntrySelectedBy for CustomControl for ビュー (形式) の SelectionCondition 要素のための Customentries 要素

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
|[CustomControl for ビュー (Format) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-customcontrol-format.md)|コントロール定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

評価されるスクリプトを指定します。

## <a name="remarks"></a>解説

選択条件には、評価するスクリプトまたはプロパティ名を少なくとも1つ指定する必要がありますが、両方を指定することはできません。 選択条件の使用方法の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="see-also"></a>参照

[CustomControl for ビュー (Format) の EntrySelectedBy の SelectionCondition 要素](./selectioncondition-element-for-entryselectedby-for-customcontrol-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
