---
ms.date: 07/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: 単一インスタンスの DSC リソースを記述する (ベスト プラクティス)
description: この記事では、構成で単一のインスタンスのみを許可する DSC リソースを定義するためのベスト プラクティスについて説明します。
ms.openlocfilehash: 4744136b5a733c86b517b239b2c37ce57a4246f7
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662642"
---
# <a name="writing-a-single-instance-dsc-resource-best-practice"></a>単一インスタンスの DSC リソースを記述する (ベスト プラクティス)

> [!NOTE]
> この記事では、構成で単一のインスタンスのみを許可する DSC リソースを定義するためのベスト プラクティスについて説明します。 現在のところ、これを行う組み込みの DSC 機能はありません。 これは将来変更される可能性があります。

構成の中で、1 つのリソースを複数回使用することを許可したくない状況があります。 たとえば、[xTimeZone](https://github.com/PowerShell/xTimeZone) リソースの以前の実装では、構成がリソースを複数回呼び出し、各リソース ブロックに別々のタイム ゾーンを設定する可能性がありました。

```powershell
Configuration SetTimeZone
{
    Param
    (
        [String[]]$NodeName = $env:COMPUTERNAME

    )

    Import-DSCResource -ModuleName xTimeZone


    Node $NodeName
    {
         xTimeZone TimeZoneExample
         {

            TimeZone = 'Eastern Standard Time'
         }

         xTimeZone TimeZoneExample2
         {

            TimeZone = 'Pacific Standard Time'

         }

    }
}
```

これは、DSC リソース キーの動作方法が原因でした。 1 つのリソースには、少なくとも 1 つのキー プロパティが必要です。 キー プロパティのすべての値の組み合わせが一意であれば、リソースのインスタンスは一意とみなされます。 リソースの以前の実装では、 [xTimeZone](https://github.com/PowerShell/xTimeZone) リソースには **TimeZone** プロパティしかなかったため、このプロパティをキーにする必要がありました。 このため、上記のような構成は、警告なしにコンパイルされ実行されました。 各 **xTimeZone** リソース ブロックは、一意とみなされます。 これにより、構成がノードに繰り返し適用され、タイムゾーンが何度も戻されたり進められたりしていました。

構成がタイムゾーンを対象のノードに 1 回だけ設定されるようにするため、リソースは、2 番目のプロパティで、キー プロパティになる **IsSingleInstance** を追加するよう更新されることになっていました。 **IsSingleInstance** は、 **ValueMap** を使用して、単一の値 "Yes" に制限されていました。 リソースの以前の MOF スキーマは、次のようでした。

```powershell
[ClassVersion("1.0.0.0"), FriendlyName("xTimeZone")]
class xTimeZone : OMI_BaseResource
{
    [Key, Description("Specifies the TimeZone.")] String TimeZone;
};
```

リソースの更新された MOF スキーマを、次に示します。

```powershell
[ClassVersion("1.0.0.0"), FriendlyName("xTimeZone")]
class xTimeZone : OMI_BaseResource
{
    [Key, Description("Specifies the resource is a single instance, the value must be 'Yes'"), ValueMap{"Yes"}, Values{"Yes"}] String IsSingleInstance;
    [Required, Description("Specifies the TimeZone.")] String TimeZone;
};
```

リソース スクリプトも、新しいパラメーターを使用するよう更新されました。 リソース スクリプトがどのように変更されたかを次に示します。

```powershell
function Get-TargetResource
{
    [CmdletBinding()]
    [OutputType([Hashtable])]
    param
    (
        [parameter(Mandatory = $true)]
        [ValidateSet('Yes')]
        [String]
        $IsSingleInstance,

        [parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $TimeZone
    )

    #Get the current TimeZone
    $CurrentTimeZone = Get-TimeZone

    $returnValue = @{
        TimeZone = $CurrentTimeZone
        IsSingleInstance = 'Yes'
    }

    #Output the target resource
    $returnValue
}

function Set-TargetResource
{
    [CmdletBinding()]
    param
    (
        [parameter(Mandatory = $true)]
        [ValidateSet('Yes')]
        [String]
        $IsSingleInstance,

        [parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $TimeZone
    )

    #Output the result of Get-TargetResource function.
    $CurrentTimeZone = Get-TimeZone

    Write-Verbose -Message "Replace the System Time Zone to $TimeZone"

    try
    {
        if($CurrentTimeZone -ne $TimeZone)
        {
            Write-Verbose -Verbose "Setting the TimeZone"
            Set-TimeZone -TimeZone $TimeZone
        }
        else
        {
            Write-Verbose -Verbose "TimeZone already set to $TimeZone"
        }
    }
    catch
    {
        $ErrorMsg = $_.Exception.Message
        Write-Verbose -Verbose $ErrorMsg
    }
}


function Test-TargetResource
{
    [CmdletBinding()]
    [OutputType([Boolean])]
    param
    (
        [parameter(Mandatory = $true)]
        [ValidateSet('Yes')]
        [String]
        $IsSingleInstance,

        [parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $TimeZone
    )

    #Output from Get-TargetResource
    $CurrentTimeZone = Get-TimeZone

    if($TimeZone -eq $CurrentTimeZone)
    {
        return $true
    }
    else
    {
        return $false
    }
}

Function Get-TimeZone {
    [CmdletBinding()]
    param()

    & tzutil.exe /g
}

Function Set-TimeZone {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [System.String]
        $TimeZone
    )

    try
    {
        & tzutil.exe /s $TimeZone
    }
    catch
    {
        $ErrorMsg = $_.Exception.Message
        Write-Verbose $ErrorMsg
    }
}

Export-ModuleMember -Function *-TargetResource
```

**TimeZone** プロパティは、もはやキーではないことにご注意ください。 現在は、構成がタイム ゾーンの設定を 2 回試みる場合 (異なる **TimeZone** 値を指定した 2 つの異なる **xTimeZone** ブロックを使用)、構成をコンパイルしようとすると、次のようにエラーが発生します。

```Output
Test-ConflictingResources : A conflict was detected between resources '[xTimeZone]TimeZoneExample (::15::10::xTimeZone)' and
'[xTimeZone]TimeZoneExample2 (::22::10::xTimeZone)' in node 'CONTOSO-CLIENT'. Resources have identical key properties but there are differences in the
following non-key properties: 'TimeZone'. Values 'Eastern Standard Time' don't match values 'Pacific Standard Time'. Please update these property
values so that they are identical in both cases.
At line:271 char:9
+         Test-ConflictingResources $keywordName $canonicalizedValue $k ...
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Write-Error], InvalidOperationException
    + FullyQualifiedErrorId : ConflictingDuplicateResource,Test-ConflictingResources
Errors occurred while processing configuration 'SetTimeZone'.
At C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateConfiguration\PSDesiredStateConfiguration.psm1:3705 char:5
+     throw $ErrorRecord
+     ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (SetTimeZone:String) [], InvalidOperationException
    + FullyQualifiedErrorId : FailToProcessConfiguration
```
