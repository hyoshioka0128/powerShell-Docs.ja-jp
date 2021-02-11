---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 5/15/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-pssession?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSSession
ms.openlocfilehash: dfb5d6f46dec89495d19680cec8b73ad12340797
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388690"
---
# Get-PSSession

## 概要
ローカルコンピューターとリモートコンピューター上の PowerShell セッションを取得します。

## SYNTAX

### 名前 (既定値)

```
Get-PSSession [-Name <String[]>] [<CommonParameters>]
```

### [ComputerName]

```
Get-PSSession [-ComputerName] <String[]> [-ApplicationName <String>] [-ConfigurationName <String>]
 [-Name <String[]>] [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>]
 [-CertificateThumbprint <String>] [-Port <Int32>] [-UseSSL] [-ThrottleLimit <Int32>]
 [-State <SessionFilterState>] [-SessionOption <PSSessionOption>] [<CommonParameters>]
```

### ComputerInstanceId

```
Get-PSSession [-ComputerName] <String[]> [-ApplicationName <String>] [-ConfigurationName <String>]
 -InstanceId <Guid[]> [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>]
 [-CertificateThumbprint <String>] [-Port <Int32>] [-UseSSL] [-ThrottleLimit <Int32>]
 [-State <SessionFilterState>] [-SessionOption <PSSessionOption>] [<CommonParameters>]
```

### ConnectionUriInstanceId

```
Get-PSSession [-ConnectionUri] <Uri[]> [-ConfigurationName <String>] [-AllowRedirection] -InstanceId <Guid[]>
 [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [-ThrottleLimit <Int32>] [-State <SessionFilterState>] [-SessionOption <PSSessionOption>] [<CommonParameters>]
```

### ConnectionUri

```
Get-PSSession [-ConnectionUri] <Uri[]> [-ConfigurationName <String>] [-AllowRedirection] [-Name <String[]>]
 [-Credential <PSCredential>] [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [-ThrottleLimit <Int32>] [-State <SessionFilterState>] [-SessionOption <PSSessionOption>] [<CommonParameters>]
```

### ContainerId

```
Get-PSSession [-ConfigurationName <String>] [-Name <String[]>] [-State <SessionFilterState>]
 -ContainerId <String[]> [<CommonParameters>]
```

### ContainerIdInstanceId

```
Get-PSSession [-ConfigurationName <String>] -InstanceId <Guid[]> [-State <SessionFilterState>]
 -ContainerId <String[]> [<CommonParameters>]
```

### VMId

```
Get-PSSession [-ConfigurationName <String>] [-Name <String[]>] [-State <SessionFilterState>] -VMId <Guid[]>
 [<CommonParameters>]
```

### VMIdInstanceId

```
Get-PSSession [-ConfigurationName <String>] -InstanceId <Guid[]> [-State <SessionFilterState>] -VMId <Guid[]>
 [<CommonParameters>]
```

### VMName

```
Get-PSSession [-ConfigurationName <String>] [-Name <String[]>] [-State <SessionFilterState>] -VMName <String[]>
 [<CommonParameters>]
```

### VMNameInstanceId

```
Get-PSSession [-ConfigurationName <String>] -InstanceId <Guid[]> [-State <SessionFilterState>]
 -VMName <String[]> [<CommonParameters>]
```

### InstanceId

```
Get-PSSession [-InstanceId <Guid[]>] [<CommonParameters>]
```

### Id

```
Get-PSSession [-Id] <Int32[]> [<CommonParameters>]
```

## Description

`Get-PSSession`コマンドレットは、ローカルコンピューターとリモートコンピューター上 **PSSessions** のユーザー管理の PowerShell セッション (pssession) を取得します。

Windows PowerShell 3.0 以降では、セッションは各接続のリモートエンドのコンピューターに格納されます。 の **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用すると、 `Get-PSSession` 現在のセッションで作成されていない場合でも、ローカルコンピューターまたはリモートコンピューターに接続するセッションを取得できます。

パラメーターを指定しない場合は、 `Get-PSSession` 現在のセッションで作成されたすべてのセッションを取得します。

**名前** 、 **ID** 、 **InstanceID** 、 **State** 、 **ApplicationName** 、 **ConfigurationName** などのフィルター処理パラメーターを使用して、が返すセッションの中から選択し `Get-PSSession` ます。

残りのパラメーターを使用して、 `Get-PSSession` **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用したときにコマンドが実行される一時的な接続を構成します。

注: Windows PowerShell 2.0 では、パラメーターを指定しない場合、 `Get-PSSession` 現在のセッションで作成されたすべてのセッションを取得します。 **ComputerName** パラメーターは、現在のセッションで作成されたセッションを取得し、指定されたコンピューターに接続します。

PowerShell セッションの詳細については、「 [about_PSSessions](about/about_PSSessions.md)」を参照してください。

## 例

### 例 1: 現在のセッションで作成されたセッションを取得する

```powershell
Get-PSSession
```

このコマンド **は、現在** のセッションで作成されたすべての pssession を取得します。 このコンピューターに接続している場合でも、他のセッションまたは他のコンピューターで作成さ **れた pssession は取得さ** れません。

### 例 2: ローカルコンピューターに接続されているセッションを取得する

```powershell
Get-PSSession -ComputerName "localhost"
```

このコマンド **は、ローカル** コンピューターに接続されている pssession を取得します。 ローカルコンピューターを指定するには、コンピューター名、localhost、またはドット (.) を入力します。

このコマンドは、別のセッションまたは別のコンピューターで作成された場合でも、ローカル コンピューター上のセッションをすべて返します。

### 例 3: コンピューターに接続されているセッションを取得する

```powershell
Get-PSSession -ComputerName "Server02"
```

```Output
 Id Name            ComputerName    State         ConfigurationName     Availability
 -- ----            ------------    -----         -----------------     ------------
  2 Session3        Server02       Disconnected  ITTasks                       Busy
  1 ScheduledJobs   Server02       Opened        Microsoft.PowerShell     Available
  3 Test            Server02       Disconnected  Microsoft.PowerShell          Busy
```

このコマンドは、Server02 コンピューターに接続され **ている pssession** を取得します。

このコマンドは、別のセッションまたは別のコンピューターで作成された場合でも、Server02 上のセッションをすべて返します。

出力は、状態が Disconnected で可用性が Busy のセッションが 2 つあることを示しています。 これらのセッション別のセッションで作成されており、現在使用されています。 開かれている ScheduledJobs セッションは、現在のセッションで作成されています。

### 例 4: このコマンドの結果を保存する

```powershell
New-PSSession -ComputerName Server01, Server02, Server03
$s1, $s2, $s3 = Get-PSSession
```

この例では、コマンドの結果を複数の変数に保存する方法を示し `Get-PSSession` ます。

最初のコマンドは、 `New-PSSession` コマンドレットを使用 **して、** 3 台のリモートコンピューター上に pssession を作成します。

2番目のコマンドは、コマンドレットを使用し `Get-PSSession` て、3つの pssession を取得 **し** ます。 次に、それぞれの **Pssessions** を別々の変数に保存します。

PowerShell がオブジェクトの配列を変数の配列に割り当てると、最初のオブジェクトが最初の変数に割り当てられ、2番目のオブジェクトが2番目の変数に代入されます。 オブジェクトの数が変数の数よりも多い場合、残りのオブジェクトはすべて配列の最後の変数に割り当てられます。 変数の数がオブジェクトの数よりも多い場合、余分な変数は使用されません。

### 例 5: インスタンス ID を使用してセッションを削除する

```powershell
Get-PSSession | Format-Table -Property ComputerName, InstanceID
$s = Get-PSSession -InstanceID a786be29-a6bb-40da-80fb-782c67f7db0f
Remove-PSSession -Session $s
```

この例では、インスタンス ID を使用して **pssession** を取得し、 **pssession** を削除する方法を示します。

最初のコマンドは、現在のセッションで作成さ **れたすべて** の pssession を取得します。 **Pssessions** を Format-Table コマンドレットに送信します。これにより、各 **PSSession** の **ComputerName** および **InstanceID** プロパティが表示されます。

2番目のコマンドは、コマンドレットを使用して `Get-PSSession` 特定の **PSSession** を取得し、変数に保存し `$s` ます。 このコマンドは、 **InstanceID** パラメーターを使用して **PSSession** を識別します。

3番目のコマンドは、Remove-PSSession コマンドレットを使用して、変数内の **PSSession** を削除し `$s` ます。

### 例 6: 特定の名前を持つセッションを取得する

この例のコマンドは、特定の名前形式に準拠し、特定のセッション構成を使用するセッションを探して接続します。 このようなコマンドを使用すれば、同僚が開始したタスクのセッションを探して接続し、そのタスクを完了することができます。

```powershell
Get-PSSession -ComputerName Server02, Server12 -Name BackupJob* -ConfigurationName ITTasks -SessionOption @{OperationTimeout=240000}
```

```Output
 Id Name            ComputerName    State         ConfigurationName     Availability
 -- ----            ------------    -----         -----------------     ------------
  3 BackupJob04     Server02        Disconnected        ITTasks                  None
```

```powershell
$s = Get-PSSession -ComputerName Server02 -Name BackupJob04 -ConfigurationName ITTasks | Connect-PSSession
$s
```

```Output
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
 5 BackupJob04     Server02        Opened        ITTasks                  Available
```

最初のコマンドは、Server02 および Server12 リモートコンピューター上のセッションを取得します。名前は BackupJob で始まり、ITTasks セッション構成を使用します。このコマンドは、 **name** パラメーターを使用して名前のパターンを指定し、 **ConfigurationName** パラメーターを使用してセッション構成を指定します。 **SessionOption** パラメーターの値は、 **OperationTimeout** の値を 240,000 ミリ秒 (4 分) に設定するハッシュ テーブルです。 この設定により、コマンドの完了に時間がかかるようになります。 **ConfigurationName** オプションと **sessionoption** パラメーターを使用して、 `Get-PSSession` 各コンピューターでコマンドレットが実行される一時的なセッションを構成します。出力は、コマンドが BackupJob04 セッションを返すことを示しています。 セッションは切断されており、 **可用性** は None であり、使用されていないことを示しています。

2番目のコマンドは、コマンドレットを使用して `Get-PSSession` BackupJob04 セッションにアクセスし、Connect-PSSession コマンドレットを使用してセッションに接続します。 このコマンドは、セッションを $s 変数に保存します。

3 番目のコマンドは、$s 変数に保存されているセッションを取得します。 出力は、コマンドが成功したことを示して `Connect-PSSession` います。 このセッションは、 **Opened** 状態にあり、使用可能です。

### 例 7: ID を使用してセッションを取得する

```powershell
Get-PSSession -Id 2
```

このコマンドは、ID が2の **PSSession** を取得します。 **Id** プロパティの値は現在のセッションでのみ一意であるため、 **id** パラメーターはローカルコマンドに対してのみ有効です。

## PARAMETERS

### -AllowRedirection

このコマンドレットがこの接続を代替 Uniform Resource Identifier (URI) にリダイレクトできることを示します。 既定では、PowerShell は接続をリダイレクトしません。

このパラメーター `Get-PSSession` は、 **connectionuri** パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ConnectionUri, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ApplicationName

アプリケーションの名前を指定します。 このコマンドレットは、指定されたアプリケーションを使用するセッションにのみ接続します。

接続 URI のアプリケーション名セグメントを入力します。 たとえば、次の接続 URI では、アプリケーション名は WSMan: `http://localhost:5985/WSMAN` です。 セッションのアプリケーション名は、セッションの **Runspace.ConnectionInfo.AppName** プロパティに保存されています。

このパラメーター値は、セッションを選択してフィルター処理するために使用されます。 セッションが使用するアプリケーションが変更されることはありません。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerName
Aliases:

Required: False
Position: Named
Default value: All sessions
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -認証

コマンドを実行するセッションの資格情報の認証に使用されるメカニズムを指定し `Get-PSSession` ます。

このパラメーターは、 `Get-PSSession` **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成します。

このパラメーターの有効値は、次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential.

既定値は Default です。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

注意: ユーザーの資格情報が認証対象のリモートコンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId
Aliases:
Accepted values: Default, Basic, Negotiate, NegotiateWithImplicitCredential, Credssp, Digest, Kerberos

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

コマンドを実行するセッションを作成するアクセス許可を持つユーザーアカウントのデジタル公開キー証明書 (X509) を指定し `Get-PSSession` ます。 証明書の拇印を入力します。

このパラメーターは、 `Get-PSSession` **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成します。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書の拇印を取得するには、PowerShell Cert: ドライブで Get-Item または Get-ChildItem コマンドを使用します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

コンピューターの名前の配列を指定します。 指定されたコンピューターに接続しているセッションを取得します。
ワイルドカード文字は使用できません。 既定値はありません。

Windows PowerShell 3.0 以降では、 **PSSession** オブジェクトは、各接続のリモート側のコンピューターに格納されます。 指定されたコンピューター上のセッションを取得するために、PowerShell は各コンピューターへの一時的な接続を作成し、コマンドを実行し `Get-PSSession` ます。

1 台以上のコンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。 ローカルコンピューターを指定するには、コンピューター名、localhost、またはドット (.) を入力します。

注: このパラメーターは、Windows PowerShell 3.0 以降のバージョンの PowerShell を実行しているコンピューターからのみセッションを取得します。 それ以前のバージョンではセッションが保存されません。

```yaml
Type: System.String[]
Parameter Sets: ComputerInstanceId, ComputerName
Aliases: Cn

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ConfigurationName

構成の名前を指定します。 このコマンドレットは、指定されたセッション構成を使用するセッションのみを取得します。

セッション構成の構成名または完全修飾リソース URI を入力します。 構成名だけを指定した場合は、次のスキーマ URI が付加され `http://schemas.microsoft.com/powershell` ます。 セッションの構成名は、セッションの **ConfigurationName** プロパティに保存されています。

このパラメーター値は、セッションを選択してフィルター処理するために使用されます。 セッションが使用するセッション構成が変更されることはありません。

セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId, ContainerId, ContainerIdInstanceId, VMId, VMIdInstanceId, VMName, VMNameInstanceId
Aliases:

Required: False
Position: Named
Default value: All sessions
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ConnectionUri

コマンドが実行される一時的なセッションの接続エンドポイントを定義する URI を指定し `Get-PSSession` ます。 URI は完全修飾名にする必要があります。

このパラメーター `Get-PSSession` は、 **connectionuri** パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成します。

この文字列の形式は次のとおりです。

`<Transport>://<ComputerName>:<Port\>/<ApplicationName>`

既定値は `http://localhost:5985/WSMAN` です。

**Connectionuri** を指定しない場合は、 **UseSSL** 、 **ComputerName** 、 **Port** 、および **ApplicationName** パラメーターを使用して、 **connectionuri** 値を指定できます。 URI のトランスポート セグメントの有効な値は、HTTP および HTTPS です。 トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合、セッションは標準ポート80で HTTP 用および 443 (HTTPS 用) で作成されます。 PowerShell リモート処理に既定のポートを使用するには、HTTP の場合はポート5985、HTTPS の場合は5986を指定します。

対象のコンピューターが接続を別の URI にリダイレクトする場合、コマンドで **Allowredirection** パラメーターを使用しない限り、PowerShell によってリダイレクトが禁止されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

このパラメーターは、Windows powershell 3.0 以降のバージョンの Windows PowerShell を実行しているコンピューターからのみセッションを取得します。 それ以前のバージョンではセッションが保存されません。

```yaml
Type: System.Uri[]
Parameter Sets: ConnectionUri, ConnectionUriInstanceId
Aliases: URI, CU

Required: True
Position: 0
Default value: Http://localhost:5985/WSMAN
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ContainerId

コンテナーの Id の配列を指定します。 このコマンドレットは、指定された各コンテナーとの対話型セッションを開始します。 `docker ps`コンテナー id の一覧を取得するには、コマンドを使用します。 詳細については、 [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) コマンドのヘルプを参照してください。

```yaml
Type: System.String[]
Parameter Sets: ContainerId, ContainerIdInstanceId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

ユーザー資格情報を指定します。 このコマンドレットは、指定したユーザーのアクセス許可を使用してコマンドを実行します。 リモートコンピューターに接続してコマンドを実行するアクセス許可を持つユーザーアカウントを指定し `Get-PSSession` ます。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

このパラメーターは、 `Get-PSSession` **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

セッション Id の配列を指定します。 このコマンドレットは、指定された Id を持つセッションのみを取得します。 1つ以上の Id をコンマで区切って入力するか、範囲演算子 (..) を使用して Id の範囲を指定します。 ID パラメーターを **ComputerName** パラメーターと共に使用することはできません。

ID は、現在のセッションでユーザーが管理するセッションを一意に識別する整数です。 これは **InstanceId** よりも覚えやすく、入力も簡単ですが、現在のセッション内でのみ一意です。 セッションの ID は、セッションの ID プロパティに保存されています。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases:

Required: True
Position: 0
Default value: All sessions
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InstanceId

セッションのインスタンス Id の配列を指定します。 このコマンドレットは、指定されたインスタンス Id を持つセッションのみを取得します。

インスタンス ID は、ローカル コンピューターまたはリモート コンピューターのセッションを一意に識別する GUID です。 **インスタンス** id は、PowerShell で複数のセッションが実行されている場合でも一意です。

セッションのインスタンス ID は、セッションの **InstanceID** プロパティに保存されています。

```yaml
Type: System.Guid[]
Parameter Sets: ComputerInstanceId, ConnectionUriInstanceId, ContainerIdInstanceId, VMIdInstanceId, VMNameInstanceId, InstanceId
Aliases:

Required: True (ComputerInstanceId, ConnectionUriInstanceId, ContainerIdInstanceId, VMIdInstanceId, VMNameInstanceId), False (InstanceId)
Position: Named
Default value: All sessions
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

セッション名の配列を指定します。 このコマンドレットは、指定されたフレンドリ名を持つセッションのみを取得します。 ワイルドカード文字を使用できます。

セッションのフレンドリ名は、セッションの **Name** プロパティに保存されています。

```yaml
Type: System.String[]
Parameter Sets: Name, ComputerName, ConnectionUri, ContainerId, VMId, VMName
Aliases:

Required: False
Position: Named
Default value: All sessions
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Port

コマンドを実行する一時的な接続に使用される、指定されたネットワークポートを指定し `Get-PSSession` ます。 リモート コンピューターに接続するには、リモート コンピューターで、接続に使用されるポートをリッスンすることが必要です。 既定のポートは5985で、これは HTTP 用の WinRM ポート、および HTTPS 用の WinRM ポートである5986です。

代替ポートを使用する前に、そのポートでリッスンするようにリモート コンピューター上の WinRM リスナーを構成する必要があります。 リスナーを構成するには、PowerShell プロンプトで次の2つのコマンドを入力します。

`Remove-Item -Path WSMan:\Localhost\listener\listener* -Recurse`

`New-Item -Path WSMan:\Localhost\listener -Transport http -Address * -Port \<port-number\>`

このパラメーターは、 `Get-PSSession` **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成します。

必要な場合を除き、 **Port** パラメーターを使用しないでください。 コマンドで設定された **ポート** は、コマンドを実行するすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: ComputerInstanceId, ComputerName
Aliases:

Required: False
Position: Named
Default value: 5985, 5986
Accept pipeline input: False
Accept wildcard characters: False
```

### -SessionOption

セッションの詳細オプションを指定します。 New-PSSessionOption コマンドレットを使用して作成する **sessionoption** オブジェクト、またはキーがセッションオプションの名前で、値がセッションオプションの値であるハッシュテーブルを入力します。

オプションの既定値は、$PSSessionOption ユーザー設定変数が設定されている場合は、その値によって決まります。 それ以外の場合、既定値はセッション構成で設定されたオプションによって決まります。

セッション オプションの値は、$PSSessionOption ユーザー設定変数およびセッション構成で設定されたセッションの既定値よりも優先されます。 ただし、セッション構成で設定された最大値、クォータ、または制限よりも優先されることはありません。

既定値を含む、セッションオプションの説明については、「」を参照してください `New-PSSessionOption` 。
ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

```yaml
Type: System.Management.Automation.Remoting.PSSessionOption
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -状態

セッション状態を指定します。 このコマンドレットは、指定された状態のセッションのみを取得します。 このパラメーターに指定できる値は、All、Opened、Disconnected、Closed、および壊れるです。 既定値は All です。

セッション状態の値は、現在のセッションを基準にしています。 現在のセッションで作成されておらず、現在のセッションに接続されていないセッションは、別のセッションに接続されている場合でも、状態は Disconnected になります。

セッションの状態は、セッションの **State** プロパティに保存されています。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: Microsoft.PowerShell.Commands.SessionFilterState
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId, ContainerId, ContainerIdInstanceId, VMId, VMIdInstanceId, VMName, VMNameInstanceId
Aliases:
Accepted values: All, Opened, Disconnected, Closed, Broken

Required: False
Position: Named
Default value: All
Accept pipeline input: False
Accept wildcard characters: False
```

### -ThrottleLimit

コマンドを実行するために確立できる同時接続の最大数を指定し `Get-PSSession` ます。 このパラメーターを省略した場合、または値 0 (ゼロ) を入力した場合は、既定値の 32 が使用されます。 スロットル制限は現在のコマンドのみに適用され、セッションまたはコンピューターには適用されません。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: ComputerInstanceId, ComputerName, ConnectionUri, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: 32
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSSL

このコマンドレットが Secure Sockets Layer (SSL) プロトコルを使用して、コマンドが実行される接続を確立することを示し `Get-PSSession` ます。 既定では、SSL は使用されません。 コマンドに使用するポートで SSL が使用できない場合に、このパラメーターを指定すると、コマンドは失敗します。

このパラメーターは、ComputerName パラメーターを使用してコマンドを実行するために作成される一時的な接続を構成し `Get-PSSession` ます。 **ComputerName**

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerInstanceId, ComputerName
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -VMId

仮想マシンの ID の配列を指定します。 このコマンドレットは、指定された各仮想マシンとの対話型セッションを開始します。 使用可能な仮想マシンを表示するには、次のコマンドを使用します。

`Get-VM | Select-Object -Property Name, ID`

```yaml
Type: System.Guid[]
Parameter Sets: VMId, VMIdInstanceId
Aliases: VMGuid

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -VMName

仮想マシンの名前の配列を指定します。 このコマンドレットは、指定された各仮想マシンとの対話型セッションを開始します。 使用可能な仮想マシンを表示するには、コマンドレットを使用し `Get-VM` ます。

```yaml
Type: System.String[]
Parameter Sets: VMName, VMNameInstanceId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システム管理. 実行空間

## 注

- このコマンドレットは、新しい PSSession **PSSession** 、 `Enter-PSSession` 、および Invoke-Command コマンドレットを使用して作成された、ユーザーが管理するセッション PSSession オブジェクトを取得します。 PowerShell の起動時に作成されるシステム管理セッションは取得されません。
- Windows PowerShell 3.0 以降では、 **PSSession** オブジェクトは、接続のサーバー側または受信側のコンピューターに格納されます。 ローカルコンピューターまたはリモートコンピューターに格納されているセッションを取得するために、PowerShell は、指定されたコンピューターに一時的なセッションを確立し、セッションでクエリコマンドを実行します。
- リモート コンピューターに接続しているセッションを取得するには、 **ComputerName** パラメーターまたは **ConnectionUri** パラメーターを使用してリモート コンピューターを指定します。 が取得するセッションをフィルター処理するには、 `Get-PSSession` **Name** 、 **ID** 、 **InstanceID** 、および **State** パラメーターを使用します。 残りのパラメーターを使用して、で使用する一時的なセッションを構成し `Get-PSSession` ます。
- **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用すると、は、 `Get-PSSession` Windows powershell 3.0 以降のバージョンの powershell を実行しているコンピューターからのセッションのみを取得します。
- **PSSession** の **State** プロパティの値は、現在のセッションを基準としています。
  したがって、値が **Disconnected** の場合は、 **PSSession** が現在のセッションに接続されていないことを意味します。 ただし、 **PSSession** がすべてのセッションから切断されているわけではありません。 別のセッションに接続されている可能性があるためです。 現在のセッションから **PSSession** に接続または再接続できるかどうかを判断するには、 **Availability** プロパティを使用します。

**Availability** の値が **None** の場合は、セッションに接続できることを示します。 値が **Busy** の場合は、PSSession が別のセッションに接続されているため、 **PSSession** に接続できないことを示します。

セッションの **State** プロパティの値の詳細については、「 [RunspaceState Enumeration](/dotnet/api/system.management.automation.runspaces.runspacestate)」を参照してください。

セッションの **Availability** プロパティの値の詳細については、「 [RunspaceAvailability Enumeration](/dotnet/api/system.management.automation.runspaces.runspaceavailability)」を参照してください。

## 関連リンク

[Connect-PSSession](Connect-PSSession.md)

[Disconnect-PSSession](Disconnect-PSSession.md)

[Receive-PSSession](Receive-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Exit-PSSession](Exit-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)

[Remove-PSSession](Remove-PSSession.md)

[about_PSSessions](About/about_PSSessions.md)

[about_Remote](About/about_Remote.md)
