---
ms.date: 09/13/2016
ms.topic: reference
title: Runspace02 (C#) コード サンプル
description: Runspace02 (C#) コード サンプル
ms.openlocfilehash: 9e2c0cf37d1bf12a92f4fbf781928c0241202915
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647445"
---
# <a name="runspace02-c-code-sample"></a>Runspace02 (C#) コード サンプル

Runspace02 サンプルの C# ソースコードを次に示します。 このサンプルでは、 [Runspaceinvoke](/dotnet/api/System.Management.Automation.RunspaceInvoke) クラスを使用して、 `Get-Process` コマンドレットを同期的に実行します。 次に、Windows フォームとデータバインディングを使用して、DataGridView コントロールに結果を表示します。

## <a name="code-sample"></a>コード サンプル

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/Runspace02/Runspace02.cs" range="11-82":::

## <a name="see-also"></a>参照

[Windows PowerShell SDK](../windows-powershell-reference.md)
