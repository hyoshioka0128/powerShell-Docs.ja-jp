---
ms.date: 09/13/2016
ms.topic: reference
title: AccessDbProviderSample04 コード サンプル
description: AccessDbProviderSample04 コード サンプル
ms.openlocfilehash: bb70ce9f1b1c94349c354a8771fedf7fcb1bb320
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647571"
---
# <a name="accessdbprovidersample04-code-sample"></a>AccessDbProviderSample04 コード サンプル

次のコードは、「 [Windows Powershell コンテナープロバイダーの作成](./creating-a-windows-powershell-container-provider.md)」で説明されている windows powershell プロバイダーの実装を示しています。
このプロバイダーは、マルチレイヤーデータストアに対して機能します。 この種類のデータストアの場合、ストアの最上位レベルにはルート項目が含まれ、それ以降の各レベルは子項目のノードと呼ばれます。 ユーザーがこれらの子ノードを操作できるようにすることで、ユーザーはデータストアを介して階層的に対話できます。

## <a name="code-sample"></a>コード サンプル

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="11-1635":::

## <a name="see-also"></a>参照

[Windows PowerShell SDK](../windows-powershell-reference.md)
