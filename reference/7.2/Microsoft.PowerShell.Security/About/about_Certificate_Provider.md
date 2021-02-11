---
description: Certificate
Locale: en-US
ms.date: 06/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/about/about_certificate_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: 証明書プロバイダー
ms.openlocfilehash: 78536024e8e953a70485a7d950b187ba9a3fc405
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601220"
---
# <a name="certificate-provider"></a>証明書プロバイダー

## <a name="provider-name"></a>プロバイダー名

Certificate

## <a name="drives"></a>ドライブ

`Cert:`

## <a name="capabilities"></a>機能

**ShouldProcess**

## <a name="short-description"></a>簡単な説明

PowerShell での x.509 証明書ストアと証明書へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

Powershell **証明書** プロバイダーを使用すると、powershell で証明書と証明書ストアを取得、追加、変更、消去、削除できます。

**証明書** ドライブは、コンピューター上の証明書ストアと証明書を含む階層的な名前空間です。

**証明書** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)
- [Invoke-Item](xref:Microsoft.PowerShell.Management.Invoke-Item)
- [Move-Item](xref:Microsoft.PowerShell.Management.Move-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Get-ItemProperty](xref:Microsoft.PowerShell.Management.Get-ItemProperty)
- [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)
- [Clear-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)
- [Get-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Get-AuthenticodeSignature)
- [Set-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Set-AuthenticodeSignature)

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

証明書ドライブは、次の種類を公開します。

- ストアの場所 (X509StoreLocation)。現在のユーザーとすべてのユーザーの証明書をグループ化する高レベルのコンテナーです。 各システムに CurrentUser と LocalMachine (すべてのユーザー) のストアの場所があります。

- 証明書ストア (System.security.cryptography.x509certificates.x509certificate2. X509Store)。証明書が保存および管理される物理ストアです。

- X.509 **system.security.cryptography.x509certificates.x509certificate2. X509Certificate2** 証明書。各証明書は、コンピューター上の x.509 証明書を表します。
  証明書は拇印により識別されます。

## <a name="navigating-the-certificate-drive"></a>証明書のドライブを移動する

**証明書** プロバイダーは、PowerShell で証明書の名前空間をドライブとして公開し `Cert:` ます。 このコマンドは、コマンドを使用して、 `Set-Location` 現在の場所を LocalMachine ストアの場所にあるルート証明書ストアに変更します。 円記号 ( \\ ) またはスラッシュ (/) を使用して、ドライブのレベルを指定し `Cert:` ます。

```powershell
Set-Location Cert:
```

他の PowerShell ドライブから証明書プロバイダーを使用することもできます。 別の場所からエイリアスを参照するには、 `Cert:` パス内のドライブ名を使用します。

```powershell
PS Cert:\> Set-Location -Path LocalMachine\Root
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、次のように入力します。

```powershell
Set-Location C:
```

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスです。これ `cd` は、 [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスです。
> と `pwd` は、 [Get Location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

## <a name="displaying-the-contents-of-the-cert-drive"></a>Cert: ドライブの内容を表示しています

このコマンドは、 `Get-ChildItem` コマンドレットを使用して、CurrentUser 証明書ストアの場所に証明書ストアを表示します。

ドライブにない場合は `Cert:` 、絶対パスを使用します。

```powershell
PS Cert:\CurrentUser\> Get-ChildItem
```

### <a name="displaying-certificate-properties-within-the-cert-drive"></a>Cert: ドライブ内の証明書のプロパティを表示しています

この例では、を使用して証明書を取得 `Get-Item` し、変数に格納します。
この例では、を使用して、新しい証明書スクリプトプロパティ (**DnsNameList**、 **EnhancedKeyUsageList**、 **SendAsTrustedIssuer**) を示し `Select-Object` ます。

```powershell
$c = Get-Item cert:\LocalMachine\My\52A149D0393CE8A8D4AF0B172ED667A9E3A1F44E
$c | Format-List DnsNameList, EnhancedKeyUsageList, SendAsTrustedIssuer
```

```output
DnsNameList          : {SERVER01.contoso.com}
EnhancedKeyUsageList : {WiFi-Machine (1.3.6.1.4.1.311.42.2.6),
                       Client Authentication (1.3.6.1.5.5.7.3.2)}
SendAsTrustedIssuer  : False
```

### <a name="find-all-codesigning-certificates"></a>すべてのコード署名証明書の検索

このコマンドは、コマンドレットの **Codesigningcert** と **再帰** パラメーターを使用して、 `Get-ChildItem` コード署名機関を持つコンピューター上のすべての証明書を取得します。

```powershell
Get-ChildItem -Path cert: -CodeSigningCert -Recurse
```

### <a name="find-expired-certificates"></a>期限切れの証明書の検索

このコマンドは、コマンドレットの **ExpiringInDays** パラメーターを使用して、 `Get-ChildItem` 次の30日以内に有効期限が切れる証明書を取得します。

```powershell
Get-ChildItem -Path cert:\LocalMachine\WebHosting -ExpiringInDays 30
```

### <a name="find-server-ssl-certificates"></a>サーバーの SSL 証明書を検索する

このコマンドは、コマンドレットの **Sslserverauthentication** パラメーターを使用して `Get-ChildItem` 、My および webhosting ストア内のすべてのサーバー SSL 証明書を取得します。

```powershell
Get-ChildItem -Path cert:\LocalMachine\My, cert:\LocalMachine\WebHosting `
  -SSLServerAuthentication
```

### <a name="find-expired-certificates-on-remote-computers"></a>リモートコンピューターで有効期限が切れた証明書を検索する

このコマンドは、コマンドレットを使用して、 `Invoke-Command` `Get-ChildItem` Srv01 コンピューターと Srv02 コンピューターでコマンドを実行します。 **ExpiringInDays** パラメーターの値がゼロ (0) の場合、Srv01 コンピューターと Srv02 コンピューターの有効期限が切れている証明書が取得されます。

```powershell
Invoke-Command -ComputerName Srv01, Srv02 {Get-ChildItem -Path cert:\* `
  -Recurse -ExpiringInDays 0}
```

### <a name="combining-filters-to-find-a-specific-set-of-certificates"></a>フィルターを組み合わせて特定の証明書セットを検索する

このコマンドは、LocalMachine ストアの場所にある、次の属性を持つすべての証明書を取得します。

- DNS 名に "fabrikam"
- EKU 内の "クライアント認証"
- `$true` **SendAsTrustedIssuer** プロパティの値
- 次の30日以内に期限切れにならないようにします。

**Notafter** プロパティには、証明書の有効期限が格納されます。

```powershell
[DateTime] $ValidThrough = (Get-Date) + (New-TimeSpan -Days 30)
Get-ChildItem -Path cert:\* -Recurse -DNSName "*fabrikam*" `
  -EKU "*Client Authentication*" | Where-Object {
                                     $_.SendAsTrustedIssuer -and `
                                     $_.NotAfter -gt $ValidThrough
                                   }
```

## <a name="opening-the-certificates-mmc-snap-in"></a>証明書 MMC スナップインを開く

`Invoke-Item`コマンドレットは、既定のアプリケーションを使用して、指定したパスを開きます。 証明書の場合、既定のアプリケーションは証明書 MMC スナップインです。

このコマンドを実行すると、証明書 MMC スナップインが開きます。特定の証明書を管理できます。

```powershell
Invoke-Item cert:\CurrentUser\my\6B8223358119BB08840DEE50FD8AF9EA776CE66B
```

## <a name="copying-certificates"></a>証明書のコピー

証明書のコピーは **証明書** プロバイダーでサポートされていません。 証明書をコピーしようとすると、このエラーが表示されます。

```
$path = "Cert:\LocalMachine\Root\E2C0F6662D3C569705B4B31FE2CBF3434094B254"
PS Cert:\LocalMachine\> Copy-Item -Path $path -Destination .\CA\
Copy-Item : Provider operation stopped because the provider does not support
this operation.
At line:1 char:1
+ Copy-Item -Path $path -Destination .\CA\
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotImplemented: (:) [Copy-Item],
                              PSNotSupportedException
    + FullyQualifiedErrorId : NotSupported,
                              Microsoft.PowerShell.Commands.CopyItemCommand
```

## <a name="moving-certificates"></a>証明書の移動

### <a name="move-all-ssl-server-authentication-certs-to-the-webhosting-store"></a>すべての SSL サーバー認証証明書を Web ホスティングストアに移動する

このコマンドは、 `Move-Item` コマンドレットを使用して、My ストアから WebHosting ストアに証明書を移動します。

`Move-Item` は、証明書ストアを移動せず、証明書を LocalMachine から CurrentUser に移動するなど、別のストアの場所に証明書を移動しません。 `Move-Item`コマンドレットでは証明書を移動しますが、秘密キーは移動しません。

このコマンドは、コマンドレットの **Sslserverauthentication** パラメーターを使用して `Get-ChildItem` 、MY 証明書ストア内の SSL サーバー認証証明書を取得します。

返された証明書はコマンドレットにパイプ処理され、 `Move-Item` 証明書が WebHosting ストアに移動します。

```powershell
Get-ChildItem cert:\LocalMachine\My -SSLServerAuthentication | Move-Item `
  -Destination cert:\LocalMachine\WebHosting
```

## <a name="deleting-certificates-and-private-keys"></a>証明書と秘密キーを削除する

`Remove-Item`コマンドレットは、指定した証明書を削除します。 `-DeleteKey`動的パラメーターは、秘密キーを削除します。

### <a name="delete-a-certificate-from-the-ca-store"></a>CA ストアから証明書を削除する

このコマンドは CA 証明書ストアから証明書を削除するが、関連付けられている秘密キーはそのまま残ります。

ドライブでは、 `Cert:` `Remove-Item` コマンドレットは **deletekey**、 **Path**、 **WhatIf**、 **Confirm** パラメーターのみをサポートしています。 その他のパラメーターはすべて無視されます。

```powershell
Remove-Item cert:\LocalMachine\CA\5DDC44652E62BF9AA1116DC41DE44AB47C87BDD0
```

### <a name="delete-a-certificate-using-a-wildcards-in-the-dns-name"></a>DNS 名にワイルドカードを使用して証明書を削除する

このコマンドは、DNS 名に「Fabrikam」が含まれるすべての証明書を削除します。 コマンドレットの **DNSName** パラメーターを使用して `Get-ChildItem` 証明書を取得し、コマンドレットを使用して証明書を `Remove-Item` 削除します。

```powershell
Get-ChildItem -Path cert:\LocalMachine -DnsName *Fabrikam* | Remove-Item
```

### <a name="delete-private-keys-from-a-remote-computer"></a>リモートコンピューターから秘密キーを削除する

この一連のコマンドは委任を有効化し、リモート コンピューターにある証明書と関連秘密キーを削除します。 リモート コンピューターの秘密キーを削除するには、委任された資格情報を利用する必要があります。

コマンドレットを使用して、 `Enable-WSManCredSSP` S1 リモートコンピューター上のクライアントで Credential Security Service Provider (CredSSP) 認証を有効にします。
CredSSP は委任認証を許可します。

```powershell
Enable-WSManCredSSP -Role Client -DelegateComputer S1
```

コマンドレットを使用して、 `Connect-WSMan` S1 コンピューターをローカルコンピューター上の WinRM サービスに接続します。 このコマンドが完了すると、S1 コンピューターが PowerShell のローカルドライブに表示され `WSMan:` ます。

```powershell
Connect-WSMan -ComputerName S1 -Credential Domain01\Admin01
```

これで、WSMan: ドライブの Set-Item コマンドレットを使用して、WinRM サービスの CredSSP 属性を有効にすることができます。

```powershell
Set-Item -Path WSMan:\S1\Service\Auth\CredSSP -Value $true
```

コマンドレットを使用して s1 コンピューターでリモートセッションを開始 `New-PSSession` し、CredSSP 認証を指定します。 変数にセッションを保存し `$s` ます。

```powershell
$s  = New-PSSession S1 -Authentication CredSSP -Credential Domain01\Admin01
```

最後に、コマンドレットを使用して、 `Invoke-Command` `Remove-Item` 変数内のセッションでコマンドを実行し `$s` ます。 この `Remove-Item` コマンドは、 **deletekey** パラメーターを使用して、指定された証明書と共に秘密キーを削除します。

```powershell
Invoke-Command -Session $s { Remove-Item `
  -Path cert:\LocalMachine\My\D2D38EBA60CAA1C12055A2E1C83B15AD450110C2 `
  -DeleteKey
  }
```

### <a name="delete-expired-certificates"></a>期限切れの証明書の削除

このコマンドは、コマンドレットの **ExpiringInDays** パラメーターに値0を指定して、 `Get-ChildItem` 有効期限が切れた web ホスティングストア内の証明書を取得します。

返された証明書を含む変数は、パイプを使用してコマンドレットに渡され `Remove-Item` 、削除されます。 このコマンドは、 **deletekey** パラメーターを使用して、証明書と共に秘密キーを削除します。

```powershell
$expired = Get-ChildItem cert:\LocalMachine\WebHosting -ExpiringInDays 0
$expired | Remove-Item -DeleteKey
```

## <a name="creating-certificates"></a>証明書の作成

`New-Item`コマンドレットでは、**証明書** プロバイダーに新しい証明書を作成しません。 [新しい-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)コマンドレットを使用して、テスト用の証明書を作成します。

## <a name="creating-certificate-stores"></a>証明書ストアの作成

Cert: ドライブでは、 `New-Item` コマンドレットは LocalMachine ストアの場所に証明書ストアを作成します。 **Name**、 **Path**、 **WhatIf**、 **Confirm** の各パラメーターがサポートされています。 その他のパラメーターはすべて無視されます。 このコマンドは、新しい証明書ストアを表す **system.security.cryptography.x509certificates.x509certificate2. X509Store** を返します。

このコマンドは LocalMachine ストアの場所で「CustomStore」という名前の新しい証明書ストアを作成します。

```powershell
New-Item -Path cert:\LocalMachine\CustomStore
```

### <a name="create-a-new-certificate-store-on-a-remote-computer"></a>リモートコンピューターに新しい証明書ストアを作成する

このコマンドは Server01 コンピューターの LocalMachine ストアの場所で「HostingStore」という名前の新しい証明書ストアを作成します。

このコマンドは、コマンドレットを使用して、 `Invoke-Command` `New-Item` Server01 コンピューターでコマンドを実行します。 このコマンドは、新しい証明書ストアを表す **system.security.cryptography.x509certificates.x509certificate2. X509Store** を返します。

```powershell
Invoke-Command { New-Item -Path cert:\LocalMachine\CustomStore } `
  -ComputerName Server01
```

## <a name="creating-client-certificates-for-ws-man"></a>WS-Man 用のクライアント証明書の作成

このコマンドは、 **ws-management** クライアントが使用できる **ClientCertificate** エントリを作成します。 新しい **ClientCertificate** は、 **ClientCertificate** ディレクトリの下に "ClientCertificate_1234567890" として表示されます。 すべてのパラメーターが必須です。 **発行者** は、発行者証明書の拇印である必要があります。

```powershell
$cred = Get-Credential
New-Item -Path WSMan:\localhost\ClientCertificate `
         -Issuer 1b3fd224d66c6413fe20d21e38b304226d192dfe `
         -URI wmicimv2/* -Credential $cred
```

## <a name="deleting-certificate-stores"></a>証明書ストアの削除

### <a name="delete-a-certificate-store-from-a-remote-computer"></a>リモートコンピューターから証明書ストアを削除する

このコマンドは、コマンドレットを使用して、 `Invoke-Command` `Remove-Item` S1 および S2 コンピューターでコマンドを実行します。 このコマンドには、 `Remove-Item` **再帰** パラメーターが含まれています。これにより、ストア内の証明書が削除されてからストアが削除されます。

```powershell
Invoke-Command { Remove-Item -Path cert:\LocalMachine\TestStore -Recurse } `
  -ComputerName S1, S2
```

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターは、PowerShell プロバイダーによって追加されるコマンドレットパラメーターであり、プロバイダーに対応したドライブでコマンドレットが使用されている場合にのみ使用できます。 これらのパラメーターは証明書プロバイダーのすべてのサブディレクトリで有効ですが、証明書に対してのみ有効です。

> [!NOTE]
> プロパティに対してフィルター処理を実行するパラメーター `EnhancedKeyUsageList` も、空のプロパティ値を持つ項目を返し `EnhancedKeyUsageList` ます。
> 空の **EnhancedKeyUsageList** が含まれている証明書は、すべての目的で使用できます。

次の証明書プロバイダーパラメーターが PowerShell 7.1 で再導入されました。

- DNSName
- DocumentEncryptionCert
- EKU
- ExpiringInDays
- SSLServerAuthentication

### <a name="codesigningcert-systemmanagementautomationswitchparameter"></a>CodeSigningCert は、<の>

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

このパラメーターは、 **EnhancedKeyUsageList** プロパティ値に "コード署名" が含まれている証明書を取得します。

### <a name="deletekey-systemmanagementautomationswitchparameter"></a>DeleteKey <を削除します。 SwitchParameter>

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)

このパラメーターは、証明書を削除するときに、関連付けられている秘密キーを削除します。

> [!IMPORTANT]
> リモートコンピューター上のストア内のユーザー証明書に関連付けられている秘密キーを削除するには `Cert:\CurrentUser` 、委任された資格情報を使用する必要があります。 `Invoke-Command`コマンドレットでは、 **CredSSP** パラメーターを使用した資格情報の委任がサポートされています。 `Remove-Item`との資格情報の委任を使用する前に、セキュリティ上のリスクを考慮する必要があり `Invoke-Command` ます。

このパラメーターは PowerShell 7.1 で再導入されました

### <a name="dnsname-microsoftpowershellcommandsdnsnamerepresentation"></a>DnsName <、> のようになります。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

このパラメーターは、証明書の **DNSNameList** プロパティに指定されたドメイン名または名前パターンを持つ証明書を取得します。 このパラメーターの値には、"Unicode" または "ASCII" を指定できます。 Punycode 値は Unicode に変換されます。 ワイルドカード文字 ( `*` ) が許可されます。

このパラメーターは PowerShell 7.1 で再導入されました

### <a name="documentencryptioncert-systemmanagementautomationswitchparameter"></a>DocumentEncryptionCert <を> します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

このパラメーターは、 **EnhancedKeyUsageList** プロパティ値に "Document Encryption" が含まれている証明書を取得します。

### <a name="eku-systemstring"></a>EKU <System.string>

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

このパラメーターは、証明書のプロパティに指定されたテキストまたはテキストパターンを持つ証明書を取得し `EnhancedKeyUsageList` ます。 ワイルドカード文字 ( `*` ) が許可されます。 プロパティには、 `EnhancedKeyUsageList` EKU のフレンドリ名と OID フィールドが含まれています。

このパラメーターは PowerShell 7.1 で再導入されました

### <a name="expiringindays-systemint32"></a>ExpiringInDays <system.string>

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

このパラメーターは、指定した日数以内に期限切れになる証明書を取得します。 値に 0 (zero) を指定した場合、失効している証明書が取得されます。

このパラメーターは PowerShell 7.1 で再導入されました

### <a name="itemtype-string"></a>ItemType \<String\>

このパラメーターを使用すると、によって作成される項目の型を指定でき `New-Item` ます。

ドライブでは、 `Certificate` 次の値が許可されます。

- 証明書プロバイダー
- Certificate
- ストア
- StoreLocation

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="sslserverauthentication-systemmanagementautomationswitchparameter"></a>SSLServerAuthentication は、<の> を指定します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

SSL Web ホスティングのサーバー証明書のみを取得します。 このパラメーターは、プロパティ値に "サーバー認証" が含まれている証明書を取得し `EnhancedKeyUsageList` ます。

このパラメーターは PowerShell 7.1 で再導入されました

## <a name="script-properties"></a>スクリプトのプロパティ

証明書を表す新しいスクリプトプロパティが **x509Certificate2** オブジェクトに追加され、証明書の検索と管理を簡単に行えるようになりました。

- `DnsNameList`: プロパティを設定するために、 `DnsNameList` 証明書プロバイダーは、DNSName エントリ (SAN) 拡張機能のコンテンツをコピーします。 SAN 拡張子が空の場合、証明書の Subject フィールドのコンテンツがプロパティに入力されます。

- `EnhancedKeyUsageList`: プロパティを設定するために、証明書 `EnhancedKeyUsageList` プロバイダーは証明書の EnhancedKeyUsage (EKU) フィールドの OID プロパティをコピーして、そのフレンドリ名を作成します。

- `SendAsTrustedIssuer`: プロパティを設定するために、証明書 `SendAsTrustedIssuer` プロバイダーは `SendAsTrustedIssuer` 証明書からプロパティをコピーします。  詳細については、「 [クライアント認証の信頼された発行者の管理](/windows-server/security/tls/what-s-new-in-tls-ssl-schannel-ssp-overview#BKMK_TrustedIssuers)」を参照してください。

これらの新機能を利用すると、DNS 名と有効期限に基づいて証明書を検索したり、拡張キー使用法 (EKU) プロパティの値でクライアントとサーバーの認証証明書を区別したりできます。

## <a name="using-the-pipeline"></a>パイプラインの使用

プロバイダーのコマンドレットは、パイプラインの入力を受け入れます。 パイプラインを使用すると、1つのコマンドレットから別のプロバイダーのコマンドレットにプロバイダーデータを送信することにより、タスクを簡単に行うことができます。
プロバイダーコマンドレットでパイプラインを使用する方法の詳細については、この記事全体で説明されているコマンドレットリファレンスを参照してください。

## <a name="getting-help"></a>ヘルプの表示

Windows PowerShell 3.0 より、プロバイダー コマンドレットのためにカスタマイズされたヘルプ トピックを取得できます。これはファイル システム ドライブでのプロバイダー コマンドレットの動作を説明します。

ファイルシステムドライブ用にカスタマイズされたヘルプトピックを取得するには、ファイルシステムドライブで [get-help](xref:Microsoft.PowerShell.Core.Get-Help) コマンドを実行するか、のパラメーターを使用して `-Path` `Get-Help` ファイルシステムドライブを指定します。

```powershell
Get-Help Get-ChildItem
```

```powershell
Get-Help Get-ChildItem -Path cert:
```

## <a name="see-also"></a>関連項目

[about_Providers](../../Microsoft.PowerShell.Core/About/about_Providers.md)

[about_Signing](../../Microsoft.PowerShell.Core/About/about_Signing.md)

[Get-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Get-AuthenticodeSignature)

[Set-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Set-AuthenticodeSignature)

[Get-PfxCertificate](xref:Microsoft.PowerShell.Security.Get-PfxCertificate)
