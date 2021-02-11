---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.date: 11/11/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/save-module?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Save-Module
ms.openlocfilehash: df5056b4402664602409388825c8b2b8acd4e02f
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "99600988"
---
# Save-Module

## 概要
モジュールとその依存関係をローカルコンピューターに保存しますが、モジュールはインストールしません。

## SYNTAX

### NameAndPathParameterSet (既定値)

```
Save-Module [-Name] <String[]> [-MinimumVersion <String>] [-MaximumVersion <String>]
 [-RequiredVersion <String>] [-Repository <String[]>] [-Path] <String> [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-Credential <PSCredential>] [-Force] [-AllowPrerelease]
 [-AcceptLicense] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameAndLiteralPathParameterSet

```
Save-Module [-Name] <String[]> [-MinimumVersion <String>] [-MaximumVersion <String>]
 [-RequiredVersion <String>] [-Repository <String[]>] -LiteralPath <String> [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-Credential <PSCredential>] [-Force] [-AllowPrerelease]
 [-AcceptLicense] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObjectAndLiteralPathParameterSet

```
Save-Module [-InputObject] <PSObject[]> -LiteralPath <String> [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-Credential <PSCredential>] [-Force] [-AcceptLicense] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### InputObjectAndPathParameterSet

```
Save-Module [-InputObject] <PSObject[]> [-Path] <String> [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-Credential <PSCredential>] [-Force] [-AcceptLicense] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Description

この `Save-Module` コマンドレットは、登録されているリポジトリからモジュールと依存関係をダウンロードします。
`Save-Module` 最新バージョンのモジュールをダウンロードして保存します。 ファイルは、ローカルコンピューター上の指定されたパスに保存されます。 モジュールがインストールされていませんが、管理者が内容を検査することができます。 保存したモジュールは、オフラインコンピューターの適切な場所にコピーでき `$env:PSModulePath` ます。

`Get-PSRepository` ローカルコンピューターの登録済みリポジトリが表示されます。 コマンドレットを使用して、 `Find-Module` 登録されているリポジトリを検索できます。

## 例

### 例 1: モジュールを保存する

この例では、モジュールとその依存関係がローカルコンピューターに保存されます。

```powershell
Save-Module -Name PowerShellGet -Path C:\Test\Modules -Repository PSGallery
Get-ChildItem -Path C:\Test\Modules
```

```Output
    Directory: C:\Test\Modules

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         7/1/2019     13:31                PackageManagement
d-----         7/1/2019     13:31                PowerShellGet
```

`Save-Module`**Name** パラメーターを使用して、モジュールの **PowerShellGet** を指定します。 **Path** パラメーターでは、ダウンロードしたモジュールを格納する場所を指定します。 **Repository** パラメーターは、登録されているリポジトリ **PSGallery** を指定します。 ダウンロードが完了すると、 `Get-ChildItem` ファイルが格納されている **パス** の内容がに表示されます。

### 例 2: モジュールの特定のバージョンを保存する

この例では、 **MaximumVersion** や **RequiredVersion** などのパラメーターを使用して、モジュールのバージョンを指定する方法を示します。

```powershell
Save-Module -Name PowerShellGet -Path C:\Test\Modules -Repository PSGallery -MaximumVersion 2.1.0
Get-ChildItem -Path C:\Test\Modules\PowerShellGet\
```

```Output
    Directory: C:\Test\Modules\PowerShellGet

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         7/1/2019     13:40                2.1.0
```

`Save-Module`**Name** パラメーターを使用して、モジュールの **PowerShellGet** を指定します。 **Path** パラメーターでは、ダウンロードしたモジュールを格納する場所を指定します。 **Repository** パラメーターは、登録されているリポジトリ **PSGallery** を指定します。 **MaximumVersion** は、バージョン **2.1.0** をダウンロードして保存することを指定します。 ダウンロードが完了すると、 `Get-ChildItem` ファイルが格納されている **パス** の内容がに表示されます。

### 例 3: 特定のバージョンのモジュールを検索して保存する

この例では、必要なモジュールバージョンがリポジトリにあり、ローカルコンピューターに保存されています。

```powershell
Find-Module -Name PowerShellGet -Repository PSGallery -RequiredVersion 1.6.5 |
  Save-Module -Path C:\Test\Modules
Get-ChildItem -Path C:\Test\Modules\PowerShellGet
```

```Output
    Directory: C:\Test\Modules\PowerShellGet

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         7/1/2019     14:04                1.6.5
```

`Find-Module`**Name** パラメーターを使用して、モジュールの **PowerShellGet** を指定します。 **Repository** パラメーターは、登録されているリポジトリ **PSGallery** を指定します。 **RequiredVersion** バージョン **1.6.5** を指定します。

オブジェクトは、パイプライン内でに送信され `Save-Module` ます。 **Path** パラメーターでは、ダウンロードしたモジュールを格納する場所を指定します。 ダウンロードが完了すると、 `Get-ChildItem` ファイルが格納されている **パス** の内容がに表示されます。

## PARAMETERS

### -AcceptLicense

パッケージで必要な場合は、使用許諾契約書に自動的に同意します。

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

### -AllowPrerelease リリース

プレリリースとしてマークされているモジュールを保存できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameAndPathParameterSet, NameAndLiteralPathParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

を実行する前に確認メッセージを表示し `Save-Module` ます。

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

モジュールを保存する権限を持つユーザーアカウントを指定します。

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

### -Force

`Save-Module`ユーザーの確認を求めることなく強制的に実行します。

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

### -InputObject

**できる psrepositoryiteminfo** オブジェクトを受け取ります。 たとえば、 `Find-Module` 変数に出力し、その変数を **InputObject** 引数として使用します。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: InputObjectAndLiteralPathParameterSet, InputObjectAndPathParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** パラメーターの値は、入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 PowerShell では、単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: NameAndLiteralPathParameterSet, InputObjectAndLiteralPathParameterSet
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaximumVersion

保存するモジュールの最大バージョンまたは最新バージョンを指定します。 **MaximumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.String
Parameter Sets: NameAndPathParameterSet, NameAndLiteralPathParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MinimumVersion

保存する1つのモジュールの最小バージョンを指定します。 複数のモジュールをインストールしようとする場合、このパラメーターを追加することはできません。 **MinimumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.String
Parameter Sets: NameAndPathParameterSet, NameAndLiteralPathParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

保存するモジュールの名前の配列を指定します。

```yaml
Type: System.String[]
Parameter Sets: NameAndPathParameterSet, NameAndLiteralPathParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

保存されているモジュールを格納するローカルコンピューター上の場所を指定します。 ワイルドカード文字を受け入れます。

```yaml
Type: System.String
Parameter Sets: NameAndPathParameterSet, InputObjectAndPathParameterSet
Aliases:

Required: True
Position: 1
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

を実行して登録されているリポジトリのフレンドリ名を指定し `Register-PSRepository` ます。 `Get-PSRepository`登録されたリポジトリを表示するには、を使用します。

```yaml
Type: System.String[]
Parameter Sets: NameAndPathParameterSet, NameAndLiteralPathParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -RequiredVersion

保存するモジュールの正確なバージョン番号を指定します。

```yaml
Type: System.String
Parameter Sets: NameAndPathParameterSet, NameAndLiteralPathParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -WhatIf

が実行された場合に何が起こるかを示し `Save-Module` ます。 コマンドレットは実行されません。

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

### System.String[]

### システムの管理. PSObject []

### System.String

### System.Uri

### システム.... PSCredential

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
