---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: PowerShell 5.0 以降での構成 ID を使用したプル クライアントのセットアップ
description: この記事では、PowerShell 5.0 以降で構成 ID を使用してプル クライアントをセットアップする方法について説明します
ms.openlocfilehash: 601858c08ce9a893a8941823d27fef3a60882b48
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92664301"
---
# <a name="set-up-a-pull-client-using-configuration-ids-in-powershell-50-and-later"></a>PowerShell 5.0 以降での構成 ID を使用したプル クライアントのセットアップ

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

以下のいずれかの例を実行すると、 **PullClientConfigID** という名前の新しい出力フォルダーが作成され、そこにメタ構成 MOF ファイルが格納されます。 この場合、メタ構成 MOF ファイルの名前は `localhost.meta.mof` になります。

構成を適用するには、 **Path** をメタ構成 MOF ファイルの場所に設定して **Set-DscLocalConfigurationManager** コマンドレットを呼び出します。 次に例を示します。

```powershell
Set-DSCLocalConfigurationManager –ComputerName localhost –Path .\PullClientConfigId –Verbose.
```

## <a name="configuration-id"></a>構成 ID

以下の例は、LCM の **ConfigurationID** プロパティをこの目的で以前に作成した **Guid** に設定します。 **ConfigurationID** は、LCM がプル サーバーで適切な構成を検索する場合に使用します。 プル サーバー上の構成 MOF ファイルは、`ConfigurationID.mof` という名前にする必要があります。ここで、 *ConfigurationID* はターゲット ノードの LCM の **ConfigurationID** プロパティの値です。 詳細については、[プル サーバーへの構成の発行 (v4/v5)](publishConfigs.md)に関するページを参照してください。

以下の例を使用するか、または [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid) コマンドレットを使用して、ランダムな **Guid** を作成できます。

```powershell
[System.Guid]::NewGuid()
```

環境で **Guid** を使用する詳細については、 [Guid の計画](secureserver.md#guids)に関する項を参照してください。

## <a name="set-up-a-pull-client-to-download-configurations"></a>構成をダウンロードするようにプル クライアントをセットアップする

各クライアントを **プル** モードで構成し、その構成が格納されているプル サーバーの url を指定する必要があります。 これを行うには、必要な情報を備えるようにローカル構成マネージャー (LCM) を構成する必要があります。 LCM を構成するには、 **DSCLocalConfigurationManager** 属性で修飾された特別な種類の構成を作成します。 LCM の構成の詳細については、「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」をご覧ください。

### <a name="http-dsc-pull-server"></a>HTTP DSC プル サーバー

次のスクリプトは、"CONTOSO-PullSrv" という名前のサーバーから構成をプルするように LCM を構成します。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'

        }
    }
}
PullClientConfigID
```

このスクリプトでは、 **ConfigurationRepositoryWeb** ブロックでプル サーバーを定義しています。 **ServerUrl** は DSC プルの url を指定します

### <a name="smb-share"></a>SMB 共有

次のスクリプトは、SMB 共有 `\\SMBPullServer\Pull` から構成をプルするように LCM を構成します。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryShare SMBPullServer
        {
            SourcePath = '\\SMBPullServer\Pull'
        }
    }
}
PullClientConfigID
```

このスクリプトでは、 **ConfigurationRepositoryShare** ブロックでプル サーバーを定義していますが、この例では、単なる SMB 共有です。

## <a name="set-up-a-pull-client-to-download-resources"></a>リソースをダウンロードするようにプル クライアントをセットアップする

LCM 構成で **ConfigurationRepositoryWeb** ブロックまたは **ConfigurationRepositoryShare** ブロックのみを指定した場合 (前の例はこれに当たります)、プル クライアントは、その構成を取得する同じ場所から、リソースをプルします。 リソース用に別の場所を指定することもできます。 リソースの場所を別のサーバーとして指定するには、 **ResourceRepositoryWeb** ブロックを使用します。 リソースの場所を SMB 共有として指定するには、 **ResourceRepositoryShare** ブロックを使用します。

> [!NOTE]
> **ConfigurationRepositoryWeb** を **ResourceRepositoryShare** と、または **ConfigurationRepositoryShare** を **ResourceRepositoryWeb** と組み合わせることができます。 この例は、以下に示していません。

### <a name="http-dsc-pull-server"></a>HTTP DSC プル サーバー

次のメタ構成は、プル クライアントが **CONTOSO-PullSrv** からその構成を取得し、 **CONTOSO-ResourceSrv** からそのリソースを取得するように、プル クライアントを構成します。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'

        }

        ResourceRepositoryWeb CONTOSO-ResourceSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

### <a name="smb-share"></a>SMB 共有

次の例は、SMB 共有 `\\SMBPullServer\Configurations` から構成を、SMB 共有 `\\SMBPullServer\Resources` からリソースをプルするように、クライアントをセットアップするメタ構成を示しています。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryShare SMBPullServer
        {
            SourcePath = '\\SMBPullServer\Configurations'
        }

        ResourceRepositoryShare SMBResourceServer
        {
            SourcePath = '\\SMBPullServer\Resources'
        }
    }
}
PullClientConfigID
```

#### <a name="automatically-download-resources-in-push-mode"></a>プッシュ モードでリソースを自動的にダウンロードする

PowerShell 5.0 以降では、プル クライアントは、 **プッシュ** モードで構成されている場合でも、SMB 共有からモジュールをダウンロードできます。 これは、プル サーバーをセットアップしないシナリオで特に便利です。 **ResourceRepositoryShare** ブロックは、 **ConfigurationRepositoryShare** を指定しなくても使用できます。 次の例に、SMB 共有 `\\SMBPullServer\Resources` からリソースをプルするようにクライアントをセットアップするメタ構成を示します。 ノードに構成が **プッシュ** されると、指定された共有から必要なすべてのリソースが自動的にダウンロードされます。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Push'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
        }

        ResourceRepositoryShare SMBResourceServer
        {
            SourcePath = '\\SMBPullServer\Resources'
        }
    }
}
PullClientConfigID
```

## <a name="set-up-a-pull-client-to-report-status"></a>状態を報告するようにプル クライアントをセットアップする

既定では、ノードは構成済みプル サーバーにレポートを送信しません。 構成、リソース、およびレポートについて単一のプル サーバーを使うことができますが、レポートをセットアップするために **ReportRepositoryWeb** ブロックを作成する必要があります。

### <a name="http-dsc-pull-server"></a>HTTP DSC プル サーバー

次の例は、単一のプル サーバーに対して構成とリソースをプルし、レポート データを送信するようにクライアントを設定するメタ構成を示しています。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }

        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

レポート サーバーを指定するには、 **ReportRepositoryWeb** ブロックを使用します。 レポート サーバーを SMB サーバーにすることはできません。 次のメタ構成は、 **CONTOSO-PullSrv** から構成を取得し、 **CONTOSO-ResourceSrv** からリソースを取得し、 **CONTOSO-ReportSrv** に状態レポートを送信するように、プル クライアントを構成します。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'

        }

        ResourceRepositoryWeb CONTOSO-ResourceSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

### <a name="smb-share"></a>SMB 共有

レポート サーバーを SMB 共有にすることはできません。

## <a name="next-steps"></a>次の手順

プル クライアントを構成したら、次のガイドを使用して、次の手順を行うことができます。

- [プル サーバーに構成を発行する (v4/v5)](publishConfigs.md)
- [リソースのパッケージ化とプル サーバーへのアップロード (v4)](package-upload-resources.md)

## <a name="see-also"></a>参照

- [構成名を使用したプル クライアントのセットアップ](pullClientConfigNames.md)
