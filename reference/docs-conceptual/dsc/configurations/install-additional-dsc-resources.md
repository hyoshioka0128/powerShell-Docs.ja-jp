---
ms.date: 12/12/2018
keywords: dsc,powershell,リソース,ギャラリー,セットアップ
title: 追加の DSC リソースをインストールする
description: この記事では、PSDesiredStateConfiguration モジュールに含まれている DSC リソースの一覧を示します。 また、PowerShell ギャラリーからリソースを検索してインストールする方法についても説明します。
ms.openlocfilehash: e75561ed539e06716c9a103f905b9d1e4f3e71d3
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92645131"
---
# <a name="install-additional-dsc-resources"></a>追加の DSC リソースをインストールする

PowerShell には、Desired State Configuration (DSC) 用に複数のそのまま利用できるリソースが含まれています。 **PSDesiredStateConfiguration** モジュールには、PowerShell の特定のインスタンスで使用可能なすべての OOB DSC リソースが含まれます。

次に示すのは、PowerShell 4.0 に含まれる OOB リソースの一覧とリソースの機能の説明です。

> [!NOTE]
> PowerShell のバージョンごとに OOB リソースの数は増えているため、これは完全な一覧ではありません。

|      リソース      |                                                                                       説明                                                                                        |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[最近使ったファイル]**           | ファイルとディレクトリの状態を制御します。 **コピー元** から **コピー先** にファイルをコピーし、日付、チェックサム、およびハッシュを比較することによって **コピー元** が変更されたらファイルを更新します。 |
| **Archive**        | アーカイブと指定された場所をアンパックします。 指定された **チェックサム** でアーカイブを検証します。                                                                                         |
| **Environment**    | 環境変数を管理します。                                                                                                                                                           |
| **グループ**          | ローカル グループを管理し、グループ メンバーシップを制御します。                                                                                                                                      |
| **Log**            | `Microsoft-Windows-Desired State Configuration/Analytic` イベント ログにメッセージを書き込みます。                                                                                               |
| **Package**        | **Arguments** 、 **LogPath** 、 **ReturnCode** 、その他の設定を使って、パッケージをインストールまたはアンインストールします。                                                                                        |
| **レジストリ**       | レジストリ キーと値を管理します。                                                                                                                                                        |
| **スクリプト**         | 独自の [get-test-set](../resources/get-test-set.md) スクリプト ブロックを設計できます。                                                                                                |
| **サービス**        | Windows サービスを構成します。                                                                                                                                                             |
| **User**           | ローカル ユーザーと属性を管理します。                                                                                                                                                      |
| **WindowsFeature** | ロールと機能を管理します。                                                                                                                                                              |
| **WindowsProcess** | Windows プロセスを構成します。                                                                                                                                                            |

OOB リソースは、一般的な操作のよい起点になります。 OOB リソースがニーズを満たさない場合は、独自の[カスタム リソース](../resources/authoringResource.md)を作成できます。 問題を解決するためのカスタム リソースを記述する前に、Microsoft と PowerShell コミュニティの両方で既に作成されている膨大な数の DSC リソースを調べる必要があります。

[PowerShell ギャラリー](https://www.powershellgallery.com/)と [GitHub](https://github.com/) の両方で、DSC リソースを見つけることができます。 また、[PowerShellGet](/powershell/module/powershellget/) を使って PowerShell コンソールから DSC リソースを直接インストールすることもできます。

## <a name="installing-powershellget"></a>PowerShellGet のインストール

**PowerShellGet** が既にあるかどうかを調べたり、そのインストールに関するヘルプ情報を見たりするには、次のガイドを参照してください: 「[PowerShellGet のインストール](/powershell/scripting/gallery/installing-psget)」。

## <a name="finding-dsc-resources-using-powershellget"></a>PowerShellGet を使用して DSC リソースを検索する

**PowerShellGet** をシステムにインストールした後は、 [PowerShell ギャラリー](https://www.powershellgallery.com/)でホストされている DSC リソースを検索してインストールできます。

最初に、[Find-DSCResource](/powershell/module/powershellget/find-dscresource) コマンドレットを使って DSC リソースを検索します。 `Find-DSCResource` を初めて実行すると、"NuGet プロバイダー" のインストールを求める次のメッセージが表示されます。

```
PS> Find-DSCResource

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based
repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies'
or 'C:\Users\xAdministrator\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install
the NuGet provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201
-Force'. Do you want PowerShellGet to install and import the NuGet provider now?
[Y] Yes  [N] No  [?] Help (default is "Y"):
```

Y キーを押すと、"NuGet" プロバイダーがインストールされて、PowerShell ギャラリーからインストールできる DSC リソースの一覧が表示されます。

> [!NOTE]
> 一覧は非常に大きいため示してありません。

ワイルドカードを使って `-Name` パラメーターを指定するか、またはワイルドカードを使わないで `-Filter` パラメーターを指定して、検索を絞り込むこともできます。 次の例では、ワイルドカードを使って "TimeZone" DSC リソースを検索しています。

> [!IMPORTANT]
> 現在は `Find-DSCResource` コマンドレットにバグがあり、`-Name` と `-Filter` 両方のパラメーターでワイルドカードを使うことはできません。 次の 2 番目の例では、`Where-Object` を使った回避策を示します。

```
PS> Find-DSCResource -Name *Time*

Name                                Version    ModuleName                          Repository
----                                -------    ----------                          ----------
Carbon_EnvironmentVariable          2.6.0      Carbon                              PSGallery
Carbon_FirewallRule                 2.6.0      Carbon                              PSGallery
Carbon_Group                        2.6.0      Carbon                              PSGallery
Carbon_IniFile                      2.6.0      Carbon                              PSGallery
Carbon_Permission                   2.6.0      Carbon                              PSGallery
Carbon_Privilege                    2.6.0      Carbon                              PSGallery
Carbon_ScheduledTask                2.6.0      Carbon                              PSGallery
Carbon_Service                      2.6.0      Carbon                              PSGallery
TimeZone                            6.0.0.0    ComputerManagementDsc               PSGallery
xTimeZone                           1.8.0.0    xTimeZone                           PSGallery
xSqlServerDefaultDir                1.0.0      mlSqlServerDSC                      PSGallery
xSqlServerMoveDatabaseFiles         1.0.0      mlSqlServerDSC                      PSGallery
xSqlServerSQLDataRoot               1.0.0      mlSqlServerDSC                      PSGallery
xSqlServerStartupParam              1.0.0      mlSqlServerDSC                      PSGallery
```

`Where-Object` を使ってさらに詳細なフィルターで DSC リソースを検索することもできます。 この方法では、組み込みのフィルター処理パラメーターを使うより処理が遅くなります。

```
PS> Find-DSCResource | Where-Object {$_.Name -like "Time*"}

Name                                Version    ModuleName                          Repository
----                                -------    ----------                          ----------
TimeZone                            6.0.0.0    ComputerManagementDsc               PSGallery
```

フィルター処理について詳しくは、「[Where-Object](/powershell/module/microsoft.powershell.core/where-object)」をご覧ください。

## <a name="installing-dsc-resources-using-powershellget"></a>PowerShellGet を使用して DSC リソースをインストールする

DSC リソースをインストールするには、 [Install-Module](/powershell/module/PowershellGet/Install-Module) コマンドレットを使い、検索結果の **[ModuleName]** の下に表示されるモジュール名を指定します。

"TimeZone" リソースは "ComputerManagementDSC" モジュールに存在するので、この例ではそのモジュールをインストールします。

> [!NOTE]
> PowerShell ギャラリーを信頼していない場合は、確認を要求し、以降のインストールでメッセージが表示されないようにする方法を示す、次のような警告が表示されます。

```
PS> Install-Module -Name ComputerManagementDSC

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change
its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to
install the modules from 'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

Y キーを押して、モジュールのインストールを続行します。 インストール後は、[Get-DSCResource](/powershell/module/PSDesiredStateConfiguration/Get-DscResource) を使って、新しいリソースがインストールされたことを確認できます。

```
PS> Get-DSCResource -Name TimeZone -Syntax

TimeZone [String] #ResourceName
{
    IsSingleInstance = [string]{ Yes }
    TimeZone = [string]
    [DependsOn = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
}
```

また、`-ModuleName` パラメーターを指定することで、新しくインストールしたモジュール内の他のリソースを表示することもできます。

```
PS> Get-DSCResource -Module ComputerManagementDSC

ImplementedAs   Name                      ModuleName                     Version    Properties
-------------   ----                      ----------                     -------    ----------
PowerShell      Computer                  ComputerManagementDsc          6.0.0.0    {Name, Credential, DependsOn, ...
PowerShell      OfflineDomainJoin         ComputerManagementDsc          6.0.0.0    {IsSingleInstance, RequestFile...
PowerShell      PowerPlan                 ComputerManagementDsc          6.0.0.0    {IsSingleInstance, Name, Depen...
PowerShell      PowerShellExecutionPolicy ComputerManagementDsc          6.0.0.0    {ExecutionPolicy, ExecutionPol...
PowerShell      ScheduledTask             ComputerManagementDsc          6.0.0.0    {TaskName, ActionArguments, Ac...
PowerShell      TimeZone                  ComputerManagementDsc          6.0.0.0    {IsSingleInstance, TimeZone, D...
PowerShell      VirtualMemory             ComputerManagementDsc          6.0.0.0    {Drive, Type, DependsOn, Initi...
```

## <a name="see-also"></a>関連項目

- [リソースとは](../resources/resources.md)
