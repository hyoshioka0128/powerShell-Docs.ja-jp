---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: 構成データでの資格情報オプション
description: DSC では、資格情報を提供して、ローカル システム アカウントではなく、特定のユーザー アカウントのコンテキストで構成設定を適用することができます。
ms.openlocfilehash: 41478dc042ca59fb70aa033de81b589a4a8c09c7
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92658654"
---
# <a name="credentials-options-in-configuration-data"></a>構成データでの資格情報オプション

> 適用先:Windows PowerShell 5.0

## <a name="plain-text-passwords-and-domain-users"></a>プレーンテキスト パスワードとドメイン ユーザー

暗号化されていない資格情報を含む DSC 構成では、プレーンテキスト パスワードについてのエラー メッセージが生成されます。 また、DSC では、ドメイン資格情報を使用すると警告が生成されます。 これらのエラーや警告メッセージが表示されないようにするには、次の DSC 構成データ キーワードを使用します。

- **PsDscAllowPlainTextPassword**
- **PsDscAllowDomainUser**

> [!NOTE]
> 暗号化されていないプレーンテキスト パスワードを格納/送信するのは一般的には安全ではありません。 このトピックで後述する手法を使って資格情報をセキュリティ保護することをお勧めします。 Azure Automation DSC サービスでは、構成でコンパイルされ、安全に格納される資格情報を一元的に管理することができます。 詳細について、「[DSC 構成のコンパイル/資格情報資産](/azure/automation/automation-dsc-compile#credential-assets)」を参照してください。

## <a name="handling-credentials-in-dsc"></a>DSC での資格情報の処理

DSC 構成リソースは、既定で `Local System` として実行されます。 ただし、`Package` リソースでソフトウェアを特定のユーザー アカウントでインストールする必要がある場合など、リソースによっては資格情報が必要となることがあります。

以前のリソースでは、ハード コードされた `Credential` プロパティ名使用してこのことに対処していました。 WMF 5.0 では、すべてのリソースに対して自動 `PsDscRunAsCredential` プロパティが追加されました。 `PsDscRunAsCredential` の使用の詳細については、「[ユーザーの資格情報を指定して DSC を実行する](runAsUser.md)」を参照してください。 新しいリソースおよびカスタム リソースでは、資格情報の独自のプロパティを作成する代わりに、この自動プロパティを使用できます。

> [!NOTE]
> 一部のリソースは特定の理由のために複数の資格情報を使用するように設計されており、それらのリソースには独自の資格情報プロパティがあることに注意してください。

リソースの使用可能な資格情報プロパティを検索するには、ISE で `Get-DscResource -Name ResourceName -Syntax` または Intellisense を使用します (`CTRL+SPACE`)。

```powershell
Get-DscResource -Name Group -Syntax
```

```Output
Group [String] #ResourceName
{
    GroupName = [string]
    [Credential = [PSCredential]]
    [DependsOn = [string[]]]
    [Description = [string]]
    [Ensure = [string]{ Absent | Present }]
    [Members = [string[]]]
    [MembersToExclude = [string[]]]
    [MembersToInclude = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
}
```

この例では、`PSDesiredStateConfiguration` 組み込み DSC リソース モジュールの [Group](../resources/resources.md) リソースを使用します。 ローカル グループを作成し、メンバーを追加または削除できます。 `Credential` プロパティと、自動 `PsDscRunAsCredential` プロパティの両方を受け取ります。 ただし、リソースでは `Credential` プロパティのみが使用されます。

`PsDscRunAsCredential`プロパティの詳細については、「[ユーザーの資格情報を指定して DSC を実行する](runAsUser.md)」を参照してください。

## <a name="example-the-group-resource-credential-property"></a>例: Group リソース資格情報プロパティ

DSC は `Local System` で実行されるため、ローカル ユーザーおよびグループを変更するためのアクセス許可が既にあります。 追加されたメンバーがローカル アカウントの場合、資格情報は必要ありません。 `Group` リソースがローカル グループにドメイン アカウントを追加する場合、資格情報が必要となります。

Active Directory への匿名クエリは許可されません。 `Group` リソースの `Credential` プロパティは、Active Directory のクエリに使用されるドメイン アカウントです。 既定では、ユーザーは Active Directory 内の大部分のオブジェクトを *読み取る* ことができるため、ほとんどの場合これは汎用ユーザー アカウントです。

## <a name="example-configuration"></a>構成の例

次のコード例では、DSC を使用してローカル グループにドメイン ユーザーを設定します。

```powershell
Configuration DomainCredentialExample
{
    param
    (
        [PSCredential] $DomainCredential
    )
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $DomainCredential
        }
    }
}

$cred = Get-Credential -UserName contoso\genericuser -Message "Password please"
DomainCredentialExample -DomainCredential $cred
```

このコードでは、エラーと警告メッセージの両方が生成されます。

```
ConvertTo-MOFInstance : System.InvalidOperationException error processing property 'Credential' OF
TYPE 'Group': Converting and storing encrypted passwords as plain text is not recommended.
For more information on securing credentials in MOF file, please refer to MSDN blog:
https://go.microsoft.com/fwlink/?LinkId=393729

At line:11 char:9
+   Group
At line:341 char:16
+     $aliasId = ConvertTo-MOFInstance $keywordName $canonicalizedValue
+                ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Write-Error], InvalidOperationException
    + FullyQualifiedErrorId : FailToProcessProperty,ConvertTo-MOFInstance
WARNING: It is not recommended to use domain credential for node 'localhost'. In order to suppress
the warning, you can add a property named 'PSDscAllowDomainUser' with a value of $true to your DSC
configuration data for node 'localhost'.

Compilation errors occurred while processing configuration
'DomainCredentialExample'. Please review the errors reported in error stream and modify your
configuration code appropriately.
At C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateConfiguration\PSDesiredStateConfiguration.psm1:3917 char:5
+     throw $ErrorRecord
+     ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (DomainCredentialExample:String) [], InvalidOperationException
    + FullyQualifiedErrorId : FailToProcessConfiguration
```

この例には、次の 2 つの問題があります。

1. プレーンテキスト パスワードが推奨されないことを説明するエラー。
2. ドメイン資格情報を使用しないよう勧める警告。

フラグ **PSDSCAllowPlainTextPassword** および **PSDSCAllowDomainUser** では、関連するリスクをユーザーに通知するエラーと警告が抑制されます。

## <a name="psdscallowplaintextpassword"></a>PSDSCAllowPlainTextPassword

最初のエラー メッセージには、ドキュメントの URL があります。 このリンクでは、[ConfigurationData](./configData.md) 構造と証明書を使用してパスワードを暗号化する方法を説明しています。 証明書と DSC の詳細については、[この投稿をご覧ください](https://aka.ms/certs4dsc)。

プレーンテキスト パスワードを強制的に使用するには、次のようにリソースの構成データ セクションに `PsDscAllowPlainTextPassword` キーワードが必要です。

```powershell
$password = "ThisIsAPlaintextPassword" | ConvertTo-SecureString -asPlainText -Force
$username = "contoso\Administrator"
[PSCredential] $credential = New-Object System.Management.Automation.PSCredential($username,$password)

Configuration DomainCredentialExample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $credential
        }
    }
}

$cd = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            PSDscAllowPlainTextPassword = $true
        }
    )
}

DomainCredentialExample -ConfigurationData $cd
```

### <a name="localhostmof"></a>localhost.mof

**PSDSCAllowPlainTextPassword** フラグを使うときは、ユーザーが MOF ファイルにプレーン テキスト パスワードを格納するリスクを認識している必要があります。 生成される MOF ファイルでは、 **SecureString** を含む **PSCredential** オブジェクトが使われている場合であっても、パスワードはプレーン テキストとして表示されます。 これは、資格情報が公開されるときだけです。 この MOF ファイルにアクセスできるすべてのユーザーは、管理者アカウントにアクセスできます。

```
/*
@TargetNode='localhost'
@GeneratedBy=Administrator
@GenerationDate=01/31/2019 06:43:13
@GenerationHost=Server01
*/

instance of MSFT_Credential as $MSFT_Credential1ref
{
Password = "ThisIsAPlaintextPassword";
 UserName = "Administrator";

};

instance of MSFT_GroupResource as $MSFT_GroupResource1ref
{
ResourceID = "[Group]DomainUserToLocalGroup";
 MembersToInclude = {
    "contoso\\alice"
};
 Credential = $MSFT_Credential1ref;
 SourceInfo = "::11::9::Group";
 GroupName = "ApplicationAdmins";
 ModuleName = "PSDesiredStateConfiguration";

ModuleVersion = "1.0";

 ConfigurationName = "DomainCredentialExample";

};
```

### <a name="credentials-in-transit-and-at-rest"></a>送信中および保存時の資格情報

- **PSDscAllowPlainTextPassword** フラグを使用すると、クリア テキストのパスワードが含まれる MOF ファイルをコンパイルできます。 クリア テキストのパスワードが含まれる MOF ファイルを格納するときは注意が必要です。
- MOF ファイルが **プッシュ** モードでノードに配信されるときは、 **AllowUnencrypted** パラメーターで既定値を上書きしない限り、WinRM によってクリア テキスト パスワードを保護するために通信が暗号化されます。
  - 証明書で MOF を暗号化すると、ノードに適用される前の保存時の MOF ファイルが保護されます。
- **プル** モードでは、HTTPS を使用してインターネット インフォメーション サーバーで指定されているプロトコルでトラフィックを暗号化するように、Windows プル サーバーを構成することができます。 詳しくは、「[DSC プル クライアントのセットアップ](../pull-server/pullclient.md)」および「[証明書を使用した MOF ファイルのセキュリティ保護](../pull-server/secureMOF.md)」をご覧ください。
  - [Azure Automation State Configuration](/azure/automation/automation-dsc-overview) サービスでは、プル トラフィックは常に暗号化されます。
- PowerShell 5.0 以降では、ノードでの保存時の MOF ファイルは暗号化されます。
  - PowerShell 4.0 での MOF ファイルは、ノードにプッシュまたはプルされるときに証明書で暗号化されていない限り、保存時には暗号化されません。

**Microsoft では、重大なセキュリティ リスクのため、プレーンテキスト パスワードを使用しないことをお勧めします。**

## <a name="domain-credentials"></a>ドメイン資格情報

構成スクリプト例を (暗号化して、またはしないで) もう一度実行しても、資格情報のドメイン アカウントを使用することは推奨されないという警告が生成されます。 ローカル アカウントを使用すると、他のサーバーで使用可能なドメイン資格情報が漏えいする可能性がなくなります。

**DSC リソースで資格情報を使用する場合、可能な場合は、ドメイン アカウントではなくローカル アカウントを選択します。**

資格情報の `Username` プロパティに '\\' または '\@' が含まれていた場合、DSC はそれをドメイン アカウントとして処理します。 ユーザー名のドメイン部分には、"localhost"、"127.0.0.1"、および "::1" の例外があります。

## <a name="psdscallowdomainuser"></a>PSDscAllowDomainUser

上記の DSC `Group` リソースの例では、Active Directory ドメインのクエリを実行するにはドメイン アカウントが *必要です* 。 この場合は、次のように `ConfigurationData` ブロックに `PSDscAllowDomainUser` プロパティを追加します。

```powershell
$password = "ThisIsAPlaintextPassword" | ConvertTo-SecureString -asPlainText -Force
$username = "contoso\Administrator"
[PSCredential] $credential = New-Object System.Management.Automation.PSCredential($username,$password)

Configuration DomainCredentialExample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $credential
        }
    }
}

$cd = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            PSDscAllowDomainUser = $true
            PSDscAllowPlainTextPassword = $true
        }
    )
}

DomainCredentialExample -ConfigurationData $cd
```

これで、構成スクリプトによってエラーや警告なしで MOF ファイルが生成されます。
