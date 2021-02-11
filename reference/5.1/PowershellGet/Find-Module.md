---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 03/11/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/find-module?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-Module
ms.openlocfilehash: b30e233eb9c4f4f9191ac6470f2821536dda6dc3
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94889829"
---
# Find-Module

## 概要
指定された条件に一致するリポジトリ内のモジュールを検索します。

## SYNTAX

### すべて

```
Find-Module [[-Name] <string[]>] [-MinimumVersion <string>] [-MaximumVersion <string>]
 [-RequiredVersion <string>] [-AllVersions] [-IncludeDependencies] [-Filter <string>]
 [-Tag <string[]>] [-Includes <string[]>] [-DscResource <string[]>] [-RoleCapability <string[]>]
 [-Command <string[]>] [-Proxy <uri>] [-ProxyCredential <pscredential>] [-Repository <string[]>]
 [-Credential <pscredential>] [-AllowPrerelease] [<CommonParameters>]
```

## Description

`Find-Module`コマンドレットは、指定された条件に一致するリポジトリ内のモジュールを検索します。
`Find-Module` 検出された各モジュールの **できる psrepositoryiteminfo** オブジェクトを返します。 オブジェクトは、などのコマンドレットにパイプラインを介して送信でき `Install-Module` ます。

最初に `Find-Module` リポジトリを使用しようとすると、更新プログラムのインストールを求めるメッセージが表示される場合があります。
リポジトリソースがコマンドレットに登録されていない場合 `Register-PSRepository` は、エラーが返されます。

`Find-Module` バージョンを制限するパラメーターが使用されていない場合は、最新バージョンのモジュールを返します。 リポジトリのモジュールバージョンの一覧を取得するには、パラメーター **AllVersions** を使用します。

**MinimumVersion** パラメーターが指定されている場合、は、 `Find-Module` 最小値以上のモジュールのバージョンを返します。 リポジトリに使用可能な新しいバージョンがある場合は、新しいバージョンが返されます。

**MaximumVersion** パラメーターが指定されている場合、は、 `Find-Module` 指定されたバージョンを超えるモジュールの最新バージョンを返します。

**RequiredVersion** パラメーターが指定されている場合は、 `Find-Module` 指定したバージョンと完全に一致するモジュールバージョンのみが返されます。 `Find-Module` は、ソース間で名前の競合が発生する可能性があるため、使用可能なすべてのモジュールを検索します。

次の例では、唯一の登録済みリポジトリとして [PowerShell ギャラリー](https://www.powershellgallery.com/) を使用します。 `Get-PSRepository` 登録されているリポジトリを表示します。 複数の登録済みリポジトリがある場合は、パラメーターを使用して、 `-Repository` リポジトリの名前を指定します。

## 例

### 例 1: 名前を指定してモジュールを検索する

この例では、既定のリポジトリでモジュールを検索します。

```powershell
Find-Module -Name PowerShellGet
```

```Output
Version   Name              Repository           Description
-------   ----              ----------           -----------
2.1.0     PowerShellGet     PSGallery            PowerShell module with commands for discovering...
```

コマンドレットでは、 `Find-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定します。

### 例 2: 類似した名前のモジュールを検索する

この例では、アスタリスク ( `*` ) ワイルドカードを使用して、類似した名前のモジュールを検索します。

```powershell
Find-Module -Name PowerShell*
```

```Output
Version   Name                            Repository    Description
-------   ----                            ----------    -----------
0.4.0     powershell-yaml                 PSGallery     Powershell module for serializing and...
2.1.0     PowerShellGet                   PSGallery     PowerShell module with commands for...
1.9       Powershell.Helper.Extension     PSGallery     # Powershell.Helper.Extension...
3.1       PowerShellHumanizer             PSGallery     PowerShell Humanizer wraps Humanizer...
4.0       PowerShellISEModule             PSGallery     a module that adds capability to the ISE
```

コマンドレットでは、 `Find-Module` **Name** パラメーターをアスタリスク () ワイルドカードと共に使用して、 `*` **PowerShell** を含むすべてのモジュールを検索します。

### 例 3: 最小バージョンでモジュールを検索する

この例では、モジュールの最小バージョンを検索します。 リポジトリに新しいバージョンのモジュールが含まれている場合は、新しいバージョンが返されます。

```powershell
Find-Module -Name PowerShellGet -MinimumVersion 1.6.5
```

```Output
Version   Name             Repository     Description
-------   ----             ----------     -----------
2.1.0     PowerShellGet    PSGallery      PowerShell module with commands for discovering...
```

コマンドレットでは、 `Find-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定します。 **MinimumVersion** はバージョン **1.6.5** を指定します。 `Find-Module` は、最小バージョンを超えており、最新バージョンであるため、PowerShellGet バージョン **2.1.0** を返します。

### 例 4: 特定のバージョンでモジュールを検索する

この例では、モジュールの特定のバージョンを表すオブジェクトが返されます。 指定したバージョンが見つからない場合は、エラーが返されます。

```powershell
Find-Module -Name PowerShellGet -RequiredVersion 1.6.5
```

```Output
Version   Name             Repository     Description
-------   ----             ----------     -----------
1.6.5     PowerShellGet    PSGallery      PowerShell module with commands for discovering...
```

コマンドレットでは、 `Find-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定します。 **RequiredVersion** パラメーターにはバージョン **1.6.5** が指定されています。

### 例 5: 特定のリポジトリでモジュールを検索する

この例では、 **repository** パラメーターを使用して、特定のリポジトリ内のモジュールを検索します。

```powershell
Find-Module -Name PowerShellGet -Repository PSGallery
```

```Output
Version   Name             Repository     Description
-------   ----             ----------     -----------
2.1.0     PowerShellGet    PSGallery      PowerShell module with commands for discovering...
```

コマンドレットでは、 `Find-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定します。 **Repository** パラメーターは、 **PSGallery** リポジトリを検索するように指定します。

### 例 6: 複数のリポジトリでモジュールを検索する

この例では、を使用して `Register-PSRepository` リポジトリを指定します。 `Find-Module` は、リポジトリを使用してモジュールを検索します。

```powershell
Register-PSRepository -Name MySource -SourceLocation https://www.myget.org/F/powershellgetdemo/
Find-Module -Name Contoso* -Repository PSGallery, MySource
```

```Output
Repository    Version   Name             Description
----------    -------   ----             -----------
PSGallery     2.0.0.0   ContosoServer    Cmdlets and DSC resources for managing Contoso Server...
MySource      1.2.0.0   ContosoClient    Cmdlets and DSC resources for managing Contoso Client...
```

コマンドレットにより、 `Register-PSRepository` 新しいリポジトリが登録されます。 **Name** パラメーターを指定すると、 **MySource** という名前が割り当てられます。 **SourceLocation** パラメーターでは、リポジトリのアドレスを指定します。

コマンドレットでは、 `Find-Module` **Name** パラメーターをアスタリスク ( `*` ) ワイルドカードと共に使用して **Contoso** モジュールを指定します。 **Repository** パラメーターでは、 **PSGallery** と **MySource** の2つのリポジトリを検索するように指定します。

### 例 7: DSC リソースが含まれているモジュールを検索する

このコマンドは、DSC リソースを含むモジュールを返します。 **インクルード** パラメーターには、リポジトリの検索に使用される4つの定義済み機能があります。 [タブの完了] を使用すると、 **インクルード** パラメーターでサポートされている4つの機能を表示できます。

```powershell
Find-Module -Repository PSGallery -Includes DscResource
```

```Output
Version     Name                            Repository    Description
-------     ----                            ----------    -----------
2.7.0       Carbon                          PSGallery     Carbon is a PowerShell module...
8.5.0.0     xPSDesiredStateConfiguration    PSGallery     The xPSDesiredStateConfiguration module...
1.3.1       PackageManagement               PSGallery     PackageManagement (a.k.a. OneGet) is...
2.7.0.0     xWindowsUpdate                  PSGallery     Module with DSC Resources...
3.2.0.0     xCertificate                    PSGallery     This module includes DSC resources...
3.1.0.0     xPowerShellExecutionPolicy      PSGallery     This DSC resource can change the user...
```

このコマンドレットでは、repository パラメーターを使用して `Find-Module` リポジトリ **PSGallery** を検索します。 
**インクルード** パラメーターは **DscResource** を指定します。これは、パラメーターがリポジトリで検索できる機能です。

### 例 8: フィルターを使用してモジュールを検索する

この例では、モジュールを検索するために、フィルターを使用してリポジトリを検索します。

NuGet ベースのリポジトリの場合、 **Filter** パラメーターは引数の名前、説明、およびタグを検索します。

```powershell
Find-Module -Filter AppDomain
```

```Output
Version    Name              Repository           Description
-------    ----              ----------           -----------
1.0.0.0  AppDomainConfig     PSGallery            Manipulate AppDomain configuration...
1.1.0    ClassExplorer       PSGallery            Quickly search the AppDomain for classes...
```

コマンドレットでは、 `Find-Module` **Filter** パラメーターを使用して、リポジトリで **AppDomain** を検索します。

## PARAMETERS

### -AllowPrerelease リリース

プレリリースとしてマークされた結果モジュールにを含めます。

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

すべてのバージョンのモジュールを結果に含めるように指定します。 **AllVersions** パラメーターは、 **MinimumVersion**、 **MaximumVersion**、または **RequiredVersion** パラメーターと共に使用することはできません。

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

### -Command

モジュール内で検索するコマンドの配列を指定します。 コマンドは、関数またはワークフローにすることができます。

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

### -Credential

指定したパッケージプロバイダーまたはソースのモジュールをインストールする権限を持つユーザーアカウントを指定します。

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

### -DscResource

DSC リソースを含むモジュールの名前または名前の一部を指定します。 PowerShell の規則ごとに、複数の引数を指定すると、 **または** 検索を実行します。

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

### -Filter

**PackageManagement** プロバイダー固有の検索構文に基づくフィルターを指定します。 NuGet モジュールの場合、このパラメーターは、 [PowerShell ギャラリー](https://www.powershellgallery.com/) web サイトの検索バーを使用した検索に相当します。

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

### -IncludeDependencies

この操作に、 **Name** パラメーターで指定されたモジュールに依存するすべてのモジュールが含まれていることを示します。

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

### -インクルード

特定の種類の PowerShell 機能を含むモジュールだけを返します。 たとえば、 **DSCResource** を含むモジュールだけを検索する場合があります。 このパラメーターに指定できる値は次のとおりです。

- コマンドレット
- DscResource
- 機能
- RoleCapability

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:
Accepted values: DscResource, Cmdlet, Function, RoleCapability

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumVersion

検索結果に含めるモジュールの最大バージョンまたは最新バージョンを指定します。
**MaximumVersion** と **RequiredVersion** を同じコマンドで使用することはできません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MinimumVersion

結果に含めるモジュールの最小バージョンを指定します。 **MinimumVersion** および **RequiredVersion** を同じコマンドで使用することはできません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

リポジトリ内で検索するモジュールの名前を指定します。 モジュール名のコンマ区切りのリストが受け入れられます。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
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

**Repository** パラメーターを使用して、モジュールを検索するリポジトリを指定します。 複数のリポジトリが登録されている場合に使用します。 リポジトリのコンマ区切りリストを受け入れます。 リポジトリを登録するには、を使用 `Register-PSRepository` します。 登録されているリポジトリを表示するには、を使用 `Get-PSRepository` します。

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

結果に含めるモジュールの正確なバージョン番号を指定します。 **RequiredVersion** は、 **MinimumVersion** または **MaximumVersion** と同じコマンドでは使用できません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Find-rolecapability

ロール機能の配列を指定します。

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

### -Tag

タグの配列を指定します。 タグの例としては、 **DesiredStateConfiguration**、 **DSC**、 **Dscresourcekit**、 **psmodule** などがあります。

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

### できる psrepositoryiteminfo

`Find-Module` などのコマンドレットにパイプラインから送信できる **できる psrepositoryiteminfo** オブジェクトを作成し `Install-Module` ます。

## 注

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## 関連リンク

[Get-PSRepository](Get-PSRepository.md)

[Install-Module](Install-Module.md)

[Publish-Module](Publish-Module.md)

[Save-Module](Save-Module.md)

[Uninstall-Module](Uninstall-Module.md)

[Update-Module](Update-Module.md)

[Register-PSRepository](Register-PSRepository.md)
