---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: PowerShell 4.0 で構成 ID を使用してプル クライアントをセットアップする
description: この記事では、PowerShell 4.0で構成 ID を使用してプル クライアントをセットアップする方法について説明します
ms.openlocfilehash: 2a3d7b79f29030620cddc2b2131cb4432e41e4eb
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92649009"
---
# <a name="set-up-a-pull-client-using-configuration-ids-in-powershell-40"></a>PowerShell 4.0 で構成 ID を使用してプル クライアントをセットアップする

>適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

> [!IMPORTANT]
> プル サーバー (Windows Feature *DSC-Service* ) は、Windows Server のサポート対象のコンポーネントですが、新機能がオファーされる予定はありません。 管理対象のクライアントは、(Windows Server のプル サーバー以降の機能が含まれる) [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) または、[こちら](pullserver.md#community-solutions-for-pull-service)に列挙されているコミュニティ ソリューションのいずれかに切り替えを開始することをお勧めします。

プル クライアントをセットアップする前に、プル サーバーを設定する必要があります。 この順序は必須ではありませんが、トラブルシューティングに役立ち、登録を確実に成功させるのに役立ちます。 プル サーバーをセットアップするには、次のガイドを使用できます。

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)

各ターゲット ノードは、構成やリソースをダウンロードし、さらにその状態を報告するように構成できます。 以下のセクションでは、SMB 共有または HTTP DSC プル サーバーを使用してプル クライアントを構成する方法を説明します。 ノードの LCM が更新されると、構成済みの場所にアクセスして、割り当てられたすべての構成がダウンロードされます。 ノードに必要なリソースが存在しない場合、構成済みの場所から自動的にダウンロードされます。 ノードに[レポート サーバー](reportServer.md)が構成されている場合、操作の状態が報告されます。

## <a name="configure-the-pull-client-lcm"></a>プル クライアント LCM を構成する

以下のいずれかの例を実行すると、 **PullClientConfigID** という名前の新しい出力フォルダーが作成され、そこにメタ構成 MOF ファイルが格納されます。 この場合、メタ構成 MOF ファイルの名前は `localhost.meta.mof` になります。

構成を適用するには、 **Path** をメタ構成 MOF ファイルの場所に設定して **Set-DscLocalConfigurationManager** コマンドレットを呼び出します。 次に例を示します。

```powershell
Set-DSCLocalConfigurationManager –ComputerName localhost –Path .\PullClientConfigId –Verbose.
```

## <a name="configuration-id"></a>構成 ID

以下の例では、LCM の **ConfigurationID** プロパティを、この目的で以前に作成した **Guid** に設定します。 **ConfigurationID** は、LCM がプル サーバーで適切な構成を検索する場合に使用します。 プル サーバー上の構成 MOF ファイルは、`ConfigurationID.mof` という名前にする必要があります。ここで、 *ConfigurationID* はターゲット ノードの LCM の **ConfigurationID** プロパティの値です。 詳細については、[プル サーバーへの構成の発行 (v4/v5)](publishConfigs.md) に関するページを参照してください。

次の例を使って、ランダムな **Guid** を作成できます。

```powershell
[System.Guid]::NewGuid()
```

## <a name="set-up-a-pull-client-to-download-configurations"></a>構成をダウンロードするようにプル クライアントをセットアップする

各クライアントを **プル** モードで構成し、その構成が格納されているプル サーバーの url を指定する必要があります。 これを行うには、必要な情報を備えるようにローカル構成マネージャー (LCM) を構成する必要があります。 LCM を構成するには、 **LocalConfigurationManager** ブロックで特別な種類の構成を作成します。 LCM の構成の詳細については、「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig4.md)」をご覧ください。

## <a name="http-dsc-pull-server"></a>HTTP DSC プル サーバー

プル サーバーが Web サービスとしてセットアップされている場合は、 **DownloadManagerName** を **WebDownloadManager** に設定します。 **WebDownloadManager** では、 **ServerUrl** を **DownloadManagerCustomData** キーに指定する必要があります。 また、次の例のように、 **AllowUnsecureConnection** の値を指定することもできます。 次のスクリプトは、"PullServer" という名前のサーバーから構成をプルするように LCM を構成します。

```powershell
Configuration PullClientConfigId
{
    LocalConfigurationManager
    {
        ConfigurationID = "1C707B86-EF8E-4C29-B7C1-34DA2190AE24";
        RefreshMode = "PULL";
        DownloadManagerName = "WebDownloadManager";
        RebootNodeIfNeeded = $true;
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 30;
        ConfigurationMode = "ApplyAndAutoCorrect";
        DownloadManagerCustomData = @{ServerUrl = "http://PullServer:8080/PSDSCPullServer/PSDSCPullServer.svc"; AllowUnsecureConnection = "TRUE"}
    }
}
PullClientConfigId -Output "."
```

## <a name="smb-share"></a>SMB 共有

プル サーバーが Web サービスではなく SMB ファイル共有としてセットアップされている場合は、 **DownloadManagerName** を **WebDownLoadManager** ではなく **DscFileDownloadManager** に設定します。 **DscFileDownloadManager** の場合は、 **DownloadManagerCustomData** で **SourcePath** プロパティを指定する必要があります。 次のスクリプトは、"CONTOSO-SERVER" という名前のサーバーの "SmbDscShare" という名前の SMB 共有から構成をプルするように LCM を構成します。

```powershell
Configuration PullClientConfigId
{
    LocalConfigurationManager
    {
        ConfigurationID = "1C707B86-EF8E-4C29-B7C1-34DA2190AE24";
        RefreshMode = "PULL";
        DownloadManagerName = "DscFileDownloadManager";
        RebootNodeIfNeeded = $true;
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 30;
        ConfigurationMode = "ApplyAndAutoCorrect";
        DownloadManagerCustomData = @{ServerUrl = "\\CONTOSO-SERVER\SmbDscShare"}
    }
}
PullClientConfigId -Output "."
```

## <a name="next-steps"></a>次の手順

プル クライアントを構成したら、次のガイドを使用して、次の手順を行うことができます。

- [プル サーバーに構成を発行する (v4/v5)](publishConfigs.md)
- [リソースのパッケージ化とプル サーバーへのアップロード (v4)](package-upload-resources.md)

## <a name="see-also"></a>参照

- [DSC Web プル サーバーを設定する](pullServer.md)
- [DSC SMB プル サーバーを設定する](pullServerSMB.md)
