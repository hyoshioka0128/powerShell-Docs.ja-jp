---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 07/16/2019
online version: https://docs.microsoft.com/powershell/module/powershellget/update-module?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-Module
ms.openlocfilehash: d903cf195bf618b461a5424cdbe06633046c5ae5
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94892554"
---
# Update-Module

## 概要
指定したモジュールの最新バージョンをオンライン ギャラリーからダウンロードし、ローカル コンピューターにインストールします。

## SYNTAX

### すべて

```
Update-Module [[-Name] <String[]>] [-RequiredVersion <String>] [-MaximumVersion <String>]
 [-Credential <PSCredential>] [-Scope <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-Force] [-AllowPrerelease] [-AcceptLicense] [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Update-Module`コマンドレットは、オンラインギャラリーからモジュールの最新バージョンをインストールします。 更新プログラムをインストールする前に、確認を求めるメッセージが表示されます。 更新プログラムは、を使用してローカルコンピューターにインストールされたモジュールに対してのみインストールされ `Install-Module` ます。 `Update-Module``$env:PSModulePath`インストールされているモジュールを検索します。

`Update-Module` パラメーターを指定せずに、インストールされているすべてのモジュールを更新します。 更新するモジュールを指定するには、 **Name** パラメーターを使用します。 **RequiredVersion** パラメーターを使用して、モジュールの特定のバージョンに更新することができます。

インストールされているモジュールが既に最新バージョンである場合、モジュールは更新されません。 モジュールがに見つからない場合 `$env:PSModulePath` は、エラーが表示されます。

インストールされているモジュールを表示するには、を使用 `Get-InstalledModule` します。

## 例

### 例 1: すべてのモジュールを更新する

この例では、オンラインギャラリーで、インストールされているすべてのモジュールを最新バージョンに更新します。

```powershell
Update-Module
```

### 例 2: 名前を指定してモジュールを更新する

この例では、オンラインギャラリーで、特定のモジュールを最新バージョンに更新します。

```powershell
Update-Module -Name SpeculationControl
```

`Update-Module`**Name** パラメーターを使用して、特定のモジュール **SpeculationControl** を更新します。

### 例 3: what-if Update-Module 実行を表示する

この例では、what-if シナリオを実行して、が実行された場合の動作を示し `Update-Module` ます。 コマンドは実行されません。

```powershell
Update-Module -WhatIf
```

```Output
What if: Performing the operation "Update-Module" on target "Version '2.8.0' of module
  'Carbon', updating to version '2.8.1'".
What if: Performing the operation "Update-Module" on target "Version '1.0.10' of module
  'SpeculationControl', updating to version '1.0.14'".
```

`Update-Module`**WhatIf** パラメーターを使用して、が実行された場合の動作を表示し `Update-Module` ます。

### 例 4: 指定したバージョンにモジュールを更新する

この例では、モジュールが特定のバージョンに更新されます。 オンラインギャラリーにバージョンが存在する必要があります。指定しないと、エラーが表示されます。

```powershell
Update-Module -Name SpeculationControl -RequiredVersion 1.0.14
```

`Update-Module`**Name** パラメーターを使用して、モジュール **SpeculationControl** を指定します。 **RequiredVersion** パラメーターは、バージョン **1.0.14** を指定します。

### 例 5: 確認せずにモジュールを更新する

この例では、オンラインギャラリーから最新バージョンにモジュールを更新する確認を要求しません。 モジュールが既にインストールされている場合は、 **Force** パラメーターを指定するとモジュールが再インストールされます。

```powershell
Update-Module -Name SpeculationControl -Force
```

`Update-Module`**Name** パラメーターを使用して、モジュール **SpeculationControl** を指定します。 **Force** パラメーターは、ユーザーの確認を要求せずにモジュールを更新します。

## PARAMETERS

### -AcceptLicense

パッケージで必要な場合は、インストール中にライセンス契約に自動的に同意します。

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

では、プレリリースとマークされた新しいモジュールを使用してモジュールを更新できます。

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

### -Confirm

実行前に確認メッセージを表示 `Update-Module` します。

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

モジュールを更新する権限を持つユーザーアカウントを指定します。

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

確認を要求するメッセージを表示せずに、指定された各モジュールを強制的に更新します。 モジュールが既にインストールされている場合、 **強制的** にモジュールを再インストールします。

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

更新する1つのモジュールの最大バージョンを指定します。 複数のモジュールを更新しようとしている場合、このパラメーターを追加することはできません。 **MaximumVersion** および **RequiredVersion** パラメーターを同じコマンドで使用することはできません。

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

更新する1つ以上のモジュールの名前を指定します。 `Update-Module``$env:PSModulePath`更新するモジュールを検索します。 指定したモジュール名のに一致するものが見つからない場合は `$env:PSModulePath` 、エラーが発生します。

モジュール名にはワイルドカードを許容します。 指定した名前にワイルドカード文字を追加し、一致するものが見つからない場合は、エラーは発生しません。

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

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

**プロキシ** パラメーターによって指定されたプロキシサーバーを使用する権限を持つユーザーアカウントを指定します。

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

### -RequiredVersion

インストールされている既存のモジュールを更新するための正確なバージョンを指定します。 **RequiredVersion** によって指定されたバージョンがオンラインギャラリーに存在している必要があります。または、エラーが表示されます。 1つのコマンドで複数のモジュールが更新された場合、 **RequiredVersion** を使用することはできません。

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

### -スコープ

モジュールのインストール スコープを指定します。 このパラメーターに指定できる値は **AllUsers** と **CurrentUser** です。 **スコープ** が指定されていない場合、更新プログラムは **CurrentUser** スコープにインストールされます。

**AllUsers** スコープには管理者特権が必要です。また、コンピューターのすべてのユーザーがアクセスできる場所にモジュールをインストールします。

`$env:ProgramFiles\PowerShell\Modules`

**CurrentUser** では、昇格されたアクセス許可は必要ありません。また、コンピューターの現在のユーザーのみがアクセスできる場所にモジュールをインストールします。

`$home\Documents\PowerShell\Modules`

**スコープ** が定義されていない場合、既定値は PowerShellGet のバージョンに基づいて設定されます。

- PowerShellGet バージョン2.0.0 以降では、既定値は **CurrentUser** で、インストールの昇格は必要ありません。
- PowerShellGet 1.x バージョンでは、既定値は **AllUsers** で、インストールの昇格が必要です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: CurrentUser, AllUsers

Required: False
Position: Named
Default value: CurrentUser
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

が実行された場合に何が起こるか `Update-Module` を示します。 コマンドレットは実行されません。

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

### System.String

### システム.... PSCredential

### System.Uri

## 出力

### System.Object

## 注

PowerShell バージョン6.0 以降では、既定のインストールスコープは常に **CurrentUser** です。
**CurrentUser** のモジュール更新プログラムには、 `$home\Documents\PowerShell\Modules` 昇格されたアクセス許可は必要ありません。 **AllUsers** のモジュール更新プログラムには、 `$env:ProgramFiles\PowerShell\Modules` 昇格されたアクセス許可が必要です。

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

`Update-Module` powershell 3.0 以降の PowerShell で、windows 7 または Windows 2008 R2 以降のリリースの Windows で実行されます。

**Name** パラメーターで指定したモジュールがを使用してインストールされなかった場合は `Install-Module` 、エラーが発生します。

を実行すると `Update-Module` 、オンラインギャラリーからインストールしたモジュールでのみ実行でき `Install-Module` ます。

が `Update-Module` 使用中のバイナリを更新しようとすると、は `Update-Module` 問題のプロセスを識別するエラーを返します。 ユーザーには、プロセスが停止した後に再試行するように通知され `Update-Module` ます。

## 関連リンク

[Find-Module](Find-Module.md)

[Get-InstalledModule](Get-InstalledModule.md)

[Install-Module](Install-Module.md)

[Publish-Module](Publish-Module.md)

[アンインストール-モジュール](Uninstall-Module.md)

