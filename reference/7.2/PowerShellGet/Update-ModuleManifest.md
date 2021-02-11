---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.date: 07/08/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/update-modulemanifest?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-ModuleManifest
ms.openlocfilehash: 4f00450257178cac72d03303358150fd9f5cd26b
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "99599289"
---
# Update-ModuleManifest

## 概要
モジュール マニフェスト ファイルを更新します。

## SYNTAX

### すべて

```
Update-ModuleManifest [-Path] <String> [-NestedModules <Object[]>] [-Guid <Guid>] [-Author <String>]
 [-CompanyName <String>] [-Copyright <String>] [-RootModule <String>] [-ModuleVersion <Version>]
 [-Description <String>] [-ProcessorArchitecture <ProcessorArchitecture>]
 [-CompatiblePSEditions <String[]>] [-PowerShellVersion <Version>] [-ClrVersion <Version>]
 [-DotNetFrameworkVersion <Version>] [-PowerShellHostName <String>]
 [-PowerShellHostVersion <Version>] [-RequiredModules <Object[]>] [-TypesToProcess <String[]>]
 [-FormatsToProcess <String[]>] [-ScriptsToProcess <String[]>] [-RequiredAssemblies <String[]>]
 [-FileList <String[]>] [-ModuleList <Object[]>] [-FunctionsToExport <String[]>]
 [-AliasesToExport <String[]>] [-VariablesToExport <String[]>] [-CmdletsToExport <String[]>]
 [-DscResourcesToExport <String[]>] [-PrivateData <Hashtable>] [-Tags <String[]>]
 [-ProjectUri <Uri>] [-LicenseUri <Uri>] [-IconUri <Uri>] [-ReleaseNotes <String[]>]
 [-Prerelease <String>] [-HelpInfoUri <Uri>] [-PassThru] [-DefaultCommandPrefix <String>]
 [-ExternalModuleDependencies <String[]>] [-PackageManagementProviders <String[]>]
 [-RequireLicenseAcceptance] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Update-ModuleManifest`コマンドレットでは、モジュールマニフェスト () ファイルを更新し `.psd1` ます。

## 例

### 例 1: モジュールマニフェストを更新する

この例では、既存のモジュールマニフェストファイルを更新します。 スプラッティングは、パラメーター値をに渡すために使用され `Update-ModuleManifest` ます。 詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md)」を参照してください。

```powershell
$Parms = @{
  Path = "C:\Test\TestManifest.psd1"
  Author = "TestUser1"
  CompanyName = "Contoso Corporation"
  Copyright = "(c) 2019 Contoso Corporation. All rights reserved."
}

Update-ModuleManifest @Parms
```

`$Parms` は、 **Path**、 **Author**、 **CompanyName**、 **Copyright** のパラメーター値を格納する記号です。 `Update-ModuleManifest` からパラメーター値を取得 `@Parms` し、モジュールマニフェスト ( **TestManifest.psd1**) を更新します。

## PARAMETERS

### -Aseasestoexport

モジュールがエクスポートするエイリアスを指定します。 ワイルドカードを使用できます。

モジュールによってエクスポートされるエイリアスを制限するには、このパラメーターを使用します。 この **エクスポート** では、エクスポートされたエイリアスの一覧からエイリアスを削除できますが、エイリアスを一覧に追加することはできません。

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

### -作成者

モジュールの作成者を指定します。

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

モジュールによってエクスポートされるコマンドレットを制限するには、このパラメーターを使用します。 コマンドレットを **エクスポート** すると、エクスポートしたコマンドレットの一覧からコマンドレットを削除できますが、コマンドレットを一覧に追加することはできません。

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

### -CompanyName

モジュールを作成した会社またはベンダーを指定します。

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

### -CompatiblePSEditions

モジュールの互換性のある **PSEditions** を指定します。 **PSEdition** の詳細については、「[モジュールと互換性のある PowerShell のエディション](/powershell/scripting/gallery/concepts/module-psedition-support)」を参照してください。

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

### -Confirm

実行前に確認メッセージを表示 `Update-ModuleManifest` します。

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

### -Copyright

モジュールの著作権表記を指定します。

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

### -DefaultCommandPrefix

既定のコマンドプレフィックスを指定します。

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

### -Description

モジュールの説明を指定します。

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
Accept wildcard characters: False
```

### -ExternalModuleDependencies

外部モジュールの依存関係の配列を指定します。

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

### -FileList

モジュールに含まれているすべての項目を指定します。

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

モジュールによってエクスポートされる関数を制限するには、このパラメーターを使用します。 **Functionstoexport** では、エクスポートされたエイリアスの一覧から関数を削除できますが、一覧に関数を追加することはできません。

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

### -Guid

モジュールの一意の識別子を指定します。 GUID は、同じ名前のモジュールを区別するために使用できます。

```yaml
Type: System.Guid
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -HelpInfoUri

モジュールの **HELPINFO XML** ファイルのインターネットアドレスを指定します。 **Http** または **https** で始まる Uniform Resource Identifier (URI) を入力します。

**HELPINFO XML** ファイルは、PowerShell バージョン3.0 で導入された更新可能なヘルプ機能をサポートしています。 これには、モジュールのダウンロード可能なヘルプファイルの場所、およびサポートされている各ロケールの最新のヘルプファイルのバージョン番号に関する情報が含まれます。

更新可能なヘルプの詳細については、「 [about_Updatable_Help](../Microsoft.PowerShell.Core/About/about_Updatable_Help.md)」を参照してください。
**HELPINFO XML** ファイルの詳細については、「[更新可能なヘルプのサポート](/powershell/scripting/developer/module/supporting-updatable-help)」を参照してください。

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

モジュールに含まれるモジュールの配列を指定します。

各モジュールの名前を文字列として、または **ModuleName**　キーと **ModuleVersion** キーを含むハッシュ テーブルとして入力します。 ハッシュ テーブルは、オプションの **GUID** キーも保持できます。 パラメーター値として文字列とハッシュ テーブルを組み合わせることができます。

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

### -NestedModules

`.psm1` `.dll` モジュールのセッション状態にインポートされるスクリプトモジュール () とバイナリモジュール () を指定します。 **Nestedmodules** キー内のファイルは、値に示されている順序で実行されます。

各モジュールの名前を文字列として、または **ModuleName**　キーと **ModuleVersion** キーを含むハッシュ テーブルとして入力します。 ハッシュ テーブルは、オプションの **GUID** キーも保持できます。 パラメーター値として文字列とハッシュ テーブルを組み合わせることができます。

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

### -PackageManagementProviders

パッケージ管理プロバイダーの配列を指定します。

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

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、は `Update-ModuleManifest` 出力を生成しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

モジュールマニフェストのパスとファイル名を指定します。 ファイル名拡張子の付いたパスとファイル名を入力し `.psd1` ます。たとえば、のように `$PSHOME\Modules\MyModule\MyModule.psd1` なります。

既存のファイルへのパスを指定すると、ファイルに `Update-ModuleManifest` 読み取り専用属性が含まれていない限り、は警告なしでファイルを置き換えます。

マニフェストはモジュールのディレクトリに配置する必要があります。マニフェストファイル名はモジュールディレクトリ名と同じである必要がありますが、 `.psd1` 拡張子はです。

`$PSHOME` `$HOME` **パス** パラメーター値のプロンプトに応答して、やなどの変数を使用することはできません。 変数を使用するには、コマンドに **Path** パラメーターを含めます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PowerShellHostName

モジュールに必要な PowerShell ホストプログラムの名前を指定します。 PowerShell ISE Host や ConsoleHost などのホストプログラムの名前を入力します。 ワイルドカードは使用できません。

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

このモジュールで動作する PowerShell の最小バージョンを指定します。 たとえば、このパラメーターの値として3.0、4.0、または5.0 を指定できます。

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

### -プレリリース

モジュールがプレリリースであることを示します。

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

### -PrivateData

インポート時にモジュールに渡されるデータを指定します。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProcessorArchitecture

モジュールに必要なプロセッサ アーキテクチャを指定します。

このパラメーターの有効値は、次のとおりです。

- Amd64
- Arm
- IA64
- MSIL
- なし (不明または未指定)
- X86

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

このバージョンのスクリプトで使用するリリースノートまたはコメントを格納する文字列配列を指定します。

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

### -RequireLicenseAcceptance

モジュールにライセンスへの同意が必要であることを指定します。

```yaml
Type: System.Management.Automation.SwitchParameter
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

このパラメーターを使用して、モジュールが必要とするすべてのアセンブリを指定します。これらのアセンブリが **nestedmodules** キーにバイナリモジュールとして表示されている場合でも、 **Formatstoprocess** または **TypesToProcess** キーに一覧表示されているフォーマットファイルまたは型ファイルを更新するために読み込む必要があるアセンブリも含まれます。

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

### -RootModule

モジュールのプライマリファイルまたはルートファイルを指定します。 スクリプト ( `.ps1` )、スクリプトモジュール ( `.psm1` )、モジュールマニフェスト ( `.psd1` )、アセンブリ ( `.dll` )、コマンドレット定義 XML ファイル ()、 `.cdxml` またはワークフロー ( `.xaml` ) のファイル名を入力します。 モジュールのインポート時に、ルート モジュール ファイルからエクスポートされるメンバーは、呼び出し元のセッション状態にインポートされます。

モジュールにマニフェストファイルがあり、 **RootModule** キーでルートファイルが指定されていない場合、マニフェストはモジュールのプライマリファイルになります。 また、モジュールはマニフェストモジュール (ModuleType = Manifest) になります。

マニフェストを持つモジュール内のまたはファイルからメンバーをエクスポートするに `.psm1` は、 `.dll` マニフェスト内の **RootModule** または **nestedmodules** キーの値でこれらのファイルの名前を指定する必要があります。 それ以外の場合、メンバーはエクスポートされません。

PowerShell 2.0 では、このキーは **ModuleToProcess** と呼ばれていました。

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

### -ScriptsToProcess

`.ps1`モジュールをインポートするときに、呼び出し元のセッション状態で実行されるスクリプト () ファイルを指定します。
ログイン スクリプトを使用する場合と同様に、これらのスクリプトを使用して、環境を準備できます。

モジュールのセッション状態で実行するスクリプトを指定するには、**NestedModules** キーを使用します。

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

モジュールによってエクスポートされる変数を制限するには、このパラメーターを使用します。 Variables **Stoexport** では、エクスポートされた変数の一覧から変数を削除できますが、変数を一覧に追加することはできません。

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

### -WhatIf

が実行された場合に何が起こるか `Update-ModuleManifest` を示します。 コマンドレットは実行されません。

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

### System.String

## 出力

### System.Object

## 注

> [!IMPORTANT]
> 2020 年 4 月時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン 1.0 および 1.1 がサポートされなくなります。 TLS 1.2 以降を使用していない場合、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、確実に TLS 1.2 を使用するようにします。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの[お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/)を参照してください。

## 関連リンク
