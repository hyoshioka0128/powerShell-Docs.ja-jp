---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/connect-wsman?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Connect-WSMan
ms.openlocfilehash: 5dfd0b355a3589bf45ea92b92ae8778023132bcd
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604966"
---
# Connect-WSMan

## 概要
リモート コンピューター上の WinRM サービスに接続します。

## SYNTAX

### ComputerName (既定値)

```
Connect-WSMan [-ApplicationName <String>] [[-ComputerName] <String>] [-OptionSet <Hashtable>] [-Port <Int32>]
 [-SessionOption <SessionOption>] [-UseSSL] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [<CommonParameters>]
```

### URI

```
Connect-WSMan [-ConnectionURI <Uri>] [-OptionSet <Hashtable>] [-Port <Int32>] [-SessionOption <SessionOption>]
 [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [<CommonParameters>]
```

## Description
**接続 WSMan** コマンドレットは、リモートコンピューター上の WinRM サービスに接続し、リモートコンピューターへの永続的な接続を確立します。
このコマンドレットを WSMan プロバイダーのコンテキストで使用して、リモートコンピューター上の WinRM サービスに接続することができます。
ただし、WSMan プロバイダーに変更する前に、このコマンドレットを使用して、リモート コンピューター上の WinRM サービスに接続することもできます。
リモートコンピューターは、WSMan プロバイダーのルートディレクトリに表示されます。

クライアントとサーバー コンピューターが別のドメインまたはワークグループに属している場合は、明示的な資格情報が必要です。

リモートコンピューター上の WinRM サービスから切断する方法の詳細については、Disconnect-WSMan コマンドレットを参照してください。

## 例

### 例 1: リモートコンピューターに接続する

```
PS C:\> Connect-WSMan -ComputerName "server01"
PS C:\> cd wsman:
PS WSMan:\>
PS WSMan:\> dir
WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan

ComputerName                                  Type
------------                                  ----
localhost                                     Container
server01                                      Container
```

このコマンドは、server01 リモート コンピューターへの接続を作成します。

**接続 wsman** コマンドレットは、通常、リモートコンピューター (この場合は server01 コンピューター) に接続するために、wsman プロバイダーのコンテキストで使用されます。
ただし、WSMan プロバイダーに変更する前に、このコマンドレットを使用して、リモート コンピューターへの接続を確立することができます。
これらの接続は、 **ComputerName** の一覧に表示されます。

### 例 2: 管理者の資格情報を使用してリモートコンピューターに接続する

```
PS C:\> $cred = Get-Credential Administrator
PS C:\> Connect-WSMan -ComputerName "server01" -Credential $cred
PS C:\> cd wsman:
PS WSMan:\>
PS WSMan:\> dir
WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan

ComputerName                                  Type
------------                                  ----
localhost                                     Container
server01                                      Container
```

このコマンドは、管理者アカウントの資格情報を使用して、リモート システム server01 への接続を作成します。

最初のコマンドは、Get-Credential コマンドレットを使用して、管理者の資格情報を取得し、$cred 変数に保存します。
**Get-Credential** は、システムレジストリ設定に応じて、ダイアログボックスまたはコマンドラインを使用して、ユーザー名とパスワードのパスワードを入力するように求めます。

2番目のコマンドは、 *Credential* パラメーターを使用して、$cred に格納されている資格情報を **接続 WSMan** に渡します。
次に、管理者の資格情報を使用して、リモートシステム server01 に **接続** します。

### 例 3: 指定されたポートを経由してリモートコンピューターに接続する

```
PS C:\> Connect-WSMan -ComputerName "server01" -Port 80
PS C:\> cd wsman:
PS WSMan:\>
PS WSMan:\> dir
WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan
ComputerName                                  Type
------------                                  ----
localhost                                     Container
server01                                      Container
```

このコマンドは、ポート 80 を介した server01 リモート コンピューターへの接続を作成します。

### 例 4: 接続オプションを使用してリモートコンピューターに接続する

```
PS C:\> $a = New-WSManSessionOption -OperationTimeout 30000
PS C:\> Connect-WSMan -ComputerName "server01" -SessionOption $a
PS C:\> cd wsman:
PS WSMan:\>
PS WSMan:\> dir
WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan
ComputerName                                  Type
------------                                  ----
localhost                                     Container
server01                                      Container
```

この例で **は、server01 コマンドで** 定義されている接続オプションを使用して、リモートコンピューターへの接続を作成します。

最初のコマンドは、 **New-WSManSessionOption** を使用して、一連の接続設定オプションを $a 変数に格納します。
この場合は、セッション オプションで接続のタイムアウトを 30 秒 (30,000 ミリ秒) に設定します。

2番目のコマンドは、 *Sessionoption* パラメーターを使用して、$a 変数に格納されている資格情報を **接続 WSMan** に渡します。
次に、指定されたセッションオプションを使用して、リモートの server01 コンピューターに **接続** します。

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
Parameter Sets: ComputerName
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
Parameter Sets: ComputerName
Aliases: cn

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ConnectionURI
接続エンドポイントを指定します。
この文字列の形式は次のとおりです。

\<Transport\>://\<Server\>:\<Port\>/\<ApplicationName\>

次の文字列は、このパラメーターに対して正しく書式設定された値です。

`http://Server01:8080/WSMAN`

URI は完全修飾名にする必要があります。

```yaml
Type: System.Uri
Parameter Sets: URI
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
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

### -OptionSet
要求の性質を変更または調整するための一連のスイッチをサービスに指定します。
これらは、サービス固有であるため、コマンドラインシェルで使用されるスイッチに似ています。
任意の数のオプションを指定できます。

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
クライアントが WinRM サービスに接続するときに使用するポートを指定します。
トランスポートが HTTP の場合、既定のポートは 80 です。
トランスポートが HTTPS の場合、既定のポートは 443 です。

HTTPS をトランスポートとして使用する場合は、 *ComputerName* パラメーターの値がサーバーの証明書共通名 (CN) と一致している必要があります。
ただし、 *Sessionoption* パラメーターの一部として *skipcncheck* パラメーターを指定した場合、サーバーの証明書の共通名がサーバーのホスト名と一致する必要はありません。
*Skipcncheck* パラメーターは、信頼されたコンピューターに対してのみ使用してください。

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

### -SessionOption
WS-Management セッションの拡張オプションを指定します。
New-WSManSessionOption コマンドレットを使用して作成する **Sessionoption** オブジェクトを入力します。
使用可能なオプションの詳細については、「」と入力 `Get-Help New-WSManSessionOption` してください。

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
リモートコンピューターへの接続を確立するために Secure Sockets Layer (SSL) プロトコルを使用することを指定します。
既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で転送されるすべての PowerShell コンテンツを暗号化します。
*UseSSL* パラメーターを使用すると、HTTP ではなく HTTPS の追加の保護を指定できます。
接続に使用するポートで SSL が使用できない場合、このパラメーターを指定すると、コマンドは失敗します。

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

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
このコマンドレットは入力を受け取りません。

## 出力

### なし
このコマンドレットは出力を生成しません。

## 注

* WS-Management セッションを作成せずに、リモート コンピューターで管理コマンドを実行することや、管理データを照会することができます。 これを行うには、Invoke-WSManAction の *ComputerName* パラメーターと、を使用します。 *ComputerName* パラメーターを使用すると、PowerShell は、単一のコマンドに使用される一時的な接続を作成します。 コマンドの実行後、その接続は閉じられます。

*

## 関連リンク

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

[Test-WSMan](Test-WSMan.md)

