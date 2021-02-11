---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/powershellget/register-psrepository?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-PSRepository
ms.openlocfilehash: 07f70f4fad6057ad689befaafd1d41f819a4538c
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94892765"
---
# Register-PSRepository

## 概要
PowerShell リポジトリを登録します。

## SYNTAX

### NameParameterSet (既定値)

```
Register-PSRepository [-Name] <String> [-SourceLocation] <Uri> [-PublishLocation <Uri>]
 [-ScriptSourceLocation <Uri>] [-ScriptPublishLocation <Uri>] [-Credential <PSCredential>]
 [-InstallationPolicy <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-PackageManagementProvider <String>] [<CommonParameters>]
```

### PSGalleryParameterSet

```
Register-PSRepository [-Default] [-InstallationPolicy <String>] [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [<CommonParameters>]
```

## Description

`Register-PSRepository`コマンドレットは、PowerShell モジュールの既定のリポジトリを登録します。 リポジトリが登録されると、 `Find-Module` 、 `Install-Module` 、およびコマンドレットから参照でき `Publish-Module` ます。 登録されたリポジトリは、およびの既定のリポジトリになり `Find-Module` `Install-Module` ます。

登録されているリポジトリはユーザー固有です。 システム全体のコンテキストには登録されません。

登録されている各リポジトリは、 **PackageManagementProvider** パラメーターで指定される oneget パッケージプロバイダーに関連付けられています。 各 OneGet プロバイダーは、特定の種類のリポジトリと対話するように設計されています。 たとえば、nuget プロバイダーは NuGet ベースのリポジトリと対話するように設計されています。 登録時に OneGet プロバイダーが指定されていない場合、PowerShellGet は、指定されたソースの場所を処理できる OneGet プロバイダーの検索を試みます。

## 例

### 例 1: リポジトリを登録する

```powershell
$parameters = @{
  Name = "myNuGetSource"
  SourceLocation = "https://www.myget.org/F/powershellgetdemo/api/v2"
  PublishLocation = "https://www.myget.org/F/powershellgetdemo/api/v2/Packages"
  InstallationPolicy = 'Trusted'
}
Register-PSRepository @parameters
Get-PSRepository
```

```Output
Name                SourceLocation          OneGetProvider       InstallationPolicy
----                --------------          --------------       ------------------
PSGallery           http://go.micro...      NuGet                Untrusted
myNuGetSource       https://myget.c...      NuGet                Trusted
```

最初のコマンドは、 `https://www.myget.org/F/powershellgetdemo/` 現在のユーザーのリポジトリとして登録します。 MyNuGetSource を登録すると、モジュールの検索、インストール、および発行時に明示的に参照できます。 **PackageManagementProvider** パラメーターが指定されていないため、リポジトリが oneget パッケージプロバイダーに明示的に関連付けられていないため、PowerShellGet は使用可能なパッケージプロバイダーをポーリングし、NuGet プロバイダーに関連付けます。

2番目のコマンドは、登録されているリポジトリを取得し、結果を表示します。

## PARAMETERS

### -Credential

リポジトリを登録する権限を持つアカウントの資格情報を指定します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Default

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PSGalleryParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InstallationPolicy

インストールポリシーを指定します。 有効な値は、信頼されている、信頼されていないです。 既定値は信頼されていません。

リポジトリのインストールポリシーでは、そのリポジトリからのインストール時に PowerShell の動作を指定します。 信頼されていないリポジトリからモジュールをインストールする場合は、ユーザーに確認を求めるメッセージが表示されます。

**Installationpolicy** は、コマンドレットを使用して設定でき `Set-PSRepository` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Trusted, Untrusted

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

登録するリポジトリの名前を指定します。 この名前を使用して、やなどのコマンドレットでリポジトリを指定することができ `Find-Module` `Install-Module` ます。

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PackageManagementProvider

OneGet パッケージプロバイダーを指定します。 このパラメーターの値を指定しない場合、PowerShellGet は使用可能なパッケージプロバイダーをポーリングし、リポジトリを処理できることを示す最初のパッケージプロバイダーにこのリポジトリを関連付けます。

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
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

### -PublishLocation

発行場所の URI を指定します。 たとえば、NuGet ベースのリポジトリの場合、発行場所はに似てい `https://someNuGetUrl.com/api/v2/Packages` ます。

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptPublishLocation

スクリプトの発行場所を指定します。

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptSourceLocation

スクリプトのソースの場所を指定します。

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceLocation

このリポジトリからモジュールを検出してインストールするための URI を指定します。 URI には、NuGet サーバーフィード (最も一般的な状況)、HTTP、HTTPS、FTP、またはファイルの場所を指定できます。

たとえば、NuGet ベースのリポジトリの場合、ソースの場所はに似てい `https://someNuGetUrl.com/api/v2` ます。

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム.... PSCredential

### System.Uri

## 出力

### System.Object

## 注

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## 関連リンク

[Get-PSRepository](Get-PSRepository.md)

[Set-PSRepository](Set-PSRepository.md)

[Unregister-PSRepository](Unregister-PSRepository.md)
