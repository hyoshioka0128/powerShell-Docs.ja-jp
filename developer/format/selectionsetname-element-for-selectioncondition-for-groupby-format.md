---
title: GroupBy (形式) の SelectionCondition SelectionSetName 要素 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 7b9a4912-d755-42f3-8058-53c0797e28e4
caps.latest.revision: 6
ms.openlocfilehash: 371913eda2b09ff6788494b68738f2ad53ccb115
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56856758"
---
# <a name="selectionsetname-element-for-selectioncondition-for-groupby-format"></a><span data-ttu-id="07f46-102">GroupBy の SelectionCondition の SelectionSetName 要素 (書式)</span><span class="sxs-lookup"><span data-stu-id="07f46-102">SelectionSetName Element for SelectionCondition for GroupBy (Format)</span></span>

<span data-ttu-id="07f46-103">条件をトリガーする .NET 型のセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="07f46-103">Specifies the set of .NET types that trigger the condition.</span></span> <span data-ttu-id="07f46-104">このセット内の型のいずれかが存在して、条件が満たされると、このコントロールを使用して、オブジェクトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="07f46-104">When any of the types in this set are present, the condition is met, and the object is displayed by using this control.</span></span> <span data-ttu-id="07f46-105">この要素は、オブジェクトの新しいグループを表示する方法を定義するときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="07f46-105">This element is used when defining how a new group of objects is displayed.</span></span>

<span data-ttu-id="07f46-106">構成要素 (形式) ViewDefinitions 要素 (形式) 表示要素 (形式) GroupBy 要素の GroupBy (形式) CustomEntry 要素にカスタム コントロールの GroupBy (形式) CustomEntries 要素のカスタム コントロール要素をビュー (形式)GroupBy (形式) の SelectionCondition の GroupBy (形式) SelectionSetName 要素 EntrySelectedBy の GroupBy (形式) SelectionCondition 要素 CustomEntry の GroupBy (形式) EntrySelectedBy 要素にカスタム コントロール</span><span class="sxs-lookup"><span data-stu-id="07f46-106">Configuration Element (Format) ViewDefinitions Element (Format) View Element (Format) GroupBy Element for View (Format) CustomControl Element for GroupBy (Format) CustomEntries Element for CustomControl for GroupBy (Format) CustomEntry Element for CustomControl for GroupBy (Format) EntrySelectedBy Element for CustomEntry for GroupBy (Format) SelectionCondition Element for EntrySelectedBy for GroupBy (Format) SelectionSetName Element for SelectionCondition for GroupBy (Format)</span></span>

## <a name="syntax"></a><span data-ttu-id="07f46-107">構文</span><span class="sxs-lookup"><span data-stu-id="07f46-107">Syntax</span></span>

```xml
<SelectionSetName>NameofSelectionSet</SelectionSetName>
```

## <a name="attributes-and-elements"></a><span data-ttu-id="07f46-108">属性と要素</span><span class="sxs-lookup"><span data-stu-id="07f46-108">Attributes and Elements</span></span>

<span data-ttu-id="07f46-109">次のセクションでは、属性、子要素、およびの親要素について説明します、`SelectionSetName`要素。</span><span class="sxs-lookup"><span data-stu-id="07f46-109">The following sections describe attributes, child elements, and the parent element of the `SelectionSetName` element.</span></span>

### <a name="attributes"></a><span data-ttu-id="07f46-110">属性</span><span class="sxs-lookup"><span data-stu-id="07f46-110">Attributes</span></span>

<span data-ttu-id="07f46-111">なし。</span><span class="sxs-lookup"><span data-stu-id="07f46-111">None.</span></span>

### <a name="child-elements"></a><span data-ttu-id="07f46-112">子要素</span><span class="sxs-lookup"><span data-stu-id="07f46-112">Child Elements</span></span>

<span data-ttu-id="07f46-113">なし。</span><span class="sxs-lookup"><span data-stu-id="07f46-113">None.</span></span>

### <a name="parent-elements"></a><span data-ttu-id="07f46-114">親要素</span><span class="sxs-lookup"><span data-stu-id="07f46-114">Parent Elements</span></span>

|<span data-ttu-id="07f46-115">要素</span><span class="sxs-lookup"><span data-stu-id="07f46-115">Element</span></span>|<span data-ttu-id="07f46-116">説明</span><span class="sxs-lookup"><span data-stu-id="07f46-116">Description</span></span>|
|-------------|-----------------|
|[<span data-ttu-id="07f46-117">GroupBy (形式) の EntrySelectedBy SelectionCondition 要素</span><span class="sxs-lookup"><span data-stu-id="07f46-117">SelectionCondition Element for EntrySelectedBy for GroupBy (Format)</span></span>](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)|<span data-ttu-id="07f46-118">コントロールの定義を使用するのに必要な条件を定義します。</span><span class="sxs-lookup"><span data-stu-id="07f46-118">Defines a condition that must exist for the control definition to be used.</span></span>|

## <a name="text-value"></a><span data-ttu-id="07f46-119">テキスト値</span><span class="sxs-lookup"><span data-stu-id="07f46-119">Text Value</span></span>

<span data-ttu-id="07f46-120">選択範囲のセットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="07f46-120">Specify the name of the selection set.</span></span>

## <a name="remarks"></a><span data-ttu-id="07f46-121">コメント</span><span class="sxs-lookup"><span data-stu-id="07f46-121">Remarks</span></span>

<span data-ttu-id="07f46-122">選択範囲のセットは、書式設定ファイルを定義する任意のビューで使用できる .NET オブジェクトの一般的なグループです。</span><span class="sxs-lookup"><span data-stu-id="07f46-122">Selection sets are common groups of .NET objects that can be used by any view that the formatting file defines.</span></span> <span data-ttu-id="07f46-123">作成と選択範囲のセットの参照の詳細については、次を参照してください。[選択範囲の設定を定義する](./defining-selection-sets.md)します。</span><span class="sxs-lookup"><span data-stu-id="07f46-123">For more information about creating and referencing selection sets, see [Defining Selection Sets](./defining-selection-sets.md).</span></span>

<span data-ttu-id="07f46-124">この要素が指定されている場合は指定できません、 [TypeName](./typename-element-for-selectioncondition-for-groupby-format.md)要素。</span><span class="sxs-lookup"><span data-stu-id="07f46-124">When this element is specified, you cannot specify the [TypeName](./typename-element-for-selectioncondition-for-groupby-format.md) element.</span></span> <span data-ttu-id="07f46-125">選択条件を定義する詳細については、次を参照してください。[を表示するデータの条件を定義する](./defining-conditions-for-displaying-data.md)します。</span><span class="sxs-lookup"><span data-stu-id="07f46-125">For more information about defining selection conditions, see [Defining Conditions for Displaying Data](./defining-conditions-for-displaying-data.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="07f46-126">参照</span><span class="sxs-lookup"><span data-stu-id="07f46-126">See Also</span></span>

[<span data-ttu-id="07f46-127">GroupBy (形式) の SelectionCondition の TypeName 要素</span><span class="sxs-lookup"><span data-stu-id="07f46-127">TypeName Element for SelectionCondition for GroupBy (Format)</span></span>](./typename-element-for-selectioncondition-for-groupby-format.md)

[<span data-ttu-id="07f46-128">GroupBy (形式) の EntrySelectedBy SelectionCondition 要素</span><span class="sxs-lookup"><span data-stu-id="07f46-128">SelectionCondition Element for EntrySelectedBy for GroupBy (Format)</span></span>](./selectioncondition-element-for-entryselectedby-for-groupby-format.md)

[<span data-ttu-id="07f46-129">データが表示される場合の条件の定義</span><span class="sxs-lookup"><span data-stu-id="07f46-129">Defining Conditions for When Data Is Displayed</span></span>](./defining-conditions-for-displaying-data.md)

[<span data-ttu-id="07f46-130">選択範囲のセットを定義します。</span><span class="sxs-lookup"><span data-stu-id="07f46-130">Defining Selection Sets</span></span>](./defining-selection-sets.md)

[<span data-ttu-id="07f46-131">PowerShell のファイルを書式設定の書き込み</span><span class="sxs-lookup"><span data-stu-id="07f46-131">Writing a PowerShell Formatting File</span></span>](./writing-a-powershell-formatting-file.md)