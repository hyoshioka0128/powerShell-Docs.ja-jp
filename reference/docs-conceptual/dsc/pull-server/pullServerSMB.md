---
ms.date: 04/11/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC SMB プル サーバーのセットアップ
description: DSC SMB プル サーバーは、DSC 構成ファイルと DSC リソースを必要なときにターゲット ノードに提供する SMB ファイル共有をホストするコンピューターです。
ms.openlocfilehash: 4ac1b0db719fa124d6fa9a654acb64ec24d9ea41
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92658434"
---
# <a name="setting-up-a-dsc-smb-pull-server"></a>DSC SMB プル サーバーのセットアップ

適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

> [!IMPORTANT]
> プル サーバー (Windows Feature *DSC-Service* ) は、Windows Server のサポート対象のコンポーネントですが、新機能がオファーされる予定はありません。 管理対象のクライアントは、(Windows Server のプル サーバー以降の機能が含まれる) [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) または、[こちら](pullserver.md#community-solutions-for-pull-service)に列挙されているコミュニティ ソリューションのいずれかに切り替えを開始することをお勧めします。

DSC [SMB](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) プル サーバーは、DSC 構成ファイルと DSC リソースを必要なときにターゲット ノードに提供する SMB ファイル共有をホストするコンピューターです。

DSC の SMB プル サーバーを使うには、次のことが必要です。

- PowerShell 4.0 以降を実行しているサーバーに SMB ファイル共有をセットアップする
- SMB 共有からプルするための、PowerShell 4.0 以降を実行しているクライアントを構成する

## <a name="using-the-xsmbshare-resource-to-create-an-smb-file-share"></a>xSmbShare リソースを使用して SMB ファイル共有を作成する

SMB ファイル共有を設定する方法はいくつかありますが、ここでは DSC を使って設定する方法を紹介します。

### <a name="install-the-xsmbshare-resource"></a>xSmbShare リソースをインストールする

[Install-Module](/powershell/module/PowershellGet/Install-Module) コマンドレットを呼び出して **xSmbShare** モジュールをインストールします。

> [!NOTE]
> `Install-Module` は、PowerShell 5.0 に含まれている **PowerShellGet** モジュールに含まれています。
> **xSmbShare** に含まれる DSC リソース **xSmbShare** を使うと、SMB ファイル共有を作成できます。

### <a name="create-the-directory-and-file-share"></a>ディレクトリとファイル共有を作成する

次の構成では、 **File** リソースを使って共有するディレクトリを作成し、 **xSmbShare** リソースを使って SMB 共有をセットアップします。

```powershell
Configuration SmbShare
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Import-DscResource -ModuleName xSmbShare

    Node localhost
    {

        File CreateFolder
        {
            DestinationPath = 'C:\DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
        }

        xSMBShare CreateShare
        {
            Name = 'DscSmbShare'
            Path = 'C:\DscSmbShare'
            FullAccess = 'administrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
        }
    }
}
```

ディレクトリ `C:\DscSmbShare` がまだ存在しない場合は、構成によって作成され、そのディレクトリが SMB ファイル共有として使用されます。 **FullAccess** は、ファイル共有に書き込む必要があるか、ファイル共有から削除する必要があるあらゆるアカウントに付与してください。 **ReadAccess** は、共有から構成または DSC リソースを取得するあらゆるクライアント ノードに付与する必要があります。

> [!NOTE]
> DSC は既定ではシステム アカウントとして実行されるので、コンピューター自体も共有にアクセスする必要があります。

### <a name="give-file-system-access-to-the-pull-client"></a>ファイル システムにプル クライアントへのアクセス権限を付与する

クライアント ノードに **ReadAccess** を与えると、そのノードは SMB 共有にアクセスできるようになりますが、その共有内のファイルやフォルダーにはアクセスできません。 クライアント ノードに、SMB 共有のフォルダーやサブフォルダーに対するアクセス権限を明示的に許可する必要があります。 DSC でこれを行うには、 [cNtfsAccessControl](https://www.powershellgallery.com/packages/cNtfsAccessControl/1.2.0) モジュールに含まれている **cNtfsPermissionEntry** リソースを使用して追加します。
次の構成では、プル クライアントに ReadAndExecute アクセスを付与する **cNtfsPermissionEntry** ブロックを追加します。

```powershell
Configuration DSCSMB
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Import-DscResource -ModuleName xSmbShare
    Import-DscResource -ModuleName cNtfsAccessControl

    Node localhost
    {

        File CreateFolder
        {
            DestinationPath = 'C:\DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
        }

        xSMBShare CreateShare
        {
            Name = 'DscSmbShare'
            Path = 'C:\DscSmbShare'
            FullAccess = 'administrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
        }

        cNtfsPermissionEntry PermissionSet1
        {
            Ensure = 'Present'
            Path = 'C:\DscSmbShare'
            Principal = 'myDomain\Contoso-Server$'
            AccessControlInformation = @(
                cNtfsAccessControlInformation
                {
                    AccessControlType = 'Allow'
                    FileSystemRights = 'ReadAndExecute'
                    Inheritance = 'ThisFolderSubfoldersAndFiles'
                    NoPropagateInherit = $false
                }
            )
            DependsOn = '[File]CreateFolder'
        }
    }
}
```

## <a name="placing-configurations-and-resources"></a>構成とリソースの配置

クライアント ノードがプルする必要のある構成 MOF ファイルや DSC リソースを SMB 共有フォルダーに保存します。

すべての構成 MOF ファイルは、 *ConfigurationID* .mof という名前である必要があります。ここで、 *ConfigurationID* はターゲット ノードの LCM の **ConfigurationID** プロパティの値です。 プル クライアントの設定の詳細については、「[構成 ID を使用したプル クライアントのセットアップ](pullClientConfigID.md)」を参照してください。

> [!NOTE]
> SMB プル サーバーを使っている場合は、構成 ID を使う必要があります。 構成名は、SMB ではサポートされません。

各リソース モジュールは、圧縮し、`{Module Name}_{Module Version}.zip` というパターンで名前を付ける必要があります。 たとえば、モジュール名が xWebAdminstration で、バージョンが 3.1.2.0 のモジュールでは、'xWebAdministration_3.2.1.0.zip' という名前になります。 各バージョンのモジュールを 1 つの zip ファイルに含める必要があります。 zip ファイル内のモジュールの個別のバージョンはサポートされていません。
プル サーバーで使うための DSC リソース モジュールをパッケージ化する前に、ディレクトリ構造に少しの変更が必要です。

WMF 5.0 の DSC リソースを含むモジュールの既定の形式は、`{Module Folder}\{Module Version}\DscResources\{DSC Resource Folder}\` です。

プル サーバー用にパッケージ化する前に、パスが `{Module Folder}\DscResources\{DSC Resource Folder}\` になるように単純に `{Module version}` フォルダーを削除します。 この変更を加えた後、上で説明したようにフォルダーを zip 圧縮し、これらの zip ファイルを SMB 共有フォルダーに置きます。

## <a name="creating-the-mof-checksum"></a>MOF チェックサムの作成

ターゲット ノード上の LCM が構成を検証できるように、構成 MOF ファイルはチェックサム ファイルと組み合わせて使用する必要があります。 チェックサムを作成するには、[New-DSCCheckSum](/powershell/module/PSDesiredStateConfiguration/New-DSCCheckSum) コマンドレットを呼び出します。 このコマンドレットは、構成 MOF が存在するフォルダーが指定された `Path` パラメーターを受け取ります。 このコマンドレットは、`ConfigurationMOFName.mof.checksum` という名前でチェックサム ファイルを作成します。ここで、`ConfigurationMOFName` は構成 MOF ファイルの名前です。 指定のフォルダーに複数の構成 MOF ファイルがある場合は、そのフォルダー内の構成ごとにチェックサムが作成されます。

チェックサム ファイルは、構成 MOF ファイルと同じディレクトリ (既定では `$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration`) に存在し、拡張子として `.checksum` が付けられた同じ名前である必要があります。

> [!NOTE]
> 何らかの方法で構成 MOF ファイルを変更した場合は、チェックサム ファイルも作成し直す必要があります。

## <a name="setting-up-a-pull-client-for-smb"></a>SMB のプル クライアントのセットアップ

SMB 共有から構成とリソースの両方または一方をプルするクライアントを設定するには、構成と DSC リソースのプル元の共有を指定する **ConfigurationRepositoryShare** ブロックと **ResourceRepositoryShare** ブロックでクライアントのローカル構成マネージャー (LCM) を構成します。

LCM 構成の詳細については、「[構成 ID を使用したプル クライアントのセットアップ](pullClientConfigID.md)」をご覧ください。

> [!NOTE]
> わかりやすくする目的で、この例では **PSDscAllowPlainTextPassword** を使用し、 **Credential** パラメーターにプレーンテキスト パスワードを渡すようにしています。 資格情報をより安全に渡す方法については、「[構成データでの資格情報オプション](../configurations/configDataCredentials.md)」を参照してください。 リソースをプルするだけであっても、SMB プル サーバーのメタ構成の **設定** ブロックに **ConfigurationID** を指定する **必要があります** 。

```powershell
$secpasswd = ConvertTo-SecureString "Pass1Word" -AsPlainText -Force
$mycreds = New-Object System.Management.Automation.PSCredential ("TestUser", $secpasswd)

[DSCLocalConfigurationManager()]
configuration SmbCredTest
{
    Node $AllNodes.NodeName
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
            ConfigurationID    = '16db7357-9083-4806-a80c-ebbaf4acd6c1'
        }

         ConfigurationRepositoryShare SmbConfigShare
        {
            SourcePath = '\\WIN-E0TRU6U11B1\DscSmbShare'
            Credential = $mycreds
        }

        ResourceRepositoryShare SmbResourceShare
        {
            SourcePath = '\\WIN-E0TRU6U11B1\DscSmbShare'
            Credential = $mycreds

        }
    }
}

$ConfigurationData = @{
    AllNodes = @(
        @{
            #the "*" means "all nodes named in ConfigData" so we don't have to repeat ourselves
            NodeName="localhost"
            PSDscAllowPlainTextPassword = $true
        })
}
```

## <a name="acknowledgements"></a>謝辞

次の方々に感謝します。

- DSC で SMB を使用することに関する Mike F. Robbins 氏の投稿を、このトピックの内容として参考にしました。 彼のブログは [Mike F Robbins](http://mikefrobbins.com/) にあります。
- **cNtfsAccessControl** モジュールを作成した Serge Nikalaichyk 氏。 このモジュールのソースは [cNtfsAccessControl](https://github.com/SNikalaichyk/cNtfsAccessControl) にあります。

## <a name="see-also"></a>関連項目

[Windows PowerShell Desired State Configuration の概要](../overview/overview.md)

[構成の適用](enactingConfigurations.md)

[構成 ID を使用したプル クライアントのセットアップ](pullClientConfigID.md)
