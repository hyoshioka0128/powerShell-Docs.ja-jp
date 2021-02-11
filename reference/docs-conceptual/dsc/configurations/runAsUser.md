---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC リソースに対して資格情報を使用する
description: DSC では、構成に資格情報を提供して、ローカル システム アカウントではなく、特定のユーザー アカウントのコンテキストで構成設定を適用することができます。
ms.openlocfilehash: e4ca39e099afacd7cb06c4d9ef889f94deb73cee
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92645082"
---
# <a name="use-credentials-with-dsc-resources"></a>DSC リソースに対して資格情報を使用する

> 適用先: Windows PowerShell 5.0、Windows PowerShell 5.1

資格情報のセットを指定して DSC リソースを実行するには、構成の中で自動 **PsDscRunAsCredential** プロパティを使います。 既定では、DSC はシステム アカウントとして各リソースを実行します。 時には、ユーザーとして実行することが必要な場合があります。たとえば、特定のユーザー コンテキストで MSI パッケージをインストールする場合や、ユーザーのレジストリ キーを設定する場合、ユーザーの特定のローカル ディレクトリにアクセスする場合、ネットワーク共有にアクセスする場合などです。 **PSDSCRunAsCredential** に指定するアカウントに対しては、ターゲット コンピューターによって、 **SeInteractiveLogonRight** が要求されます。 詳細については、「[Account Rights Constants (アカウントの権限に関する定数)](/windows/desktop/secauthz/account-rights-constants)」をご覧ください。

すべての DSC リソースには、任意のユーザー資格情報に設定できる **PsDscRunAsCredential** プロパティがあります ( [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクト)。 資格情報は、構成内でプロパティの値としてハードコーディングするか、[Get-Credential](/powershell/module/Microsoft.PowerShell.Security/Get-Credential) に対して値を設定することができます。後者の場合は、構成のコンパイル時に資格情報を入力するように求めるプロンプトが表示されます (構成のコンパイルの詳細については、[構成](configurations.md)に関する記事を参照してください)。

> [!NOTE]
> PowerShell 5.0 では、複合リソースを呼び出す構成での **PsDscRunAsCredential** プロパティの使用はサポートされていませんでした。 PowerShell 5.1 では、複合リソースを呼び出す構成で **PsDscRunAsCredential** プロパティがサポートされています。 **PsDscRunAsCredential** プロパティは PowerShell 4.0 では使用できません。

次の例では、`Get-Credential` を使って、ユーザーに資格情報を入力するようにプロンプトが出ます。 また、 **Registry** リソースを使って、Windows のコマンド プロンプト ウィンドウの背景色を指定するレジストリ キーを変更します。

```powershell
Configuration ChangeCmdBackGroundColor
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    Node $AllNodes.NodeName
    {
        Registry CmdPath
        {
            Key                  = 'HKEY_CURRENT_USER\SOFTWARE\Microsoft\Command Processor'
            ValueName            = 'DefaultColor'
            ValueData            = '1F'
            ValueType            = 'DWORD'
            Ensure               = 'Present'
            Force                = $true
            Hex                  = $true
            PsDscRunAsCredential = Get-Credential
        }
    }
}

$configData = @{
    AllNodes = @(
        @{
            NodeName             = 'localhost';
            PSDscAllowDomainUser = $true
            CertificateFile      = 'C:\publicKeys\targetNode.cer'
            Thumbprint           = '7ee7f09d-4be0-41aa-a47f-96b9e3bdec25'
        }
    )
}

ChangeCmdBackGroundColor -ConfigurationData $configData
```

> [!NOTE]
> この例では、有効な証明書が `C:\publicKeys\targetNode.cer` に存在することと、示されている値がその証明書の拇印であることを想定しています。 DSC 構成 MOF ファイル内の資格情報を暗号化する方法については、「[MOF ファイルのセキュリティ保護](../pull-server/secureMOF.md)」をご覧ください。
