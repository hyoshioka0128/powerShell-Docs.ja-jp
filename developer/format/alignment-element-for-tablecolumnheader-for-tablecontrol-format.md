---
title: TableColumnHeader TableControl (形式) の要素の配置 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: ff85e83a-c9c2-4c37-accc-e6a27c182f3c
caps.latest.revision: 19
ms.openlocfilehash: 16b41535109ca503e679a135f5ba30054e33de5b
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56853758"
---
# <a name="alignment-element-for-tablecolumnheader-for-tablecontrol-format"></a><span data-ttu-id="10c0c-102">TableControl の TableColumnHeader の Alignment 要素 (書式)</span><span class="sxs-lookup"><span data-stu-id="10c0c-102">Alignment Element for TableColumnHeader for TableControl (Format)</span></span>

<span data-ttu-id="10c0c-103">列ヘッダー内のデータを表示する方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="10c0c-103">Defines how the data in a column header is displayed.</span></span>

<span data-ttu-id="10c0c-104">要素 (形式) ViewDefinitions 要素 (形式) 表示要素 (形式) TableControl 要素 (形式) TableHeaders 要素 (形式) TableColumnHeader 要素 (形式) の配置の構成要素 TableColumnHeader (形式)</span><span class="sxs-lookup"><span data-stu-id="10c0c-104">Configuration Element (Format) ViewDefinitions Element (Format) View Element (Format) TableControl Element (Format) TableHeaders Element (Format) TableColumnHeader Element (Format) Alignment Element for TableColumnHeader (Format)</span></span>

## <a name="syntax"></a><span data-ttu-id="10c0c-105">構文</span><span class="sxs-lookup"><span data-stu-id="10c0c-105">Syntax</span></span>

```xml
<Alignment>AlignmentType</Alignment>
```

## <a name="attributes-and-elements"></a><span data-ttu-id="10c0c-106">属性と要素</span><span class="sxs-lookup"><span data-stu-id="10c0c-106">Attributes and Elements</span></span>

<span data-ttu-id="10c0c-107">次のセクションでは、属性、子要素、およびの親要素について説明します、`Alignment`要素。</span><span class="sxs-lookup"><span data-stu-id="10c0c-107">The following sections describe the attributes, child elements, and parent element of the `Alignment` element.</span></span>

### <a name="attributes"></a><span data-ttu-id="10c0c-108">属性</span><span class="sxs-lookup"><span data-stu-id="10c0c-108">Attributes</span></span>

<span data-ttu-id="10c0c-109">なし。</span><span class="sxs-lookup"><span data-stu-id="10c0c-109">None.</span></span>

### <a name="child-elements"></a><span data-ttu-id="10c0c-110">子要素</span><span class="sxs-lookup"><span data-stu-id="10c0c-110">Child Elements</span></span>

<span data-ttu-id="10c0c-111">なし。</span><span class="sxs-lookup"><span data-stu-id="10c0c-111">None.</span></span>

### <a name="parent-elements"></a><span data-ttu-id="10c0c-112">親要素</span><span class="sxs-lookup"><span data-stu-id="10c0c-112">Parent Elements</span></span>

|<span data-ttu-id="10c0c-113">要素</span><span class="sxs-lookup"><span data-stu-id="10c0c-113">Element</span></span>|<span data-ttu-id="10c0c-114">説明</span><span class="sxs-lookup"><span data-stu-id="10c0c-114">Description</span></span>|
|-------------|-----------------|
|[<span data-ttu-id="10c0c-115">TableColumnHeader 要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="10c0c-115">TableColumnHeader Element (Format)</span></span>](./tablecolumnheader-element-format.md)|<span data-ttu-id="10c0c-116">ラベル、幅、およびテーブルの列のデータのアラインメントを定義します。</span><span class="sxs-lookup"><span data-stu-id="10c0c-116">Defines a label, the width, and the alignment of the data for a column of the table.</span></span>|

## <a name="text-value"></a><span data-ttu-id="10c0c-117">テキスト値</span><span class="sxs-lookup"><span data-stu-id="10c0c-117">Text Value</span></span>

<span data-ttu-id="10c0c-118">次の値のいずれかを指定します。</span><span class="sxs-lookup"><span data-stu-id="10c0c-118">Specify one of the following values.</span></span> <span data-ttu-id="10c0c-119">これらの値は区別されません。</span><span class="sxs-lookup"><span data-stu-id="10c0c-119">These values are not case-sensitive.</span></span>

<span data-ttu-id="10c0c-120">この要素が指定されていない場合、既定値は、これは、左寄せで左側の列にデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10c0c-120">Left Aligns the data displayed in the column on the left This is the default if this element is not specified.</span></span>

<span data-ttu-id="10c0c-121">データは右寄せで右側の列に表示されます。</span><span class="sxs-lookup"><span data-stu-id="10c0c-121">Right Aligns the data displayed in the column on the right.</span></span>

<span data-ttu-id="10c0c-122">中央揃えの列に表示されるデータ。</span><span class="sxs-lookup"><span data-stu-id="10c0c-122">Center Centers the data displayed in the column.</span></span>

## <a name="remarks"></a><span data-ttu-id="10c0c-123">コメント</span><span class="sxs-lookup"><span data-stu-id="10c0c-123">Remarks</span></span>

<span data-ttu-id="10c0c-124">テーブル ビューのコンポーネントに関する詳細については、次を参照してください。[テーブル ビューを作成する](./creating-a-table-view.md)します。</span><span class="sxs-lookup"><span data-stu-id="10c0c-124">For more information about the components of a table view, see [Creating a Table View](./creating-a-table-view.md).</span></span>

## <a name="example"></a><span data-ttu-id="10c0c-125">例</span><span class="sxs-lookup"><span data-stu-id="10c0c-125">Example</span></span>

<span data-ttu-id="10c0c-126">この例では、`TableColumnHeader`要素のデータは、左側に揃えて配置します。</span><span class="sxs-lookup"><span data-stu-id="10c0c-126">This example shows a `TableColumnHeader` element whose data is aligned on the left.</span></span>

```xml
<TableColumnHeader>
  <Label>Column 1</Label)
  <Width>16</Width>
  <Alignment>Left</Alignment>
</TableColumnHeader>
```

## <a name="see-also"></a><span data-ttu-id="10c0c-127">参照</span><span class="sxs-lookup"><span data-stu-id="10c0c-127">See Also</span></span>

[<span data-ttu-id="10c0c-128">テーブル ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="10c0c-128">Creating a Table View</span></span>](./creating-a-table-view.md)

[<span data-ttu-id="10c0c-129">TableColumnHeader 要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="10c0c-129">TableColumnHeader Element (Format)</span></span>](./tablecolumnheader-element-format.md)

[<span data-ttu-id="10c0c-130">PowerShell のファイルを書式設定の書き込み</span><span class="sxs-lookup"><span data-stu-id="10c0c-130">Writing a PowerShell Formatting File</span></span>](./writing-a-powershell-formatting-file.md)