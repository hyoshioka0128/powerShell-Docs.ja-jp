---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell プロバイダーを作成する方法
description: Windows PowerShell プロバイダーを作成する方法
ms.openlocfilehash: 51f19bf0dfa5f976a5045ae1342730c8f22f695e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647476"
---
# <a name="how-to-create-a-windows-powershell-provider"></a>Windows PowerShell プロバイダーを作成する方法

このセクションでは、Windows PowerShell プロバイダーを構築する方法について説明します。 Windows PowerShell プロバイダーは、次の2つの方法で考慮できます。 プロバイダーは、ユーザーに対して格納されているデータのセットを表します。 たとえば、格納されているデータには、インターネットインフォメーションサービス (IIS) メタベース、Microsoft Windows レジストリ、Windows ファイルシステム、Active Directory、および Windows PowerShell によって格納される変数および別名データを使用できます。

開発者にとって、Windows PowerShell プロバイダーは、ユーザーとユーザーがアクセスする必要があるデータの間のインターフェイスです。 この観点からは、このセクションで説明する各種類のプロバイダーは、Windows PowerShell ランタイムが特定のコマンドレットを共通の方法でユーザーに公開できるようにする、特定の基本クラスとインターフェイスのセットをサポートしています。

## <a name="providers-provided-by-windows-powershell"></a>Windows PowerShell によって提供されるプロバイダー

Windows PowerShell には、既知のデータストアへのアクセスに使用されるいくつかのプロバイダー (FileSystem プロバイダー、レジストリプロバイダー、別名プロバイダーなど) が用意されています。 Windows PowerShell によって提供されるプロバイダーの詳細については、次のコマンドを使用して、オンラインヘルプにアクセスしてください。

**PS>get-help about_providers**

## <a name="accessing-the-stored-data-using-windows-powershell-paths"></a>Windows PowerShell パスを使用した格納されたデータへのアクセス

Windows powershell プロバイダーは、windows powershell ランタイムにアクセスしたり、Windows PowerShell パスを使用してプログラムでコマンドを実行したりすることができます。 ほとんどの場合、これらのパスは、プロバイダーを介してデータに直接アクセスするために使用されます。 ただし、一部のパスは、コマンドレットが Windows PowerShell 以外のアプリケーションプログラミングインターフェイス (Api) を使用してデータにアクセスできるようにする、プロバイダー内部パスに解決される場合があります。 Windows powershell プロバイダーが Windows PowerShell 内で動作する方法の詳細については、「 [Windows powershell のしくみ](/previous-versions/ms714658(v=vs.85))」を参照してください。

## <a name="exposing-provider-cmdlets-using-windows-powershell-drives"></a>Windows PowerShell ドライブを使用したプロバイダーコマンドレットの公開

Windows PowerShell プロバイダーは、仮想 Windows PowerShell ドライブを使用して、サポートされているコマンドレットを公開します。
Windows PowerShell では、Windows PowerShell ドライブに対して次の規則が適用されます。

- ドライブ名は、任意の英数字のシーケンスにすることができます。
- ドライブは、"ルート" と呼ばれるパス上の任意の有効なポイントで指定できます。
- ドライブは、ファイルシステムだけでなく、格納されているデータに対しても実装できます。
- 各ドライブは、現在の作業場所を保持するので、ユーザーはドライブ間を移動するときにコンテキストを保持することができます。

## <a name="in-this-section"></a>このセクションの内容

次の表に、相互に構築されるコード例を含むトピックを示します。 2番目のトピックでは、Windows powershell ランタイムによって基本的な Windows PowerShell プロバイダーを初期化し、初期化することができます。次のトピックでは、データにアクセスするための機能を追加します。次のトピックでは、データを操作するための機能 (格納されたデータの項目) などを追加します。

|                                                    トピック                                                    |                                                                                         定義                                                                                          |
| ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Windows PowerShell プロバイダーを設計する](./designing-your-windows-powershell-provider.md)               | このトピックでは、Windows PowerShell プロバイダーを実装する前に考慮する必要がある事項について説明します。 使用される Windows PowerShell プロバイダーの基本クラスとインターフェイスの概要を示します。 |
| [基本的な Windows PowerShell プロバイダーを作成する](./creating-a-basic-windows-powershell-provider.md)           | このトピックでは、Windows powershell ランタイムがプロバイダーを初期化および初期化解除できるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                        |
| [Windows PowerShell ドライブ プロバイダーを作成する](./creating-a-windows-powershell-drive-provider.md)           | このトピックでは、windows PowerShell ドライブを介してユーザーがデータストアにアクセスできるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                                |
| [Windows PowerShell アイテム プロバイダーを作成する](./creating-a-windows-powershell-item-provider.md)             | このトピックでは、ユーザーがデータストア内の項目を操作できるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                                                  |
| [Windows PowerShell コンテナー プロバイダーを作成する](./creating-a-windows-powershell-container-provider.md)   | このトピックでは、ユーザーがマルチレイヤーデータストアを操作できるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                                                        |
| [Windows PowerShell ナビゲーション プロバイダーを作成する](./creating-a-windows-powershell-navigation-provider.md) | このトピックでは、ユーザーがデータストアの項目を階層構造で移動できるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                           |
| [Windows PowerShell コンテンツ プロバイダーを作成する](./creating-a-windows-powershell-content-provider.md)       | このトピックでは、ユーザーがデータストア内の項目の内容を操作できるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                                       |
| [Windows PowerShell プロパティ プロバイダーを作成する](./creating-a-windows-powershell-property-provider.md)     | このトピックでは、ユーザーがデータストア内の項目のプロパティを操作できるようにする Windows PowerShell プロバイダーを作成する方法について説明します。                                                    |

## <a name="see-also"></a>参照

[Windows PowerShell の動作](/previous-versions/ms714658(v=vs.85))

[Windows PowerShell SDK](../windows-powershell-reference.md)

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)
