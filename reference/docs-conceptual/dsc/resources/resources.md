---
ms.date: 07/23/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC リソース
description: DSC リソースには、DSC 構成の構成要素が用意されています。 リソースでは構成可能なプロパティ (スキーマ) が公開され、構成を適用するために LCM によって使用される PowerShell スクリプト関数が含まれています。
ms.openlocfilehash: 33268c68638bb581e0b2235a53aee9d186dff6be
ms.sourcegitcommit: 0f003644684422e425a59b7361121e05ac772e15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2021
ms.locfileid: "98771789"
---
# <a name="dsc-resources"></a>DSC リソース

> 適用対象: Windows PowerShell 4.0 以上。

## <a name="overview"></a>概要

Desired State Configuration (DSC) リソースは、DSC 構成の構成要素を提供します。 リソースは、構成 (スキーマ) 可能なプロパティを公開し、また、指定されたことを実現するためにローカル構成マネージャー (LCM) が呼び出す PowerShell スクリプト関数が含まれています。

リソースでは、ファイルのように汎用的なものをモデル化したり、IIS サーバー設定のように具体的なものをモデル化したりできます。 リソースなどのグループは、DSC モジュールに結合されます。DSC モジュールでは、リソースの使用目的を識別するためのメタデータが含まれている移植可能な構造にすべての必須ファイルが編成されます。

各リソースには、[Configuration](../configurations/configurations.md) でリソースを使用するために必要な構文を決定する*スキーマがあります。 リソースのスキーマは、次のように定義できます。

- `Schema.Mof` ファイル:ほとんどのリソースでは、[マネージド オブジェクト フォーマット](/windows/desktop/wmisdk/managed-object-format--mof-)を使用して、`schema.mof` ファイルに "_スキーマ_" を定義します。
- `<Resource Name>.schema.psm1` ファイル:[複合リソース](../configurations/compositeConfigs.md)では、[パラメーター ブロック](/powershell/module/microsoft.powershell.core/about/about_functions#functions-with-parameters)を使用して、`<ResourceName>.schema.psm1` ファイルに _スキーマ_ を定義します。
- `<Resource Name>.psm1` ファイル:クラス ベースの DSC リソースは、クラス定義で "_スキーマ_" を定義します。 構文の項目は Class プロパティとして表されます。 詳細については、「[about_Remote](/powershell/module/psdesiredstateconfiguration/about/about_classes_and_dsc)」を参照してください。

DSC リソースの構文を取得するには、**Syntax** パラメーターを指定して、[Get-DSCResource](/powershell/module/PSDesiredStateConfiguration/Get-DscResource) コマンドレットを使用します。 この使用法は、**Syntax** パラメーターを指定して、[Get-Command](/powershell/module/microsoft.powershell.core/get-command) を使用して、コマンドレットの構文を取得する場合と似ています。 表示される出力には、指定したリソースのリソース ブロックに使用されているテンプレートが示されます。

```powershell
Get-DscResource -Syntax Service
```

このリソースの構文は今後変更される可能性がありますが、表示される出力は以下ような出力になります。 コマンドレットの構文と同様に、角かっこで囲まれた "_キー_" は省略可能です。 型には、各キーで想定されているデータの型を指定します。

> [!NOTE]
> **Ensure** キーは既定値が "Present" なので省略可能です。

```output
Service [String] #ResourceName
{
    Name = [string]
    [BuiltInAccount = [string]{ LocalService | LocalSystem | NetworkService }]
    [Credential = [PSCredential]]
    [Dependencies = [string[]]]
    [DependsOn = [string[]]]
    [Description = [string]]
    [DisplayName = [string]]
    [Ensure = [string]{ Absent | Present }]
    [Path = [string]]
    [PsDscRunAsCredential = [PSCredential]]
    [StartupType = [string]{ Automatic | Disabled | Manual }]
    [State = [string]{ Running | Stopped }]
}
```

> [!NOTE]
> PowerShell バージョン 7.0 より前のバージョンでは、`Get-DscResource` によるクラス ベースの DSC リソースの検出は行われません。

Configuration 内では、スプーラー サービスが実行されていることを確認 (**Ensure**) するため、**Service** リソース ブロックを次のようにすることができます。

> [!NOTE]
> Configuration でリソースを使用する前に、[Import-DSCResource](../configurations/import-dscresource.md) を使用してリソースをインポートする必要があります。

```powershell
Configuration TestConfig
{
    # It is best practice to always directly import resources, even if the
    # resource is a built-in resource.
    Import-DSCResource -Name Service
    Node localhost
    {
        # The name of this resource block, can be anything you choose, as l
        # ong as it is of type [String] as indicated by the schema.
        Service "Spooler:Running"
        {
            Name = "Spooler"
            State = "Running"
        }
    }
}
```

Configuration には、同じリソースの種類の複数インスタンスを含めることができます。 各インスタンスには一意の名前を付ける必要があります。 次の例では、"DHCP" サービスを構成するために 2 つ目の **Service** リソース ブロックが追加されています。

```powershell
Configuration TestConfig
{
    # It is best practice to always directly import resources, even if the
    # resource is a built-in resource.
    Import-DSCResource -Name Service
    Node localhost
    {
        # The name of this resource block, can be anything you choose, as
        # long as it is of type [String] as indicated by the schema.
        Service "Spooler:Running"
        {
            Name = "Spooler"
            State = "Running"
        }

        # To configure a second service resource block, add another Service
        # resource block and use a unique name.
        Service "DHCP:Running"
        {
            Name = "DHCP"
            State = "Running"
        }
    }
}
```

> [!NOTE]
> PowerShell 5.0 以降、IntelliSense が DSC に追加されました。 この新機能を使用すると、<kbd>Tab</kbd> キーと <kbd>Ctrl</kbd>+<kbd>Space</kbd> キーを使用してキー名をオートコンプリートすることができます。

![Tab 補完機能を使用したリソース IntelliSense](media/resources/resource-tabcompletion.png)

## <a name="types-of-resources"></a>リソースの種類

Windows には組み込みリソースが付属しており、Linux には OS 固有のリソースがあります。 [ノード間の依存関係](../configurations/crossNodeDependencies.md)のリソース、パッケージ管理リソース、および[コミュニティ所有の管理リソース](https://github.com/dsccommunity)があります。 前述の手順を使用して、これらのリソースの構文とその使用方法を判断できます。 これらのリソースを提供するページは、「**関連項目**」にアーカイブされています。

### <a name="windows-built-in-resources"></a>Windows 組み込みリソース

- [アーカイブ リソース](../reference/resources/windows/archiveResource.md)
- [環境リソース](../reference/resources/windows/environmentResource.md)
- [ファイル リソース](../reference/resources/windows/fileResource.md)
- [グループ リソース](../reference/resources/windows/groupResource.md)
- [GroupSet リソース](../reference/resources/windows/groupSetResource.md)
- [ログ リソース](../reference/resources/windows/logResource.md)
- [パッケージ リソース](../reference/resources/windows/packageResource.md)
- [ProcessSet リソース](../reference/resources/windows/ProcessSetResource.md)
- [レジストリ リソース](../reference/resources/windows/registryResource.md)
- [スクリプト リソース](../reference/resources/windows/scriptResource.md)
- [サービス リソース](../reference/resources/windows/serviceResource.md)
- [ServiceSet リソース](../reference/resources/windows/serviceSetResource.md)
- [ユーザー リソース](../reference/resources/windows/userResource.md)
- [WindowsFeature リソース](../reference/resources/windows/windowsFeatureResource.md)
- [WindowsFeatureSet リソース](../reference/resources/windows/windowsFeatureSetResource.md)
- [WindowsOptionalFeature リソース](../reference/resources/windows/windowsOptionalFeatureResource.md)
- [WindowsOptionalFeatureSet リソース](../reference/resources/windows/windowsOptionalFeatureSetResource.md)
- [WindowsPackageCabResource リソース](../reference/resources/windows/windowsPackageCabResource.md)
- [WindowsProcess リソース](../reference/resources/windows/windowsProcessResource.md)

### <a name="cross-node-dependency-resources"></a>ノード間の依存関係リソース

- [WaitForAll リソース](../reference/resources/windows/waitForAllResource.md)
- [WaitForSome リソース](../reference/resources/windows/waitForSomeResource.md)
- [WaitForAny リソース](../reference/resources/windows/waitForAnyResource.md)

### <a name="package-management-resources"></a>パッケージ管理リソース

- [PackageManagement リソース](../reference/resources/packagemanagement/PackageManagementDscResource.md)
- [PackageManagementSource リソース](../reference/resources/packagemanagement/PackageManagementSourceDscResource.md)

#### <a name="linux-resources"></a>Linux リソース

- [Linux Archive リソース](../reference/resources/linux/lnxArchiveResource.md)
- [Linux Environment リソース](../reference/resources/linux/lnxEnvironmentResource.md)
- [Linux FileLine リソース](../reference/resources/linux/lnxFileLineResource.md)
- [Linux File リソース](../reference/resources/linux/lnxFileResource.md)
- [Linux Group リソース](../reference/resources/linux/lnxGroupResource.md)
- [Linux Package リソース](../reference/resources/linux/lnxPackageResource.md)
- [Linux Script リソース](../reference/resources/linux/lnxScriptResource.md)
- [Linux Service リソース](../reference/resources/linux/lnxServiceResource.md)
- [Linux SshAuthorizedKeys リソース](../reference/resources/linux/lnxSshAuthorizedKeysResource.md)
- [Linux User リソース](../reference/resources/linux/lnxUserResource.md)
