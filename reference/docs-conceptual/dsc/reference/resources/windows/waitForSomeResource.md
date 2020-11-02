---
ms.date: 07/16/2020
ms.topic: reference
title: DSC WaitForSome リソース
description: DSC WaitForSome リソース
ms.openlocfilehash: bc9c3df2b476e7046ccfe6257acc1d1641e7594b
ms.sourcegitcommit: 196c7f8cd24560cac70c88acc89909f17a86aea9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2020
ms.locfileid: "93143094"
---
# <a name="dsc-waitforsome-resource"></a>DSC WaitForSome リソース

> 適用先:Windows PowerShell 5.x

**WaitForSome** Desired State Configuration (DSC) リソースを [DSC 構成](../../../configurations/configurations.md)のノード ブロック内で使用して、他のノードの構成の依存関係を指定することができます。

[!INCLUDE [Updated DSC Resources](../../../../../includes/dsc-resources.md)]

このリソースは、 **ResourceName** プロパティで指定されたリソースが、 **NodeName** プロパティで定義された最小数 ( **NodeCount** で指定) のノードで目的の状態になった場合に成功します。

> [!NOTE]
> **WaitForSome** リソースでは、Windows リモート管理を使用して他のノードの状態を確認します。 WinRM でのポートとセキュリティ要件の詳細については、「[PowerShell リモート処理のセキュリティに関する考慮事項](/powershell/scripting/learn/remoting/winrmsecurity)」を参照してください。

## <a name="syntax"></a>構文

```Syntax
WaitForSome [String] #ResourceName
{
    NodeCount = [UInt32]
    NodeName = [string[]]
    ResourceName = [string]
    [ RetryCount = [UInt32] ]
    [ RetryIntervalSec = [UInt64] ]
    [ ThrottleLimit = [UInt32] ]
    [ DependsOn = [string[]] ]
    [ PsDscRunAsCredential = [PSCredential] ]
}
```

## <a name="properties"></a>Properties

|プロパティ |説明 |
|---|---|
|NodeCount |このリソースが成功するために、目的の状態にする必要があるノードの最小数。 |
|NodeName |依存するリソースのターゲット ノード。 |
|ResourceName |依存するリソースの名前。 このリソースが別の構成に属している場合は、名前を `[ResourceType]ResourceName::[ConfigurationName]::[ConfigurationName]` という書式に設定します。 |
|RetryIntervalSec |再試行するまでの秒数。 最小値は 1 です。 |
|RetryCount |再試行の回数の最大数。 |
|ThrottleLimit |同時に接続するコンピューターの数。 既定値は、`New-CimSession` の既定値です。 |

## <a name="common-properties"></a>共通プロパティ

|プロパティ |説明 |
|---|---|
|DependsOn |このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が ResourceName で、そのタイプが ResourceType である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。 |
|PsDscRunAsCredential |リソース全体を実行するための資格情報を設定します。 |

> [!NOTE]
> **PsDscRunAsCredential** という共通プロパティは、他の資格情報という文脈の中であらゆる DSC リソースを実行するために WMF 5.0 で追加されました。 詳細については、「[DSC リソースに対して資格情報を使用する](../../../configurations/runasuser.md)」を参照してください。

## <a name="example"></a>例

このリソースを使用する方法の例は、「[ノードの相互依存関係の指定](../../../configurations/crossNodeDependencies.md)」を参照してください。
