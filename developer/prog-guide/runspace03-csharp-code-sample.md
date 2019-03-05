---
title: RunSpace03 (C#) コード サンプル |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 9ac8ab99-1856-4d6f-b30d-c0a18b8dd1fc
caps.latest.revision: 6
ms.openlocfilehash: d2e5b8c0a7622481bfca21d5c5b46afa01c02d2c
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56863528"
---
# <a name="runspace03-c-code-sample"></a><span data-ttu-id="00df8-102">RunSpace03 (C#) コード サンプル</span><span class="sxs-lookup"><span data-stu-id="00df8-102">RunSpace03 (C#) Code Sample</span></span>

<span data-ttu-id="00df8-103">ここでは、C#で説明されているコンソール アプリケーションのコードをソース[コンソール アプリケーションを実行に指定されたスクリプトを作成する](http://msdn.microsoft.com/en-us/a93e6006-36db-4bcc-b9da-c5bebf4ffd68)します。</span><span class="sxs-lookup"><span data-stu-id="00df8-103">Here is the C# source code for the console application described in [Creating a Console Application That Runs a Specified Script](http://msdn.microsoft.com/en-us/a93e6006-36db-4bcc-b9da-c5bebf4ffd68).</span></span> <span data-ttu-id="00df8-104">このサンプルでは、 [System.Management.Automation.Runspaceinvoke](/dotnet/api/System.Management.Automation.RunspaceInvoke)クラスを取得しますが、スクリプトに渡されるプロセス名のリストを使用して情報を処理するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="00df8-104">This sample uses the [System.Management.Automation.Runspaceinvoke](/dotnet/api/System.Management.Automation.RunspaceInvoke) class to execute a script that retrieves process information by using the list of process names passed into the script.</span></span> <span data-ttu-id="00df8-105">入力オブジェクトをスクリプトに渡す方法と、エラー オブジェクトと出力オブジェクトを取得する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="00df8-105">It shows how to pass input objects to a script and how to retrieve error objects as well as the output objects.</span></span>

> [!NOTE]
> <span data-ttu-id="00df8-106">ダウンロードすることができます、 C# Microsoft Windows ソフトウェア開発キットの Windows Vista と Microsoft .NET Framework 3.0 ランタイム コンポーネントを使用してこのサンプルのソース ファイル (runspace03.cs)。</span><span class="sxs-lookup"><span data-stu-id="00df8-106">You can download the C# source file (runspace03.cs) for this sample using the Microsoft Windows Software Development Kit for Windows Vista and Microsoft .NET Framework 3.0 Runtime Components.</span></span> <span data-ttu-id="00df8-107">ダウンロードの手順については、次を参照してください。 [Windows PowerShell のインストールと、Windows PowerShell SDK をダウンロードする方法](/powershell/developer/installing-the-windows-powershell-sdk)します。</span><span class="sxs-lookup"><span data-stu-id="00df8-107">For download instructions, see [How to Install Windows PowerShell and Download the Windows PowerShell SDK](/powershell/developer/installing-the-windows-powershell-sdk).</span></span>
> <span data-ttu-id="00df8-108">ダウンロードすることができます、 C# Microsoft Windows ソフトウェア開発キットの Windows Vista と Microsoft .NET Framework 3.0 ランタイム コンポーネントを使用してこのサンプルのソース ファイル (runspace03.cs)。</span><span class="sxs-lookup"><span data-stu-id="00df8-108">You can download the C# source file (runspace03.cs) for this sample using the Microsoft Windows Software Development Kit for Windows Vista and Microsoft .NET Framework 3.0 Runtime Components.</span></span> <span data-ttu-id="00df8-109">ダウンロードの手順については、次を参照してください。 [Windows PowerShell のインストールと、Windows PowerShell SDK をダウンロードする方法](/powershell/developer/installing-the-windows-powershell-sdk)します。</span><span class="sxs-lookup"><span data-stu-id="00df8-109">For download instructions, see [How to Install Windows PowerShell and Download the Windows PowerShell SDK](/powershell/developer/installing-the-windows-powershell-sdk).</span></span>
>
> <span data-ttu-id="00df8-110">ダウンロードしたソース ファイルは、  **\<PowerShell のサンプル >** ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="00df8-110">The downloaded source files are available in the **\<PowerShell Samples>** directory.</span></span>

## <a name="code-sample"></a><span data-ttu-id="00df8-111">コード サンプル</span><span class="sxs-lookup"><span data-stu-id="00df8-111">Code Sample</span></span>

[!code-csharp[Runspace03.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace03/Runspace03.cs#L11-L88 "Runspace03.cs")]

## <a name="see-also"></a><span data-ttu-id="00df8-112">参照</span><span class="sxs-lookup"><span data-stu-id="00df8-112">See Also</span></span>

[<span data-ttu-id="00df8-113">Windows PowerShell プログラマー ガイド</span><span class="sxs-lookup"><span data-stu-id="00df8-113">Windows PowerShell Programmer's Guide</span></span>](./windows-powershell-programmer-s-guide.md)

[<span data-ttu-id="00df8-114">Windows PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="00df8-114">Windows PowerShell SDK</span></span>](../windows-powershell-reference.md)