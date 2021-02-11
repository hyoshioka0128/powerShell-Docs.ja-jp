---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell API サンプル
description: Windows PowerShell API サンプル
ms.openlocfilehash: b6336ae7a2abb98cbbaa69bf6c9455f893db2d94
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657544"
---
# <a name="windows-powershell-api-samples"></a>Windows PowerShell API サンプル

このセクションには、機能を制限する実行空間を作成する方法と、実行空間プールを使用して実行空間を提供するコマンドを非同期で実行する方法を示すサンプルコードが含まれています。 Microsoft Visual Studio を使用してコンソールアプリケーションを作成し、このセクションのトピックからホストアプリケーションにコードをコピーすることができます。

## <a name="in-this-section"></a>このセクションの内容

[PowerShell01 サンプル](./windows-powershell01-sample.md) このサンプルでは、 [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) オブジェクトを使用して、実行空間の機能を制限する方法を示します。 このサンプルの出力では、実行空間の言語モードを制限する方法、コマンドレットをプライベートとしてマークする方法、コマンドレットとプロバイダーを追加および削除する方法、プロキシコマンドを追加する方法などについて説明します。

[PowerShell02 サンプル](./windows-powershell02-sample.md) このサンプルでは、実行空間プールの実行空間を使用して、コマンドを非同期的に実行する方法を示します。 このサンプルでは、コマンドの一覧を生成し、それらのコマンドを実行します。 Windows PowerShell エンジンは、必要に応じて、プールから実行空間を開きます。
