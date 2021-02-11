---
ms.date: 09/12/2016
ms.topic: reference
title: 更新可能なヘルプをテストする方法
description: 更新可能なヘルプをテストする方法
ms.openlocfilehash: 47873089bfa1b918ea9970915e829a22aa7254c5
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667625"
---
# <a name="how-to-test-updatable-help"></a>更新可能なヘルプをテストする方法

このトピックでは、モジュールの更新可能なヘルプをテストする方法について説明します。

## <a name="using-verbose-to-detect-errors"></a>Verbose を使用してエラーを検出する

モジュールの HelpInfo XML ファイルと CAB ファイルをアップロードしたら、 **Verbose** パラメーターを指定して [update-help](/powershell/module/Microsoft.PowerShell.Core/Update-Help)コマンドを実行し、ファイルをテストします。 **Verbose** パラメーターは、 `Update-Help` アクションの重要なステップを報告するように指示します。これには、モジュールマニフェストの **helpinfouri** キーを読み取って、展開された CAB ファイル内のファイルの種類を検証し、それらのファイルを言語固有のモジュールディレクトリに配置します。

すべての詳細メッセージを解決したら、 `Update-Help` **Debug** パラメーターを指定してコマンドを実行します。
このパラメーターは、更新可能なヘルプファイルに関する残りの問題を検出する必要があります。
