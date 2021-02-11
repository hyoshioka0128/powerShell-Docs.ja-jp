---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 06/03/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/find-command?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-Command
ms.openlocfilehash: eb305801bb6f8c84ffdf0ff34936ec3156ba5b0e
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94891256"
---
# Find-Command

## 概要
モジュール内の PowerShell コマンドを検索します。

## SYNTAX

### すべて

```
Find-Command [[-Name] <String[]>] [-ModuleName <String>] [-MinimumVersion <String>]
 [-MaximumVersion <String>] [-RequiredVersion <String>] [-AllVersions] [-AllowPrerelease]
 [-Tag <String[]>] [-Filter <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-Repository <String[]>] [<CommonParameters>]
```

## Description

`Find-Command`コマンドレットでは、コマンドレット、エイリアス、関数、ワークフローなどの PowerShell コマンドを検索します。 `Find-Command` 登録されているリポジトリ内のモジュールを検索します。

によって検出された各コマンドに対して `Find-Command` 、 **Psgetcommandinfo** オブジェクトが返されます。 **Psgetcommandinfo** オブジェクトは、パイプラインを使用してコマンドレットに送信でき `Install-Module` ます。
`Install-Module` コマンドを含むモジュールをインストールします。

## 例

### 例 1: 指定されたリポジトリ内のすべてのコマンドを検索する

コマンドレットでは、 `Find-Command` 登録されているリポジトリでモジュールを検索します。

```powershell
Find-Command -Repository PSGallery | Select-Object -First 10
```

```output
Name                                Version    ModuleName          Repository
----                                -------    ----------          ----------
Disable-AzureRmDataCollection       5.8.3      AzureRM.profile     PSGallery
Disable-AzureRmContextAutosave      5.8.3      AzureRM.profile     PSGallery
Enable-AzureRmDataCollection        5.8.3      AzureRM.profile     PSGallery
Enable-AzureRmContextAutosave       5.8.3      AzureRM.profile     PSGallery
Remove-AzureRmEnvironment           5.8.3      AzureRM.profile     PSGallery
Get-AzureRmEnvironment              5.8.3      AzureRM.profile     PSGallery
Set-AzureRmEnvironment              5.8.3      AzureRM.profile     PSGallery
Add-AzureRmEnvironment              5.8.3      AzureRM.profile     PSGallery
Get-AzureRmSubscription             5.8.3      AzureRM.profile     PSGallery
Connect-AzureRmAccount              5.8.3      AzureRM.profile     PSGallery
```

`Find-Command`**repository** パラメーターを使用して、登録されているリポジトリの名前を指定します。 オブジェクトは、パイプラインを介して送信されます。 `Select-Object` オブジェクトを受け取り、 **1 番目** のパラメーターを使用して最初の10件の結果を表示します。

### 例 2: 名前を指定してコマンドを検索する

`Find-Command` では、コマンドの名前を使用して、リポジトリ内のモジュールを見つけることができます。 コマンド名が複数の **modulenames** 存在する可能性があります。

```powershell
Find-Command -Repository PSGallery -Name Get-TargetResource
```

```Output
Name                  Version    ModuleName                      Repository
----                  -------    ----------                      ----------
Get-TargetResource    3.1.0.0    xPowerShellExecutionPolicy      PSGallery
Get-TargetResource    1.0.0      xInternetExplorerHomePage       PSGallery
Get-TargetResource    1.2.0.0    SystemLocaleDsc                 PSGallery
```

`Find-Command`**Repository** パラメーターを使用して **PSGallery** を検索します。 **Name** パラメーターは、コマンド **set-targetresource** を指定します。

### 例 3: 名前を指定してコマンドを検索し、モジュールをインストールする

`Find-Command` は、コマンドとモジュールを見つけて、オブジェクトをに送信でき `Install-Module` ます。 複数のモジュールにコマンドが含まれている場合は、コマンド `Find-Command` レット **Module-Name** パラメーターを使用します。
それ以外の場合は、インストールしたくないモジュールがインストールされている可能性があります。

```powershell
PS> Find-Command -Name Get-TargetResource -Repository PSGallery -ModuleName SystemLocaleDsc |
    Install-Module

PS> Get-InstalledModule

Version   Name               Repository   Description
-------   ----               ----------   -----------
1.2.0.0   SystemLocaleDsc    PSGallery    This DSC Resource allows configuration of the Windows...
```

`Find-Command`**Name** パラメーターを使用して、コマンド **set-targetresource** を指定します。 **Repository** パラメーターは、 **PSGallery** を検索します。 **ModuleName** パラメーターは、インストールするモジュール ( **Systemlocaledsc**) を指定します。 オブジェクトは、パイプラインの下に送信され、 `Install-Module` モジュールがインストールされます。 インストールが完了したら、を使用し `Get-InstalledModule` て結果を表示できます。

### 例 4: コマンドを検索し、そのモジュールを保存する

```
PS> Find-Command -Name Invoke-ScriptAnalyzer -Repository PSGallery | Save-Module -Path C:\Test\Modules -Verbose

VERBOSE: Downloading 'https://www.powershellgallery.com/api/v2/package/PSScriptAnalyzer/1.18.0'.
VERBOSE: Completed downloading 'https://www.powershellgallery.com/api/v2/package/PSScriptAnalyzer/1.18.0'.
VERBOSE: Completed downloading 'PSScriptAnalyzer'.
VERBOSE: Module 'PSScriptAnalyzer' was saved successfully to path 'C:\Test\Modules\PSScriptAnalyzer\1.18.0'.
```

`Find-Command`では、 **Name** と **repository** パラメーターを使用して、 **PSGallery** リポジトリでコマンド **Invoke-scriptanalyzer** を検索します。 オブジェクトは、パイプライン内でに送信され `Save-Module` ます。 **Path** パラメーターは、モジュールを保存する場所を決定します。 **Verbose** は省略可能なパラメーターですが、PowerShell コンソールに状態出力が表示されます。 詳細出力は、トラブルシューティングに役立ちます。

## PARAMETERS

### -AllowPrerelease リリース

結果にプレリリースとしてマークされているモジュールが含まれます。

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

このコマンドレットがモジュールのすべてのバージョンを取得することを示します。

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

**PackageManagement** プロバイダーの検索構文に基づいてモジュールを検索します。 たとえば、 **ModuleName** プロパティと **Description** プロパティ内で検索する単語を指定します。

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

結果に含めるモジュールの最大バージョンを指定します。 **MaximumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

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

結果に含めるモジュールの最小バージョンを指定します。 **MinimumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

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

コマンドを検索するモジュールの名前を指定します。 既定値はすべてのモジュールです。

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

リポジトリ内で検索するコマンド名を指定します。 コマンド名の配列を区切るには、コンマを使用します。

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

**Proxy** パラメーターに指定したプロキシ サーバーを使用するアクセス許可を持つユーザー アカウントを指定します。

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

コマンドを検索するリポジトリを指定します。 リポジトリ名の配列を区切るには、コンマを使用します。 既定値は [すべてのリポジトリです。

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

結果に含めるモジュールのバージョンを指定します。

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

### PSGetCommandInfo

`Find-Command`**Psgetcommandinfo** オブジェクトを出力します。

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

[Save-Module](Save-Module.md)

[Select-Object](../Microsoft.PowerShell.Utility/Select-Object.md)

[アンインストール-モジュール](Uninstall-Module.md)
