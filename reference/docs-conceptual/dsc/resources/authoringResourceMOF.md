---
ms.date: 07/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: MOF を使用したカスタム DSC リソースの記述
description: この記事では、MOF ファイルで DSC カスタム リソースのスキーマを定義し、そのリソースを PowerShell スクリプト ファイルで実装します。
ms.openlocfilehash: e79a37699c468b2c55c307c96f1c193a2c1595b3
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92667183"
---
# <a name="writing-a-custom-dsc-resource-with-mof"></a>MOF を使用したカスタム DSC リソースの記述

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

この記事では、MOF ファイルで Windows PowerShell Desired State Configuration (DSC) カスタム リソースのスキーマを定義し、そのリソースを Windows PowerShell スクリプト ファイルで実装します。
このカスタム リソースは、Web サイトを作成および保守するためのものです。

## <a name="creating-the-mof-schema"></a>MOF スキーマの作成

スキーマでは、DSC 構成スクリプトによって構成できるリソースのプロパティを定義します。

### <a name="folder-structure-for-a-mof-resource"></a>MOF リソースのフォルダー構造

MOF スキーマを使用して DSC カスタム リソースを実装するには、次のフォルダー構造を作成します。 MOF スキーマは、`Demo_IISWebsite.schema.mof` ファイルで定義され、リソース スクリプトは `Demo_IISWebsite.psm1` で定義されます。 必要に応じて、モジュール マニフェスト (psd1) ファイルを作成できます。

```
$env:ProgramFiles\WindowsPowerShell\Modules (folder)
    |- MyDscResources (folder)
        |- DSCResources (folder)
            |- Demo_IISWebsite (folder)
                |- Demo_IISWebsite.psd1 (file, optional)
                |- Demo_IISWebsite.psm1 (file, required)
                |- Demo_IISWebsite.schema.mof (file, required)
```

> [!NOTE]
> 最上位のフォルダーに DSCResources という名前のフォルダーを作成し、各リソースのフォルダーにリソースと同じ名前を付ける必要があります。

### <a name="the-contents-of-the-mof-file"></a>MOF ファイルの内容

カスタム Web サイト リソースに使用できる MOF ファイルの例を次に示します。 この例に従うには、このスキーマをファイルに保存し、ファイルの名前は `Demo_IISWebsite.schema.mof` にします。

```
[ClassVersion("1.0.0"), FriendlyName("Website")]
class Demo_IISWebsite : OMI_BaseResource
{
  [Key] string Name;
  [Required] string PhysicalPath;
  [write,ValueMap{"Present", "Absent"},Values{"Present", "Absent"}] string Ensure;
  [write,ValueMap{"Started","Stopped"},Values{"Started", "Stopped"}] string State;
  [write] string Protocol[];
  [write] string BindingInfo[];
  [write] string ApplicationPool;
  [read] string ID;
};
```

前のコードについて、次のことに注意してください。

- `FriendlyName` では、DSC 構成スクリプトでこのカスタム リソースを参照するために使用できる名前を定義します。 この例では、`Website` は、組み込みのアーカイブ リソースのフレンドリ名 `Archive` に相当します。
- カスタム リソース用に定義するクラスは、`OMI_BaseResource` から派生する必要があります。
- プロパティの型修飾子 `[Key]` は、このプロパティがリソース インスタンスを一意に識別することを示します。 1 つ以上の `[Key]` プロパティが必要です。
- `[Required]` 修飾子は、プロパティが必須であることを示します (このリソースを使用する構成スクリプトで値を指定する必要があります)。
- `[write]` 修飾子は、構成スクリプトでカスタム リソースを使用するときにこのプロパティが省略可能であることを示します。 `[read]` 修飾子は、プロパティが構成では設定できず、報告のみを目的とするとを示します。
- `Values` は、プロパティに割り当てることのできる値を `ValueMap` で定義されている値の一覧に制限します。 詳細については、「[ValueMap and Value Qualifiers (ValueMap 修飾子と Value 修飾子)](/windows/desktop/WmiSdk/value-map)」を参照してください。
- 組み込みの DSC リソースとの一貫したスタイルを維持する方法として、値 `Present` と `Absent` を持つ `Ensure` というプロパティをリソースに含めることをお勧めします。
- カスタム リソースのスキーマ ファイルには、`classname.schema.mof` のように名前を付けます。ここで、`classname` はスキーマ定義内の `class` キーワードに続く識別子です。

### <a name="writing-the-resource-script"></a>リソース スクリプトの作成

リソース スクリプトでは、リソースのロジックを実装します。 このモジュールでは、`Get-TargetResource`、`Set-TargetResource`、および `Test-TargetResource` という 3 つの関数を含める必要があります。 3 つのすべての関数は、リソース用に作成した MOF スキーマで定義されている一連のプロパティと同じパラメーター セットを受け取る必要があります。 このドキュメントでは、この一連のプロパティを "リソース プロパティ" と呼びます。 これらの 3 つの関数は `<ResourceName>.psm1`というファイルに格納します。 次の例では、関数は `Demo_IISWebsite.psm1` というファイルに格納されます。

> [!NOTE]
> リソースに対して同じ構成スクリプトを複数回実行する場合、エラーが発生せずに、スクリプトを 1 回実行したときと同じ状態にリソースが保たれる必要があります。 これを実現するには、`Get-TargetResource` と `Test-TargetResource` 関数によってリソースが変更されないようにし、シーケンス内で同じパラメーター値を使用して `Set-TargetResource` 関数を複数回呼び出した場合に、1 回呼び出した場合と常に同じ結果になるようにします。

`Get-TargetResource` 関数の実装では、パラメーターとして指定されたキー リソース プロパティ値を使用して、指定されたリソース インスタンスの状態を確認します。 この関数は、キーとしてすべてのリソース プロパティを、対応する値としてこれらのプロパティの実際の値を一覧表示するハッシュ テーブルを返す必要があります。 コードの例は次のとおりです。

```powershell
# DSC uses the Get-TargetResource function to fetch the status of the resource instance
# specified in the parameters for the target machine
function Get-TargetResource
{
    param
    (
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

        $getTargetResourceResult = $null;

        <#
          Insert logic that uses the mandatory parameter values to get the website and
          assign it to a variable called $Website
          Set $ensureResult to "Present" if the requested website exists and to "Absent" otherwise
        #>

        # Add all Website properties to the hash table
        # This simple example assumes that $Website is not null
        $getTargetResourceResult = @{
            Name = $Website.Name
            Ensure = $ensureResult
            PhysicalPath = $Website.physicalPath
            State = $Website.state
            ID = $Website.id
            ApplicationPool = $Website.applicationPool
            Protocol = $Website.bindings.Collection.protocol
            Binding = $Website.bindings.Collection.bindingInformation
        }

        $getTargetResourceResult
}
```

構成スクリプトでリソース プロパティに指定されている値に応じて、`Set-TargetResource` は次のいずれかを実行する必要があります。

- 新しい Web サイトの作成
- 既存の Web サイトの更新
- 既存の Web サイトの削除

次の例を使って説明します。

```powershell
# The Set-TargetResource function is used to create, delete or configure a website on the target machine.
function Set-TargetResource
{
    [CmdletBinding(SupportsShouldProcess=$true)]
    param
    (
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

    <#
        If Ensure is set to "Present" and the website specified in the mandatory input parameters
          does not exist, then create it using the specified parameter values
        Else, if Ensure is set to "Present" and the website does exist, then update its properties
          to match the values provided in the non-mandatory parameter values
        Else, if Ensure is set to "Absent" and the website does not exist, then do nothing
        Else, if Ensure is set to "Absent" and the website does exist, then delete the website
    #>
}
```

最後に、`Test-TargetResource` 関数は、`Get-TargetResource` および `Set-TargetResource` と同じパラメーター セットを受け取る必要があります。 `Test-TargetResource` の実装で、キー パラメーターで指定されているリソース インスタンスの状態を確認します。 リソース インスタンスの実際の状態がパラメーター セットで指定された値と一致しない場合は、 `$false` を返します。 それ以外の場合は、`$true` を返します。

次のコードでは、`Test-TargetResource` 関数を実装します。

```powershell
function Test-TargetResource
{
    [CmdletBinding()]
    [OutputType([System.Boolean])]
    param
    (
        [ValidateSet("Present","Absent")]
        [System.String]
        $Ensure,

        [parameter(Mandatory = $true)]
        [System.String]
        $Name,

        [parameter(Mandatory = $true)]
        [System.String]
        $PhysicalPath,

        [ValidateSet("Started","Stopped")]
        [System.String]
        $State,

        [System.String[]]
        $Protocol,

        [System.String[]]
        $BindingData,

        [System.String]
        $ApplicationPool
    )

    # Get the current state
    $currentState = Get-TargetResource -Ensure $Ensure -Name $Name -PhysicalPath $PhysicalPath -State $State -ApplicationPool $ApplicationPool -BindingInfo $BindingInfo -Protocol $Protocol

    # Write-Verbose "Use this cmdlet to deliver information about command processing."

    # Write-Debug "Use this cmdlet to write debug information while troubleshooting."

    # Include logic to
    $result = [System.Boolean]
    # Add logic to test whether the website is present and its status matches the supplied
    # parameter values. If it does, return true. If it does not, return false.
    $result
}
```

> [!NOTE]
> 簡単にデバッグするには、前の 3 つの関数の実装で `Write-Verbose` コマンドレットを使用します。 このコマンドレットは、テキストを詳細メッセージ ストリームに書き込みます。 既定では、詳細メッセージ ストリームは表示されません。表示するには、 **$VerbosePreference** 変数の値を変更するか、DSC コマンドレットで **Verbose** パラメーターを使用します。

### <a name="creating-the-module-manifest"></a>モジュール マニフェストの作成

最後に、`New-ModuleManifest` コマンドレットを使用して、ご自分のカスタム リソース モジュールに `<ResourceName>.psd1` ファイルを定義します。 このコマンドレットを呼び出すときに、前のセクションで説明したスクリプト モジュール (.psm1) ファイルを参照します。 `Get-TargetResource`、`Set-TargetResource`、および `Test-TargetResource` をエクスポートする関数の一覧に含めます。 マニフェスト ファイルの例を次に示します。

```powershell
# Module manifest for module 'Demo.IIS.Website'
#
# Generated on: 1/10/2013
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '6AB5ED33-E923-41d8-A3A4-5ADDA2B301DE'

# Author of this module
Author = 'Contoso'

# Company or vendor of this module
CompanyName = 'Contoso'

# Copyright statement for this module
Copyright = 'Contoso. All rights reserved.'

# Description of the functionality provided by this module
Description = 'This Module is used to support the creation and configuration of IIS Websites through Get, Set and Test API on the DSC managed nodes.'

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '4.0'

# Minimum version of the common language runtime (CLR) required by this module
CLRVersion = '4.0'

# Modules that must be imported into the global environment prior to importing this module
RequiredModules = @("WebAdministration")

# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
NestedModules = @("Demo_IISWebsite.psm1")

# Functions to export from this module
FunctionsToExport = @("Get-TargetResource", "Set-TargetResource", "Test-TargetResource")

# Cmdlets to export from this module
#CmdletsToExport = '*'

# HelpInfo URI of this module
# HelpInfoURI = ''
}
```

## <a name="supporting-psdscrunascredential"></a>PsDscRunAsCredential のサポート

> [!Note]
> **PsDscRunAsCredential** は PowerShell 5.0 以降でサポートされています。

**PsDscRunAsCredential** プロパティを [DSC 構成](../configurations/configurations.md)リソース ブロックで使用して、指定した資格情報のもとでリソースを実行する必要があることを指定できます。 詳細については、「[ユーザーの資格情報を指定して DSC を実行する](../configurations/runAsUser.md)」を参照してください。

カスタム リソース内からユーザー コンテキストにアクセスするには、自動変数 `$PsDscContext` を使用できます。

たとえば、次のコードは、リソースが詳細出力ストリームに実行しているユーザー コンテキストを記述します。

```powershell
if (PsDscContext.RunAsUser) {
    Write-Verbose "User: $PsDscContext.RunAsUser";
}
```

## <a name="rebooting-the-node"></a>Node を再起動する

`Set-TargetResource` 関数で行われるアクションに再起動が必要となる場合、グローバル フラグを使用し、Node の再起動を LCM に指示できます。 この再起動は、`Set-TargetResource` 関数の完了直後に行われます。

`Set-TargetResource` 関数内で次のコード行を追加します。

```powershell
# Include this line if the resource requires a system reboot.
$global:DSCMachineStatus = 1
```

LCM で Node を再起動するには、 **RebootNodeIfNeeded** フラグを `$true` に設定する必要があります。
また、 **ActionAfterReboot** 設定を既定である **ContinueConfiguration** に設定してください。 LCM の構成方法については、「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」または「[ローカル構成マネージャーの構成 (v4)](../managing-nodes/metaConfig4.md)」を参照してください。
