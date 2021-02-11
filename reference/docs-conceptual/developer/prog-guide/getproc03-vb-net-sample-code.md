---
ms.date: 09/12/2016
ms.topic: reference
title: GetProc03 (VB.NET) サンプル コード
description: GetProc03 (VB.NET) サンプル コード
ms.openlocfilehash: fc70496178c85e101b380e68aacd64c8d1f350e9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355563"
---
# <a name="getproc03-vbnet-sample-code"></a>GetProc03 (VB.NET) サンプル コード

次のコードは、 `Get-Process` パイプライン入力を受け入れることができるコマンドレットの実装を示しています。 この実装は `Name` 、パイプライン入力を受け取り、指定された名前に基づいてローカルコンピューターからプロセス情報を取得し、パイプラインにオブジェクトを送信するための出力機構として [WriteObject (system.object, system.string)](/dotnet/api/system.management.automation.cmdlet.writeobject#System_Management_Automation_Cmdlet_WriteObject_System_Object_System_Boolean_) メソッドを使用するパラメーターを定義します。

## <a name="code-sample"></a>コード サンプル

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplesgetproc03#getproc03vbAll](Msh_samplesgetproc03#getproc03vbAll)]  -->
