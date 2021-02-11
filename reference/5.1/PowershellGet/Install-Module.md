---
external help file: PSModule-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: PowerShellGet
ms.date: 08/03/2020
online version: https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Install-Module
ms.openlocfilehash: ec9862e9003bd73e952422a8d15d373193a80c12
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94889778"
---
# Install-Module

## 概要
1つ以上のモジュールをリポジトリからダウンロードし、ローカルコンピューターにインストールします。

## SYNTAX

### NameParameterSet (既定値)

```
Install-Module [-Name] <String[]> [-MinimumVersion <String>] [-MaximumVersion <String>]
 [-RequiredVersion <String>] [-Repository <String[]>] [-Credential <PSCredential>] [-Scope <String>]
 [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-AllowClobber] [-SkipPublisherCheck] [-Force]
 [-AllowPrerelease] [-AcceptLicense] [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObject

```
Install-Module [-InputObject] <PSObject[]> [-Credential <PSCredential>] [-Scope <String>]
 [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-AllowClobber] [-SkipPublisherCheck] [-Force]
 [-AcceptLicense] [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Install-Module`コマンドレットは、指定された条件を満たす1つ以上のモジュールをオンラインリポジトリから取得します。 コマンドレットでは、検索結果が有効なモジュールであることを確認し、モジュールフォルダーをインストール場所にコピーします。 インストールされたモジュールは、インストール後に自動的にインポートされません。
指定されたモジュールの最小、最大、および正確なバージョンに基づいて、どのモジュールがインストールされているかをフィルター処理できます。

インストールされているモジュールの名前またはバージョンが同じであるか、既存のモジュールにコマンドが含まれている場合は、警告メッセージが表示されます。 モジュールをインストールして警告をオーバーライドすることを確認したら、 `-Force` パラメーターとパラメーターを使用し `-AllowClobber` ます。 リポジトリの設定によっては、モジュールのインストールを続行するためのプロンプトが表示される場合があります。

これらの例では、登録されている唯一のリポジトリとして [PowerShell ギャラリー](https://www.powershellgallery.com/) を使用します。 `Get-PSRepository` 登録されているリポジトリを表示します。 複数の登録済みリポジトリがある場合は、パラメーターを使用して、 `-Repository` リポジトリの名前を指定します。

## 例

### 例 1: モジュールを検索してインストールする

この例では、リポジトリ内のモジュールを検索し、モジュールをインストールします。

```powershell
Find-Module -Name PowerShellGet | Install-Module
```

では、 `Find-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定しています。 既定では、最新バージョンのモジュールがリポジトリからダウンロードされます。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Install-Module` ます。 `Install-Module` のすべてのユーザーのモジュールをインストール `$env:ProgramFiles\WindowsPowerShell\Modules` します。

### 例 2: 名前を指定してモジュールをインストールする

この例では、 **PowerShellGet** モジュールの最新バージョンがインストールされています。

```powershell
Install-Module -Name PowerShellGet
```

では、 `Install-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定しています。 既定では、最新バージョンのモジュールがリポジトリからダウンロードされ、インストールされます。

### 例 3: 最小バージョンを使用してモジュールをインストールする

この例では、 **PowerShellGet** モジュールの最小バージョンがインストールされています。 **MinimumVersion** パラメーターは、インストールする必要があるモジュールの最小バージョンを指定します。 新しいバージョンのモジュールを使用できる場合は、そのバージョンがダウンロードされ、すべてのユーザーにインストールされます。

```powershell
Install-Module -Name PowerShellGet -MinimumVersion 2.0.1
```

では、 `Install-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定しています。 **MinimumVersion** パラメーターは、バージョン **2.0.1** がリポジトリからダウンロードされ、インストールされることを指定します。 バージョン **2.0.4** が使用可能なので、このバージョンはすべてのユーザーにダウンロードされ、インストールされます。

### 例 4: モジュールの特定のバージョンをインストールする

この例では、 **PowerShellGet** モジュールの特定のバージョンがインストールされています。

```powershell
Install-Module -Name PowerShellGet -RequiredVersion 2.0.0
```

では、 `Install-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定しています。 **RequiredVersion** パラメーターは、すべてのユーザーにバージョン **2.0.0** がダウンロードされ、インストールされることを指定します。

### 例 5: 現在のユーザーに対してのみモジュールをインストールする

この例では、最新バージョンのモジュールをダウンロードし、現在のユーザーに対してのみインストールします。

```powershell
Install-Module -Name PowerShellGet -Scope CurrentUser
```

では、 `Install-Module` **Name** パラメーターを使用して **PowerShellGet** モジュールを指定しています。
`Install-Module` 最新バージョンの **PowerShellGet** をダウンロードし、現在のユーザーのディレクトリ () にインストールし `$home\Documents\WindowsPowerShell\Modules` ます。

## PARAMETERS

### -AcceptLicense

ライセンスが必要なモジュールの場合、 **Acceptlicense** はインストール中にライセンス契約に自動的に同意します。 詳細については、「ライセンスへの [同意が必要なモジュール](/powershell/scripting/gallery/concepts/module-license-acceptance)」を参照してください。

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

### -AllowClobber

コンピューター上の既存のコマンドに関するインストールの競合に関する警告メッセージを上書きします。
モジュールによってインストールされたコマンドと同じ名前を持つ既存のコマンドを上書きします。
**AllowClobber** と **Force** は、コマンドで一緒に使用でき `Install-Module` ます。

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

プレリリースとしてマークされているモジュールをインストールできます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

コマンドレットを実行する前に確認メッセージを表示 `Install-Module` します。

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

### -Force

モジュールをインストールし、モジュールのインストール競合に関する警告メッセージを上書きします。 同じ名前のモジュールがコンピューターに既に存在する場合、 **Force** では複数のバージョンをインストールできます。 同じ名前とバージョンのモジュールが既に存在する場合、 **強制的** にそのバージョンが上書きされます。 **Force** と **AllowClobber** は、コマンドで一緒に使用でき `Install-Module` ます。

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

パイプラインの入力に使用されます。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -MaximumVersion

インストールする1つのモジュールの最大バージョンを指定します。 インストールされているバージョンは、 **MaximumVersion** 以下である必要があります。 複数のモジュールをインストールする場合は、 **MaximumVersion** を使用できません。 **MaximumVersion** と **RequiredVersion** を同じコマンドで使用することはできません `Install-Module` 。

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MinimumVersion

インストールする1つのモジュールの最小バージョンを指定します。 インストールされているバージョンは、 **MinimumVersion** 以上である必要があります。 使用可能な新しいバージョンのモジュールがある場合は、新しいバージョンがインストールされます。 複数のモジュールをインストールする場合は、 **MinimumVersion** を使用することはできません。
**MinimumVersion** および **RequiredVersion** を同じコマンドで使用することはできません `Install-Module` 。

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

オンラインギャラリーからインストールするモジュールの正確な名前を指定します。 モジュール名のコンマ区切りのリストが受け入れられます。 モジュール名は、リポジトリ内のモジュール名と一致する必要があります。 `Find-Module`モジュール名の一覧を取得するには、を使用します。

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

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

**Repository** パラメーターを使用して、モジュールのダウンロードとインストールに使用するリポジトリを指定します。 複数のリポジトリが登録されている場合に使用します。 コマンドで登録されているリポジトリの名前を指定し `Install-Module` ます。 リポジトリを登録するには、を使用 `Register-PSRepository` します。
登録されているリポジトリを表示するには、を使用 `Get-PSRepository` します。

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredVersion

インストールする1つのモジュールの正確なバージョンを指定します。 指定したバージョンのリポジトリに一致するものがない場合は、エラーが表示されます。 複数のモジュールをインストールする場合は、 **RequiredVersion** を使用できません。 **RequiredVersion** `Install-Module` は、 **MinimumVersion** または **MaximumVersion** と同じコマンドでは使用できません。

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -スコープ

モジュールのインストール スコープを指定します。 このパラメーターに指定できる値は **AllUsers** と **CurrentUser** です。

**AllUsers** スコープは、コンピューターのすべてのユーザーがアクセスできる場所にモジュールをインストールします。

`$env:ProgramFiles\WindowsPowerShell\Modules`

**CurrentUser** を実行すると、コンピューターの現在のユーザーのみがアクセスできる場所にモジュールがインストールされます。 次に例を示します。

`$home\Documents\WindowsPowerShell\Modules`

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
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipPublisherCheck

コンピューターに既に存在するモジュールの新しいバージョンをインストールできます。 たとえば、既存のモジュールが信頼できる発行元によってデジタル署名されていても、新しいバージョンが信頼できる発行元によってデジタル署名されていない場合などです。

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

### -WhatIf

`Install-Module`コマンドが実行された場合に何が起こるかを示します。 このコマンドレットは実行されません。

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

### できる psrepositoryiteminfo

`Find-Module` パイプラインの下に送信できる **できる psrepositoryiteminfo** オブジェクトを作成し `Install-Module` ます。

### System.String[]

### システムの管理. PSObject []

### System.String

### システム.... PSCredential

### System.Uri

## 出力

### できる psrepositoryiteminfo です。

**PassThru** パラメーターを使用すると、 `Install-Module` モジュールの **できる psrepositoryiteminfo** オブジェクトが出力されます。 これは、コマンドレットから取得した情報と同じです `Find-Module` 。

## 注

`Install-Module` は、windows 7 または Windows 2008 R2 以降のリリースの Windows で、PowerShell 5.0 以降のリリースで実行されます。

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

セキュリティのベストプラクティスとして、コマンドレットまたは関数を初めて実行する前に、モジュールのコードを評価することをお勧めします。 悪意のあるコードを含むモジュールの実行を防ぐため、インストールされたモジュールはインストール後に自動的にインポートされません。

**Name** パラメーターによって指定されたモジュール名がリポジトリに存在しない場合、は `Install-Module` エラーを返します。

複数のモジュールをインストールするには、 **Name** パラメーターを使用して、コンマで区切られたモジュール名の配列を指定します。 複数のモジュール名を指定する場合は、 **MinimumVersion**、 **MaximumVersion**、または **RequiredVersion** を使用することはできません。 `Find-Module` パイプラインの下に送信できる **できる psrepositoryiteminfo** オブジェクトを作成し `Install-Module` ます。 パイプラインは、複数のモジュールを指定して1つのコマンドでインストールするもう1つの方法です。

既定では、 **AllUsers** のスコープのモジュールはにインストールされ `$env:ProgramFiles\WindowsPowerShell\Modules` ます。 既定では、PowerShell Desired State Configuration (DSC) リソースをインストールするときに混乱を防ぐことができます。

モジュールのインストールが失敗し、 `.psm1` `.psd1` `.dll` フォルダー内に同じ名前の、、またはがない場合は、インポートできません。 モジュールをインストールするには、 **Force** パラメーターを使用します。

既存のモジュールのバージョンが **name** パラメーターで指定された名前と一致し、 **MinimumVersion** または **RequiredVersion** パラメーターが使用されていない場合、は `Install-Module` 警告なしで続行しますが、モジュールはインストールされません。

既存のモジュールのバージョンが **MinimumVersion** パラメーターの値より大きいか、または **RequiredVersion** パラメーターの値と等しい場合、は `Install-Module` 警告なしで続行しますが、モジュールはインストールされません。

既存のモジュールが **MinimumVersion** パラメーターまたは **RequiredVersion** パラメーターによって指定された値と一致しない場合、コマンドでエラーが発生し `Install-Module` ます。 たとえば、既存のインストールされているモジュールのバージョンが **MinimumVersion** 値より小さいか、 **RequiredVersion** 値と等しくない場合などです。

モジュールをインストールすると、モジュールの発行元によって要求された依存モジュールもインストールされます。
発行元は、モジュールマニフェストで必要なモジュールとそのバージョンを指定します。

## 関連リンク

[Find-Module](Find-Module.md)

[Get-PSRepository](Get-PSRepository.md)

[Import-Module](../Microsoft.PowerShell.Core/Import-Module.md)

[Publish-Module](Publish-Module.md)

[Register-PSRepository](Register-PSRepository.md)

[Uninstall-Module](Uninstall-Module.md)

[Update-Module](Update-Module.md)

[about_Module](../Microsoft.PowerShell.Core/About/about_Modules.md)
