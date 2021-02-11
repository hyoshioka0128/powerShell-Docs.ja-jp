---
ms.date: 01/17/2019
keywords: DSC, PowerShell, 構成, セットアップ
title: ノードを再起動する
description: 多くの構成設定では、構成の変更を完了するために、コンピューターの再起動が必要になることがあります。 この記事では、構成での再起動の管理方法について説明します。
ms.openlocfilehash: d2b0f77c34ebcb006821da1f4f8d7c4b046f7a95
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92645116"
---
# <a name="reboot-a-node"></a>ノードを再起動する

> [!NOTE]
> このトピックでは、ノードの再起動方法について説明します。 再起動を成功させるために、 **ActionAfterReboot** と **RebootNodeIfNeeded** の LCM 設定は適切に構成される必要があります。 ローカル構成マネージャーの設定については、「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」または[ローカル構成マネージャー (v4) の構成](../managing-nodes/metaConfig4.md)に関するページを参照してください。

ノードは、`$global:DSCMachineStatus` フラグを使用することで、リソース内から再起動できます。 `Set-TargetResource` 関数でこのフラグを `1` に設定すると、現在のリソースの **Set** メソッドの直後に、LCM によってノードが再起動されます。 このフラグを使用して、 [ComputerManagementDsc](https://github.com/PowerShell/ComputerManagementDsc) DSC リソース モジュールの **PendingReboot** リソースで、DSC の外部で再起動が保留されているかどうかを検出します。

ご利用の[構成](configurations.md)では、ノードの再起動を必要とするステップを実行できます。 これには次のような内容を含めることができます。

- Windows の更新プログラム
- ソフトウェアのインストール
- ファイル名の変更
- コンピューター名の変更

**PendingReboot** リソースでは、特定のコンピューターの場所を確認して、再起動が保留中かどうかが判断されます。 ノードで DSC の外部で再起動が必要な場合、 **PendingReboot** リソースでは `$global:DSCMachineStatus` フラグに `1` を設定して、再起動が強制され、保留中の状態が解決されます。

> [!NOTE]
> 任意の DSC リソースにより、`Set-TargetResource` 関数にこのフラグを設定することで、LCM にノードを再起動するように指示できます。 詳細については、「[MOF を使用したカスタム DSC リソースの記述](../resources/authoringResourceMOF.md)」を参照してください。

## <a name="syntax"></a>構文

```
PendingReboot [String] #ResourceName
{
    Name = [string]
    [DependsOn = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
    [SkipCcmClientSDK = [bool]]
    [SkipComponentBasedServicing = [bool]]
    [SkipPendingComputerRename = [bool]]
    [SkipPendingFileRename = [bool]]
    [SkipWindowsUpdate = [bool]]
}
```

## <a name="properties"></a>Properties

| プロパティ | 説明 |
| --- | --- |
| 名前| 構成内のリソースのインスタンスごとに一意である必要がある必須のパラメーターです。|
| SkipComponentBasedServicing | コンポーネント ベース サービシング コンポーネントによってトリガーされた再起動がスキップされます。 |
| SkipWindowsUpdate | Windows の更新プログラムによってトリガーされた再起動がスキップされます。|
| SkipPendingFileRename | 保留中のファイル名の変更による再起動がスキップされます。 |
| SkipCcmClientSDK | ConfigMgr クライアントによってトリガーされた再起動がスキップされます。 |
| SkipComputerRename | コンピューター名の変更によってトリガーされた再起動がスキップされます。 |
| PSDSCRunAsCredential | v5 でサポートされています。 特定のユーザーとしてリソースを実行します。 |
| DependsOn | このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が **ResourceName** で、そのタイプが **ResourceType** である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。 詳細については、[DependsOn の使用](resource-depends-on.md)に関するページを参照してください。|

## <a name="example"></a>例

次の例では、[xExchange](https://github.com/PowerShell/xExchange) リソースを使用して Microsoft Exchange をインストールします。 インストールの間中、 **PendingReboot** リソースはノードの再起動に使用されます。

> [!NOTE]
> この例には、Exchange サーバーをフォレストに追加する権限を持つ、アカウントの資格情報が必要です。 DSC での資格情報の使用に関する詳細については、「[DSC での資格情報の処理](../configurations/configDataCredentials.md)」を参照してください。

```powershell
$ConfigurationData = @{
    AllNodes = @(
        @{
            NodeName                    = '*'
            PSDSCAllowPlainTextPassword = $true
        },
        @{
            NodeName = 'DSCPULL-1'
        }
    )
}

Configuration Example
{
    param
    (
        [Parameter(Mandatory = $true)]
        [System.Management.Automation.PSCredential]
        $ExchangeAdminCredential
    )

    Import-DscResource -Module xExchange
    Import-DscResource -Module ComputerManagementDsc

    Node $AllNodes.NodeName
    {
        # Copy the Exchange setup files locally
        File ExchangeBinaries
        {
            Ensure          = 'Present'
            Type            = 'Directory'
            Recurse         = $true
            SourcePath      = '\\rras-1\Binaries\E15CU6'
            DestinationPath = 'C:\Binaries\E15CU6'
        }

        # Check if a reboot is needed before installing Exchange
        PendingReboot BeforeExchangeInstall
        {
            Name       = 'BeforeExchangeInstall'
            DependsOn  = '[File]ExchangeBinaries'
        }

        # Do the Exchange install
        xExchInstall InstallExchange
        {
            Path       = 'C:\Binaries\E15CU6\Setup.exe'
            Arguments  = '/mode:Install /role:Mailbox /Iacceptexchangeserverlicenseterms'
            Credential = $ExchangeAdminCredential
            DependsOn  = '[PendingReboot]BeforeExchangeInstall'
        }

        # See if a reboot is required after installing Exchange
        PendingReboot AfterExchangeInstall
        {
            Name      = 'AfterExchangeInstall'
            DependsOn = '[xExchInstall]InstallExchange'
        }
    }
}
```

> [!NOTE]
> この例では、再起動と、再起動後に構成を継続できるように、ローカル構成マネージャーを構成しているとします。

## <a name="where-to-download"></a>ダウンロードする場所

次の場所でこのトピックで使用したリソースをダウンロードできます。PowerShell ギャラリーを使用してダウンロードすることもできます。

- [ComputerManagementDsc](https://github.com/PowerShell/ComputerManagementDsc)
- [xExchange](https://github.com/PowerShell/xExchange)
