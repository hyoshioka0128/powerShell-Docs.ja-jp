---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC WindowsFeature リソース
ms.openlocfilehash: 7a57f4b2797ab3bb202aea8b2543d1e3f14074e9
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076703"
---
# <a name="dsc-windowsfeature-resource"></a>DSC WindowsFeature リソース

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) の **WindowsFeature** リソースは、役割と機能がターゲット ノードで追加または削除されることを保証するためのメカニズムを備えています。

## <a name="syntax"></a>構文

```
WindowsFeature [string] #ResourceName
{
    Name = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ IncludeAllSubFeature = [bool] ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ Source = [string] ]
}
```

## <a name="properties"></a>プロパティ

|  プロパティ  |  説明   |
|---|---|
| 名前| 追加または削除されることを保証する役割または機能の名前を示します。 これは、[Get-WindowsFeature](/powershell/module/servermanager/Get-WindowsFeature) コマンドレットからの __Name__ プロパティと同じものであり、役割または機能の表示名ではありません。|
| Credential| 役割または機能の追加や削除に使用する資格情報を示します。|
| Ensure| 役割または機能が追加されるかどうかを示します。 役割または機能が追加されることを保証するには、このプロパティを "Present" に設定します。役割または機能が削除されることを保証するには、このプロパティを "Absent" に設定します。|
| IncludeAllSubFeature| __Name__ プロパティで指定した機能の状態を使用して必要なすべてのサブ機能の状態を保証するには、このプロパティを __$true__ に設定します。|
| LogPath| リソース プロバイダーの操作を記録するログ ファイルへのパスを示します。|
| DependsOn| このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が __ResourceName__ で、そのタイプが __ResourceType__ である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。|
| ソース| 必要に応じて、インストールに使用するソース ファイルの場所を示します。|

## <a name="example"></a>例
```powershell
WindowsFeature RoleExample
{
    Ensure = "Present"
    # Alternatively, to ensure the role is uninstalled, set Ensure to "Absent"
    Name = "Web-Server" # Use the Name property from Get-WindowsFeature
}
```