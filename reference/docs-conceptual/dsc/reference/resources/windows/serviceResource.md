---
ms.date: 01/06/2021
ms.topic: reference
title: DSC Service リソース
description: DSC Service リソース
ms.openlocfilehash: bb151e11475c6e67f1fcb2d73336ff2e34b749b8
ms.sourcegitcommit: afefb3636362857036648c2fe80215bc4e81f5ac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97957038"
---
# <a name="dsc-service-resource"></a>DSC Service リソース

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.x

PowerShell Desired State Configuration (DSC) の **Service** リソースは、ターゲット ノード上でサービスを管理するためのメカニズムを備えています。

[!INCLUDE [Updated DSC Resources](../../../../../includes/dsc-resources.md)]

## <a name="syntax"></a>構文

```Syntax
Service [string] #ResourceName
{
    Name = [string]
    [ BuiltInAccount = [string] { LocalService | LocalSystem | NetworkService }  ]
    [ Credential = [PSCredential] ]
    [ StartupType = [string] { Automatic | Disabled | Manual }  ]
    [ State = [string] { Running | Stopped }  ]
    [ Dependencies = [string[]] ]
    [ Description = [string] ]
    [ DisplayName = [string] ]
    [ Path = [string] ]
    [ DependsOn = [string[]] ]
    [ Ensure = [string] { Absent | Present } ]
    [ PsDscRunAsCredential = [PSCredential] ]
}
```

## <a name="properties"></a>Properties

|プロパティ |説明 |
|---|---|
|名前 |サービス名を示します。 これは、表示名とは異なることがあります。 `Get-Service` コマンドレットを使用すると、サービスとその現在の状態の一覧を取得できます。 |
|BuiltInAccount |サービスに使用するサインイン アカウントを示します。 このプロパティで有効な値は、**LocalService**、**LocalSystem**、**NetworkService** です。 |
|資格情報 |サービスを実行するアカウントの資格情報を示します。 このプロパティおよび **BuiltinAccount** プロパティを同時に使用することはできません。 |
|StartupType |サービスのスタートアップの種類を示します。 このプロパティで有効な値は、**Automatic**、**Disabled**、**Manual** です。 |
|State |サービスに対して保証する状態を示します。 値は次のとおりです。**Running** または **Stopped**。 |
|依存関係 | サービスに含まれる依存関係の名前の配列。 |
|説明 |ターゲット サービスの説明を示します。 |
|DisplayName |ターゲット サービスの表示名を示します。 |
|Path |新しいサービスのバイナリ ファイルのパスを示します。 |

## <a name="common-properties"></a>共通プロパティ

|プロパティ |説明 |
|---|---|
|DependsOn |このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が ResourceName で、そのタイプが ResourceType である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。 |
|Ensure |ターゲット サービスがシステムに存在するかどうかを示します。 ターゲット サービスが存在しないようにするには、このプロパティを **[Absent]** に設定します。 対象サービスの存在を保証するには、これを **Present** に設定します 既定値は **Present** です。 |
|PsDscRunAsCredential |リソース全体を実行するための資格情報を設定します。 |

> [!NOTE]
> **PsDscRunAsCredential** という共通プロパティは、他の資格情報という文脈の中であらゆる DSC リソースを実行するために WMF 5.0 で追加されました。 詳細については、「[DSC リソースに対して資格情報を使用する](../../../configurations/runasuser.md)」を参照してください。

## <a name="example"></a>例

```powershell
configuration ServiceTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {

        Service ServiceExample
        {
            Name        = "TermService"
            StartupType = "Manual"
            State       = "Running"
        }
    }
}
```
