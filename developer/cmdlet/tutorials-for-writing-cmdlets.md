---
title: コマンドレットを作成するためのチュートリアル |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: d0b548aa-febf-45dd-bf71-2077730b9b73
caps.latest.revision: 6
ms.openlocfilehash: 767b392bd1603e83d80bad5b3fd9cb42ff142ce6
ms.sourcegitcommit: 10c347a8c3dcbf8962295601834f5ba85342a87b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "56863608"
---
# <a name="tutorials-for-writing-cmdlets"></a><span data-ttu-id="03b1a-102">コマンドレットの記述に関するチュートリアル</span><span class="sxs-lookup"><span data-stu-id="03b1a-102">Tutorials for Writing Cmdlets</span></span>

<span data-ttu-id="03b1a-103">このセクションには、コマンドレットの記述に関するチュートリアルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="03b1a-103">This section contains tutorials for writing cmdlets.</span></span> <span data-ttu-id="03b1a-104">これらのチュートリアルには、コマンドレットと、コードが必要な理由の説明を記述するために必要なコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="03b1a-104">These tutorials include the code needed to write the cmdlets, plus an explanation of why the code is needed.</span></span> <span data-ttu-id="03b1a-105">これらのトピックは、コマンドレットの記述を使い始めたばかりの方のため非常に便利になります。</span><span class="sxs-lookup"><span data-stu-id="03b1a-105">These topics will be very helpful for those who are just starting to write cmdlets.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="03b1a-106">ユーザーにとって以下の説明とコード例について、次を参照してください。[コマンドレット サンプル](./cmdlet-samples.md)します。</span><span class="sxs-lookup"><span data-stu-id="03b1a-106">For those who want code examples with less description, see [Cmdlet Samples](./cmdlet-samples.md).</span></span>

## <a name="in-this-section"></a><span data-ttu-id="03b1a-107">このセクションの内容</span><span class="sxs-lookup"><span data-stu-id="03b1a-107">In This Section</span></span>

<span data-ttu-id="03b1a-108">[GetProc チュートリアル](./getproc-tutorial.md)-このチュートリアルは、コマンドレット クラスを定義し、パラメーターを追加して、エラーの報告などの基本的な機能を追加する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="03b1a-108">[GetProc Tutorial](./getproc-tutorial.md) - This tutorial describes how to define a cmdlet class and add basic functionality such as adding parameters and reporting errors.</span></span> <span data-ttu-id="03b1a-109">このチュートリアルで説明されているコマンドレットとよく似ていますが、 [Get-process](/powershell/module/Microsoft.PowerShell.Management/Get-Process)コマンドレットの Windows PowerShell によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="03b1a-109">The cmdlet described in this tutorial is very similar to the [Get-Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) cmdlet provided by Windows PowerShell.</span></span>

<span data-ttu-id="03b1a-110">[StopProc チュートリアル](./stopproc-tutorial.md)- このチュートリアルは、コマンドレットを定義し、ユーザー プロンプト、ワイルドカードのサポートなどの機能を追加する方法を説明し、パラメーターの使用を設定します。</span><span class="sxs-lookup"><span data-stu-id="03b1a-110">[StopProc Tutorial](./stopproc-tutorial.md) - This tutorial describes how to define a cmdlet and add functionality such as user prompts, wildcard support, and the use of parameter sets.</span></span> <span data-ttu-id="03b1a-111">ここで説明するコマンドレットと同じタスクを実行する、 [Stop-process](/powershell/module/Microsoft.PowerShell.Management/Stop-Process)コマンドレットの Windows PowerShell によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="03b1a-111">The cmdlet described here performs the same task as the [Stop-Process](/powershell/module/Microsoft.PowerShell.Management/Stop-Process) cmdlet provided by Windows PowerShell.</span></span>

<span data-ttu-id="03b1a-112">[SelectStr チュートリアル](./selectstr-tutorial.md)-このチュートリアルは、データ ストアにアクセスするためのコマンドレットを定義する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="03b1a-112">[SelectStr Tutorial](./selectstr-tutorial.md) - This tutorial describes how to define a cmdlet that accesses a data store.</span></span> <span data-ttu-id="03b1a-113">ここで説明するコマンドレットと同じタスクを実行する、 [Select-string](/powershell/module/microsoft.powershell.utility/select-string)コマンドレットの Windows PowerShell によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="03b1a-113">The cmdlet described here performs the same task as the [Select-String](/powershell/module/microsoft.powershell.utility/select-string) cmdlet provided by Windows PowerShell.</span></span>

## <a name="see-also"></a><span data-ttu-id="03b1a-114">参照</span><span class="sxs-lookup"><span data-stu-id="03b1a-114">See Also</span></span>

[<span data-ttu-id="03b1a-115">GetProc チュートリアル</span><span class="sxs-lookup"><span data-stu-id="03b1a-115">GetProc Tutorial</span></span>](./getproc-tutorial.md)

[<span data-ttu-id="03b1a-116">StopProc チュートリアル</span><span class="sxs-lookup"><span data-stu-id="03b1a-116">StopProc Tutorial</span></span>](./stopproc-tutorial.md)

[<span data-ttu-id="03b1a-117">SelectStr チュートリアル</span><span class="sxs-lookup"><span data-stu-id="03b1a-117">SelectStr Tutorial</span></span>](./selectstr-tutorial.md)

[<span data-ttu-id="03b1a-118">Windows PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="03b1a-118">Windows PowerShell SDK</span></span>](../windows-powershell-reference.md)