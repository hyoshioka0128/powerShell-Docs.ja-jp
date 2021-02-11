---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: PowerShell 5.0 以降での構成名を使用したプル クライアントのセットアップ
description: この記事では、PowerShell 5.0 以降で構成名を使用してプル クライアントをセットアップする方法について説明します
ms.openlocfilehash: db2b08605dd8bc7e48d9d5153861ce9b36118e21
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92644920"
---
# <a name="set-up-a-pull-client-using-configuration-names-in-powershell-50-and-later"></a>PowerShell 5.0 以降での構成名を使用したプル クライアントのセットアップ

> 適用先:Windows PowerShell 5.0

> [!IMPORTANT]
> プル サーバー (Windows Feature *DSC-Service* ) は、Windows Server のサポート対象のコンポーネントですが、新機能がオファーされる予定はありません。 管理対象のクライアントは、(Windows Server のプル サーバー以降の機能が含まれる) [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) または、[こちら](pullserver.md#community-solutions-for-pull-service)に列挙されているコミュニティ ソリューションのいずれかに切り替えを開始することをお勧めします。

プル クライアントをセットアップする前に、プル サーバーを設定する必要があります。 この順序は必須ではありませんが、トラブルシューティングに役立ち、登録を確実に成功させるのに役立ちます。 プル サーバーをセットアップするには、次のガイドを使用できます。

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)

各ターゲット ノードは、構成やリソースをダウンロードし、さらにその状態を報告するように構成できます。 以下のセクションでは、SMB 共有または HTTP DSC プル サーバーを使用してプル クライアントを構成する方法を説明します。 ノードの LCM が更新されると、構成済みの場所にアクセスして、割り当てられたすべての構成がダウンロードされます。 ノードに必要なリソースが存在しない場合、構成済みの場所から自動的にダウンロードされます。 ノードに[レポート サーバー](reportServer.md)が構成されている場合、操作の状態が報告されます。

> [!NOTE]
> このトピックは、PowerShell 5.0 に適用されます。 PowerShell 4.0 でのプル クライアントのセットアップについては、「[PowerShell 4.0 での構成 ID を使用したプル クライアントのセットアップ](pullClientConfigID4.md)」をご覧ください。

## <a name="configure-the-pull-client-lcm"></a>プル クライアント LCM を構成する

以下のいずれかの例を実行すると、 **PullClientConfigName** という名前の新しい出力フォルダーが作成され、そこにメタ構成 MOF ファイルが格納されます。 この場合、メタ構成 MOF ファイルの名前は `localhost.meta.mof` になります。

構成を適用するには、 **Path** をメタ構成 MOF ファイルの場所に設定して **Set-DscLocalConfigurationManager** コマンドレットを呼び出します。 次に例を示します。

```powershell
Set-DSCLocalConfigurationManager –ComputerName localhost –Path .\PullClientConfigName –Verbose.
```

## <a name="configuration-name"></a>構成名

下の例では、この目的で作成され、前にコンパイルしてある Configuration の名前に LCM の **ConfigurationName** プロパティが設定されます。 **ConfigurationName** は、LCM がプル サーバーで適切な構成を検索する場合に使用します。 プル サーバーの構成 MOF ファイルの名前は `<ConfigurationName>.mof` にする必要があります。今回は "ClientConfig.mof" です。 詳細については、[プル サーバーへの構成の発行 (v4/v5)](publishConfigs.md) に関するページを参照してください。

## <a name="set-up-a-pull-client-to-download-configurations"></a>構成をダウンロードするようにプル クライアントをセットアップする

各クライアントを **プル** モードで構成し、その構成が格納されているプル サーバーの url を指定する必要があります。 これを行うには、必要な情報を備えるようにローカル構成マネージャー (LCM) を構成する必要があります。 LCM を構成するには、 **DSCLocalConfigurationManager** 属性で修飾された特別な種類の構成を作成します。 LCM の構成の詳細については、「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」をご覧ください。

次のスクリプトは、"CONTOSO-PullSrv" という名前のサーバーから構成をプルするように LCM を構成します。

- このスクリプトでは、 **ConfigurationRepositoryWeb** ブロックでプル サーバーを定義しています。 **ServerURL** プロパティには、プル サーバー用のエンドポイントを指定します。

- **RegistrationKey** プロパティは、プル サーバーのすべてのクライアント ノードとそのプル サーバーとの間で共有されるキーです。 同じ値がプル サーバー上のファイルに格納されます。 > [!NOTE] > 登録キーは、 **Web** プル サーバーでのみ動作します。 **SMB** プル サーバーでは、引き続き **ConfigurationID** を使用する必要があります。 > **ConfigurationID** を使用したプル サーバーの構成については、「[構成 ID を使用したプル クライアントのセットアップ](pullClientConfigId.md)」を参照してください

- **ConfigurationNames** プロパティは、クライアント ノード用の構成の名前を指定する配列です。 > **"注:** **ConfigurationNames** に複数の値を指定する場合、構成に **PartialConfiguration** ブロックも指定する必要があります。 >部分構成の詳細については、「[PowerShell Desired State Configuration の部分構成](partialConfigs.md)」を参照してください。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigNames
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
            ConfigurationNames = @('ClientConfig')
        }
    }
}
PullClientConfigNames
```

## <a name="set-up-a-pull-client-to-download-resources"></a>リソースをダウンロードするようにプル クライアントをセットアップする

LCM 構成で **ConfigurationRepositoryWeb** ブロックまたは **ConfigurationRepositoryShare** ブロックのみを指定した場合 (前の例はこれに当たります)、プル クライアントは、".mof" ファイルが保存されている同じ場所からリソースをプルします。 クライアントがリソースをダウンロードできる別の場所を指定することもできます。 リソース サーバーを指定するには、 **ResourceRepositoryWeb** (Web プル サーバーの場合) または **ResourceRepositoryShare** ブロック (SMB プル サーバーの場合) を使用します。

次の例は、プル サーバーから構成を、SMB 共有からリソースをダウンロードするようにクライアントをセットアップするメタ構成を示しています。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigNames
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = 'fbc6ef09-ad98-4aad-a062-92b0e0327562'
        }

        ResourceRepositoryShare SMBResources
        {
            SourcePath = '\\SMBPullServer\Resources'
        }
    }
}
PullClientConfigNames
```

## <a name="set-up-a-pull-client-to-report-status"></a>状態を報告するようにプル クライアントをセットアップする

構成、リソース、レポートについて単一のプル サーバーを使用できます。 レポートはクライアントに対して既定では構成されません。 状態を報告するようにクライアントを構成するには、 **ReportRepositoryWeb** ブロックを作成する必要があります。 次の例は、単一のプル サーバーに対して構成とリソースをプルし、レポート データを送信するようにクライアントを設定するメタ構成を示しています。

> [!NOTE]
> レポート サーバーを SMB 共有にすることはできません。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigNames
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = 'fbc6ef09-ad98-4aad-a062-92b0e0327562'
        }

        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = 'fbc6ef09-ad98-4aad-a062-92b0e0327562'
        }
    }
}
PullClientConfigNames
```

## <a name="see-also"></a>参照

- [構成 ID を使用したプル クライアントのセットアップ](PullClientConfigNames.md)
- [DSC Web プル サーバーのセットアップ](pullServer.md)
