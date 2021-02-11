---
ms.date: 01/07/2021
keywords: DSC, PowerShell, 構成, セットアップ
title: Linux 用 Desired State Configuration (DSC) の概要
description: このトピックでは、Linux 用 PowerShell Desired State Configuration (DSC) の使用を開始する方法について説明します。
ms.openlocfilehash: 6f0dea956b16c0828964a10b43abbbc65879ea33
ms.sourcegitcommit: b9826dcf402db8a2b6d3eab37edb82c6af113343
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040924"
---
# <a name="get-started-with-desired-state-configuration-dsc-for-linux"></a>Linux 用 Desired State Configuration (DSC) の概要

このトピックでは、Linux 用 PowerShell Desired State Configuration (DSC) の使用を開始する方法について説明します。
DSC に関する一般的な情報については、「[Windows PowerShell Desired State Configuration の概要](../overview/overview.md)」を参照してください。

## <a name="supported-linux-operation-system-versions"></a>サポートされている Linux オペレーティング システム バージョン

次の Linux オペレーティング システム バージョンは、Linux 用 DSC によってサポートされています。

- CentOS 5、6、および 7 (x86/x64)
- Debian GNU/Linux 6、7、および 8 (x86/x64)
- Oracle Linux 5、6 および 7 (x86/x64)
- Red Hat Enterprise Linux Server 5、6、および 7 (x86/x64)
- SUSE Linux Enterprise Server 10、11、および 12 (x86/x64)
- Ubuntu Server 12.04 LTS、14.04 LTS、16.04 LTS、18.04 (x86/x64)

## <a name="installing-dsc-for-linux"></a>Linux 用 DSC のインストール

Linux 用 DSC をインストールする前に、[Open Management Infrastructure (OMI)](https://github.com/Microsoft/omi) をインストールする必要があります。

### <a name="installing-omi"></a>OMI のインストール

Linux 用 Desired State Configuration には、Open Management Infrastructure (OMI) CIM サーバーの 1.0.8.1 以降のバージョンが必要です。 OMI は、The Open Group: [Open Management Infrastructure (OMI)](https://github.com/Microsoft/omi) からダウンロードできます。

OMI をインストールするには、Linux システムに適したパッケージ (.rpm または .deb)、OpenSSL バージョンに適したパッケージ (ssl_098 または ssl_100)、およびアーキテクチャに適したパッケージ (x86/x64) をインストールします。 CentOS、Red Hat Enterprise Linux、SUSE Linux Enterprise Server、および Oracle Linux には、RPM パッケージが適しています。 Debian GNU/Linux および Ubuntu Server には、DEB パッケージが適しています。 OpenSSL 0.9.8 がインストールされているコンピューターには ssl_098 パッケージが適し、OpenSSL 1.0 がインストールされているコンピューターには ssl_100 パッケージが適しています。

> [!NOTE]
> インストールされている OpenSSL のバージョンを調べるには、コマンド `openssl version` を実行します。

CentOS 7 x64 システムに OMI をインストールするには、次のコマンドを実行します。

`# sudo rpm -Uvh omiserver-1.0.8.ssl_100.rpm`

### <a name="installing-dsc"></a>DSC のインストール

Linux 用の DSC は、リポジトリの [PowerShell-DSC-for-Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux/releases/tag/v1.1.1-926) リポジトリからダウンロードできます。

DSC をインストールするには、Linux システムに適したパッケージ (.rpm または .deb)、OpenSSL バージョンに適したパッケージ (ssl_098 または ssl_100)、およびアーキテクチャに適したパッケージ (x86/x64) をインストールします。 CentOS、Red Hat Enterprise Linux、SUSE Linux Enterprise Server、および Oracle Linux には、RPM パッケージが適しています。 Debian GNU/Linux および Ubuntu Server には、DEB パッケージが適しています。 OpenSSL 0.9.8 がインストールされているコンピューターには ssl_098 パッケージが適し、OpenSSL 1.0 がインストールされているコンピューターには ssl_100 パッケージが適しています。

> [!NOTE]
> インストールされている OpenSSL のバージョンを調べるには、コマンド openssl version を実行します。

CentOS 7 x64 システムに DSC をインストールするには、次のコマンドを実行します。

`# sudo rpm -Uvh dsc-1.0.0-254.ssl_100.x64.rpm`

## <a name="using-dsc-for-linux"></a>Linux 用 DSC の使用

次のセクションでは、Linux コンピューターで DSC 構成を作成し、実行する方法を説明します。

### <a name="creating-a-configuration-mof-document"></a>構成 MOF ドキュメントの作成

Linux コンピューターの構成を作成するには、Windows コンピューターの場合と同じように、Windows PowerShell 構成キーワードを使用します。 次の手順では、Windows PowerShell を使用して Linux コンピューターの構成ドキュメントを作成する方法について説明します。

1. nx モジュールのインポート nx Windows PowerShell モジュールには Linux 用 DSC の組み込みリソースのスキーマが含まれており、このモジュールをローカル コンピューターにインストールし、構成にインポートする必要があります。

   - nx モジュールをインストールするには、nx モジュール ディレクトリを `$env:USERPROFILE\Documents\WindowsPowerShell\Modules\` または `$PSHOME\Modules` にコピーします。 nx モジュールは、Linux 用 DSC のインストール パッケージに含まれています。 構成に nx モジュールをインポートするには、`Import-DSCResource` コマンドを使用します。

   ```powershell
   Configuration ExampleConfiguration{

    Import-DSCResource -Module nx

   }
   ```

2. 構成を定義し、構成ドキュメントを生成します。

   ```powershell
   Configuration ExampleConfiguration
   {
        Import-DscResource -Module nx

        Node  "linuxhost.contoso.com"
        {
            nxFile ExampleFile
            {
                DestinationPath = "/tmp/example"
                Contents = "hello world `n"
                Ensure = "Present"
                Type = "File"
            }
        }
   }

   ExampleConfiguration -OutputPath:"C:\temp"
   ```

### <a name="push-the-configuration-to-the-linux-computer"></a>Linux コンピューターへの構成のプッシュ

構成ドキュメント (MOF ファイル) を Linux コンピューターにプッシュするには、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを使用します。 Linux コンピューターに対してリモートからこのコマンドレットを [Get-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Get-DscConfiguration) と共に使用するか、または [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration) コマンドレットを使用するためには、CIMSession を使用する必要あります。 Linux コンピューターに **CIMSession** を作成するには、[New-CimSession](/powershell/module/CimCmdlets/New-CimSession) コマンドレットを使用します。

次のコードは、Linux 用 DSC の CIMSession を作成する方法を示しています。

```powershell
$Node = "ostc-dsc-01"
$Credential = Get-Credential -UserName "root" -Message "Enter Password:"

#Ignore SSL certificate validation
# $opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

#Options for a trusted SSL certificate
$opt = New-CimSessionOption -UseSsl

$sessParams = @{
    Credential = $credential
    ComputerName = $Node
    Port = 5986
    Authentication = 'basic'
    SessionOption = $opt
    OperationTimeoutSec = 90
}

$Sess = New-CimSession @sessParams
```

> [!NOTE]
> "プッシュ" モードの場合、ユーザーの資格情報は Linux コンピューターの root ユーザーである必要があります。 Linux 用 DSC では SSL/TLS 接続のみがサポートされているため、`New-CimSession` を使用するときは、–UseSSL パラメーターを $true に設定する必要があります。 OMI (DSC 用) で使用される SSL 証明書は、pemfile プロパティと keyfile プロパティを使用して `/etc/opt/omi/conf/omiserver.conf` ファイルに指定します。 [New-CimSession](/powershell/module/CimCmdlets/New-CimSession) コマンドレットを実行している Windows コンピューターでこの証明書が信頼されていない場合は、`-SkipCACheck -SkipCNCheck -SkipRevocationCheck` の各 CIMSession オプションを使用して証明書の検証を無視することを選択できます。

Linux ノードに DSC 構成をプッシュするには、次のコマンドを実行します。

`Start-DscConfiguration -Path:"C:\temp" -CimSession $Sess -Wait -Verbose`

### <a name="distribute-the-configuration-with-a-pull-server"></a>プル サーバーを使用した構成の配布

構成を Linux コンピューターに配布するには、Windows コンピューターの場合と同じく、プル サーバーを使用できます。 プル サーバーを使用する方法の詳細については、「[DSC Web プル サーバーのセットアップ](../pull-server/pullServer.md)」をご覧ください。 プル サーバーでの Linux コンピューターの使用に関する追加情報および制限事項については、Linux 用 Desired State Configuration のリリース ノートをご覧ください。

### <a name="working-with-configurations-locally"></a>ローカルでの構成の操作

Linux 用 DSC には、ローカルの Linux コンピューターから構成を操作するためのスクリプトが含まれています。 これらのスクリプトは、`/opt/microsoft/dsc/Scripts` にあり、次のものが含まれています。

- GetDscConfiguration.py

  コンピューターに適用される現在の構成を返します。 Windows PowerShell コマンドレットの `Get-DscConfiguration` コマンドレットに似ています。

  `# sudo ./GetDscConfiguration.py`

- GetDscLocalConfigurationManager.py

  コンピューターに適用される現在のメタ構成を返します。 [Get-DSCLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Get-DscLocalConfigurationManager) コマンドレットに似ています。

  `# sudo ./GetDscLocalConfigurationManager.py`

- InstallModule.py

  カスタムの DSC リソース モジュールをインストールします。 モジュール共有オブジェクト ライブラリおよびスキーマ MOF ファイルが含まれている .zip ファイルへのパスが必要です。

  `# sudo ./InstallModule.py /tmp/cnx_Resource.zip`

- RemoveModule.py

  カスタムの DSC リソース モジュールを削除します。 削除するモジュールの名前が必要です。

  `# sudo ./RemoveModule.py cnx_Resource`

- StartDscLocalConfigurationManager.py

  構成 MOF ファイルをコンピューターに適用します。 [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットに似ています。 適用する構成 MOF へのパスが必要です。

  `# sudo ./StartDscLocalConfigurationManager.py –configurationmof /tmp/localhost.mof`

- SetDscLocalConfigurationManager.py

  メタ構成 MOF ファイルをコンピューターに適用します。 [Set-DSCLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Set-DscLocalConfigurationManager) コマンドレットに似ています。 適用するメタ構成 MOF へのパスが必要です。

  `# sudo ./SetDscLocalConfigurationManager.py –configurationmof /tmp/localhost.meta.mof`

## <a name="powershell-desired-state-configuration-for-linux-log-files"></a>Linux 用 PowerShell Desired State Configuration のログ ファイル

Linux 用 DSC のメッセージのために次のログ ファイルが生成されます。

|     ログ ファイル      |     ディレクトリ      |                                               説明                                                |
| ----------------- | ------------------ | -------------------------------------------------------------------------------------------------------- |
| **omiserver.log** | `/var/opt/omi/log` | OMI CIM サーバーの操作に関連するメッセージ。                                                |
| **dsc.log**       | `/var/opt/omi/log` | ローカル構成マネージャー (LCM) と DSC リソースの操作に関連するメッセージ。 |
