---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 03/31/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/new-wsmaninstance?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-WSManInstance
ms.openlocfilehash: dd0343e9b6014e079c322309b699874683bacd6a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602389"
---
# New-WSManInstance

## 概要
管理リソースの新しいインスタンスを作成します。

## SYNTAX

### ComputerName (既定値)

```
New-WSManInstance [-ApplicationName <String>] [-ComputerName <String>] [-FilePath <String>]
 [-OptionSet <Hashtable>] [-Port <Int32>] [-ResourceURI] <Uri> [-SelectorSet] <Hashtable>
 [-SessionOption <SessionOption>] [-UseSSL] [-ValueSet <Hashtable>] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [<CommonParameters>]
```

### URI

```
New-WSManInstance [-ConnectionURI <Uri>] [-FilePath <String>] [-OptionSet <Hashtable>] [-ResourceURI] <Uri>
 [-SelectorSet] <Hashtable> [-SessionOption <SessionOption>] [-ValueSet <Hashtable>]
 [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-WSManInstance` 管理リソースの新しいインスタンスを作成します。 リソース URI と値セットまたは入力ファイルを使用して、管理リソースの新しいインスタンスを作成します。

このコマンドレットは、WinRM の接続/トランスポート層を使用して、管理リソースのインスタンスを作成します。

## 例

### 例 1: HTTPS リスナーを作成する

このコマンドは、すべての IP アドレスの WS-Management HTTPS リスナーのインスタンスを作成します。

```powershell
New-WSManInstance winrm/config/Listener -SelectorSet @{Transport='HTTPS'; Address='*'} -ValueSet @{Hostname="HOST";CertificateThumbprint="XXXXXXXXXX"}
```

## PARAMETERS

### -ApplicationName

接続のアプリケーション名を指定します。 **ApplicationName** パラメーターの既定値は **WSMAN** です。 リモート エンドポイントの完全な識別子は、次の形式です。

`<transport>://<server>:<port>/<ApplicationName>`

次に例を示します。

`http://server01:8080/WSMAN`

セッションをホストするインターネット インフォメーション サービス (IIS) は、このエンドポイントで、指定されたアプリケーションに要求を転送します。 この **WSMAN** の既定の設定は、ほとんどの用途に適しています。 このパラメーターは、Windows PowerShell を実行している 1 台のコンピューターに多数のコンピューターからリモート接続を確立する場合に使用するように設計されています。 この場合、IIS は効率を上げるために Web Services for Management (WS-MANAGEMENT) をホストします。

```yaml
Type: System.String
Parameter Sets: ComputerName
Aliases:

Required: False
Position: Named
Default value: Wsman
Accept pipeline input: False
Accept wildcard characters: False
```

### -認証

サーバーで使用される認証メカニズムを指定します。 次のいずれかの値になります。

- Basic: Basic は、ユーザー名とパスワードがクリアテキストでサーバーまたはプロキシに送信されるスキームです。
- 既定値: WS-Management プロトコルによって実装される認証方法を使用します。 既定値です。
- ダイジェスト: ダイジェストは、サーバーで指定されたデータ文字列をチャレンジに使用するチャレンジ/レスポンススキームです。
- Kerberos: クライアントコンピューターとサーバーは、Kerberos 証明書を使用して相互に認証します。
- Negotiate: Negotiate は、認証に使用するスキームを決定するために、サーバーまたはプロキシとネゴシエートするチャレンジ/レスポンススキームです。 たとえば、このパラメーター値を指定すると、Kerberos プロトコルと NTLM のどちらが使用されるかを特定するネゴシエーションを行うことができます。
- CredSSP: Credential Security Support Provider (CredSSP) 認証を使用します。これにより、ユーザーは資格情報を委任できます。 このオプションは、1 台のリモート コンピューターで実行するが、他のリモート コンピューターからデータを収集するコマンドや、他のリモート コンピューターで追加のコマンドを実行するコマンドを対象としています。

> [!CAUTION]
> CredSSP では、ユーザーの資格情報がローカル コンピューターからリモート コンピューターに委任されます。 そのため、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡された資格情報を使用してネットワーク セッションが制御される場合があります。

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

この処理を実行するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書の拇印を取得するに `Get-Item` は、PowerShell Cert: ドライブのまたはコマンドを使用し `Get-ChildItem` ます。

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

管理操作を実行するコンピューターを指定します。 値には、完全修飾ドメイン名、NetBIOS 名、または IP アドレスを指定できます。 ローカルコンピューター名を使用するか、localhost を使用するか、ドット () を使用し `.` てローカルコンピューターを指定します。 ローカル コンピューターは、既定値です。 リモート コンピューターがユーザーとは異なるドメインにある場合は、完全修飾ドメイン名を使用する必要があります。 パイプを使用して、このパラメーターの値をコマンドレットに渡すことができます。

```yaml
Type: System.String
Parameter Sets: ComputerName
Aliases: cn

Required: False
Position: Named
Default value: Localhost
Accept pipeline input: False
Accept wildcard characters: False
```

### -ConnectionURI

接続エンドポイントを指定します。
この文字列の形式は次のとおりです。

`<Transport>://<Server>:<Port>/<ApplicationName>`

次の文字列は、このパラメーターの正しい形式の値です。

`http://Server01:8080/WSMAN`

URI は完全修飾名にする必要があります。

```yaml
Type: System.Uri
Parameter Sets: URI
Aliases: CURI, CU

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。 "User01"、"Domain01\user01」"、"" などのユーザー名を入力し User@Domain.com ます。 または、コマンドレットによって返されるような PSCredential オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードの入力を促すメッセージが表示されます。

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

### -FilePath

管理リソースの作成に使用するファイルのパスを指定します。 管理リソースは、 **ResourceURI** パラメーターと **SelectorSet** パラメーターを使用して指定します。 たとえば、次のコマンドは、 **File** パラメーターを使用します。

`Invoke-WSManAction -Action stopservice -ResourceUri wmi/cimv2/Win32_Service -SelectorSet @{Name="spooler"} -File c:\input.xml -Authentication Default`

このコマンドは、ファイルからの入力を使用してスプーラサービスの **stopservice** メソッドを呼び出します。 ファイルには、 `Input.xml` 次のコンテンツが含まれています。

`<p:StopService_INPUT xmlns:p="http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service" />`

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: Path

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OptionSet

要求のオプションを変更または調整する一連のスイッチをサービスに渡します。 これらは、サービス固有であるため、コマンド ライン シェルで使用されるスイッチに似ています。 任意の数のオプションを指定できます。

次の例では、a、b、および c パラメーターに値 1、2、および 3 を渡す構文を示します。

`-OptionSet @{a=1;b=2;c=3}`

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases: os

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Port

クライアントが WinRM サービスに接続するときに使用するポートを指定します。 トランスポートが HTTP の場合、既定のポートは 80 です。 トランスポートが HTTPS の場合、既定のポートは 443 です。

HTTPS をトランスポートとして使用する場合は、 **ComputerName** パラメーターの値がサーバーの証明書共通名 (CN) と一致している必要があります。 ただし、 **Sessionoption** パラメーターの一部として **skipcncheck** パラメーターを指定した場合、サーバーの証明書の共通名がサーバーのホスト名と一致する必要はありません。 **Skipcncheck** パラメーターは、信頼されたコンピューターに対してのみ使用してください。

```yaml
Type: System.Int32
Parameter Sets: ComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceURI

リソース クラスまたはインスタンスの Uniform Resource Identifier (URI) を含めます。 URI は、ディスクやプロセスなど、コンピューター上の特定の種類のリソースを特定するために使用されます。

URI は、プレフィックスとリソースのパスで構成されます。 次に例を示します。

`http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`

`http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/CIM_NumericSensor`

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases: ruri

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SelectorSet

特定の管理リソース インスタンスの選択に使用する値のペアのセットを指定します。 **SelectorSet** パラメーターは、リソースの複数のインスタンスが存在する場合に使用されます。 **SelectorSet** パラメーターの値は、ハッシュテーブルである必要があります。

次の例では、このパラメーターの値を入力する方法を示します。

`-SelectorSet @{Name="WinRM";ID="yyy"}`

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -SessionOption

WS-Management セッションの一連の拡張オプションを定義します。 コマンドレットを使用して作成する **Sessionoption** オブジェクトを入力し `New-WSManSessionOption` ます。 使用可能なオプションの詳細については、「」を参照してください `New-WSManSessionOption` 。

```yaml
Type: Microsoft.WSMan.Management.SessionOption
Parameter Sets: (All)
Aliases: so

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSSL

Secure Sockets Layer (SSL) プロトコルを使用してリモート コンピューターとの接続を確立するように指定します。 既定では、SSL は使用されません。

WS-Management は、ネットワークを介して転送されるすべての Windows PowerShell コンテンツを暗号化します。 **UseSSL** パラメーターを使用すると、HTTP ではなく HTTPS の追加の保護を指定できます。 接続に使用するポートで SSL を使用できない場合に、このパラメーターを指定すると、コマンドは失敗します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ValueSet

管理リソースの変更に役立つハッシュ テーブルを指定します。 管理リソースは、 **ResourceURI** パラメーターと **SelectorSet** パラメーターを使用して指定します。 **Valueset** パラメーターの値は、ハッシュテーブルである必要があります。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

このコマンドレットは入力を受け取りません。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

`Set-WmiInstance`コマンドレットである Windows Management Instrumentation (WMI) コマンドレットも似ています。
`Set-WmiInstance` は、DCOM 接続/トランスポート層を使用して、WMI インスタンスを作成または更新します。

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

