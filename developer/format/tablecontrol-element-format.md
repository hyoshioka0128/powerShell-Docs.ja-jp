---
title: TableControl 要素 (式) |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 1550b068-dfbc-4ae0-9aa1-72c9a680ec59
caps.latest.revision: 15
ms.openlocfilehash: dd48e82452e83f93e2e005c9db53bbc51d8b2eb4
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56858918"
---
# <a name="tablecontrol-element-format"></a><span data-ttu-id="e7717-102">TableControl 要素 (書式)</span><span class="sxs-lookup"><span data-stu-id="e7717-102">TableControl Element (Format)</span></span>

<span data-ttu-id="e7717-103">ビューのテーブル形式を定義します。</span><span class="sxs-lookup"><span data-stu-id="e7717-103">Defines a table format for a view.</span></span>

<span data-ttu-id="e7717-104">ViewDefinitions 要素 (形式) 表示要素 (形式) TableControl 要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="e7717-104">ViewDefinitions Element (Format) View Element (Format) TableControl Element (Format)</span></span>

## <a name="syntax"></a><span data-ttu-id="e7717-105">構文</span><span class="sxs-lookup"><span data-stu-id="e7717-105">Syntax</span></span>

```xml
<TableControl>
  <AutoSize/>
  <HideTableHeaders/>
  <TableHeaders>...</TableHeaders>
  <TableRowEntries>...</TableRowEntries>
</TableControl>

```

## <a name="attributes-and-elements"></a><span data-ttu-id="e7717-106">属性と要素</span><span class="sxs-lookup"><span data-stu-id="e7717-106">Attributes and Elements</span></span>

<span data-ttu-id="e7717-107">次のセクションでは、属性、子要素、およびの親要素について説明します、`TableControl`要素。</span><span class="sxs-lookup"><span data-stu-id="e7717-107">The following sections describe attributes, child elements, and parent element of the `TableControl` element.</span></span> <span data-ttu-id="e7717-108">テーブルの行を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e7717-108">You must specify the rows of the table.</span></span> <span data-ttu-id="e7717-109">その他のすべての子要素は省略可能です。</span><span class="sxs-lookup"><span data-stu-id="e7717-109">All other child elements are optional.</span></span>

### <a name="attributes"></a><span data-ttu-id="e7717-110">属性</span><span class="sxs-lookup"><span data-stu-id="e7717-110">Attributes</span></span>

<span data-ttu-id="e7717-111">なし。</span><span class="sxs-lookup"><span data-stu-id="e7717-111">None.</span></span>

### <a name="child-elements"></a><span data-ttu-id="e7717-112">子要素</span><span class="sxs-lookup"><span data-stu-id="e7717-112">Child Elements</span></span>

|<span data-ttu-id="e7717-113">要素</span><span class="sxs-lookup"><span data-stu-id="e7717-113">Element</span></span>|<span data-ttu-id="e7717-114">説明</span><span class="sxs-lookup"><span data-stu-id="e7717-114">Description</span></span>|
|-------------|-----------------|
|[<span data-ttu-id="e7717-115">TableControl (形式) の AutoSize 要素</span><span class="sxs-lookup"><span data-stu-id="e7717-115">AutoSize Element for TableControl (Format)</span></span>](./autosize-element-for-tablecontrol-format.md)|<span data-ttu-id="e7717-116">省略可能な要素です。</span><span class="sxs-lookup"><span data-stu-id="e7717-116">Optional element.</span></span><br /><br /> <span data-ttu-id="e7717-117">かどうか、列のサイズと列の数が調整のデータのサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="e7717-117">Specifies whether the column size and the number of columns are adjusted based on the size of the data.</span></span>|
|[<span data-ttu-id="e7717-118">TableControl (形式) の HideTableHeaders 要素</span><span class="sxs-lookup"><span data-stu-id="e7717-118">HideTableHeaders Element for TableControl (Format)</span></span>](./hidetableheaders-element-format.md)|<span data-ttu-id="e7717-119">省略可能な要素です。</span><span class="sxs-lookup"><span data-stu-id="e7717-119">Optional element.</span></span><br /><br /> <span data-ttu-id="e7717-120">テーブルのヘッダーは表示されないかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="e7717-120">Indicates whether the header of the table is not displayed.</span></span>|
|[<span data-ttu-id="e7717-121">TableControl (形式) の TableHeaders 要素</span><span class="sxs-lookup"><span data-stu-id="e7717-121">TableHeaders Element for TableControl (Format)</span></span>](./tableheaders-element-format.md)|<span data-ttu-id="e7717-122">必須の要素。</span><span class="sxs-lookup"><span data-stu-id="e7717-122">Required element.</span></span><br /><br /> <span data-ttu-id="e7717-123">ラベル、幅、およびテーブル ビューの列のデータの配置を定義します。</span><span class="sxs-lookup"><span data-stu-id="e7717-123">Defines the labels, the widths, and the alignment of the data for the columns of the table view.</span></span>|
|[<span data-ttu-id="e7717-124">TableCotrol (形式) の TableRowEntries 要素</span><span class="sxs-lookup"><span data-stu-id="e7717-124">TableRowEntries Element for TableCotrol (Format)</span></span>](./tablerowentries-element-for-tablecontrol-format.md)|<span data-ttu-id="e7717-125">省略可能な要素です。</span><span class="sxs-lookup"><span data-stu-id="e7717-125">Optional element.</span></span><br /><br /> <span data-ttu-id="e7717-126">テーブル ビューの定義を提供します。</span><span class="sxs-lookup"><span data-stu-id="e7717-126">Provides the definitions of the table view.</span></span>|

### <a name="parent-elements"></a><span data-ttu-id="e7717-127">親要素</span><span class="sxs-lookup"><span data-stu-id="e7717-127">Parent Elements</span></span>

|<span data-ttu-id="e7717-128">要素</span><span class="sxs-lookup"><span data-stu-id="e7717-128">Element</span></span>|<span data-ttu-id="e7717-129">説明</span><span class="sxs-lookup"><span data-stu-id="e7717-129">Description</span></span>|
|-------------|-----------------|
|[<span data-ttu-id="e7717-130">ビュー要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="e7717-130">View Element (Format)</span></span>](./view-element-format.md)|<span data-ttu-id="e7717-131">1 つまたは複数のオブジェクトのメンバーを表示するために使用するビューを定義します。</span><span class="sxs-lookup"><span data-stu-id="e7717-131">Defines a view that is used to display the members of one or more objects.</span></span>|

## <a name="remarks"></a><span data-ttu-id="e7717-132">コメント</span><span class="sxs-lookup"><span data-stu-id="e7717-132">Remarks</span></span>

<span data-ttu-id="e7717-133">テーブル ビューのコンポーネントに関する詳細については、次を参照してください。[テーブル ビューを作成する](./creating-a-table-view.md)します。</span><span class="sxs-lookup"><span data-stu-id="e7717-133">For more information about the components of a table view, see [Creating a Table View](./creating-a-table-view.md).</span></span>

## <a name="example"></a><span data-ttu-id="e7717-134">例</span><span class="sxs-lookup"><span data-stu-id="e7717-134">Example</span></span>

<span data-ttu-id="e7717-135">この例では、`TableControl`要素のプロパティを表示するために使用される、 [System.Serviceprocess.Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e7717-135">This example shows a `TableControl` element that is used to display the properties of the [System.Serviceprocess.Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) object.</span></span>

```xml
<View>
  <Name>service</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <TableControl>
    <TableHeaders>...</TableHeaders>
    <TableRowEntries>...</TableRowEntries>
  </TableControl>
</View>

```

## <a name="see-also"></a><span data-ttu-id="e7717-136">参照</span><span class="sxs-lookup"><span data-stu-id="e7717-136">See Also</span></span>

[<span data-ttu-id="e7717-137">テーブル ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e7717-137">Creating a Table View</span></span>](./creating-a-table-view.md)

[<span data-ttu-id="e7717-138">ビュー要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="e7717-138">View Element (Format)</span></span>](./view-element-format.md)

[<span data-ttu-id="e7717-139">TableControl (形式) の AutoSize 要素</span><span class="sxs-lookup"><span data-stu-id="e7717-139">AutoSize Element for TableControl (Format)</span></span>](./autosize-element-for-tablecontrol-format.md)

[<span data-ttu-id="e7717-140">HideTableHeaders 要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="e7717-140">HideTableHeaders Element (Format)</span></span>](./hidetableheaders-element-format.md)

[<span data-ttu-id="e7717-141">TableHeaders 要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="e7717-141">TableHeaders Element (Format)</span></span>](./tableheaders-element-format.md)

[<span data-ttu-id="e7717-142">TableRowEntries 要素 (形式)</span><span class="sxs-lookup"><span data-stu-id="e7717-142">TableRowEntries Element (Format)</span></span>](./tablerowentries-element-for-tablecontrol-format.md)

[<span data-ttu-id="e7717-143">PowerShell のファイルを書式設定の書き込み</span><span class="sxs-lookup"><span data-stu-id="e7717-143">Writing a PowerShell Formatting File</span></span>](./writing-a-powershell-formatting-file.md)