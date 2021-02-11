---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: ノードの相互依存関係の指定
description: DSC には、他のノードの構成に対する依存関係を指定するために構成内で使用される、特別なリソースが用意されています。
ms.openlocfilehash: a9fc09af922839b37db476c24c113efc5e3e8cb1
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92658492"
---
# <a name="specifying-cross-node-dependencies"></a>ノードの相互依存関係の指定

> 適用先:Windows PowerShell 5.0

DSC には、 **WaitForAll** 、 **WaitForAny** 、 **WaitForSome** などの特別なリソースが用意されています。このリソースを構成で使用すると、他のノードの構成への依存関係を指定することができます。 これらのリソースの動作は次のとおりです。

- **WaitForAll** : **NodeName** プロパティで定義されているすべてのターゲット ノードで、指定されたリソースが目的の状態である場合に成功します。
- **WaitForAny** : **NodeName** プロパティで定義されているターゲット ノードの少なくとも 1 つで、指定されたリソースが目的の状態である場合に成功します。
- **WaitForSome** : **NodeName** プロパティのほか、 **NodeCount** プロパティも指定します。 リソースは、 **NodeName** プロパティで定義されたノードの最小数 ( **NodeCount** で指定) で目的の状態になった場合に成功します。

## <a name="syntax"></a>構文

**WaitForAll** リソースと **WaitForAny** リソースは構文が同じです。 次の例の `<ResourceType>` を、 **WaitForAny** または **WaitForAll** に置き換えます。 **DependsOn** キーワードと同じように、名前は `[ResourceType]ResourceName` という形式にする必要があります。 リソースが異なる [Configuration](configurations.md) に属している場合は、 **ConfigurationName** を書式設定する文字列 `[ResourceType]ResourceName::[ConfigurationName]::[ConfigurationName]` に含めます。 **NodeName** は、現在のリソースが待機する必要があるコンピューターまたはノードです。

```
<ResourceType> [string] #ResourceName
{
    ResourceName = [string]
    NodeName = [string]
    [ DependsOn = [string[]] ]
    [ PsDscRunAsCredential = [PSCredential]]
    [ RetryCount = [Uint32] ]
    [ RetryIntervalSec = [Uint64] ]
    [ ThrottleLimit = [Uint32]]
}
```

**WaitForSome** リソースは上の例と似た構文ですが、 **NodeCount** キーを追加します。 **NodeCount** は、現在のリソースが待機する必要のあるノードの数を示します。

```
WaitForSome [String] #ResourceName
{
    NodeCount = [UInt32]
    NodeName = [string[]]
    ResourceName = [string]
    [DependsOn = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
    [RetryCount = [UInt32]]
    [RetryIntervalSec = [UInt64]]
    [ThrottleLimit = [UInt32]]
}
```

すべての **WaitForXXXX** が次の構文キーを共有します。

|       プロパティ       |                                                                           説明                                                                           |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RetryIntervalSec     | 再試行するまでの秒数。 最小値は 1 です。                                                                                                            |
| RetryCount           | 再試行の回数の最大数。                                                                                                                           |
| ThrottleLimit        | 同時に接続するコンピューターの数。 既定値は、`New-CimSession` の既定値です。                                                                              |
| DependsOn            | このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 詳細については、[DependsOn](resource-depends-on.md) に関するページを参照してください。 |
| PsDscRunAsCredential | [ユーザーの資格情報を指定した DSC の使用](./runAsUser.md)に関するページを参照してください。                                                                                                           |

## <a name="using-waitforxxxx-resources"></a>WaitForXXXX リソースの使用

各 **WaitForXXXX** リソースは、指定されたノードで指定された数のリソースが完了するのを待機します。
同じ Configuration の他のリソースは、 **DependsOn** キーを使用して **WaitForXXXX** リソースに " *依存する* " ことができます。

たとえば、次の構成では、ターゲット ノードは **MyDC** ノード上の **xADDomain** リソースが完了するまで 15 秒間隔で最大 30 回試行しながら待機した後、ドメインに参加できるようになります。

既定では、 **WaitForXXX** リソースは 1 回試行してから失敗します。 これは必須ではありませんが、通常は、 **RetryCount** と **RetryIntervalSec** を指定します。

```powershell
Configuration JoinDomain
{
    Import-DscResource -Module xComputerManagement, xActiveDirectory

    Node myDC
    {
        WindowsFeature InstallAD
        {
            Ensure = 'Present'
            Name = 'AD-Domain-Services'
        }

        xADDomain NewDomain
        {
            DomainName = 'Contoso.com'
            DomainAdministratorCredential = (Get-Credential)
            SafemodeAdministratorPassword = (Get-Credential)
            DatabasePath = "C:\Windows\NTDS"
            LogPath = "C:\Windows\NTDS"
            SysvolPath = "C:\Windows\Sysvol"
        }
    }

    Node myDomainJoinedServer
    {
        WaitForAll DC
        {
            ResourceName      = '[xADDomain]NewDomain'
            NodeName          = 'MyDC'
            RetryIntervalSec  = 15
            RetryCount        = 30
        }

        xComputer JoinDomain
        {
            Name             = 'myPC'
            DomainName       = 'Contoso.com'
            Credential       = (Get-Credential)
            DependsOn        ='[WaitForAll]DC'
        }
    }
}
```

Configuration をコンパイルすると、2 つの ".mof" ファイルが生成されます。 [Start-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを使って、両方の ".mof" ファイルをターゲットの Node に適用します

> [!NOTE]
> **WaitForXXX** リソースでは、Windows リモート管理を使用して他のノードの状態を確認します。 WinRM でのポートとセキュリティ要件の詳細については、「[PowerShell リモート処理のセキュリティに関する考慮事項](/powershell/scripting/learn/remoting/winrmsecurity)」を参照してください。

## <a name="see-also"></a>参照

- [DSC 構成](configurations.md)
- [リソースの依存関係を使用する](resource-depends-on.md)
- [DSC リソース](../resources/resources.md)
- [ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)
