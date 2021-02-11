---
ms.date: 06/12/2017
title: Azure Automation にデプロイする
description: この記事では、PowerShell ギャラリーを使用して Azure Automation にパッケージをデプロイする方法について説明します。
ms.openlocfilehash: e9de079ee6cc950c8a268423b9eabd515959b718
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662365"
---
# <a name="deploy-to-azure-automation"></a>Azure Automation にデプロイする

パッケージの詳細ページの [Azure Automation にデプロイする] ボタンでは、PowerShell ギャラリーから Azure Automation にパッケージがデプロイされます。

![[Azure Automation にデプロイする] ボタン](media/deploy-to-azure-automation/DeployToAzureAutomationButton.png)

クリックすると Azure 管理ポータルにリダイレクトされるため、そこで Azure アカウント資格情報を使用してサインインします。 パッケージに依存関係がある場合、すべての依存関係も Azure Automation にデプロイされます。

> [!WARNING]
> お使いの Automation アカウントに既に同じパッケージとバージョンがある場合、PowerShell ギャラリーからこれを再度デプロイすると、Automation アカウントのパッケージが上書きされます。

モジュールをデプロイする場合は、[Azure Automation] の [モジュール] セクションに表示されます。 スクリプトをデプロイする場合は、[Azure Automation] の [Runbooks] セクションに表示されます。

[Azure Automation にデプロイする] ボタンは、AzureAutomationNotSupported タグをパッケージのメタデータに追加すると無効にできます。

## <a name="require-license-acceptance-on-deploy-to-azure-automation"></a>Azure Automation へのデプロイに表示されるライセンス同意リクエスト

Azure Automation へデプロイするモジュールでライセンスへの同意が必要な場合、ポータルの UI に、"このモジュールには、ライセンスの同意が必要です。 [OK] をクリックすると、ライセンス条項に同意します。" という警告が表示されます。

![ライセンスへの同意が必要な Azure Automation へのデプロイ](media/deploy-to-azure-automation/DeployToAzureAutomationRequireLicenseAcceptanceDisclaimer.png)

## <a name="more-details"></a>詳細

- [PowerShellGet でのライセンス同意リクエスト](../../concepts/module-license-acceptance.md)
- [PowerShell ギャラリーでのライセンス同意リクエスト](packages-that-require-license-acceptance.md)
- [Azure Automation の Web サイト](https://azure.microsoft.com/services/automation/)
