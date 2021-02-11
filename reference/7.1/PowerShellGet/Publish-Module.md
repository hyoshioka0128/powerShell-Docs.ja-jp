---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 10/03/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/publish-module?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Publish-Module
ms.openlocfilehash: 3ada5513343a5d6527cf1b091e1ee85e51f7f8de
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94892251"
---
# Publish-Module

## 概要
指定したモジュールをローカル コンピューターからオンライン ギャラリーに発行します。

## SYNTAX

### ModuleNameParameterSet (既定値)

```
Publish-Module -Name <String> [-RequiredVersion <String>] [-NuGetApiKey <String>]
 [-Repository <String>] [-Credential <PSCredential>] [-FormatVersion <Version>]
 [-ReleaseNotes <String[]>] [-Tags <String[]>] [-LicenseUri <Uri>] [-IconUri <Uri>]
 [-ProjectUri <Uri>] [-Exclude <String[]>] [-Force] [-AllowPrerelease] [-SkipAutomaticTags]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ModulePathParameterSet

```
Publish-Module -Path <String> [-NuGetApiKey <String>] [-Repository <String>]
 [-Credential <PSCredential>] [-FormatVersion <Version>] [-ReleaseNotes <String[]>]
 [-Tags <String[]>] [-LicenseUri <Uri>] [-IconUri <Uri>] [-ProjectUri <Uri>] [-Force]
 [-SkipAutomaticTags] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Publish-Module`コマンドレットは、ギャラリーにユーザーのプロファイルの一部として格納されている API キーを使用して、モジュールをオンラインの NuGet ベースのギャラリーに発行します。 モジュールの名前、またはモジュールを含むフォルダーへのパスのいずれかでモジュールを発行するように指定できます。

モジュールを名前で指定すると、は、を `Publish-Module` 実行して検出される最初のモジュールを発行し `Get-Module -ListAvailable <Name>` ます。 発行するモジュールの最小バージョンを指定すると、は、指定した最小バージョン以上のバージョンを `Publish-Module` 持つ最初のモジュールを発行します。

モジュールを発行するには、モジュールのギャラリー ページに表示されるメタデータが必要です。 必要なメタデータには、モジュールの名前、バージョン、説明、作成者が含まれています。 ほとんどのメタデータはモジュールマニフェストから取得されますが、一部のメタデータは、 `Publish-Module` **Tag**、 **ReleaseNote**、 **IconUri**、 **ProjectUri**、 **LicenseUri** などのパラメーターで指定する必要があります。これらのパラメーターは、NuGet ベースのギャラリーのフィールドと一致するためです。

## 例

### 例 1: モジュールを公開する

この例では、API キーを使用して、モジュール所有者のオンラインギャラリーアカウントを示すことによって、MyDscModule がオンラインギャラリーに発行されます。 MyDscModule が、名前、バージョン、説明、および作成者を指定する有効なマニフェストモジュールでない場合は、エラーが発生します。

```powershell
Publish-Module -Name "MyDscModule" -NuGetApiKey "11e4b435-6cb4-4bf7-8611-5162ed75eb73"
```

### 例 2: ギャラリーメタデータを使用してモジュールを公開する

この例では、API キーを使用して、モジュール所有者のギャラリーアカウントを示すことで、MyDscModule がオンラインギャラリーに発行されます。 提供された追加のメタデータは、ギャラリー内のモジュールの web ページに表示されます。 所有者は、モジュールに対して2つの検索タグを追加し、それを Active Directory に関連付けます。簡単なリリースノートが追加されます。 MyDscModule が、名前、バージョン、説明、および作成者を指定する有効なマニフェストモジュールでない場合は、エラーが発生します。

```powershell
Publish-Module -Name "MyDscModule" -NuGetApiKey "11e4b435-6cb4-4bf7-8611-5162ed75eb73" -LicenseUri "http://contoso.com/license" -Tag "Active Directory","DSC" -ReleaseNote "Updated the ActiveDirectory DSC Resources to support adding users."
```

## PARAMETERS

### -AllowPrerelease リリース

プレリリースとしてマークされているモジュールを公開できるようにします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

を実行する前に確認メッセージを表示し `Publish-Module` ます。

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

### -Credential

指定したパッケージプロバイダーまたはソースのモジュールを発行する権限を持つユーザーアカウントを指定します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -除外

パブリッシュされたモジュールから除外するファイルを定義します。

```yaml
Type: System.String[]
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

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

### -FormatVersion

**Validateset** 属性によって指定された有効な値のみを受け入れます。

詳細については、「 [Validateset 属性の宣言](/powershell/scripting/developer/cmdlet/validateset-attribute-declaration) と [ValidateSetAttribute](/dotnet/api/system.management.automation.validatesetattribute)」を参照してください。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:
Accepted values: 2.0

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IconUri

モジュールのアイコンの URL を指定します。 指定されたアイコンは、そのモジュールのギャラリー web ページに表示されます。

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

発行するモジュールのライセンス条項の URL を指定します。

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

### -Name

発行するモジュールの名前を指定します。 `Publish-Module` 指定されたモジュール名をで検索 `$Env:PSModulePath` します。

```yaml
Type: System.String
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NuGetApiKey

オンラインギャラリーにモジュールを発行するために使用する API キーを指定します。 API キーは、オンラインギャラリーのプロファイルの一部であり、ギャラリーのユーザーアカウントページで確認できます。 API キーは NuGet 固有の機能です。

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

### -Path

発行するモジュールへのパスを指定します。 このパラメーターは、モジュールを含むフォルダーへのパスを受け取ります。

```yaml
Type: System.String
Parameter Sets: ModulePathParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProjectUri

このプロジェクトに関する web ページの URL を指定します。

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

このバージョンのモジュールのユーザーが使用できるようにするリリースノートまたはコメントを含む文字列を指定します。

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

### -リポジトリ

を実行して登録されているリポジトリのフレンドリ名を指定し `Register-PSRepository` ます。 リポジトリには、有効な NuGet URI である **PublishLocation** が必要です。
**PublishLocation** は、を実行して設定でき `Set-PSRepository` ます。

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

### -RequiredVersion

発行する1つのモジュールの正確なバージョンを指定します。

```yaml
Type: System.String
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Skip自動タグ

コマンドおよびリソースをタグとして含めないようにします。 自動的にタグをモジュールに追加しないようにします。

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

### -タグ

発行するモジュールに1つ以上のタグを追加します。 タグの例としては、DesiredStateConfiguration、DSC、DSCResourceKit、PSModule などがあります。 複数のタグはコンマで区切ります。

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

### -WhatIf

が実行された場合に何が起こるかを示し `Publish-Module` ます。 このコマンドレットは実行されません。

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

### システム.... PSCredential

## 出力

### System.Object

## 注

`Publish-Module` powershell 3.0 以降の PowerShell で、windows 7 または Windows 2008 R2 以降のリリースの Windows で実行されます。

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

モジュールを発行するには、モジュールのギャラリー ページに表示されるメタデータが必要です。 必要なメタデータには、モジュールの名前、バージョン、説明、作成者が含まれています。 ほとんどのメタデータはモジュールマニフェストから取得されますが、一部のメタデータは、 `Publish-Module` **Tag**、 **ReleaseNote**、 **IconUri**、 **ProjectUri**、 **LicenseUri** などのパラメーターで指定できます。 詳細については、「 [POWERSHELL ギャラリー UI に影響するパッケージマニフェスト値](/powershell/scripting/gallery/concepts/package-manifest-affecting-ui)」を参照してください。

## 関連リンク

[Find-Module](Find-Module.md)

[Install-Module](Install-Module.md)

[Register-PSRepository](Register-PSRepository.md)

[Set-PSRepository](Set-PSRepository.md)

[Uninstall-Module](Uninstall-Module.md)

[Update-Module](Update-Module.md)
