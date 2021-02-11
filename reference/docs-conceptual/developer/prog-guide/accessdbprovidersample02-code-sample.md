---
ms.date: 09/13/2016
ms.topic: reference
title: AccessDbProviderSample02 コード サンプル
description: AccessDbProviderSample02 コード サンプル
ms.openlocfilehash: f467ee59b36027e80852ae1f7b2f1af5c9aa8569
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659390"
---
# <a name="accessdbprovidersample02-code-sample"></a>AccessDbProviderSample02 コード サンプル

次のコードは、「 [Windows Powershell ドライブプロバイダーの作成](./creating-a-windows-powershell-drive-provider.md)」で説明されている windows powershell プロバイダーの実装を示しています。
この実装では、パスを作成し、Access データベースへの接続を確立してから、ドライブを削除します。

> [!NOTE]
> このプロバイダーの C# ソースファイル (AccessDBSampleProvider02.cs) は、Windows Vista 用 Microsoft Windows ソフトウェア開発キットおよび Microsoft .NET Framework 3.0 ランタイムコンポーネントを使用してダウンロードできます。 ダウンロードの手順については、「 [Windows powershell をインストールする方法」および「Windows POWERSHELL SDK をダウンロードする方法](/powershell/scripting/developer/installing-the-windows-powershell-sdk)」を参照してください。
> ダウンロードしたソースファイルは、ディレクトリにあり **\<PowerShell Samples>** ます。 その他の Windows PowerShell プロバイダーの実装の詳細については、「 [Windows Powershell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」を参照してください。

## <a name="code-sample"></a>コード サンプル

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample02/AccessDBProviderSample02.cs" range="11-154":::

## <a name="see-also"></a>参照

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
