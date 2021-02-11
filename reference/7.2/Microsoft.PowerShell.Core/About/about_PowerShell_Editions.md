---
description: PowerShell のさまざまなエディションは、基になるさまざまなランタイムで実行されます。
Locale: en-US
ms.date: 03/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_editions?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PowerShell_Editions
ms.openlocfilehash: 3b1b938874b36919a1a0627f3b492cbc961e4a2f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601228"
---
# <a name="about-powershell-editions"></a>PowerShell のエディションについて

## <a name="short-description"></a>簡単な説明
PowerShell のさまざまなエディションは、基になるさまざまなランタイムで実行されます。

## <a name="long-description"></a>長い説明

PowerShell 5.1 では、それぞれが異なる .NET ランタイムで実行される PowerShell の複数の *エディション* があります。 PowerShell 6.2 の場合、PowerShell には次の2つのエディションがあります。

- **デスクトップ**。 .NET Framework で実行されます。 PowerShell 4 以降、および Windows デスクトップ、Windows Server、Windows Server Core、その他のほとんどの Windows オペレーティングシステムなどの全機能を備えた windows オペレーティングシステムの PowerShell 5.1 は、Desktop edition です。
  これは、元の PowerShell エディションです。
- **コア**。 .net core で実行されます。 PowerShell 6.0 以降、および .NET Framework が使用できない windows Nano Server や Windows IoT などのフットプリントが縮小された windows エディションの PowerShell 5.1。

PowerShell のエディションは .NET ランタイムに対応しているため、.NET API と PowerShell モジュールの互換性の主要な指標となります。.net の Api、型、またはメソッドの中には、両方の .NET ランタイムで使用できないものがあります。これは、それらに依存する PowerShell スクリプトとモジュールに影響します。

## <a name="the-psedition-automatic-variable"></a>`$PSEdition`自動変数

PowerShell 5.1 以降では、自動変数を使用して実行しているエディションを調べることができ `$PSEdition` ます。

```powershell
$PSEdition
```

```Output
Core
```

PowerShell 4 以降では、この変数は存在しません。 `$PSEdition` null の場合は、値を持つのと同じように処理する必要があり `Desktop` ます。

### <a name="edition-in-psversiontable"></a>のエディション `$PSVersionTable`

自動変数には、 `$PSVersionTable` PowerShell 5.1 以降のエディション情報も含まれています。

```powershell
$PSVersionTable
```

```Output
Name                           Value
----                           -----
PSVersion                      6.2.0-rc.1
PSEdition                      Core           # <-- Edition information
GitCommitId                    6.2.0-rc.1
OS                             Microsoft Windows 10.0.18865
Platform                       Win32NT
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

フィールドには、 `PSEdition` 自動変数と同じ値が設定され `$PSEdition` ます。

## <a name="the-compatiblepseditions-module-manifest-field"></a>`CompatiblePSEditions`モジュールマニフェストフィールド

PowerShell モジュールでは、モジュールマニフェストのフィールドを使用して、互換性のある PowerShell のエディションを宣言でき `CompatiblePSEditions` ます。

たとえば、PowerShell のとの両方のエディションとの互換性を宣言するモジュールマニフェストは `Desktop` `Core` 次のようになります。

```powershell
@{
    ModuleVersion = '1.0'
    FunctionsToExport = @('Test-MyModule')
    CompatiblePSEditions = @('Desktop', 'Core')
}
```

互換性のみを持つモジュールマニフェストの例を `Desktop` 次に示します。

```powershell
@{
    ModuleVersion = '1.0'
    FunctionsToExport = @('Test-MyModule')
    CompatiblePSEditions = @('Desktop')
}
```

`CompatiblePSEditions`モジュールマニフェストからフィールドを除外すると、そのフィールドをに設定した場合と同じ効果が得られ `Desktop` ます。このフィールドを導入する前に作成されたモジュールは、このエディションに対して暗黙的に記述されているためです。

Windows の一部として出荷されていないモジュール (つまり、ギャラリーから作成またはインストールするモジュール) では、このフィールドは情報提供のみを目的としています。PowerShell では、フィールドに基づく動作は変更されません `CompatiblePSEditions` が、 `PSModuleInfo` `Get-Module` 独自のロジックに対して (によって返される) オブジェクトで公開されます。

```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion '5.1'
$ModuleInfo = Test-ModuleManifest -Path .\TestModuleWithEdition.psd1
$ModuleInfo.CompatiblePSEditions
```

```Output
Desktop
Core
```

> [!NOTE]
> `CompatiblePSEditions`モジュールフィールドは、PowerShell 5.1 以降とのみ互換性があります。
> このフィールドを含めると、モジュールは PowerShell 4 以下と互換性がなくなります。
> フィールドは純粋な情報であるため、後の PowerShell バージョンでは省略できます。

PowerShell 6.1 で `Get-Module -ListAvailable` は、各モジュールのエディション互換性を表示するために、フォーマッタが更新されています。

```PowerShell
Get-Module -ListAvailable
```

```Output

    Directory: C:\Users\me\Documents\PowerShell\Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Script     1.4.0      Az                                  Core,Desk
Script     1.3.1      Az.Accounts                         Core,Desk {Disable-AzDataCollection, Disable-AzContextAutosave, E...
Script     1.0.1      Az.Aks                              Core,Desk {Get-AzAks, New-AzAks, Remove-AzAks, Import-AzAksCreden...

...

Script     4.4.0      Pester                              Desk      {Describe, Context, It, Should...}
Script     1.18.0     PSScriptAnalyzer                    Desk      {Get-ScriptAnalyzerRule, Invoke-ScriptAnalyzer, Invoke-...
Script     1.0.0      WindowsCompatibility                Core      {Initialize-WinSession, Add-WinFunction, Invoke-WinComm...

```

## <a name="edition-compatibility-for-modules-that-ship-as-part-of-windows"></a>エディション-Windows の一部として出荷されるモジュールの互換性

Windows の一部として提供されるモジュール (または役割または機能の一部としてインストールされるモジュール) では、PowerShell 6.1 以降ではフィールドが異なる方法で処理され `CompatiblePSEditions` ます。 このようなモジュールは、Windows PowerShell のシステムモジュールディレクトリ () にあり `%windir%\System\WindowsPowerShell\v1.0\Modules` ます。

このディレクトリに読み込まれた、またはこのディレクトリにあるモジュールの場合、PowerShell 6.1 以降では、 `CompatiblePSEditions` モジュールが現在のセッションと互換性があるかどうかを判断するためにフィールドを使用し、それに応じて動作します。

を使用した場合 `Import-Module` 、に含まれていないモジュールはインポートされず、 `Core` `CompatiblePSEditions` エラーが表示されます。

```powershell
Import-Module BitsTransfer
```

```Output
Import-Module : Module 'C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\BitsTransfer\BitsTransfer.psd1' does not support current PowerShell edition 'Core'. Its supported editions are 'Desktop'. Use 'Import-Module -SkipEditionCheck' to ignore the compatibility of this module.
At line:1 char:1
+ Import-Module BitsTransfer
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : ResourceUnavailable: (C:\WINDOWS\system32\u2026r\BitsTransfer.psd1:String) [Import-Module], InvalidOperationException
+ FullyQualifiedErrorId : Modules_PSEditionNotSupported,Microsoft.PowerShell.Commands.ImportModuleCommand
```

`Get-Module -ListAvailable`を使用した場合、 `Core` に含まれていないモジュールは表示され `CompatiblePSEditions` ません。

```powershell
Get-Module -ListAvailable BitsTransfer
```

```Output
# No output
```

どちらの場合も、 `-SkipEditionCheck` スイッチパラメーターを使用して、この動作をオーバーライドできます。

```powershell
Get-Module -ListAvailable -SkipEditionCheck BitsTransfer
```

```Output

    Directory: C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Manifest   2.0.0.0    BitsTransfer                        Desk      {Add-BitsFile, Complete-BitsTransfer, Get-BitsTransfer,...

```

> [!WARNING]
> `Import-Module -SkipEditionCheck` モジュールに対して成功するように見えますが、そのモジュールを使用すると、後で非互換性が発生する可能性があります。最初にモジュールの読み込みが成功しても、コマンドは後で互換性のない API を呼び出し、自発的に失敗します。

## <a name="authoring-powershell-modules-for-edition-cross-compatibility"></a>エディション間の互換性のための PowerShell モジュールの作成

Powershell モジュールを作成 `Desktop` して powershell のエディションとエディションの両方を対象とする場合は `Core` 、エディション間の互換性を確保するために実行できることがあります。

ただし、互換性を確認し、継続的に検証する唯一の方法は、スクリプトまたはモジュールのテストを記述し、それらを PowerShell のすべてのバージョンおよびエディションで実行して互換性が必要な場合です。 この場合の推奨されるテストフレームワークは、 [次のよう][]になります。

### <a name="powershell-script"></a>PowerShell スクリプト

言語として、PowerShell はエディションによっても同じように機能します。これは、エディションの互換性によって影響を受ける、使用するコマンドレット、モジュール、.NET Api です。

一般に、PowerShell 6.1 以降で動作するスクリプトは Windows PowerShell 5.1 で動作しますが、いくつかの例外があります。

バージョン 1.18.0 [Psscriptanalyzer][] モジュールには [`PSUseCompatibleCommands`][] 、 [`PSUseCompatibleTypes`][] PowerShell スクリプトでコマンドと .net api の互換性のない使用方法を検出できるやのような規則があります。

### <a name="net-assemblies"></a>.NET アセンブリ

ソースコードから生成された .NET アセンブリ (Dll) を組み込んだバイナリモジュールまたはモジュールを作成する場合は、.NET と PowerShell API の互換性のコンパイル時の互換性検証のために、 [.NET Standard][] と [powershell の標準][] に対してコンパイルする必要があります。

これらのライブラリはコンパイル時に一部の互換性を確認できますが、エディション間の動作の違いを検出することはできません。 この場合も、テストを記述する必要があります。

## <a name="see-also"></a>関連項目

- [about_Automatic_Variables](about_Automatic_Variables.md)
- [Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)
- [Get-Module](xref:Microsoft.PowerShell.Core.Get-Module)
- [互換性のある PowerShell エディションが含まれるモジュール](/powershell/scripting/gallery/concepts/module-psedition-support)

[Pester]: https://github.com/pester/Pester/wiki/Pester
[PSScriptAnalyzer]: https://github.com/PowerShell/PSScriptAnalyzer
['PSUseCompatibleCommands']: https://github.com/PowerShell/PSScriptAnalyzer/blob/master/RuleDocumentation/UseCompatibleCommands.md
['PSUseCompatibleTypes']: https://github.com/PowerShell/PSScriptAnalyzer/blob/master/RuleDocumentation/UseCompatibleTypes.md
[.NET Standard]: /dotnet/standard/net-standard
[PowerShell Standard]: https://devblogs.microsoft.com/powershell/powershell-standard-library-build-single-module-that-works-across-windows-powershell-and-powershell-core/
