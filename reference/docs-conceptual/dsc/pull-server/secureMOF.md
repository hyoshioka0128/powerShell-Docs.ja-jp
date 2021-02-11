---
ms.date: 07/06/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: MOF ファイルのセキュリティ保護
description: この記事では、ターゲット ノードが MOF ファイルを暗号化したことを確認する方法について説明します。
ms.openlocfilehash: ca94a901468626e5644880574457d899a012d311
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97090349"
---
# <a name="securing-the-mof-file"></a>MOF ファイルのセキュリティ保護

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

DSC では、ローカル構成マネージャー (LCM) が必要な終了状態を実装する、MOF ファイルに格納されている情報を適用することで、サーバー ノードの構成を管理します。 このファイルには構成の詳細が含まれているため、安全に保つことが重要です。 この記事では、ターゲット ノードがファイルを暗号化したことを確認する方法について説明します。

PowerShell バージョン 5.0 以降では、`Start-DSCConfiguration` コマンドレットを使用してノードに適用されている場合、MOF ファイル全体が既定で暗号化されます。 この記事で説明されているプロセスが必要になるのは、証明書が管理されていない場合にプル サービス プロトコルを使用してソリューションを実装する場合のみです。これにより、ターゲット ノードによってダウンロードされた構成を暗号化解除し、適用される前にシステムで読み取れるようになります (たとえば、Windows Server でプル サービスが使用できるようになります)。 [Azure Automation DSC](/azure/automation/automation-dsc-overview) に登録されているノードには自動的に証明書がインストールされ、サービスによって管理されます。管理オーバーヘッドは必要ありません。

> [!NOTE]
> このトピックでは、暗号化に使用する証明書について説明します。 暗号化には、自己署名証明書で十分です。秘密キーは常に秘密に保たれますし、暗号化はドキュメントの信頼性を意味しないからです。 認証の目的では、自己署名証明書を使用 _しないでください_。 認証の目的では、常に信頼された証明機関 (CA) からの証明書を使用する必要があります。

## <a name="prerequisites"></a>前提条件

DSC 構成のセキュリティ保護に使用される資格情報を正常に暗号化するには、次のものが必要になります。

- **証明書を発行して配布するための手段**。 このトピックとその例では、Active Directory 証明機関を使用することを前提としています。 Active Directory 証明書サービスの背景情報の詳細については、「[Active Directory 証明書サービスの概要](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11))」を参照してください。
- **ターゲット ノードへの管理アクセス**。
- **各ターゲット ノードでは、暗号化可能な証明書がそれぞれの個人用ストアに保存されています**。 Windows PowerShell では、ストアへのパスは Cert:\LocalMachine\My です。 このトピックの例では、"ワークステーション認証" テンプレートを使用します。このテンプレートは、その他の証明書テンプレートと共に、[[既定の証明書テンプレート]](/previous-versions/windows/it-pro/windows-server-2003/cc740061(v=ws.10)) にあります。
- ターゲット ノード以外のコンピューターでこの構成を実行する場合は、**証明書の公開キーをエクスポート** して、構成の実行元であるコンピューターにインポートします。 **公開** キーのみをエクスポートし、秘密キーは安全に保護してください。

> [!NOTE]
> スクリプト リソースには、暗号化に関する制限があります。 詳細については、「[スクリプト リソース](../reference/resources/windows/scriptResource.md#known-limitations)」を参照してください。

## <a name="overall-process"></a>全体的なプロセス

 1. ターゲット ノードごとに証明書のコピーが保持され、構成コンピューターに公開キーと拇印が保持されるように、証明書、キー、および拇印をセットアップします。
 1. 公開キーのパスと拇印が含まれた構成データ ブロックを作成します。
 1. ターゲット ノードに必要な構成を定義し、ターゲット ノードで暗号化の解除をセットアップする構成スクリプトを作成します。暗号化の解除は、証明書とその拇印を使用して構成データを解読するように、ローカル構成マネージャーに指示することによって行います。
 1. 構成を実行すると、ローカル構成マネージャーの設定が行われ、DSC 構成が起動します。

![資格情報の暗号化のプロセス フロー](media/secureMOF/CredentialEncryptionDiagram1.png)

## <a name="certificate-requirements"></a>証明書の要件

資格情報の暗号化を指定するには、公開キー証明書が、DSC 構成の作成に使用されているコンピューターから **信頼された**_ターゲット ノード_ で使用可能である必要があります。 この公開キー証明書を DSC 資格情報の暗号化に使うには、満たす必要のある特定の要件があります。

1. **キー使用法**:
   - 含める必要がある: "KeyEncipherment" と "DataEncipherment"。
   - 含める "_べきではない_": "Digital Signature"。
1. **拡張キー使用法**:
   - 含める必要がある: ドキュメントの暗号化 (1.3.6.1.4.1.311.80.1)。
   - 含める "_べきではない_": クライアント認証 (1.3.6.1.5.5.7.3.2) とサーバー認証 (1.3.6.1.5.5.7.3.1)。
1. 証明書の秘密キーが *ターゲット ノード_ で使用可能であること。
1. 証明書の **プロバイダー** は、"Microsoft RSA SChannel Cryptographic Provider" でなければならない。

> [!IMPORTANT]
> 'Digital Signature' のキー使用法またはいずれかの認証 EKU が含まれている証明書を使用することはできますが、暗号化キーの誤用や、攻撃に対する脆弱性を引き起こしやすくなります。 したがって、ベスト プラクティスとしては、DSC の資格情報をセキュリティで保護する目的で特別に作成した証明書を使い、その証明書ではこれらのキー使用法と EKU を省略することをお勧めします。

_ターゲット ノード_ にある既存の証明書で、これらの条件を満たしているものがあれば、DSC 資格情報をセキュリティで保護するために使うことができます。

## <a name="certificate-creation"></a>証明書の作成

必要な暗号化証明書 (公開/秘密キー ペア) を作成して使用するための方法は 2 つあります。

1. **ターゲット ノード** 上に作成し、公開キーのみを **オーサリング ノード** にエクスポートする
1. **オーサリング ノード** 上に作成し、キー ペア全体を **ターゲット ノード** にエクスポートする

MOF の証明書の暗号化を解除するための秘密キーが常にターゲット ノードで保持されるため、1 つ目の方法をお勧めします。

### <a name="creating-the-certificate-on-the-target-node"></a>ターゲット ノードでの証明書の作成

秘密キーは、**ターゲット ノード** で MOF の暗号化解除に使用されるため、秘密に保つ必要があります。そのための最も簡単な方法は、**ターゲット ノード** で秘密キー証明書を作成し、DSC 構成を MOF ファイルに作成するために使用されるコンピューターに **公開キー証明書** をコピーすることです。 次に例を示します。

1. **ターゲット ノード** で証明書を作成します。
1. 公開キー証明書を **ターゲット ノード** にエクスポートします。
1. 公開キー証明書を **オーサリング ノード** の **マイ** 証明書ストアにインポートします。

#### <a name="on-the-target-node-create-and-export-the-certificate"></a>ターゲット ノード: 証明書を作成してエクスポートする

> ターゲット ノード: Windows Server 2016 および Windows 10

```powershell
# note: These steps need to be performed in an Administrator PowerShell session
$cert = New-SelfSignedCertificate -Type DocumentEncryptionCertLegacyCsp -DnsName 'DscEncryptionCert' -HashAlgorithm SHA256
# export the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

エクスポートしたら、`DscPublicKey.cer` を **オーサリング ノード** にコピーする必要があります。

> ターゲット ノード: Windows Server 2012 R2/Windows 8.1 以前

> [!WARNING]
> Windows 10 および Windows Server 2016 より前の Windows オペレーティング システムの `New-SelfSignedCertificate` コマンドレットでは、**Type** パラメーターがサポートされていないため、これらのオペレーティング システムでは、他の方法でこの証明書を作成する必要があります。 この場合は、`makecert.exe` または `certutil.exe` を使って証明書を作成できます。 この例では、Microsoft スクリプト センターの [New-SelfSignedCertificateEx.ps1](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6) スクリプトを、証明書を作成する代替方法として使用しています。 このスクリプトの更新バージョンは、PowerShell ギャラリーの [PSPKI](https://www.powershellgallery.com/packages/PSPKI/) モジュールで入手できます。

```powershell
# note: These steps need to be performed in an Administrator PowerShell session
# and in the folder that contains New-SelfSignedCertificateEx.ps1
. .\New-SelfSignedCertificateEx.ps1
New-SelfsignedCertificateEx `
    -Subject "CN=${ENV:ComputerName}" `
    -EKU 'Document Encryption' `
    -KeyUsage 'KeyEncipherment, DataEncipherment' `
    -SAN ${ENV:ComputerName} `
    -FriendlyName 'DSC Credential Encryption certificate' `
    -Exportable `
    -StoreLocation 'LocalMachine' `
    -KeyLength 2048 `
    -ProviderName 'Microsoft Enhanced Cryptographic Provider v1.0' `
    -AlgorithmName 'RSA' `
    -SignatureAlgorithm 'SHA256'
# Locate the newly created certificate
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {
        ($_.FriendlyName -eq 'DSC Credential Encryption certificate') -and ($_.Subject -eq "CN=${ENV:ComputerName}")
    } | Select-Object -First 1
# export the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

エクスポートしたら、```DscPublicKey.cer``` を **オーサリング ノード** にコピーする必要があります。

#### <a name="on-the-authoring-node-import-the-certs-public-key"></a>オーサリング ノード: 証明書の公開キーをインポートする

```powershell
# Import to the my store
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\My
```

### <a name="creating-the-certificate-on-the-authoring-node"></a>オーサリング ノードでの証明書の作成

**オーサリング ノード** で暗号化証明書を作成し、**秘密キー** と共に PFX ファイルとしてエクスポートして、**ターゲット ノード** にインポートすることもできます。 これは、DSC 資格情報の暗号化を実装するために _Nano Server_ で実行されている現在の手法です。 PFX はパスワードで保護されていますが、転送中はセキュリティで保護する必要があります。 次に例を示します。

1. **オーサリング ノード** で証明書を作成します。
1. **オーサリング ノード** で、秘密キーを含む証明書をエクスポートします。
1. **オーサリング ノード** から秘密キーを削除します。ただし、公開キー証明書を **マイ** ストアに保管しておきます。
1. 秘密キー証明書を **ターゲット ノード** のマイ (個人用) 証明書ストアにインポートします。
   - これはルート ストアに追加されるため、**ターゲット ノード** で信頼されるようになります。

#### <a name="on-the-authoring-node-create-and-export-the-certificate"></a>オーサリング ノード: 証明書を作成してエクスポートする

> ターゲット ノード: Windows Server 2016 および Windows 10

```powershell
# note: These steps need to be performed in an Administrator PowerShell session
$cert = New-SelfSignedCertificate -Type DocumentEncryptionCertLegacyCsp -DnsName 'DscEncryptionCert' -HashAlgorithm SHA256
# export the private key certificate
$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText
$cert | Export-PfxCertificate -FilePath "$env:temp\DscPrivateKey.pfx" -Password $mypwd -Force
# remove the private key certificate from the node but keep the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
$cert | Remove-Item -Force
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\My
```

エクスポートしたら、`DscPrivateKey.pfx` を **ターゲット ノード** にコピーする必要があります。

> ターゲット ノード: Windows Server 2012 R2/Windows 8.1 以前

> [!WARNING]
> Windows 10 および Windows Server 2016 より前の Windows オペレーティング システムの `New-SelfSignedCertificate` コマンドレットでは、**Type** パラメーターがサポートされていないため、これらのオペレーティング システムでは、他の方法でこの証明書を作成する必要があります。 この場合は、`makecert.exe` または `certutil.exe` を使って証明書を作成できます。 また、別の方法として、Microsoft スクリプト センターから [New-SelfSignedCertificateEx.ps1](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6) スクリプトをダウンロードし、それを使用して証明書を作成することもできます。

```powershell
# note: These steps need to be performed in an Administrator PowerShell session
# and in the folder that contains New-SelfSignedCertificateEx.ps1
. .\New-SelfSignedCertificateEx.ps1
New-SelfsignedCertificateEx `
    -Subject "CN=${ENV:ComputerName}" `
    -EKU 'Document Encryption' `
    -KeyUsage 'KeyEncipherment, DataEncipherment' `
    -SAN ${ENV:ComputerName} `
    -FriendlyName 'DSC Credential Encryption certificate' `
    -Exportable `
    -StoreLocation 'LocalMachine' `
    -KeyLength 2048 `
    -ProviderName 'Microsoft Enhanced Cryptographic Provider v1.0' `
    -AlgorithmName 'RSA' `
    -SignatureAlgorithm 'SHA256'
# Locate the newly created certificate
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {
        ($_.FriendlyName -eq 'DSC Credential Encryption certificate') -and ($_.Subject -eq "CN=${ENV:ComputerName}")
    } | Select-Object -First 1
# export the public key certificate
$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText
$cert | Export-PfxCertificate -FilePath "$env:temp\DscPrivateKey.pfx" -Password $mypwd -Force
# remove the private key certificate from the node but keep the public key certificate
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
$cert | Remove-Item -Force
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\My
```

#### <a name="on-the-target-node-import-the-certs-private-key-as-a-trusted-root"></a>ターゲット ノード: 証明書の秘密キーを信頼されたルートとしてインポートする

```powershell
# Import to the root store so that it is trusted
$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText
Import-PfxCertificate -FilePath "$env:temp\DscPrivateKey.pfx" -CertStoreLocation Cert:\LocalMachine\My -Password $mypwd > $null
```

## <a name="configuration-data"></a>構成データ

構成データ ブロックでは、操作対象のターゲット ノード、資格情報を暗号化するかどうか、暗号化の手段、およびその他の情報を定義します。 構成データ ブロックの詳細については、「[Separating Configuration and Environment Data (構成データと環境データの分離)](../configurations/configData.md)」を参照してください。

資格情報の暗号化に関連するノードごとに構成可能な要素は次のとおりです。

- **NodeName** - 資格情報の暗号化が構成されているターゲット ノードの名前。
- **PsDscAllowPlainTextPassword** - 暗号化されていない資格情報をこのノードに渡してよいかどうか。 これは **推奨されません**。
- **Thumbprint** - _ターゲット ノード_ 上で DSC 構成に含まれる資格情報を復号化するために使用される証明書の拇印です。 **この証明書は、ターゲット ノード上のローカル コンピューターの証明書ストアに存在する必要があります。**
- **CertificateFile** - _ターゲット ノード_ 用の証明書を暗号化するために使用する必要のある証明書ファイル (公開キーのみを含む)。 これは、DER Encoded Binary X.509 または Base-64 encoded X.509 のいずれかの形式の証明書ファイルである必要があります。

この例に示す構成データ ブロックでは、処理対象のターゲット ノードの名前を targetNode とし、公開キー証明書ファイル (targetNode.cer という名前) へのパス、および公開キーの拇印を指定しています。

```powershell
$ConfigData= @{
    AllNodes = @(
            @{
                # The name of the node we are describing
                NodeName = "targetNode"

                # The path to the .cer file containing the
                # public key of the Encryption Certificate
                # used to encrypt credentials for this node
                CertificateFile = "C:\publicKeys\targetNode.cer"


                # The thumbprint of the Encryption Certificate
                # used to decrypt the credentials on target node
                Thumbprint = "AC23EA3A9E291A75757A556D0B71CBBF8C4F6FD8"
            }
        )
    }
```

## <a name="configuration-script"></a>構成スクリプト

構成スクリプト自体では、`PsCredential` パラメーターを使用して、できる限り短時間で資格情報が格納されるようにします。 ここに示した例を実行すると、DSC では資格情報の入力を求め、構成データ ブロック内のターゲット ノードに関連付けられている CertificateFile を使用して MOF ファイルを暗号化します。 このコード例では、ユーザーに対してセキュリティ保護された共有からファイルをコピーしています。

```powershell
configuration CredentialEncryptionExample
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PsCredential] $credential
    )

    Node $AllNodes.NodeName
    {
        File exampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $credential
        }
    }
}
```

## <a name="setting-up-decryption"></a>暗号化の解除のセットアップ

[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) が機能するようにするには、各ターゲット ノードのローカル構成マネージャーに対して、どの証明書を使用して資格情報の暗号化を解除するかを指示し、CertificateID リソースを使用して証明書の拇印を検証する必要があります。 この例の関数は、適切なローカル証明書を検出します (使用する証明書が正しく検出されるようにカスタマイズすることが必要になる場合もあります)。

```powershell
# Get the certificate that works for encryption
function Get-LocalEncryptionCertificateThumbprint
{
    (dir Cert:\LocalMachine\my) | %{
        # Verify the certificate is for Encryption and valid
        if ($_.PrivateKey.KeyExchangeAlgorithm -and $_.Verify())
        {
            return $_.Thumbprint
        }
    }
}
```

拇印で証明書を識別する場合は、その値を使用するように構成スクリプトを更新します。

```powershell
configuration CredentialEncryptionExample
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PsCredential] $credential
    )

    Node $AllNodes.NodeName
    {
        File exampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $credential
        }

        LocalConfigurationManager
        {
             CertificateId = $node.Thumbprint
        }
    }
}
```

## <a name="running-the-configuration"></a>構成の実行

この時点では、構成を実行すると、次の 2 つのファイルが出力されます。

- `*.meta.mof ` ファイル。ローカル コンピューター ストアに格納され、拇印で識別される証明書を使用して資格情報の暗号化を解除するように、ローカル構成マネージャーを構成します。
  [Set-DscLocalConfigurationManager](/powershell/module/psdesiredstateconfiguration/Set-DscLocalConfigurationManager) は、`*.meta.mof ` ファイルを適用します。
- 構成を実際に適用する MOF ファイル。 Start-DscConfiguration は、構成を適用します。

次のコマンドがこれらの手順を実施します。

```powershell
Write-Host "Generate DSC Configuration..."
CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample

Write-Host "Setting up LCM to decrypt credentials..."
Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose

Write-Host "Starting Configuration..."
Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose
```

この例では、DSC 構成をターゲット ノードにプッシュします。 DSC プル サーバーが利用可能な場合は、DSC プル サーバーを使って DSC 構成を適用することもできます。

DSC プル サーバーを使って DSC 構成を適用する方法の詳細については、「[DSC プル クライアントのセットアップ](pullClient.md)」を参照してください。

## <a name="credential-encryption-module-example"></a>資格情報暗号化モジュールの例

次に、これらのすべての手順に加え、公開キーをエクスポートおよびコピーするヘルパー コマンドレットが組み込まれた完全な例を示します。

```powershell
# A simple example of using credentials
configuration CredentialEncryptionExample
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PsCredential] $credential
    )

    Node $AllNodes.NodeName
    {
        File exampleFile
        {
            SourcePath = "\\server\share\file.txt"
            DestinationPath = "C:\Users\user"
            Credential = $credential
        }

        LocalConfigurationManager
        {
            CertificateId = $node.Thumbprint
        }
    }
}

# A Helper to invoke the configuration, with the correct public key
# To encrypt the configuration credentials
function Start-CredentialEncryptionExample
{
    [CmdletBinding()]
    param ($computerName)

    [string] $thumbprint = Get-EncryptionCertificate -computerName $computerName -Verbose
    Write-Verbose "using cert: $thumbprint"

    $certificatePath = join-path -Path "$env:SystemDrive\$script:publicKeyFolder" -childPath "$computername.EncryptionCertificate.cer"

    $ConfigData=    @{
        AllNodes = @(
                        @{
                            # The name of the node we are describing
                            NodeName = "$computerName"

                            # The path to the .cer file containing the
                            # public key of the Encryption Certificate
                            CertificateFile = "$certificatePath"

                            # The thumbprint of the Encryption Certificate
                            # used to decrypt the credentials
                            Thumbprint = $thumbprint
                        };
                    );
    }

    Write-Verbose "Generate DSC Configuration..."
    CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample `
        -credential (Get-Credential -UserName "$env:USERDOMAIN\$env:USERNAME" -Message "Enter credentials for configuration")

    Write-Verbose "Setting up LCM to decrypt credentials..."
    Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose

    Write-Verbose "Starting Configuration..."
    Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose
}

#region HelperFunctions

# The folder name for the exported public keys
$script:publicKeyFolder = "publicKeys"

# Get the certificate that works for encryptions
function Get-EncryptionCertificate
{
    [CmdletBinding()]
    param ($computerName)

    $returnValue= Invoke-Command -ComputerName $computerName -ScriptBlock {
        $certificates = dir Cert:\LocalMachine\my

        $certificates | %{
                    # Verify the certificate is for Encryption and valid
            if ($_.PrivateKey.KeyExchangeAlgorithm -and $_.Verify())
            {
                # Create the folder to hold the exported public key
                $folder= Join-Path -Path $env:SystemDrive\ -ChildPath $using:publicKeyFolder
                if (! (Test-Path $folder))
                {
                    md $folder | Out-Null
                }

                # Export the public key to a well known location
                $certPath = Export-Certificate -Cert $_ -FilePath (Join-Path -path $folder -childPath "EncryptionCertificate.cer")

                # Return the thumbprint, and exported certificate path
                return @($_.Thumbprint,$certPath);
            }
        }
    }

    Write-Verbose "Identified and exported cert..."
    # Copy the exported certificate locally
    $destinationPath = join-path -Path "$env:SystemDrive\$script:publicKeyFolder" -childPath "$computername.EncryptionCertificate.cer"
    Copy-Item -Path (join-path -path \\$computername -childPath $returnValue[1].FullName.Replace(":","$"))  $destinationPath | Out-Null

    # Return the thumbprint
    return $returnValue[0]
}

Start-CredentialEncryptionExample
```
