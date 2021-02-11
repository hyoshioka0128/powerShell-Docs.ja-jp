---
ms.date: 07/10/2019
keywords: JEA, PowerShell, セキュリティ
title: JEA の構成の登録
description: システムに JEA エンドポイントを登録すると、ユーザーおよび自動化エンジンでエンドポイントが使用できるようになります。
ms.openlocfilehash: 6e7f8cdc1e7a666bddaa42034d70fcbcf55c1972
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92499911"
---
# <a name="registering-jea-configurations"></a>JEA の構成の登録

[ロール機能](role-capabilities.md)と[セッション構成ファイル](session-configurations.md)を作成したら、最後の手順は、JEA エンドポイントを登録することです。 システムに JEA エンドポイントを登録すると、ユーザーおよび自動化エンジンでエンドポイントが使用できるようになります。

## <a name="single-machine-configuration"></a>単一コンピューターの構成

小規模な環境の場合、[Register-PSSessionConfiguration](/powershell/module/microsoft.powershell.core/register-pssessionconfiguration) コマンドレットを使ってセッション構成ファイルを登録することにより、JEA を展開できます。

作業を開始する前に、次の前提条件を満たしていることを確認してください。

- 1 つ以上のロールが作成されて、PowerShell モジュールの **RoleCapabilities** フォルダーに配置されていること。
- セッション構成ファイルの作成とテストが済んでいること。
- JEA 構成を登録するユーザーに、システムに対する管理者権限があること。
- JEA エンドポイントの名前を選択していること。

ユーザーが JEA を使ってシステムに接続するときに、JEA エンドポイントの名前が必要です。 [Get-PSSessionConfiguration](/powershell/module/microsoft.powershell.core/get-pssessionconfiguration) コマンドレットは、システム上のエンドポイントの名前を一覧表示します。 `microsoft` で始まるエンドポイントは、通常、Windows に付属しています。 `microsoft.powershell` エンドポイントは、リモート PowerShell エンドポイントに接続するときに使われる既定のエンドポイントです。

```powershell
Get-PSSessionConfiguration | Select-Object Name
```

```Output
Name
----
microsoft.powershell
microsoft.powershell.workflow
microsoft.powershell32
```

次のコマンドを実行してエンドポイントを登録します。

```powershell
Register-PSSessionConfiguration -Path .\MyJEAConfig.pssc -Name 'JEAMaintenance' -Force
```

> [!WARNING]
> 前のコマンドにより、システム上の WinRM サービスが再起動します。 これにより、すべての PowerShell リモート処理セッションおよび実行中の DSC 構成が終了されます。 業務の中断を防ぐため、コマンドを実行する前に、運用環境のコンピューターをオフラインにすることをお勧めします。

登録すると、すぐに [JEA を使用](using-jea.md)できます。 セッション構成ファイルは、いつでも削除できます。 構成ファイルは、エンドポイントの登録後には使用されません。

## <a name="multi-machine-configuration-with-dsc"></a>DSC での複数コンピューター構成

JEA を複数のコンピューターに展開するときに、最も簡単な展開モデルでは JEA の [Desired State Configuration (DSC)](../../../dsc/overview/overview.md) リソースを使用して、各コンピューターに JEA を迅速かつ一貫して展開します。

DSC で JEA を展開するには、次の前提条件が満たされていることを確認します。

- 1 つまたは複数のロール機能が作成され、PowerShell モジュールに追加されていること。
- ロールを含む PowerShell モジュールが、各コンピューターからアクセスできる (読み取り専用の) ファイル共有に格納されていること。
- セッション構成の設定が決定されていること。 JEA DSC リソースを使うときは、セッション構成ファイルを作成する必要はありません。
- 各コンピューターで管理操作ができる、またはコンピューターの管理に使われる DSC プル サーバーにアクセスできる資格情報があること。
- [JEA DSC リソース](https://github.com/powershell/JEA/tree/master/DSC%20Resource)をダウンロードしてあること。

ターゲット コンピューターまたはプル サーバー上で、JEA エンドポイント用の DSC 構成を作成します。 この構成では、**JustEnoughAdministration** DSC リソースでセッション構成ファイルを定義し、**File** リソースでファイル共有からロール機能をコピーします。

DSC リソースを使って次のプロパティを構成できます。

- ロールの定義
- 仮想アカウント グループ
- グループ管理されたサービス アカウント名
- トランスクリプト ディレクトリ
- ユーザー ドライブ
- 条件付きアクセス規則
- JEA セッションのスタートアップ スクリプト

DSC 構成でのこれらの各プロパティの構文は、PowerShell セッションの構成ファイルと一致しています。

一般的なサーバー メンテナンス モジュールの DSC 構成の例を以下に示します。 ここでは、ロール機能を含む有効な PowerShell モジュールが `\\myfileshare\JEA` ファイル共有に配置されていることを前提にしています。

```powershell
Configuration JEAMaintenance
{
    Import-DscResource -Module JustEnoughAdministration, PSDesiredStateConfiguration

    File MaintenanceModule
    {
        SourcePath = "\\myfileshare\JEA\ContosoMaintenance"
        DestinationPath = "C:\Program Files\WindowsPowerShell\Modules\ContosoMaintenance"
        Checksum = "SHA-256"
        Ensure = "Present"
        Type = "Directory"
        Recurse = $true
    }

    JeaEndpoint JEAMaintenanceEndpoint
    {
        EndpointName = "JEAMaintenance"
        RoleDefinitions = "@{ 'CONTOSO\JEAMaintenanceAuditors' = @{ RoleCapabilities = 'GeneralServerMaintenance-Audit' }; 'CONTOSO\JEAMaintenanceAdmins' = @{ RoleCapabilities = 'GeneralServerMaintenance-Audit', 'GeneralServerMaintenance-Admin' } }"
        TranscriptDirectory = 'C:\ProgramData\JEAConfiguration\Transcripts'
        DependsOn = '[File]MaintenanceModule'
    }
}
```

その後、[ローカル構成マネージャー](/powershell/scripting/dsc/managing-nodes/metaConfig)を直接呼び出すことにより、または[プル サーバーの構成](/powershell/scripting/dsc/pull-server/pullServer)を更新することにより、この構成がシステムに適用されます。

DSC リソースを使うと、既定の **Microsoft.PowerShell** エンドポイントを置き換えることもできます。 置き換えると、リソースで **Microsoft.PowerShell.Restricted** という名前のバックアップ エンドポイントが自動的に登録されます。 バックアップ エンドポイントには、リモート管理ユーザーとローカルの Administrators グループのメンバーのアクセスを許可する既定の WinRM ACL があります。

## <a name="unregistering-jea-configurations"></a>JEA の構成の登録解除

[Unregister-PSSessionConfiguration](/powershell/module/microsoft.powershell.core/Unregister-PSSessionConfiguration) コマンドレットは、JEA エンドポイントを削除します。 JEA エンドポイントの登録を解除すると、新しいユーザーがシステムに新しい JEA セッションを作成できなくなります。 また、同じエンドポイント名を使って更新されたセッション構成ファイルを再登録することで、JEA 構成を更新することもできます。

```powershell
# Unregister the JEA endpoint called "ContosoMaintenance"
Unregister-PSSessionConfiguration -Name 'ContosoMaintenance' -Force
```

> [!WARNING]
> JEA エンドポイントの登録を解除すると、WinRM サービスが再起動されます。 これにより、他の PowerShell セッション、WMI 呼び出し、一部の管理ツールなど、実行中のほとんどのリモート管理操作が中断されます。 計画されたメンテナンス期間中にのみ、PowerShell エンドポイントの登録を解除してください。

## <a name="next-steps"></a>次のステップ

[JEA エンドポイントをテストする](using-jea.md)
