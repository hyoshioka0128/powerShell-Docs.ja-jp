---
ms.date: 09/13/2016
ms.topic: reference
title: GetProc01 (C#) サンプル コード
description: GetProc01 (C#) サンプル コード
ms.openlocfilehash: 79509ff12afb32c907058f4ddb0c707f3823857a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92654490"
---
# <a name="getproc01-c-sample-code"></a>GetProc01 (C#) サンプル コード

次のコードは、GetProc01 サンプルコマンドレットの実装を示しています。 このコマンドレットは、実際にプロセスを取得する作業を、 [System. Getprocesses *](/dotnet/api/System.Diagnostics.Process.GetProcesses) メソッドのままにすることによって簡略化されています。

> [!NOTE]
> この Get-Proc コマンドレットの C# ソースファイル (getproc01.cs) をダウンロードするには、Microsoft Windows Software Development Kit for Windows Vista および .NET Framework 3.0 ランタイムコンポーネントを使用します。 ダウンロードの手順については、「 [Windows powershell をインストールする方法」および「Windows POWERSHELL SDK をダウンロードする方法](/powershell/scripting/developer/installing-the-windows-powershell-sdk)」を参照してください。
> ダウンロードしたソースファイルは、ディレクトリにあり **\<PowerShell Samples>** ます。

## <a name="code-sample"></a>コード サンプル

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/GetProcessSample01/GetProcessSample01.cs" range="11-126":::

## <a name="see-also"></a>参照

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
