---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: プル サーバーからのノードを更新する
description: この記事では、プル サーバーから DSC マネージド ノードを更新する方法について説明します
ms.openlocfilehash: 7256a0e1fdfaa8e56150c4f7299640bc95b82cee
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92656766"
---
# <a name="update-nodes-from-a-pull-server"></a>プル サーバーからのノードを更新する

以下のセクションでは、プル サーバーを既にセットアップしてあるものとします。 プル サーバーをセットアップしていない場合は、次のガイドを使用できます。

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)

各ターゲット ノードは、構成やリソースをダウンロードし、さらにその状態を報告するように構成できます。 この記事では、ダウンロードできるようにリソースをアップロードする方法、およびリソースを自動的にダウンロードするようにクライアントを構成する方法を示します。 ノードは、割り当てられた構成を **プル** または **プッシュ** (v5) によって受け取ると、構成で必要なすべてのリソースを LCM で指定された場所から自動的にダウンロードします。

## <a name="using-the-update-dscconfiguration-cmdlet"></a>Update-DSCConfiguration コマンドレットの使用

PowerShell 5.0 以降では、[Update-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) コマンドレットによって、LCM で構成されているプル サーバーからノードの構成が強制的に更新されます。

```powershell
Update-DSCConfiguration -ComputerName "Server01"
```

## <a name="using-invoke-cimmethod"></a>Invoke-CIMMethod の使用

PowerShell 4.0 ではまだ、[Invoke-CIMMethod](/powershell/module/cimcmdlets/invoke-cimmethod) を使用して手動で強制的にプル クライアントの構成を更新できます。 次の例では、指定した資格情報で CIM セッションを作成し、適切な CIM メソッドを呼び出して、セッションを削除します。

```powershell
$cimSession = New-CimSession -ComputerName "Server01" -Credential $(Get-Credential)
Invoke-CimMethod -CimSession $cimSession -Namespace 'root/microsoft/windows/desiredstateconfiguration' -Class 'MSFT_DscLocalConfigurationManager' -MethodName 'PerformRequiredConfigurationChecks' -Arguments @{ 'Flags' = [uint32]1 } -Verbose
$cimSession | Remove-CimSession
```

## <a name="see-also"></a>参照

[PerformRequiredConfigurationChecks](../reference/mof-classes/msft-dsclocalconfigurationmanager-performrequiredconfigurationchecks.md)
