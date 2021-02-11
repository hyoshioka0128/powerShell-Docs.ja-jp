---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 06/04/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/find-dscresource?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-DscResource
ms.openlocfilehash: 39896c4f48d6cd8809d4a680e776d36deb65b0d8
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94889863"
---
# Find-DscResource

## 概要
Desired State Configuration (DSC) リソースを検索します。

## SYNTAX

### すべて

```
Find-DscResource [[-Name] <String[]>] [-ModuleName <String>] [-MinimumVersion <String>]
 [-MaximumVersion <String>] [-RequiredVersion <String>] [-AllVersions] [-AllowPrerelease]
 [-Tag <String[]>] [-Filter <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-Repository <String[]>] [<CommonParameters>]
```

## Description

`Find-DscResource`コマンドレットは、登録されているリポジトリを検索して、モジュールに含まれる DSC リソースを検索します。 既定では、 `Find-DscResource` すべての登録済みリポジトリが検索されます。

によって検出されたモジュールごとに `Find-DscResource` 、 **Psgetdscresourceinfo** オブジェクトが返されます。
**Psgetdscresourceinfo** オブジェクトは、パイプラインを使用してコマンドレットに送信でき `Install-Module` ます。
`Install-Module` モジュールをインストールします。

## 例

### 例 1: すべての DSC リソースを検索する

`Find-DscResource` 登録されているリポジトリから DSC リソースを返します。 特定のリポジトリを検索するには、 **repository** パラメーターを使用します。

```powershell
Find-DscResource
```

```Output
Name                           Version    ModuleName                     Repository
----                           -------    ----------                     ----------
Carbon_Privilege               2.8.1      Carbon                         PSGallery
Carbon_ScheduledTask           2.8.1      Carbon                         PSGallery
Carbon_Service                 2.8.1      Carbon                         PSGallery
PackageManagement              1.4        PackageManagement              PSGallery
PackageManagementSource        1.4        PackageManagement              PSGallery
PSModule                       2.1.4      PowerShellGet                  PSGallery
PSRepository                   2.1.4      PowerShellGet                  PSGallery
xArchive                       8.7.0.0    xPSDesiredStateConfiguration   PSGallery
xDSCWebService                 8.7.0.0    xPSDesiredStateConfiguration   PSGallery
xEnvironment                   8.7.0.0    xPSDesiredStateConfiguration   PSGallery
```

### 例 2: 名前を指定して DSC リソースを検索する

`Find-DscResource` 名前を指定して DSC リソースを検索します。 リソース名の配列を区切るには、コンマを使用します。

```powershell
Find-DscResource -Name xWebsite, xWebApplication, xWebSiteDefaults
```

```Output
Name               Version    ModuleName            Repository
----               -------    ----------            ----------
xWebApplication    2.6.0.0    xWebAdministration    PSGallery
xWebsite           2.6.0.0    xWebAdministration    PSGallery
xWebSiteDefaults   2.6.0.0    xWebAdministration    PSGallery
```

`Find-DscResource`**Name** パラメーターを使用して、指定された DSC リソースの配列を検索します。

### 例 3: DSC リソースを検索してインストールする

`Find-DscResource` DSC リソースを検索し、インストールするパイプラインの下にオブジェクトを送信します。
インストール後、を使用し `Get-InstalledModule` て結果を表示します。

同じモジュールの複数のリソースをパイプラインからに送信でき `Install-Module` ます。
`Install-Module` モジュールのインストールを1回だけ試行します。

```powershell
Find-DscResource -Name xWebsite | Install-Module
```

`Find-DscResource`**Name** パラメーターを使用して、 **xwebsite** という名前のリソースを検索します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Install-Module` ます。 `Install-Module` リソースの **Xwebadministration** モジュールをインストールします。

### 例 4: モジュール内のすべての DSC リソースを検索する

`Find-DscResource` 指定されたモジュールに含まれるすべての DSC リソースを検索します。 既定では、現在のバージョンが表示されます。 他のバージョンを表示するには、 **AllVersions** または **requiredversions** パラメーターを使用します。

```powershell
Find-DscResource -ModuleName xWebAdministration
```

```Output
Name                                Version    ModuleName              Repository
----                                -------    ----------              ----------
WebApplicationHandler               2.6.0.0    xWebAdministration      PSGallery
xIisFeatureDelegation               2.6.0.0    xWebAdministration      PSGallery
xIisHandler                         2.6.0.0    xWebAdministration      PSGallery
xIisLogging                         2.6.0.0    xWebAdministration      PSGallery
```

`Find-DscResource`**ModuleName** パラメーターを使用して **xwebadministration** を指定し、モジュールに含まれる DSC リソースを検索します。 各リソースの現在のバージョンが表示されます。

### 例 5: タグおよび必要なバージョンで DSC リソースを検索する

DSC リソースは、parameters **タグ** および **RequiredVersion** を使用して見つけることができます。 **タグ** は、リポジトリ内の指定されたタグを含むすべてのリソースの現在のバージョンを表示します。
**RequiredVersion** には **ModuleName** パラメーターが必要であり、 **Name** パラメーターは省略可能です。 **Name** パラメーターと **ModuleName** パラメーターは、出力を制限します。 DSC リソースの使用可能なバージョンを表示するには、 **AllVersions** パラメーターを使用します。

```powershell
Find-DscResource -ModuleName xWebAdministration -Tag DSC -RequiredVersion 1.20
```

```output
Name                    Version    ModuleName             Repository
----                    -------    ----------             ----------
xIisFeatureDelegation   1.20.0.0   xWebAdministration     PSGallery
xIisHandler             1.20.0.0   xWebAdministration     PSGallery
xIisLogging             1.20.0.0   xWebAdministration     PSGallery
xIisMimeTypeMapping     1.20.0.0   xWebAdministration     PSGallery
```

### 例 6: フィルターを使用してリソースを検索する

`Find-DscResource` すべてのリソースを検索し、 **Filter** パラメーターを使用して結果を **ドメイン** 別に指定します。 **Filter** パラメーターを指定すると、オブジェクトの説明またはモジュール名のフィルター値が検索されます。 `Select-Object`オブジェクトのプロパティを表示するには、コマンドレットを使用します。

```powershell
Find-DscResource -Filter Domain
```

```output
Name                    Version    ModuleName                 Repository
----                    -------    ----------                 ---------
xComputer               4.1.0.0    xComputerManagement        PSGallery
Computer                6.4.0.0    ComputerManagementDsc      PSGallery
xDSCDomainjoin          1.1        xDSCDomainjoin             PSGallery
xDisk                   1.0        xDisk                      PSGallery
xDSCFirewall            1.6.21     xDSCFirewall               PSGallery
dmAwsTagInstance        1.0.1      domainAwsDSCResources      PSGallery
```

## PARAMETERS

### -AllowPrerelease リリース

結果にプレリリースとしてマークされたリソースが含まれます。

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

### -AllVersions

**AllVersions** パラメーターには、DSC リソースの使用可能なバージョンがそれぞれ表示されます。 **AllVersions** パラメーターは、 **MinimumVersion**、 **MaximumVersion**、または **RequiredVersion** パラメーターと共に使用することはできません。

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

### -Filter

**PackageManagement** プロバイダーの検索構文に基づいてリソースを検索します。 たとえば、 **ModuleName** プロパティと **Description** プロパティ内で検索する単語を指定します。

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

### -MaximumVersion

結果に含めるリソースの最大バージョンを指定します。 **MaximumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

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

### -MinimumVersion

結果に含めるリソースの最小バージョンを指定します。 **MinimumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

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

### -ModuleName

DSC リソースを含むモジュールを指定します。

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

### -Name

リソースの名前を指定します。 既定値はすべてのリソースです。 リソース名の配列を区切るには、コンマを使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -プロキシ

インターネットリソースに直接接続するのではなく、要求のプロキシサーバーを指定します。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProxyCredential

**プロキシ** パラメーターで指定されたプロキシサーバーを使用するためのアクセス許可を持つユーザーアカウントを指定します。

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

### -リポジトリ

リソースを検索するリポジトリを指定します。 リポジトリ名の配列を区切るには、コンマを使用します。

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

### -RequiredVersion

結果に含めるモジュールの正確なバージョン番号を指定します。 **RequiredVersion** パラメーターと **MinimumVersion** パラメーターを同じコマンドで使用することはできません。

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

### -Tag

リポジトリ内のモジュールを分類するタグを指定します。 タグの配列を区切るには、コンマを使用します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

## 出力

### PSGetDscResourceInfo

`Find-DscResource`**Psgetdscresourceinfo** オブジェクトを返します。

## 注

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## 関連リンク

[Get-InstalledModule](Get-InstalledModule.md)

[Install-Module](Install-Module.md)

[Register-PSRepository](Register-PSRepository.md)

[Select-Object](../Microsoft.PowerShell.Utility/Select-Object.md)

[アンインストール-モジュール](Uninstall-Module.md)
