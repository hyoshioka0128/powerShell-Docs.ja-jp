---
ms.date: 10/17/2017
title: プレリリース バージョンのスクリプト
description: PowerShellGet モジュールでは、セマンティック バージョニングを使用して、1.0.0 以上のバージョンのスクリプトにプレリリースとしてのタグ付けをすることができます。
ms.openlocfilehash: e9873a69148fd80553e566b31c7455a4ecaee5ce
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661476"
---
---
# <a name="prerelease-versions-of-scripts"></a>プレリリース バージョンのスクリプト

バージョン 1.6.0 より、PowerShellGet および PowerShell ギャラリーで 1.0.0 以降のバージョンをプレリリースとしてタグ付けできるようになりました。 この機能より前では、プレリリース パッケージは 0 で始まるバージョンに制限されていました。 この機能の目的は、PowerShell バージョン 3 以降、または既存のバージョンの PowerShellGet との後方互換性を失わずに [SemVer v1.0.0](http://semver.org/spec/v1.0.0.html) のバージョン管理規則に対してより優れたサポートを提供することです。 このトピックでは、スクリプトに固有の機能に焦点を当てています。 モジュールの同等の機能は、「[Prerelease Module Versions](module-prerelease-support.md)」(プレリリース モジュールのバージョン) のトピックにあります。 これらの機能を使用して、パブリッシャーはスクリプトをバージョン 2.5.0-alpha と識別し、プレリリース バージョンより優先される実稼働可能なバージョン 2.5.0 を後からリリースすることができます。

大まかには、プレリリースのスクリプト機能には次のようなものがあります。

- スクリプト マニフェストのバージョン文字列に PrereleaseString サフィックスを追加します。 スクリプトが PowerShell ギャラリーに発行されると、このデータがマニフェストから抽出され、プレリリース パッケージを識別するために使用されます。
- プレリリース パッケージを取得するには、-AllowPrerelease フラグを Find-Script、Install-Script、Update-Script、Save-Script の PowerShellGet コマンドに追加する必要があります。 フラグが指定されていないと、プレリリース パッケージが表示されません。
- Find-Script、Get-InstalledScript、および PowerShell ギャラリーに表示されるスクリプト バージョンには 2.5.0-alpha のように PrereleaseString が表示されます。

この機能の詳細については後述します。

## <a name="identifying-a-script-version-as-a-prerelease"></a>スクリプト バージョンをプレリリースとして識別する

プレリリース バージョンに対する PowerShellGet のサポートは、モジュールよりもスクリプトの方が簡単です。 スクリプトのバージョン管理は PowerShellGet でのみサポートされているため、Prerelease 文字列の追加によって生じる互換性の問題はありません。 PowerShell ギャラリーのスクリプトをプレリリースとして識別するには、スクリプトのメタデータの適切に書式設定されたバージョン文字列にプレリリースのサフィックスを追加します。

プレリリース バージョンのスクリプト マニフェストの例のセクションは、次のようになります。

```powershell
<#PSScriptInfo

.VERSION 3.2.1-alpha12

.GUID

...

#>
```

プレリリースのサフィックスを使用するには、バージョン文字列が次の要件を満たす必要があります。

- プレリリースのサフィックスは、Major.Minor.Build の Version が 3 セグメントである場合にのみ指定できます
  これは、SemVer v1.0.0 と適合します
- プレリリースのサフィックスはハイフンで始まる文字列で、ASCII 英数字 [0-9A-Za-z-] を含めることができます
- 現時点では、SemVer v1.0.0 の Prerelease 文字列のみがサポートされているため、プレリリースのサフィックスに (SemVer 2.0 では許可されている) ピリオドまたは + [.+] のどちらかを含めることは **できません**
- サポートされている PrereleaseString 文字列には、-alpha、-alpha1、-BETA、-update20171020 などがあります

### <a name="prerelease-versioning-impact-on-sort-order-and-installation-folders"></a>並べ替え順序およびインストール フォルダーへのプレリリース バージョン管理の影響

並べ替え順序は、PowerShell ギャラリーへの発行時に重要なプレリリース バージョンを使用するときや、PowerShellGet コマンドを使用してスクリプトをインストールするときに変更されます。 バージョン番号を含む 2 つのスクリプトのバージョンが存在する場合、並べ替え順序はハイフンの後の文字列部分に基づきます。 そのため、バージョン 2.5.0-alpha は 2.5.0-beta より小さく、2.5.0-beta は 2.5.0-gamma より小さくなります。 2 つのスクリプトに同じバージョン番号が含まれ、うち 1 つにのみ PrereleaseString が含まれる場合は、プレリリースのサフィックスの **ない** スクリプトが実稼働可能なバージョンと見なされ、プレリリース バージョンより後のバージョンとして並べ替えられます。 たとえば、2.5.0 と 2.5.0-beta を比較すると、2.5.0 バージョンの方が後だと見なされます。

PowerShell ギャラリーに発行すると、既定により、発行されているスクリプトのバージョンが必ず PowerShell ギャラリーにある以前に発行されているバージョンよりも後のバージョンになります。 パブリッシャーは 2.5.0-alpha のバージョンを 2.5.0-beta、または 2.5.0 (プレリリースのサフィックスなし) に更新することができます。

## <a name="finding-and-acquiring-prerelease-packages-using-powershellget-commands"></a>PowerShellGet コマンドを使用してプレリリース パッケージを検索および取得する

Find-Script、Install-Script、Update-Script、Save-Script の PowerShellGet コマンドを使用してプレリリース パッケージを操作するには、-AllowPrerelease フラグを追加する必要があります。 -AllowPrerelease が指定されていると、プレリリース パッケージが存在する場合にはそのパッケージが含まれます。 -AllowPrerelease フラグが指定されていないと、プレリリース パッケージが表示されません。

PowerShellGet スクリプトのコマンドで、これに対する例外が Get-InstalledScript です。場合によっては Uninstall-Script も含まれます。

- Get-InstalledScript はバージョン文字列にプレリリース情報がある場合に、常に自動的に表示します。
- Uninstall-Script は **バージョンが指定されていない** 場合に、既定でスクリプトの最新のバージョンをアンインストールします。 この動作は変更されていません。 ただし、`-RequiredVersion` を使用してプレリリース バージョンが指定された場合は、`-AllowPrerelease` が必要になります。

## <a name="examples"></a>例

```powershell
# Assume the PowerShell Gallery has TestPackage versions 1.8.0 and 1.9.0-alpha.
# If -AllowPrerelease is not specified, only version 1.8.0 will be returned.
C:\windows\system32> Find-Script TestPackage

Version        Name                                Repository           Description
-------        ----                                ----------           -----------
1.8.0          TestPackage                         PSGallery            Package used to validate changes to the PowerShe...

C:\windows\system32> Find-Script TestPackage -AllowPrerelease

Version        Name                                Repository           Description
-------        ----                                ----------           -----------
1.9.0-alpha    TestPackage                         PSGallery            Package used to validate changes to PowerShe...

# To install a prerelease, you must specify -AllowPrerelease. Specifying a prerelease version string is not sufficient.

C:\windows\system32> Install-Script TestPackage -RequiredVersion 1.9.0-alpha

PackageManagement\Find-Package : No match was found for the specified search criteria and script name 'TestPackage'.
Try Get-PSRepository to see all available registered script repositories.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\1.6.0\PSModule.psm1:1455 char:3
+         PackageManagement\Find-Package @PSBoundParameters | Microsoft ...
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Microsoft.Power...ets.FindPackage:FindPackage)[Find-Package], Exception
    + FullyQualifiedErrorId : NoMatchFoundForCriteria,Microsoft.PowerShell.PackageManagement.Cmdlets.FindPackage

# The previous command failed because -AllowPrerelease was not specified.
# Adding -AllowPrerelease will result in success.

C:\windows\system32> Install-Script TestPackage -RequiredVersion 1.9.0-alpha -AllowPrerelease
C:\windows\system32> Get-InstalledScript TestPackage

Version         Name                                Repository           Description
-------         ----                                ----------           -----------
1.9.0-alpha     TestPackage                         PSGallery            Package used to validate changes to PowerShe...

# Note that Get-InstalledScript shows the prerelease version.
# If -RequiredVersion is not specified, all installed scripts will be displayed by Get-InstalledScript
```

-RequiredVersion が指定されていない場合、Uninstall-Script によってスクリプトの最新バージョンが削除されます。
-RequiredVersion が指定されていて、これがプレリリースである場合、-AllowPrerelease をコマンドに追加する必要があります。

``` powershell
C:\windows\system32> Get-InstalledScript TestPackage

Version         Name                                Repository           Description
-------         ----                                ----------           -----------
1.9.0-alpha     TestPackage                         PSGallery            Package used to validate changes to PowerShe...

C:\windows\system32> Uninstall-Script TestPackage -RequiredVersion 1.9.0-alpha
Uninstall-Script: The '-AllowPrerelease' parameter must be specified when using the Prerelease string in
MinimumVersion, MaximumVersion, or RequiredVersion.
At line:1 char:1
+ Uninstall-Script TestPackage -RequiredVersion 1.9.0-beta
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Uninstall-Script], ArgumentException
    + FullyQualifiedErrorId : AllowPrereleaseRequiredToUsePrereleaseStringInVersion,Uninstall-script


C:\windows\system32> Uninstall-Script TestPackage -RequiredVersion 1.9.0-alpha -AllowPrerelease
# Since script versions are not installed side-by-side, the above could be simply "Uninstall-Script TestPackage"

C:\windows\system32> Get-Installedscript TestPackage
PackageManagement\Get-Package : No match was found for the specified search criteria and script names 'testpackage'.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\1.5.0.0\PSModule.psm1:4088 char:9
+         PackageManagement\Get-Package @PSBoundParameters | Microsoft. ...
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Microsoft.Power...lets.GetPackage:GetPackage) [Get-Package], Exception
    + FullyQualifiedErrorId : NoMatchFound,Microsoft.PowerShell.PackageManagement.Cmdlets.GetPackage
```

## <a name="more-details"></a>詳細

- [プレリリース モジュールのバージョン](module-prerelease-support.md)
- [Find-script](/powershell/module/powershellget/find-script)
- [Install-script](/powershell/module/powershellget/install-script)
- [Save-script](/powershell/module/powershellget/save-script)
- [Update-script](/powershell/module/powershellget/update-script)
- [Get-Installedscript](/powershell/module/powershellget/get-installedscript)
- [Uninstall-script](/powershell/module/powershellget/uninstall-script)
