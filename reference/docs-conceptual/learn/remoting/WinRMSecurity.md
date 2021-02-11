---
ms.date: 06/11/2020
keywords: powershell,コマンドレット
title: WinRM を使用した PowerShell リモート処理のセキュリティに関する考慮事項
description: このドキュメントでは、PowerShell リモート処理を使用する場合のセキュリティ上の問題、推奨事項、およびベスト プラクティスを取り上げています。
ms.openlocfilehash: 48167bd297905883b3d75caf9a07d06e6a9fc467
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92501475"
---
# <a name="security-considerations-for-powershell-remoting-using-winrm"></a>WinRM を使用した PowerShell リモート処理のセキュリティに関する考慮事項

PowerShell リモート処理は、Windows システムの管理に推奨されている方法です。 Windows Server 2012 R2 では、PowerShell リモート処理が既定で有効になっています。 このドキュメントでは、PowerShell リモート処理を使用する場合のセキュリティ上の問題、推奨事項、およびベスト プラクティスを取り上げています。

## <a name="what-is-powershell-remoting"></a>PowerShell リモート処理とは

PowerShell リモート処理は、[Windows リモート管理 (WinRM)](/windows/win32/winrm/portal) を使用しています。これは、ユーザーがリモート コンピューター上で PowerShell コマンドを実行できるように、Microsoft によって [Web Services for Management (WS-Management)](https://www.dmtf.org/sites/default/files/standards/documents/DSP0226_1.2.0.pdf) プロトコルが実装されたものです。 PowerShell リモート処理の使用方法の詳細については、「[リモート コマンドの実行](running-remote-commands.md)」を参照してください。

PowerShell リモート処理は、コマンドレットの **ComputerName** パラメーターを使用してリモート コンピューターでコマンドを実行することとは異なります。この場合は、基盤となるプロトコルとしてリモート プロシージャ コール (RPC) が使用されています。

## <a name="powershell-remoting-default-settings"></a>PowerShell リモート処理の既定の設定

PowerShell リモート処理 (および WinRM) は、次のポートをリッスンします。

- HTTP: 5985
- HTTPS: 5986

既定では、PowerShell リモート処理で許可されている接続は、Administrators グループのメンバーからのもののみです。
セッションはユーザーのコンテキストで開始されるため、PowerShell リモート処理を介して接続されている間は、個々のユーザーとグループに適用されているオペレーティング システムのアクセス制御がすべて、そのセッションに引き続き適用されます。

プライベート ネットワークの場合、PowerShell リモート処理の既定の Windows ファイアウォール規則はすべての接続を受け入れます。 パブリック ネットワークの場合、既定の Windows ファイアウォール規則で許可されるのは、同じサブネット内からの PowerShell リモート処理接続のみです。 パブリック ネットワーク上のすべての接続に対して PowerShell リモート処理を可能にするには、その規則を明示的に変更する必要があります。

> [!Warning]
> パブリック ネットワークのファイアウォール規則は、悪意のある外部接続からコンピューターを保護することを目的としています。 この規則を削除する際は注意してください。

## <a name="process-isolation"></a>プロセスの分離

PowerShell リモート処理によって、コンピューター間の通信に WinRM が使用されます。 WinRM は Network Service アカウントでサービスとして実行されており、ユーザー アカウントとして実行される分離プロセスを生成して、PowerShell インスタンスをホストします。 1 ユーザーとして実行されている PowerShell のインスタンスは、別のユーザーとして PowerShell のインスタンスを実行しているプロセスにアクセスすることはできません。

## <a name="event-logs-generated-by-powershell-remoting"></a>PowerShell リモート処理によって生成されたイベント ログ

FireEye は、PowerShell リモート処理セッションによって生成されたイベント ログやその他のセキュリティ証拠をまとめたものを提供しています。これは、概要をまとめられています。これについては、「[Investigating PowerShell Attacks](https://www.fireeye.com/content/dam/fireeye-www/global/en/solutions/pdfs/wp-lazanciyan-investigating-powershell-attacks.pdf)」(PowerShell 攻撃の調査) を参照してください。

## <a name="encryption-and-transport-protocols"></a>暗号化とトランスポート プロトコル

PowerShell リモート処理の接続のセキュリティについては、初期認証と進行中の通信という 2 つの観点から考慮することをお勧めします。

使用されているトランスポート プロトコル (HTTP または HTTPS) に関係なく、WinRM により、常にすべての PowerShell リモート処理の通信が、初期認証後に暗号化されます。

### <a name="initial-authentication"></a>初期認証

認証では、サーバーに対するクライアントの ID と、理想的にはクライアントに対するサーバーを確認します。

クライアントがコンピューター名を使用してドメイン サーバーに接続する場合、既定の認証プロトコルは [Kerberos](/windows/win32/secauthn/microsoft-kerberos) です。 Kerberos では、再利用可能な資格情報を送信しなくても、ユーザー ID とサーバー ID の両方が保証されます。

クライアントが IP アドレスを使用してドメイン サーバーに接続する場合、またはワークグループ サーバーに接続する場合は、Kerberos 認証を使用できません。 その場合は、PowerShell リモート処理は [NTLM 認証プロトコル](/windows/win32/secauthn/microsoft-ntlm)を利用します。 NTLM 認証プロトコルでは、委任可能な資格情報を送信しなくてもユーザー ID が保証されます。 NTLM プロトコルは、ユーザー ID を証明するために、クライアントとサーバーの両方でユーザーのパスワードからセッション キーを計算することを要求します。パスワード自体は交換しません。 サーバーは、通常ユーザーのパスワードを知らないため、ドメイン コントローラーと通信します。ドメイン コントローラーがユーザーのパスワードを知っていて、サーバー用にセッション キーを計算します。

一方、NTLM プロトコルでは、サーバー ID が保証されません。 ドメインに参加しているコンピューターのコンピューター アカウントにアクセスできる攻撃者は、認証に NTLM を使用するプロトコルと同様にドメイン コントローラーを呼び出して NTLM セッション キーを計算することで、サーバーになりすます場合があります。

NTLM ベースの認証は既定では無効になっていますが、ターゲット サーバーで SSL を構成するか、クライアントで WinRM TrustedHosts 設定を構成することで許可できます。

#### <a name="using-ssl-certificates-to-validate-server-identity-during-ntlm-based-connections"></a>NTLM ベースの接続時に SSL 証明書を使用してサーバー ID を検証する

NTLM 認証プロトコルではターゲット サーバーの ID を保証できないため (パスワードを認識しているだけのため)、PowerShell リモート処理に SSL を使用するようターゲット サーバーを構成できます。
SSL 証明書をターゲット サーバーに割り当てると (クライアントも信頼している証明機関によって発行されている場合)、ユーザー ID とサーバー ID の両方が保証される NTLM ベースの認証が可能になります。

#### <a name="ignoring-ntlm-based-server-identity-errors"></a>NTLM ベースのサーバー ID を無視する

SSL 証明書を NTLM 接続用のサーバーに展開できない場合は、そのサーバーを WinRM の **TrustedHosts** 一覧に追加することで ID エラーの発生を抑制できます。 サーバー名を **TrustedHosts** の一覧に追加することは、ホスト自体の信頼性を表明するいずれの形でもないことにご注意ください。NTLM 認証プロトコルでは、実際に接続先となるホストが、意図する接続先であるとは限らないからです。 代わりに、TrustedHosts の設定を、サーバーの ID を確認できないことが原因で生成されたエラーを抑制するホストの一覧と考える必要があります。

### <a name="ongoing-communication"></a>進行中の通信

初期認証が完了すると、WinRM では、進行中の通信が暗号化されます。 HTTPS 経由で接続する場合は、データの転送に使用される暗号化をネゴシエートするために TLS プロトコルが使用されます。
HTTP 経由で接続する場合、メッセージレベルの暗号化は、使用される初期認証プロトコルによって決定されます。

- 基本認証では、暗号化は行われません。
- NTLM 認証では、128 ビット キーを使用して RC4 暗号が使用されます。
- Kerberos 認証の暗号化は、TGS チケットの `etype` によって決定されます。 これは、最新のシステムでの AES-256 です。
- CredSSP 暗号化では、ハンドシェイクでネゴシエートされた TLS 暗号スイートが使用されます。

## <a name="making-the-second-hop"></a>次ホップの実行

PowerShell リモート処理では、既定で、認証に Kerberos (使用可能な場合) または NTLM が使用されます。 このどちらのプロトコルも、資格情報を送信することなく、リモート コンピューターに対して認証を行います。 これは認証方法としては最も安全ですが、リモート コンピューターにユーザーの資格情報がないため、このリモート コンピューターは、ユーザーに代わって他のコンピューターおよびサービスにアクセスすることはできません。 これは "次ホップの問題" と呼ばれます。

この問題を回避する方法はいくつかあります。 これらの方法およびその長所と短所については、「[PowerShell リモート処理での次ホップの実行](PS-remoting-second-hop.md)」をご覧ください。

## <a name="references"></a>References

- [Windows リモート管理 (WinRM)](/windows/win32/winrm/portal)
- [Web Services for Management (WS-Management)](https://www.dmtf.org/sites/default/files/standards/documents/DSP0226_1.2.0.pdf)
- [2.2.9.1 暗号化されたメッセージ型](/openspecs/windows_protocols/ms-wsmv/58421aa4-861a-4410-831a-c999f094cdb7)
- [Kerberos](/windows/win32/secauthn/microsoft-kerberos)
- [NTLM 認証プロトコル](/windows/win32/secauthn/microsoft-ntlm)
- [PowerShell 攻撃の調査](https://www.fireeye.com/content/dam/fireeye-www/global/en/solutions/pdfs/wp-lazanciyan-investigating-powershell-attacks.pdf)
