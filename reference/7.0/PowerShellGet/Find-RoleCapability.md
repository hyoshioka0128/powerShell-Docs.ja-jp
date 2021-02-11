---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 06/05/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/find-rolecapability?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-RoleCapability
ms.openlocfilehash: 1cee4ac54bf83d387f61a4842104a80512a8b223
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94891526"
---
# Find-RoleCapability

## 概要
モジュール内のロール機能を検索します。

## SYNTAX

### すべて

```
Find-RoleCapability [[-Name] <String[]>] [-ModuleName <String>] [-MinimumVersion <String>] 
[-MaximumVersion <String>] [-RequiredVersion <String>] [-AllVersions] [-AllowPrerelease] 
[-Tag <String[]>] [-Filter <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>] 
[-Repository <String[]>] [<CommonParameters>]
```

## Description

`Find-RoleCapability`コマンドレットは、登録されているリポジトリを検索して、PowerShell ロール機能とモジュールを検索します。

によって検出された各ロール機能に対して `Find-RoleCapability` 、 **Psgetrolecap信頼性情報** オブジェクトが返されます。 **Psgetrolecap信頼性情報** オブジェクトは、パイプラインを使用して、 `Install-Module` またはコマンドレットに送信でき `Save-Module` ます。

PowerShell ロール機能では、十分な管理 (JEA) エンドポイントでユーザーが使用できるコマンドとアプリケーションを定義します。 ロール機能は、拡張子の付いたファイルによって定義され `.psrc` ます。

## 例

### 例 1: ロール機能を検索する

`Find-RoleCapability` 登録されている各リポジトリのロール機能を検索します。 特定のリポジトリを検索するには、 **repository** パラメーターを使用します。

```powershell
Find-RoleCapability
```

```output
Name             Version    ModuleName     Repository
----             -------    ----------     ----------
General-Lev1     1.0        JeaExamples    PSGallery
General-Lev2     1.0        JeaExamples    PSGallery
IIS-Lev1         1.0        JeaExamples    PSGallery
IIS-Lev2         1.0        JeaExamples    PSGallery
```

### 例 2: 名前を指定してロール機能を検索する

`Find-RoleCapability` ロール機能を名前で検索します。 名前の配列を区切るには、コンマを使用します。

```powershell
Find-RoleCapability -Name General-Lev1, IIS-Lev2
```

```output
Name             Version    ModuleName     Repository
----             -------    ----------     ----------
General-Lev1     1.0        JeaExamples    PSGallery
IIS-Lev2         1.0        JeaExamples    PSGallery
```

### 例 3: ロール機能のモジュールを検索して保存する

コマンドレットでは、 `Find-RoleCapability` ロール機能を検索し、そのオブジェクトをパイプラインの下に送信します。
`Save-Module` ロール機能のモジュールをファイルシステムに保存します。 `Get-ChildItem` モジュールのディレクトリの内容を表示します。

```
PS> Find-RoleCapability -Name General-Lev1 | Save-Module -Path C:\Test\Modules

PS> Get-ChildItem -Path C:\Test\Modules\JeaExamples\1.0\

    Directory: C:\Test\Modules\JeaExamples\1.0

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----          6/4/2019    16:37                RoleCapabilities
-a----          2/5/2019    18:46           1702 CreateRegisterPSSC.ps1
-a----          2/5/2019    18:46           7656 JeaExamples.psd1
-a----         10/1/2018    08:16            595 JeaExamples.psm1
```

`Find-RoleCapability`**Name** パラメーターを使用して、 **Lev1** ロール機能を指定します。
オブジェクトは、パイプラインで送信されます。 `Save-Module` ファイルシステムの場所に **Path** パラメーターを使用してモジュールを保存します。 モジュールが保存された後、 `Get-ChildItem` モジュールの **パス** を指定し、 **JeaExamples** モジュールのディレクトリの内容を表示します。

### 例 4: ロール機能のモジュールを検索してインストールする

`Find-RoleCapability` モジュールを検索し、そのオブジェクトをパイプラインの下に送信します。 `Install-Module` モジュールをインストールします。 インストール後、を使用し `Get-InstalledModule` て結果を確認します。

```
PS> Find-RoleCapability -Name General-Lev1 | Install-Module -Verbose

VERBOSE: Downloading 'https://www.powershellgallery.com/api/v2/package/JeaExamples/1.0.0'.
VERBOSE: Completed downloading 'https://www.powershellgallery.com/api/v2/package/JeaExamples/1.0.0'.
VERBOSE: Completed downloading 'JeaExamples'.
VERBOSE: InstallPackageLocal' - name='JeaExamples', version='1.0',
VERBOSE: Validating the 'JeaExamples' module contents
VERBOSE: Test-ModuleManifest successfully validated the module manifest file
VERBOSE: Module 'JeaExamples' was installed successfully to path

PS> Get-InstalledModule

Version    Name            Repository     Description
-------    ----            ----------     -----------
1.0        JeaExamples     PSGallery      Some example Jea roles for general server maintenance...
```

`Find-RoleCapability`**Name** パラメーターを使用して、 **Lev1** ロール機能を指定します。
オブジェクトは、パイプラインで送信されます。 `Install-Module` では、インストール中に **Verbose** パラメーターを使用してステータスメッセージが表示されます。 インストールが完了すると、JeaExamples モジュールがインストールされたことが出力によっ `Get-InstalledModule` て確認されます。 

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

このコマンドレットがモジュールのすべてのバージョンを取得することを示します。 **AllVersions** パラメーターには、モジュールの使用可能なバージョンがそれぞれ表示されます。

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

ロール機能を検索するモジュールの名前を指定します。 既定値はすべてのモジュールです。

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

ロール機能の名前を指定します。 既定値は、すべてのロール機能です。 リソース名の配列を区切るには、コンマを使用します。

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

ロール機能を検索するリポジトリを指定します。 リポジトリ名の配列を区切るには、コンマを使用します。

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

### System.Uri

### システム.... PSCredential

## 出力

### PSGetRoleCapabilityInfo

`Find-RoleCapability`コマンドレットは、 **Psgetrolecap信頼性情報** オブジェクトを返します。

## 注

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## 関連リンク

[Get-ChildItem](../Microsoft.PowerShell.Management/Get-ChildItem.md)

[Get-InstalledModule](Get-InstalledModule.md)

[Install-Module](Install-Module.md)

[New-PSRoleCapabilityFile](../Microsoft.PowerShell.Core/New-PSRoleCapabilityFile.md)

[Save-Module](Save-Module.md)
