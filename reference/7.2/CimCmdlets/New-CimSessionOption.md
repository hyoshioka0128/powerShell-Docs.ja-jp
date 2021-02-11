---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/new-cimsessionoption?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-CimSessionOption
ms.openlocfilehash: 54a2ca8a28d54bfe1d9676acdaace4592f62620f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603210"
---
# New-CimSessionOption

## 概要
New-CimSession コマンドレットの詳細オプションを指定します。

## SYNTAX

### ProtocolTypeSet (既定値)

```
New-CimSessionOption [-Protocol] <ProtocolType> [-UICulture <CultureInfo>] [-Culture <CultureInfo>]
 [<CommonParameters>]
```

### WSManParameterSet

```
New-CimSessionOption [-NoEncryption] [-SkipCACheck] [-SkipCNCheck] [-SkipRevocationCheck]
 [-EncodePortInServicePrincipalName] [-Encoding <PacketEncoding>] [-HttpPrefix <Uri>]
 [-MaxEnvelopeSizeKB <UInt32>] [-ProxyAuthentication <PasswordAuthenticationMechanism>]
 [-ProxyCertificateThumbprint <String>] [-ProxyCredential <PSCredential>] [-ProxyType <ProxyType>]
 [-UseSsl] [-UICulture <CultureInfo>] [-Culture <CultureInfo>] [<CommonParameters>]
```

### DcomParameterSet

```
New-CimSessionOption [-Impersonation <ImpersonationType>] [-PacketIntegrity] [-PacketPrivacy]
 [-UICulture <CultureInfo>] [-Culture <CultureInfo>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-CimSessionOption` CIM セッションオプションオブジェクトのインスタンスを作成します。 Cim セッションオプションオブジェクトをコマンドレットへの入力として使用して、 `New-CimSession` cim セッションのオプションを指定します。

このコマンドレットには2つのパラメーターセットがあります。1つは WsMan オプション用で、もう1つは分散コンポーネントオブジェクトモデル (DCOM) オプション用です。 コマンドレットは、使用するパラメーターに応じて、DCOM セッションオプションのインスタンスを返すか、WsMan セッションオプションを返します。

## 例

### 例 1: DCOM の CIM セッションオプションオブジェクトを作成する

この例では、DCOM プロトコル用の CIM セッションオプションオブジェクトを作成し、という名前の変数に格納し `$so` ます。 次に、変数の内容がコマンドレットに渡され `New-CimSession` ます。
`New-CimSession` 次に、変数に定義されているオプションを使用して、Server01 というリモートサーバーを使用して新しい CIM セッションを作成します。

```powershell
$so = New-CimSessionOption -Protocol DCOM
New-CimSession -ComputerName Server01 -SessionOption $so
```

### 例 2: WsMan の CIM セッションオプションオブジェクトを作成する

この例では、WsMan プロトコル用の CIM セッションオプションオブジェクトを作成します。 オブジェクトには、 **Proxyauthentication** パラメーターによって指定された **Kerberos** の認証モードの構成、 **proxyauthentication** パラメーターによって指定された資格情報が含まれています。また、コマンドが CA チェックをスキップし、CN チェックをスキップし、SSL を使用することを指定します。

```powershell
New-CimSessionOption -ProxyAuthentication Kerberos -ProxyCredential $cred -SkipCACheck -SkipCNCheck -UseSsl
```

### 例 3: 指定されたカルチャで CIM セッションオプションオブジェクトを作成する

```powershell
New-CimSessionOption -Culture Fr-Fr -Protocol Wsman
```

この例では、CIM セッションに使用するカルチャを指定します。 既定では、クライアントのカルチャは、操作の実行時に使用されます。 ただし、既定のカルチャは、 **culture** パラメーターを使用してオーバーライドできます。

## PARAMETERS

### -Culture

CIM セッションに使用するユーザーインターフェイスのカルチャを指定します。 次のいずれかの形式を使用して、このパラメーターの値を指定します。

- "EN-US" などの形式のカルチャ名 `<languagecode2>-<country/regioncode2>` 。
- **CultureInfo** オブジェクトを含む変数です。
- **CultureInfo** オブジェクトを取得するコマンド ( [Get Culture](../Microsoft.PowerShell.Utility/Get-Culture.md)など)

```yaml
Type: System.Globalization.CultureInfo
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -EncodePortInServicePrincipalName

Kerberos 接続がサービスに接続しているサービスプリンシパル名 (SPN) にサービスポート番号が含まれていることを示します。 この種類の接続は一般的ではありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Encoding

WsMan プロトコルに使用されるエンコーディングを指定します。 このパラメーターに指定できる値は、 **Default**、 **Utf8**、または **Utf16** です。

```yaml
Type: Microsoft.Management.Infrastructure.Options.PacketEncoding
Parameter Sets: WSManParameterSet
Aliases:
Accepted values: Default, Utf8, Utf16

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -HttpPrefix

コンピューター名とポート番号の後にある HTTP URL の部分を指定します。 この変更は一般的ではありません。 既定では、このパラメーターの値は **/wsman** です。

```yaml
Type: System.Uri
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -偽装

偽装を使用して Windows Management Instrumentation (WMI) を行う DCOM セッションを作成します。

このパラメーターの有効な値は次のとおりです。

- 既定: DCOM は、通常のセキュリティネゴシエーションアルゴリズムを使用して偽装レベルを選択できます。
- None: クライアントはサーバーに対して匿名です。 サーバープロセスはクライアントを偽装できますが、偽装トークンには情報が含まれていないため、使用できません。
- 識別: オブジェクトが呼び出し元の資格情報を照会できるようにします。
- Impersonate: オブジェクトが呼び出し元の資格情報を使用できるようにします。
- Delegate: オブジェクトが、呼び出し元の資格情報の使用を他のオブジェクトに許可できるようにします。

**権限借用** が指定されていない場合、 `New-CimSession` コマンドレットは **Impersonate** の値を使用します。

```yaml
Type: Microsoft.Management.Infrastructure.Options.ImpersonationType
Parameter Sets: DcomParameterSet
Aliases:
Accepted values: Default, None, Identify, Impersonate, Delegate

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaxEnvelopeSizeKB

どちらの方向に対しても WsMan XML メッセージのサイズ制限を指定します。

```yaml
Type: System.UInt32
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NoEncryption

データの暗号化をオフにすることを指定します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PacketIntegrity

WMI に対して作成された DCOM セッションが、Component Object Model (COM) _Packetintegrity_ 機能を使用することを指定します。 既定では、DCOM を使用して作成されたすべての CIM セッションで **Packetintegrity** パラメーターが **True** に設定されています。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DcomParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PacketPrivacy

COM _Packetprivacy_ を使用して、WMI への DCOM セッションを作成します。 既定では、DCOM を使用して作成されたすべての CIM セッションで、 **Packetprivacy** パラメーターが **true** に設定されています。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DcomParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Protocol

使用するプロトコルを指定します。 このパラメーターに指定できる値は、 **DCOM**、 **Default**、または **Wsman** です。

```yaml
Type: Microsoft.Management.Infrastructure.CimCmdlets.ProtocolType
Parameter Sets: ProtocolTypeSet
Aliases:
Accepted values: Dcom, Default, Wsman

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProxyAuthentication

プロキシの解決に使用する認証方法を指定します。 このパラメーターに指定できる値は、 **Default**、 **Digest**、 **Negotiate**、 **Basic**、 **Kerberos**、 **ntlmdomain**、 **CredSsp** です。

```yaml
Type: Microsoft.Management.Infrastructure.Options.PasswordAuthenticationMechanism
Parameter Sets: WSManParameterSet
Aliases:
Accepted values: Default, Digest, Negotiate, Basic, Kerberos, NtlmDomain, CredSsp

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProxyCertificateThumbprint

プロキシ認証用のユーザーアカウントの (x.509) デジタル公開キー証明書を指定します。
証明書の拇印を入力します。 証明書は、クライアント証明書ベースの認証で使用されます。 ローカルユーザーアカウントにしかマップできず、ドメインアカウントでは機能しません。

証明書の拇印を取得するに `Get-Item` は、 `Get-ChildItem` PowerShell Cert: ドライブのまたはコマンドレットを使用します。

```yaml
Type: System.String
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProxyCredential

プロキシ認証に使用する資格情報を指定します。 次のいずれかを入力します。

- PSCredential オブジェクトを含む変数です。
- PSCredential オブジェクトを取得するコマンド (たとえば、 `Get-Credential`

このオプションが設定されていない場合、資格情報を指定することはできません。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyType

使用するホスト名解決メカニズムを指定します。 このパラメーターに指定できる値は、 **None**、 **WinHttp**、 **Auto**、または **internetexplorer** です。

このパラメーターの既定値は **Internetexplorer** です。

```yaml
Type: Microsoft.Management.Infrastructure.Options.ProxyType
Parameter Sets: WSManParameterSet
Aliases:
Accepted values: None, WinHttp, Auto, InternetExplorer

Required: False
Position: Named
Default value: InternetExplorer
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SkipCACheck

HTTPS 経由で接続する場合、クライアントは、サーバー証明書が信頼された証明機関 (CA) によって署名されているかどうかを検証しません。

リモートコンピューターが物理的に安全で分離されているネットワークの一部である場合、またはリモートコンピューターが WinRM 構成で信頼されたホストとして一覧表示されている場合など、リモートコンピューターが別のメカニズムを使用して信頼されている場合にのみ、このパラメーターを使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SkipCNCheck

サーバーの証明書の共通名 (CN) がサーバーのホスト名と一致する必要がないことを示します。 このパラメーターは、HTTPS プロトコルを使用する信頼されたコンピューターでのリモート操作に対してのみ使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SkipRevocationCheck

サーバー証明書の失効確認がスキップされることを示します。 このパラメーターは、信頼されたコンピューターに対してのみ使用してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -UICulture

CIM セッションに使用するユーザーインターフェイスのカルチャを指定します。 次のいずれかの形式を使用して、このパラメーターの値を指定します。

- "EN-US" などの形式のカルチャ名 `<languagecode2>-<country/regioncode2>` 。
- CultureInfo オブジェクトを含む変数です。
- などの CultureInfo オブジェクトを取得するコマンド `Get-Culture` 。

```yaml
Type: System.Globalization.CultureInfo
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -UseSsl

リモートコンピューターへの接続を確立するために SSL を使用する必要があることを示します。 既定では、SSL は使用されません。 WsMan は、HTTP を使用している場合でも、ネットワーク経由で転送されるすべてのコンテンツを暗号化します。

このパラメーターを使用すると、HTTP ではなく HTTPS の追加の保護を指定できます。 接続に使用するポートで SSL が使用できない場合、このパラメーターを指定すると、コマンドは失敗します。

このパラメーターは、 **Packetprivacy** パラメーターが指定されていない場合にのみ使用することをお勧めします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WSManParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは、入力オブジェクトを受け入れません。

## 出力

### CIMSessionOption

このコマンドレットは、CIM セッションオプションの情報を含むオブジェクトを返します。

## 注

## 関連リンク

[Get-ChildItem](../microsoft.powershell.management/get-childitem.md)

[Get-Credential](../microsoft.powershell.security/get-credential.md)

[Get-Culture](../microsoft.powershell.utility/get-culture.md)

[Get-Item](../microsoft.powershell.management/get-item.md)

[New-CimSession](New-CimSession.md)

