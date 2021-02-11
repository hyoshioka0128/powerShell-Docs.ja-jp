---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 12/20/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-pssession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-PSSession
ms.openlocfilehash: 4b40d765002791f45f093c7912b8f9ba58248da9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599320"
---
# New-PSSession

## 概要
ローカル コンピューターまたはリモート コンピューターへの永続的な接続を作成します。

## SYNTAX

### ComputerName (既定値)

```
New-PSSession [[-ComputerName] <String[]>] [-Credential <PSCredential>] [-Name <String[]>]
 [-EnableNetworkAccess] [-ConfigurationName <String>] [-Port <Int32>] [-UseSSL]
 [-ApplicationName <String>] [-ThrottleLimit <Int32>] [-SessionOption <PSSessionOption>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [<CommonParameters>]
```

### Uri

```
New-PSSession [-Credential <PSCredential>] [-Name <String[]>] [-EnableNetworkAccess]
 [-ConfigurationName <String>] [-ThrottleLimit <Int32>] [-ConnectionUri] <Uri[]> [-AllowRedirection]
 [-SessionOption <PSSessionOption>] [-Authentication <AuthenticationMechanism>]
 [-CertificateThumbprint <String>] [<CommonParameters>]
```

### VMId

```
New-PSSession -Credential <PSCredential> [-Name <String[]>] [-ConfigurationName <String>]
 [-VMId] <Guid[]> [-ThrottleLimit <Int32>] [<CommonParameters>]
```

### VMName

```
New-PSSession -Credential <PSCredential> [-Name <String[]>] [-ConfigurationName <String>]
 -VMName <String[]> [-ThrottleLimit <Int32>] [<CommonParameters>]
```

### セッション

```
New-PSSession [[-Session] <PSSession[]>] [-Name <String[]>] [-EnableNetworkAccess]
 [-ThrottleLimit <Int32>] [<CommonParameters>]
```

### ContainerId

```
New-PSSession [-Name <String[]>] [-ConfigurationName <String>] -ContainerId <String[]>
 [-RunAsAdministrator] [-ThrottleLimit <Int32>] [<CommonParameters>]
```

### UseWindowsPowerShellParameterSet

```
New-PSSession [-Name <String[]>] [-UseWindowsPowerShell] [<CommonParameters>]
```

### SSHHost

```
New-PSSession [-Name <String[]>] [-Port <Int32>] [-HostName] <String[]> [-UserName <String>]
 [-KeyFilePath <String>] [-SSHTransport] [-Subsystem <String>] [<CommonParameters>]
```

### SSHHostHashParam

```
New-PSSession [-Name <String[]>] -SSHConnection <Hashtable[]> [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-PSSession` ローカルコンピューターまたはリモートコンピューターに PowerShell セッション (**PSSession**) を作成します。 **PSSession** を作成すると、PowerShell によって、リモートコンピューターへの永続的な接続が確立されます。

**PSSession** を使用して、関数や変数の値など、データを共有する複数のコマンドを実行します。 **PSSession** でコマンドを実行するには、コマンドレットを使用し `Invoke-Command` ます。 **PSSession** を使用してリモートコンピューターと直接対話するには、 `Enter-PSSession` コマンドレットを使用します。 詳細については、「 [about_PSSessions](about/about_PSSessions.md)」を参照してください。

またはの **ComputerName** パラメーターを使用して、 **PSSession** を作成せずにリモートコンピューターでコマンドを実行でき `Enter-PSSession` `Invoke-Command` ます。 **ComputerName** パラメーターを使用すると、PowerShell はコマンドに使用される一時的な接続を作成してから閉じます。

PowerShell 6.0 以降では、Secure Shell (SSH) を使用して接続を確立し、リモートコンピューター上にセッションを作成することができます (SSH がローカルコンピューターで利用可能で、リモートコンピューターに PowerShell SSH エンドポイントが構成されている場合)。 SSH ベースの PowerShell リモートセッションの利点は、複数のプラットフォーム (Windows、Linux、macOS) で動作できることです。 SSH ベースのセッションの場合は、 **HostName** パラメーターまたは **SSHConnection** パラメーターを設定して、リモートコンピューターと関連する接続情報を指定します。 PowerShell SSH リモート処理の設定方法の詳細については、「 [Ssh 経由の Powershell リモート](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)処理」を参照してください。

> [!NOTE]
> Linux または macOS クライアントから、サーバー証明書が信頼されていない HTTPS エンドポイント (自己署名証明書など) で WSMan リモート処理を使用する場合。 接続を正常に `PSSessionOption` `-SkipCACheck` 確立するには、とを含むを指定する必要があり `-SkipCNCheck` ます。 この操作は、サーバー証明書とターゲットシステムへのネットワーク接続を特定できる環境内にある場合にのみ実行してください。

## 例

### 例 1: ローカルコンピューターにセッションを作成する

```powershell
$s = New-PSSession
```

このコマンドは、ローカルコンピューター上に新しい **pssession** を作成し、その変数に **pssession** を保存し `$s` ます。

この **PSSession** を使用して、ローカルコンピューターでコマンドを実行できるようになりました。

### 例 2: リモートコンピューターにセッションを作成する

```powershell
$Server01 = New-PSSession -ComputerName Server01
```

このコマンドは、Server01 コンピューター上に新しい **PSSession** を作成し、変数に保存し `$Server01` ます。

複数の **PSSession** オブジェクトを作成する場合は、便利な名前の変数に割り当てます。 これは、後続のコマンドで **PSSession** オブジェクトを管理するのに役立ちます。

### 例 3: 複数のコンピューターでセッションを作成する

```powershell
$s1, $s2, $s3 = New-PSSession -ComputerName Server01,Server02,Server03
```

このコマンドは、 **ComputerName** パラメーターで指定された各コンピューター上に1つずつ、3つの **PSSession** オブジェクトを作成します。

このコマンドは、代入演算子 (=) を使用して、新しい **PSSession** オブジェクトを変数に代入します: `$s1` 、 `$s2` 、 `$s3` 。 Server01 **pssession** をに、Server02 `$s1` **Pssession** をに、 `$s2` Server03 **pssession** をに割り当て `$s3` ます。

複数のオブジェクトを一連の変数に割り当てると、PowerShell によって各オブジェクトが系列の変数にそれぞれ割り当てられます。 変数よりオブジェクトが多い場合、残りのすべてのオブジェクトが最後の変数に代入されます。 オブジェクトより変数が多い場合、残りの変数は空 (null) になります。

### 例 4: 指定したポートを使用してセッションを作成する

```powershell
New-PSSession -ComputerName Server01 -Port 8081 -UseSSL -ConfigurationName E12
```

このコマンドは、サーバーポート8081に接続し、SSL プロトコルを使用する Server01 コンピューター上に新しい **PSSession** を作成します。 新しい **PSSession** では、E12 という別のセッション構成を使用します。

ポートを設定する前に、ポート 8081 でリッスンするように、リモートのコンピューター上で WinRM リスナーを構成する必要があります。 詳細については、 **Port** パラメーターの説明を参照してください。

### 例 5: 既存のセッションに基づいてセッションを作成する

```powershell
New-PSSession -Session $s -Credential Domain01\User01
```

このコマンドは、既存の **pssession** と同じプロパティを使用して **pssession** を作成します。 このコマンド形式は、既存の **pssession** のリソースが使い果たされ、要求の一部をオフロードするために新しい **pssession** が必要な場合に使用できます。

このコマンドは、の **Session** パラメーターを使用して、 `New-PSSession` 変数に保存されている **PSSession** を指定し `$s` ます。 Domain1\Admin01 ユーザーの資格情報を使用して、コマンドを完成させます。

### 例 6: 別のドメイン内のグローバルスコープを持つセッションを作成する

```powershell
$global:s = New-PSSession -ComputerName Server1.Domain44.Corpnet.Fabrikam.com -Credential Domain01\Admin01
```

この例では、別のドメインにあるコンピューター上のグローバルスコープを持つ **PSSession** を作成する方法を示します。

既定では、コマンドラインで作成された **pssession** オブジェクトはローカルスコープで作成され、スクリプトで作成された **pssession** オブジェクトはスクリプトスコープを持ちます。

グローバルスコープの **pssession** を作成するには、新しい **pssession** を作成し、グローバルスコープにキャストされる変数に **pssession** を格納します。 この場合、 `$s` 変数はグローバルスコープにキャストされます。

このコマンドは、**ComputerName** パラメーターを使用してリモート コンピューターを指定します。 コンピューターがユーザーアカウントとは別のドメインにあるため、コンピューターの完全名がユーザーの資格情報と共に指定されます。

### 例 7: 多くのコンピューターのセッションを作成する

```powershell
$rs = Get-Content C:\Test\Servers.txt | New-PSSession -ThrottleLimit 50
```

このコマンドは、Servers.txt ファイルに一覧表示されている200コンピューターそれぞれに **pssession** を作成し、結果の **pssession** を変数に格納し `$rs` ます。 **PSSession** オブジェクトのスロットル制限は50です。

このコマンドの形式は、コンピューターの名前がデータベース、スプレッドシート、テキスト ファイル、またはその他のテキストに変換できる形式に格納されている場合、使用することができます。

### 例 8: URI を使用してセッションを作成する

```powershell
$s = New-PSSession -URI http://Server01:91/NewSession -Credential Domain01\User01
```

このコマンドは、Server01 コンピューター上に **PSSession** を作成し、変数に格納し `$s` ます。 **URI** パラメーターを使用して、トランスポートプロトコル、リモートコンピューター、ポート、および代替のセッション構成を指定します。 また、 **Credential** パラメーターを使用して、リモートコンピューター上にセッションを作成するアクセス許可を持つユーザーアカウントを指定します。

### 例 9: 一連のセッションでバックグラウンドジョブを実行する

```powershell
$s = New-PSSession -ComputerName (Get-Content Servers.txt) -Credential Domain01\Admin01 -ThrottleLimit 16
Invoke-Command -Session $s -ScriptBlock {Get-Process PowerShell} -AsJob
```

これらのコマンドは、 **pssession** オブジェクトのセットを作成し、各 **pssession** オブジェクトでバックグラウンドジョブを実行します。

最初のコマンドは、Servers.txt ファイルに一覧表示されている各コンピューター上に新しい **PSSession** を作成します。 コマンドレットを使用して `New-PSSession` **PSSession** を作成します。 **ComputerName** パラメーターの値は、コマンドレットを使用して、 `Get-Content` Servers.txt ファイルのコンピューター名の一覧を取得するコマンドです。

このコマンドは、 **Credential** パラメーターを使用して、ドメイン管理者のアクセス許可を持つ **PSSession** オブジェクトを作成します。また、 **ThrottleLimit** パラメーターを使用して、コマンドの同時接続数を16に制限します。 このコマンドは、 **PSSession** オブジェクトを変数に保存し `$s` ます。

2番目のコマンドは、コマンドレットの **AsJob** パラメーターを使用して、の `Invoke-Command` `Get-Process PowerShell` 各 **PSSession** オブジェクトでコマンドを実行するバックグラウンドジョブを開始し `$s` ます。

PowerShell バックグラウンドジョブの詳細については、「 [about_Jobs](About/about_Jobs.md) 」および「 [about_Remote_Jobs](About/about_Remote_Jobs.md)」を参照してください。

### 例 10: URI を使用してコンピューターのセッションを作成する

```powershell
New-PSSession -ConnectionURI https://management.exchangelabs.com/Management
```

このコマンドは、コンピューター名の代わりに URI で指定されたコンピューターに接続する **PSSession** オブジェクトを作成します。

### 例 11: セッションオプションを作成する

```powershell
$so = New-PSSessionOption -SkipCACheck
New-PSSession -ConnectionUri https://management.exchangelabs.com/Management -SessionOption $so -Credential Server01\Admin01
```

この例では、セッションオプションオブジェクトを作成し、 **sessionoption** パラメーターを使用する方法を示します。

最初のコマンドは、コマンドレットを使用して `New-PSSessionOption` セッションオプションを作成します。 結果として得られる **Sessionoption** オブジェクトを変数に保存し `$so` ます。

2 番目のコマンドは、新しいセッションでこのオプションを使用します。 コマンドは、コマンド `New-PSSession` レットを使用して新しいセッションを作成します。 SessionOption パラメーターの値は、変数内の **sessionoption** オブジェクトです `$so` 。

### 例 12: SSH を使用してセッションを作成する

```powershell
New-PSSession -HostName UserA@LinuxServer01
```

この例では、Secure Shell (SSH) を使用して新しい **PSSession** を作成する方法を示します。 SSH がリモートコンピューターでパスワードを要求するように構成されている場合は、パスワードの入力を求めるプロンプトが表示されます。 それ以外の場合は、SSH キーベースのユーザー認証を使用する必要があります。

### 例 13: SSH を使用してセッションを作成し、ポートとユーザー認証キーを指定する

```powershell
New-PSSession -HostName UserA@LinuxServer01:22 -KeyFilePath c:\<path>\userAKey_rsa
```

この例では、Secure Shell (SSH) を使用して **PSSession** を作成する方法を示します。 **Port** パラメーターを使用して、使用するポートを指定し、 **keyfilepath** パラメーターを使用して、リモートコンピューター上のユーザーを識別し、認証するために使用する RSA キーを指定します。

### 例 14: SSH を使用して複数のセッションを作成する

```powershell
$sshConnections = @{ HostName="WinServer1"; UserName="domain\userA"; KeyFilePath="c:\users\UserA\id_rsa" }, @{ HostName="UserB@LinuxServer5"; KeyFilePath="c:\UserB\<path>\id_rsa" }
New-PSSession -SSHConnection $sshConnections
```

この例では、Secure Shell (SSH) と **SSHConnection** パラメーターセットを使用して複数のセッションを作成する方法を示します。 **SSHConnection** パラメーターは、各セッションの接続情報を含むハッシュテーブルの配列を受け取ります。 この例では、ターゲットのリモートコンピューターで、キーベースのユーザー認証をサポートするように SSH が構成されている必要があることに注意してください。

## PARAMETERS

### -AllowRedirection

このコマンドレットがこの接続を代替 Uniform Resource Identifier (URI) にリダイレクトできることを示します。

**ConnectionURI** パラメーターを使用すると、リモートの送信先は別の URI にリダイレクトするように指示を返すことができます。 既定では、PowerShell は接続をリダイレクトしませんが、このパラメーターを使用して接続をリダイレクトできるようにすることができます。

また、**MaximumConnectionRedirectionCount** セッション オプション値を変更することで、接続をリダイレクトする回数を制限することもできます。 コマンドレットの **Maximumredirection** パラメーターを使用する `New-PSSessionOption` か、 **$PSSessionOption** preference 変数の **MaximumConnectionRedirectionCount** プロパティを設定します。 既定値は 5 です。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Uri
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ApplicationName

接続 URI のアプリケーション名セグメントを指定します。 このパラメーターは、このコマンドの **ConnectionURI** パラメーターを使用しないときに、アプリケーション名を指定するために使用します。

既定値は、 `$PSSessionApplicationName` ローカルコンピューター上のユーザー設定変数の値です。 このユーザー設定変数が定義されていない場合、既定値は WSMAN です。 この値はほとんどの用途に適しています。 詳細については、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

WinRM サービスは、アプリケーション名を使用して、接続要求を処理するリスナーを選択します。
このパラメーターの値は、リモート コンピューター上のリスナーの **URLPrefix** プロパティの値に一致している必要があります。

```yaml
Type: System.String
Parameter Sets: ComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -認証

ユーザーの資格情報の認証に使用するメカニズムを指定します。
このパラメーターの有効値は、次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential

既定値は Default です。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!CAUTION]
> ユーザー資格情報が認証対象のリモートコンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: ComputerName, Uri
Aliases:
Accepted values: Default, Basic, Negotiate, NegotiateWithImplicitCredential, Credssp, Digest, Kerberos

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

この処理を実行するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書を取得するに `Get-Item` は、 `Get-ChildItem` PowerShell Cert: ドライブのまたはコマンドを使用します。

```yaml
Type: System.String
Parameter Sets: ComputerName, Uri
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

コンピューターの名前の配列を指定します。 このコマンドレットは、指定されたコンピューターへの永続的な接続 (**PSSession**) を作成します。 複数のコンピューター名を入力すると、は、 `New-PSSession` コンピューターごとに1つずつ、複数の **PSSession** オブジェクトを作成します。 既定値はローカル コンピューターです。

1 台または複数のリモート コンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。 ローカルコンピューターを指定するには、コンピューター名、localhost、またはドット (.) を入力します。 コンピューターがユーザーとは異なるドメインにある場合は、完全修飾ドメイン名が必要です。 パイプを使用してコンピューター名を引用符で囲むこともでき `New-PSSession` ます。

**ComputerName** パラメーターの値に IP アドレスを使用するには、コマンドに **Credential** パラメーターを含める必要があります。 また、HTTPS トランスポート用にコンピューターを構成するか、リモート コンピューターの IP アドレスをローカル コンピューター上の WinRM TrustedHosts 一覧に含める必要があります。 TrustedHosts の一覧にコンピューター名を追加する手順については、 [about_Remote_Troubleshooting](about/about_Remote_Troubleshooting.md)の「信頼されたホストの一覧にコンピューターを追加する方法」を参照してください。

ローカルコンピューターを **ComputerName** パラメーターの値に含めるには、[管理者として実行] オプションを使用して Windows PowerShell を起動します。

```yaml
Type: System.String[]
Parameter Sets: ComputerName
Aliases: Cn

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -ConfigurationName

新しい **PSSession** に使用するセッション構成を指定します。

セッション構成の構成名または完全修飾リソース URI を入力します。 構成名だけを指定した場合は、次のスキーマ URI が付加され `http://schemas.microsoft.com/PowerShell` ます。

セッションのセッション構成は、リモート コンピューター上にあります。 指定したセッション構成がリモート コンピューター上に存在しない場合、コマンドは失敗します。

既定値は、 `$PSSessionConfigurationName` ローカルコンピューター上のユーザー設定変数の値です。 このユーザー設定変数が設定されていない場合、既定値は Microsoft.PowerShell です。 詳細については、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: ComputerName, Uri, VMId, VMName, ContainerId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ConnectionUri

セッションの接続エンドポイントを定義する URI を指定します。 URI は完全修飾名にする必要があります。 この文字列の形式は次のとおりです。

`<Transport>://<ComputerName>:<Port>/<ApplicationName>`

既定値は、次のとおりです。

`http://localhost:5985/WSMAN`

**ConnectionURI** を指定しなかった場合は、**UseSSL**、**ComputerName**、**Port**、および **ApplicationName** パラメーターを使用して、**ConnectionURI** 値を指定できます。

URI のトランスポート セグメントの有効な値は、HTTP および HTTPS です。 トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合、セッションは標準ポート80で HTTP 用および 443 (HTTPS 用) で作成されます。 PowerShell リモート処理に既定のポートを使用するには、HTTP の場合はポート5985、HTTPS の場合は5986を指定します。

対象のコンピューターが接続を別の URI にリダイレクトする場合、コマンドで **Allowredirection** パラメーターを使用しない限り、PowerShell によってリダイレクトが禁止されます。

```yaml
Type: System.Uri[]
Parameter Sets: Uri
Aliases: URI, CU

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ContainerId

コンテナーの Id の配列を指定します。 このコマンドレットは、指定された各コンテナーとの対話型セッションを開始します。 `docker ps`コンテナー id の一覧を取得するには、コマンドを使用します。 詳細については、 [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) コマンドのヘルプを参照してください。

```yaml
Type: System.String[]
Parameter Sets: ContainerId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

このアクションを実行するアクセス許可を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ComputerName, Uri, VMId, VMName
Aliases:

Required: True (VMId, VMName), False (ComputerName, Uri)
Position: Named
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -EnableNetworkAccess

このコマンドレットが、ループバックセッションに対話型セキュリティトークンを追加することを示します。 対話型トークンを使用すると、他のコンピューターからデータを取得するコマンドをループバック セッションで実行できます。 たとえば、リモート コンピューターからローカル コンピューターに XML ファイルをコピーするコマンドをセッションで実行できます。

ループバックセッションは、同じコンピューター上で発生して終了する **PSSession** です。 ループバックセッションを作成するには、 **ComputerName** パラメーターを省略するか、値をドット (.)、localhost、またはローカルコンピューターの名前に設定します。

既定では、このコマンドレットはネットワークトークンを使用してループバックセッションを作成しますが、これにより、リモートコンピューターを認証するための十分なアクセス許可が与えられない場合があります。

**EnableNetworkAccess** パラメーターは、ループバック セッションでのみ有効です。 リモートコンピューターでセッションを作成するときに **EnableNetworkAccess** を使用すると、コマンドは成功しますが、パラメーターは無視されます。

また、 **認証** パラメーターの CredSSP 値を使用して、ループバックセッションでリモートアクセスを有効にすることもできます。これにより、セッションの資格情報が他のコンピューターに委任されます。

悪意のあるアクセスからコンピューターを保護するために、 **EnableNetworkAccess** パラメーターを使用して作成された、対話型トークンを持つ切断されたループバックセッションは、セッションが作成されたコンピューターからのみ再接続することができます。 CredSSP 認証を使用するセッションが切断された場合には、他のコンピューターから再接続することができます。 詳細については、「`Disconnect-PSSession`」を参照してください。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName, Uri, Session
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -HostName

Secure Shell (SSH) ベースの接続に使用するコンピューター名の配列を指定します。 これは ComputerName パラメーターと似ていますが、リモートコンピューターへの接続は Windows WinRM ではなく SSH を使用して行われる点が異なります。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: SSHHost
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -KeyFilePath

リモートコンピューター上のユーザーを認証するために Secure Shell (SSH) によって使用されるキーファイルパスを指定します。

SSH では、基本パスワード認証の代わりに、秘密キーまたは公開キーを使用してユーザー認証を実行できます。 リモートコンピューターがキー認証用に構成されている場合、このパラメーターを使用して、ユーザーを識別するキーを提供できます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: SSHHost
Aliases: IdentityFilePath

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

**PSSession** のフレンドリ名を指定します。

やなどの他のコマンドレットを使用する場合は、名前を使用して **PSSession** を参照でき `Get-PSSession` `Enter-PSSession` ます。 名前は、コンピューターや現在のセッションで一意である必要ありません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Port

この接続に使用するリモート コンピューター上のネットワーク ポートを指定します。 リモート コンピューターに接続するには、リモート コンピューターで、接続に使用されるポートをリッスンすることが必要です。 既定のポートは5985で、これは HTTP 用の WinRM ポート、および HTTPS 用の WinRM ポートである5986です。

別のポートを使用する前に、そのポートでリッスンするようにリモートコンピューター上の WinRM リスナーを構成する必要があります。 リスナーを構成するには、次のコマンドを使用します。

1. `winrm delete winrm/config/listener?Address=*+Transport=HTTP`
2. `winrm create winrm/config/listener?Address=*+Transport=HTTP @{Port="\<port-number\>"}`

必要な場合を除き、**Port** パラメーターを使用しないでください。 コマンドのポート設定は、コマンドが実行されるすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。

```yaml
Type: System.Int32
Parameter Sets: ComputerName, SSHHost
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunAsAdministrator

**PSSession** が管理者として実行されることを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ContainerId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -セッション

このコマンドレットが新しい **pssession** のモデルとして使用する **pssession** オブジェクトの配列を指定します。 このパラメーターは、指定された **pssession** オブジェクトと同じプロパティを持つ新しい **PSSession** オブジェクトを作成します。

**Pssession** オブジェクトを含む変数、  `New-PSSession` またはコマンドやコマンドなどの pssession オブジェクトを作成または取得するコマンドを入力し `Get-PSSession` ます。

結果として得られる **PSSession** オブジェクトのコンピューター名、アプリケーション名、接続 URI、ポート、構成名、スロットル制限、および SECURE SOCKETS LAYER (SSL) 値は元のものと同じですが、表示名、ID、インスタンス ID (GUID) は異なります。

```yaml
Type: System.Management.Automation.Runspaces.PSSession[]
Parameter Sets: Session
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -SessionOption

セッションの詳細オプションを指定します。 コマンドレットを使用して作成する **sessionoption** オブジェクト、 `New-PSSessionOption` またはキーがセッションオプションの名前で、値がセッションオプションの値であるハッシュテーブルを入力します。

オプションの既定値は、設定されている場合は、ユーザー設定変数の値によって決まり `$PSSessionOption` ます。 それ以外の場合、既定値はセッション構成で設定されたオプションによって決まります。

セッションオプションの値は、ユーザー設定変数およびセッション構成で設定されたセッションの既定値よりも優先され `$PSSessionOption` ます。 ただし、セッション構成で設定された最大値、クォータ、または制限よりも優先されることはありません。

既定値を含むセッションオプションの説明については、「」を参照してください `New-PSSessionOption` 。 ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

```yaml
Type: System.Management.Automation.Remoting.PSSessionOption
Parameter Sets: ComputerName, Uri
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SSHConnection

このパラメーターは、ハッシュテーブルの配列を受け取ります。各ハッシュテーブルには、Secure Shell (SSH) 接続を確立するために必要な1つ以上の接続パラメーター (HostName、Port、UserName、KeyFilePath) が格納されています。

ハッシュテーブルの接続パラメーターは、 **HostName** パラメーターセットに対して定義されているものと同じです。

**SSHConnection** パラメーターは、各セッションが異なる接続情報を必要とする複数のセッションを作成する場合に便利です。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Collections.Hashtable[]
Parameter Sets: SSHHostHashParam
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SSHTransport

リモート接続が Secure Shell (SSH) を使用して確立されることを示します。

既定では、PowerShell は Windows WinRM を使用してリモートコンピューターに接続します。 このスイッチは、SSH ベースのリモート接続を確立するために、ホスト名パラメーターセットを使用するように PowerShell を強制します。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: SSHHost
Aliases:
Accepted values: true

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ThrottleLimit

このコマンドを実行するために確立できる最大コンカレント接続数を指定します。
このパラメーターを省略した場合、または値 0 (ゼロ) を入力した場合は、既定値の 32 が使用されます。

スロットル制限は現在のコマンドのみに適用され、セッションまたはコンピューターには適用されません。

```yaml
Type: System.Int32
Parameter Sets: ComputerName, Uri, VMId, VMName, Session, ContainerId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserName

リモートコンピューターでセッションを作成するために使用するアカウントのユーザー名を指定します。 ユーザー認証方法は、リモートコンピューターで Secure Shell (SSH) がどのように構成されているかによって異なります。

SSH が基本パスワード認証用に構成されている場合は、ユーザーのパスワードの入力を求められます。

SSH がキーベースのユーザー認証用に構成されている場合、KeyFilePath パラメーターを使用してキーファイルのパスを指定することができます。パスワードプロンプトは表示されません。 クライアントユーザーキーファイルが SSH の既知の場所にある場合、キーベースの認証に KeyFilePath パラメーターは必要ありません。ユーザー認証は、ユーザー名に基づいて自動的に実行されます。 詳細については、キーベースのユーザー認証に関する SSH ドキュメントを参照してください。

これは必須のパラメーターではありません。 UserName パラメーターが指定されていない場合は、現在のログオンユーザー名が接続に使用されます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: SSHHost
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSSL

このコマンドレットが SSL プロトコルを使用してリモートコンピューターへの接続を確立することを示します。
既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で送信されるすべての PowerShell コンテンツを暗号化します。 **UseSSL** パラメーターは、HTTP 接続ではなく HTTPS 接続を介してデータを送信する追加の保護を提供します。

このパラメーターを使用しても、コマンドに使用されているポートで SSL を使用できない場合、コマンドは失敗します。

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

### -VMId

仮想マシンの ID の配列を指定します。 このコマンドレットは、指定された各仮想マシンとの対話型セッションを開始します。 使用可能な仮想マシンを表示するには、次のコマンドを使用します。

`Get-VM | Select-Object -Property Name, ID`

```yaml
Type: System.Guid[]
Parameter Sets: VMId
Aliases: VMGuid

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -VMName

仮想マシンの名前の配列を指定します。 このコマンドレットは、指定された各仮想マシンとの対話型セッションを開始します。 使用可能な仮想マシンを表示するには、コマンドレットを使用し `Get-VM` ます。

```yaml
Type: System.String[]
Parameter Sets: VMName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -サブシステム

新しい **PSSession** に使用する SSH サブシステムを指定します。

これは sshd_config で定義されているターゲットで使用するサブシステムを指定します。
サブシステムは、定義済みのパラメーターを使用して特定のバージョンの PowerShell を起動します。
指定したサブシステムがリモートコンピューターに存在しない場合、コマンドは失敗します。

このパラメーターを使用しない場合、既定値は ' powershell ' サブシステムになります。

```yaml
Type: System.String
Parameter Sets: SSHHost
Aliases:

Required: False
Position: Named
Default value: powershell
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -UseWindowsPowerShell

{{UseWindowsPowerShell の説明を入力}}

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: UseWindowsPowerShellParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「about_CommonParameters」 (https://go.microsoft.com/fwlink/?LinkID=113216) ) を参照してください。

## 入力

### System.string、System.string、system......。

パイプを使用して、文字列、URI、またはセッションオブジェクトをこのコマンドレットに送ることができます。

## 出力

### システム管理. 実行空間

## 注

- このコマンドレットは、PowerShell リモート処理インフラストラクチャを使用します。 このコマンドレットを使用するには、ローカルコンピューターとリモートコンピューターを PowerShell リモート処理用に構成する必要があります。 詳細については、「[about_Remote_Requirements](About/about_Remote_Requirements.md)」を参照してください。
- ローカルコンピューター上に **PSSession** を作成するには、[管理者として実行] オプションを使用して PowerShell を起動します。
- **Pssession** が終了したら、コマンドレットを使用して `Remove-PSSession` **pssession** を削除し、そのリソースを解放します。
- **ホスト名** と **SSHConnection** パラメーターセットは、PowerShell 6.0 以降に含まれていました。
  これらは、Secure Shell (SSH) に基づいて PowerShell リモート処理を提供するために追加されました。 複数のプラットフォーム (Windows、Linux、macOS) では SSH と PowerShell の両方がサポートされており、powershell リモート処理は PowerShell と SSH がインストールおよび構成されているこれらのプラットフォーム上で機能します。 これは、WinRM に基づく以前の Windows のみのリモート処理とは別のものであり、WinRM 固有の機能の多くは適用されません。 たとえば、WinRM ベースのクォータ、セッションオプション、カスタムエンドポイントの構成、および切断/再接続機能は現在サポートされていません。 PowerShell SSH リモート処理の設定方法の詳細については、「 [Ssh 経由の Powershell リモート](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)処理」を参照してください。

## 関連リンク

[Connect-PSSession](Connect-PSSession.md)

[Disconnect-PSSession](Disconnect-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Exit-PSSession](Exit-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[Receive-PSSession](Receive-PSSession.md)

[Remove-PSSession](Remove-PSSession.md)

