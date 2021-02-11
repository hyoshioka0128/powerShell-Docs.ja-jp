---
description: PowerShell でのリモート操作のトラブルシューティング方法について説明します。
Locale: en-US
ms.date: 10/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_troubleshooting?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_Troubleshooting
ms.openlocfilehash: cedf38f3982f4036aae59a2019ab72b556e50cfd
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600969"
---
# <a name="about-remote-troubleshooting"></a>リモートトラブルシューティングについて

## <a name="short-description"></a>簡単な説明
PowerShell でのリモート操作のトラブルシューティング方法について説明します。

## <a name="long-description"></a>長い説明

このセクションでは、WS-Management テクノロジに基づく PowerShell のリモート処理機能を使用する場合に発生する可能性がある問題のいくつかについて説明します。また、これらの問題に対する解決策を提案します。

PowerShell リモート処理を使用する前に、構成と基本的な使用方法について [about_Remote](about_remote.md) と [about_Remote_Requirements](about_Remote_Requirements.md) を参照してください。 また、各リモート処理コマンドレットのヘルプトピック (特にパラメーターの説明) には、問題を回避するために設計された有用な情報が含まれています。

> [!NOTE]
> WSMan: ドライブ内のローカルコンピューターの設定を表示または変更するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

## <a name="troubleshooting-permission-and-authentication-issues"></a>アクセス許可と認証の問題のトラブルシューティング

ここでは、ユーザーおよびコンピューターのアクセス許可とリモート処理の要件に関連するリモート処理の問題について説明します。

### <a name="how-to-run-as-administrator"></a>管理者として実行する方法

```
ERROR: Access is denied. You need to run this cmdlet from an elevated
process.
```

ローカルコンピューターでリモートセッションを開始したり、WSMan: ドライブのローカルコンピューターの設定を表示または変更したりするには、[ **管理者として実行** ] オプションを使用して Windows PowerShell を起動します。

[ **管理者として実行** ] オプションを使用して Windows PowerShell を起動するには

- Windows PowerShell (または Windows PowerShell ISE) アイコンを右クリックし、[ **管理者として実行**] をクリックします。

  Windows 7 と Windows Server 2008 R2 で [ **管理者として実行** ] オプションを使用して windows PowerShell を起動するには

- Windows タスクバーで、Windows PowerShell アイコンを右クリックし、[ **管理者として実行**] をクリックします。

  Windows Server 2008 R2 では、Windows PowerShell アイコンが既定でタスクバーに固定されています。

### <a name="how-to-enable-remoting"></a>リモート処理を有効にする方法

```
ERROR:  ACCESS IS DENIED

or

ERROR: The connection to the remote host was refused. Verify that the
WS-Management service is running on the remote host and configured to
listen for requests on the correct port and HTTP URL.
```

コンピューターがリモートコマンドを送信できるようにするための構成は必要ありません。
ただし、リモートコマンドを受信するには、コンピューターで PowerShell リモート処理を有効にする必要があります。 を有効にするには、WinRM サービスの開始、WinRM サービスのスタートアップの種類の自動への設定、HTTP 接続と HTTPS 接続のためのリスナーの作成、既定のセッション構成の作成が含まれます。

Windows PowerShell リモート処理は、windows server 2012 以降の windows Server の既定のリリースで有効になっています。 他のすべてのシステムでは、コマンドレットを実行して `Enable-PSRemoting` リモート処理を有効にします。 `Enable-PSRemoting`リモート処理が無効になっている場合は、コマンドレットを実行して、windows server 2012 および Windows server の新しいリリースでリモート処理を再度有効にすることもできます。

リモートコマンドを受信するようにコンピューターを構成するには、コマンドレットを使用し `Enable-PSRemoting` ます。 次のコマンドは、必要なすべてのリモート設定を有効にし、セッション構成を有効にして、WinRM サービスを再起動して変更を有効にします。

`Enable-PSRemoting`

すべてのユーザープロンプトを非表示にするには、次のように入力します。

`Enable-PSRemoting -Force`

詳細については、「 [enable-psremoting](xref:Microsoft.PowerShell.Core.Enable-PSRemoting)」を参照してください。

### <a name="how-to-enable-remoting-in-an-enterprise"></a>企業でリモート処理を有効にする方法

```
ERROR:  ACCESS IS DENIED

or

ERROR: The connection to the remote host was refused. Verify that the
WS-Management service is running on the remote host and configured to listen
for requests on the correct port and HTTP URL.
```

1台のコンピューターでリモート PowerShell コマンドを受信して接続を受け入れるには、コマンドレットを使用し `Enable-PSRemoting` ます。

企業内の複数のコンピューターでリモート処理を有効にするには、次のスケーリングされたオプションを使用できます。

- リモート処理のリスナーを構成するには、[ **リスナーの自動構成を許可** する] グループポリシーを有効にします。

- 複数のコンピューターで Windows リモート管理 (WinRM) のスタートアップの種類を [自動] に設定するには、コマンドレットを使用し `Set-Service` ます。

- ファイアウォールの例外を有効にするには、[ **Windows ファイアウォール: ローカルポートの例外を許可** する] グループポリシーを使用します。

### <a name="how-to-enable-listeners-by-using-a-group-policy"></a>グループポリシーを使用してリスナーを有効にする方法

```
ERROR:  ACCESS IS DENIED

or

ERROR: The connection to the remote host was refused. Verify that the
WS-Management service is running on the remote host and configured to listen
for requests on the correct port and HTTP URL.
```

ドメイン内のすべてのコンピューターのリスナーを構成するには、次のグループポリシーパスで [ **リスナーの自動構成を許可** する] ポリシーを有効にします。

```
Computer Configuration\Administrative Templates\Windows Components
    \Windows Remote Management (WinRM)\WinRM service
```

ポリシーを有効にし、IPv4 フィルターと IPv6 フィルターを指定します。 ワイルドカード ( `*` ) を使用できます。

### <a name="how-to-enable-remoting-on-public-networks"></a>パブリックネットワークでリモート処理を有効にする方法

```
ERROR:  Unable to check the status of the firewall
```

`Enable-PSRemoting`コマンドレットは、ローカルネットワークがパブリックで、 **SkipNetworkProfileCheck** パラメーターがコマンドで使用されていない場合に、このエラーを返します。

Windows のサーバーバージョンでは、は `Enable-PSRemoting` すべてのネットワークの場所の種類に対して成功します。 プライベートおよびドメイン ("ホーム" および "作業") ネットワークへのリモートアクセスを許可するファイアウォール規則を作成します。 パブリックネットワークの場合、同じローカルサブネットからのリモートアクセスを許可するファイアウォール規則が作成されます。

Windows のクライアントバージョンでは、は `Enable-PSRemoting` プライベートネットワークとドメインネットワークで成功します。 既定では、パブリックネットワークでは失敗しますが、 **SkipNetworkProfileCheck** パラメーターを使用すると、は `Enable-PSRemoting` 成功し、同じローカルサブネットからのトラフィックを許可するファイアウォール規則を作成します。

パブリックネットワークのローカルサブネットの制限を削除し、任意の場所からのリモートアクセスを許可するには、次のコマンドを実行します。

```powershell
Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP-PUBLIC" -RemoteAddress Any
```

`Set-NetFirewallRule`コマンドレットは、NetSecurity モジュールによってエクスポートされます。

> [!NOTE]
> Windows PowerShell 2.0 では、サーバーバージョンの Windows を実行しているコンピューターで、 `Enable-PSRemoting` プライベート、ドメイン、およびパブリックネットワークでのリモートアクセスを許可するファイアウォール規則を作成します。 クライアントバージョンの Windows を実行しているコンピューターでは、は、 `Enable-PSRemoting` プライベートネットワークとドメインネットワーク上でのみリモートアクセスを許可するファイアウォール規則を作成します。

### <a name="how-to-enable-a-firewall-exception-by-using-a-group-policy"></a>グループポリシーを使用してファイアウォールの例外を有効にする方法

```
ERROR:  ACCESS IS DENIED

or

ERROR: The connection to the remote host was refused. Verify that the
WS-Management service is running on the remote host and configured to listen
for requests on the correct port and HTTP URL.
```

ドメイン内のすべてのコンピューターでのファイアウォールの例外を有効にするには、次のグループポリシーパスで [ **Windows ファイアウォール: ローカルポートの例外を許可** する] ポリシーを有効にします。

```
Computer Configuration\Administrative Templates\Network
    \Network Connections\Windows Firewall\Domain Profile
```

このポリシーにより、コンピューターの Administrators グループのメンバーは、**コントロールパネル** の **Windows ファイアウォール** を使用して、Windows リモート管理サービスのファイアウォールの例外を作成できます。

ポリシーの構成が正しくない場合は、次のエラーが表示されることがあります。

```
The client cannot connect to the destination specified in the request. Verify
that the service on the destination is running and is accepting requests.
```

ポリシーの構成エラーが発生すると、 **ListeningOn** プロパティの値が空になります。 次のコマンドを使用して、値を確認します。

```powershell
PS> Get-WSManInstance winrm/config/listener -Enumerate

cfg                   : http://schemas.microsoft.com/wbem/wsman/1/config/listener
xsi                   : http://www.w3.org/2001/XMLSchema-instance
Source                : GPO
lang                  : en-US
Address               : *
Transport             : HTTP
Port                  : 5985
Hostname              :
Enabled               : true
URLPrefix             : wsman
CertificateThumbprint :
ListeningOn           : {}
```

### <a name="how-to-set-the-startup-type-of-the-winrm-service"></a>WinRM サービスのスタートアップの種類を設定する方法

```
ERROR:  ACCESS IS DENIED
```

PowerShell リモート処理は、Windows リモート管理 (WinRM) サービスに依存します。
リモートコマンドをサポートするには、サービスが実行されている必要があります。

Windows のサーバーバージョンでは、Windows リモート管理 (WinRM) サービスのスタートアップの種類が [自動] になります。

ただし、クライアントバージョンの Windows では、WinRM サービスは既定で無効になっています。

リモートコンピューター上のサービスのスタートアップの種類を設定するには、 `Set-Service` コマンドレットを使用します。

複数のコンピューターでコマンドを実行するには、コンピューター名のテキストファイルまたは CSV ファイルを作成します。

たとえば、次のコマンドは、ファイルからコンピューター名の一覧を取得 `Servers.txt` し、すべてのコンピューター上の WinRM サービスのスタートアップの種類を **自動** に設定します。

```powershell
$servers = Get-Content servers.txt
Set-Service WinRM -ComputerName $servers -startuptype Automatic
```

結果を表示するには、 `Get-WMIObject` コマンドレットを **Win32_Service** オブジェクトと共に使用します。 詳細については、「 [Set-Service](xref:Microsoft.PowerShell.Management.Set-Service)」を参照してください。

### <a name="how-to-recreate-the-default-session-configurations"></a>既定のセッション構成を再作成する方法

```
ERROR:  ACCESS IS DENIED
```

ローカルコンピューターに接続してコマンドをリモートで実行するには、ローカルコンピューターにリモートコマンドのセッション構成が含まれている必要があります。

を使用すると `Enable-PSRemoting` 、ローカルコンピューター上に既定のセッション構成が作成されます。 リモートユーザーは、リモートコマンドに **ConfigurationName** パラメーターが含まれていない場合に、これらのセッション構成を使用します。

コンピューターの既定の構成が登録解除または削除されている場合は、コマンドレットを使用して `Enable-PSRemoting` 再作成します。 このコマンドレットを繰り返し使用できます。 機能が既に構成されている場合、エラーは生成されません。

既定のセッション構成を変更し、元の既定のセッション構成を復元する場合は、コマンドレットを使用して `Unregister-PSSessionConfiguration` 変更されたセッション構成を削除してから、コマンドレットを使用して `Enable-PSRemoting` 復元します。
`Enable-PSRemoting` では、既存のセッション構成は変更されません。

> [!NOTE]
> は、既定のセッション構成を復元するときに、 `Enable-PSRemoting` 構成に対して明示的なセキュリティ記述子を作成しません。 代わりに、構成は RootSDDL のセキュリティ記述子を継承します。これは既定ではセキュリティで保護されています。

RootSDDL セキュリティ記述子を表示するには、次のように入力します。

```powershell
Get-Item wsman:\localhost\Service\RootSDDL
```

RootSDDL を変更するには、 `Set-Item` WSMan: ドライブのコマンドレットを使用します。 セッション構成のセキュリティ記述子を変更するには、 `Set-PSSessionConfiguration` コマンドレットを **Securitydescriptor sddl** または **showsecuritydescriptor ui** パラメーターと共に使用します。

WSMan: ドライブの詳細については、WSMan プロバイダーのヘルプトピック ("Get-help WSMan") を参照してください。

### <a name="how-to-provide-administrator-credentials"></a>管理者の資格情報を指定する方法

```
ERROR:  ACCESS IS DENIED
```

リモートコンピューターで PSSession を作成したりコマンドを実行したりするには、既定では、現在のユーザーがリモートコンピューターの Administrators グループのメンバーである必要があります。 現在のユーザーが Administrators グループのメンバーであるアカウントにログオンしている場合でも、資格情報が必要になることがあります。

現在のユーザーがリモートコンピューターの Administrators グループのメンバーである場合、または Administrators グループのメンバーの資格情報を提供できる場合は、、、またはコマンドレットの **Credential** パラメーターを使用して `New-PSSession` 、 `Enter-PSSession` `Invoke-Command` リモートで接続します。

たとえば、次のコマンドは、管理者の資格情報を提供します。

```powershell
Invoke-Command -ComputerName Server01 -Credential Domain01\Admin01
```

**Credential** パラメーターの詳細については、「 [New-pssession](xref:Microsoft.PowerShell.Core.New-PSSession)」、「 [-pssession](xref:Microsoft.PowerShell.Core.Enter-PSSession) 」、または「 [Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)」を参照してください。

### <a name="how-to-enable-remoting-for-non-administrative-users"></a>管理者以外のユーザーのリモート処理を有効にする方法

```
ERROR:  ACCESS IS DENIED
```

リモートコンピューター上で PSSession を確立したりコマンドを実行したりするには、リモートコンピューターでセッション構成を使用するためのアクセス許可が必要です。

既定では、コンピューターの Administrators グループのメンバーだけが、既定のセッション構成を使用するアクセス許可を持っています。 したがって、Administrators グループのメンバーだけがリモートでコンピューターに接続できます。

他のユーザーがローカルコンピューターに接続できるようにするには、ローカルコンピューター上の既定のセッション構成に対する実行アクセス許可をユーザーに付与します。

次のコマンドを実行すると、プロパティシートが開き、ローカルコンピューター上の既定の Microsoft PowerShell セッション構成のセキュリティ記述子を変更できます。

```powershell
Set-PSSessionConfiguration Microsoft.PowerShell -ShowSecurityDescriptorUI
```

詳細については、「 [about_Session_Configurations](about_Session_Configurations.md)」を参照してください。

### <a name="how-to-enable-remoting-for-administrators-in-other-domains"></a>他のドメインの管理者のリモート処理を有効にする方法

```
ERROR:  ACCESS IS DENIED
```

別のドメインのユーザーがローカルコンピューターの Administrators グループのメンバーである場合、ユーザーは管理者特権でローカルコンピューターにリモート接続することはできません。 既定では、他のドメインからのリモート接続は、標準のユーザー特権トークンのみを使用して実行されます。

ただし、 **LocalAccountTokenFilterPolicy** レジストリエントリを使用して既定の動作を変更し、Administrators グループのメンバーであるリモートユーザーが管理者特権で実行できるようにすることができます。

> [!CAUTION]
> **LocalAccountTokenFilterPolicy** エントリは、影響を受けるすべてのコンピューターのすべてのユーザーに対するユーザーアカウント制御 (UAC) リモート制限を無効にします。 ポリシーを変更する前に、この設定の影響を慎重に検討してください。

ポリシーを変更するには、次のコマンドを使用して、 **LocalAccountTokenFilterPolicy** レジストリエントリの値を1に設定します。

```powershell
New-ItemProperty -Name LocalAccountTokenFilterPolicy `
  -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System `
  -PropertyType DWord -Value 1
```

### <a name="how-to-use-an-ip-address-in-a-remote-command"></a>リモートコマンドで ip アドレスを使用する方法

```
ERROR: The WinRM client cannot process the request. If the authentication
scheme is different from Kerberos, or if the client computer is not joined to a
domain, then HTTPS transport must be used or the destination machine must be
added to the TrustedHosts configuration setting.
```

、、およびコマンドレットの **ComputerName** パラメーターは、 `New-PSSession` `Enter-PSSession` `Invoke-Command` IP アドレスを有効な値として受け入れます。 ただし、Kerberos 認証では IP アドレスがサポートされないため、IP アドレスを指定するたびに、NTLM 認証が既定で使用されます。

NTLM 認証を使用する場合、リモート処理には次の手順が必要です。

1. HTTPS トランスポート用にコンピューターを構成するか、ローカルコンピューターの TrustedHosts の一覧にリモートコンピューターの IP アドレスを追加します。

1. すべてのリモートコマンドで **Credential** パラメーターを使用します。

   これは、現在のユーザーの資格情報を送信する場合でも必要です。

### <a name="how-to-connect-remotely-from-a-workgroup-based-computer"></a>ワークグループベースのコンピューターからリモート接続する方法

```
ERROR: The WinRM client cannot process the request. If the authentication
scheme is different from Kerberos, or if the client computer is not joined to a
domain, then HTTPS transport must be used or the destination machine must be
added to the TrustedHosts configuration setting.
```

ローカルコンピューターがドメイン内にない場合、リモート処理には次の手順が必要です。

1. HTTPS トランスポート用にコンピューターを構成するか、ローカルコンピューターの TrustedHosts の一覧にリモートコンピューターの名前を追加します。

1. ワークグループベースのコンピューターにパスワードが設定されていることを確認します。 パスワードが設定されていない場合、またはパスワード値が空の場合は、リモートコマンドを実行できません。

   ユーザーアカウントのパスワードを設定するには、コントロールパネルの [ユーザーアカウント] を使用します。

1. すべてのリモートコマンドで **Credential** パラメーターを使用します。

   これは、現在のユーザーの資格情報を送信する場合でも必要です。

### <a name="how-to-add-a-computer-to-the-trusted-hosts-list"></a>信頼されたホストの一覧にコンピューターを追加する方法

TrustedHosts 項目には、コンピューター名、IP アドレス、および完全修飾ドメイン名のコンマ区切りの一覧を含めることができます。 ワイルドカードを使用できます。

信頼されたホストの一覧を表示または変更するには、WSMan: ドライブを使用します。 TrustedHost 項目は `WSMan:\localhost\Client` ノードにあります。

コンピューターの Administrators グループのメンバーだけが、コンピューター上の信頼されたホストの一覧を変更するアクセス許可を持っています。

注意: TrustedHosts 項目に対して設定した値は、コンピューターのすべてのユーザーに影響します。

信頼されたホストの一覧を表示するには、次のコマンドを使用します。

```powershell
Get-Item wsman:\localhost\Client\TrustedHosts
```

また、 `Set-Location` コマンドレット (alias = cd) を使用して、WSMan: ドライブを場所に移動することもできます。 次に例を示します。

```powershell
cd WSMan:\localhost\Client; dir
```

すべてのコンピューターを信頼されたホストの一覧に追加するには、次のコマンドを使用します。このコマンドは、ComputerName に * (all) の値を指定します。

```powershell
Set-Item wsman:localhost\client\trustedhosts -Value *
```

ワイルドカード文字 () を使用し `*` て、特定のドメインにあるすべてのコンピューターを、信頼されたホストの一覧に追加することもできます。 たとえば、次のコマンドは、Fabrikam ドメインにあるすべてのコンピューターを、信頼されたホストの一覧に追加します。

```powershell
Set-Item wsman:localhost\client\trustedhosts *.fabrikam.com
```

特定のコンピューターの名前を信頼されたホストの一覧に追加するには、次のコマンド形式を使用します。

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -Value <ComputerName>
```

各値 `<ComputerName>` の形式は次のとおりでなければなりません。

```
<Computer>.<Domain>.<Company>.<top-level-domain>
```

次に例を示します。

```powershell
$server = 'Server01.Domain01.Fabrikam.com'
Set-Item wsman:\localhost\Client\TrustedHosts -Value $server
```

既存の信頼されたホストの一覧にコンピューター名を追加するには、まず、現在の値を変数に保存し、その値を、現在の値と新しい値を含むコンマ区切りのリストに設定します。

たとえば、Server01 コンピューターを既存の信頼されたホストの一覧に追加するには、次のコマンドを使用します。

```powershell
$curValue = (Get-Item wsman:\localhost\Client\TrustedHosts).value

Set-Item wsman:\localhost\Client\TrustedHosts -Value `
  "$curValue, Server01.Domain01.Fabrikam.com"
```

特定のコンピューターの IP アドレスを信頼されたホストの一覧に追加するには、次のコマンド形式を使用します。

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -Value <IP Address>
```

次に例を示します。

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -Value 172.16.0.0
```

コンピューターをリモートコンピューターの TrustedHosts の一覧に追加するには、コマンドレットを使用して、 `Connect-WSMan` リモートコンピューターのノードをローカルコンピューターの WSMan: ドライブに追加します。 次に、コマンドを使用し `Set-Item` てコンピューターを追加します。

コマンドレットの詳細につい `Connect-WSMan` ては、「 [接続-WSMan](xref:Microsoft.WSMan.Management.Connect-WSMan)」を参照してください。

## <a name="troubleshooting-computer-configuration-issues"></a>コンピューターの構成に関する問題のトラブルシューティング

ここでは、コンピューター、ドメイン、または企業の特定の構成に関連するリモート処理の問題について説明します。

### <a name="how-to-configure-remoting-on-alternate-ports"></a>代替ポートでリモート処理を構成する方法

```
ERROR: The connection to the specified remote host was refused. Verify that the
WS-Management service is running on the remote host and configured to listen
for requests on the correct port and HTTP URL.
```

PowerShell リモート処理では、既定で HTTP トランスポートにポート80を使用します。 既定のポートは、ユーザーがリモートコマンドで **Connectionuri** または **ポート** パラメーターを指定していない場合に常に使用されます。

PowerShell が使用する既定のポートを変更するには、WSMan: ドライブのコマンドレットを使用して、 `Set-Item` リスナーのリーフノードのポート値を変更します。

たとえば、次のコマンドは、既定のポートを8080に変更します。

```powershell
Set-Item wsman:\localhost\listener\listener*\port -Value 8080
```

### <a name="how-to-configure-remoting-with-a-proxy-server"></a>プロキシサーバーを使用してリモート処理を構成する方法

```
ERROR: The client cannot connect to the destination specified in the request.
Verify that the service on the destination is running and is accepting
requests.
```

PowerShell リモート処理は HTTP プロトコルを使用するため、HTTP プロキシ設定の影響を受けます。 プロキシサーバーを使用している企業では、ユーザーは PowerShell リモートコンピューターに直接アクセスすることはできません。

この問題を解決するには、リモートコマンドでプロキシ設定オプションを使用します。 次の設定を使用できます。

- System.management.automation.remoting.proxyaccesstype
- ProxyAuthentication
- ProxyCredential

特定のコマンドに対してこれらのオプションを設定するには、次の手順に従います。

1. コマンドレットの **system.management.automation.remoting.proxyaccesstype**、 **Proxyauthentication**、および **proxyauthentication** パラメーターを使用して、 `New-PSSessionOption` 企業のプロキシ設定を使用してセッションオプションオブジェクトを作成します。 [保存] オプションオブジェクトは変数です。

1. 、、またはコマンドの **Sessionoption** パラメーターの値として、option オブジェクトを含む変数を使用し `New-PSSession` `Enter-PSSession` `Invoke-Command` ます。

たとえば、次のコマンドは、プロキシセッションオプションを使用してセッションオプションオブジェクトを作成し、オブジェクトを使用してリモートセッションを作成します。

```powershell
$SessionOption = New-PSSessionOption -ProxyAccessType IEConfig `
-ProxyAuthentication Negotiate -ProxyCredential Domain01\User01

New-PSSession -ConnectionURI https://www.fabrikam.com
```

コマンドレットの詳細につい `New-PSSessionOption` ては、「 [New-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)」を参照してください。

現在のセッションのすべてのリモートコマンドに対してこれらのオプションを設定するには、ユーザー `New-PSSessionOption` 設定変数の値にを作成する option オブジェクトを使用し `$PSSessionOption` ます。 詳細については、「 [about_Preference_Variables](about_Preference_Variables.md)」を参照してください。

すべてのリモートコマンドに対して、ローカルコンピューター上のすべての PowerShell セッションに対してこれらのオプションを設定するには、 `$PSSessionOption` ユーザー設定変数を powershell プロファイルに追加します。 PowerShell プロファイルの詳細については、「 [about_Profiles](about_Profiles.md)」を参照してください。

### <a name="how-to-detect-a-32-bit-session-on-a-64-bit-computer"></a>64ビットコンピューターで32ビットセッションを検出する方法

```
ERROR: The term "<tool-Name>" is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
```

リモートコンピューターが64ビットバージョンの Windows を実行していて、リモートコマンドが Microsoft.powershell32 などの32ビットのセッション構成を使用している場合、Windows リモート管理 (WinRM) は WOW64 プロセスを読み込み、Windows はディレクトリへのすべての参照をディレクトリに自動的にリダイレクトし `$env:Windir\System32` `$env:Windir\SysWOW64` ます。

このため、のように、SysWow64 ディレクトリ (など) に対応するものがない System32 ディレクトリ内のツールを使用しようとすると、 `Defrag.exe` ツールがディレクトリに見つかりません。

セッションで使用されているプロセッサアーキテクチャを確認するには、 **PROCESSOR_ARCHITECTURE** 環境変数の値を使用します。 次のコマンドは、変数内のセッションのプロセッサアーキテクチャを検索し `$s` ます。

```powershell
$s = New-PSSession -ComputerName Server01 -ConfigurationName CustomShell
Invoke-Command -Session $s {$env:PROCESSOR_ARCHITECTURE}
```

```Output
x86
```

セッション構成の詳細については、「 [about_Session_Configurations](about_Session_Configurations.md)」を参照してください。

## <a name="troubleshooting-policy-and-preference-issues"></a>ポリシーと基本設定に関する問題のトラブルシューティング

ここでは、ローカルコンピューターとリモートコンピューターで設定されたポリシーと基本設定に関連するリモート処理の問題について説明します。

### <a name="how-to-change-the-execution-policy-for-import-pssession-and-import-module"></a>Import-PSSession および Import-Module の実行ポリシーを変更する方法

```
ERROR: Import-Module: File <filename> cannot be loaded because the
execution of scripts is disabled on this system.
```

`Import-PSSession`および `Export-PSSession` コマンドレットは、署名されていないスクリプトファイルとフォーマットファイルを含むモジュールを作成します。

またはを使用してこれらのコマンドレットによって作成されたモジュールをインポートするには、 `Import-PSSession` `Import-Module` 現在のセッションの実行ポリシーを制限することも AllSigned することもできません。 PowerShell 実行ポリシーの詳細については、「 [about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

レジストリに設定されているローカルコンピューターの実行ポリシーを変更せずにモジュールをインポートするには、の **Scope** パラメーターを使用して、 `Set-ExecutionPolicy` 1 つのプロセスに対して制限の少ない実行ポリシーを設定します。

たとえば、次のコマンドは、実行ポリシーを使用してプロセスを開始し `RemoteSigned` ます。 実行ポリシーの変更は現在のプロセスのみに影響し、PowerShell **set-executionpolicy** レジストリ設定は変更されません。

```powershell
Set-ExecutionPolicy -Scope process -ExecutionPolicy RemoteSigned
```

の **set-executionpolicy** パラメーターを使用して、 `PowerShell.exe` より制限の少ない実行ポリシーで1つのセッションを開始することもできます。

```powershell
PowerShell.exe -ExecutionPolicy RemoteSigned
```

実行ポリシーの詳細については、「 [about_Execution_Policies](about_Execution_Policies.md)」を参照してください。 詳細を表示するには「`PowerShell.exe -?`」を入力します。

### <a name="how-to-set-and-change-quotas"></a>クォータを設定および変更する方法

```
ERROR: The total data received from the remote client exceeded allowed
maximum.
```

クォータを使用すると、ローカルコンピューターとリモートコンピューターが、偶発的でも悪意のあるリソースでも使用されないように保護することができます。

基本構成では、次のクォータを使用できます。

- WSMan プロバイダー (WSMan:)では、ノードの **MaxEnvelopeSizeKB** および **maxproviderrequests** 設定、 `WSMan:<ComputerName>` ノードの **MaxConcurrentOperations**、 **maxconcurrentoperationsperuser プロパティ**、 **MaxConnections** の各設定など、いくつかのクォータ設定が提供され `WSMan:<ComputerName>\Service` ます。

- コマンドレットとユーザー設定変数の **MaximumReceivedDataSizePerCommand** パラメーターと **MaximumReceivedObjectSize** パラメーターを使用して、ローカルコンピューターを保護することができ `New-PSSessionOption` `$PSSessionOption` ます。

- コマンドレットの **MaximumReceivedDataSizePerCommandMB** パラメーターと **MaximumReceivedObjectSizeMB** パラメーターを使用するなどして、セッション構成に制限を追加することで、リモートコンピューターを保護することができ `Register-PSSessionConfiguration` ます。

クォータがコマンドと競合している場合は、PowerShell によってエラーが生成されます。

このエラーを解決するには、クォータに準拠するようにリモートコマンドを変更します。 または、クォータのソースを特定し、クォータを増やしてコマンドを完了できるようにします。

たとえば、次のコマンドは、リモートコンピューター上の Microsoft PowerShell セッション構成のオブジェクトサイズクォータを 10 MB (既定値) から 11 MB に増やします。

```powershell
Set-PSSessionConfiguration -Name microsoft.PowerShell `
  -MaximumReceivedObjectSizeMB 11 -Force
```

コマンドレットの詳細については `New-PSSessionOption` 、「」を参照してください `New-PSSessionOption` 。

WS-Management クォータの詳細については、「 [about_WSMan_Provider](../../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)」を参照してください。

### <a name="how-to-resolve-timeout-errors"></a>タイムアウトエラーを解決する方法

```
ERROR: The WS-Management service cannot complete the operation within
the time specified in OperationTimeout.
```

タイムアウトを使用して、ローカルコンピューターとリモートコンピューターが、偶発的でも悪意のあるリソースでも使用されないように保護することができます。 ローカルコンピューターとリモートコンピューターの両方でタイムアウトが設定されている場合、PowerShell では最短タイムアウト設定が使用されます。

基本構成では、次のタイムアウトを使用できます。

- WSMan プロバイダー (WSMan:)では、ノードの **Maxtimeoutms** 設定 `WSMan:<ComputerName>` やノードの **EnumerationTimeoutms** と **MaxPacketRetrievalTimeSeconds** の設定など、クライアント側とサービス側のタイムアウト設定がいくつか提供されて `WSMan:<ComputerName>\Service` います。

- コマンドレットとユーザー設定変数の **canceltimeout**、 **IdleTimeout**、 **OpenTimeout**、および **operationtimeout** パラメーターを使用して、ローカルコンピューターを保護することができ `New-PSSessionOption` `$PSSessionOption` ます。

- セッションのセッション構成でタイムアウト値をプログラムによって設定することで、リモートコンピューターを保護することもできます。

タイムアウト値によって操作の完了が許可されていない場合、PowerShell は操作を終了し、エラーを生成します。

このエラーを解決するには、タイムアウト間隔内に完了するようにコマンドを変更するか、タイムアウト制限のソースを特定し、タイムアウト間隔を長くしてコマンドを完了できるようにします。

たとえば、次のコマンドでは、コマンドレットを使用して、 `New-PSSessionOption` **operationtimeout** 値が4分 (ミリ秒) のセッションオプションオブジェクトを作成し、そのセッションオプションオブジェクトを使用してリモートセッションを作成します。

```powershell
$pso = New-PSSessionoption -OperationTimeout 240000

New-PSSession -ComputerName Server01 -sessionOption $pso
```

WS-Management タイムアウトの詳細については、WSMan プロバイダーのヘルプトピック (型) を参照してください `Get-Help WSMan` 。

コマンドレットの詳細につい `New-PSSessionOption` ては、「 [New-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)」を参照してください。

## <a name="troubleshooting-unresponsive-behavior"></a>応答しない動作のトラブルシューティング

このセクションでは、コマンドが完了しないようにするリモート処理の問題について説明し、PowerShell プロンプトを返します。

### <a name="how-to-interrupt-a-command"></a>コマンドを中断する方法

ユーザーインターフェイスを備えたプログラム、入力を要求するコンソールアプリケーション、Win32 コンソール API を使用するコンソールアプリケーションなど、一部のネイティブ Windows プログラムは、PowerShell リモートホストでは正しく機能しません。

これらのプログラムを使用すると、出力がない、部分的な出力、または完了していないリモートコマンドなど、予期しない動作が発生する可能性があります。

応答しないプログラムを終了するには、 <kbd>CTRL C キーを押し</kbd> + <kbd></kbd>ます。 報告された可能性があるエラーを表示するには、 `$error` ローカルホストとリモートセッションを入力します。

## <a name="how-to-recover-from-an-operation-failure"></a>操作エラーから回復する方法

```
ERROR: The I/O operation has been aborted because of either a thread exit
or an  application request.
```

このエラーは、操作が完了する前に操作が終了したときに返されます。
通常、このエラーは、他の WinRM 操作の実行中に WinRM サービスが停止または再起動したときに発生します。

この問題を解決するには、WinRM サービスが実行されていることを確認してから、コマンドを再試行してください。

1. [ **管理者として実行** ] オプションを使用して PowerShell を起動します。
1. 次のコマンドを実行します。

   `Start-Service WinRM`

1. エラーを生成したコマンドを再実行します。

## <a name="linux-and-macos-limitations"></a>Linux と macOS の制限事項

### <a name="authentication"></a>認証

MacOS では基本認証のみが機能し、他の認証方式を使用しようとするとプロセスがクラッシュする可能性があります。

[Omi 認証](https://github.com/PowerShell/psl-omi-provider#connecting-from-linux-to-windows)の手順を参照してください。

## <a name="see-also"></a>関連項目

[Import-PSSession](xref:Microsoft.PowerShell.Utility.Import-PSSession)

[Export-PSSession](xref:Microsoft.PowerShell.Utility.Export-PSSession)

[Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)

[about_Remote](about_Remote.md)

[about_Remote_Requirements](about_Remote_Requirements.md)

[about_Remote_Variables](about_Remote_Variables.md)
