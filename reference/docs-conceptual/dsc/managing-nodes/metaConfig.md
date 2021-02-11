---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: ローカル構成マネージャーの構成
description: ローカル構成マネージャー (LCM) は、ノードに送信された構成の解析と適用を担当する、DSC のエンジンです。
ms.openlocfilehash: 73711536d419cd0305d541e3be23195ae6b91782
ms.sourcegitcommit: 61765d08321623743dc5db5367160f6982fe7857
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2020
ms.locfileid: "97352690"
---
# <a name="configuring-the-local-configuration-manager"></a>ローカル構成マネージャーの構成

> 適用先:Windows PowerShell 5.0

ローカル構成マネージャー (LCM) は、Desired State Configuration (DSC) のエンジンです。 LCM は、すべてのターゲット ノード上で実行され、ノードに送信される構成の解析と適用を担当します。 次のような DSC のその他のさまざまな側面も担当します。

- 更新モード (プッシュまたはプル) の決定。
- ノードで構成をプルし、適用する頻度の指定。
- プル サービスへのノードの関連付け。
- 部分構成の指定。

特殊な種類の構成を使用して、これらの各動作を指定するように LCM を構成します。 ここでは、LCM を構成する方法について説明します。

Windows PowerShell 5.0 には、ローカル構成マネージャーを管理するための新しい設定が導入されました。 Windows PowerShell 4.0 で LCM を構成する方法については、[以前のバージョンの Windows PowerShell でのローカル構成マネージャーの構成](metaconfig4.md)に関するページを参照してください。

## <a name="writing-and-enacting-an-lcm-configuration"></a>LCM 構成の作成と適用

LCM を構成するには、LCM 設定に適用する特殊な種類の構成を作成して実行します。
LCM 構成を指定するには、DscLocalConfigurationManager 属性を使用します。 LCM をプッシュ モードに設定する単純な構成を次に示します。

```powershell
[DSCLocalConfigurationManager()]
configuration LCMConfig
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Push'
        }
    }
}
```

LCM に設定を適用する手順は、DSC 構成を適用する場合と同様です。 LCM 構成を作成して MOF ファイルにコンパイルし、ノードに適用します。 ただし、DSC 構成とは異なり、LCM 構成の適用は [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを呼び出すことでは行いません。 代わりに、パラメーターとして LCM 構成の MOF のパスを指定して、[Set-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Set-DscLocalConfigurationManager) を呼び出します。 LCM 構成を適用した後で、[Get-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Get-DscLocalConfigurationManager) コマンドレットを呼び出すと LCM のプロパティを確認できます。

LCM 構成には、限定されたリソースのセットに対するブロックのみを含めることができます。 前の例では、呼び出されたリソースは、**Settings** のみです。 その他の使用可能なリソースは次のとおりです。

- **ConfigurationRepositoryWeb**: 構成の HTTP プル サービスを指定します。
- **ConfigurationRepositoryShare**: 構成の SMB 共有を指定します。
- **ResourceRepositoryWeb**: モジュールの HTTP プル サービスを指定します。
- **ResourceRepositoryShare**: モジュールの SMB 共有を指定します。
- **ReportServerWeb**: レポートの送信先となる HTTP プル サービスを指定します。
- **PartialConfiguration**: 部分構成を有効にするデータを指定します。

## <a name="basic-settings"></a>[基本設定]

プル サービスのエンドポイントおよびパスと部分構成の指定を除き、LCM 構成のすべてのプロパティは **Settings** ブロックで構成します。 **Settings** ブロックでは、次のプロパティを使用できます。

|  プロパティ  |  Type  |  説明   |
|----------- |------- |--------------- |
| ActionAfterReboot| string| 構成の適用中の再起動後の動作を指定します。 指定できる値は __"ContinueConfiguration"__ と __"StopConfiguration"__ です。 <ul><li> __ContinueConfiguration__: コンピューターの再起動後、現在の構成を引き続き適用します。 これは、既定値です。</li><li>__StopConfiguration__: コンピューターの再起動後、現在の構成の適用を停止します。</li></ul>|
| AllowModuleOverwrite| [bool]| プル サービスからダウンロードされた新しい構成でのターゲット ノードの古い構成の上書きを許可する場合は、 __$TRUE__。 それ以外の場合は、$FALSE。|
| CertificateID| string| 構成で渡される資格情報をセキュリティで保護するために使用される証明書の拇印。 詳細については、「[Want to secure credentials in Windows PowerShell Desired State Configuration? (Windows PowerShell Desired State Configuration で資格情報をセキュリティ保護する)](https://devblogs.microsoft.com/powershell/want-to-secure-credentials-in-windows-powershell-desired-state-configuration/)」を参照してください。 <br> __注:__ Azure Automation DSC プル サービスを使用している場合、このプロパティは自動で管理されます。|
| ConfigurationDownloadManagers| CimInstance[]| 互換性のために残されています。 構成プル サービスのエンドポイントを定義するには、__ConfigurationRepositoryWeb__ ブロックと __ConfigurationRepositoryShare__ ブロックを使用します。|
| ConfigurationID| string| 旧バージョンのプル サービスとの互換性用。 プル サービスから取得する構成ファイルを識別する GUID。 構成 MOF の名前が ConfigurationID.mof の場合、ノードはプル サービスで構成をプルします。<br> __注:__ このプロパティを設定した場合、__RegistrationKey__ を使用してプル サービスへノードを登録することはできません。 詳細については、「[構成名を使用したプル クライアントのセットアップ](../pull-server/pullClientConfigNames.md)」をご覧ください。|
| ConfigurationMode| string | LCM が実際に構成をターゲット ノードに適用する方法を指定します。 指定できる値は __"ApplyOnly"__ 、 __"ApplyAndMonitior"__ 、 __"ApplyAndAutoCorrect"__ です。 <ul><li>__ApplyOnly__:DSC によって構成が適用され、それ以上は何も行われません。ただし、ターゲット ノードに新しい構成がプッシュされた場合、または新しい構成がサービスからプルされた場合を除きます。 新しい構成が最初に適用された後、DSC は以前の構成された状態からの誤差を確認しません。 DSC は成功するまで構成の適用を試みて、成功すると __ApplyOnly__ が有効になります。 </li><li> __ApplyAndMonitor__:これが既定値です。 LCM が任意の新しい構成を適用します。 新しい構成が最初に適用された後、ターゲット ノードが目的の状態から変わった場合、DSC はログに不一致を報告します。 DSC は成功するまで構成の適用を試みて、成功すると __ApplyAndMonitor__ が有効になります。</li><li>__ApplyAndAutoCorrect__:DSC によって新しい構成が適用されます。 新しい構成を最初に適用した後、ターゲット ノードが望ましい状態からずれた場合、DSC では、ログで不一致を報告し、現在の構成を再度適用します。</li></ul>|
| ConfigurationModeFrequencyMins| UInt32| 現在の構成がチェックおよび適用される頻度 (分単位) ConfigurationMode プロパティが ApplyOnly に設定されている場合、このプロパティは無視されます。 既定値は 15 です。|
| DebugMode| string| 指定できる値は __None__、__ForceModuleImport__、および __All__ です。 <ul><li>キャッシュされたリソースを使用する場合は、__None__ に設定します。 これが既定値であり、運用シナリオではこの値を使う必要があります。</li><li>__ForceModuleImport__ に設定すると、以前に読み込まれ、キャッシュされた DSC リソース モジュールも LCM によって再読み込みされます。 これは、使用時に各モジュールが再読み込みされるため、DSC 操作のパフォーマンスに影響します。 通常、リソースのデバッグ中には、この値を使用します</li><li>このリリースでは、__All__ は、__ForceModuleImport__ と同じです。</li></ul> |
| RebootNodeIfNeeded| [bool]| これを `$true` に設定して、リソースにより `$global:DSCMachineStatus` フラグを使用したノードが再起動されるようにします。 設定しない場合は、再起動が必要な構成のノードを手動で再起動する必要があります。 既定値は `$false` です。 DSC 以外 (Windows インストーラーなど) で再起動の条件が有効化されている場合にこの設定を使用するには、この設定を [ComputerManagementDsc](https://github.com/PowerShell/ComputerManagementDsc) モジュールの __PendingReboot__ リソースと併用します。|
| RefreshMode| string| LCM が構成を取得する方法を指定します。 指定できる値は、 __"Disabled"__ 、 __"Push"__ 、 __"Pull"__ です。 <ul><li>__Disabled__: このノードの DSC 構成が無効になります。</li><li> __Push__: [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを呼び出すことによって構成を開始します。 構成は、ノードにすぐに適用されます。 これが既定値です。</li><li>__Pull:__ プル サービスまたは SMB パスで構成を定期的にチェックするようにノードを構成します。 このプロパティを __Pull__ に設定する場合、__ConfigurationRepositoryWeb__ ブロックまたは __ConfigurationRepositoryShare__ ブロックで HTTP (サービス) または SMB (共有) パスを指定する必要があります。</li></ul>|
| RefreshFrequencyMins| Uint32| LCM がプル サービスをチェックして最新の構成を取得し、ドリフトがないかローカル構成を確認する時間間隔 (分)。 この構成は、更新プログラムがダウンロードされたかどうかに関係なく、適用されます。 この値は、LCM がプル モードで構成されていない場合は無視されます。 既定値は 30 です。|
| ReportManagers| CimInstance[]| 互換性のために残されています。 プル サービスへデータをレポートするエンドポイントを定義するには、__ReportServerWeb__ ブロックを使用します。|
| ResourceModuleManagers| CimInstance[]| 互換性のために残されています。 プル サービスの HTTP エンドポイントまたは SMB パスを定義するには、__ResourceRepositoryWeb__ ブロックまたは __ResourceRepositoryShare__ ブロックをそれぞれ使用します。|
| PartialConfigurations| CimInstance| 実装されていません。 使用しないでください。|
| StatusRetentionTimeInDays | UInt32| LCM が現在の構成の状態を保持する日数。|

> [!NOTE]
> LCM は次に基づいて **ConfigurationModeFrequencyMins** サイクルを開始します。
>
> - **ConfigurationModeFrequencyMins** への変更が含まれた新しい metaconfig は、`Set-DscLocalConfigurationManager` を使用して適用されます
> - コンピューターの再起動
>
> タイマー プロセスでクラッシュが発生するすべての状況で、それが 30 秒以内に検出され、サイクルが再開されます。 同時実行操作によって、サイクルの開始が遅延する可能性があり、この操作の期間が構成済みのサイクル頻度を超えた場合、次のタイマーは開始されません。 たとえば、メタ構成が 15 分のプル頻度で構成されており、プルが T1 で発生するとします。 ノードにより 16 分間で作業が完了されません。 最初の 15 分のサイクルは無視され、次のプルが T1 + 15 + 15 で発生します。
>
> プル シナリオの本来の意図は、`RefreshFrequencyMins` を `ConfigurationModeFrequencyMins` より長い時間に設定することでした。 構成のドリフトを回避するため、ローカル構成は主に `ConfigurationModeFrequencyMins` によって管理され、管理者が行う実際の構成変更を追跡記録する目的で `RefreshFrequencyMins` が使用されます。

## <a name="pull-service"></a>プル サービス

LCM 構成では、次の種類のプル サービス エンドポイントを定義できます。

- **構成サーバー**: DSC 構成のリポジトリ。 **ConfigurationRepositoryWeb** (Web ベースのサーバーの場合) ブロックと **ConfigurationRepositoryShare** (SMB ベースのサーバーの場合) ブロックを使用して、構成サーバーを定義します。
- **リソース サーバー**: PowerShell モジュールとしてパッケージ化された DSC リソースのリポジトリ。 **ResourceRepositoryWeb** (Web ベースのサーバーの場合) ブロックと **ResourceRepositoryShare** (SMB ベースのサーバーの場合) ブロックを使用して、リソース サーバーを定義します。
- **レポート サーバー**: DSC によってレポート データが送信される先のサービス。 **ReportServerWeb** ブロックを使用して、レポート サーバーを定義します。 レポート サーバーは、Web サービスである必要があります。

プル サービスの詳細については、[Desired State Configuration プル サービス](../pull-server/pullServer.md)に関するページを参照してください。

## <a name="configuration-server-blocks"></a>構成サーバーのブロック

Web ベースの構成サーバーを定義するには、**ConfigurationRepositoryWeb** ブロックを作成します。 **ConfigurationRepositoryWeb** は次のプロパティを定義します。

|プロパティ|Type|説明|
|---|---|---|
|AllowUnsecureConnection|[bool]|認証なしのノードからサーバーへの接続を許可するには、 **$TRUE** に設定します。 認証を要求するには、 **$FALSE** に設定します。|
|CertificateID|string|サーバーへの認証に使用される証明書の拇印。|
|ConfigurationNames|String[]|ターゲット ノードによってプルされる構成の名前の配列。 ノードが **RegistrationKey** を使用してプル サービスに登録されている場合にのみ使用します。 詳細については、「[構成名を使用したプル クライアントのセットアップ](../pull-server/pullClientConfigNames.md)」をご覧ください。|
|RegistrationKey|string|プル サービスにノードを登録する GUID。 詳細については、「[構成名を使用したプル クライアントのセットアップ](../pull-server/pullClientConfigNames.md)」をご覧ください。|
|ServerURL|string|構成サービスの URL。|
|ProxyURL*|string|構成サービスと通信するときに使用する http プロキシの URL。|
|ProxyCredential*|pscredential|http プロキシに使用する資格情報。|

> [!NOTE]
> Windows バージョン 1809 以降でサポートされています。

オンプレミス ノードの ConfigurationRepositoryWeb 値の設定を簡単に行うサンプル スクリプトが用意されています。「[DSC メタ構成の生成](/azure/automation/automation-dsc-onboarding#generating-dsc-metaconfigurations)」を参照してください。

SMB ベースの構成サーバーを定義するには、**ConfigurationRepositoryShare** ブロックを作成します。 **ConfigurationRepositoryShare** は次のプロパティを定義します。

|  プロパティ  |      Type       |                      説明                      |
| ---------- | --------------- | ----------------------------------------------------- |
| 資格情報 | MSFT_Credential | SMB 共有への認証に使用される資格情報。 |
| SourcePath | string          | SMB 共有のパス。                            |

## <a name="resource-server-blocks"></a>リソース サーバーのブロック

Web ベースのリソース サーバーを定義するには、**ResourceRepositoryWeb** ブロックを作成します。
**ResourceRepositoryWeb** は次のプロパティを定義します。

|        プロパティ         |     Type     |                                                              説明                                                               |
| ----------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| AllowUnsecureConnection | [bool]         | 認証なしのノードからサーバーへの接続を許可するには、 **$TRUE** に設定します。 認証を要求するには、 **$FALSE** に設定します。 |
| CertificateID           | string       | サーバーへの認証に使用される証明書の拇印。                                                                    |
| RegistrationKey         | string       | プル サービスにノードを指定する GUID。                                                                                   |
| ServerURL               | string       | 構成サーバーの URL。                                                                                                   |
| ProxyURL*               | string       | 構成サービスと通信するときに使用する http プロキシの URL。                                                    |
| ProxyCredential*        | pscredential | http プロキシに使用する資格情報。                                                                                                  |

> [!NOTE]
> Windows バージョン 1809 以降でサポートされています。

オンプレミス ノードの ResourceRepositoryWeb 値の設定を簡単に行うサンプル スクリプトが用意されています。「[DSC メタ構成の生成](/azure/automation/automation-dsc-onboarding#generating-dsc-metaconfigurations)」を参照してください。

SMB ベースのリソース サーバーを定義するには、**ResourceRepositoryShare** ブロックを作成します。
**ResourceRepositoryShare** は次のプロパティを定義します。

|プロパティ|Type|説明|
|---|---|---|
|資格情報|MSFT_Credential|SMB 共有への認証に使用される資格情報。 資格情報を渡す例については、「[DSC SMB プル サーバーのセットアップ](../pull-server/pullServerSMB.md)」をご覧ください。|
|SourcePath|string|SMB 共有のパス。|

## <a name="report-server-blocks"></a>レポート サーバーのブロック

レポート サーバーを定義するには、**ReportServerWeb** ブロックを作成します。 レポート サーバーの役割には、SMB ベースのプル サービスとの互換性はありません。 **ReportServerWeb** は次のプロパティを定義します。

|        プロパティ         |     Type     |                                                              説明                                                               |
| ----------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| AllowUnsecureConnection | [bool]         | 認証なしのノードからサーバーへの接続を許可するには、 **$TRUE** に設定します。 認証を要求するには、 **$FALSE** に設定します。 |
| CertificateID           | string       | サーバーへの認証に使用される証明書の拇印。                                                                    |
| RegistrationKey         | string       | プル サービスにノードを指定する GUID。                                                                                   |
| ServerURL               | string       | 構成サーバーの URL。                                                                                                   |
| ProxyURL*               | string       | 構成サービスと通信するときに使用する http プロキシの URL。                                                    |
| ProxyCredential*        | pscredential | http プロキシに使用する資格情報。                                                                                                  |

> [!NOTE]
> Windows バージョン 1809 以降でサポートされています。

オンプレミス ノードの ReportServerWeb 値の設定を簡単に行うサンプル スクリプトが用意されています。「[DSC メタ構成の生成](/azure/automation/automation-dsc-onboarding#generating-dsc-metaconfigurations)」を参照してください。

## <a name="partial-configurations"></a>部分構成

部分構成を定義するには、**PartialConfiguration** ブロックを作成します。 部分構成の詳細については、「[PowerShell Desired State Configuration の部分構成](../pull-server/partialConfigs.md)」をご覧ください。
**PartialConfiguration** は次のプロパティを定義します。

|プロパティ|Type|説明|
|---|---|---|
|ConfigurationSource|string[]|**ConfigurationRepositoryWeb** および **ConfigurationRepositoryShare** ブロックで以前に定義した、部分構成をプルする構成サーバーの名前の配列。|
|DependsOn|string{}|この部分構成が適用される前に完了する必要があるその他の構成の名前の一覧。|
|説明|string|部分構成を記述するために使用するテキスト。|
|ExclusiveResources|string[]|この部分構成に固有のリソースの配列。|
|RefreshMode|string|LCM がこの部分構成を取得する方法を指定します。 指定できる値は、 __"Disabled"__ 、 __"Push"__ 、 __"Pull"__ です。 <ul><li>__Disabled__: この部分的な構成が無効になります。</li><li> __Push__: [Publish-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Publish-DscConfiguration) コマンドレットを呼び出すと、部分構成がノードにプッシュされます。 ノードのすべての部分構成がプッシュされたか、またはサービスからプルされた後、`Start-DscConfiguration –UseExisting` を呼び出すことで構成を開始できます。 これが既定値です。</li><li>__Pull:__ プル サービスで部分構成を定期的にチェックするようにノードを構成します。 このプロパティを __Pull__ に設定する場合、__ConfigurationSource__ プロパティでプル サービスを指定する必要があります。 Azure Automation プル サービスの詳細については、「[Azure Automation DSC Overview](/azure/automation/automation-dsc-overview)」を参照してください。</li></ul>|
|ResourceModuleSource|string[]|この部分構成に必要なリソースのダウンロード元となるリソース サーバーの名前の配列。 これらの名前では、**ResourceRepositoryWeb** ブロックおよび **ResourceRepositoryShare** ブロックで以前に定義したサービス エンドポイントを参照する必要があります。|

> [!NOTE]
> 部分構成は Azure Automation DSC でサポートされていますが、各 Automation アカウントからプルできる構成はノードごとに 1 つだけです。

## <a name="see-also"></a>参照

### <a name="concepts"></a>概念

[Desired State Configuration の概要](../overview/overview.md)

[Azure Automation DSC の使用](/azure/automation/automation-dsc-getting-started)

### <a name="other-resources"></a>その他のリソース

[Set-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Set-DscLocalConfigurationManager)

[構成名を使用したプル クライアントのセットアップ](../pull-server/pullClientConfigNames.md)
