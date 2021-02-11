---
ms.date: 05/14/2020
keywords: powershell,コマンドレット
title: PowerShell リモート処理での次ホップの実行
description: この記事では、セキュリティへの影響や推奨事項など、PowerShell のリモート処理のために次ホップ認証を構成するさまざまな方法について説明します。
ms.openlocfilehash: 905b27b4e6c612249c945a741bbe0d2ba9ae28aa
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92501373"
---
# <a name="making-the-second-hop-in-powershell-remoting"></a>PowerShell リモート処理での次ホップの実行

"次ホップの問題" とは次のような状況のことです。

1. _ServerA_ にログインしています。
2. _ServerA_ からリモート PowerShell セッションを開始して _ServerB_ に接続します。
3. PowerShell リモート処理セッションを介して _ServerB_ で実行するコマンドは、_ServerC_ 上のリソースにアクセスを試みます。
4. PowerShell リモート処理セッションの作成に使った資格情報は _ServerB_ から _ServerC_ に渡されないため、_ServerC_ 上のリソースへのアクセスは拒否されます。

この問題に対処する方法はいくつかあります。 次の表に、優先順位順に方法の一覧を示します。

|                      構成                       |                                  Note                                  |
| -------------------------------------------------------- | ---------------------------------------------------------------------- |
| CredSSP                                                  | 使いやすさとセキュリティのバランスが取れている                                      |
| リソースに基づく Kerberos の制約付き委任           | 単純な構成で高いセキュリティ                             |
| Kerberos の制約付き委任                          | セキュリティは高いが、ドメイン管理者が必要                         |
| Kerberos の委任 (無制限)                      | 推奨されません                                                        |
| Just Enough Administration (JEA)                         | セキュリティは最も高いが、より詳細な構成が必要 |
| RunAs を使用する PSSessionConfiguration                       | 簡単に構成できるが、資格情報の管理が必要                |
| `Invoke-Command` スクリプト ブロックの内部で資格情報を渡す | 最も簡単に使用できるが、資格情報を指定する必要がある                       |

## <a name="credssp"></a>CredSSP

認証に[資格情報のセキュリティ サポート プロバイダー (CredSSP)][credssp] を使用できます。
CredSSP はリモート サーバー (_ServerB_) に資格情報をキャッシュするため、これを使用すると資格情報の盗難攻撃にさらされます。 リモート コンピューターが侵害されると、攻撃者はユーザーの資格情報にアクセスできます。 CredSSP は、既定では、クライアント コンピューターとサーバー コンピューターの両方で無効になっています。 CredSSP は、最も信頼性の高い環境でのみ有効にしてください。 たとえば、ドメイン コントローラーは信頼性が高いため、ドメイン コントローラーに接続しているドメイン管理者が有効にすることをお勧めします。

PowerShell リモート処理で CredSSP を使用する場合のセキュリティに関する注意事項の詳細については、「[Accidental Sabotage: Beware of CredSSP (予想外の妨害行為: CredSSP に関する注意事項)][beware]」を参照してください。

資格情報の盗難攻撃の詳細については、「[Mitigating Pass-the-Hash (PtH) Attacks and Other Credential Theft (Pass-the-Hash (PtH) 攻撃とその他の資格情報の盗難の抑制)][pth]」を参照してください。

PowerShell リモート処理用に CredSSP を有効にして使用する方法の例については、「[CredSSP を使用して PowerShell "次ホップ" 機能を有効にする][credssp-psblog]」をご覧ください。

**長所**

- Windows Server 2008 以降のすべてのサーバーで機能します。

**短所**

- セキュリティの脆弱性があります。
- クライアント ロールとサーバー ロールの両方の構成が必要です。
- Protected Users グループでは機能しません。 詳細については、「[Protected Users セキュリティ グループ][protected-users]」をご覧ください。

## <a name="kerberos-constrained-delegation"></a>Kerberos の制約付き委任

従来の (リソースに基づかない) 制約付き委任を使って、次ホップを実行できます。 プロトコル遷移を許可するには、[任意の認証プロトコルを使う] オプションを使用して Kerberos の制約付き委任を構成します。

**長所**

- 特別なコーディングが必要ありません。
- 資格情報が保存されません。

**短所**

- WinRM の次ホップはサポートされません。
- 構成には、ドメイン管理者のアクセス権が必要です。
- リモート サーバー (_ServerB_) の Active Directory オブジェクトで構成する必要があります。
- 1 つのドメインに制限されます。 複数のドメインまたはフォレストにまたがることはできません。
- オブジェクトとサービス プリンシパル名 (SPN) を更新する権限が必要です。
- _ServerB_ は、ユーザーの介入なしに、ユーザーに代わって _ServerC_ への Kerberos チケットを取得できます。

> [!NOTE]
> **[アカウントは重要なので委任できない]** プロパティが設定されている Active Directory アカウントは委任できません。 詳しくは、「[Security Focus: セキュリティ フォーカス: 特権アカウントに対する "アカウントは重要なので委任できない" の分析][blog]」および「[Kerberos 認証のツールと設定][ktools]」をご覧ください。

## <a name="resource-based-kerberos-constrained-delegation"></a>リソースに基づく Kerberos の制約付き委任

リソースに基づく Kerberos の制約付き委任 (Windows Server 2012 で導入) を使って、リソースが存在するサーバー オブジェクトでの資格情報の委任を構成します。 上で説明した次ホップのシナリオでは、_ServerC_ を構成して、どこから委任された資格情報の受け入れるかを指定します。

**長所**

- 資格情報が保存されません。
- PowerShell コマンドレットを使用して構成します。 特別にコードを記述する必要がありません。
- 構成に、ドメイン管理者のアクセス権は必要ありません。
- 複数のドメインおよびフォレストをまたいで動作します。

**短所**

- Windows Server 2012 以降が必要です。
- WinRM の次ホップはサポートされません。
- オブジェクトとサービス プリンシパル名 (SPN) を更新する権限が必要です。

> [!NOTE]
> **[アカウントは重要なので委任できない]** プロパティが設定されている Active Directory アカウントは委任できません。 詳しくは、「[Security Focus: セキュリティ フォーカス: 特権アカウントに対する "アカウントは重要なので委任できない" の分析][blog]」および「[Kerberos 認証のツールと設定][ktools]」をご覧ください。

### <a name="example"></a>例

_ServerB_ からの委任された資格情報を許可するように _ServerC_ でリソースに基づく制約付き委任を構成する PowerShell の例を示します。 この例では、すべてのサーバーが Windows Server 2012 以降を実行しており、いずれかのサーバーが属している各ドメインに少なくとも 1 つの Windows Server 2012 ドメイン コントローラーがあるものとします。

制約付き委任を構成する前に、`RSAT-AD-PowerShell` 機能を追加して Active Directory PowerShell モジュールをインストールした後、そのモジュールをセッションにインポートする必要があります。

```powershell
Add-WindowsFeature RSAT-AD-PowerShell
Import-Module ActiveDirectory
Get-Command -ParameterName PrincipalsAllowedToDelegateToAccount
```

使用できる複数のコマンドレットに、**PrincipalsAllowedToDelegateToAccount** パラメーターが追加されています。

```Output
CommandType Name                 ModuleName
----------- ----                 ----------
Cmdlet      New-ADComputer       ActiveDirectory
Cmdlet      New-ADServiceAccount ActiveDirectory
Cmdlet      New-ADUser           ActiveDirectory
Cmdlet      Set-ADComputer       ActiveDirectory
Cmdlet      Set-ADServiceAccount ActiveDirectory
Cmdlet      Set-ADUser           ActiveDirectory
```

**PrincipalsAllowedToDelegateToAccount** パラメーターは、Active Directory オブジェクトの **msDS-AllowedToActOnBehalfOfOtherIdentity** 属性を設定します。この属性には、関連付けられたアカウント (この例では、_Server_ のコンピューター アカウント) に資格情報を委任する権限を持つアカウントを指定するアクセス制御リスト (ACL) が含まれます。

サーバーを表すために使用する変数を設定します。

```powershell
# Set up variables for reuse
$ServerA = $env:COMPUTERNAME
$ServerB = Get-ADComputer -Identity ServerB
$ServerC = Get-ADComputer -Identity ServerC
```

WinRM (したがって PowerShell リモート処理) は、既定でコンピューター アカウントとして実行します。 これは、`winrm` サービスの **StartName** プロパティを見て確認できます。

```powershell
Get-CimInstance Win32_Service -Filter 'Name="winrm"' | Select-Object StartName
```

```Output
StartName
---------
NT AUTHORITY\NetworkService
```

_ServerC_ で _ServerB_ の PowerShell リモート処理セッションからの委任を許可するため、_ServerC_ の **PrincipalsAllowedToDelegateToAccount** パラメーターを _ServerB_ のコンピューター オブジェクトに設定する必要があります。

```powershell
# Grant resource-based Kerberos constrained delegation
Set-ADComputer -Identity $ServerC -PrincipalsAllowedToDelegateToAccount $ServerB

# Check the value of the attribute directly
$x = Get-ADComputer -Identity $ServerC -Properties msDS-AllowedToActOnBehalfOfOtherIdentity
$x.'msDS-AllowedToActOnBehalfOfOtherIdentity'.Access

# Check the value of the attribute indirectly
Get-ADComputer -Identity $ServerC -Properties PrincipalsAllowedToDelegateToAccount
```

Kerberos の[キー配布センター (KDC)](/windows/win32/secauthn/key-distribution-center) は、拒否されたアクセス試行を 15 分間キャッシュします (ネガティブ キャッシュ)。 _ServerB_ が以前に _ServerC_ にアクセスしようとした場合、次のコマンドを呼び出すことによって _ServerB_ 上のキャッシュをクリアする必要があります。

```powershell
Invoke-Command -ComputerName $ServerB.Name -Credential $cred -ScriptBlock {
    klist purge -li 0x3e7
}
```

キャッシュを消去するには、コンピューターを再起動するか、15 分以上待つのでもかまいません。

キャッシュをクリアした後は、_ServerA_ から _ServerB_ を経由して _ServerC_ にコードを正常に実行できます。

```powershell
# Capture a credential
$cred = Get-Credential Contoso\Alice

# Test kerberos double hop
Invoke-Command -ComputerName $ServerB.Name -Credential $cred -ScriptBlock {
    Test-Path \\$($using:ServerC.Name)\C$
    Get-Process lsass -ComputerName $($using:ServerC.Name)
    Get-EventLog -LogName System -Newest 3 -ComputerName $($using:ServerC.Name)
}
```

この例では、_ServerB_ が `$ServerC` 変数を認識できるようにするため、`$using` 変数を使用しています。
`$using` 変数については、「[About Remote Variables](/powershell/module/Microsoft.PowerShell.Core/About/about_Remote_Variables)」 (リモート変数について) を参照してください。

複数のサーバーが _ServerC_ に資格情報を委任できるようにするには、_ServerC_ で **PrincipalsAllowedToDelegateToAccount** パラメーターの値に配列を設定します。

```powershell
# Set up variables for each server
$ServerB1 = Get-ADComputer -Identity ServerB1
$ServerB2 = Get-ADComputer -Identity ServerB2
$ServerB3 = Get-ADComputer -Identity ServerB3
$ServerC  = Get-ADComputer -Identity ServerC

# Grant resource-based Kerberos constrained delegation
Set-ADComputer -Identity $ServerC `
    -PrincipalsAllowedToDelegateToAccount @($ServerB1,$ServerB2,$ServerB3)
```

ドメインをまたいで次ホップを実行する場合は、_ServerB_ が属するドメインのドメイン コントローラーの完全修飾ドメイン名 (FQDN) を追加します。

```powershell
# For ServerC in Contoso domain and ServerB in other domain
$ServerB = Get-ADComputer -Identity ServerB -Server dc1.alpineskihouse.com
$ServerC = Get-ADComputer -Identity ServerC
Set-ADComputer -Identity $ServerC -PrincipalsAllowedToDelegateToAccount $ServerB
```

ServerC に資格情報を委任する機能を削除するには、_ServerC_ の **PrincipalsAllowedToDelegateToAccount** パラメーターの値に `$null` を設定します。

```powershell
Set-ADComputer -Identity $ServerC -PrincipalsAllowedToDelegateToAccount $null
```

### <a name="information-on-resource-based-kerberos-constrained-delegation"></a>リソースに基づく Kerberos の制約付き委任についての情報

- [Kerberos 認証の新機能][whats-new]
- [Windows Server 2012 による Kerberos の制約付き委任の処理方法、第 1 部][kcd2012-1]
- [Windows Server 2012 による Kerberos の制約付き委任の処理方法、第 2 部][kcd2012-2]
- [統合 Windows 認証での Azure Active Directory アプリケーション プロキシ展開に対する Kerberos の制約付き委任の概要][kcdpaper]
- [[MS-ADA2] Active Directory スキーマ属性 M2.210 属性 msDS-AllowedToActOnBehalfOfOtherIdentity][MS-ADA2]
- [[MS-SFU] Kerberos プロトコル拡張:Service for User および制約付き委任プロトコルの 1.3.2 S4U2proxy][MS-SFU]
- [PrincipalsAllowedToDelegateToAccount を使用した制約付き委任を使用しないリモート管理][remote-admin]

## <a name="kerberos-delegation-unconstrained"></a>Kerberos の委任 (無制限)

Kerberos の無制限の委任を使って、次ホップを実行することもできます。 すべての Kerberos のシナリオと同様に、資格情報は保存されません。 この方法では、WinRM の次ホップはサポートされません。

> [!WARNING]
> この方法では、委任された資格情報がどこで使われるかは制御されません。 CredSSP よりもセキュリティが低いです。 このメソッドは、テスト シナリオにのみ使用してください。

## <a name="just-enough-administration-jea"></a>Just Enough Administration (JEA)

JEA では、PowerShell セッションの間に管理者が実行できるコマンドを制限できます。 これを使って、次ホップの問題を解決できます。

JEA については、「[Just Enough Administration](/powershell/scripting/learn/remoting/jea/overview)」をご覧ください。

**長所**

- 仮想アカウントを使う場合にパスワードの管理が不要です。

**短所**

- WMF 5.0 以降が必要です。
- すべての中間サーバー (_ServerB_) の構成が必要です。

## <a name="pssessionconfiguration-using-runas"></a>RunAs を使用する PSSessionConfiguration

_ServerB_ にセッション構成を作成し、その **RunAsCredential** パラメーターを設定できます。

**PSSessionConfiguration** と **RunAs** を使って次ホップの問題を解決する方法については、[PowerShell をリモートから使用する場合のマルチホップの別の解決策][pssessionconfig]に関するページをご覧ください。

**長所**

- WMF 3.0 以降のすべてのサーバーで機能します。

**短所**

- すべての中間サーバー (_ServerB_) で **PSSessionConfiguration** と **RunAs** を構成する必要があります。
- ドメインの **RunAs** アカウントを使うときは、パスワードの管理が必要です。

## <a name="pass-credentials-inside-an-invoke-command-script-block"></a>Invoke-Command スクリプト ブロックの内部で資格情報を渡す

[Invoke-Command](/powershell/module/microsoft.powershell.core/invoke-command) コマンドレットを呼び出すときに **ScriptBlock** パラメーターの内部で資格情報を渡すことができます。

**長所**

- 特別なサーバー構成は必要ありません。
- WMF 2.0 以降を実行する任意のサーバーで動作します。

**短所**

- 面倒なコード技法が必要です。
- WMF 2.0 を実行している場合は、リモート セッションに引数を渡すために異なる構文が必要です。

### <a name="example"></a>例

**Invoke-Command** スクリプト ブロックで資格情報を渡す方法の例を次に示します。

```powershell
# This works without delegation, passing fresh creds
# Note $Using:Cred in nested request
$cred = Get-Credential Contoso\Administrator
Invoke-Command -ComputerName ServerB -Credential $cred -ScriptBlock {
    hostname
    Invoke-Command -ComputerName ServerC -Credential $Using:cred -ScriptBlock {hostname}
}
```

## <a name="see-also"></a>関連項目

[PowerShell リモート処理のセキュリティに関する考慮事項](WinRMSecurity.md)

<!-- link references -->
[blog]: /archive/blogs/poshchap/security-focus-analysing-account-is-sensitive-and-cannot-be-delegated-for-privileged-accounts
[ktools]: /previous-versions/windows/it-pro/windows-server-2003/cc738673(v=ws.10)
[credssp]: /windows/win32/secauthn/credential-security-support-provider
[beware]: https://www.powershellmagazine.com/2014/03/06/accidental-sabotage-beware-of-credssp
[pth]: https://www.microsoft.com/download/details.aspx?id=36036
[credssp-psblog]: https://devblogs.microsoft.com/scripting/enable-powershell-second-hop-functionality-with-credssp/
[whats-new]: /previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)
[kcd2012-1]: https://www.itprotoday.com/windows-server/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1
[kcd2012-2]: https://www.itprotoday.com/windows-server/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-2
[kcdpaper]: https://aka.ms/kcdpaper
[MS-ADA2]: /openspecs/windows_protocols/ms-ada2/cea4ac11-a4b2-4f2d-84cc-aebb4a4ad405
[MS-SFU]: /openspecs/windows_protocols/ms-sfu/bde93b0e-f3c9-4ddf-9f44-e1453be7af5a
[remote-admin]: /archive/blogs/taylorb/remote-administration-without-constrained-delegation-using-principalsallowedtodelegatetoaccount
[pssessionconfig]: /archive/blogs/sergey_babkins_blog/another-solution-to-multi-hop-powershell-remoting
[protected-users]: /windows-server/security/credentials-protection-and-management/protected-users-security-group
