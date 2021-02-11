---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 07/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enter-pssession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enter-PSSession
ms.openlocfilehash: 9c08e78d840815a396b55eb26bf8e21d73cb81aa
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601005"
---
# Enter-PSSession

## 概要
リモート コンピューターとの対話型セッションを開始します。

## SYNTAX

### ComputerName (既定値)

```
Enter-PSSession [-ComputerName] <String> [-EnableNetworkAccess] [[-Credential] <PSCredential>]
 [-ConfigurationName <String>] [-Port <Int32>] [-UseSSL] [-ApplicationName <String>]
 [-SessionOption <PSSessionOption>] [-Authentication <AuthenticationMechanism>]
 [-CertificateThumbprint <String>] [<CommonParameters>]
```

### SSHHost

```
Enter-PSSession [-HostName] <String> [-Port <Int32>] [-UserName <String>] [-KeyFilePath <String>]
 [-SSHTransport] [<CommonParameters>]
```

### セッション

```
Enter-PSSession [[-Session] <PSSession>] [<CommonParameters>]
```

### Uri

```
Enter-PSSession [[-ConnectionUri] <Uri>] [-EnableNetworkAccess] [[-Credential] <PSCredential>]
 [-ConfigurationName <String>] [-AllowRedirection] [-SessionOption <PSSessionOption>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [<CommonParameters>]
```

### InstanceId

```
Enter-PSSession [-InstanceId <Guid>] [<CommonParameters>]
```

### Id

```
Enter-PSSession [[-Id] <Int32>] [<CommonParameters>]
```

### 名前

```
Enter-PSSession [-Name <String>] [<CommonParameters>]
```

### VMId

```
Enter-PSSession [-VMId] <Guid> [-Credential] <PSCredential> [-ConfigurationName <String>] [<CommonParameters>]
```

### VMName

```
Enter-PSSession [-VMName] <String> [-Credential] <PSCredential> [-ConfigurationName <String>]
 [<CommonParameters>]
```

### ContainerId

```
Enter-PSSession [-ContainerId] <String> [-ConfigurationName <String>] [-RunAsAdministrator]
 [<CommonParameters>]
```

## Description

`Enter-PSSession`コマンドレットは、1台のリモートコンピューターで対話型セッションを開始します。
セッション中、入力したコマンドはリモートコンピューター上で直接入力した場合と同じように、リモートコンピューター上で実行されます。 一度に 1 つのみの対話型セッションを確立できます。

通常、**ComputerName** パラメーターを使用して、リモート コンピューターの名前を指定します。
ただし、 `New-PSSession` 対話型セッションのコマンドレットを使用して作成したセッションを使用することもできます。 ただし、 `Disconnect-PSSession` 、 `Connect-PSSession` 、またはコマンドレットを使用して、 `Receive-PSSession` 対話型セッションとの接続を切断したり、再接続したりすることはできません。

PowerShell 6.0 以降では、Secure Shell (SSH) を使用してリモートコンピューターへの接続を確立できます (SSH がローカルコンピューターで利用可能で、リモートコンピューターに PowerShell SSH エンドポイントが構成されている場合)。 SSH ベースの PowerShell リモートセッションの利点は、複数のプラットフォーム (Windows、Linux、macOS) で動作することです。 SSH ベースのリモート処理では、 **HostName** パラメーターを設定して、リモートコンピューターと関連する接続情報を指定します。 PowerShell SSH リモート処理の設定方法の詳細については、「 [Ssh 経由の Powershell リモート](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)処理」を参照してください。

対話型セッションを終了し、リモートコンピューターから切断するには、 `Exit-PSSession` コマンドレットを使用するか、「」と入力 `exit` します。

## 例

### 例 1: 対話型セッションを開始する

```
PS> Enter-PSSession
[localhost]: PS>
```

このコマンドは、ローカル コンピューターで対話型セッションを開始します。 コマンド プロンプトが変更され、現在、別のセッションでコマンドを実行していることを示します。

入力したコマンドは新しいセッションで実行され、結果がテキストとして既定のセッションに返されます。

### 例 2: 対話型セッションを操作する

最初のコマンドは、 `Enter-PSSession` コマンドレットを使用して、リモートコンピューター Server01 との対話型セッションを開始します。 セッションを開始すると、コマンド プロンプトが変更され、コンピューター名が含まれます。

2 番目のコマンドは、PowerShell プロセスを取得し、Process.txt ファイルに出力をリダイレクトします。 コマンドは、リモート コンピューターに送信され、ファイルはリモート コンピューター上に保存されます。

3番目のコマンドは、 **Exit** キーワードを使用して対話型セッションを終了し、接続を閉じます。
4 番目のコマンドは、Process.txt ファイルがリモート コンピューター上にあることを確認します。 `Get-ChildItem`ローカルコンピューター上の ("dir") コマンドがファイルを見つけることができません。

```powershell
PS C:\> Enter-PSSession -ComputerName Server01
[Server01]: PS>
[Server01]: PS C:\> Get-Process PowerShell > C:\ps-test\Process.txt
[Server01]: PS C:\> exit
PS C:\>
PS C:\> dir C:\ps-test\process.txt
Get-ChildItem : Cannot find path 'C:\ps-test\process.txt' because it does not exist.
At line:1 char:4
+ dir <<<<  c:\ps-test\process.txt
```

このコマンドは、リモート コンピューターとの対話型セッションで作業する方法を示します。

### 例 3: セッションパラメーターを使用する

```powershell
PS> $s = New-PSSession -ComputerName Server01
PS> Enter-PSSession -Session $s
[Server01]: PS>
```

これらのコマンドは、の **Session** パラメーターを使用して、 `Enter-PSSession` 既存の PowerShell セッション (**PSSession**) で対話型セッションを実行します。

### 例 4: 対話型セッションを開始し、ポートと資格情報のパラメーターを指定する

```powershell
PS> Enter-PSSession -ComputerName Server01 -Port 90 -Credential Domain01\User01
[Server01]: PS>
```

このコマンドは、Server01 コンピューターとの対話型セッションを開始します。 **Port パラメーターを** 使用してポートを指定し、 **Credential** パラメーターを使用して、リモートコンピューターに接続する権限を持つユーザーのアカウントを指定します。

### 例 5: 対話型セッションを停止する

```powershell
PS> Enter-PSSession -ComputerName Server01
[Server01]: PS> Exit-PSSession
PS>
```

この例は、対話型セッションを開始および停止する方法を示しています。 最初のコマンドは、 `Enter-PSSession` コマンドレットを使用して、Server01 コンピューターとの対話型セッションを開始します。

2番目のコマンドは、 `Exit-PSSession` コマンドレットを使用してセッションを終了します。 また、 **Exit** キーワードを使用して、対話型セッションを終了することもできます。 `Exit-PSSession` また、 **Exit** は同じ効果を持ちます。

### 例 6: SSH を使用して対話型セッションを開始する

```powershell
PS> Enter-PSSession -HostName UserA@LinuxServer01
```

この例では、Secure Shell (SSH) を使用して対話型セッションを開始する方法を示します。 SSH がリモートコンピューターでパスワードを要求するように構成されている場合は、パスワードの入力を求めるプロンプトが表示されます。
それ以外の場合は、SSH キーベースのユーザー認証を使用する必要があります。

### 例 7: SSH を使用して対話型セッションを開始し、ポートとユーザー認証キーを指定する

```powershell
PS> Enter-PSSession -HostName UserA@LinuxServer02:22 -KeyFilePath c:\<path>\userAKey_rsa
```

この例では、SSH を使用して対話型セッションを開始する方法を示します。 **Port** パラメーターを使用して、使用するポートを指定し、 **keyfilepath** パラメーターを使用して、リモートコンピューター上でユーザーを認証するために使用する RSA キーを指定します。

## PARAMETERS

### -AllowRedirection

この接続を代替 Uniform Resource Identifier (URI) にリダイレクトできます。 既定では、リダイレクトは許可されません。

**ConnectionURI** パラメーターを使用すると、リモートの送信先は別の URI にリダイレクトするように指示を返すことができます。 既定では、PowerShell は接続をリダイレクトしませんが、このパラメーターを使用して接続をリダイレクトすることができます。

また、**MaximumConnectionRedirectionCount** セッション オプション値を変更することで、接続をリダイレクトする回数を制限することもできます。 コマンドレットの **Maximumredirection** パラメーターを使用する `New-PSSessionOption` か、Preference 変数の **MaximumConnectionRedirectionCount** プロパティを設定し `$PSSessionOption` ます。 既定値は 5 です。

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

**WinRM** サービスは、アプリケーション名を使用して、接続要求を処理するリスナーを選択します。 このパラメーターの値は、リモート コンピューター上のリスナーの **URLPrefix** プロパティの値に一致している必要があります。

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

ユーザーの資格情報の認証に使用するメカニズムを指定します。 このパラメーターの有効値は、次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential

既定値は Default です。

CredSSP 認証は、windows Vista、Windows Server 2008、およびそれ以降のバージョンの Windows オペレーティングシステムでのみ使用できます。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

注意: ユーザーの資格情報が認証対象のリモートコンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

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

コンピューター名を指定します。 このコマンドレットは、指定されたリモートコンピューターとの対話型セッションを開始します。 コンピューター名を1 つだけ入力します。 既定値はローカル コンピューターです。

コンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。 パイプを使用してコンピューター名をにパイプすることもでき `Enter-PSSession` ます。

**ComputerName** パラメーターの値に IP アドレスを使用するには、コマンドに **Credential** パラメーターを含める必要があります。 また、HTTPS トランスポート用にコンピューターを構成するか、リモート コンピューターの IP アドレスをローカル コンピューター上の WinRM TrustedHosts 一覧に含める必要があります。 TrustedHosts の一覧にコンピューター名を追加する手順については、 [about_Remote_Troubleshooting](About/about_Remote_Troubleshooting.md)の「信頼されたホストの一覧にコンピューターを追加する方法」を参照してください。

注: windows Vista 以降のバージョンの Windows オペレーティングシステムでは、ローカルコンピューターを **ComputerName** パラメーターの値に含めるには、[管理者として実行] オプションを使用して PowerShell を起動する必要があります。

```yaml
Type: System.String
Parameter Sets: ComputerName
Aliases: Cn

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -ConfigurationName

対話型セッションに使用するセッション構成を指定します。

セッション構成の構成名または完全修飾リソース URI を入力します。 構成名だけを指定した場合は、次のスキーマ URI が付加され `http://schemas.microsoft.com/powershell` ます。

SSH で使用する場合は、sshd_config で定義されているターゲットで使用するサブシステムを指定します。 SSH の既定値は `powershell` サブシステムです。

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

URI のトランスポート セグメントの有効な値は、HTTP および HTTPS です。 トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合は、HTTP の場合は80、HTTPS の場合は443という標準ポートを使用してセッションが作成されます。 PowerShell リモート処理に既定のポートを使用するには、HTTP の場合はポート5985、HTTPS の場合は5986を指定します。

対象のコンピューターが接続を別の URI にリダイレクトする場合、コマンドで **Allowredirection** パラメーターを使用しない限り、PowerShell によってリダイレクトが禁止されます。

```yaml
Type: System.Uri
Parameter Sets: Uri
Aliases: URI, CU

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ContainerId

コンテナーの ID を指定します。

```yaml
Type: System.String
Parameter Sets: ContainerId
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ComputerName, Uri, VMId, VMName
Aliases:

Required: True (VMId, VMName), False (ComputerName, Uri)
Position: 1
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -EnableNetworkAccess

このコマンドレットが、ループバックセッションに対話型セキュリティトークンを追加することを示します。 対話型トークンを使用すると、他のコンピューターからデータを取得するコマンドをループバック セッションで実行できます。 たとえば、リモート コンピューターからローカル コンピューターに XML ファイルをコピーするコマンドをセッションで実行できます。

ループバックセッションは、同じコンピューター上で発生して終了する **PSSession** です。 ループバックセッションを作成するには、 **ComputerName** パラメーターを省略するか、値をに設定します。 (ドット)、localhost、またはローカルコンピューターの名前。

既定では、ループバックセッションはネットワークトークンを使用して作成されますが、リモートコンピューターに対して認証を行うための十分なアクセス許可がない可能性があります。

**EnableNetworkAccess** パラメーターは、ループバック セッションでのみ有効です。 リモートコンピューターでセッションを作成するときに **EnableNetworkAccess** を使用すると、コマンドは成功しますが、パラメーターは無視されます。

**Authentication** パラメーターの **CredSSP** 値を使用して、ループバック セッションでリモート アクセスを許可することもできます。その場合、セッションの資格情報が他のコンピューターに委任されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName, Uri
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -HostName

Secure Shell (SSH) ベースの接続のコンピューター名を指定します。 これは **ComputerName** パラメーターと似ていますが、リモートコンピューターへの接続は Windows WinRM ではなく SSH を使用して行われる点が異なります。 このパラメーターでは、という形式を使用して、ホスト名パラメーター値の一部としてユーザー名またはポートを指定 `user@hostname:port` できます。 ホスト名の一部として指定されたユーザー名またはポートは、 `-UserName` 指定されている場合、パラメーターおよびパラメーターよりも優先され `-Port` ます。 これにより、複数のコンピューター名をこのパラメーターに渡すことができます。ここでは、特定のユーザー名やポートを持つものもあれば、パラメーターとパラメーターからユーザー名やポートを使用するものもあり `-UserName` `-Port` ます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: SSHHost
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Id

既存のセッションの ID を指定します。 `Enter-PSSession` 対話型セッションに対して指定されたセッションを使用します。

セッションの ID を検索するには、コマンドレットを使用し `Get-PSSession` ます。

```yaml
Type: System.Int32
Parameter Sets: Id
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InstanceId

既存のセッションのインスタンス ID を指定します。 `Enter-PSSession` 対話型セッションに対して指定されたセッションを使用します。

インスタンス ID は GUID です。 セッションのインスタンス ID を検索するには、コマンドレットを使用し `Get-PSSession` ます。 **Session**、 **Name**、または **ID** パラメーターを使用して、既存のセッションを指定することもできます。 または、 **ComputerName** パラメーターを使用して、一時的なセッションを開始することもできます。

```yaml
Type: System.Guid
Parameter Sets: InstanceId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
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

既存のセッションのフレンドリ名を指定します。 `Enter-PSSession` 対話型セッションに対して指定されたセッションを使用します。

指定した名前が複数のセッションと一致する場合、コマンドは失敗します。 **Session**、 **InstanceID**、または **ID** パラメーターを使用して、既存のセッションを指定することもできます。 または、 **ComputerName** パラメーターを使用して、一時的なセッションを開始することもできます。

セッションのフレンドリ名を設定するには、コマンドレットの **name** パラメーターを使用し `New-PSSession` ます。

```yaml
Type: System.String
Parameter Sets: Name
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Port

このコマンドに使用するリモートコンピューターのネットワークポートを指定します。

PowerShell 6.0 では、このパラメーターは、Secure Shell (SSH) 接続をサポートする **HostName** パラメーターセットに含まれていました。

**WinRM (ComputerName パラメーターセット)**

リモート コンピューターに接続するには、リモート コンピューターで、接続に使用されるポートをリッスンすることが必要です。 既定のポートは5985で、これは HTTP 用の WinRM ポート、および HTTPS 用の WinRM ポートである5986です。

代替ポートを使用する前に、そのポートでリッスンするようにリモート コンピューター上の WinRM リスナーを構成する必要があります。 リスナーを構成するには、次のコマンドを使用します。

1. `winrm delete winrm/config/listener?Address=*+Transport=HTTP`
2. `winrm create winrm/config/listener?Address=*+Transport=HTTP @{Port="\<port-number\>"}`

必要な場合を除き、**Port** パラメーターを使用しないでください。 コマンドのポート設定は、コマンドが実行されるすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。

**SSH (ホスト名パラメーターセット)**

リモートコンピューターに接続するには、リモートコンピューターが SSH サービス (SSHD) を使用して構成されており、接続が使用するポートでリッスンしている必要があります。 SSH の既定のポートは22です。

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

対話型セッションに使用する PowerShell セッション (**PSSession**) を指定します。 このパラメーターは、セッション オブジェクトを受け取ります。 また、 **Name**、 **InstanceID**、または **ID** パラメーターを使用して **PSSession** を指定することもできます。

セッションオブジェクトを含む変数、またはやコマンドなどのセッションオブジェクトを作成または取得するコマンドを入力し `New-PSSession` `Get-PSSession` ます。 パイプを使用してセッションオブジェクトをにパイプすることもでき `Enter-PSSession` ます。 このパラメーターを使用すると、 **PSSession** を1つだけ送信できます。 複数の **PSSession** を含む変数を入力すると、コマンドは失敗します。

`Exit-PSSession`または **EXIT** キーワードを使用すると、対話型セッションは終了しますが、作成した **PSSession** は開いたままになり、使用できるようになります。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: Session
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -SessionOption

セッションの詳細オプションを設定します。 コマンドレットを使用して作成する **sessionoption** オブジェクト、 `New-PSSessionOption` またはキーがセッションオプションの名前で、値がセッションオプションの値であるハッシュテーブルを入力します。

オプションの既定値は、設定されている場合は、ユーザー設定変数の値によって決まり `$PSSessionOption` ます。 それ以外の場合、既定値はセッション構成で設定されたオプションによって決まります。

セッションオプションの値は、ユーザー設定変数およびセッション構成で設定されたセッションの既定値よりも優先され `$PSSessionOption` ます。 ただし、セッション構成で設定された最大値、クォータ、または制限よりも優先されることはありません。

既定値を含む、セッションオプションの説明については、「」を参照してください `New-PSSessionOption` 。
ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

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

### -サブシステム

新しい **PSSession** に使用する SSH サブシステムを指定します。

これは sshd_config で定義されているターゲットで使用するサブシステムを指定します。 サブシステムは、定義済みのパラメーターを使用して特定のバージョンの PowerShell を起動します。 指定したサブシステムがリモートコンピューターに存在しない場合、コマンドは失敗します。

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

### -UserName

リモートコンピューターでセッションを作成するために使用するアカウントのユーザー名を指定します。 ユーザー認証方法は、リモートコンピューターで Secure Shell (SSH) がどのように構成されているかによって異なります。

SSH が基本パスワード認証用に構成されている場合は、ユーザーのパスワードの入力を求められます。

SSH がキーベースのユーザー認証用に構成されている場合、 **Keyfilepath** パラメーターを使用してキーファイルのパスを指定することができます。パスワードプロンプトは表示されません。 クライアントユーザーキーファイルが SSH の既知の場所にある場合、キーベースの認証に **Keyfilepath** パラメーターは必要ありません。ユーザー認証は、ユーザー名に基づいて自動的に実行されます。 詳細については、キーベースのユーザー認証に関する SSH ドキュメントを参照してください。

これは必須のパラメーターではありません。 **UserName** パラメーターが指定されていない場合は、現在のログオンユーザー名が接続に使用されます。

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

このコマンドレットが、リモートコンピューターへの接続を確立するために Secure Sockets Layer (SSL) プロトコルを使用することを示します。 既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で送信されるすべての PowerShell コンテンツを暗号化します。 **UseSSL** パラメーターは、HTTP 接続ではなく HTTPS 接続を介してデータを送信する追加の保護です。

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

仮想マシンの ID を指定します。

```yaml
Type: System.Guid
Parameter Sets: VMId
Aliases: VMGuid

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -VMName

仮想マシンの名前を指定します。

```yaml
Type: System.String
Parameter Sets: VMName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.string、システムの管理.... 実行空間

パイプを使用して、コンピューター名を文字列として、またはセッションオブジェクトをこのコマンドレットに渡します。

## 出力

### なし

このコマンドレットは出力を返しません。

## 注

リモート コンピューターに接続するには、リモート コンピューターの Administrators グループのメンバーであることが必要です。 ローカルコンピューターで対話型セッションを開始するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動する必要があります。

を使用すると `Enter-PSSession` 、リモートコンピューターのユーザープロファイルが対話型セッションに使用されます。 PowerShell モジュールを追加するコマンドやコマンドプロンプトを変更するコマンドなど、リモートユーザープロファイル内のコマンドは、リモートプロンプトが表示される前に実行されます。

`Enter-PSSession` ローカルコンピューターの UI カルチャ設定を対話型セッションに使用します。 ローカル UI カルチャを検索するには、 `$UICulture` 自動変数を使用します。

`Enter-PSSession` には `Get-Command` 、、 `Out-Default` 、およびの各コマンドレットが必要です `Exit-PSSession` 。 これらのコマンドレットがリモートコンピューターのセッション構成に含まれていない場合、 `Enter-PSSession` コマンドは失敗します。

とは異なり、 `Invoke-Command` コマンドをリモートコンピューターに送信する前に解析して解釈すると、は、 `Enter-PSSession` コマンドを解釈せずにリモートコンピューターに直接送信します。

入力するセッションがコマンドの処理中である場合は、PowerShell がコマンドに応答するまでに遅延が発生する可能性があり `Enter-PSSession` ます。 セッションが使用可能になるとすぐに接続されます。 コマンドを取り消すには `Enter-PSSession` 、 <kbd>ctrl</kbd> + <kbd>C</kbd>キーを押します。

**ホスト名** パラメーターセットは、PowerShell 6.0 以降に含まれていました。 これは、Secure Shell (SSH) に基づいて PowerShell リモート処理を提供するために追加されました。 複数のプラットフォーム (Windows、Linux、macOS) では SSH と PowerShell の両方がサポートされており、powershell リモート処理は PowerShell と SSH がインストールおよび構成されているこれらのプラットフォーム上で機能します。 これは、WinRM に基づく以前の Windows のみのリモート処理とは別のものであり、WinRM 固有の機能の多くは適用されません。 たとえば、WinRM ベースのクォータ、セッションオプション、カスタムエンドポイントの構成、および切断/再接続機能は現在サポートされていません。 PowerShell SSH リモート処理の設定方法の詳細については、「 [Ssh 経由の Powershell リモート](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)処理」を参照してください。

PowerShell 7.1 より前では、SSH 経由のリモート処理は、次ホップのリモート セッションをサポートしていませんでした。 この機能は、WinRM を使用したセッションに限定されていました。 PowerShell 7.1 を使用すると、任意の対話型リモート セッション内で `Enter-PSSession` と `Enter-PSHostProcess` が機能します。

## 関連リンク

[Exit-PSSession](Exit-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)

[Remove-PSSession](Remove-PSSession.md)

[Connect-PSSession](Connect-PSSession.md)

[Disconnect-PSSession](Disconnect-PSSession.md)

[Receive-PSSession](Receive-PSSession.md)

[about_PSSessions](About/about_PSSessions.md)

[about_Remote](About/about_Remote.md)
