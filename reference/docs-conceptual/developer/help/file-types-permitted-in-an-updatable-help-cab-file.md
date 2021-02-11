---
ms.date: 03/22/2012
ms.topic: reference
title: 更新可能なヘルプ CAB ファイルで許可されるファイルの種類
description: 更新可能なヘルプ CAB ファイルで許可されるファイルの種類
ms.openlocfilehash: bb69b8b89a9a3a5c8c00059ec404a4d41a5db9a5
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92654740"
---
# <a name="file-types-permitted-in-an-updatable-help-cab-file"></a>更新可能なヘルプ CAB ファイルで許可されるファイルの種類

圧縮されていない CAB ファイルの内容は、既定で 1 GB に制限されています。 この [制限を回避](/powershell/module/Microsoft.PowerShell.Core/Save-Help)するには、ユーザー [は update-help コマンドレットと help](/powershell/module/Microsoft.PowerShell.Core/Update-Help)コマンドレットの **Force** パラメーターを使用する必要があります。

インターネットからダウンロードされたヘルプファイルのセキュリティを確保するために、更新可能なヘルプ CAB ファイルには、以下に示すファイルの種類のみを含めることができます。 Update-help [コマンドレット](/powershell/module/Microsoft.PowerShell.Core/Update-Help) は、ヘルプトピックスキーマに対してすべてのファイルを検証します。 `Update-Help`コマンドレットで、無効なファイルや許可されていない種類のファイルが検出された場合、無効なファイルはインストールされず、ユーザーのコンピューター上の CAB からのファイルのインストールは停止します。

- コマンドレットの XML ベースのヘルプトピックです。
- スクリプトと関数の XML ベースのヘルプトピックです。
- PowerShell プロバイダーの XML ベースのヘルプトピックです。
- トピックの概要などのテキストベースのヘルプトピック。

[Update-help](/powershell/module/Microsoft.PowerShell.Core/Update-Help)は、cab をアンパックするときに cab の内容を確認します。 は `Update-Help` 、更新可能なヘルプ CAB ファイルで準拠していないファイルの種類を検出すると、終了エラーを生成し、操作を停止します。 準拠しているファイルの種類であっても、CAB からはヘルプファイルはインストールされません。
