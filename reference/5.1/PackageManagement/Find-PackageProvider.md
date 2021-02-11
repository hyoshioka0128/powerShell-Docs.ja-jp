---
external help file: Microsoft.PowerShell.PackageManagement.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PackageManagement
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/packagemanagement/find-packageprovider?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-PackageProvider
ms.openlocfilehash: f8ef02a4afb9491375e3a1c37a9b5846eef7e057
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94892901"
---
# Find-PackageProvider

## 概要
インストールに使用できる Package Management パッケージプロバイダーの一覧を返します。

## SYNTAX

```
Find-PackageProvider [[-Name] <String[]>] [-AllVersions] [-Source <String[]>] [-IncludeDependencies]
 [-Credential <PSCredential>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-RequiredVersion <String>]
 [-MinimumVersion <String>] [-MaximumVersion <String>] [-Force] [-ForceBootstrap] [<CommonParameters>]
```

## Description

**検索 PackageProvider** コマンドレットは、PowerShellGet に登録されているパッケージソースで使用できる一致する PackageManagement プロバイダーを検索します。
これらは、Install-PackageProvider コマンドレットを使用したインストールに使用可能なパッケージ プロバイダーです。
既定では、これには、 **PackageManagement** タグと **Provider** タグを持つ PowerShell ギャラリーで使用できるモジュールが含まれます。

**検索-PackageProvider** は、Package Management Azure Blob ストアで利用できる一致する Package Management プロバイダーも検索します。
ブートストラッププロバイダーを使用して、それらを検索してインストールします。

## 例

### 例 1: 使用可能なすべてのパッケージプロバイダーを検索する

```
PS C:\> Find-PackageProvider
```

このコマンドは、Package Management でサポートされているリポジトリで使用可能なすべてのパッケージプロバイダーの一覧を取得します。
既定では、これらのパッケージプロバイダーは、PowerShell ギャラリーと Package Management ブートストラップアプリケーションを使用して入手できます。

### 例 2: プロバイダーのすべてのバージョンを検索する

```
PS C:\> Find-PackageProvider -Name "Nuget" -AllVersions
```

このコマンドは、Nuget という名前のパッケージプロバイダーのすべてのバージョンを検索します。

### 例 3: 指定されたソースからプロバイダーを検索する

```
PS C:\> Find-PackageProvider -Name "Gistprovider" -Source "PSGallery"
```

このコマンドは、指定されたパッケージソースを使用して利用可能なパッケージプロバイダーを検索します。

## PARAMETERS

### -AllVersions

このコマンドレットが、使用可能なすべてのバージョンのパッケージプロバイダーを返すことを示します。
既定では、 **検索-PackageProvider** は、利用可能な最新バージョンのみを返します。

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

### -Credential

パッケージプロバイダーを検索するアクセス許可を持つユーザーアカウントを指定します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。
現時点では、これは *Forcebootstrap* パラメーターと同じです。

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

### -ForceBootstrap

このコマンドレットによって、パッケージプロバイダーを自動的にインストールするように Package Management を強制することを示します。

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

### -IncludeDependencies

このコマンドレットに依存関係が含まれていることを示します。

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

### -MaximumVersion

検索するパッケージプロバイダーの許可される最大バージョンを指定します。
このパラメーターを追加しない場合は、使用可能な最も高いバージョンのプロバイダーが **検索** されます。

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

検索するパッケージプロバイダーの許可される最小バージョンを指定します。
このパラメーターを追加しない場合、 **検索-PackageProvider** は、 *MaximumVersion* パラメーターで指定された、指定された最大バージョンを満たすパッケージの利用可能な最も高いバージョンを検索します。

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

1つまたは複数のパッケージプロバイダーモジュール名、またはワイルドカード文字を含むプロバイダー名を指定します。
複数のパッケージ名はコンマで区切ります。

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
Accept pipeline input: False
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
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredVersion

検索するパッケージプロバイダーの完全に許可されているバージョンを指定します。
このパラメーターを追加しない場合、 **検索-PackageProvider** は、 *MaximumVersion* パラメーターで指定された最大バージョンも満たす、使用可能な最大バージョンのプロバイダーを検索します。

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

### -Source

1つまたは複数のパッケージソースを指定します。
使用可能なパッケージソースの一覧を取得するには、Get-PackageSource コマンドレットを使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

## 出力

### Microsoft. パッケージ Id

このコマンドレットは、 **ソフトウェア id** オブジェクトを返します。
**ソフトウェア id** オブジェクトを **インストール-packageprovider** にパイプして、**検索** 結果をインストールすることができます。

## 注

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## 関連リンク

[about_PackageManagement](../Microsoft.PowerShell.Core/About/about_PackageManagement.md)

[Unregister-PackageSource](Unregister-PackageSource.md)

[Get-PackageSource](Get-PackageSource.md)

[Register-PackageSource](Register-PackageSource.md)

[Install-PackageProvider](Install-PackageProvider.md)
