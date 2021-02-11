---
ms.date: 07/10/2019
keywords: JEA, PowerShell, セキュリティ
title: JEA セッションの構成
description: セッションの構成によって、誰が JEA エンドポイントを使用でき、どのロールにアクセスできるかが定義されます。
ms.openlocfilehash: b616d5bf260bbdfe89b6422fd4a8b4866f7fdc67
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92501560"
---
# <a name="jea-session-configurations"></a>JEA セッションの構成

JEA エンドポイントは、PowerShell セッション構成ファイルを作成して登録することにより、システムに登録されます。 セッションの構成によって、誰が JEA エンドポイントを使用でき、どのロールにアクセスできるかが定義されます。 また、JEA セッション内のすべてのユーザーに適用されるグローバル設定も定義されます。

## <a name="create-a-session-configuration-file"></a>セッション構成ファイルを作成する

JEA エンドポイントを登録するには、そのエンドポイントの構成方法を指定する必要があります。 考慮すべきオプションは多数あります。 最も重要なオプションは次のとおりです。

- 誰が JEA エンドポイントにアクセスできるか
- どのようなロールが彼らに割り当てられるか
- JEA は内部でどの ID を使用するか
- JEA エンドポイントの名前

これらのオプションは、PowerShell セッションの構成ファイルである `.pssc` 拡張子の付いた PowerShell データ ファイルに定義されています。 このセッション構成ファイルは、任意のテキスト エディターで編集できます。

空のテンプレート構成ファイルを作成するには、次のコマンドを実行します。

```powershell
New-PSSessionConfigurationFile -SessionType RestrictedRemoteServer -Path .\MyJEAEndpoint.pssc
```

> [!TIP]
> テンプレート ファイルには、既定で最も一般的な構成オプションのみが含まれています。 適用できるすべての設定を生成される PSSC に含めるには、`-Full` スイッチを使います。

`-SessionType RestrictedRemoteServer` フィールドは、セキュリティ管理のためにセッション構成が JEA によって使われることを示します。 このような種類のセッションは、**NoLanguage** モードで動作し、次の既定のコマンド (およびエイリアス) のみにアクセスできます。

- Clear-Host (cls、clear)
- Exit-PSSession (exsn、exit)
- Get-Command (gcm)
- Get-FormatData
- Get-Help
- Measure-Object (measure)
- Out-Default
- Select-Object (select)

PowerShell プロバイダーおよび外部プログラム (実行可能ファイルまたはスクリプト) は使用できません。

言語モードの詳細については、「[about_Language_modes](/powershell/module/microsoft.powershell.core/about/about_language_modes)」(言語モードについて) に関するページを参照してください。

### <a name="choose-the-jea-identity"></a>JEA ID を選択する

バックグラウンドで、接続ユーザーのコマンドの実行中に、JEA は使用する ID (アカウント) が必要です。
セッション構成ファイルには、JEA が使う ID を定義します。

#### <a name="local-virtual-account"></a>ローカル仮想アカウント

ローカル仮想アカウントは、JEA エンドポイント用に定義されているすべてのロールがローカル マシンの管理に使われ、ローカル管理者アカウントがコマンドを正常に実行するために十分である場合に便利です。
仮想アカウントは、特定のユーザーに固有の一時的なアカウントであり、PowerShell セッションの期間中にのみ存在します。 メンバー サーバーまたはワークステーションでは、仮想アカウントはローカル コンピューターの **Administrators** グループに属しています。 Active Directory ドメイン コントローラーでは、仮想アカウントはドメインの **Domain Admins** グループに属しています。

```powershell
# Setting the session to use a virtual account
RunAsVirtualAccount = $true
```

セッション構成で定義されているロールに完全な管理権限が必要ない場合は、仮想アカウントが属するセキュリティ グループを指定します。 メンバー サーバーまたはワークステーションでは、ドメインのグループではなくローカル グループからセキュリティ グループを指定する必要があります。

1 つ以上のセキュリティ グループを指定すると、その仮想アカウントはローカル グループまたはドメイン管理者グループには割り当てられません。

```powershell
# Setting the session to use a virtual account that only belongs to the NetworkOperator and NetworkAuditor local groups
RunAsVirtualAccount = $true
RunAsVirtualAccountGroups = 'NetworkOperator', 'NetworkAuditor'
```

> [!NOTE]
> 仮想アカウントには、ローカル サーバーのセキュリティ ポリシーで、サービスとしてログオンが一時的に付与されます。 指定されている VirtualAccountGroups の 1 つにポリシーでこれが既に付与されている場合、個別の仮想アカウントがポリシーに追加されたり、ポリシーから削除されたりすることがなくなります。 セキュリティ ポリシーの改訂が念入りに監査されるドメイン コントローラーのようなシナリオで役立ちます。 これは、2018 年 11 月以降のロールアップが適用された Windows Server 2016 と、2019 年 1 月以降のロールアップが適用された Windows Server 2019 でのみ利用できます。

#### <a name="group-managed-service-account"></a>グループ管理サービス アカウント

グループ管理サービス アカウント (GMSA) とは、JEA ユーザーがファイル共有や Web サービスなどのネットワーク リソースにアクセスする必要がある場合の使用に適した ID です。 GMSA では、ドメイン内の任意のコンピューター上のリソースに対する認証に使用するドメイン ID を得ることができます。 GMSA によって提供される権限は、ユーザーがアクセスするリソースによって決まります。 コンピューターまたはサービスの管理者が明示的に GMSA に管理者権限を付与しない限り、ユーザーにはコンピューターまたはサービスに対する管理者権限は付与されません。

```powershell
# Configure JEA sessions to use the GMSA in the local computer's domain
# with the sAMAccountName of 'MyJEAGMSA'
GroupManagedServiceAccount = 'Domain\MyJEAGMSA'
```

GMSA は必要な場合のみ使用する必要があります。

- GMSA を使用した場合、アクションをユーザーまでたどることは困難です。 どのユーザーにも、同じ実行者 ID が使用されます。 個々のユーザーとそのアクションの関連付けを確認するには、PowerShell セッションのトランスクリプトとログを確認する必要があります。

- GMSA は、接続しているユーザーがアクセスする必要のない、多くのネットワーク リソースにアクセスできる場合があります。 常に最小限の特権の原則に従って、JEA セッションで有効なアクセス許可を制限してください。

> [!NOTE]
> グループ管理サービス アカウントは、PowerShell 5.1 以降を使用した、ドメインに参加しているコンピューターでのみ使用できます。

JEA セッションのセキュリティ保護の詳細については、[セキュリティに関する考慮事項](security-considerations.md)に関する記事を参照してください。

### <a name="session-transcripts"></a>セッションのトランスクリプト

ユーザー セッションのトランスクリプトを自動的に記録するには、JEA エンドポイントを構成することをお勧めします。 PowerShell セッションのトランスクリプトには、接続しているユーザー、そのユーザーに割り当てられている実行 ID、そのユーザーによって実行されたコマンドに関する情報が含まれます。 これは、システムに特定の変更を行ったユーザーを把握する必要がある監査チームに役に立ちます。

セッション構成ファイルで自動トランスクリプトを構成するには、トランスクリプトを保存するフォルダーへのパスを指定します。

```powershell
TranscriptDirectory = 'C:\ProgramData\JEAConfiguration\Transcripts'
```

トランスクリプトは、**ローカル システム** アカウントによってフォルダーに書き込まれるので、このアカウントにはこのディレクトリに対する読み書きのアクセス権が必要です。 標準ユーザーは、このフォルダーにアクセスできないようにする必要があります。 トランスクリプトを監査できるセキュリティ管理者の数は制限する必要があります。

### <a name="user-drive"></a>ユーザー ドライブ

接続しているユーザーが、JEA エンドポイントにファイルをコピーしたり、そこからコピーする必要がある場合、セッション構成ファイルでユーザー ドライブを有効にします。 ユーザー ドライブは、接続しているユーザーごとに固有のフォルダーにマップされた **PSDrive** です。 このフォルダーでは、ユーザーにファイル システム全体へのアクセス権を与えたり、FileSystem プロバイダーを公開したりせずに、ユーザーはシステムからファイルをコピーしたり、システムにコピーできます。 ユーザー ドライブの内容は、ネットワーク接続が中断される場合に備えて、セッションの間は保持されます。

```powershell
MountUserDrive = $true
```

既定では、ユーザー ドライブを使用することで、ユーザーごとに最大 50 MB のデータを格納できます。 *UserDriveMaximumSize* フィールドで、ユーザーが使用できるデータの量を制限できます。

```powershell
# Enables the user drive with a per-user limit of 500MB (524288000 bytes)
MountUserDrive = $true
UserDriveMaximumSize = 524288000
```

ユーザー ドライブにデータを永続的に保持しない場合は、システムにタスクをスケジュールして、毎晩フォルダーを自動的にクリーンアップします。

> [!NOTE]
> ユーザー ドライブを使用できるのは、PowerShell 5.1 以降のみです。

PSDrives の詳細については、[PowerShell ドライブの管理](/powershell/scripting/samples/managing-windows-powershell-drives)に関するページを参照してください。

### <a name="role-definitions"></a>ロールの定義

セッション構成ファイルのロールの定義では、**ロール** への **ユーザー** のマッピングを定義します。 このフィールドに含まれるすべてのユーザーまたはグループには、登録時に JEA エンドポイントへのアクセス許可が自動的に付与されます。
各ユーザーまたはグループは、ハッシュ テーブルのキーとして含まれるのは 1 回だけですが、複数のロールを割り当てることができます。 ロール機能の名前は、ロール機能ファイルの名前から `.psrc` 拡張子を除いたものにする必要があります。

```powershell
RoleDefinitions = @{
    'CONTOSO\JEA_DNS_ADMINS'    = @{ RoleCapabilities = 'DnsAdmin', 'DnsOperator', 'DnsAuditor' }
    'CONTOSO\JEA_DNS_OPERATORS' = @{ RoleCapabilities = 'DnsOperator', 'DnsAuditor' }
    'CONTOSO\JEA_DNS_AUDITORS'  = @{ RoleCapabilities = 'DnsAuditor' }
}
```

ユーザーがロール定義の複数のグループに所属している場合、それぞれのロールのアクセス許可を付与されます。 2 つのロールに同じコマンドレットへのアクセスが許可されている場合、最も制限の少ないパラメーター セットがユーザーに付与されます。

ロール定義フィールドでローカル ユーザーまたはグループを指定する場合は、必ず (**localhost** やワイルドカードではなく) コンピューター名を指定してください。 コンピューター名を確認するには、`$env:COMPUTERNAME` 変数を調べます。

```powershell
RoleDefinitions = @{
    'MyComputerName\MyLocalGroup' = @{ RoleCapabilities = 'DnsAuditor' }
}
```

### <a name="role-capability-search-order"></a>ロール機能の検索順序

上の例で示されているように、ロール機能はロール機能ファイルのベース名によって参照されます。 ファイルのベース名とは、拡張子を除いたファイル名です。 システムに同じ名前のロール機能が複数ある場合、PowerShell は暗黙的な検索順序を使って有効なロール機能ファイルを選びます。 JEA は、同じ名前のすべてのロール機能ファイルにはアクセスを付与 **しません**。

JEA は `$env:PSModulePath` 環境変数を使用して、ロール機能ファイルをスキャンするパスを決定します。 それらのパス内では、JEA は "RoleCapabilities" サブフォルダを含む有効な PowerShell モジュールを検索します。 モジュールのインポートと同様に、JEA は同じ名前のカスタム ロール機能よりも Windows に同梱されているロール機能を優先します。

競合する他のすべての名前の優先順位は、Windows によってファイルがディレクトリ内で列挙される順序によって決まります。 この順序は、アルファベット順であるとは限りません。 指定した名前と一致する最初に検出されたロール機能ファイルは、接続ユーザーに対して使われます。 ロール機能の検索順序は決定論的ではないので、ロール機能のファイル名を一意にすることを **強くお勧め** します。

### <a name="conditional-access-rules"></a>条件付きアクセス規則

**RoleDefinitions** フィールドに含まれるすべてのユーザーとグループは、JEA エンドポイントへのアクセスが自動的に許可されます。 条件付きアクセス規則を使うと、このアクセスを調整して、ユーザーに割り当て済みのロールに影響を与えない別のセキュリティ グループに、ユーザーを含めることができます。 これは、Just-In-Time 特権アクセスの管理ソリューション、スマート カード認証、またはその他の多要素認証ソリューションを JEA と統合する場合に役立ちます。

条件付きアクセス規則は、セッション構成ファイルの RequiredGroups フィールドで定義します。
そこでは、"And" キーと "Or" キーを使って規則を作成するハッシュ テーブル (必要に応じて入れ子になっている) を指定できます。 このフィールドの使用方法について、次に例を示します。

```powershell
# Example 1: Connecting users must belong to a security group called "elevated-jea"
RequiredGroups = @{ And = 'elevated-jea' }

# Example 2: Connecting users must have signed on with 2 factor authentication or a smart card
# The 2 factor authentication group name is "2FA-logon" and the smart card group name is "smartcard-logon"
RequiredGroups = @{ Or = '2FA-logon', 'smartcard-logon' }

# Example 3: Connecting users must elevate into "elevated-jea" with their JIT system and have logged on with 2FA or a smart card
RequiredGroups = @{ And = 'elevated-jea', @{ Or = '2FA-logon', 'smartcard-logon' }}
```

> [!NOTE]
> 条件付きアクセス規則を使用できるのは、PowerShell 5.1 以降のみです。

### <a name="other-properties"></a>他のプロパティ

セッション構成ファイルではロール機能ファイルで実行できるすべてのことを実行できますが、接続しているユーザーが別のコマンドにアクセスできるようにする機能だけはありません。 すべてのユーザーに特定のコマンドレット、関数、またはプロバイダーへのアクセスを許可する場合は、セッション構成ファイルで行うことができます。
セッション構成ファイルでサポートされているプロパティの一覧については、`Get-Help New-PSSessionConfigurationFile -Full` を実行してください。

## <a name="testing-a-session-configuration-file"></a>セッション構成ファイルのテスト

[Test-PSSessionConfigurationFile](/powershell/module/microsoft.powershell.core/test-pssessionconfigurationfile) コマンドレットを使って、セッション構成をテストできます。 `.pssc` ファイルを手動で編集した場合は、セッション構成ファイルをテストすることをお勧めします。 テストすることにより、構文が正しいことが確認されます。 このテストが失敗するセッション構成ファイルは、システムに登録できません。

## <a name="sample-session-configuration-file"></a>セッション構成ファイルのサンプル

JEA のセッション構成を作成して検証する方法を、次の例に示します。 作成したロール定義は、使いやすくまた読みやすくするため、`$roles` 変数に保存されます。 ただし、必ずこのようにする必要はありません。

```powershell
$roles = @{
    'CONTOSO\JEA_DNS_ADMINS'    = @{ RoleCapabilities = 'DnsAdmin', 'DnsOperator', 'DnsAuditor' }
    'CONTOSO\JEA_DNS_OPERATORS' = @{ RoleCapabilities = 'DnsOperator', 'DnsAuditor' }
    'CONTOSO\JEA_DNS_AUDITORS'  = @{ RoleCapabilities = 'DnsAuditor' }
}

New-PSSessionConfigurationFile -SessionType RestrictedRemoteServer -Path .\JEAConfig.pssc -RunAsVirtualAccount -TranscriptDirectory 'C:\ProgramData\JEAConfiguration\Transcripts' -RoleDefinitions $roles -RequiredGroups @{ Or = '2FA-logon', 'smartcard-logon' }
Test-PSSessionConfigurationFile -Path .\JEAConfig.pssc # should yield True
```

## <a name="updating-session-configuration-files"></a>セッション構成ファイルの更新

JEA セッション構成で、ロールへのユーザーのマッピングなどのプロパティを変更するには、[登録解除](register-jea.md#unregistering-jea-configurations)する必要があります。 その後、更新されたセッション構成ファイルを使用して JEA セッション構成を[再登録](register-jea.md)します。

## <a name="next-steps"></a>次のステップ

- [JEA 構成を登録する](register-jea.md)
- [ロール機能](role-capabilities.md)
