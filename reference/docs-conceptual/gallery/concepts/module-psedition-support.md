---
ms.date: 06/10/2020
title: 互換性のある PowerShell エディションが含まれるモジュール
description: この記事では、PowerShellGet コマンドレットで、PowerShell モジュールの Desktop エディションと Core エディションがどのようにサポートされるかについて説明します。
ms.openlocfilehash: 530101590cf83a1f43cbb9ce32d07a7e0ec79253
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661495"
---
# <a name="modules-with-compatible-powershell-editions"></a>互換性のある PowerShell エディションが含まれるモジュール

バージョン 5.1 から、PowerShell はさまざまな機能セットとプラットフォーム互換性を備えるさまざまなエディションで使用できます。

- **Desktop Edition:** .NET Framework に基づいて構成され、Windows デスクトップ、Windows Server、Windows Server Core、およびその他のほとんどの Windows エディション上の Windows PowerShell v4.0 以下と Windows PowerShell 5.1 に適用されます。
- **Core Edition:** .NET Core に基づいて構築され、Windows IoT や Windows Nanoserver などのフットプリントが小さい Windows エディション上の PowerShell Core 6.0 以上と Windows PowerShell 5.1 に適用されます。

PowerShell のエディションの詳細については、[about_PowerShell_Editions][] に関するページをご覧ください。

## <a name="declaring-compatible-editions"></a>互換性のあるエディションを宣言する

モジュールの作成者は、`CompatiblePSEditions` モジュール マニフェスト キーを使用して、モジュールが 1 つ以上の PowerShell エディションと互換性があることを宣言できます。 このキーは、PowerShell 5.1 以降でのみサポートされます。

> [!NOTE]
> `CompatiblePSEditions` キーを使用して指定された、または `$PSEdition` 変数を使用したモジュール マニフェストは、PowerShell v4 またはそれ以前にインポートすることはできません。

```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1
$ModuleInfo = Test-ModuleManifest -Path .\TestModuleWithEdition.psd1
$ModuleInfo.CompatiblePSEditions
```

```Output
Desktop
Core
```

```powershell
$ModuleInfo | Get-Member CompatiblePSEditions
```

```Output
   TypeName: System.Management.Automation.PSModuleInfo

Name                 MemberType Definition
----                 ---------- ----------
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}
```

使用可能なモジュールの一覧を取得するときには、PowerShell のエディションを条件としてフィルター処理できます。

```powershell
Get-Module -ListAvailable -PSEdition Desktop
```

```Output
    Directory: C:\Program Files\WindowsPowerShell\Modules

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0        ModuleWithPSEditions
```

```powershell
Get-Module -ListAvailable -PSEdition Core | % CompatiblePSEditions
```

```Output
Desktop
Core
```

PowerShell 6 以降では、モジュールが `$env:windir\System32\WindowsPowerShell\v1.0\Modules` からインポートされた場合にそのモジュールに互換性があるかどうかを判断するために `CompatiblePSEditions` 値が使用されます。
この動作は、Windows にのみ適用されます。 このシナリオ以外では、値はメタデータとしてのみ使用されます。

## <a name="finding-compatible-modules"></a>互換性のあるモジュールの検索

PowerShell ギャラリー ユーザーは、 **PSEdition_Desktop** および **PSEdition_Core** のタグを使用して特定の PowerShell エディションでサポートされているモジュールの一覧を表示できます。

**PSEdition_Desktop** および **PSEdition_Core** のタグがないモジュールは、PowerShell Desktop エディションで正常に動作するものと見なされます。

```powershell
# Find modules supported on PowerShell Desktop edition
Find-Module -Tag PSEdition_Desktop

# Find modules supported on PowerShell Core editions
Find-Module -Tag PSEdition_Core
```

## <a name="targeting-multiple-editions"></a>複数のエディションを対象にする

モジュールの作成者が、PowerShell エディションのどちらかまたは両方を対象とする 1 つのモジュールを発行することができます。

1 つのモジュールが、Desktop と Core の両方のエディションで動作します。そのモジュール内で、作成者は、`$PSEdition` 変数を使用して、RootModule またはモジュール マニフェストで必要なロジックを追加する必要があります。 モジュールでは、 **CoreCLR** と **FullCLR** の両方を対象とするコンパイル済み DLL の 2 つのセットを使用できます。 適切な DLL を読み込むためのロジックを含むパッケージ化オプションを次に示します。

### <a name="option-1-packaging-a-module-for-targeting-multiple-versions-and-multiple-editions-of-powershell"></a>オプション 1: PowerShell の複数のバージョンおよび複数のエディションを対象としてパッケージ化する

モジュール フォルダーの内容

- Microsoft.Windows.PowerShell.ScriptAnalyzer.BuiltinRules.dll
- Microsoft.Windows.PowerShell.ScriptAnalyzer.dll
- PSScriptAnalyzer.psd1
- PSScriptAnalyzer.psm1
- ScriptAnalyzer.format.ps1xml
- ScriptAnalyzer.types.ps1xml
- coreclr\Microsoft.Windows.PowerShell.ScriptAnalyzer.BuiltinRules.dll
- coreclr\Microsoft.Windows.PowerShell.ScriptAnalyzer.dll
- en-US\about_PSScriptAnalyzer.help.txt
- en-US\Microsoft.Windows.PowerShell.ScriptAnalyzer.dll-Help.xml
- PSv3\Microsoft.Windows.PowerShell.ScriptAnalyzer.BuiltinRules.dll
- PSv3\Microsoft.Windows.PowerShell.ScriptAnalyzer.dll
- Settings\CmdletDesign.psd1
- Settings\DSC.psd1
- Settings\ScriptFunctions.psd1
- Settings\ScriptingStyle.psd1
- Settings\ScriptSecurity.psd1

`PSScriptAnalyzer.psd1` ファイルの内容

```powershell
@{

# Author of this module
Author = 'Microsoft Corporation'

# Script module or binary module file associated with this manifest.
RootModule = 'PSScriptAnalyzer.psm1'

# Version number of this module.
ModuleVersion = '1.6.1'

# ---
}
```

以下のロジックは、現在のエディションまたはバージョンに応じて必要なアセンブリを読み込みます。

`PSScriptAnalyzer.psm1` ファイルの内容:

```powershell
#
# Script module for module 'PSScriptAnalyzer'
#
Set-StrictMode -Version Latest

# Set up some helper variables to make it easier to work with the module
$PSModule = $ExecutionContext.SessionState.Module
$PSModuleRoot = $PSModule.ModuleBase

# Import the appropriate nested binary module based on the current PowerShell version
$binaryModuleRoot = $PSModuleRoot


if (($PSVersionTable.Keys -contains "PSEdition") -and ($PSVersionTable.PSEdition -ne 'Desktop')) {
    $binaryModuleRoot = Join-Path -Path $PSModuleRoot -ChildPath 'coreclr'
}
else
{
    if ($PSVersionTable.PSVersion -lt [Version]'5.0')
    {
        $binaryModuleRoot = Join-Path -Path $PSModuleRoot -ChildPath 'PSv3'
    }
}

$binaryModulePath = Join-Path -Path $binaryModuleRoot -ChildPath 'Microsoft.Windows.PowerShell.ScriptAnalyzer.dll'
$binaryModule = Import-Module -Name $binaryModulePath -PassThru

# When the module is unloaded, remove the nested binary module that was loaded with it
$PSModule.OnRemove = {
    Remove-Module -ModuleInfo $binaryModule
}
```

### <a name="option-2-use-psedition-variable-in-the-psd1-file-to-load-the-proper-dlls"></a>オプション 2:PSD1 ファイルで $PSEdition 変数を使用して適切な DLL を読み込む

PS 5.1 以降では、`$PSEdition` グローバル変数をモジュール マニフェスト ファイル内で使用できます。 この変数を使用すると、モジュールの作成者が、モジュール マニフェスト ファイル内で条件値を指定できます。 `$PSEdition` 変数は、制限された言語モードまたはデータ セクション内で参照できます。

`CompatiblePSEditions` キーを持つサンプル モジュール マニフェスト ファイル。

```powershell
@{
    # Script module or binary module file associated with this manifest.
    RootModule = if($PSEdition -eq 'Core')
    {
        'coreclr\MyCoreClrRM.dll'
    }
    else # Desktop
    {
        'clr\MyFullClrRM.dll'
    }

    # Supported PSEditions
    CompatiblePSEditions = 'Desktop', 'Core'

    # Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
    NestedModules = if($PSEdition -eq 'Core')
    {
        'coreclr\MyCoreClrNM1.dll',
        'coreclr\MyCoreClrNM2.dll'
    }
    else # Desktop
    {
        'clr\MyFullClrNM1.dll',
        'clr\MyFullClrNM2.dll'
    }
}
```

モジュールの内容

- ModuleWithEditions\ModuleWithEditions.psd1
- ModuleWithEditions\clr\MyFullClrNM1.dll
- ModuleWithEditions\clr\MyFullClrNM2.dll
- ModuleWithEditions\clr\MyFullClrRM.dll
- ModuleWithEditions\coreclr\MyCoreClrNM1.dll
- ModuleWithEditions\coreclr\MyCoreClrNM2.dll
- ModuleWithEditions\coreclr\MyCoreClrRM.dll

## <a name="more-details"></a>詳細

[PSEditions のスクリプト](script-psedition-support.md)

[PowerShellGallery での PSEditions のサポート](../how-to/finding-packages/searching-by-compatibility.md)

[モジュール マニフェストを更新する](/powershell/module/powershellget/update-modulemanifest)

[about_PowerShell_Editions][]

[about_PowerShell_Editions]: /powershell/module/Microsoft.PowerShell.Core/About/about_PowerShell_Editions
