---
title: GetProc01 (VB.NET) サンプル コード |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 3ee87f67-6d2c-48cc-b300-3ae917c6dc88
caps.latest.revision: 5
ms.openlocfilehash: 1f11c89f60aef44905cbce3fe669def26119d12e
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56856068"
---
# <a name="getproc01-vbnet-sample-code"></a><span data-ttu-id="dab11-102">GetProc01 (VB.NET) サンプル コード</span><span class="sxs-lookup"><span data-stu-id="dab11-102">GetProc01 (VB.NET) Sample Code</span></span>

<span data-ttu-id="dab11-103">次のコードでは、GetProc01 サンプル コマンドレットの実装を示します。</span><span class="sxs-lookup"><span data-stu-id="dab11-103">The following code shows the implementation of the GetProc01 sample cmdlet.</span></span> <span data-ttu-id="dab11-104">コマンドレットが取得されるプロセスの実際の作業にすることで簡略化されたことに注意してください、 [System.Diagnostics.Process.Getprocesses\*](/dotnet/api/System.Diagnostics.Process.GetProcesses)メソッド。</span><span class="sxs-lookup"><span data-stu-id="dab11-104">Notice that the cmdlet is simplified by leaving the actual work of process retrieval to the [System.Diagnostics.Process.Getprocesses\*](/dotnet/api/System.Diagnostics.Process.GetProcesses) method.</span></span>

> [!NOTE]
> <span data-ttu-id="dab11-105">ダウンロードすることができます、C#この Get-proc コマンドレットは、Microsoft Windows ソフトウェア開発キットの Windows Vista と .NET Framework 3.0 ランタイム コンポーネントを使用してソース ファイル (getproc01.cs)。</span><span class="sxs-lookup"><span data-stu-id="dab11-105">You can download the C# source file (getproc01.cs) for this Get-Proc cmdlet using the Microsoft Windows Software Development Kit for Windows Vista and .NET Framework 3.0 Runtime Components.</span></span> <span data-ttu-id="dab11-106">ダウンロードの手順については、次を参照してください。 [Windows PowerShell のインストールと、Windows PowerShell SDK をダウンロードする方法](/powershell/developer/installing-the-windows-powershell-sdk)します。</span><span class="sxs-lookup"><span data-stu-id="dab11-106">For download instructions, see [How to Install Windows PowerShell and Download the Windows PowerShell SDK](/powershell/developer/installing-the-windows-powershell-sdk).</span></span>
> <span data-ttu-id="dab11-107">ダウンロードすることができます、C#この Get-proc コマンドレットは、Microsoft Windows ソフトウェア開発キットの Windows Vista と .NET Framework 3.0 ランタイム コンポーネントを使用してソース ファイル (getproc01.cs)。</span><span class="sxs-lookup"><span data-stu-id="dab11-107">You can download the C# source file (getproc01.cs) for this Get-Proc cmdlet using the Microsoft Windows Software Development Kit for Windows Vista and .NET Framework 3.0 Runtime Components.</span></span> <span data-ttu-id="dab11-108">ダウンロードの手順については、次を参照してください。 [Windows PowerShell のインストールと、Windows PowerShell SDK をダウンロードする方法](/powershell/developer/installing-the-windows-powershell-sdk)します。</span><span class="sxs-lookup"><span data-stu-id="dab11-108">For download instructions, see [How to Install Windows PowerShell and Download the Windows PowerShell SDK](/powershell/developer/installing-the-windows-powershell-sdk).</span></span>
>
> <span data-ttu-id="dab11-109">ダウンロードしたソース ファイルは、  **\<PowerShell のサンプル >** ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="dab11-109">The downloaded source files are available in the **\<PowerShell Samples>** directory.</span></span>

## <a name="code-sample"></a><span data-ttu-id="dab11-110">コード サンプル</span><span class="sxs-lookup"><span data-stu-id="dab11-110">Code Sample</span></span>

<!-- TODO!!!: review snippet reference  [!CODE [msh_samplesgetproc01#getproc01vball](msh_samplesgetproc01#getproc01vball)]  -->

## <a name="see-also"></a><span data-ttu-id="dab11-111">参照</span><span class="sxs-lookup"><span data-stu-id="dab11-111">See Also</span></span>

[<span data-ttu-id="dab11-112">Windows PowerShell プログラマー ガイド</span><span class="sxs-lookup"><span data-stu-id="dab11-112">Windows PowerShell Programmer's Guide</span></span>](./windows-powershell-programmer-s-guide.md)

[<span data-ttu-id="dab11-113">Windows PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="dab11-113">Windows PowerShell SDK</span></span>](../windows-powershell-reference.md)