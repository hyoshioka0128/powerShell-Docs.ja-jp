---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/test-wsman?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-WSMan
ms.openlocfilehash: a29a9ef1e35f0c0f802952b99d853c617614a97b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599692"
---
# Test-WSMan

## 概要
WinRM サービスがローカル コンピューターとリモート コンピューターのどちらで実行されているかをテストします。

## SYNTAX

```
Test-WSMan [[-ComputerName] <String>] [-Authentication <AuthenticationMechanism>] [-Port <Int32>] [-UseSSL]
 [-ApplicationName <String>] [-Credential <PSCredential>] [-CertificateThumbprint <String>]
 [<CommonParameters>]
```

## Description

**テスト WSMan** コマンドレットは、WinRM サービスがローカルコンピューターとリモートコンピューターのどちらで実行されているかを判断する識別要求を送信します。
テスト対象のコンピューターでサービスが実行されている場合、このコマンドレットは、WS-Management ID スキーマ、プロトコルのバージョン、製品ベンダー、およびテスト対象のサービスの製品バージョンを表示します。

## 例

### 例 1: WinRM サービスの状態を確認する

```
PS C:\> Test-WSMan
wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd

ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd

ProductVendor   : Microsoft Corporation

ProductVersion  : OS: 0.0.0 SP: 0.0 Stack: 2.0
```

このコマンドは、WinRM サービスがローカル コンピューターとリモート コンピューターのどちらで実行されているかを確認します。

### 例 2: リモートコンピューター上の WinRM サービスの状態を確認する

```
PS C:\> Test-WSMan -ComputerName "server01"
wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd

ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd

ProductVendor   : Microsoft Corporation

ProductVersion  : OS: 0.0.0 SP: 0.0 Stack: 2.0
```

このコマンドは、server01 コンピューターで WinRM サービスが実行されているかどうかを確認します。

### 例 3: WinRM サービスの状態とオペレーティングシステムのバージョンを確認する

```
PS C:\> Test-WSMan -Authentication default
wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd

ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd

ProductVendor   : Microsoft Corporation

ProductVersion  : OS: 6.0.6001 SP: 1.0 Stack: 2.0
```

このコマンドは、WS-Management (WinRM) サービスがローカルコンピューター上で認証パラメーターを使用して実行されているかどうかをテストします。

Authentication パラメーターを使用すると、 **テスト WSMan** はオペレーティングシステムのバージョンを返すことができます。

### 例 4: WinRM サービスの状態とリモートコンピューター上のオペレーティングシステムのバージョンを確認する

```
PS C:\> Test-WSMan -ComputerName "server01" -Authentication default
wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd

ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd

ProductVendor   : Microsoft Corporation

ProductVersion  : OS: 6.1.7021 SP: 0.0 Stack: 2.0
```

このコマンドは、認証パラメーターを使用して、WS-Management (WinRM) サービスが server01 という名前のコンピューター上で実行されているかどうかをテストします。

Authentication パラメーターを使用すると、 **テスト WSMan** はオペレーティングシステムのバージョンを返すことができます。

## PARAMETERS

### -ApplicationName

接続のアプリケーション名を指定します。
*ApplicationName* パラメーターの既定値は WSMAN です。
リモート エンドポイントの完全な識別子は、次の形式です。

\<transport\>://\<server\>:\<port\>/\<ApplicationName\>

例: `http://server01:8080/WSMAN`

セッションをホストするインターネット インフォメーション サービス (IIS) は、このエンドポイントで、指定されたアプリケーションに要求を転送します。
この WSMAN の既定の設定は、ほとんどの用途に適しています。
このパラメーターは、多くのコンピューターが PowerShell を実行する1台のコンピューターへのリモート接続を確立する場合に使用するように設計されています。
この場合、IIS は効率を上げるために Web Services for Management (WS-MANAGEMENT) をホストします。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -認証

サーバーで使用される認証メカニズムを指定します。
このパラメーターの有効値は、次のとおりです。

- 基本。
Basic は、ユーザー名およびパスワードがクリア テキストでサーバーまたはプロキシに送信されるスキームです。
- 既定値。
WS-Management プロトコルによって実装されている認証方法を使用します。
既定値です。
- ダイジェスト。
Digest は、サーバーで指定されたデータ文字列をチャレンジで使用するチャレンジ/レスポンス スキームです。
- Kerberos。
クライアント コンピューターとサーバーが Kerberos 証明書を使用して相互に認証します。
- Negotiate
Negotiate は、認証で使用するスキームを特定するために、サーバーまたはプロキシとネゴシエートするチャレンジ/レスポンス スキームです。
たとえば、このパラメーター値を指定すると、Kerberos プロトコルと NTLM のどちらが使用されているかをネゴシエーションで判断できます。
- CredSSP.
Credential Security Support Provider (CredSSP) 認証を使用して、ユーザーが資格情報を委任できるようにします。
このオプションは、1 台のリモート コンピューターで実行するが、他のリモート コンピューターからデータを収集するコマンドや、他のリモート コンピューターで追加のコマンドを実行するコマンドを対象としています。

注意: CredSSP は、ローカルコンピューターからリモートコンピューターにユーザーの資格情報を委任します。
そのため、リモート操作のセキュリティ リスクが高まります。
リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡された資格情報を使用してネットワーク セッションが制御される場合があります。

重要: *認証* パラメーターを指定しない場合、認証を使用せずに、 **テスト WSMan** 要求がリモートコンピューターに匿名で送信されます。
要求が匿名で行われると、オペレーティングシステムのバージョンに固有の情報は返されません。
代わりに、このコマンドレットでは、オペレーティングシステムのバージョンと Service Pack レベル (OS: 0.0.0 SP: 0.0) の null 値が表示されます。

```yaml
Type: Microsoft.WSMan.Management.AuthenticationMechanism
Parameter Sets: (All)
Aliases: auth, am
Accepted values: None, Default, Digest, Negotiate, Basic, Kerberos, ClientCertificate, Credssp

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

この処理を実行するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。
証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。
これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書の拇印を取得するには、PowerShell Cert: ドライブで Get-Item または Get-ChildItem コマンドを使用します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

管理操作を実行するコンピューターを指定します。
値には、完全修飾ドメイン名、NetBIOS 名、または IP アドレスを指定できます。
ローカル コンピューターを指定するには、ローカル コンピューター名、localhost、またはドット (.) を使用します。
ローカル コンピューターは、既定値です。
リモート コンピューターがユーザーとは異なるドメインにある場合は、完全修飾ドメイン名を使用する必要があります。
パイプを使用して、このパラメーターの値をコマンドレットに渡すことができます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: cn

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。
既定値は現在のユーザーです。
User01、Domain01\user01」、などのユーザー名を入力し User@Domain.com ます。
または、Get-Credential コマンドレットによって返されるような **PSCredential** オブジェクトを入力します。
ユーザー名を入力すると、このコマンドレットによってパスワードの入力が求められます。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases: cred, c

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Port

クライアントが WinRM サービスに接続するときに使用するポートを指定します。
トランスポートが HTTP の場合、既定のポートは 80 です。
トランスポートが HTTPS の場合、既定のポートは 443 です。

HTTPS をトランスポートとして使用する場合は、 *ComputerName* パラメーターの値がサーバーの証明書共通名 (CN) と一致している必要があります。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSSL

リモートコンピューターへの接続を確立するために Secure Sockets Layer (SSL) プロトコルを使用することを指定します。
既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で転送されるすべての PowerShell コンテンツを暗号化します。
*UseSSL* パラメーターを使用すると、HTTP ではなく HTTPS の追加の保護を指定できます。
接続に使用するポートで SSL が使用できない場合、このパラメーターを指定すると、コマンドは失敗します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは入力を受け取りません。

## 出力

### なし

このコマンドレットは出力オブジェクトを生成しません。

## 注

* 既定では、 **テスト WSMan** コマンドレットは認証を使用せずに WinRM サービスを照会し、オペレーティングシステムのバージョンに固有の情報は返しません。 代わりに、オペレーティングシステムのバージョンと Service Pack レベル (OS: 0.0.0 SP: 0.0) の null 値が表示されます。

*

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

