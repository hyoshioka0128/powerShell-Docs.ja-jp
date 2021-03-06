---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: Import-DSCResource の使用
ms.openlocfilehash: ee0b2f0469c6507c8f0148138198597a9e57cdd7
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62080103"
---
# <a name="using-import-dscresource"></a>Import-DSCResource の使用

`Import-DScResource` は動的なキーワードであり、構成スクリプト ブロックの内部でのみ使用できます。 `Import-DSCResource` キーワードは、構成で必要なリソースをインポートするために使います。 `$pshome` の下にあるリソースは自動的にインポートされますが、[構成](Configurations.md)内で使用されるすべてのリソースを明示的にインポートするのがやはりベスト プラクティスと考えられます。

`Import-DSCResource` の構文は次に示すとおりです。  モジュールを名前で指定するときは、1 行に 1 つずつ列記する必要があります。

```syntax
Import-DscResource [-Name <ResourceName(s)>] [-ModuleName <ModuleName>]
```

|パラメーター  |説明  |
|---------|---------|
|`-Name`|インポートする必要がある DSC リソースの名前です。 モジュール名を指定した場合は、そのモジュール内でこれらの DSC リソースが検索されます。それ以外の場合は、すべての DSC リソース パスで DSC リソースが検索されます。 ワイルド カードがサポートされます。|
|`-ModuleName`|モジュール名またはモジュールの指定です。  モジュールからインポートするリソースを指定した場合は、それらのリソースだけのインポートが試みられます。 モジュールのみを指定した場合は、モジュール内のすべての DSC リソースがインポートされます。|

```powershell
Import-DscResource -ModuleName xActiveDirectory;
```

## <a name="example-use-import-dscresource-within-a-configuration"></a>次に例を示します。構成内で Import-DSCResource を使用する

```powershell
Configuration MSDSCConfiguration
{
    # Search for and imports Service, File, and Registry from the module PSDesiredStateConfiguration.
    Import-DSCResource -ModuleName PSDesiredStateConfiguration -Name Service, File, Registry
    
    # Search for and import Resource1 from the module that defines it.
    # If only –Name parameter is used then resources can belong to different PowerShell modules as well.
    # TimeZone resource is from the ComputerManagementDSC module which is not installed by default.
    # As a best practice, list each requirement on a different line if possible.  This makes reviewing
    # multiple changes in source control a bit easier.
    Import-DSCResource -Name File
    Import-DSCResource -Name TimeZone

    # Search for and import all DSC resources inside the module PSDesiredStateConfiguration.
    # When specifying the modulename parameter, it is a requirement to list each on a new line.
    Import-DSCResource -ModuleName PSDesiredStateConfiguration
    Import-DSCResource -ModuleName ComputerManagementDsc
...
```

> [!NOTE]
> 同じコマンド内でリソース名およびモジュール名に複数の値を指定することはサポートされていません。 同じリソースが複数のモジュールに存在する場合、どのモジュールからリソースが読み込まれるかはわかりません。 次のコマンドでは、コンパイル中にエラーが発生します。
>
> ```powershell
> Import-DscResource -Name UserConfigProvider*,TestLogger1 -ModuleName UserConfigProv,PsModuleForTestLogger
> ```

Name パラメーターのみを使用するときに考慮すべき事項:

- コンピューターにインストールされているモジュールの数によっては、リソースを大量に消費する操作です。
- 指定した名前で最初に見つかったリソースが読み込まれます。 同じ名前で複数のリソースがインストールされている場合は、間違ったリソースが読み込まれる可能性があります。

推奨される使用方法は、以下で説明するように、`-Name` パラメーターと共に `–ModuleName` を指定することです。

この使用方法には次のようなメリットがあります。

- 指定したリソースに検索範囲が制限されるため、パフォーマンスへの影響が減少します。
- リソースが定義されているモジュールを明示的に定義することで、正しいリソースが確実に読み込まれます。

> [!NOTE]
> PowerShell 5.0 では、DSC リソースに複数バージョンを用意することができ、コンピューターにその複数のバージョンをサイド バイ サイドでインストールできます。 これを実装するには、リソース モジュールの複数のバージョンを同じモジュール フォルダーに格納します。
> 詳細については、「[複数のバージョンがあるリソースの使用](sxsresource.md)」を参照してください。

## <a name="intellisense-with-import-dscresource"></a>Import-DSCResource での IntelliSense

ISE で DSC の構成を作成するとき、PowerShell ではリソースおよびリソースのプロパティに対して IntelliSence が提供されます。 `$pshome` モジュール パスの下にあるリソースの定義が、自動的に読み込まれます。 `Import-DSCResource` キーワードを使ってリソースをインポートすると、指定したリソース定義が追加されて、インポートされたリソースのスキーマを含むように IntelliSence が拡張されます。

![リソースの IntelliSense](/media/resource-intellisense.png)

> [!NOTE]
> PowerShell 5.0 以降では、DSC リソースとそのプロパティ用の ISE に対して Tab 補完機能が追加されました。 詳しくは、[リソース](../resources/resources.md)に関する記事をご覧ください。

構成をコンパイルするとき、PowerShell では、インポートされたリソース定義を使って、構成内のすべてのリソース ブロックが検証されます。
リソースのスキーマ定義を使って、各リソース ブロックで次の規則が検証されます。

- スキーマで定義されているプロパティのみが使用されている。
- 各プロパティのデータ型が正しい。
- キー プロパティが指定されている。
- 読み取り専用プロパティが使用されていない。
- 値の検証が型にマップされる。

以下のような構成について考えます。

```powershell
Configuration SchemaValidationInCorrectEnumValue
{
    # It is best practice to explicitly import all resources used in your Configuration.
    # This includes resources that are imported automatically, like WindowsFeature.
    Import-DSCResource -Name WindowsFeature
    Node localhost
    {
        WindowsFeature ROLE1
        {
            Name = “Telnet-Client”
            Ensure = “Invalid”
        }
    }
}
```

この構成をコンパイルすると、結果はエラーになります。

```output
PSDesiredStateConfiguration\WindowsFeature: At least one of the values ‘Invalid’ is not supported or valid for property ‘Ensure’ on class ‘WindowsFeature’. Please specify only supported values: Present, Absent.
```

IntelliSence とスキーマ検証により、解析とコンパイルの間にさらに多くのエラーをキャッチでき、実行時の問題を回避できます。

> [!NOTE]
> 各 DSC リソースは、名前と、リソースのスキーマによって定義された **FriendlyName** を持つことができます。 "MSFT_ServiceResource.shema.mof" の先頭の 2 行を次に示します。
> ```syntax
> [ClassVersion("1.0.0"),FriendlyName("Service")]
> class MSFT_ServiceResource : OMI_BaseResource
> ```
> このリソースを構成で使用するときは、**MSFT_ServiceResource** または **Service** を指定できます。

## <a name="powershell-v4-and-v5-differences"></a>PowerShell の v4 と v5 の違い

構成を作成するときに、複数の相違点が PowerShell 4.0 と PowerShell 5.0 以降の間に存在します。 このセクションでは、特にこの記事に関連する相違点を示します。

### <a name="multiple-resource-versions"></a>複数のリソース バージョン

複数のバージョンのリソースのサイド バイ サイドでのインストールと使用は、PowerShell 4.0 ではサポートされていませんでした。 リソースを構成にインポートするときに問題がある場合は、インストールされているリソースのバージョンが 1 つだけであることを確認してください。

次の図では、2 つのバージョンの **xPSDesiredStateConfiguration** モジュールがインストールされています。

![修正された複数リソース バージョン](/media/multiple-resource-versions-broken.md)

必要なモジュールのバージョンの内容を、モジュール ディレクトリの最上位レベルにコピーします。

![修正された複数リソース バージョン](/media/multiple-resource-versions-fixed.md)

### <a name="resource-location"></a>リソースの場所

構成を作成してコンパイルするとき、[PSModulePath](/powershell/developer/module/modifying-the-psmodulepath-installation-path) で指定した任意のディレクトリにリソースを格納できます。 PowerShell 4.0 の LCM では、すべての DSC リソース モジュールが "Program Files\WindowsPowerShell\Modules" または `$pshome\Modules` に格納されている必要があります。 PowerShell 5.0 以降では、この要件はなくなり、`PSModulePath` で指定した任意のディレクトリにリソース モジュールを格納できます。

## <a name="see-also"></a>関連項目

- [リソース](../resources/resources.md)
