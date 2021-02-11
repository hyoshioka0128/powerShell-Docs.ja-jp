---
ms.date: 06/12/2017
title: PowerShell ギャラリーの概要
description: この記事では、PowerShell ギャラリーと PowerShellGet コマンドレットの使用を開始する方法について説明します
ms.openlocfilehash: a5045e4ecee2eb8bc9aa373a6b8ec3a940b4de33
ms.sourcegitcommit: 560a9f3c3148acab4655e91e8b07745ab74d5d26
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "96913282"
---
# <a name="getting-started-with-the-powershell-gallery"></a>PowerShell ギャラリーの概要

PowerShell ギャラリーは、ダウンロードして利用できるスクリプト、モジュール、DSC リソースが含まれるパッケージ リポジトリです。 PowerShell ギャラリーからパッケージをインストールするには、[PowerShellGet](/powershell/module/powershellget) モジュールのコマンドレットを使います。 PowerShell ギャラリーから項目をダウンロードするのにサインインする必要はありません。

> [!NOTE]
> PowerShell ギャラリーから直接パッケージをダウンロードすることもできますが、この方法はお勧めできません。 詳細については、「[パッケージの手動ダウンロード](how-to/working-with-packages/manual-download.md)」を参照してください。

## <a name="discovering-packages-from-the-powershell-gallery"></a>PowerShell ギャラリーでのパッケージの検出

PowerShell ギャラリーの [ホーム ページ](https://www.powershellgallery.com)の **[検索]** コントロールを使用して、または [[パッケージ] ページ](https://www.powershellgallery.com/packages)からモジュールとスクリプトを参照することによって、PowerShell ギャラリー内のパッケージを検索できます。 また、[Find-Module][]、[Find-DscResource]、[Find-Script][] コマンドレットを、パッケージの種類に応じて `-Repository PSGallery` を指定して実行すると、PowerShell ギャラリーからパッケージを検索できます。

次のパラメーターを使用して、ギャラリーからの結果をフィルター処理できます。

- 名前
- AllVersions
- MinimumVersion
- RequiredVersion
- タグ
- Includes
- DscResource
- RoleCapability
- コマンド
- Assert

ギャラリー内の特定の DSC リソースのみを検出したい場合は、[Find-DscResource][] コマンドレットを実行します。 Find-DscResource では、ギャラリーに含まれている DSC リソースのデータが返されます。 DSC リソースは常にモジュールの一部として配布されるため、この DSC リソースをインストールする場合も [Install-Module][] を実行する必要があります。

## <a name="learning-about-packages-in-the-powershell-gallery"></a>PowerShell ギャラリーのパッケージの詳細

関心のあるパッケージを特定できたら、その詳細を入手することができます。 これは、ギャラリー上でパッケージの特定のページを調べると見つかります。 そのページでは、そのパッケージと共にアップロードされているメタデータをすべて参照することができます。 このメタデータはパッケージの作成者が提供するものであり、Microsoft で検証したものではありません。 パッケージの所有者は、パッケージの公開に使用したギャラリーのアカウントと厳密に関連付けられており、[作成者] フィールドよりも信頼性があります。

誠意をもって公開されていると思われないパッケージが見つかった場合は、そのパッケージのページの **[不正使用を報告]** をクリックします。

[Find-Module][] または [Find-Script][] を実行している場合、返される PSGetModuleInfo オブジェクトにこのデータが表示されます。 たとえば、`Find-Module -Name PSReadLine -Repository PSGallery |Get-Member` を実行すると、ギャラリーの PSReadLine モジュールのデータが返されます。

## <a name="downloading-packages-from-the-powershell-gallery"></a>PowerShell ギャラリーでのパッケージのダウンロード

PowerShell ギャラリーからパッケージをダウンロードするとき、次のプロセスをお勧めします。

### <a name="inspect"></a>検査

検査のためにギャラリーからパッケージをダウンロードするには、パッケージの種類に応じて [Save-Module][] または [Save-Script][] コマンドレットを実行します。 これにより、パッケージをインストールすることなくローカルに保存し、パッケージの内容を検査することができます。 保存したパッケージは必ず手動で削除してください。

このパッケージの一部は Microsoft によって作成されており、その他は PowerShell コミュニティによって作成されています。 このギャラリーのパッケージの内容とコードは、インストールする前に確認することをお勧めします。

誠意をもって公開されていると思われないパッケージが見つかった場合は、そのパッケージのページの **[不正使用を報告]** をクリックします。

### <a name="install"></a>インストール

使用のためにギャラリーからパッケージをインストールするには、パッケージの種類に応じて [Install-Module][] または [Install-Script][] コマンドレットを実行します。

[Install-module][] は、既定では `$env:ProgramFiles\WindowsPowerShell\Modules` にモジュールをインストールします。
これには管理者アカウントが必要です。 `-Scope CurrentUser` パラメーターを追加する場合は、モジュールは `$env:USERPROFILE\Documents\WindowsPowerShell\Modules` にインストールされます。

[Install-script][] は、既定では、 `$env:ProgramFiles\WindowsPowerShell\Scripts` にスクリプトをインストールします。
これには管理者アカウントが必要です。 `-Scope CurrentUser` パラメーターを追加する場合は、スクリプトは `$env:USERPROFILE\Documents\WindowsPowerShell\Scripts` にインストールされます。

既定では、[Install-Module][] および [Install-Script][] は最新バージョンのパッケージをインストールします。 前のバージョンのパッケージをインストールするには、`-RequiredVersion` パラメーターを追加します。

### <a name="deploy"></a>配置

パッケージを PowerShell ギャラリーから Azure Automation にデプロイするには、**[Azure Automation]** をクリックした後、パッケージの詳細ページで **[Deploy to Azure Automation]\(Azure Automation にデプロイする\)** をクリックします。 Azure 管理ポータルにリダイレクトされるため、そこで Azure アカウント資格情報を使用してサインインします。 依存関係のあるパッケージをデプロイすると、すべての依存関係が Azure Automation にデプロイされることに注意してください。 [Azure Automation にデプロイする] ボタンは、**AzureAutomationNotSupported** タグをパッケージのメタデータに追加すると無効にできます。

Azure Automation の詳細については、[Azure Automation](/azure/automation) のドキュメントをご覧ください。

## <a name="updating-packages-from-the-powershell-gallery"></a>PowerShell ギャラリーからのパッケージの更新

PowerShell ギャラリーからインストールされたパッケージを更新するには、[Update-Module][] または [Update-Script][] コマンドレットを実行します。 パラメーターを追加せずに [Update-Module][] を実行すると、[Install-Module][] を実行してインストールされたすべてのモジュールの更新が試行されます。 モジュールを選択して更新するには、`-Name` パラメーターを追加します。

同様に、パラメーターを追加せずに [Update-Script][] を実行する場合も、[Install-Script][] を実行してインストールされたすべてのスクリプトの更新が試行されます。 スクリプトを選択して更新するには、`-Name` パラメーターを追加します。

## <a name="list-packages-that-you-have-installed-from-the-powershell-gallery"></a>PowerShell ギャラリーからインストールしたパッケージの一覧

PowerShell ギャラリーからどのモジュールをインストールしたかを調べるには、[Get-InstalledModule][] コマンドレットを実行します。 このコマンドは、PowerShell ギャラリーから直接インストールした、システム上にあるモジュールをすべて一覧表示します。

同様に、PowerShell ギャラリーからどのスクリプトをインストールしたかを調べるには、[Get-InstalledScript][] コマンドレットを実行します。 このコマンドは、PowerShell ギャラリーから直接インストールした、システム上にあるスクリプトをすべて一覧表示します。

## <a name="network-access-to-the-powershell-gallery"></a>PowerShell ギャラリーへのネットワーク アクセス

PowerShell ギャラリーでは、次のホスト名が使用されます。

- `psg-prod-eastus.azureedge.net` - CDN ホスト名
- `az818661.vo.msecnd.net` - CDN ホスト名
- `devopsgallerystorage.blob.core.windows.net` - ストレージ アカウントのホスト名
- `*.powershellgallery.com` - Web サイト
- `go.microsoft.com` - リダイレクト サービス

これらのホスト名は、ご利用のネットワークからのアクセスを制御する許可リストに追加する必要があります。

[!INCLUDE [TLS 1.2 Requirement](../../includes/tls-gallery.md)]

[Find-DscResource]: /powershell/module/powershellget/Find-DscResource
[Find-Module]: /powershell/module/powershellget/Find-Module
[Find-Script]: /powershell/module/powershellget/Find-Script
[Get-InstalledModule]: /powershell/module/powershellget/Get-InstalledModule
[Get-InstalledScript]: /powershell/module/powershellget/Get-InstalledScript
[Install-Module]: /powershell/module/powershellget/Install-Module
[Install-Script]: /powershell/module/powershellget/Install-Script
[Publish-Module]: /powershell/module/powershellget/Publish-Module
[Publish-Script]: /powershell/module/powershellget/Publish-Script
[Register-PSRepository]: /powershell/module/powershellget/Register-Repository
[Save-Module]: /powershell/module/powershellget/Save-Module
[Save-Script]: /powershell/module/powershellget/Save-Script
[Update-Module]: /powershell/module/powershellget/Update-Module
[Update-Script]: /powershell/module/powershellget/Update-Script
