---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/invoke-wsmanaction?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-WSManAction
ms.openlocfilehash: e2456e1da866929d60c36cc0e09990399b41614b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600782"
---
# Invoke-WSManAction

## 概要
リソース URI とセレクターによって指定されたオブジェクトのアクションを呼び出します。

## SYNTAX

### URI (既定)

```
Invoke-WSManAction [-Action] <String> [-ConnectionURI <Uri>] [-FilePath <String>] [-OptionSet <Hashtable>]
 [[-SelectorSet] <Hashtable>] [-SessionOption <SessionOption>] [-ValueSet <Hashtable>] [-ResourceURI] <Uri>
 [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [<CommonParameters>]
```

### [ComputerName]

```
Invoke-WSManAction [-Action] <String> [-ApplicationName <String>] [-ComputerName <String>] [-FilePath <String>]
 [-OptionSet <Hashtable>] [-Port <Int32>] [[-SelectorSet] <Hashtable>] [-SessionOption <SessionOption>]
 [-UseSSL] [-ValueSet <Hashtable>] [-ResourceURI] <Uri> [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [<CommonParameters>]
```

## Description
**呼び出し WSManAction** は、RESOURCE_URI で指定されたオブジェクトに対してアクションを実行します。ここで、パラメーターはキーと値のペアによって指定されます。

このコマンドレットは、WSMan の接続/トランスポート層を使用して、アクションを実行します。

## 例

### 例 1: メソッドを呼び出す

```powershell
Invoke-WSManAction -Action startservice -ResourceURI wmicimv2/win32_service  -SelectorSet @{name="spooler"} -Authentication default
```

```Output
xsi         : http://www.w3.org/2001/XMLSchema-instance
p           : http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service
cim         : http://schemas.dmtf.org/wbem/wscim/1/common
lang        : en-US
ReturnValue : 0
```

このコマンドは、スプーラサービスに対応する Win32_Service WMI クラスインスタンスの **startservice** メソッドを呼び出します。

戻り値は、アクションが成功したかどうかを示します。
この場合、戻り値の 0 は成功を示します。
戻り値の 5 は、サービスが既に開始されていることを示します。

### 例 2: メソッドを呼び出す

```powershell
Invoke-WSManAction -Action stopservice -ResourceURI wmicimv2/Win32_Service -SelectorSet @{Name="spooler"} -FilePath:input.xml -Authentication default
```

```Output
xsi         : http://www.w3.org/2001/XMLSchema-instance
p           : http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service
cim         : http://schemas.dmtf.org/wbem/wscim/1/common
lang        : en-US
ReturnValue : 0
```

このコマンドは、ファイルからの入力を使用してスプーラサービスの **stopservice** メソッドを呼び出します。
Input.xml ファイルには、次の内容が含まれています。

`<p:StopService_INPUT xmlns:p="http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service" />`

### 例 3: 指定されたパラメーター値を使用してメソッドを呼び出す

```powershell
Invoke-WSManAction -Action create -ResourceURI wmicimv2/win32_process -ValueSet @{commandline="notepad.exe";currentdirectory="C:\"}
```

```Output
xsi         : http://www.w3.org/2001/XMLSchema-instance
p           : http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Process
cim         : http://schemas.dmtf.org/wbem/wscim/1/common
lang        : en-US
ProcessId   : 6356
ReturnValue : 0
```

このコマンドは、Win32_Process クラスの **Create** メソッドを呼び出します。
メソッドに2つのパラメーター値 Notepad.exe と C:\ を渡します。
その結果、メモ帳を実行する新しいプロセスが作成され、新しいプロセスの現在のディレクトリが C:\ に設定されます。

### 例 4: リモートコンピューターでメソッドを呼び出す

```powershell
Invoke-WSManAction -Action startservice -ResourceURI wmicimv2/win32_service  -SelectorSet @{name="spooler"} -ComputerName server01 -Authentication default
```

```Output
xsi         : http://www.w3.org/2001/XMLSchema-instance
p           : http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service
cim         : http://schemas.dmtf.org/wbem/wscim/1/common
lang        : en-US
ReturnValue : 0
```

このコマンドは、スプーラサービスに対応する Win32_Service WMI クラスインスタンスの **startservice** メソッドを呼び出します。
*ComputerName* パラメーターを指定すると、リモートの server01 コンピューターに対してコマンドが実行されます。

## PARAMETERS

### -Action
ResourceURI およびセレクターによって指定された管理オブジェクトで実行するメソッドを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

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
Position: Named
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
Aliases: CURI, CU

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

### -FilePath
管理リソースの更新に使用するファイルのパスを指定します。
管理リソースは、 *ResourceURI* パラメーターと *SelectorSet* パラメーターを使用して指定します。
たとえば、次のコマンドは、 *FilePath* パラメーターを使用します。

`Invoke-WSManAction -Action stopservice -ResourceUri wmicimv2/Win32_Service -SelectorSet @{Name="spooler"} -FilePath c:\input.xml -Authentication default`

このコマンドは、ファイルからの入力を使用して **スプーラ** サービスの **stopservice** メソッドを呼び出します。
Input.xml ファイルには、次の内容が含まれています。

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
Accept pipeline input: True (ByPropertyName, ByValue)
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
Parameter Sets: ComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceURI
リソースクラスまたはインスタンスの URI を指定します。
URI は、ディスクやプロセスなど、コンピューター上の特定の種類のリソースを特定するために使用されます。

URI は、リソースのプレフィックスとパスで構成されます。
次に例を示します。

`http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`

`http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/CIM_NumericSensor`

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases: ruri

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -SelectorSet
特定の管理リソース インスタンスの選択に使用する値のペアのセットを指定します。
*SelectorSet* は、リソースの複数のインスタンスが存在する場合に使用します。
*SelectorSet* の値は、ハッシュテーブルである必要があります。

次の例では、このパラメーターの値を入力する方法を示します。

`-SelectorSet @{Name="WinRM";ID="yyy"}`

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
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

### -ValueSet
管理リソースの変更に役立つハッシュ テーブルを指定します。
管理リソースは、 *ResourceURI* と *SelectorSet* を使用して指定します。
*Valueset* パラメーターの値は、ハッシュテーブルである必要があります。

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
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
このコマンドレットは入力を受け取りません。

## 出力

### なし
このコマンドレットは出力を生成しません。

## 注

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

