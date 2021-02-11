---
description: PowerShell でリモートコマンドを実行するためのシステム要件と構成要件について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_Requirements
ms.openlocfilehash: 4a994ded51195e5932af7bb4af0b2e6fb3a67857
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602749"
---
# <a name="about-remote-requirements"></a>リモートの要件について

## <a name="short-description"></a>概要
PowerShell でリモートコマンドを実行するためのシステム要件と構成要件について説明します。

## <a name="long-description"></a>詳細説明

このトピックでは、PowerShell でリモート接続を確立し、リモートコマンドを実行するためのシステム要件、ユーザー要件、およびリソース要件について説明します。 また、リモート操作を構成する手順についても説明します。

注: 多くのコマンドレット (Get Service、Get-wmiobject、、Get EventLog、および Get-WinEvent コマンドレットを含む) は、オブジェクトを取得するために Microsoft .NET Framework メソッドを使用してリモートコンピューターからオブジェクトを取得します。 PowerShell リモート処理インフラストラクチャは使用しません。 このドキュメントの要件は、これらのコマンドレットには適用されません。

ComputerName パラメーターを持ち、PowerShell リモート処理を使用しないコマンドレットを見つけるには、コマンドレットの ComputerName パラメーターの説明を参照してください。

## <a name="system-requirements"></a>システム要件

Windows PowerShell 3.0 でリモートセッションを実行するには、ローカルコンピューターとリモートコンピューターに次のものが必要です。

- Windows PowerShell 3.0 以降
- Microsoft .NET Framework 4 以降
- Windows リモート管理3.0

Windows PowerShell 2.0 でリモートセッションを実行するには、ローカルコンピューターとリモートコンピューターに次のものが必要です。

- Windows PowerShell 2.0 以降
- Microsoft .NET Framework 2.0 以降
- Windows リモート管理2.0

Windows PowerShell 2.0 と Windows PowerShell 3.0 を実行しているコンピューター間にリモートセッションを作成することができます。 ただし、セッションを切断して再接続する機能など、Windows PowerShell 3.0 でのみ実行される機能は、両方のコンピューターで Windows PowerShell 3.0 を実行している場合にのみ使用できます。

インストールされている PowerShell のバージョン番号を確認するには、$PSVersionTable 自動変数を使用します。

Windows リモート管理 (WinRM) 3.0 および Microsoft .NET Framework 4 は、windows 8、Windows Server 2012、および Windows オペレーティングシステムの新しいリリースに含まれています。 WinRM 3.0 は、以前のオペレーティングシステム用の Windows Management Framework 3.0 に含まれています。 コンピューターに必要なバージョンの WinRM または Microsoft .NET Framework がインストールされていない場合、インストールは失敗します。

## <a name="user-permissions"></a>ユーザーのアクセス許可

リモートセッションを作成してリモートコマンドを実行するには、既定で、現在のユーザーがリモートコンピューターの Administrators グループのメンバーであるか、管理者の資格情報を提供している必要があります。 それ以外の場合、コマンドは失敗します。

リモートコンピューター (またはローカルコンピューター上のリモートセッション) でセッションを作成し、コマンドを実行するために必要なアクセス許可は、セッションの接続先となるリモートコンピューター上のセッション構成 ("エンドポイント" とも呼ばれます) によって確立されます。 具体的には、セッション構成のセキュリティ記述子は、セッション構成にアクセスできるユーザーと、それを使用して接続するユーザーを決定します。

既定のセッション構成のセキュリティ記述子、Microsoft.powershell32、および Microsoft. PowerShell は、Administrators グループのメンバーのみにアクセスを許可します。

現在のユーザーがセッション構成を使用するアクセス許可を持っていない場合は、コマンドを実行して (一時セッションを使用)、またはリモートコンピューターで永続的なセッションを作成するコマンドは失敗します。 ユーザーは、セッションを作成するコマンドレットの ConfigurationName パラメーターを使用して、別のセッション構成 (使用可能な場合) を選択できます。

コンピューターの Administrators グループのメンバーは、既定のセッション構成のセキュリティ記述子を変更し、異なるセキュリティ記述子を使用して新しいセッション構成を作成することによって、コンピューターにリモート接続するアクセス許可を持つユーザーを判断できます。

セッション構成の詳細については、「 [about_Session_Configurations](about_Session_Configurations.md)」を参照してください。

## <a name="windows-network-locations"></a>WINDOWS ネットワークの場所

Windows PowerShell 3.0 以降では、Enable-PSRemoting コマンドレットを使用して、プライベート、ドメイン、およびパブリックネットワーク上の Windows のクライアントおよびサーバーバージョンでリモート処理を有効にすることができます。

プライベートネットワークとドメインネットワークを使用する Windows のサーバーバージョンでは、Enable-PSRemoting コマンドレットにより、無制限のリモートアクセスを許可するファイアウォール規則が作成されます。 また、同じローカルサブネット内のコンピューターからのリモートアクセスのみを許可する、パブリックネットワーク用のファイアウォール規則も作成します。 このローカルサブネットファイアウォール規則は、パブリックネットワーク上の Windows のサーバーバージョンでは既定で有効になっていますが、変更または削除された場合には、規則が再適用 Enable-PSRemoting ます。

プライベートネットワークとドメインネットワークを使用する Windows のクライアントバージョンでは、Enable-PSRemoting コマンドレットによって、無制限のリモートアクセスを許可するファイアウォール規則が既定で作成されます。

パブリックネットワークを使用してクライアントバージョンの Windows でリモート処理を有効にするには、Enable-PSRemoting コマンドレットの SkipNetworkProfileCheck パラメーターを使用します。 同じローカルサブネット内のコンピューターからのリモートアクセスのみを許可するファイアウォール規則を作成します。

パブリックネットワークのローカルサブネットの制限を削除し、クライアントバージョンとサーバーバージョンの Windows 上のすべての場所からのリモートアクセスを許可するには、NetSecurity モジュールの Set-NetFirewallRule コマンドレットを使用します。 次のコマンドを実行します。

```powershell
Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP-PUBLIC" -RemoteAddress Any
```

Windows PowerShell 2.0 では、サーバーバージョンの Windows では、Enable-PSRemoting によって、すべてのネットワークでのリモートアクセスを許可するファイアウォール規則が作成されます。

Windows PowerShell 2.0 では、クライアントバージョンの Windows では、Enable-PSRemoting は、プライベートネットワークとドメインネットワークにのみファイアウォール規則を作成します。 ネットワークの場所がパブリックの場合、Enable-PSRemoting は失敗します。

## <a name="run-as-administrator"></a>管理者として実行

次のリモート処理操作には、管理者特権が必要です。

- ローカルコンピューターへのリモート接続を確立する。 これは、一般的に "ループバック" シナリオと呼ばれます。

- ローカルコンピューター上のセッション構成を管理する。

- ローカルコンピューター上の WS-Management 設定を表示および変更します。 WSMAN: ドライブの LocalHost ノードの設定を次に示します。

これらのタスクを実行するには、ローカルコンピューターの Administrators グループのメンバーであっても、[管理者として実行] オプションを使用して PowerShell を起動する必要があります。

Windows 7 と Windows Server 2008 R2 で、[管理者として実行] オプションを使用して Windows PowerShell を起動するには、次のようにします。

1. [スタート]、[すべてのプログラム]、[アクセサリ] の順にクリックし、[Windows PowerShell] フォルダーをクリックします。
2. [Windows PowerShell] を右クリックし、[管理者として実行] をクリックします。

[管理者として実行] オプションを使用して Windows PowerShell を起動するには

1. [スタート] をクリックし、[すべてのプログラム] をクリックして、[Windows PowerShell] フォルダーをクリックします。
2. [Windows PowerShell] を右クリックし、[管理者として実行] をクリックします。

[管理者として実行] オプションは、Windows PowerShell の他のエクスプローラーエントリ (ショートカットを含む) でも使用できます。 項目を右クリックし、[管理者として実行] をクリックします。

Cmd.exe などの別のプログラムから Windows PowerShell を起動する場合は、[管理者として実行] オプションを使用してプログラムを起動します。

## <a name="how-to-configure-your-computer-for-remoting"></a>リモート処理用にコンピューターを構成する方法

サポートされているすべてのバージョンの Windows を実行しているコンピューターは、PowerShell でリモート接続を確立し、構成なしでリモートコマンドを実行できます。 ただし、接続を受信し、ローカルおよびリモートのユーザー管理 PowerShell セッション ("PSSessions") をユーザーが作成して、ローカルコンピューターでコマンドを実行できるようにするには、コンピューターで PowerShell リモート処理を有効にする必要があります。

Windows server 2012 および Windows Server の新しいリリースでは、PowerShell リモート処理が既定で有効になっています。 設定が変更された場合は、Enable-PSRemoting コマンドレットを実行して既定の設定を復元できます。

その他のサポートされているすべてのバージョンの Windows では、Enable-PSRemoting コマンドレットを実行して PowerShell リモート処理を有効にする必要があります。

PowerShell のリモート処理機能は、WinRM サービスによってサポートされています。これは、Web Services for Management (WS-MANAGEMENT) プロトコルの Microsoft による実装です。 PowerShell リモート処理を有効にした場合、WS-Management の既定の構成を変更し、ユーザーが WS-MANAGEMENT に接続できるようにするシステム構成を追加します。

リモートコマンドを受信するように PowerShell を構成するには

1. [管理者として実行] オプションを使用して PowerShell を起動します。
2. コマンド プロンプトに `Enable-PSRemoting` を入力します。

リモート処理が正しく構成されていることを確認するには、次のコマンドのようなテストコマンドを実行します。このコマンドは、ローカルコンピューターでリモートセッションを作成します。

```powershell
New-PSSession
```

リモート処理が正しく構成されている場合、コマンドはローカルコンピューター上にセッションを作成し、そのセッションを表すオブジェクトを返します。 出力は次のサンプル出力のようになります。

```output
Id Name        ComputerName    State    ConfigurationName
-- ----        ------------    -----    -----
1  Session1    localhost       Opened   Microsoft.PowerShell
```

コマンドが失敗した場合は、「 [about_Remote_Troubleshooting](about_Remote_Troubleshooting.md)」を参照してください。

## <a name="understand-policies"></a>ポリシーを理解する

リモートで作業する場合は、PowerShell の2つのインスタンスを使用します。1つはローカルコンピューター上に、もう1つはリモートコンピューター上にあります。 その結果、ローカルコンピューターとリモートコンピューターの Windows ポリシーと PowerShell ポリシーの影響を受けます。

一般に、接続を確立する前に、ローカルコンピューター上のポリシーが有効になります。 接続を使用すると、リモートコンピューター上のポリシーが有効になります。

## <a name="basic-authentication-limitations-on-linux-and-macos"></a>Linux と macOS の基本認証の制限事項

Linux または macOS システムから Windows に接続する場合、HTTP 経由の基本認証はサポートされていません。 基本認証を HTTPS 経由で使用するには、対象サーバーに証明書をインストールします。 証明書には、ホスト名と一致する CN 名、期限切れまたは失効していない名前が必要です。 自己署名証明書は、テスト目的で使用できます。

詳細については、「 [方法: HTTPS 用に WINRM を構成する](https://support.microsoft.com/help/2019527/how-to-configure-winrm-for-https) 」を参照してください。

管理者特権のコマンドプロンプトから次のコマンドを実行すると、インストールされている証明書を使用して Windows 上の HTTPS リスナーが構成されます。

```powershell
$hostinfo = '@{Hostname="<DNS_NAME>"; CertificateThumbprint="<THUMBPRINT>"}'
winrm create winrm/config/Listener?Address=*+Transport=HTTPS $hostinfo
```

Linux または macOS 側で、[認証および-UseSSl] の [基本] を選択します。

> 注: 基本認証は、ドメインアカウントでは使用できません。ローカルアカウントが必要であり、アカウントが Administrators グループに含まれている必要があります。

```powershell
# The specified local user must have administrator rights on the target machine.
# Specify the unqualified username.
$cred = Get-Credential username
$session = New-PSSession -Computer <hostname> -Credential $cred `
  -Authentication Basic -UseSSL
```

HTTPS 経由の基本認証の代わりに Negotiate があります。 この結果、クライアントとサーバー間の NTLM 認証が HTTP 経由で暗号化されます。

次の例では、New-PSSession で Negotiate を使用しています。

```powershell
# The specified user must have administrator rights on the target machine.
$cred = Get-Credential username@hostname
$session = New-PSSession -Computer <hostname> -Credential $cred `
  -Authentication Negotiate
```

> [!NOTE]
> Windows Server では、組み込みの管理者以外の管理者が NTLM を使用して接続できるようにするために、追加のレジストリ設定が必要です。
> 「[リモート接続の認証](/windows/win32/winrm/authentication-for-remote-connections)で認証をネゴシエートする」の LocalAccountTokenFilterPolicy レジストリ設定を参照してください。

## <a name="see-also"></a>関連項目

[about_Remote](about_Remote.md)

[about_Remote_Variables](about_Remote_Variables.md)

[about_PSSessions](about_PSSessions.md)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

