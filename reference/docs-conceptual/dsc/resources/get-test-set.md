---
ms.date: 07/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: Get-Test-Set
description: この記事では、DSC 構成で Get、Test、および Set メソッドを実装する方法について説明します。
ms.openlocfilehash: e0da1452a1237c550f52a4a4f9e4400f801ed7cd
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92646801"
---
# <a name="get-test-set"></a>Get-Test-Set

>適用先: Windows PowerShell 4.0、Windows PowerShell 5.0

PowerShell の Desired State Configuration は、 **Get** 、 **Test** 、および **Set** プロセスを中心に構成されています。 DSC の各[リソース](resources.md)には、これらの各操作を完了するためのメソッドが含まれています。
「 [構成](../configurations/configurations.md)」では、リソースの **Get** 、 **Test** 、および **Set** メソッドのパラメーターとなるキーを入力するリソース ブロックを定義します。

これは **Service** リソース ブロックの構文です。 **Service** リソースでは、Windows サービスを構成します。

```syntax
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

**Service** リソースの **Get** 、 **Test** 、および **Set** メソッドには、その値を受け入れるパラメーター ブロックがあります。

```powershell
param
(
    [parameter(Mandatory = $true)]
    [ValidateNotNullOrEmpty()]
    [System.String]
    $Name,

    [System.String]
    [ValidateSet("Automatic", "Manual", "Disabled")]
    $StartupType,

    [System.String]
    [ValidateSet("LocalSystem", "LocalService", "NetworkService")]
    $BuiltInAccount,

    [System.Management.Automation.PSCredential]
    [ValidateNotNull()]
    $Credential,

    [System.String]
    [ValidateSet("Running", "Stopped")]
    $State="Running",

    [System.String]
    [ValidateNotNullOrEmpty()]
    $DisplayName,

    [System.String]
    [ValidateNotNullOrEmpty()]
    $Description,

    [System.String]
    [ValidateNotNullOrEmpty()]
    $Path,

    [System.String[]]
    [ValidateNotNullOrEmpty()]
    $Dependencies,

    [System.String]
    [ValidateSet("Present", "Absent")]
    $Ensure="Present"
)
```

> [!NOTE]
> リソースの定義に使用される言語とメソッドによって、 **Get** 、 **Test** 、および **Set** の各メソッドの定義方法が決まります。

**Service** リソースには必要なキーが 1 つしかないため (`Name`)、 **Service** ブロック リソースは次のように単純になる可能性があります。

```powershell
Configuration TestConfig
{
    Import-DSCResource -Name Service
    Node localhost
    {
        Service "MyService"
        {
            Name = "Spooler"
        }
    }
}
```

上記の Configuration をコンパイルすると、キーに指定した値は、生成された `.mof` ファイルに格納されます。 詳細については、[MOF](/windows/desktop/wmisdk/managed-object-format--mof-) に関する記事を参照してください。

```
instance of MSFT_ServiceResource as $MSFT_ServiceResource1ref
{
SourceInfo = "::5::1::Service";
 ModuleName = "PsDesiredStateConfiguration";
 ResourceID = "[Service]MyService";
 Name = "Spooler";

ModuleVersion = "1.0";

 ConfigurationName = "Test";

};
```

適用されると、 [ローカル構成マネージャー](../managing-nodes/metaConfig.md) (LCM) によって `.mof` ファイルから値 "Spooler" が読み取られ、それが **Service** リソースの "MyService" インスタンスの **Get** メソッド、 **Test** メソッド、 **Set** メソッドの **Name** パラメーターに渡されます。

## <a name="get"></a>取得

リソースの **Get** メソッドでは、ターゲット ノードに構成されているリソースの状態を取得します。 この状態は[ハッシュテーブル](/powershell/module/microsoft.powershell.core/about/about_hash_tables)として返されます。 **ハッシュテーブル** のキーは、リソースが受け入れる構成可能な値、つまりパラメーターになります。

**Get** メソッドは、 [Get-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/get-dscconfiguration) コマンドレットに直接マップします。
`Get-DSCConfiguration` を呼び出すと、LCM では現在適用されている構成内の各リソースの **Get** メソッドを実行します。 LCM では、`.mof` ファイルに格納されているキー値を、対応する各リソース インスタンスへのパラメーターとして使用します。

これは、"Spooler" サービスを構成する **Service** リソースの出力例です。

```output
ConfigurationName    : Test
DependsOn            :
ModuleName           : PsDesiredStateConfiguration
ModuleVersion        : 1.1
PsDscRunAsCredential :
ResourceId           : [Service]Spooler
SourceInfo           :
BuiltInAccount       : LocalSystem
Credential           :
Dependencies         : {RPCSS, http}
Description          : This service spools print jobs and handles interaction with the printer.  If you turn off
                       this service, you won't be able to print or see your printers.
DisplayName          : Print Spooler
Ensure               :
Name                 : Spooler
Path                 : C:\WINDOWS\System32\spoolsv.exe
StartupType          : Automatic
State                : Running
Status               :
PSComputerName       :
CimClassName         : MSFT_ServiceResource
```

出力には、 **Service** リソースで構成できる現在の値のプロパティが表示されます。

```syntax
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

## <a name="test"></a>テスト

リソースの **Test** メソッドでは、ターゲット ノードがリソースの " _望ましい状態_ " に現在準拠しているかどうかを判断します。 **Test** メソッドは、Node が準拠しているかどうかのみを示す `$true` または `$false` を返します。 [Test-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration) を呼び出すと、LCM では現在適用されている構成内の各リソースの **Test** メソッドを呼び出します。 LCM は、".mof" ファイルに格納されているキー値を、対応する各リソース インスタンスへのパラメーターとして使用します。

個々のリソースの **Test** の結果が `$false` の場合、`Test-DSCConfiguration` は Node が準拠していないことを示す `$false` を返します。 すべてのリソースの **Test** メソッドが `$true` を返す場合、`Test-DSCConfiguration` は Node が準拠していることを示す `$true` を返します。

```powershell
Test-DSCConfiguration
```

```output
True
```

PowerShell 5.0 以降では、 **Detailed** パラメーターが追加されました。 **Detailed** を指定すると、`Test-DSCConfiguration` は、準拠および非準拠リソースの結果のコレクションが含まれるオブジェクトを返します。

```powershell
Test-DSCConfiguration -Detailed
```

```output
PSComputerName  ResourcesInDesiredState        ResourcesNotInDesiredState     InDesiredState
--------------  -----------------------        --------------------------     --------------
localhost       {[Service]Spooler}                                            True
```

詳細については、「[Test-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration)」を参照してください。

## <a name="set"></a>オン

リソースの **Set** メソッドは、Node がリソースの " *望ましい状態* " に準拠した状態になるように試行します。 **Set** メソッドは **べき等** であるように意図されています。つまり、 **Set** を複数回実行し、エラーなしで常に同じ結果を取得することができます。 [Start-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/Start-DSCConfiguration) を実行すると、LCM では現在適用されている構成内の各リソースが循環処理されます。 LCM では ".mof" ファイルから現在のリソース インスタンスのキー値を取得し、それらを **Test** メソッドのパラメーターとして使用します。 **Test** メソッドが `$true` を返した場合、Node は現在のリソースに準拠しているため、 **Set** メソッドはスキップされます。 **Test** が `$false` を返した場合、Node は準拠していません。 LCM はリソース インスタンスのキー値をパラメーターとしてリソースの **Set** メソッドに渡し、Node を準拠状態に復元します。

**Verbose** パラメーターと **Wait** パラメーターを指定することで、`Start-DSCConfiguration` コマンドレットの進行状況を確認できます。 この例では、Node は既に準拠しています。 `Verbose` の出力は、 **Set** メソッドがスキップされたことを示しています。

```
PS> Start-DSCConfiguration -Verbose -Wait -UseExisting

VERBOSE: Perform operation 'Invoke CimMethod' with following parameters, ''methodName' =
ApplyConfiguration,'className' = MSFT_DSCLocalConfigurationManager,'namespaceName' =
root/Microsoft/Windows/DesiredStateConfiguration'.
VERBOSE: An LCM method call arrived from computer SERVER01 with user sid
S-1-5-21-124525095-708259637-1543119021-1282804.
VERBOSE: [SERVER01]:                            [] Starting consistency engine.
VERBOSE: [SERVER01]:                            [] Checking consistency for current configuration.
VERBOSE: [SERVER01]:                            [DSCEngine] Importing the module
C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateConfiguration\DscResources\MSFT_ServiceResource\MSFT
_ServiceResource.psm1 in force mode.
VERBOSE: [SERVER01]: LCM:  [ Start  Resource ]  [[Service]Spooler]
VERBOSE: [SERVER01]: LCM:  [ Start  Test     ]  [[Service]Spooler]
VERBOSE: [SERVER01]:                            [[Service]Spooler] Importing the module MSFT_ServiceResource in
force mode.
VERBOSE: [SERVER01]: LCM:  [ End    Test     ]  [[Service]Spooler]  in 0.2540 seconds.
VERBOSE: [SERVER01]: LCM:  [ Skip   Set      ]  [[Service]Spooler]
VERBOSE: [SERVER01]: LCM:  [ End    Resource ]  [[Service]Spooler]
VERBOSE: [SERVER01]:                            [] Consistency check completed.
VERBOSE: Operation 'Invoke CimMethod' complete.
VERBOSE: Time taken for configuration job to complete is 1.379 seconds
```

## <a name="see-also"></a>関連項目

- [Azure Automation DSC の概要](/azure/automation/automation-dsc-overview)
- [SMB プル サーバーのセットアップ](../pull-server/pullServerSMB.md)
- [プル クライアントの構成](../pull-server/pullClientConfigID.md)
