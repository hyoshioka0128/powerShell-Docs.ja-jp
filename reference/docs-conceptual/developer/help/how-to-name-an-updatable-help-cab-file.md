---
ms.date: 09/12/2016
ms.topic: reference
title: 更新可能なヘルプ CAB ファイルに名前を付ける方法
description: 更新可能なヘルプ CAB ファイルに名前を付ける方法
ms.openlocfilehash: 57ea188d07a382d1a986a49c9ae22c5919dafa8e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92658921"
---
# <a name="how-to-name-an-updatable-help-cab-file"></a>更新可能なヘルプ CAB ファイルに名前を付ける方法

このトピックでは、更新可能なヘルプキャビネット () ファイルに必要な名前の形式について説明し `.CAB` ます。

## <a name="how-to-name-an-updatable-help-cab-file"></a>更新可能なヘルプ CAB ファイルに名前を付ける方法

更新可能なキャビネット () ファイルには、 `.CAB` 次の形式の名前を付ける必要があります。

`<ModuleName>_<ModuleGUID>_<UICulture>_HelpContent.cab`

名前の要素は次のとおりです。

- `<ModuleName>`-[モジュール](/powershell/module/Microsoft.PowerShell.Core/Get-Module)コマンドレットが返す **Moduleinfo** オブジェクトの **Name** プロパティの値。
- `<ModuleGUID>` -モジュールマニフェスト内の **GUID** キーの値。
- `<UICulture>` -CAB ファイル内のヘルプファイルの UI カルチャ。 この値は、モジュールの HelpInfo XML ファイル内のいずれかの **UICulture** 要素の値と一致する必要があります。

たとえば、モジュール名が "TestModule" で、モジュール GUID が 9cabb9ad-4914-a46b-bfc1bebf07f9 で、UI カルチャが "en-us" の場合、CAB ファイルの名前は次のようになります。

`TestModule_9cabb9ad-f2ac-4914-a46b-bfc1bebf07f9_en-US_HelpContent.cab`
