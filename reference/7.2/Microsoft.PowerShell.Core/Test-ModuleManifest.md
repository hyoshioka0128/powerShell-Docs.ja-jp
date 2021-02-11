---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/test-modulemanifest?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-ModuleManifest
ms.openlocfilehash: c86a7253335b91011d2eb225bc53d1c5cc671aff
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602758"
---
# Test-ModuleManifest

## 概要
モジュールのマニフェスト ファイルにモジュールの内容が正確に記述されていることを確認します。

## SYNTAX

```
Test-ModuleManifest [-Path] <String> [<CommonParameters>]
```

## Description

**New-modulemanifest** コマンドレットは、モジュールマニフェスト (.psd1) ファイルに一覧表示されているファイルが、実際に指定されたパスにあることを確認します。

このコマンドレットは、モジュールの作成者がマニフェスト ファイルをテストできるようにすることを目的として設計されています。
モジュールユーザーは、スクリプトやコマンドでこのコマンドレットを使用して、モジュールに依存するスクリプトを実行する前にエラーを検出することもできます。

**New-modulemanifest** は、モジュールを表すオブジェクトを返します。
これは Get-Module が返すオブジェクトの型と同じです。
マニフェストに指定された場所にいずれかのファイルが存在しない場合、不足しているファイルごとにエラーが生成されます。

## 例

### 例 1: マニフェストをテストする

```powershell
test-ModuleManifest -Path "$pshome\Modules\TestModule.psd1"
```

このコマンドは、TestModule.psd1 モジュール マニフェストをテストします。

### 例 2: パイプラインを使用してマニフェストをテストする

```powershell
"$pshome\Modules\TestModule.psd1" | test-modulemanifest
```

```Output
Test-ModuleManifest : The specified type data file 'C:\Windows\System32\Wi
ndowsPowerShell\v1.0\Modules\TestModule\TestTypes.ps1xml' could not be processed because the file was not found. Please correct the path and try again.
At line:1 char:34
+ "$pshome\Modules\TestModule.psd1" | test-modulemanifest <<<<
+ CategoryInfo          : ResourceUnavailable: (C:\Windows\System32\WindowsPowerShell\v1.0\Modules\TestModule\TestTypes.ps1xml:String) [Test-ModuleManifest], FileNotFoundException
+ FullyQualifiedErrorId : Modules_TypeDataFileNotFound,Microsoft.PowerShell.Commands.TestModuleManifestCommandName

Name              : TestModule
Path              : C:\Windows\system32\WindowsPowerShell\v1.0\Modules\TestModule\TestModule.psd1
Description       :
Guid              : 6f0f1387-cd25-4902-b7b4-22cff6aefa7b
Version           : 1.0
ModuleBase        : C:\Windows\system32\WindowsPowerShell\v1.0\Modules\TestModule
ModuleType        : Manifest
PrivateData       :
AccessMode        : ReadWrite
ExportedAliases   : {}
ExportedCmdlets   : {}
ExportedFunctions : {}
ExportedVariables : {}
NestedModules     : {}
```

このコマンドは、パイプライン演算子 (|) を使用してパス文字列を **new-modulemanifest** に送信します。

コマンド出力にはテストが失敗したことが示されています。これは、マニフェストに示されている TestTypes.ps1xml ファイルが見つからなかったためです。

### 例 3: モジュールマニフェストをテストする関数を記述する

```powershell
function Test-ManifestBool ($path)
```

```Output
{$a = dir $path | Test-ModuleManifest -ErrorAction SilentlyContinue; $?}
```

この関数は **new-modulemanifest** に似ていますが、ブール値を返します。
関数は、マニフェストがテストに合格した場合は $True を返し、それ以外の場合は $False を返します。

関数は、Get-ChildItem コマンドレット alias = dir を使用して、$path 変数によって指定されたモジュールマニフェストを取得します。
このコマンドは、パイプライン演算子 (|) を使用して、ファイルオブジェクトを **new-modulemanifest** に渡します。

**New-modulemanifest** は、 *erroraction* 共通パラメーターに SilentlyContinue の値を使用して、コマンドによって生成されるエラーの表示を抑制します。
また、 **PSModuleInfo** が $a 変数に返す **new-modulemanifest** オブジェクトも保存します。
そのため、オブジェクトは表示されません。

次に、別のコマンドで、関数は $? の値を表示します。
自動変数。
前のコマンドでエラーが生成されなかった場合、コマンドは $True を表示し、それ以外の場合は $False します。

この関数は、条件付きステートメントで使用できます。たとえば、 **インポートモジュール** のコマンドの前にある場合もあれば、モジュールを使用するコマンドの場合もあります。

## PARAMETERS

### -Path

マニフェストファイルのパスとファイル名を指定します。
.Psd1 ファイル名拡張子を持つモジュールマニフェストファイルのパスと名前を入力します (省略可能)。
既定の場所は、現在のディレクトリです。
ワイルドカード文字はサポートされていますが、1つのモジュールマニフェストファイルに解決する必要があります。
このパラメーターは必須です。
パイプを使用してパスを **new-modulemanifest** にパイプすることもできます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用してモジュールマニフェストへのパスをこのコマンドレットに渡します。

## 出力

### PSModuleInfo (システム管理)

このコマンドレットは、モジュールを表す **PSModuleInfo** オブジェクトを返します。
このオブジェクトは、マニフェストにエラーがある場合でも返されます。

## 注

## 関連リンク

[Export-ModuleMember](Export-ModuleMember.md)

[Get-Module](Get-Module.md)

[Import-Module](Import-Module.md)

[New-Module](New-Module.md)

[New-ModuleManifest](New-ModuleManifest.md)

[Remove-Module](Remove-Module.md)

[about_Modules](About/about_Modules.md)

