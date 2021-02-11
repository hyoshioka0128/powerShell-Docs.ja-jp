---
ms.date: 10/16/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: 構成の適用
description: PowerShell DSC 構成を適用するには、プッシュ モードとプル モードの 2 つの方法があります。
ms.openlocfilehash: 466eb43cd266ceb4ae81cc997a7b338e041f8a15
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661732"
---
# <a name="enacting-configurations"></a>構成の適用

> 適用先: Windows PowerShell 4.0、Windows PowerShell 5.0

PowerShell Desired State Configuration (DSC) 構成を適用するには、プッシュ モードとプル モードの 2 つの方法があります。

## <a name="push-mode"></a>プッシュ モード

![プッシュ モードの概要](media/enactingConfigurations/pushModel.png "プッシュ モードのしくみ")

プッシュ モードは、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを呼び出してターゲット ノードに構成を適用するユーザー アクティビティを示します。

構成を作成し、コンパイルした後、プッシュ モードで適用するには、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを呼び出し、コマンドレットの -Path パラメーターを構成 MOF が配置されているパスに設定します。 たとえば、構成 MOF が `C:\DSC\Configurations\localhost.mof` にある場合は、`Start-DscConfiguration -Path 'C:\DSC\Configurations'` というコマンドを使用してローカル コンピューターに適用します。

> [!NOTE]
> 既定では、DSC はバックグラウンド ジョブとして構成を実行します。 構成を対話的に実行するには、 **Wait** パラメーターを指定して [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) を呼び出します。

## <a name="pull-mode"></a>プル モード

![プル モードの概要](media/enactingConfigurations/pullModel.png "プル モードのしくみ")

プル モードでは、プル クライアントはリモート プル サービスから Desired State Configuration を取得するように構成されます。 同様に、プル サービスは、DSC サービスをホストするようにセットアップされ、プル クライアントに必要な構成とリソースを使用してプロビジョニングされています。 それぞれのプル クライアントには、ノードの構成に対して定期的なコンプライアンス チェックを実行するイベントがスケジュールされています。 イベントが最初にトリガーされたとき、プル クライアント上のローカル構成マネージャー (LCM) によって、LCM で指定された構成を取得するためのプル サービスへの要求が行われます。 プル サービスにその構成が存在し、最初の検証チェックに合格した場合、構成はプル クライアントにダウンロードされ、LCM によって実行されます。

LCM はクライアントが構成に準拠しているかどうかを、LCM の **ConfigurationModeFrequencyMins** プロパティで指定された間隔で定期的にチェックします。 LCM はプル サービスで更新された構成を、LCM の **RefreshModeFrequency** プロパティで指定された間隔で定期的にチェックします。 LCM の構成の詳細については、「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」を参照してください。

プル サービスをホストするための推奨ソリューションは、DSC クラウド サービスである [Azure Automation](https://azure.microsoft.com/services/automation/) です。 これはホスト型ソリューションであり、管理とレポートをグラフィカルに行い、管理を一元化できます。

Windows Server でのプル サービスのセットアップについては、「[DSC Web プル サーバーのセットアップ](pullServer.md)」を参照してください。 ただし、この実装では機能が制限されており、統合の一部を "ユーザー自身で" 行う必要があることに注意してください。

次のトピックでは、プル サービスとクライアントについて説明します。

- [Azure Automation DSC の概要](/azure/automation/automation-dsc-overview)
- [SMB プル サーバーのセットアップ](pullServerSMB.md)
- [Configuring a pull client (プル クライアントの構成)](pullClientConfigID.md)
