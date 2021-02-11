---
external help file: Microsoft.PowerShell.PackageManagement.dll-Help.xml
Locale: en-US
Module Name: PackageManagement
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/packagemanagement/import-packageprovider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-PackageProvider
ms.openlocfilehash: 6c19d1cbc7b7e4dc37e24c466f83efae688f3cec
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "99603216"
---
# Import-PackageProvider

## 概要
Package Management パッケージプロバイダーを現在のセッションに追加します。

## SYNTAX

```
Import-PackageProvider [-Name] <String[]> [-RequiredVersion <String>] [-MinimumVersion <String>]
 [-MaximumVersion <String>] [-Force] [-ForceBootstrap] [<CommonParameters>]
```

## Description

`Import-PackageProvider`コマンドレットでは、現在のセッションに1つ以上のパッケージプロバイダーを追加します。
インポートするプロバイダーは、ローカルコンピューターにインストールされている必要があります。

使用可能なプロバイダーの一覧を取得するには、を実行 `Get-PackageProvider -ListAvailable` します。
パッケージプロバイダー名は、モジュール名とは異なる場合があることに注意してください。

セキュリティ上の理由により、 **PackageManagement** では、C# ベースのプロバイダーにが含まれている必要があり `provider.manifest` ます。 が挿入されたプロバイダーをビルドする方法の詳細については `provider.manifest` 、のプロジェクトファイルを参照してください `.csproj` [https://github.com/oneget/oneget](https://github.com/oneget/oneget) 。

## 例

### 例 1: ローカルコンピューターからパッケージプロバイダーをインポートする

```powershell
PS C:\> Import-PackageProvider -Name "Nuget"
```

このコマンドは、ローカルコンピューターにインストールされた Nuget プロバイダーをインポートします。

### 例 2: パッケージプロバイダーの特定のバージョンをインポートする

```powershell
PS C:\> Find-PackageProvider -Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force
Get-PackageProvider -ListAvailable
Import-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Verbose
```

このコマンドは、特定のバージョンの Nuget パッケージプロバイダーを検索し、インストールして、インポートします。

## PARAMETERS

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。
パッケージプロバイダーを再インポートします。

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

### -MaximumVersion

インポートするパッケージプロバイダーの許可される最大バージョンを指定します。 このパラメーターを追加しない場合、では、 `Import-PackageProvider` プロバイダーの利用可能な最も高いバージョンがインポートされます。

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

インポートするパッケージプロバイダーの許可される最小バージョンを指定します。 このパラメーターを追加しない場合、は、 `Import-PackageProvider` *MaximumVersion* パラメーターを使用して指定された最大バージョンも満たす、パッケージの利用可能な最も高いバージョンをインポートします。

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

1つまたは複数のパッケージプロバイダー名を指定します。 ワイルドカードは使用できません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -RequiredVersion

インポートするパッケージプロバイダーの正確なバージョンを指定します。 このパラメーターを追加しない場合、は、 `Import-PackageProvider` **MaximumVersion** パラメーターを使用して指定された最大バージョンを満たすプロバイダーの、利用可能な最も高いバージョンをインポートします。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### Microsoft... 実装. PackageProvider

によって返された **Packageprovider** オブジェクトをにパイプ処理でき `Get-PackageProvider` `Import-PackageProvider` ます。

## 出力

## 注

> [!IMPORTANT]
> 2020 年 4 月時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン 1.0 および 1.1 がサポートされなくなります。 TLS 1.2 以降を使用していない場合、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、確実に TLS 1.2 を使用するようにします。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの[お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/)を参照してください。

## 関連リンク

[about_PackageManagement](../Microsoft.PowerShell.Core/About/about_PackageManagement.md)

[Unregister-PackageSource](Unregister-PackageSource.md)

[Get-PackageSource](Get-PackageSource.md)

[Register-PackageSource](Register-PackageSource.md)

[Get-PackageProvider](Get-PackageProvider.md)
