---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/new-cimsession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-CimSession
ms.openlocfilehash: 59e70f75ac9d822db00419d84055d92a3882c11d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603017"
---
# New-CimSession

## 概要

CIM セッションを作成します。

## SYNTAX

### CredentialParameterSet (既定値)

```
New-CimSession [-Authentication <PasswordAuthenticationMechanism>] [[-Credential] <PSCredential>]
 [[-ComputerName] <String[]>] [-Name <String>] [-OperationTimeoutSec <UInt32>] [-SkipTestConnection]
 [-Port <UInt32>] [-SessionOption <CimSessionOptions>] [<CommonParameters>]
```

### CertificateParameterSet

```
New-CimSession [-CertificateThumbprint <String>] [[-ComputerName] <String[]>] [-Name <String>]
 [-OperationTimeoutSec <UInt32>] [-SkipTestConnection] [-Port <UInt32>]
 [-SessionOption <CimSessionOptions>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-CimSession` CIM セッションを作成します。 CIM セッションは、ローカルコンピューターまたはリモートコンピューターへの接続を表すクライアント側オブジェクトです。 CIM セッションには、 **ComputerName**、使用されるプロトコル、さまざまな識別子など、接続に関する情報が含まれています。

このコマンドレットは、他のすべての CIM コマンドレットで使用できる CIM セッションオブジェクトを返します。

## 例

### 例 1: 既定のオプションを使用して CIM セッションを作成する

この例では、既定のオプションを使用してローカル CIM セッションを作成します。 **ComputerName** が指定されていない場合、は `New-CimSession` ローカルコンピューターに対して DCOM セッションを作成します。

```powershell
New-CimSession
```

### 例 2: 特定のコンピューターに対して CIM セッションを作成する

この例では、 **ComputerName** によって指定されたコンピューターに CIM セッションを作成します。
既定では、 `New-CimSession` **ComputerName** が指定されている場合、は WSMan セッションを作成します。

```powershell
New-CimSession -ComputerName Server01
```

### 例 3: 複数のコンピューターに CIM セッションを作成する

この例では、 **ComputerName** によって指定された各コンピューターに、コンマ区切りの一覧で CIM セッションを作成します。

```powershell
New-CimSession -ComputerName Server01,Server02,Server03
```

### 例 4: フレンドリ名を使用して CIM セッションを作成する

この例では、 **ComputerName** によって指定された各コンピューターに対して、コンマ区切りの一覧でリモート CIM セッションを作成し、 **名前** を指定することによって、新しいセッションにフレンドリ名を割り当てます。

```powershell
New-CimSession -ComputerName Server01,Server02 -Name FileServers
Get-CimSession -Name File*
```

CIM セッションのフレンドリ名を使用して、他の CIM コマンドレットでセッションを参照することができます (たとえば、 [CimSession](Get-CimSession.md))。

### 例 5: PSCredential オブジェクトを使用してコンピューターに CIM セッションを作成する

この例では、ComputerName **によって** 指定された **PSCredential** オブジェクトと、 **authentication** によって指定された認証の種類を使用して、 **ComputerName** によって指定されたコンピューターに CIM セッションを作成します。

```powershell
New-CimSession -ComputerName Server01 -Credential $cred -Authentication Negotiate
```

コマンドレットを使用して **PSCredential** オブジェクトを作成でき [`Get-Credential`](../Microsoft.PowerShell.Security/Get-Credential.md) ます。

### 例 6: 特定のポートを使用してコンピューターに CIM セッションを作成する

この例では、**ポート** によって指定された TCP ポートを使用して、 **ComputerName** によって指定されたコンピューターに CIM セッションを作成します。

```powershell
New-CimSession -ComputerName Server01 -Port 1234
```

### 例 7: DCOM を使用して CIM セッションを作成する

この例では、WSMan ではなく分散 COM (DCOM) プロトコルを使用して CIM セッションを作成します。

```powershell
$SessionOption = New-CimSessionOption -Protocol DCOM
New-CimSession -ComputerName Server1 -SessionOption $SessionOption
```

## PARAMETERS

### -認証

ユーザーの資格情報に使用される認証の種類を指定します。 このパラメーターの有効値は、次のとおりです。

- Default
- ダイジェスト
- ネゴシエート
- Basic
- Kerberos
- NtlmDomain
- CredSsp

ローカルコンピューターへの接続に **Ntlmdomain** 認証の種類を使用することはできません。 **CredSSP** 認証は、windows Vista、windows Server 2008、およびそれ以降のバージョンの windows でのみ使用できます。

> [!CAUTION]
> Credential Security Service Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

```yaml
Type: Microsoft.Management.Infrastructure.Options.PasswordAuthenticationMechanism
Parameter Sets: CredentialParameterSet
Aliases:
Accepted values: Default, Digest, Negotiate, Basic, Kerberos, NtlmDomain, CredSsp

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -CertificateThumbprint

この操作を実行するアクセス許可を持つユーザーアカウントのデジタル公開キー証明書 (x.509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書の拇印を取得するに [`Get-Item`](../Microsoft.Powershell.Management/Get-Item.md) は、 [`Get-ChildItem`](../Microsoft.Powershell.Management/Get-ChildItem.md) PowerShell 証明書プロバイダーでまたはコマンドレットを使用します。

詳細については、「 [about_Certificate_Provider](../Microsoft.PowerShell.Security/About/about_Certificate_Provider.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: CertificateParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ComputerName

CIM セッションを作成するコンピューターの名前を指定します。 1つのコンピューター名、またはコンマで区切られた複数のコンピューター名を指定してください。

**ComputerName** が指定されていない場合は、ローカルコンピューターへの CIM セッションが作成されます。 コンピューター名の値は、次のいずれかの形式で指定できます。

- 1つまたは複数の NetBIOS 名
- 1つ以上の IP アドレス
- 1つ以上の完全修飾ドメイン名。

コンピューターがユーザーとは異なるドメインにある場合は、完全修飾ドメイン名を指定する必要があります。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: CN, ServerName

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 **Credential** が指定されていない場合は、現在のユーザーアカウントが使用されます。

次のいずれかの形式を使用して、 **Credential** の値を指定します。

- ユーザー名: "User01"
- ドメイン名とユーザー名: "Domain01\user01」"
- ユーザープリンシパル名: " User@Domain.com "
- コマンドレットによって返されるような PSCredential オブジェクト [`Get-Credential`](../Microsoft.PowerShell.Security/Get-Credential.md) 。

ユーザー名を入力すると、パスワードの入力を促すメッセージが表示されます。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: CredentialParameterSet
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

CIM セッションのフレンドリ名を指定します。

名前を使用して、 [CimSession](Get-CimSession.md) コマンドレットなど、他のコマンドレットを使用するときに CIM セッションを参照することができます。
名前は、コンピューターや現在のセッションで一意である必要ありません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -OperationTimeoutSec

コマンドレットがサーバーからの応答を待機する期間。

既定では、このパラメーターの値は0です。これは、コマンドレットがサーバーの既定のタイムアウト値を使用することを意味します。

**Operationtimeoutsec** パラメーターが、堅牢な接続再試行タイムアウトの3分未満の値に設定されている場合、 **operationtimeoutsec** パラメーターの値を超えるネットワークエラーは復旧できません。これは、クライアントが再接続する前に、サーバー上の操作がタイムアウトになるためです。

```yaml
Type: System.UInt32
Parameter Sets: (All)
Aliases: OT

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Port

この接続に使用するリモート コンピューター上のネットワーク ポートを指定します。 リモート コンピューターに接続するには、リモート コンピューターで、接続に使用されるポートをリッスンすることが必要です。 既定のポート番号は 5985 (HTTP 用の WinRM ポート) と 5986 (HTTPS 用の WinRM ポート) です。

代替ポートを使用する前に、そのポートでリッスンするようにリモート コンピューター上の WinRM リスナーを構成する必要があります。 リスナーを構成するには、次のコマンドを使用します。

`winrm delete winrm/config/listener?Address=*+Transport=HTTP`

`winrm create winrm/config/listener?Address=*+Transport=HTTP @{Port="\<port-number>"}`

必要な場合を除き、**Port** パラメーターを使用しないでください。 コマンドのポート設定は、コマンドが実行されるすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。

```yaml
Type: System.UInt32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SessionOption

新しい CIM セッションの詳細オプションを設定します。 コマンドレットを使用して作成された **CimSessionOption** オブジェクトの名前を入力し [`New-CimSessionOption`](New-CimSessionOption.md) ます。

```yaml
Type: Microsoft.Management.Infrastructure.Options.CimSessionOptions
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SkipTestConnection

既定では、 `New-CimSession` コマンドレットは、リモートの WS-Management エンドポイントとの接続を確立します。これは、リモートサーバーが **port** パラメーターを使用して指定されたポート番号でリッスンしていることを確認し、指定されたアカウントの資格情報を検証するためです。 検証は、標準の WS-Identity 操作を使用して行われます。 リモート WS-Management エンドポイントが WS-ADDRESSING を使用できない場合や、データ転送時間を短縮する場合は、 **Skiptestconnection** スイッチパラメーターを追加できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

このコマンドレットは入力を受け入れません。

## 出力

### Microsoft.Management.Infrastructure.CimSession

## 注

## 関連リンク

[Get-ChildItem](../Microsoft.Powershell.Management/Get-ChildItem.md)

[Get-Credential](../Microsoft.PowerShell.Security/Get-Credential.md)

[Get-Item](../Microsoft.Powershell.Management/Get-Item.md)

[Get-CimSession](Get-CimSession.md)

[Remove-CimSession](Remove-CimSession.md)

[New-CimSessionOption](New-CimSessionOption.md)

[about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)

