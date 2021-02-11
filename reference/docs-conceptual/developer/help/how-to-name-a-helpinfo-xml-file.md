---
ms.date: 09/12/2016
ms.topic: reference
title: HelpInfo XML ファイルに名前を付ける方法
description: HelpInfo XML ファイルに名前を付ける方法
ms.openlocfilehash: 55bc2ef9530fc457e4d9ddf18e79e7226c991663
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659038"
---
# <a name="how-to-name-a-helpinfo-xml-file"></a>HelpInfo XML ファイルに名前を付ける方法

このトピックでは、HelpInfo XML ファイルと呼ばれる、更新可能なヘルプ情報ファイルに必要な名前の形式について説明します。

## <a name="how-to-name-a-helpinfo-xml-file"></a>HelpInfo XML ファイルに名前を付ける方法

HelpInfo XML ファイルには、次の形式の名前を付ける必要があります。

`<ModuleName>_<ModuleGUID>_HelpInfo.xml`

名前の要素は次のとおりです。

- `<ModuleName>`-[モジュール](/powershell/module/Microsoft.PowerShell.Core/Get-Module)コマンドレットが返す **Moduleinfo** オブジェクトの **Name** プロパティの値。

- `<ModuleGUID>` -モジュールマニフェスト内の **GUID** キーの値。

たとえば、モジュール名が "TestModule" で、モジュール GUID が 9cabb9ad-f2ac-4914-a46b-bfc1bebf07f9 の場合、モジュールの HelpInfo XML ファイルの名前は次のようになります。

`TestModule_9cabb9ad-f2ac-4914-a46b-bfc1bebf07f9_HelpInfo.xml`
