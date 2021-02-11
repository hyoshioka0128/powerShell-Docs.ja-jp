---
ms.date: 01/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC プル サービス
description: Local Configuration Manager (LCM) は、プル サービス ソリューションで一元管理できます。 この方法を使用する場合、管理対象のノードはサービスに登録され、LCM 設定で構成が割り当てられます。
ms.openlocfilehash: 67d405deda23569964e5eb401a4405a584369430
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92659119"
---
# <a name="desired-state-configuration-pull-service"></a>Desired State Configuration プル サービス

> [!IMPORTANT]
> プル サーバー (Windows Feature *DSC-Service* ) は、Windows Server のサポート対象のコンポーネントですが、新機能がオファーされる予定はありません。 管理対象のクライアントは、(Windows Server のプル サーバー以降の機能が含まれる) [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) または、[こちら](pullserver.md#community-solutions-for-pull-service)に列挙されているコミュニティ ソリューションのいずれかに切り替えを開始することをお勧めします。

Local Configuration Manager (LCM) は、プル サービス ソリューションで一元管理できます。 この方法を使用する場合、管理対象のノードはサービスに登録され、LCM 設定で構成が割り当てられます。 構成の依存関係として必要な構成とすべての DSC リソースは、マシンにダウンロードされ、構成を管理するために LCM によって使用されます。
管理対象のマシンの状態に関する情報は、レポートのためにサービスにアップロードされます。
この概念は "プル サービス" と呼ばれます。

現在選択できるプル サービスは以下のとおりです。

- Azure Automation Desired State Configuration サービス
- Windows Server で実行されるプル サービス
- コミュニティが維持するオープン ソースのソリューション
- SMB 共有

各ソリューションの推奨スケールは次のとおりです。

|                   解決策                   |              クライアント ノード              |
| -------------------------------------------- | -------------------------------------- |
| MDB/ESENT データベースを使用する Windows プル サーバー | 最大 500 ノード                        |
| SQL データベースを使用する Windows プル サーバー       | 最大 3500 ノード                       |
| Azure Automation DSC                         | 小規模環境と大規模環境の両方      |

**推奨されるソリューション** であり、最も多くの機能を使用できる選択肢は [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) です。 Automation アカウントあたりのノード数の上限は指定されていません。

Azure サービスでは、プライベート データセンター内にあるオンプレミス ノードと、パブリック クラウド （Azure や AWS など) 内にあるノードのどちらも管理できます。 インターネットへのサーバーの直接接続が許可されないプライベート環境の場合は、公開されている Azure の IP 範囲 ([Azure データセンターの IP 範囲](https://www.microsoft.com/download/details.aspx?id=41653)に関するページを参照) のみに送信トラフィックを制限することを検討してください。

現時点で Windows Server 上のプル サービスでは利用できないオンライン サービスの機能は以下のとおりです。

- 転送中および保存中のすべてのデータの暗号化
- クライアント証明書の自動作成および管理
- [パスワード/資格情報](/azure/automation/automation-credentials) または[変数](/azure/automation/automation-variables) (サーバー名や接続文字列など) を一元管理するためのシークレット ストア
- [LCM 構成](../managing-nodes/metaConfig.md#basic-settings)ノードの一元管理
- クライアント ノードへ構成を一元的に割り当てる
- 運用環境への適用前に "カナリア グループ" へ構成の変更をリリースしてテストする
- グラフィカル レポート
  - きめ細かな DSC リソース レベルでの状態の詳細
  - クライアント マシンからの詳細なエラー メッセージによるトラブルシューティング
- [Azure Log Analytics との統合](/azure/automation/automation-dsc-diagnostics)によるアラートとタスクの自動化、およびレポートとアラート用の Android/iOS アプリ

## <a name="dsc-pull-service-in-windows-server"></a>Windows Server の DSC プル サービス

Windows Server 上で実行するようにプル サービスを構成することができます。 Windows Server に含まれるプル サービス ソリューションには、ダウンロード用の構成とモジュールを格納する機能と、レポート データをデータベースにキャプチャする機能のみが含まれている点に注意してください。 Azure のサービスで提供される機能の多くは含まれていないため、サービスの使用方法を評価する場合に適したツールではありません。

Windows Server で提供されるプル サービスは、OData インターフェイスを使用してターゲット ノードからの要求に応じて DSC 構成ファイルをそのターゲット ノードで使用できるようにする、IIS 内の Web サービスです。

プル サーバーを使用するための要件:

- 次のものを実行するサーバー。
  - WMF/PowerShell 4.0 以降
  - IIS サーバー ロール
  - DSC サービス
- 理想としては、証明書を生成する何らかの手段。これは、ターゲット ノードのローカル構成マネージャー (LCM) に渡された資格情報をセキュリティ保護するためのものです。

プル サービスをホストするように Windows Server を構成するには、DSC 構成を使用する方法が最適です。 スクリプトの例を以下に示します。

### <a name="supported-database-systems"></a>サポートされているデータベース システム

| WMF 4.0 |       WMF 5.0        |       WMF 5.1        | WMF 5.1 (Windows Server Insider プレビュー 17090) |
| ------- | -------------------- | -------------------- | ---------------------------------------------- |
| MDB     | ESENT (既定)、MDB | ESENT (既定)、MDB | ESENT (既定)、SQL Server、MDB               |

Windows Server のリリース 17090 以降では、SQL Server は、プル サービス (Windows Feature *DSC-Service* ) でサポートされるオプションです。 これは、[Azure Automation DSC](/azure/automation/automation-dsc-getting-started) に移行されていない大規模な DSC 環境をスケーリングする新しいオプションです。

> [!NOTE]
> SQL Server は WMF 5.1 以前のバージョンには追加されず、17090 以降の Windows Server バージョンでのみ使用できます。

プル サーバーで SQL Server を使用するよう構成するには、 **SqlProvider** を `$true` に、そして **SqlConnectionString** を有効な SQL Server 接続文字列に設定します。 詳細については、「[SqlClient 接続文字列](/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)」を参照してください。
**xDscWebService** を使用した SQL Server の構成例については、まず「 [xDscWebService リソースの使用](#using-the-xdscwebservice-resource)」に目を通してから、GitHub の「 [Sample_xDscWebServiceRegistration_UseSQLProvider.ps1](https://github.com/dsccommunity/xPSDesiredStateConfiguration/blob/master/source/Examples/Sample_xDscWebServiceRegistration_UseSQLProvider.ps1)」を参照してください。

### <a name="using-the-xdscwebservice-resource"></a>xDscWebService リソースの使用

Web プル サーバーをセットアップする最も簡単な方法は、 **xPSDesiredStateConfiguration** モジュールに含まれる **xDscWebService** リソースを使用することです。 次の手順では、Web サービスを設定する `Configuration` 内でリソースを使用する方法について説明します。

1. [Install-Module](/powershell/module/PowerShellGet/Install-Module) コマンドレットを呼び出して、 **xPSDesiredStateConfiguration** モジュールをインストールします。

   > [!NOTE]
   > `Install-Module` は、 **PowerShellGet** モジュールに含まれています。これは PowerShell 5.0 以降に含まれています。

1. DSC プル サーバーの SSL 証明書を、自社組織内またはパブリック証明機関のいずれかの信頼された証明機関から取得します。 証明機関から受け取る証明書は、通常、PFX 形式です。
1. 証明書は、DSC プル サーバーになるノードの既定の場所 (`CERT:\LocalMachine\My` である必要があります) にインストールします。
   - 証明書の拇印をメモしておきます。
1. 登録キーとして使う GUID を選択します。 PowerShell を使って GUID を生成するには、PS プロンプトに「`[guid]::newGuid()`」または「`New-Guid`」と入力し、Enter キーを押します。 このキーは、登録時にクライアント ノードによって認証のために共有キーとして使用されます。 詳細については、この後の「登録キー」セクションを参照してください。
1. PowerShell ISE で、次の構成スクリプトを起動 ( <kbd>F5</kbd>) します ( **xPSDesiredStateConfiguration** モジュールのフォルダーに `Sample_xDscWebServiceRegistration.ps1` として含まれています)。 このスクリプトは、プル サーバーをセットアップします。

    ```powershell
    configuration Sample_xDscWebServiceRegistration
    {
        param
        (
            [string[]]$NodeName = 'localhost',

            [ValidateNotNullOrEmpty()]
            [string] $certificateThumbPrint,

            [Parameter(HelpMessage='This should be a string with enough entropy (randomness) to protect the registration of clients to the pull server.  We will use new GUID by default.')]
            [ValidateNotNullOrEmpty()]
            [string] $RegistrationKey   # A guid that clients use to initiate conversation with pull server
        )

        Import-DSCResource -ModuleName PSDesiredStateConfiguration
        Import-DSCResource -ModuleName xPSDesiredStateConfiguration

        Node $NodeName
        {
            WindowsFeature DSCServiceFeature
            {
                Ensure = "Present"
                Name   = "DSC-Service"
            }

            xDscWebService PSDSCPullServer
            {
                Ensure                  = "Present"
                EndpointName            = "PSDSCPullServer"
                Port                    = 8080
                PhysicalPath            = "$env:SystemDrive\inetpub\PSDSCPullServer"
                CertificateThumbPrint   = $certificateThumbPrint
                ModulePath              = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules"
                ConfigurationPath       = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration"
                State                   = "Started"
                DependsOn               = "[WindowsFeature]DSCServiceFeature"
                RegistrationKeyPath     = "$env:PROGRAMFILES\WindowsPowerShell\DscService"
                AcceptSelfSignedCertificates = $true
                UseSecurityBestPractices     = $true
                Enable32BitAppOnWin64   = $false
            }

            File RegistrationKeyFile
            {
                Ensure          = 'Present'
                Type            = 'File'
                DestinationPath = "$env:ProgramFiles\WindowsPowerShell\DscService\RegistrationKeys.txt"
                Contents        = $RegistrationKey
            }
        }
    }
    ```

1. 構成を実行します。このとき、SSL 証明書の拇印を **certificateThumbPrint** パラメーターとして渡し、GUID 登録キーを **RegistrationKey** パラメーターとして渡します。

    ```powershell
    # To find the Thumbprint for an installed SSL certificate for use with the pull server list all
    # certificates in your local store and then copy the thumbprint for the appropriate certificate
    # by     reviewing the certificate subjects

    dir Cert:\LocalMachine\my

    # Then include this thumbprint when running the configuration
    Sample_xDscWebServiceRegistration -certificateThumbprint 'A7000024B753FA6FFF88E966FD6E19301FAE9CCC' -RegistrationKey '140a952b-b9d6-406b-b416-e0f759c9c0e4' -OutputPath c:\Configs\PullServer

    # Run the compiled configuration to make the target node a DSC Pull Server
    Start-DscConfiguration -Path c:\Configs\PullServer -Wait -Verbose
    ```

#### <a name="registration-key"></a>登録キー

サーバーにクライアント ノードを登録して、構成 ID の代わりに構成名を使用できるように、上の構成で作成された登録キーは、`C:\Program Files\WindowsPowerShell\DscService` にある `RegistrationKeys.txt` という名前のファイルに保存されます。 登録キーは、クライアントがプル サーバーに初期登録を行うときに共有シークレットとして機能します。 登録が正常に完了すると、クライアントは、プル サーバーに一意に認証されるために使う自己署名証明書を生成します。 この証明書の拇印がローカルに保存され、プル サーバーの URL に関連付けられます。

> [!NOTE]
> 登録キーは、PowerShell 4.0 ではサポートされていません。

プル サーバーで認証するようにノードを構成するには、登録キーが、このプル サーバーに登録されるすべてのターゲット ノードのメタ構成に含まれている必要があります。 なお、次のメタ構成にある **RegistrationKey** は、ターゲット コンピューターが正常に登録された後に削除され、値はプル サーバーの `RegistrationKeys.txt` ファイルに格納されている値と一致する必要があります (この例では '140a952b-b9d6-406b-b416-e0f759c9c0e4')。 登録キー値を知ると、任意のターゲット コンピューターをプル サーバーに登録できるようになるので、登録キー値は常にセキュリティで保護してください。

```powershell
[DSCLocalConfigurationManager()]
configuration Sample_MetaConfigurationToRegisterWithLessSecurePullServer
{
    param
    (
        [ValidateNotNullOrEmpty()]
        [string] $NodeName = 'localhost',

        [ValidateNotNullOrEmpty()]
        [string] $RegistrationKey, #same as the one used to set up pull server in previous configuration

        [ValidateNotNullOrEmpty()]
        [string] $ServerName = 'localhost' #node name of the pull server, same as $NodeName used in previous configuration
    )

    Node $NodeName
    {
        Settings
        {
            RefreshMode        = 'Pull'
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL          = "https://$ServerName`:8080/PSDSCPullServer.svc" # notice it is https
            RegistrationKey    = $RegistrationKey
            ConfigurationNames = @('ClientConfig')
        }

        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL       = "https://$ServerName`:8080/PSDSCPullServer.svc" # notice it is https
            RegistrationKey = $RegistrationKey
        }
    }
}

Sample_MetaConfigurationToRegisterWithLessSecurePullServer -RegistrationKey $RegistrationKey -OutputPath c:\Configs\TargetNodes
```

> [!NOTE]
> **ReportServerWeb** セクションでは、レポートするデータをプル サーバーに送信できるようにしています。

メタ構成ファイルに **ConfigurationID** プロパティがないことは、暗黙的に、そのプル サーバーが V2 バージョンのプル サーバー プロトコルをサポートしていることを示すので、初期登録が必要になります。 逆に、 **ConfigurationID** が存在することは、V1 バージョンのプル サーバー プロトコルが使用され、登録処理がないことを意味します。

> [!NOTE]
> プッシュのシナリオでは、現在のリリースにバグが存在するため、プル サーバーに登録されていないノードにも、メタ構成ファイルに ConfigurationID プロパティを定義する必要があります。 こうすると、V1 プル サーバー プロトコルが強制されるので、登録エラーのメッセージが表示されません。

## <a name="placing-configurations-and-resources"></a>構成とリソースの配置

プル サーバーのセットアップが完了すると、プル サーバーの構成で **ConfigurationPath** プロパティと **ModulePath** のプロパティによって定義されているフォルダーは、プルするためにターゲット ノードで使用可能にするモジュールと構成を配置する場所になります。 これらのファイルをプル サーバーが正しく処理するためには、特定の形式である必要があります。

### <a name="dsc-resource-module-package-format"></a>DSC リソース モジュールのパッケージの形式

各リソース モジュールは、圧縮し、`{Module Name}_{Module Version}.zip` というパターンで名前を付ける必要があります。

たとえば、モジュール名が **xWebAdminstration** で、バージョンが 3.1.2.0 のモジュールでは、`xWebAdministration_3.1.2.0.zip` という名前になります。 各バージョンのモジュールを 1 つの zip ファイルに含める必要があります。
各 zip ファイルには 1 つのバージョンのリソースのみが含まれるので、WMF 5.0 で追加された、単一のディレクトリに複数のモジュール バージョンを入れるモジュール形式はサポートされていません。 このため、プル サーバーで使うための DSC リソース モジュールをパッケージ化する前に、ディレクトリ構造に少しの変更が必要です。 WMF 5.0 の DSC リソースを含むモジュールの既定の形式は、`{Module Folder}\{Module Version}\DscResources\{DSC Resource Folder}\` です。 プル サーバー用にパッケージ化する前に、パスが `{Module Folder}\DscResources\{DSC Resource Folder}\` になるように **{Module version}** フォルダーを削除します。 この変更を加えた後、上で説明したようにフォルダーを zip 圧縮し、これらの zip ファイルを **ModulePath** フォルダーに置きます。

新しく追加したモジュールのチェックサム ファイルを作成するには、`New-DscChecksum {module zip file}` を使用します。

### <a name="configuration-mof-format"></a>構成 MOF の形式

ターゲット ノード上の LCM が構成を検証できるように、構成 MOF ファイルはチェックサム ファイルと組み合わせて使用する必要があります。 チェックサムを作成するには、[New-DscChecksum](/powershell/module/PSDesiredStateConfiguration/New-DSCCheckSum) コマンドレットを呼び出します。 このコマンドレットは、構成 MOF が存在するフォルダーが指定された **Path** パラメーターを受け取ります。 このコマンドレットは、`ConfigurationMOFName.mof.checksum` という名前でチェックサム ファイルを作成します。ここで、`ConfigurationMOFName` は構成 MOF ファイルの名前です。 指定のフォルダーに複数の構成 MOF ファイルがある場合は、そのフォルダー内の構成ごとにチェックサムが作成されます。 MOF ファイルと、それに関連するチェックサム ファイルは、 **ConfigurationPath** フォルダーに配置します。

> [!NOTE]
> 何らかの方法で構成 MOF ファイルを変更した場合は、チェックサム ファイルも作成し直す必要があります。

### <a name="tooling"></a>ツール

プル サーバーのセットアップ、検証、管理を簡素化するために、次のツールが、xPSDesiredStateConfiguration モジュールの最新バージョンに例として含まれています。

1. プル サーバーに使用する DSC リソース モジュールおよび構成ファイルをパッケージ化するために役立つモジュール。
   [PublishModulesAndMofsToPullServer.psm1](https://github.com/dsccommunity/xPSDesiredStateConfiguration/blob/master/source/Modules/DscPullServerSetup/DscPullServerSetup.psm1)。
   次の例です。

    ```powershell
    # Example 1 - Package all versions of given modules installed locally and MOF files are in c:\LocalDepot
    $moduleList = @('xWebAdministration', 'xPhp')
    Publish-DSCModuleAndMof -Source C:\LocalDepot -ModuleNameList $moduleList

    # Example 2 - Package modules and mof documents from c:\LocalDepot
    Publish-DSCModuleAndMof -Source C:\LocalDepot -Force
    ```

1. プル サーバーが正しく構成されていることを検証するスクリプト。
   [PullServerSetupTests.ps1](https://github.com/dsccommunity/xPSDesiredStateConfiguration/blob/master/source/Modules/DscPullServerSetup/DscPullServerSetupTest/DscPullServerSetupTest.ps1)。

## <a name="community-solutions-for-pull-service"></a>プル サービスのコミュニティ ソリューション

DSC コミュニティは、プル サービス プロトコルを実装するための複数のソリューションを作成しています。 これらはオンプレミス環境向けに、プル サービス機能と、段階的な機能強化でコミュニティに貢献する機会を提供します。

- [Tug](https://github.com/powershellorg/tug)
- [DSC-TRÆK](https://github.com/powershellorg/dsc-traek)

## <a name="pull-client-configuration"></a>プル クライアントの構成

次のトピックでは、プル クライアントのセットアップについて詳しく説明します。

- [構成 ID を使用したプル クライアントのセットアップ](pullClientConfigID.md)
- [構成名を使用したプル クライアントのセットアップ](pullClientConfigNames.md)
- [部分構成](partialConfigs.md)

## <a name="see-also"></a>関連項目

- [Windows PowerShell Desired State Configuration の概要](../overview/overview.md)
- [構成の適用](enactingConfigurations.md)
- [DSC レポート サーバーの使用](reportServer.md)
- [[MS-DSCPM]: Desired State Configuration Pull Model プロトコル](/openspecs/windows_protocols/ms-dscpm/ea744c01-51a2-4000-9ef2-312711dcc8c9)
- [[MS-DSCPM]: Desired State Configuration Pull Model プロトコルの正誤表](/openspecs/windows_protocols/ms-winerrata/f5fc7ae3-9172-41e8-ac6a-2a5a5b7bfaf5)
