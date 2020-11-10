---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 04/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-modulemanifest?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-ModuleManifest
ms.openlocfilehash: 50a3d85b3d598653a03bc6bf8e82c249bba42759
ms.sourcegitcommit: 9b28fb9a3d72655bb63f62af18b3a5af6a05cd3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/07/2020
ms.locfileid: "93211971"
---
# New-ModuleManifest

## 概要
新しいモジュール マニフェストを作成します。

## SYNTAX

### All

```
New-ModuleManifest [-Path] <string> [-NestedModules <Object[]>] [-Guid <guid>] [-Author <string>]
 [-CompanyName <string>] [-Copyright <string>] [-RootModule <string>] [-ModuleVersion <version>]
 [-Description <string>] [-ProcessorArchitecture <ProcessorArchitecture>]
 [-PowerShellVersion <version>] [-ClrVersion <version>] [-DotNetFrameworkVersion <version>]
 [-PowerShellHostName <string>] [-PowerShellHostVersion <version>] [-RequiredModules <Object[]>]
 [-TypesToProcess <string[]>] [-FormatsToProcess <string[]>] [-ScriptsToProcess <string[]>]
 [-RequiredAssemblies <string[]>] [-FileList <string[]>] [-ModuleList <Object[]>]
 [-FunctionsToExport <string[]>] [-AliasesToExport <string[]>] [-VariablesToExport <string[]>]
 [-CmdletsToExport <string[]>] [-DscResourcesToExport <string[]>] [-CompatiblePSEditions <string[]>]
 [-PrivateData <Object>] [-Tags <string[]>] [-ProjectUri <uri>] [-LicenseUri <uri>] [-IconUri <uri>]
 [-ReleaseNotes <string>] [-HelpInfoUri <string>] [-PassThru] [-DefaultCommandPrefix <string>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

この `New-ModuleManifest` コマンドレットは、新しいモジュールマニフェスト () ファイルを作成し `.psd1` 、その値を設定して、指定されたパスにマニフェストファイルを保存します。

モジュールの作成者は、このコマンドレットを使用して、モジュールのマニフェストを作成します。 モジュールマニフェストは、 `.psd1` ハッシュテーブルを含むファイルです。 モジュールの内容や属性について説明するハッシュ テーブル内のキーと値が、前提条件を定義し、コンポーネントの処理方法を決定します。 モジュールにはマニフェストは必要ありません。

`New-ModuleManifest` 共通に使用されるすべてのマニフェストキーを含むマニフェストを作成します。そのため、既定の出力をマニフェストテンプレートとして使用できます。 値を追加または変更したり、このコマンドレットでは追加されないモジュールキーを追加したりするには、結果のファイルをテキストエディターで開きます。

**Path** および **PassThru** を除き、各パラメーターは、モジュールマニフェストキーとその値を作成します。
モジュール マニフェストでは、 **ModuleVersion** キーのみが必須です。 パラメーターの説明で指定されていない限り、コマンドからパラメーターを省略すると、によっ `New-ModuleManifest` て影響を与えることなく、関連付けられた値のコメント文字列がによって作成されます。

PowerShell 2.0 では、では、 `New-ModuleManifest` 必須のパラメーター値に加えて、コマンドで指定されていない一般的に使用されるパラメーターの値を入力するように求められます。 PowerShell 3.0 以降では、 `New-ModuleManifest` 必要なパラメーター値が指定されていない場合にのみプロンプトが表示されます。

PowerShell ギャラリーにモジュールを発行する予定の場合、マニフェストには特定のプロパティの値が含まれている必要があります。 詳細については、ギャラリーのドキュメントの「 [PowerShell ギャラリーに発行された項目に必要なメタデータ](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package#required-metadata-for-items-published-to-the-powershell-gallery) 」を参照してください。

## 例

### 例 1-新しいモジュールマニフェストを作成する

この例では、 **Path** パラメーターで指定したファイルに新しいモジュールマニフェストを作成します。
**PassThru** パラメーターは、出力をパイプラインとファイルに送信します。

出力は、マニフェスト内のすべてのキーの既定値を示しています。

```powershell
New-ModuleManifest -Path C:\ps-test\Test-Module\Test-Module.psd1 -PassThru
```

```Output
#
# Module manifest for module 'Test-Module'
#
# Generated by: ContosoAdmin
#
# Generated on: 1/22/2019
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '1.0'

# Supported PSEditions
# CompatiblePSEditions = @()

# ID used to uniquely identify this module
GUID = '47179120-0bcb-4f14-8d80-f4560107f85c'

# Author of this module
Author = 'ContosoAdmin'

# Company or vendor of this module
CompanyName = 'Unknown'

# Copyright statement for this module
Copyright = '(c) 2019 ContosoAdmin. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the Windows PowerShell engine required by this module
# PowerShellVersion = ''

# Name of the Windows PowerShell host required by this module
# PowerShellHostName = ''

# Minimum version of the Windows PowerShell host required by this module
# PowerShellHostVersion = ''

# Minimum version of Microsoft .NET Framework required by this module. This prerequisite is valid for the PowerShell Desktop edition only.
# DotNetFrameworkVersion = ''

# Minimum version of the common language runtime (CLR) required by this module. This prerequisite is valid for the PowerShell Desktop edition only.
# CLRVersion = ''

# Processor architecture (None, X86, Amd64) required by this module
# ProcessorArchitecture = ''

# Modules that must be imported into the global environment prior to importing this module
# RequiredModules = @()

# Assemblies that must be loaded prior to importing this module
# RequiredAssemblies = @()

# Script files (.ps1) that are run in the caller's environment prior to importing this module.
# ScriptsToProcess = @()

# Type files (.ps1xml) to be loaded when importing this module
# TypesToProcess = @()

# Format files (.ps1xml) to be loaded when importing this module
# FormatsToProcess = @()

# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
# NestedModules = @()

# Functions to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no functions to export.
FunctionsToExport = @()

# Cmdlets to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no cmdlets to export.
CmdletsToExport = @()

# Variables to export from this module
VariablesToExport = '*'

# Aliases to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no aliases to export.
AliasesToExport = @()

# DSC resources to export from this module
# DscResourcesToExport = @()

# List of all modules packaged with this module
# ModuleList = @()

# List of all files packaged with this module
# FileList = @()

# Private data to pass to the module specified in RootModule/ModuleToProcess. This may also contain a PSData hashtable with additional module metadata used by PowerShell.
PrivateData = @{

    PSData = @{

        # Tags applied to this module. These help with module discovery in online galleries.
        # Tags = @()

        # A URL to the license for this module.
        # LicenseUri = ''

        # A URL to the main website for this project.
        # ProjectUri = ''

        # A URL to an icon representing this module.
        # IconUri = ''

        # ReleaseNotes of this module
        # ReleaseNotes = ''

    } # End of PSData hashtable

} # End of PrivateData hashtable

# HelpInfo URI of this module
# HelpInfoURI = ''

# Default prefix for commands exported from this module. Override the default prefix using Import-Module -Prefix.
# DefaultCommandPrefix = ''

}
```

### 例 2-いくつかの事前設定される設定を使用して新しいマニフェストを作成する

この例では、新しいモジュールマニフェストを作成します。 **PowerShellVersion** および **AliasesToExport** 　パラメーターを使用して、値を対応するマニフェスト キーに追加します。

```powershell
New-ModuleManifest -PowerShellVersion 1.0 -AliasesToExport JKBC, DRC, TAC -Path C:\ps-test\ManifestTest.psd1
```

### 例 3-他のモジュールを必要とするマニフェストを作成する

この例では、文字列形式を使用して **BitsTransfer** モジュールの名前を指定し、ハッシュテーブル形式を使用して、 **psscheduledjob** モジュールの名前、 **GUID** 、およびバージョンを指定します。

```powershell
$moduleSettings = @{
  RequiredModules = ("BitsTransfer", @{
    ModuleName="PSScheduledJob"
    ModuleVersion="1.0.0.0";
    GUID="50cdb55f-5ab7-489f-9e94-4ec21ff51e59"
  })
  Path = 'C:\ps-test\ManifestTest.psd1'
}
New-ModuleManifest @moduleSettings
```

この例では、 **Modulelist** 、 **RequiredModules** 、および **nestedmodules** パラメーターの文字列形式とハッシュテーブル形式を使用する方法を示します。 同じパラメーター値に、文字列とハッシュ テーブルを組み合わせることができます。

### 例 4-更新可能なヘルプをサポートするマニフェストを作成する

この例では、 **helpinfouri** パラメーターを使用して、モジュールマニフェストに **helpinfouri** キーを作成します。 パラメーターの値とキーは、 **http** または **https** で始まる必要があります。 この値は、モジュールの HelpInfo XML 更新可能なヘルプの情報ファイルを検索するための更新可能なヘルプ システムを示しています。

```powershell
$moduleSettings = @{
  HelpInfoUri = 'http://https://go.microsoft.com/fwlink/?LinkID=603'
  Path = 'C:\ps-test\ManifestTest.psd1'
}
New-ModuleManifest @moduleSettings
```

更新可能なヘルプの詳細については、「 [about_Updatable_Help](./About/about_Updatable_Help.md)」を参照してください。
HelpInfo XML ファイルの詳細については、「 [更新可能なヘルプのサポート](/powershell/scripting/developer/module/supporting-updatable-help)」を参照してください。

### 例 5-モジュール情報を取得する

この例では、モジュールの構成値を取得する方法を示します。 モジュールマニフェスト内の値は、モジュールオブジェクトのプロパティの値に反映されます。

`Get-Module`コマンドレットを使用して、 **List** パラメーターを使用して、 **PowerShell 診断** モジュールを取得します。 このコマンドは、モジュールをコマンドレットに送信して、 `Format-List` モジュールオブジェクトのすべてのプロパティと値を表示します。

```powershell
Get-Module Microsoft.PowerShell.Diagnostics -List | Format-List -Property *
```

```Output
LogPipelineExecutionDetails : False
Name                        : Microsoft.PowerShell.Diagnostics
Path                        : C:\Windows\system32\WindowsPowerShell\v1.0\Modules\Microsoft.PowerShell.Diagnostics\Micro
                              soft.PowerShell.Diagnostics.psd1
Definition                  :
Description                 :
Guid                        : ca046f10-ca64-4740-8ff9-2565dba61a4f
HelpInfoUri                 : https://go.microsoft.com/fwlink/?LinkID=210596
ModuleBase                  : C:\Windows\system32\WindowsPowerShell\v1.0\Modules\Microsoft.PowerShell.Diagnostics
PrivateData                 :
Version                     : 3.0.0.0
ModuleType                  : Manifest
Author                      : Microsoft Corporation
AccessMode                  : ReadWrite
ClrVersion                  : 4.0
CompanyName                 : Microsoft Corporation
Copyright                   : Microsoft Corporation. All rights reserved.
DotNetFrameworkVersion      :
ExportedFunctions           : {}
ExportedCmdlets             : {[Get-WinEvent, Get-WinEvent], [Get-Counter, Get-Counter], [Import-Counter,
                              Import-Counter], [Export-Counter, Export-Counter]...}
ExportedCommands            : {[Get-WinEvent, Get-WinEvent], [Get-Counter, Get-Counter], [Import-Counter,
                              Import-Counter], [Export-Counter, Export-Counter]...}
FileList                    : {}
ModuleList                  : {}
NestedModules               : {}
PowerShellHostName          :
PowerShellHostVersion       :
PowerShellVersion           : 3.0
ProcessorArchitecture       : None
Scripts                     : {}
RequiredAssemblies          : {}
RequiredModules             : {}
RootModule                  :
ExportedVariables           : {}
ExportedAliases             : {}
ExportedWorkflows           : {}
SessionState                :
OnRemove                    :
ExportedFormatFiles         : {C:\Windows\system32\WindowsPowerShell\v1.0\Event.format.ps1xml,
                              C:\Windows\system32\WindowsPowerShell\v1.0\Diagnostics.format.ps1xml}
ExportedTypeFiles           : {C:\Windows\system32\WindowsPowerShell\v1.0\GetEvent.types.ps1xml}
```

## PARAMETERS

### -Aseasestoexport

モジュールがエクスポートするエイリアスを指定します。 ワイルドカードを使用できます。

このパラメーターを使用して、モジュールによってエクスポートされるエイリアスを制限できます。 エクスポートされたエイリアスの一覧からエイリアスを削除できますが、一覧にエイリアスを追加することはできません。

このパラメーターを省略した場合は、によって、 `New-ModuleManifest` **AliasesToExport** `*` モジュールで定義されているすべてのエイリアスがマニフェストによってエクスポートされることを示す値 (all) を使用して、指定されたキーが作成されます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: * (all)
Accept pipeline input: False
Accept wildcard characters: True
```

### -作成者

モジュールの作成者を指定します。

このパラメーターを省略すると、では、 `New-ModuleManifest` 現在のユーザーの名前を使用して **作成者** キーが作成されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Name of the current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -ClrVersion

モジュールに必要な、Microsoft .NET Framework の共通言語ランタイム (CLR) の最低限のバージョンを指定します。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -///エクスポート

モジュールがエクスポートするコマンドレットを指定します。 ワイルドカードを使用できます。

このパラメーターを使用して、モジュールによってエクスポートされるコマンドレットを制限できます。 エクスポートされたコマンドレットの一覧からコマンドレットを削除できますが、一覧にコマンドレットを追加することはできません。

このパラメーターを省略すると、によって、 `New-ModuleManifest` モジュールで定義されているすべてのコマンドレットがマニフェストによってエクスポートされることを意味する、(すべて) の値を持つコマンドレット **stoexport** キーが作成さ `*` れます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: * (all)
Accept pipeline input: False
Accept wildcard characters: True
```

### -CompanyName

モジュールを作成した企業またはベンダーを識別します。

このパラメーターを省略した場合、では、 `New-ModuleManifest` "Unknown" という値を持つ **CompanyName** キーが作成されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: "Unknown"
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -CompatiblePSEditions

モジュールの互換性のある PSEditions を指定します。 PSEdition の詳細については、「 [モジュールと互換性のある PowerShell のエディション](/powershell/scripting/gallery/concepts/module-psedition-support)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:
Accepted values: Desktop, Core

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Copyright

モジュールの著作権表記を指定します。

このパラメーターを省略すると、では、 `New-ModuleManifest` 値がの **著作権** キーが作成され `(c) <year> <username>. All rights reserved.` ます。ここで、 `<year>` は現在の年、 `<username>` は **Author** キーの値です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: (c) <year> <username>. All rights reserved.
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description

モジュールの内容について説明します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DotNetFrameworkVersion

モジュールに必要な、Microsoft .NET Framework の最低限のバージョンを指定します。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DscResourcesToExport

モジュールによってエクスポートされる Desired State Configuration (DSC) リソースを指定します。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -FileList

モジュールに含まれているすべての項目を指定します。

このキーは、モジュール インベントリとしての動作を想定して設計されています。 このキーに記載されているファイルはモジュールの発行時に含まれますが、すべての関数は自動的にエクスポートされません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FormatsToProcess

`.ps1xml`モジュールのインポート時に実行される書式設定ファイル () を指定します。

モジュールをインポートすると、PowerShell は `Update-FormatData` 指定されたファイルを使用してコマンドレットを実行します。
書式設定ファイルはスコープが設定されていないため、セッションのすべてのセッション状態に影響します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FunctionsToExport

モジュールがエクスポートする関数を指定します。 ワイルドカードを使用できます。

このパラメーターを使用して、モジュールによってエクスポートされる関数を制限できます。 エクスポートされたエイリアスの一覧から関数を削除できますが、一覧に関数を追加することはできません。

このパラメーターを省略した場合、に `New-ModuleManifest` よって **Functionstoexport** キーが作成されます。これは、 `*` モジュールで定義されているすべての関数がマニフェストによってエクスポートされることを意味します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: * (all)
Accept pipeline input: False
Accept wildcard characters: True
```

### -Guid

モジュールの一意の識別子を指定します。 **GUID** は、同じ名前のモジュールを区別するために使用できます。

このパラメーターを省略すると、は `New-ModuleManifest` マニフェストに **guid** キーを作成し、値の **guid** を生成します。

PowerShell で新しい **GUID** を作成するには、「」と入力 `[guid]::NewGuid()` します。

```yaml
Type: System.Guid
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: A GUID generated for the module
Accept pipeline input: False
Accept wildcard characters: False
```

### -HelpInfoUri

モジュールの HelpInfo XML ファイルのインターネットアドレスを指定します。 **Http** または **https** で始まる Uniform Resource Identifier (URI) を入力します。

HelpInfo XML ファイルは、PowerShell 3.0 で導入された更新可能なヘルプ機能をサポートしています。 これには、モジュールのダウンロード可能なヘルプ ファイルの場所、およびサポートされている各ロケールの最新のヘルプ ファイルのバージョン番号に関する情報が含まれます。

更新可能なヘルプの詳細については、「 [about_Updatable_Help](./About/about_Updatable_Help.md)」を参照してください。
HelpInfo XML ファイルの詳細については、「 [更新可能なヘルプのサポート](/powershell/scripting/developer/module/supporting-updatable-help)」を参照してください。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IconUri

モジュールのアイコンの URL を指定します。 指定されたアイコンは、モジュールのギャラリー web ページに表示されます。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LicenseUri

モジュールのライセンス条項の URL を指定します。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModuleList

このモジュールに含まれているすべてのモジュールを一覧表示します。

各モジュールの名前を文字列として、または **ModuleName** 　キーと **ModuleVersion** キーを含むハッシュ テーブルとして入力します。 ハッシュ テーブルは、オプションの **GUID** キーも保持できます。 パラメーター値として文字列とハッシュ テーブルを組み合わせることができます。

このキーは、モジュール インベントリとしての動作を想定して設計されています。 このキーの値に示されているモジュールは自動的に処理されません。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModuleVersion

モジュールのバージョンを指定します。

このパラメーターは必須ではありませんが、マニフェストには **ModuleVersion** キーが必要です。 このパラメーターを省略すると、では、 `New-ModuleManifest` 値が1.0 の **ModuleVersion** キーが作成されます。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 1.0
Accept pipeline input: False
Accept wildcard characters: False
```

### -NestedModules

`.psm1` `.dll` モジュールのセッション状態にインポートされるスクリプトモジュール () とバイナリモジュール () を指定します。 **Nestedmodules** キー内のファイルは、値に示されている順序で実行されます。

各モジュールの名前を文字列として、または **ModuleName** 　キーと **ModuleVersion** キーを含むハッシュ テーブルとして入力します。 ハッシュ テーブルは、オプションの **GUID** キーも保持できます。 パラメーター値として文字列とハッシュ テーブルを組み合わせることができます。

通常、入れ子になったモジュールには、ルート モジュールが内部処理のために必要とするコマンドが含まれています。
既定では、入れ子になったモジュール内のコマンドは、モジュールのセッション状態から呼び出し元のセッション状態にエクスポートされますが、ルートモジュールは、エクスポートするコマンドを制限できます。 たとえば、コマンドを使用し `Export-ModuleMember` ます。

モジュールセッション状態の入れ子になったモジュールは、ルートモジュールで使用できますが、 `Get-Module` 呼び出し元のセッション状態のコマンドからは返されません。

`.ps1` **Nestedmodules** キーに記載されているスクリプト () は、呼び出し元のセッション状態ではなく、モジュールのセッション状態で実行されます。 呼び出し元のセッション状態でスクリプトを実行するには、マニフェストの **ScriptsToProcess** キーの値にスクリプト ファイル名を列挙します。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

結果のモジュールマニフェストをコンソールに書き込み、ファイルを作成し `.psd1` ます。 既定では、このコマンドレットは出力を生成しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

新しいモジュール マニフェストのパスとファイル名を指定します。 ファイル名拡張子の付いたパスとファイル名を入力し `.psd1` ます。たとえば、のように `$pshome\Modules\MyModule\MyModule.psd1` なります。 **Path** パラメーターは必須です。

既存のファイルへのパスを指定すると、ファイルに `New-ModuleManifest` 読み取り専用属性が含まれていない限り、は警告なしでファイルを置き換えます。

マニフェストはモジュールのディレクトリに配置する必要があります。マニフェストファイル名はモジュールディレクトリ名と同じである必要がありますが、 `.psd1` ファイル名拡張子が付きます。

> [!NOTE]
> `$PSHOME` `$HOME` **パス** パラメーター値のプロンプトに応答して、やなどの変数を使用することはできません。 変数を使用するには、コマンドに **Path** パラメーターを含めます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PowerShellHostName

モジュールに必要な PowerShell ホストプログラムの名前を指定します。 ホストプログラムの名前 ( **Windows PowerShell ISE host** や **consolehost** など) を入力します。 ワイルドカードは使用できません。

ホストプログラムの名前を検索するには、プログラムで「」と入力 `$Host.Name` します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PowerShellHostVersion

モジュールで動作する PowerShell ホストプログラムの最小バージョンを指定します。 1.1 などのバージョン番号を入力します。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PowerShellVersion

このモジュールで動作する PowerShell の最小バージョンを指定します。 たとえば、パラメーターの値として1.0、2.0、または3.0 を入力できます。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PrivateData

インポート時にモジュールに渡されるデータを指定します。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProcessorArchitecture

モジュールに必要なプロセッサ アーキテクチャを指定します。 有効な値は、x86、AMD64、IA64、MSIL、および None (不明または未指定) です。

```yaml
Type: System.Reflection.ProcessorArchitecture
Parameter Sets: (All)
Aliases:
Accepted values: None, MSIL, X86, IA64, Amd64, Arm

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProjectUri

このプロジェクトについての web ページの URL を指定します。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReleaseNotes

リリースノートを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredAssemblies

モジュールに必要なアセンブリ () ファイルを指定し `.dll` ます。 アセンブリ ファイル名を入力します。
PowerShell は、型または形式を更新する前、入れ子になったモジュールをインポートする前、または **RootModule** キーの値に指定されているモジュールファイルをインポートする前に、指定されたアセンブリを読み込みます。

このパラメーターを使用して、アセンブリが **NestedModules** キーでバイナリ モジュールとして列挙されている場合でも、 **FormatsToProcess** キーまたは **TypesToProcess** キーに列挙される形式または型ファイルを更新するために読み込む必要があるアセンブリを含め、モジュールに必要なすべてのアセンブリを列挙できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredModules

グローバル セッション状態にする必要があるモジュールを指定します。 必要なモジュールがグローバルセッション状態でない場合は、PowerShell によってインポートされます。 必要なモジュールが使用できない場合、 `Import-Module` コマンドは失敗します。

各モジュールの名前を文字列として、または **ModuleName** 　キーと **ModuleVersion** キーを含むハッシュ テーブルとして入力します。 ハッシュ テーブルは、オプションの **GUID** キーも保持できます。 パラメーター値として文字列とハッシュ テーブルを組み合わせることができます。

PowerShell 2.0 では、は `Import-Module` 必要なモジュールを自動的にインポートしません。 必要なモジュールがグローバル セッション状態であることを確認するだけです。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptsToProcess

`.ps1`モジュールをインポートするときに、呼び出し元のセッション状態で実行されるスクリプト () ファイルを指定します。
ログイン スクリプトを使用する場合と同様に、これらのスクリプトを使用して、環境を準備できます。

モジュールのセッション状態で実行するスクリプトを指定するには、 **NestedModules** キーを使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -タグ

タグの配列を指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypesToProcess

`.ps1xml`モジュールをインポートするときに実行される型ファイル () を指定します。

モジュールをインポートすると、PowerShell は `Update-TypeData` 指定されたファイルを使用してコマンドレットを実行します。
型ファイルはスコープが設定されていないため、セッションのすべてのセッション状態に影響します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -変数 Stoexport

モジュールがエクスポートする変数を指定します。 ワイルドカードを使用できます。

このパラメーターを使用して、モジュールによってエクスポートされる変数を制限できます。 エクスポートされた変数の一覧から変数を削除することはできますが、一覧に変数を追加することはできません。

このパラメーターを省略した場合、によって、 `New-ModuleManifest` (all) の値を持つ変数 **stoexport** キーが作成され `*` ます。これは、モジュールで定義されているすべての変数がマニフェストによってエクスポートされることを意味します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: * (all)
Accept pipeline input: False
Accept wildcard characters: True
```

### -DefaultCommandPrefix

セッションにインポートするときに、モジュール内のすべてのコマンドの名詞の前に付加されるプレフィックスを指定します。 プレフィックス文字列を入力します。 プレフィックスは、ユーザーのセッションでのコマンド名の競合を防ぎます。

モジュールユーザーは、コマンドレットの **prefix** パラメーターを指定することによって、このプレフィックスをオーバーライドでき `Import-Module` ます。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RootModule

モジュールのプライマリファイルまたはルートファイルを指定します。 スクリプト ( `.ps1` )、スクリプトモジュール ( `.psm1` )、モジュールマニフェスト ( `.psd1` )、アセンブリ ( `.dll` )、コマンドレット定義 XML ファイル ()、 `.cdxml` またはワークフロー ( `.xaml` ) のファイル名を入力します。 モジュールのインポート時に、ルート モジュール ファイルからエクスポートされるメンバーは、呼び出し元のセッション状態にインポートされます。

モジュールにマニフェストファイルがあり、 **RootModule** キーでルートファイルが指定されていない場合、マニフェストはモジュールのプライマリファイルになり、モジュールはマニフェストモジュール (ModuleType = manifest) になります。

マニフェストを持つモジュール内のまたはファイルからメンバーをエクスポートするに `.psm1` は、 `.dll` マニフェスト内の **RootModule** または **nestedmodules** キーの値でこれらのファイルの名前を指定する必要があります。 それ以外の場合、メンバーはエクスポートされません。

> [!NOTE]
> PowerShell 2.0 では、このキーは **ModuleToProcess** と呼ばれていました。 **RootModule** パラメーター名またはその **ModuleToProcess** エイリアスを使用できます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: ModuleToProcess

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

が実行された場合に何が起こるか `New-ModuleManifest` を示します。 コマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットに入力をパイプすることはできません。

## 出力

### None または System.String

既定では、は `New-ModuleManifest` 出力を生成しません。 ただし、 **PassThru** パラメーターを使用すると、モジュールマニフェストを表す **system.string** オブジェクトが生成されます。

## 注

`New-ModuleManifest``.psd1` **UTF16** としてエンコードされたモジュールマニフェスト () ファイルを作成します。

モジュール マニフェストは、通常、オプションです。 ただし、モジュール マニフェストは、グローバル アセンブリ キャッシュにインストールされているアセンブリをエクスポートするために必要です。

ディレクトリ内のファイルを追加または変更するには、 `$pshome\Modules` [ **管理者として実行** ] オプションを使用して PowerShell を起動します。

PowerShell 2.0 では、 `New-ModuleManifest` モジュールマニフェストで要求されなかった場合でも、の多くのパラメーターが必須でした。 PowerShell 3.0 以降では、 **Path** パラメーターだけが必須です。

セッションは、PowerShell 実行環境のインスタンスです。 セッションは、1 つまたは複数のセッション状態の場合があります。 既定では、セッションはグローバルなセッション状態のみですが、インポートされた各モジュールは独自のセッション状態があります。 セッション状態によって、モジュール内のコマンドは、グローバルなセッション状態に影響を与えずに実行できます。

呼び出し元のセッション状態は、モジュールのインポート先のセッション状態です。 通常は、グローバルなセッション状態を指しますが、モジュールが入れ子になったモジュールをインポートする場合、呼び出し元はモジュールであり、呼び出し元のセッション状態はモジュールのセッション状態です。

## 関連リンク

[Export-ModuleMember](Export-ModuleMember.md)

[Get-Module](Get-Module.md)

[Import-Module](Import-Module.md)

[New-Module](New-Module.md)

[Remove-Module](Remove-Module.md)

[Test-ModuleManifest](Test-ModuleManifest.md)

[about_Modules](./About/about_Modules.md)