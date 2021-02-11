---
ms.date: 02/03/2020
keywords: powershell、core
title: モジュールとコマンドレットのリリース履歴
description: この記事では、PowerShell のさまざまなバージョンに含まれるモジュールとコマンドレットの一覧を示します。
ms.openlocfilehash: e79735e516c9aaa485c6513fb80de623014f06f5
ms.sourcegitcommit: 2fc6ee49a70bda4c59135136bd5cc7782836a124
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2020
ms.locfileid: "94810353"
---
# <a name="release-history-of-modules-and-cmdlets"></a>モジュールとコマンドレットのリリース履歴

この記事では、PowerShell のさまざまなバージョンに含まれるモジュールとコマンドレットの一覧を示します。 これは、リリース ノートに記載されている情報のまとめです。 詳細については、リリース ノートを参照してください。

- [PowerShell 7.0 の新機能](what-s-new-in-powershell-70.md)

この記事は作成中です。 以下の情報を最新の状態に保つためにご協力ください。

## <a name="module-release-history"></a>モジュールのリリース履歴

|         モジュール名 / PS バージョン          |   5.1   |   6.x   |   7.0   |   7.1   |     Note     |
| ----------------------------------------- | :-----: | :-----: | :-----: | :-----: | ------------ |
| CimCmdlets                                | &check; | &check; | &check; | &check; | Windows のみ |
| ISE (2.0 で導入)                   | &check; |         |         |         | Windows のみ |
| Microsoft.PowerShell.Archive              | &check; | &check; | &check; | &check; |              |
| Microsoft.PowerShell.Core                 | &check; | &check; | &check; | &check; |              |
| Microsoft.PowerShell.Diagnostics          | &check; | &check; | &check; | &check; | Windows のみ |
| Microsoft.PowerShell.Host                 | &check; | &check; | &check; | &check; |              |
| Microsoft.PowerShell.LocalAccounts        | &check; |         |         |         | Windows のみ |
| Microsoft.PowerShell.Management           | &check; | &check; | &check; | &check; |              |
| Microsoft.PowerShell.ODataUtils           | &check; |         |         |         | Windows のみ |
| Microsoft.PowerShell.Operation.Validation | &check; |         |         |         | Windows のみ |
| Microsoft.PowerShell.Security             | &check; | &check; | &check; | &check; |              |
| Microsoft.PowerShell.Utility              | &check; | &check; | &check; | &check; |              |
| Microsoft.WsMan.Management                | &check; | &check; | &check; | &check; | Windows のみ |
| PackageManagement                         | &check; | &check; | &check; | &check; |              |
| PowershellGet                             | &check; | &check; | &check; | &check; |              |
| PSDesiredStateConfiguration               | &check; | &check; | &check; | &check; |              |
| PSDiagnostics                             | &check; | &check; | &check; | &check; | Windows のみ |
| PSReadline 1.x                            | &check; |         |         |         | Windows のみ |
| PSReadline 2.0                            |         | &check; | &check; |         |              |
| PSReadline 2.1                            |         |         |         | &check; |              |
| PSScheduledJob                            | &check; |         |         |         | Windows のみ |
| PSWorkflow                                | &check; |         |         |         | Windows のみ |
| PSWorkflowUtility                         | &check; |         |         |         | Windows のみ |
| ThreadJob                                 |         | &check; | &check; | &check; | PowerShell 5.1 にインストール可能 |

## <a name="cmdlet-release-history"></a>コマンドレットのリリース履歴

### <a name="cimcmdlets"></a>CimCmdlets

|         コマンドレット名         |   5.1   |   6.x   |   7.0   |   7.1   |     Note     |
| --------------------------- | :-----: | :-----: | :-----: | :-----: | ------------ |
| Export-BinaryMiLog          | &check; | &check; | &check; | &check; | Windows のみ |
| Get-CimAssociatedInstance   | &check; | &check; | &check; | &check; | Windows のみ |
| Get-CimClass                | &check; | &check; | &check; | &check; | Windows のみ |
| Get-CimInstance             | &check; | &check; | &check; | &check; | Windows のみ |
| Get-CimSession              | &check; | &check; | &check; | &check; | Windows のみ |
| Import-BinaryMiLog          | &check; | &check; | &check; | &check; | Windows のみ |
| Invoke-CimMethod            | &check; | &check; | &check; | &check; | Windows のみ |
| New-CimInstance             | &check; | &check; | &check; | &check; | Windows のみ |
| New-CimSession              | &check; | &check; | &check; | &check; | Windows のみ |
| New-CimSessionOption        | &check; | &check; | &check; | &check; | Windows のみ |
| Register-CimIndicationEvent | &check; | &check; | &check; | &check; | Windows のみ |
| Remove-CimInstance          | &check; | &check; | &check; | &check; | Windows のみ |
| Remove-CimSession           | &check; | &check; | &check; | &check; | Windows のみ |
| Set-CimInstance             | &check; | &check; | &check; | &check; | Windows のみ |

### <a name="ise-introduced-in-20"></a>ISE (2.0 で導入)

|    コマンドレット名    |   5.1   | 6.x  |  7.0  |  7.1  |     Note     |
| ----------------- | :-----: | :--- | :---: | :---: | ------------ |
| Get-IseSnippet    | &check; |      |       |       | Windows のみ |
| Import-IseSnippet | &check; |      |       |       | Windows のみ |
| New-IseSnippet    | &check; |      |       |       | Windows のみ |

### <a name="microsoftpowershellarchive"></a>Microsoft.PowerShell.Archive

|   コマンドレット名    |   5.1   |   6.x   |   7.0   |   7.1   | Note |
| ---------------- | :-----: | :-----: | :-----: | :-----: | ---- |
| Compress-Archive | &check; | &check; | &check; | &check; |      |
| Expand-Archive   | &check; | &check; | &check; | &check; |      |

### <a name="microsoftpowershellcore"></a>Microsoft.PowerShell.Core

|            コマンドレット名            |   5.1   |   6.x   |   7.0   |   7.1   |            Note            |
| --------------------------------- | :-----: | :-----: | :-----: | :-----: | -------------------------- |
| Add-History                       | &check; | &check; | &check; | &check; |                            |
| Add-PSSnapin                      | &check; |         |         |         | Windows のみ               |
| Clear-History                     | &check; | &check; | &check; | &check; |                            |
| Clear-Host                        | &check; | &check; | &check; | &check; |                            |
| Connect-PSSession                 | &check; | &check; | &check; | &check; | Windows のみ               |
| Debug-Job                         | &check; | &check; | &check; | &check; |                            |
| Disable-ExperimentalFeature       |         |   6.2   | &check; | &check; |                            |
| Disable-PSRemoting                | &check; | &check; | &check; | &check; | Windows のみ               |
| Disable-PSSessionConfiguration    | &check; | &check; | &check; | &check; | Windows のみ               |
| Disconnect-PSSession              | &check; | &check; | &check; | &check; | Windows のみ               |
| Enable-ExperimentalFeature        |         |   6.2   | &check; | &check; |                            |
| Enable-PSRemoting                 | &check; | &check; | &check; | &check; | Windows のみ               |
| Enable-PSSessionConfiguration     | &check; | &check; | &check; | &check; | Windows のみ               |
| Enter-PSHostProcess               | &check; | &check; | &check; | &check; | 6\.2 で Linux サポートが追加されました |
| Enter-PSSession                   | &check; | &check; | &check; | &check; |                            |
| Exit-PSHostProcess                | &check; | &check; | &check; | &check; | 6\.2 で Linux サポートが追加されました |
| Exit-PSSession                    | &check; | &check; | &check; | &check; |                            |
| Export-Console                    | &check; |         |         |         | Windows のみ               |
| Export-ModuleMember               | &check; | &check; | &check; | &check; |                            |
| ForEach-Object                    | &check; | &check; | &check; | &check; |                            |
| Get-Command                       | &check; | &check; | &check; | &check; |                            |
| Get-ExperimentalFeature           |         |   6.2   | &check; | &check; |                            |
| Get-Help                          | &check; | &check; | &check; | &check; |                            |
| Get-History                       | &check; | &check; | &check; | &check; |                            |
| Get-Job                           | &check; | &check; | &check; | &check; |                            |
| Get-Module                        | &check; | &check; | &check; | &check; |                            |
| Get-PSHostProcessInfo             | &check; | &check; | &check; | &check; | 6\.2 で Linux サポートが追加されました |
| Get-PSSession                     | &check; | &check; | &check; | &check; |                            |
| Get-PSSessionCapability           | &check; | &check; | &check; | &check; |                            |
| Get-PSSessionConfiguration        | &check; | &check; | &check; | &check; |                            |
| Get-PSSnapin                      | &check; |         |         |         | Windows のみ               |
| Get-Verb                          | &check; |         |         |         | Microsoft.PowerShell.Utility 6.0 以降に移動 |
| Import-Module                     | &check; | &check; | &check; | &check; |                            |
| Invoke-Command                    | &check; | &check; | &check; | &check; |                            |
| Invoke-History                    | &check; | &check; | &check; | &check; |                            |
| New-Module                        | &check; | &check; | &check; | &check; |                            |
| New-ModuleManifest                | &check; | &check; | &check; | &check; |                            |
| New-PSRoleCapabilityFile          | &check; | &check; | &check; | &check; |                            |
| New-PSSession                     | &check; | &check; | &check; | &check; |                            |
| New-PSSessionConfigurationFile    | &check; | &check; | &check; | &check; | Windows のみ               |
| New-PSSessionOption               | &check; | &check; | &check; | &check; |                            |
| New-PSSessionOption             | &check; | &check; | &check; | &check; |                            |
| Out-Default                       | &check; | &check; | &check; | &check; |                            |
| Out-Host                          | &check; | &check; | &check; | &check; |                            |
| Out-Null                          | &check; | &check; | &check; | &check; |                            |
| Receive-Job                       | &check; | &check; | &check; | &check; |                            |
| Receive-PSSession                 | &check; | &check; | &check; | &check; | Windows のみ               |
| Register-ArgumentCompleter        | &check; | &check; | &check; | &check; |                            |
| Register-PSSessionConfiguration   | &check; | &check; | &check; | &check; | Windows のみ               |
| Remove-Job                        | &check; | &check; | &check; | &check; |                            |
| Remove-Module                     | &check; | &check; | &check; | &check; |                            |
| Remove-PSSession                  | &check; | &check; | &check; | &check; |                            |
| Remove-PSSnapin                   | &check; |         |         |         | Windows のみ               |
| Resume-Job                        | &check; |         |         |         |                            |
| Save-Help                         | &check; | &check; | &check; | &check; |                            |
| Set-PSDebug                       | &check; | &check; | &check; | &check; |                            |
| Set-PSSessionConfiguration        | &check; | &check; | &check; | &check; | Windows のみ               |
| Set-StrictMode                    | &check; | &check; | &check; | &check; |                            |
| Start-Job                         | &check; | &check; | &check; | &check; |                            |
| Stop-Job                          | &check; | &check; | &check; | &check; |                            |
| Suspend-Job                       | &check; |         |         |         | Windows のみ               |
| Test-ModuleManifest               | &check; | &check; | &check; | &check; |                            |
| Test-PSSessionConfigurationFile   | &check; | &check; | &check; | &check; | Windows のみ               |
| Unregister-PSSessionConfiguration | &check; | &check; | &check; | &check; | Windows のみ               |
| Update-Help                       | &check; | &check; | &check; | &check; |                            |
| Wait-Job                          | &check; | &check; | &check; | &check; |                            |
| Where-Object                      | &check; | &check; | &check; | &check; |                            |

### <a name="microsoftpowershelldiagnostics"></a>Microsoft.PowerShell.Diagnostics

|  コマンドレット名   |   5.1   |   6.x   |   7.0   |   7.1   |     Note     |
| -------------- | :-----: | :-----: | :-----: | :-----: | ------------ |
| Export-Counter | &check; |         |         |         | Windows のみ |
| Get-Counter    | &check; |         | &check; | &check; | Windows のみ |
| Get-WinEvent   | &check; | &check; | &check; | &check; | Windows のみ |
| Import-Counter | &check; |         |         |         | Windows のみ |
| New-WinEvent   | &check; | &check; | &check; | &check; | Windows のみ |

### <a name="microsoftpowershellhost"></a>Microsoft.PowerShell.Host

|   コマンドレット名    |   5.1   |   6.x   |   7.0   |   7.1   | Note |
| ---------------- | :-----: | :-----: | :-----: | :-----: | ---- |
| Start-Transcript | &check; | &check; | &check; | &check; |      |
| Stop-Transcript  | &check; | &check; | &check; | &check; |      |

### <a name="microsoftpowershelllocalaccounts"></a>Microsoft.PowerShell.LocalAccounts

|       コマンドレット名       |   5.1   | 6.x  |  7.0  |  7.1  |     Note     |
| ----------------------- | :-----: | :--- | :---: | :---: | ------------ |
| Add-LocalGroupMember    | &check; |      |       |       | Windows のみ |
| Disable-LocalUser       | &check; |      |       |       | Windows のみ |
| Enable-LocalUser        | &check; |      |       |       | Windows のみ |
| Get-LocalGroup          | &check; |      |       |       | Windows のみ |
| Get-LocalGroupMember    | &check; |      |       |       | Windows のみ |
| Get-LocalUser           | &check; |      |       |       | Windows のみ |
| New-LocalGroup          | &check; |      |       |       | Windows のみ |
| New-LocalUser           | &check; |      |       |       | Windows のみ |
| Remove-LocalGroup       | &check; |      |       |       | Windows のみ |
| Remove-LocalGroupMember | &check; |      |       |       | Windows のみ |
| Remove-LocalUser        | &check; |      |       |       | Windows のみ |
| Rename-LocalGroup       | &check; |      |       |       | Windows のみ |
| Rename-LocalUser        | &check; |      |       |       | Windows のみ |
| Set-LocalGroup          | &check; |      |       |       | Windows のみ |
| Set-LocalUser           | &check; |      |       |       | Windows のみ |

### <a name="microsoftpowershellmanagement"></a>Microsoft.PowerShell.Management

|          コマンドレット名          |   5.1   |   6.x   |   7.0   |   7.1   |               Note               |
| ----------------------------- | :-----: | :-----: | :-----: | :-----: | -------------------------------- |
| Add-Computer                  | &check; |         |         |         | Windows のみ                     |
| Add-Content                   | &check; | &check; | &check; | &check; |                                  |
| Checkpoint-Computer           | &check; |         |         |         | Windows のみ                     |
| Clear-Content                 | &check; | &check; | &check; | &check; |                                  |
| Clear-EventLog                | &check; |         |         |         | Windows のみ                     |
| Clear-Item                    | &check; | &check; | &check; | &check; |                                  |
| Clear-ItemProperty            | &check; | &check; | &check; | &check; |                                  |
| Clear-RecycleBin              | &check; |         | &check; | &check; | Windows のみ                     |
| Complete-Transaction          | &check; |         |         |         | Windows のみ                     |
| Convert-Path                  | &check; | &check; | &check; | &check; |                                  |
| Copy-Item                     | &check; | &check; | &check; | &check; |                                  |
| Copy-ItemProperty             | &check; | &check; | &check; | &check; |                                  |
| Debug-Process                 | &check; | &check; | &check; | &check; |                                  |
| Disable-ComputerRestore       | &check; |         |         |         | Windows のみ                     |
| Enable-ComputerRestore        | &check; |         |         |         | Windows のみ                     |
| Get-ChildItem                 | &check; | &check; | &check; | &check; |                                  |
| Get-Clipboard                 | &check; |         | &check; | &check; | macOS ではサポートされていません           |
| Get-ComputerInfo              | &check; | &check; | &check; | &check; | Windows のみ                     |
| Get-ComputerRestorePoint      | &check; |         |         |         | Windows のみ                     |
| Get-Content                   | &check; | &check; | &check; | &check; |                                  |
| Get-ControlPanelItem          | &check; |         |         |         | Windows のみ                     |
| Get-EventLog                  | &check; |         |         |         | Windows のみ                     |
| Get-HotFix                    | &check; |         | &check; | &check; | Windows のみ                     |
| Get-Item                      | &check; | &check; | &check; | &check; |                                  |
| Get-ItemProperty              | &check; | &check; | &check; | &check; |                                  |
| Get-ItemPropertyValue         | &check; | &check; | &check; | &check; |                                  |
| Get-Location                  | &check; | &check; | &check; | &check; |                                  |
| Get-Process                   | &check; | &check; | &check; | &check; |                                  |
| Get-PSDrive                   | &check; | &check; | &check; | &check; |                                  |
| Get-PSProvider                | &check; | &check; | &check; | &check; |                                  |
| Get-Service                   | &check; | &check; | &check; | &check; | Windows のみ                     |
| Get-TimeZone                  | &check; | &check; | &check; | &check; | Windows のみ                     |
| Get-Transaction               | &check; |         |         |         | Windows のみ                     |
| Get-WmiObject                 | &check; |         |         |         | Windows のみ                     |
| Invoke-Item                   | &check; | &check; | &check; | &check; |                                  |
| Invoke-WmiMethod              | &check; |         |         |         | Windows のみ                     |
| Join-Path                     | &check; | &check; | &check; | &check; |                                  |
| Limit-EventLog                | &check; |         |         |         | Windows のみ                     |
| Move-Item                     | &check; | &check; | &check; | &check; |                                  |
| Move-ItemProperty             | &check; | &check; | &check; | &check; |                                  |
| New-EventLog                  | &check; |         |         |         | Windows のみ                     |
| New-Item                      | &check; | &check; | &check; | &check; |                                  |
| New-ItemProperty              | &check; | &check; | &check; | &check; |                                  |
| New-PSDrive                   | &check; | &check; | &check; | &check; |                                  |
| New-Service                   | &check; | &check; | &check; | &check; | Windows のみ                     |
| New-WebServiceProxy           | &check; |         |         |         | Windows のみ                     |
| Pop-Location                  | &check; | &check; | &check; | &check; |                                  |
| Push-Location                 | &check; | &check; | &check; | &check; |                                  |
| Register-WmiEvent             | &check; |         |         |         | Windows のみ                     |
| Remove-Computer               | &check; |         |         |         | Windows のみ                     |
| Remove-EventLog               | &check; |         |         |         | Windows のみ                     |
| Remove-Item                   | &check; | &check; | &check; | &check; |                                  |
| Remove-ItemProperty           | &check; | &check; | &check; | &check; |                                  |
| Remove-PSDrive                | &check; | &check; | &check; | &check; |                                  |
| Remove-Service                |         | &check; | &check; | &check; | Windows のみ                     |
| Remove-WmiObject              | &check; |         |         |         | Windows のみ                     |
| Rename-Computer               | &check; | &check; | &check; | &check; | Windows のみ                     |
| Rename-Item                   | &check; | &check; | &check; | &check; |                                  |
| Rename-ItemProperty           | &check; | &check; | &check; | &check; |                                  |
| Reset-ComputerMachinePassword | &check; |         |         |         | Windows のみ                     |
| Resolve-Path                  | &check; | &check; | &check; | &check; |                                  |
| Restart-Computer              | &check; | &check; | &check; | &check; | 7\.1 で Linux と macOS のサポートが追加 |
| Restart-Service               | &check; | &check; | &check; | &check; | Windows のみ                     |
| Restore-Computer              | &check; |         |         |         | Windows のみ                     |
| Resume-Service                | &check; | &check; | &check; | &check; | Windows のみ                     |
| Set-Clipboard                 | &check; |         | &check; | &check; |                                  |
| Set-Content                   | &check; | &check; | &check; | &check; |                                  |
| Set-Item                      | &check; | &check; | &check; | &check; |                                  |
| Set-ItemProperty              | &check; | &check; | &check; | &check; |                                  |
| Set-Location                  | &check; | &check; | &check; | &check; |                                  |
| Set-Service                   | &check; | &check; | &check; | &check; | Windows のみ                     |
| Set-TimeZone                  | &check; | &check; | &check; | &check; | Windows のみ                     |
| Set-WmiInstance               | &check; |         |         |         | Windows のみ                     |
| Show-ControlPanelItem         | &check; |         |         |         | Windows のみ                     |
| Show-EventLog                 | &check; |         |         |         | Windows のみ                     |
| Split-Path                    | &check; | &check; | &check; | &check; |                                  |
| Start-Process                 | &check; | &check; | &check; | &check; |                                  |
| Start-Service                 | &check; | &check; | &check; | &check; | Windows のみ                     |
| Start-Transaction             | &check; |         |         |         | Windows のみ                     |
| Stop-Computer                 | &check; | &check; | &check; | &check; | 7\.1 で Linux と macOS のサポートが追加 |
| Stop-Process                  | &check; | &check; | &check; | &check; |                                  |
| Stop-Service                  | &check; | &check; | &check; | &check; | Windows のみ                     |
| Suspend-Service               | &check; | &check; | &check; | &check; | Windows のみ                     |
| Test-ComputerSecureChannel    | &check; |         |         |         | Windows のみ                     |
| Test-Connection               | &check; | &check; | &check; | &check; |                                  |
| Test-Path                     | &check; | &check; | &check; | &check; |                                  |
| Undo-Transaction              | &check; |         |         |         | Windows のみ                     |
| Use-Transaction               | &check; |         |         |         | Windows のみ                     |
| Wait-Process                  | &check; | &check; | &check; | &check; | Linux/macOS では機能しません     |
| Write-EventLog                | &check; |         |         |         | Windows のみ                     |

### <a name="microsoftpowershellodatautils"></a>Microsoft.PowerShell.ODataUtils

|        コマンドレット名        |   5.1   | 6.x  |  7.0  |  7.1  |     Note     |
| ------------------------- | :-----: | :--- | :---: | :---: | ------------ |
| Export-ODataEndpointProxy | &check; |      |       |       | Windows のみ |

### <a name="microsoftpowershelloperationvalidation"></a>Microsoft.PowerShell.Operation.Validation

|        コマンドレット名         |   5.1   | 6.x  |  7.0  |  7.1  |     Note     |
| -------------------------- | :-----: | :--- | :---: | :---: | ------------ |
| Get-OperationValidation    | &check; |      |       |       | Windows のみ |
| Invoke-OperationValidation | &check; |      |       |       | Windows のみ |

### <a name="microsoftpowershellsecurity"></a>Microsoft.PowerShell.Security

|        コマンドレット名        |   5.1   |   6.x   |   7.0   |   7.1   |                  Note                   |
| ------------------------- | :-----: | :-----: | :-----: | :-----: | --------------------------------------- |
| ConvertFrom-SecureString  | &check; | &check; | &check; | &check; |                                         |
| ConvertTo-SecureString    | &check; | &check; | &check; | &check; |                                         |
| Get-Acl                   | &check; | &check; | &check; | &check; | Windows のみ                            |
| Get-AuthenticodeSignature | &check; | &check; | &check; | &check; | Windows のみ                            |
| Get-CmsMessage            | &check; | &check; | &check; | &check; | 7\.1 で Linux/macOS のサポート追加    |
| Get-Credential            | &check; | &check; | &check; | &check; |                                         |
| Get-ExecutionPolicy       | &check; | &check; | &check; | &check; | Linux/macOS では **[無制限]** が返されます |
| Get-PfxCertificate        | &check; | &check; | &check; | &check; |                                         |
| New-FileCatalog           | &check; | &check; | &check; | &check; | Windows のみ                            |
| Protect-CmsMessage        | &check; | &check; | &check; | &check; | 7\.1 で Linux/macOS のサポート追加    |
| Set-Acl                   | &check; | &check; | &check; | &check; | Windows のみ                            |
| Set-AuthenticodeSignature | &check; | &check; | &check; | &check; | Windows のみ                            |
| Set-ExecutionPolicy       | &check; | &check; | &check; | &check; | Linux/macOS では何も起きません             |
| Test-FileCatalog          | &check; | &check; | &check; | &check; | Windows のみ                            |
| Unprotect-CmsMessage      | &check; | &check; | &check; | &check; | 7\.1 で Linux/macOS のサポート追加    |

### <a name="microsoftpowershellutility"></a>Microsoft.PowerShell.Utility

|        コマンドレット名        |   5.1   |   6.x   |   7.0   |   7.1   |                   Note                    |
| ------------------------- | :-----: | :-----: | :-----: | :-----: | ----------------------------------------- |
| Add-Member                | &check; | &check; | &check; | &check; |                                           |
| Add-Type                  | &check; | &check; | &check; | &check; |                                           |
| Clear-Variable            | &check; | &check; | &check; | &check; |                                           |
| Compare-Object            | &check; | &check; | &check; | &check; |                                           |
| ConvertFrom-Csv           | &check; | &check; | &check; | &check; |                                           |
| ConvertFrom-Json          | &check; | &check; | &check; | &check; |                                           |
| ConvertFrom-Markdown      |         |   6.1   | &check; | &check; |                                           |
| ConvertFrom-SddlString    | &check; | &check; | &check; | &check; | Windows のみ                              |
| ConvertFrom-String        | &check; |         |         |         |                                           |
| ConvertFrom-StringData    | &check; | &check; | &check; | &check; |                                           |
| Convert-String            | &check; |         |         |         |                                           |
| ConvertTo-Csv             | &check; | &check; | &check; | &check; |                                           |
| ConvertTo-Html            | &check; | &check; | &check; | &check; |                                           |
| ConvertTo-Json            | &check; | &check; | &check; | &check; |                                           |
| ConvertTo-Xml             | &check; | &check; | &check; | &check; |                                           |
| Debug-Runspace            | &check; | &check; | &check; | &check; |                                           |
| Disable-PSBreakpoint      | &check; | &check; | &check; | &check; |                                           |
| Disable-RunspaceDebug     | &check; | &check; | &check; | &check; |                                           |
| Enable-PSBreakpoint       | &check; | &check; | &check; | &check; |                                           |
| Enable-RunspaceDebug      | &check; | &check; | &check; | &check; |                                           |
| Export-Alias              | &check; | &check; | &check; | &check; |                                           |
| Export-Clixml             | &check; | &check; | &check; | &check; |                                           |
| Export-Csv                | &check; | &check; | &check; | &check; |                                           |
| Export-FormatData         | &check; | &check; | &check; | &check; |                                           |
| Export-PSSession          | &check; | &check; | &check; | &check; |                                           |
| Format-Custom             | &check; | &check; | &check; | &check; |                                           |
| Format-Hex                | &check; | &check; | &check; | &check; |                                           |
| Format-List               | &check; | &check; | &check; | &check; |                                           |
| Format-Table              | &check; | &check; | &check; | &check; |                                           |
| Format-Wide               | &check; | &check; | &check; | &check; |                                           |
| Get-Alias                 | &check; | &check; | &check; | &check; |                                           |
| Get-Culture               | &check; | &check; | &check; | &check; |                                           |
| Get-Date                  | &check; | &check; | &check; | &check; |                                           |
| Get-Error                 |         |         | &check; | &check; |                                           |
| Get-Event                 | &check; | &check; | &check; | &check; | Linux/macOS で使用可能なイベント ソースはありません |
| Get-EventSubscriber       | &check; | &check; | &check; | &check; |                                           |
| Get-FileHash              | &check; | &check; | &check; | &check; |                                           |
| Get-FormatData            | &check; | &check; | &check; | &check; |                                           |
| Get-Host                  | &check; | &check; | &check; | &check; |                                           |
| Get-MarkdownOption        |         |   6.1   | &check; | &check; |                                           |
| Get-Member                | &check; | &check; | &check; | &check; |                                           |
| Get-PSBreakpoint          | &check; | &check; | &check; | &check; |                                           |
| Get-PSCallStack           | &check; | &check; | &check; | &check; |                                           |
| Get-Random                | &check; | &check; | &check; | &check; |                                           |
| Get-Runspace              | &check; | &check; | &check; | &check; |                                           |
| Get-RunspaceDebug         | &check; | &check; | &check; | &check; |                                           |
| Get-TraceSource           | &check; | &check; | &check; | &check; |                                           |
| Get-TypeData              | &check; | &check; | &check; | &check; |                                           |
| Get-UICulture             | &check; | &check; | &check; | &check; |                                           |
| Get-Unique                | &check; | &check; | &check; | &check; |                                           |
| Get-Uptime                |         | &check; | &check; | &check; |                                           |
| Get-Variable              | &check; | &check; | &check; | &check; |                                           |
| Get-Verb                  |         | &check; | &check; | &check; | Microsoft.PowerShelll.Core から移動しました     |
| Group-Object              | &check; | &check; | &check; | &check; |                                           |
| Import-Alias              | &check; | &check; | &check; | &check; |                                           |
| Import-Clixml             | &check; | &check; | &check; | &check; |                                           |
| Import-Csv                | &check; | &check; | &check; | &check; |                                           |
| Import-LocalizedData      | &check; | &check; | &check; | &check; |                                           |
| Import-PowerShellDataFile | &check; | &check; | &check; | &check; |                                           |
| Import-PSSession          | &check; | &check; | &check; | &check; |                                           |
| Invoke-Expression         | &check; | &check; | &check; | &check; |                                           |
| Invoke-RestMethod         | &check; | &check; | &check; | &check; |                                           |
| Invoke-WebRequest         | &check; | &check; | &check; | &check; |                                           |
| Join-String               |         | &check; | &check; | &check; |                                           |
| Measure-Command           | &check; | &check; | &check; | &check; |                                           |
| Measure-Object            | &check; | &check; | &check; | &check; |                                           |
| New-Alias                 | &check; | &check; | &check; | &check; |                                           |
| New-Event                 | &check; | &check; | &check; | &check; | Linux/macOS で使用可能なイベント ソースはありません |
| New-Guid                  | &check; | &check; | &check; | &check; |                                           |
| New-Object                | &check; | &check; | &check; | &check; |                                           |
| New-TemporaryFile         | &check; | &check; | &check; | &check; |                                           |
| New-TimeSpan              | &check; | &check; | &check; | &check; |                                           |
| New-Variable              | &check; | &check; | &check; | &check; |                                           |
| Out-File                  | &check; | &check; | &check; | &check; |                                           |
| Out-GridView              | &check; |         | &check; | &check; | Windows のみ                              |
| Out-Printer               | &check; |         | &check; | &check; | Windows のみ                              |
| Out-String                | &check; | &check; | &check; | &check; |                                           |
| Read-Host                 | &check; | &check; | &check; | &check; |                                           |
| Register-EngineEvent      | &check; | &check; | &check; | &check; | Linux/macOS で使用可能なイベント ソースはありません |
| Register-ObjectEvent      | &check; | &check; | &check; | &check; |                                           |
| Remove-Alias              |         | &check; | &check; | &check; |                                           |
| Remove-Event              | &check; | &check; | &check; | &check; | Linux/macOS で使用可能なイベント ソースはありません |
| Remove-PSBreakpoint       | &check; | &check; | &check; | &check; |                                           |
| Remove-TypeData           | &check; | &check; | &check; | &check; |                                           |
| Remove-Variable           | &check; | &check; | &check; | &check; |                                           |
| Select-Object             | &check; | &check; | &check; | &check; |                                           |
| Select-String             | &check; | &check; | &check; | &check; |                                           |
| Select-Xml                | &check; | &check; | &check; | &check; |                                           |
| Send-MailMessage          | &check; | &check; | &check; | &check; |                                           |
| Set-Alias                 | &check; | &check; | &check; | &check; |                                           |
| Set-Date                  | &check; | &check; | &check; | &check; |                                           |
| Set-MarkdownOption        |         |   6.1   | &check; | &check; |                                           |
| Set-PSBreakpoint          | &check; | &check; | &check; | &check; |                                           |
| Set-TraceSource           | &check; | &check; | &check; | &check; |                                           |
| Set-Variable              | &check; | &check; | &check; | &check; |                                           |
| Show-Command              | &check; |         | &check; | &check; | Windows のみ                              |
| Show-Markdown             |         |   6.1   | &check; | &check; |                                           |
| Sort-Object               | &check; | &check; | &check; | &check; |                                           |
| Start-Sleep               | &check; | &check; | &check; | &check; |                                           |
| Tee-Object                | &check; | &check; | &check; | &check; |                                           |
| Test-Json                 |         | &check; | &check; | &check; |                                           |
| Trace-Command             | &check; | &check; | &check; | &check; |                                           |
| Unblock-File              | &check; | &check; | &check; | &check; | 7\.0 で macOS サポートが追加されました            |
| Unregister-Event          | &check; | &check; | &check; | &check; | Linux/macOS で使用可能なイベント ソースはありません |
| Update-FormatData         | &check; | &check; | &check; | &check; |                                           |
| Update-List               | &check; |         | &check; | &check; |                                           |
| Update-TypeData           | &check; | &check; | &check; | &check; |                                           |
| Wait-Debugger             | &check; | &check; | &check; | &check; |                                           |
| Wait-Event                | &check; | &check; | &check; | &check; |                                           |
| Write-Debug               | &check; | &check; | &check; | &check; |                                           |
| Write-Error               | &check; | &check; | &check; | &check; |                                           |
| Write-Host                | &check; | &check; | &check; | &check; |                                           |
| Write-Information         | &check; | &check; | &check; | &check; |                                           |
| Write-Output              | &check; | &check; | &check; | &check; |                                           |
| Write-Progress            | &check; | &check; | &check; | &check; |                                           |
| Write-Verbose             | &check; | &check; | &check; | &check; |                                           |
| Write-Warning             | &check; | &check; | &check; | &check; |                                           |

### <a name="microsoftwsmanmanagement"></a>Microsoft.WsMan.Management

|      コマンドレット名       |   5.1   |   6.x   |   7.0   |   7.1   |     Note     |
| ---------------------- | :-----: | :-----: | :-----: | :-----: | ------------ |
| Connect-WSMan          | &check; | &check; | &check; | &check; | Windows のみ |
| Disable-WSManCredSSP   | &check; | &check; | &check; | &check; | Windows のみ |
| Disconnect-WSMan       | &check; | &check; | &check; | &check; | Windows のみ |
| Enable-WSManCredSSP    | &check; | &check; | &check; | &check; | Windows のみ |
| Get-WSManCredSSP       | &check; | &check; | &check; | &check; | Windows のみ |
| Get-WSManInstance      | &check; | &check; | &check; | &check; | Windows のみ |
| Invoke-WSManAction     | &check; | &check; | &check; | &check; | Windows のみ |
| New-WSManInstance      | &check; | &check; | &check; | &check; | Windows のみ |
| New-WSManSessionOption | &check; | &check; | &check; | &check; | Windows のみ |
| Remove-WSManInstance   | &check; | &check; | &check; | &check; | Windows のみ |
| Set-WSManInstance      | &check; | &check; | &check; | &check; | Windows のみ |
| Set-WSManQuickConfig   | &check; | &check; | &check; | &check; | Windows のみ |
| Test-WSMan             | &check; | &check; | &check; | &check; | Windows のみ |

### <a name="packagemanagement"></a>PackageManagement

|       コマンドレット名        |   5.1   |   6.x   |   7.0   |   7.1   | Note |
| ------------------------ | :-----: | :-----: | :-----: | :-----: | ---- |
| Find-Package             | &check; | &check; | &check; | &check; |      |
| Find-PackageProvider     | &check; | &check; | &check; | &check; |      |
| Get-Package              | &check; | &check; | &check; | &check; |      |
| Get-PackageProvider      | &check; | &check; | &check; | &check; |      |
| Get-PackageSource        | &check; | &check; | &check; | &check; |      |
| Import-PackageProvider   | &check; | &check; | &check; | &check; |      |
| Install-Package          | &check; | &check; | &check; | &check; |      |
| Install-PackageProvider  | &check; | &check; | &check; | &check; |      |
| Register-PackageSource   | &check; | &check; | &check; | &check; |      |
| Save-Package             | &check; | &check; | &check; | &check; |      |
| Set-PackageSource        | &check; | &check; | &check; | &check; |      |
| Uninstall-Package        | &check; | &check; | &check; | &check; |      |
| Unregister-PackageSource | &check; | &check; | &check; | &check; |      |

### <a name="powershellget"></a>PowershellGet

|           コマンドレット名           |   5.1   |   6.x   |   7.0   |   7.1   | Note |
| ------------------------------- | :-----: | :-----: | :-----: | :-----: | ---- |
| Find-Command                    | &check; | &check; | &check; | &check; |      |
| Find-DscResource                | &check; | &check; | &check; | &check; |      |
| Find-Module                     | &check; | &check; | &check; | &check; |      |
| Find-RoleCapability             | &check; | &check; | &check; | &check; |      |
| Find-Script                     | &check; | &check; | &check; | &check; |      |
| Get-CredsFromCredentialProvider |         | &check; | &check; | &check; |      |
| Get-InstalledModule             | &check; | &check; | &check; | &check; |      |
| Get-InstalledScript             | &check; | &check; | &check; | &check; |      |
| Get-PSRepository                | &check; | &check; | &check; | &check; |      |
| Install-Module                  | &check; | &check; | &check; | &check; |      |
| Install-Script                  | &check; | &check; | &check; | &check; |      |
| New-ScriptFileInfo              | &check; | &check; | &check; | &check; |      |
| Publish-Module                  | &check; | &check; | &check; | &check; |      |
| Publish-Script                  | &check; | &check; | &check; | &check; |      |
| Register-PSRepository           | &check; | &check; | &check; | &check; |      |
| Save-Module                     | &check; | &check; | &check; | &check; |      |
| Save-Script                     | &check; | &check; | &check; | &check; |      |
| Set-PSRepository                | &check; | &check; | &check; | &check; |      |
| Test-ScriptFileInfo             | &check; | &check; | &check; | &check; |      |
| Uninstall-Module                | &check; | &check; | &check; | &check; |      |
| Uninstall-Script                | &check; | &check; | &check; | &check; |      |
| Unregister-PSRepository         | &check; | &check; | &check; | &check; |      |
| Update-Module                   | &check; | &check; | &check; | &check; |      |
| Update-ModuleManifest           | &check; | &check; | &check; | &check; |      |
| Update-Script                   | &check; | &check; | &check; | &check; |      |
| Update-ScriptFileInfo           | &check; | &check; | &check; | &check; |      |

### <a name="psdesiredstateconfiguration"></a>PSDesiredStateConfiguration

|                コマンドレット名                 |   5.1   |   6.x   |   7.0   |   7.1   |     Note     |
| ------------------------------------------ | :-----: | :-----: | :-----: | :-----: | ------------ |
| Disable-DscDebug                           | &check; |         |         |         | Windows のみ |
| Enable-DscDebug                            | &check; |         |         |         | Windows のみ |
| Get-DscConfiguration                       | &check; |         |         |         | Windows のみ |
| Get-DscConfigurationStatus                 | &check; |         |         |         | Windows のみ |
| Get-DscLocalConfigurationManager           | &check; |         |         |         | Windows のみ |
| Get-DscResource                            | &check; | &check; | &check; | &check; |              |
| Invoke-DscResource                         | &check; |         | &check; | &check; |              |
| New-DSCCheckSum                            | &check; | &check; | &check; | &check; |              |
| Publish-DscConfiguration                   | &check; |         |         |         | Windows のみ |
| Remove-DscConfigurationDocument            | &check; |         |         |         | Windows のみ |
| Restore-DscConfiguration                   | &check; |         |         |         | Windows のみ |
| Set-DscLocalConfigurationManager           | &check; |         |         |         | Windows のみ |
| Start-DscConfiguration                     | &check; |         |         |         | Windows のみ |
| Stop-DscConfiguration                      | &check; |         |         |         | Windows のみ |
| Test-DscConfiguration                      | &check; |         |         |         | Windows のみ |
| Update-DscConfiguration                    | &check; |         |         |         | Windows のみ |

### <a name="psdiagnostics"></a>PSDiagnostics

|         コマンドレット名          |   5.1   |   6.x   |   7.0   |   7.1   |     Note     |
| ---------------------------- | :-----: | :-----: | :-----: | :-----: | ------------ |
| Disable-PSTrace              | &check; |   6.2   | &check; | &check; | Windows のみ |
| Disable-PSWSManCombinedTrace | &check; |   6.2   | &check; | &check; | Windows のみ |
| Disable-WSManTrace           | &check; | &check; | &check; | &check; | Windows のみ |
| Enable-PSTrace               | &check; | &check; | &check; | &check; | Windows のみ |
| Enable-PSWSManCombinedTrace  | &check; | &check; | &check; | &check; | Windows のみ |
| Enable-WSManTrace            | &check; |   6.2   | &check; | &check; | Windows のみ |
| Get-LogProperties            | &check; |   6.2   | &check; | &check; | Windows のみ |
| Set-LogProperties            | &check; |   6.2   | &check; | &check; | Windows のみ |
| Start-Trace                  | &check; |   6.2   | &check; | &check; | Windows のみ |
| Stop-Trace                   | &check; |   6.2   | &check; | &check; | Windows のみ |

### <a name="psreadline"></a>PSReadline

|         コマンドレット名         |   5.1   |   6.x   |   7.0   |   7.1   | Note |
| --------------------------- | :-----: | :-----: | :-----: | :-----: | ---- |
| Get-PSReadlineKeyHandler    | &check; | &check; | &check; | &check; |      |
| Get-PSReadlineOption        | &check; | &check; | &check; | &check; |      |
| PSConsoleHostReadline       | &check; | &check; | &check; | &check; |      |
| Remove-PSReadlineKeyHandler | &check; | &check; | &check; | &check; |      |
| Set-PSReadlineKeyHandler    | &check; | &check; | &check; | &check; |      |
| Set-PSReadlineOption        | &check; | &check; | &check; | &check; |      |

### <a name="psscheduledjob"></a>PSScheduledJob

|       コマンドレット名       |   5.1   | 6.x  |  7.0  |  7.1  |     Note     |
| ----------------------- | :-----: | :--- | :---: | :---: | ------------ |
| Add-JobTrigger          | &check; |      |       |       | Windows のみ |
| Disable-JobTrigger      | &check; |      |       |       | Windows のみ |
| Disable-ScheduledJob    | &check; |      |       |       | Windows のみ |
| Enable-JobTrigger       | &check; |      |       |       | Windows のみ |
| Enable-ScheduledJob     | &check; |      |       |       | Windows のみ |
| Get-JobTrigger          | &check; |      |       |       | Windows のみ |
| Get-ScheduledJob        | &check; |      |       |       | Windows のみ |
| Get-ScheduledJobOption  | &check; |      |       |       | Windows のみ |
| New-JobTrigger          | &check; |      |       |       | Windows のみ |
| New-ScheduledJobOption  | &check; |      |       |       | Windows のみ |
| Register-ScheduledJob   | &check; |      |       |       | Windows のみ |
| Remove-JobTrigger       | &check; |      |       |       | Windows のみ |
| Set-JobTrigger          | &check; |      |       |       | Windows のみ |
| Set-ScheduledJob        | &check; |      |       |       | Windows のみ |
| Set-ScheduledJobOption  | &check; |      |       |       | Windows のみ |
| Unregister-ScheduledJob | &check; |      |       |       | Windows のみ |

### <a name="psworkflow--psworkflowutility"></a>PSWorkflow と PSWorkflowUtility

|          コマンドレット名          |   5.1   | 6.x  |  7.0  |  7.1  |     Note     |
| ----------------------------- | :-----: | :--- | :---: | :---: | ------------ |
| New-PSWorkflowExecutionOption | &check; |      |       |       | Windows のみ |
| New-PSWorkflowSession         | &check; |      |       |       | Windows のみ |
| Invoke-AsWorkflow             | &check; |      |       |       | Windows のみ |

### <a name="threadjob"></a>ThreadJob

|   コマンドレット名   |  5.1  |   6.x   |   7.0   |   7.1   | Note |
| --------------- | :---: | :-----: | :-----: | :-----: | ---- |
| Start-ThreadJob |       | &check; | &check; | &check; | PowerShell 5.1 にインストール可能 |
