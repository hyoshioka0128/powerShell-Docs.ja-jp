---
title: PowerShell 7 モジュールの互換性
ms.date: 02/03/2020
description: この記事では、他の Microsoft 製品向けに公開されている PowerShell モジュールでの PowerShell 7 の状態を示します。
ms.openlocfilehash: 718ba0f502a23bc2c2a9268d65d3b8129de0af49
ms.sourcegitcommit: 7f712e12ec5b3f3f3e695da804b050ea0ce58b3a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "94661377"
---
# <a name="powershell-7-module-compatibility"></a>PowerShell 7 モジュールの互換性

この記事では、Microsoft 発行の PowerShell モジュールの一覧を示します。 このモジュールには、さまざまな Microsoft 製品およびサービスの管理とサポートが用意されています。 これらのモジュールは、PowerShell 7 でネイティブに動作するように更新されているか、PowerShell 7 との互換性がテストされています。 さらに別のモジュールが特定およびテストされると、このリストは新しい情報によって更新されます。

共有すべき情報をお持ちの場合、または特定のモジュールに関する問題があるときは、[WindowsCompatibility リポジトリ](https://github.com/PowerShell/WindowsCompatibility)で問題を報告してください。

## <a name="windows-management-modules"></a>Windows 管理モジュール

Windows 管理モジュールは、Windows のエディションと、そのエディションのモジュールのパッケージ方法に応じてさまざまな方法でインストールされます。

Windows Server では、[Install-WindowsFeature](/powershell/module/servermanager/install-windowsfeature) コマンドレットを使って、管理者として機能名を使用します。 次に例を示します。

```powershell
Install-WindowsFeature -Name ActiveDirectory
```

Windows 10 では、Windows 管理モジュールは **Windows オプション機能** または **Windows 機能** として使用できます。 次のコマンドは、 **[管理者として実行]** を使用して、管理者特権セッションから実行する必要があります。

- Windows オプション機能の場合

  オプション機能の一覧を取得するには、次のコマンドを実行します。

  ```powershell
  Get-WindowsOptionalFeature -Online
  ```

  機能をインストールするには:

  ```powershell
  Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Management-PowerShell
  ```

  詳細情報

  - [Get-WindowsOptionalFeature](/powershell/module/dism/get-windowsoptionalfeature)
  - [Enable-WindowsOptionalFeature](/powershell/module/dism/enable-windowsoptionalfeature)

- Windows 機能の場合

  Windows 機能の一覧を取得するには、次のコマンドを実行します。

  ```powershell
  Get-WindowsCapability -online
  ```

  機能パッケージの名前の末尾が `~~~~0.0.1.0` であることに注意してください。 機能をインストールするには、完全名を使用する必要があります。

  ```powershell
  Add-WindowsCapability -Online -Name Rsat.ServerManager.Tools~~~~0.0.1.0
  ```

  詳細情報

  - [Get-WindowsCapability](/powershell/module/dism/get-windowscapability)
  - [Add-WindowsCapability](/powershell/module/dism/add-windowscapability)

### <a name="module-list"></a>モジュール一覧

| モジュール名                        | Status                               | サポート対象 OS                       |
| ---------------------------------- | ------------------------------------ | ---------------------------------- |
| ActiveDirectory                    | ネイティブに互換性あり                  | RSAT-AD-PowerShell 搭載 Windows Server 1809 以降<br>Rsat.ActiveDirectory.DS-LDS.Tools 搭載 Windows 10 1809 以降 |
| ADDSDeployment                     | 互換性レイヤーで動作する       |  Windows Server 2019 1809+         |
| ADFS                               | 互換性レイヤーでテストされていない    |                                    |
| AppBackgroundTask                  | ネイティブに互換性あり                  | Windows 10 1903 以降                   |
| AppLocker                          | 互換性レイヤーでテストされていない    |                                    |
| AppvClient                         | 互換性レイヤーでテストされていない    |                                    |
| appx                               | ネイティブに互換性あり**                | Windows Server 1809 以降<br>Windows 10 1809 以降<br>**PowerShell 7.1 で互換性レイヤーを使用する必要があります |
| AssignedAccess                     | ネイティブに互換性あり                  | Windows 10 1809 以降                   |
| BestPractices                      | 互換性レイヤーでサポートされていない |                                    |
| BitLocker                          | ネイティブに互換性あり                  | BitLocker 搭載 Windows Server 1809 以降<br>Windows 10 1809 以降 |
| BitsTransfer                       | ネイティブに互換性あり                  | Windows Server 20H1<br>Windows 10 20H1 |
| BootEventCollector                 | 互換性レイヤーでテストされていない    |                                        |
| BranchCache                        | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降 |
| CimCmdlets                         | ネイティブに互換性あり                  | PowerShell 7 に組み込み |
| ClusterAwareUpdating               | 互換性レイヤーでテストされていない    |                         |
| ConfigCI                           | 互換性レイヤーでテストされていない    |                         |
| Defender                           | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降  |
| DeliveryOptimization               | ネイティブに互換性あり                  | Windows Server 1903 以降<br>Windows 10 1903 以降  |
| DFSN                               | ネイティブに互換性あり                  | FS-DFS-Namespace 搭載 Windows Server 1809 以降<br>Rsat.FailoverCluster.Management.Tools 搭載 Windows 10 1809 以降 |
| DFSR                               | 互換性レイヤーでテストされていない    |                                   |
| DhcpServer                         | 互換性レイヤーでテストされていない    |                                   |
| DirectAccessClientComponents       | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降  |
| Dism                               | ネイティブに互換性あり                  | Windows Server 1903 以降<br>Windows 10 1903 以降  |
| DnsClient                          | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降  |
| DnsServer                          | ネイティブに互換性あり                  | DNS または RSAT-DNS-Server 搭載 Windows Server 1809 以降<br>Rsat.Dns.Tools 搭載 Windows 10 1809 以降 |
| EventTracingManagement             | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降  |
| FailoverClusters                   | 互換性レイヤーでテストされていない    |                                  |
| FailoverClusterSet                 | 互換性レイヤーでテストされていない    |                                  |
| FileServerResourceManager          | ネイティブに互換性あり                  | FS-Resource-Manager 搭載 Windows Server 1809 以降 |
| GroupPolicy                        | 互換性レイヤーでテストされていない    |                                               |
| HgsClient                          | ネイティブに互換性あり                  | Hyper-V または RSAT-Shielded-VM-Tools 搭載 Windows Server 1903 以降<br>Rsat.Shielded.VM.Tools 搭載 Windows 10 1903 以降 |
| HgsDiagnostics                     | ネイティブに互換性あり                  | Hyper-V または RSAT-Shielded-VM-Tools 搭載 Windows Server 1809 以降<br>Rsat.Shielded.VM.Tools 搭載 Windows 10 1809 以降 |
| Hyper-V                            | ネイティブに互換性あり                  | Hyper-V-PowerShell 搭載 Windows Server 1809 以降<br>Microsoft-Hyper-V-Management-PowerShell 搭載 Windows 10 1809 以降 |
| IISAdministration                  | 互換性レイヤーでテストされていない    |                                               |
| 地域と言語                      | ネイティブに互換性あり                  | Windows Server 1903 以降<br>Windows 10 1903 以降      |
| IpamServer                         | 互換性レイヤーでテストされていない    |                                               |
| iSCSI                              | 互換性レイヤーでテストされていない    |                                               |
| IscsiTarget                        | 互換性レイヤーでテストされていない    |                                               |
| ISE                                | 互換性レイヤーでテストされていない    |                                               |
| Kds                                | ネイティブに互換性あり                  | Windows Server 20H1<br>Windows 10 20H1        |
| Microsoft.PowerShell.Archive       | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| Microsoft.PowerShell.Diagnostics   | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| Microsoft.PowerShell.Host          | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| Microsoft.PowerShell.LocalAccounts | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| Microsoft.PowerShell.Management    | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| Microsoft.PowerShell.ODataUtils    | 互換性レイヤーでテストされていない    |                                               |
| Microsoft.PowerShell.Security      | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| Microsoft.PowerShell.Utility       | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| Microsoft.WSMan.Management         | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| MMAgent                            | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| MPIO                               | ネイティブに互換性あり                  | Multipath-IO 搭載 Windows Server 1809 以降        |
| MsDtc                              | 互換性レイヤーでテストされていない    |                                               |
| NetAdapter                         | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetConnection                      | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetEventPacketCapture              | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetLbfo                            | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetLldpAgent                       | 互換性レイヤーでテストされていない    |                                               |
| NetNat                             | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetQos                             | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetSecurity                        | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetSwitchTeam                      | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetTCPIP                           | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetWNV                             | 互換性レイヤーでテストされていない    |                                               |
| NetworkConnectivityStatus          | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetworkController                  | 互換性レイヤーでテストされていない    |                                               |
| NetworkControllerDiagnostics       | 互換性レイヤーでテストされていない    |                                               |
| NetworkLoadBalancingClusters       | 互換性レイヤーでテストされていない    |                                               |
| NetworkSwitchManager               | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NetworkTransition                  | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| NFS                                | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Rsat.ServerManager.Tools 搭載 Windows 10 1809 以降 |
| PackageManagement                  | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| PcsvDevice                         | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| PersistentMemory                   | 互換性レイヤーでテストされていない    |                                               |
| PKI                                | 互換性レイヤーでテストされていない    |                                               |
| PnpDevice                          | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| PowerShellGet                      | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| PrintManagement                    | ネイティブに互換性あり                  | Print-Services 搭載 Windows Server 1903 以降<br>Windows 10 1903 以降  |
| ProcessMitigations                 | ネイティブに互換性あり                  | Windows Server 1903 以降<br>Windows 10 1903 以降      |
| プロビジョニング                       | 互換性レイヤーでテストされていない    |                                               |
| PSDesiredStateConfiguration        | 部分的                            | PowerShell 7 に組み込み                       |
| PSDiagnostics                      | ネイティブに互換性あり                  | PowerShell 7 に組み込み                       |
| PSScheduledJob                     | 互換性レイヤーでサポートされていない | PowerShell 5.1 に組み込み                     |
| PSWorkflow                         | 互換性レイヤーでテストされていない    |                                               |
| PSWorkflowUtility                  | 互換性レイヤーでテストされていない    |                                               |
| RemoteAccess                       | 互換性レイヤーでテストされていない    |                                               |
| RemoteDesktop                      | 互換性レイヤーでテストされていない    |                                               |
| ScheduledTasks                     | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| SecureBoot                         | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| ServerCore                         | 互換性レイヤーでテストされていない    |                                               |
| ServerManager                      | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Rsat.ServerManager.Tools 搭載 Windows 10 1809 以降<br>"_下記のメモを参照してください_" |
| ServerManagerTasks                 | 互換性レイヤーでテストされていない    |                                               |
| ShieldedVMDataFile                 | ネイティブに互換性あり                  | RSAT-Shielded-VM-Tools 搭載 Windows Server 1903 以降<br>Rsat.Shielded.VM.Tools 搭載 Windows 10 1903 以降 |
| ShieldedVMProvisioning             | ネイティブに互換性あり                  | HostGuardian 搭載 Windows Server 1809 以降<br>HostGuardian 搭載 Windows 10 1809 以降  |
| ShieldedVMTemplate                 | ネイティブに互換性あり                  | RSAT-Shielded-VM-Tools 搭載 Windows Server 1809 以降<br>Rsat.Shielded.VM.Tools 搭載 Windows 10 1809 以降 |
| SmbShare                           | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| SmbWitness                         | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| SMISConfig                         | ネイティブに互換性あり                  | WindowsStorageManagementService 搭載 Windows Server 1903 以降 |
| SMS                                | 互換性レイヤーでテストされていない    |                                               |
| SoftwareInventoryLogging           | ネイティブに互換性あり                  | Windows Server 1809 以降                          |
| StartLayout                        | ネイティブに互換性あり                  | デスクトップ エクスペリエンス搭載 Windows Server 1809 以降<br>Windows 10 1809 以降 |
| ストレージ                            | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| StorageBusCache                    | 互換性レイヤーでテストされていない    |                                               |
| StorageMigrationService            | 互換性レイヤーでテストされていない    |                                               |
| StorageQOS                         | ネイティブに互換性あり                  | RSAT-Clustering-PowerShell 搭載 Windows Server 1809 以降<br>Rsat.FailoverCluster.Management.Tools 搭載 Windows 10 1809 以降 |
| StorageReplica                     | 互換性レイヤーでテストされていない    |                                               |
| SyncShare                          | ネイティブに互換性あり                  | FS-SyncShareService 搭載 Windows Server 1809 以降 |
| SystemInsights                     | 互換性レイヤーでテストされていない    |                                               |
| TLS                                | 互換性レイヤーでテストされていない    |                                               |
| TroubleshootingPack                | ネイティブに互換性あり                  | Windows 10 1903 以降                              |
| TrustedPlatformModule              | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| UEV                                | ネイティブに互換性あり                  | Windows Server ??将来のバージョンのデスクトップ エクスペリエンス搭載サーバー??<br>Windows 10 1903 以降 |
| UpdateServices                     | 互換性レイヤーでサポートされていない |                                               |
| VpnClient                          | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| Wdac                               | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| WebAdministration                  | 互換性レイヤーでテストされていない    |                                               |
| WHEA                               | ネイティブに互換性あり                  | Windows Server 1903 以降<br>Windows 10 1903 以降      |
| WindowsDeveloperLicense            | ネイティブに互換性あり                  | デスクトップ エクスペリエンス搭載 Windows Server 1809 以降<br>Windows 10 1809 以降 |
| WindowsErrorReporting              | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降      |
| WindowsSearch                      | ネイティブに互換性あり                  | Windows 10 1903 以降                              |
| WindowsServerBackup                | ネイティブに互換性あり                  | Windows-Server-Backup 搭載 Windows Server 19H2 |
| WindowsUpdate                      | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降       |
| WindowsUpdateProvider              | ネイティブに互換性あり                  | Windows Server 1809 以降<br>Windows 10 1809 以降       |

## <a name="notes"></a>メモ

### <a name="servermanager-module"></a>ServerManager モジュール

モジュールには、PowerShell 7 の書式付き出力に関するマイナーな互換性の問題がいくつかあります。 たとえば、`Get-WindowsFeature` コマンドレットは、すべてのプロパティが含まれる適切なオブジェクトを返しますが、既定の表示書式を使用すると、一部のプロパティが空として表示されます。 実際の値は、`Select-Object` を使用するか直接メンバー アクセスによって、オブジェクトのプロパティで確認できます。

