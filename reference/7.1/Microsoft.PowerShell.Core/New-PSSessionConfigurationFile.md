---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/24/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-pssessionconfigurationfile?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-PSSessionConfigurationFile
ms.openlocfilehash: 939dc011307b4c98340bb376032b96289e1c1bc3
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94345954"
---
# New-PSSessionConfigurationFile

## 概要
セッション構成を定義するファイルを作成します。

## SYNTAX

```
New-PSSessionConfigurationFile [-Path] <String> [-SchemaVersion <Version>] [-Guid <Guid>]
 [-Author <String>] [-Description <String>] [-CompanyName <String>] [-Copyright <String>]
 [-SessionType <SessionType>] [-TranscriptDirectory <String>] [-RunAsVirtualAccount]
 [-RunAsVirtualAccountGroups <String[]>] [-MountUserDrive] [-UserDriveMaximumSize <Int64>]
 [-GroupManagedServiceAccount <String>] [-ScriptsToProcess <String[]>]
 [-RoleDefinitions <IDictionary>] [-RequiredGroups <IDictionary>] [-LanguageMode <PSLanguageMode>]
 [-ExecutionPolicy <ExecutionPolicy>] [-PowerShellVersion <Version>] [-ModulesToImport <Object[]>]
 [-VisibleAliases <String[]>] [-VisibleCmdlets <Object[]>] [-VisibleFunctions <Object[]>]
 [-VisibleExternalCommands <String[]>] [-VisibleProviders <String[]>]
 [-AliasDefinitions <IDictionary[]>] [-FunctionDefinitions <IDictionary[]>]
 [-VariableDefinitions <Object>] [-EnvironmentVariables <IDictionary>] [-TypesToProcess <String[]>]
 [-FormatsToProcess <String[]>] [-AssembliesToLoad <String[]>] [-Full] [<CommonParameters>]
```

## Description

この `New-PSSessionConfigurationFile` コマンドレットは、セッション構成を定義する設定のファイルと、セッション構成を使用して作成されたセッションの環境を作成します。
セッション構成でファイルを使用するには、 **Path** `Register-PSSessionConfiguration` コマンドレットまたはコマンドレットの Path パラメーターを使用し `Set-PSSessionConfiguration` ます。

によって作成されるセッション構成ファイル `New-PSSessionConfigurationFile` は、セッション構成のプロパティと値のハッシュテーブルを含む、人間が判読できるテキストファイルです。 ファイル名の拡張子が付いてい `.pssc` ます。

`New-PSSessionConfigurationFile` **Path** パラメーターを除き、のすべてのパラメーターは省略可能です。
パラメーターを省略すると、パラメーターの説明に記載された部分を除き、セッション構成ファイル内の対応するキーがコメント アウトされます。

セッション構成は、エンドポイントとも呼ばれ、コンピューターに接続する PowerShell **セッション (pssession** ) の環境を定義するローカルコンピューター上の設定のコレクションです。 すべての pssession は、セッション構成を **使用します** 。 特定のセッション構成を指定するには、コマンドレットなど、セッションを作成するコマンドレットの **ConfigurationName** パラメーターを使用し `New-PSSession` ます。

session configuration file は、セッション構成の定義をより容易にします。複雑なスクリプトやコード アセンブリは必要ありません。 ファイルの設定は、セッション構成内のオプションのスタートアップスクリプトとアセンブリで使用されます。

セッション構成とセッション構成ファイルの詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md) 」および「 [about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)」を参照してください。

このコマンドレットは、PowerShell 3.0 で導入されました。

## 例

### 例 1: NoLanguage セッションの作成と使用

この例では、言語なしのセッションを使用してを作成する方法を示します。

これには次の手順が含まれます。

1. 新しい構成ファイルを作成します。
1. 構成を登録します。
1. 構成を使用する新しいセッションを作成します。
1. その新しいセッションでコマンドを実行します。

この例のコマンドを実行するには、[管理者として実行] オプションを使用して PowerShell を起動します。 このオプションは、コマンドレットを実行するために必要です `Register-PSSessionConfiguration` 。

```powershell
New-PSSessionConfigurationFile -Path .\NoLanguage.pssc -LanguageMode NoLanguage
Register-PSSessionConfiguration -Path .\NoLanguage.pssc -Name NoLanguage -Force
$NoLanguageSession = New-PSSession -ComputerName Srv01 -ConfigurationName NoLanguage
Invoke-Command -Session $NoLanguageSession -ScriptBlock {
  if ((Get-Date) -lt '1January2099') {'Before'} else {'After'}
}
```

```Output
The syntax is not supported by this runspace. This might be because it is in no-language mode.
    + CategoryInfo          : ParserError: (if ((Get-Date) ...') {'Before'}  :String) [], ParseException
    + FullyQualifiedErrorId : ScriptsNotAllowed
    + PSComputerName        : localhost
```

この例では、 `Invoke-Command` **LanguageMode** が **nolanguage** に設定されているため、は失敗します。

### 例 2: RestrictedLanguage セッションの作成と使用

この例では、言語なしのセッションを使用してを作成する方法を示します。

これには次の手順が含まれます。

1. 新しい構成ファイルを作成します。
1. 構成を登録します。
1. 構成を使用する新しいセッションを作成します。
1. その新しいセッションでコマンドを実行します。

この例のコマンドを実行するには、[管理者として実行] オプションを使用して PowerShell を起動します。 このオプションは、コマンドレットを実行するために必要です `Register-PSSessionConfiguration` 。

```powershell
New-PSSessionConfigurationFile -Path .\NoLanguage.pssc -LanguageMode RestrictedLanguage
Register-PSSessionConfiguration -Path .\NoLanguage.pssc -Name RestrictedLanguage -Force
$RestrictedSession = New-PSSession -ComputerName Srv01 -ConfigurationName RestrictedLanguage
Invoke-Command -Session $RestrictedSession -ScriptBlock {
  if ((Get-Date) -lt '1January2099') {'Before'} else {'After'}
}
```

```Output
Before
```

この例では、 `Invoke-Command` **LanguageMode** が **RestrictedLanguage** に設定されているため、は成功します。

### 例 3: セッション構成ファイルを変更する

この例では、"ITTasks" という名前の既存のセッションで使用されているセッション構成ファイルを変更する方法を示します。 以前は、これらのセッションにはコアモジュールと内部 **Ittasks** モジュールのみがありました。 管理者は、ITTasks セッション構成を使用して作成されたセッションに **Psscheduledjob** モジュールを追加する必要があります。

```powershell
New-PSSessionConfigurationFile -Path .\New-ITTasks.pssc -ModulesToImport Microsoft*, ITTasks, PSScheduledJob
Set-PSSessionConfiguration -Name ITTasks -Path .\New-ITTasks.pssc
```

コマンドレットを作成して、 `New-PSSessionConfigurationFile` 必要なモジュールをインポートするセッション構成ファイルを作成します。 コマンドレットでは、 `Set-PSSessionConfiguration` 現在の構成ファイルを新しい構成ファイルに置き換えます。 この新しい構成は、変更後に作成された新しいセッションにのみ影響します。
既存の "ITTasks" セッションは影響を受けません。

### 例 4: セッション構成ファイルを編集する

この例は、構成ファイルのアクティブなセッション構成のコピーを編集することで、セッション構成を変更する方法を示しています。 構成ファイルのセッション構成のコピーを変更するには、ファイルへのフルコントロールアクセス権を持っている必要があります。 このため、ファイルのアクセス許可を変更することが必要になる場合があります。

このシナリオでは、 `Select-String` アクティブな構成ファイルを編集して、コマンドレットの新しいエイリアスを追加します。

次のコード例では、次の手順を実行してこの変更を行います。

1. ITConfig セッションの構成ファイルのパスを取得します。
1. ユーザーは、 **Notepad.exe** を使用して構成ファイルを編集し、 **エイリアス定義** の値を次のように変更します `AliasDefinitions = @(@{Name='slst';Value='Select-String'})` 。
1. 更新された構成ファイルをテストします。

```powershell
$ITConfig = Get-PSSessionConfiguration -Name ITConfig
notepad.exe $ITConfig.ConfigFilePath
Test-PSSessionConfigurationFile -Path $ITConfig.ConfigFilePath
```

```Output
True
```

で **Verbose** パラメーターを使用して、 `Test-PSSessionConfigurationFile` 検出されたエラーを表示します。 `$True`ファイルでエラーが検出されなかった場合、コマンドレットはを返します。

### 例 5: サンプル構成ファイルを作成する

この例は `New-PSSessionConfigurationFile` 、すべてのコマンドレットパラメーターを使用するコマンドを示しています。
各パラメーターの正しい入力形式を示すために含まれています。

最終的な SampleFile.pssc が出力に表示されます。

```powershell
$configSettings = @{
    Path = '.\SampleFile.pssc'
    SchemaVersion = '1.0.0.0'
    Author = 'User01'
    Copyright = '(c) Fabrikam Corporation. All rights reserved.'
    CompanyName = 'Fabrikam Corporation'
    Description = 'This is a sample file.'
    ExecutionPolicy = 'AllSigned'
    PowerShellVersion = '3.0'
    LanguageMode = 'FullLanguage'
    SessionType = 'Default'
    EnvironmentVariables = @{TESTSHARE='\\Test2\Test'}
    ModulesToImport = @{ModuleName='PSScheduledJob'; ModuleVersion='1.0.0.0'; GUID='50cdb55f-5ab7-489f-9e94-4ec21ff51e59'},'PSDiagnostics'
    AssembliesToLoad = 'System.Web.Services','FSharp.Compiler.CodeDom.dll'
    TypesToProcess = 'Types1.ps1xml','Types2.ps1xml'
    FormatsToProcess = 'CustomFormats.ps1xml'
    ScriptsToProcess = 'Get-Inputs.ps1'
    AliasDefinitions = @{Name='hlp';Value='Get-Help';Description='Gets help.';Options='AllScope'},
        @{Name='Update';Value='Update-Help';Description='Updates help';Options='ReadOnly'}
    FunctionDefinitions = @{Name='Get-Function';ScriptBlock={Get-Command -CommandType Function};Options='ReadOnly'}
    VariableDefinitions = @{Name='WarningPreference';Value='SilentlyContinue'}
    VisibleAliases = 'c*','g*','i*','s*'
    VisibleCmdlets = 'Get*'
    VisibleFunctions = 'Get*'
    VisibleProviders = 'FileSystem','Function','Variable'
    RunAsVirtualAccount = $true
    RunAsVirtualAccountGroups = 'Backup Operators'
}
New-PSSessionConfigurationFile @configSettings
Get-Content SampleFile.pssc
```

```Output
@{

# Version number of the schema used for this document
SchemaVersion = '1.0.0.0'

# ID used to uniquely identify this document
GUID = '1caeff7f-27ca-4360-97cf-37846f594235'

# Author of this document
Author = 'User01'

# Description of the functionality provided by these settings
Description = 'This is a sample file.'

# Company associated with this document
CompanyName = 'Fabrikam Corporation'

# Copyright statement for this document
Copyright = '(c) Fabrikam Corporation. All rights reserved.'

# Session type defaults to apply for this session configuration. Can be 'RestrictedRemoteServer' (recommended), 'Empty', or 'Default'
SessionType = 'Default'

# Directory to place session transcripts for this session configuration
# TranscriptDirectory = 'C:\Transcripts\'

# Whether to run this session configuration as the machine's (virtual) administrator account
RunAsVirtualAccount = $true

# Groups associated with machine's (virtual) administrator account
RunAsVirtualAccountGroups = 'Backup Operators'

# Scripts to run when applied to a session
ScriptsToProcess = 'Get-Inputs.ps1'

# User roles (security groups), and the role capabilities that should be applied to them when applied to a session
# RoleDefinitions = @{ 'CONTOSO\SqlAdmins' = @{ RoleCapabilities = 'SqlAdministration' }; 'CONTOSO\SqlManaged' = @{ RoleCapabilityFiles = 'C:\RoleCapability\SqlManaged.psrc' }; 'CONTOSO\ServerMonitors' = @{ VisibleCmdlets = 'Get-Process' } }

# Language mode to apply when applied to a session. Can be 'NoLanguage' (recommended), 'RestrictedLanguage', 'ConstrainedLanguage', or 'FullLanguage'
LanguageMode = 'FullLanguage'

# Execution policy to apply when applied to a session
ExecutionPolicy = 'AllSigned'

# Version of the PowerShell engine to use when applied to a session
PowerShellVersion = '3.0'

# Modules to import when applied to a session
ModulesToImport = @{
    'GUID' = '50cdb55f-5ab7-489f-9e94-4ec21ff51e59'
    'ModuleName' = 'PSScheduledJob'
    'ModuleVersion' = '1.0.0.0' }, 'PSDiagnostics'

# Aliases to make visible when applied to a session
VisibleAliases = 'c*', 'g*', 'i*', 's*'

# Cmdlets to make visible when applied to a session
VisibleCmdlets = 'Get*'

# Functions to make visible when applied to a session
VisibleFunctions = 'Get*'

# Providers to make visible when applied to a session
VisibleProviders = 'FileSystem', 'Function', 'Variable'

# Aliases to be defined when applied to a session
AliasDefinitions = @{
    'Description' = 'Gets help.'
    'Name' = 'hlp'
    'Options' = 'AllScope'
    'Value' = 'Get-Help' }, @{
    'Description' = 'Updates help'
    'Name' = 'Update'
    'Options' = 'ReadOnly'
    'Value' = 'Update-Help' }

# Functions to define when applied to a session
FunctionDefinitions = @{
    'Name' = 'Get-Function'
    'Options' = 'ReadOnly'
    'ScriptBlock' = {Get-Command -CommandType Function} }

# Variables to define when applied to a session
VariableDefinitions = @{
    'Name' = 'WarningPreference'
    'Value' = 'SilentlyContinue' }

# Environment variables to define when applied to a session
EnvironmentVariables = @{
    'TESTSHARE' = '\\Test2\Test' }

# Type files (.ps1xml) to load when applied to a session
TypesToProcess = 'Types1.ps1xml', 'Types2.ps1xml'

# Format files (.ps1xml) to load when applied to a session
FormatsToProcess = 'CustomFormats.ps1xml'

# Assemblies to load when applied to a session
AssembliesToLoad = 'System.Web.Services', 'FSharp.Compiler.CodeDom.dll'

}
```

## PARAMETERS

### -エイリアスの定義

セッション構成を使用するセッションに、指定されたエイリアスを追加します。 ハッシュ テーブルに次のキーを入力します。

- 名前-エイリアスの名前。 このキーは必須です。
- Value-エイリアスが表すコマンド。 このキーは必須です。
- Description-エイリアスを説明するテキスト文字列。 このキーはオプションです。
- オプション-エイリアスオプション。 このキーはオプションです。 既定値は **None** です。 このパラメーターに指定できる値は、None、ReadOnly、Constant、Private、または AllScope です。

例: `@{Name='hlp';Value='Get-Help';Description='Gets help';Options='ReadOnly'}`

```yaml
Type: System.Collections.IDictionary[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AssembliesToLoad

セッション構成を使用するセッションに読み込むためのアセンブリを指定します。

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

### -作成者

セッション構成または構成ファイルの作成者を指定します。 既定値は現在のユーザーです。 このパラメーターの値は、セッション構成ファイルに表示されますが、セッション構成オブジェクトのプロパティではありません。

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

### -CompanyName

セッション構成または構成ファイルを作成した会社を指定します。 既定値は **Unknown** です。 このパラメーターの値は、セッション構成ファイルに表示されますが、セッション構成オブジェクトのプロパティではありません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Unknown
Accept pipeline input: False
Accept wildcard characters: False
```

### -Copyright

セッション構成ファイルの著作権を指定します。 このパラメーターの値は、セッション構成ファイルに表示されますが、セッション構成オブジェクトのプロパティではありません。

このパラメーターを省略すると、は `New-PSSessionConfigurationFile` **Author** パラメーターの値を使用して著作権情報ステートメントを生成します。

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

### -Description

セッション構成またはセッション構成ファイルの説明を指定します。 このパラメーターの値は、セッション構成ファイルに表示されますが、セッション構成オブジェクトのプロパティではありません。

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

### -EnvironmentVariables

環境変数をセッションに追加します。 キーが環境変数名で、値が環境変数値のハッシュ テーブルを入力します。

例: `EnvironmentVariables=@{TestShare='\\Server01\TestShare'}`

```yaml
Type: System.Collections.IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ExecutionPolicy

セッション構成を使用するセッションの実行ポリシーを指定します。 このパラメーターを省略すると、セッション構成ファイルの **set-executionpolicy** キーの値が **制限** されます。 PowerShell の実行ポリシーの詳細については、「 [about_Execution_Policies](about/about_Execution_Policies.md)」を参照してください。

```yaml
Type: Microsoft.PowerShell.ExecutionPolicy
Parameter Sets: (All)
Aliases:
Accepted values: Unrestricted, RemoteSigned, AllSigned, Restricted, Default, Bypass, Undefined

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FormatsToProcess

セッション構成を使用するセッションで実行する書式設定ファイル (.ps1xml) を指定します。
このパラメーターの値は、書式設定ファイルの完全パスまたは絶対パスである必要があります。

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

### -Full

この操作に、セッション構成ファイルで使用可能なすべての構成プロパティが含まれていることを示します。

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

### -FunctionDefinitions

セッション構成を使用するセッションに、指定された関数を追加します。 ハッシュ テーブルに次のキーを入力します。

- 名前-関数の名前。 このキーは必須です。
- ScriptBlock-関数本体。 スクリプト ブロックを入力します。 このキーは必須です。
- オプション-関数のオプション。 このキーはオプションです。 既定値は **None** です。 このパラメーターに指定できる値は、None、ReadOnly、Constant、Private、または AllScope です。

例: `@{Name='Get-PowerShellProcess';ScriptBlock={Get-Process PowerShell};Options='AllScope'}`

```yaml
Type: System.Collections.IDictionary[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -GroupManagedServiceAccount

このセッション構成を使用して、指定したグループの管理されたサービスアカウントのコンテキストで実行するセッションを構成します。 セッション構成が登録されているコンピューターには、セッションが正常に作成されるためには、gMSA パスワードを要求するアクセス許可が必要です。 このフィールドは、 **RunAsVirtualAccount** パラメーターと共に使用することはできません。

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

### -Guid

セッション構成ファイルの一意の識別子を指定します。 このパラメーターを省略すると、に `New-PSSessionConfigurationFile` よってファイルの GUID が生成されます。 PowerShell で新しい GUID を作成するには、「」と入力 `New-Guid` します。

```yaml
Type: System.Guid
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LanguageMode

このセッション構成を使用するセッションで許可される PowerShell 言語の要素を決定します。 このパラメーターを使用して、特定のユーザーがコンピューター上で実行できるコマンドを制限できます。

このパラメーターの有効値は、次のとおりです。

- FullLanguage-すべての言語要素が許可されます。
- ConstrainedLanguage-評価対象のスクリプトが含まれているコマンドは使用できません。 ConstrainedLanguage モードは、ユーザーのアクセスを Microsoft .NET Framework の型、オブジェクト、またはメソッドに制限します。
- NoLanguage-ユーザーはコマンドレットと関数を実行できますが、スクリプトブロック、変数、または演算子などの言語要素の使用は許可されていません。
- RestrictedLanguage-ユーザーはコマンドレットと関数を実行できますが、、、 `$PSCulture` `$PSUICulture` `$True` 、 `$False` 、およびの各変数を除き、スクリプトブロックや変数の使用は許可されていません `$Null` 。 ユーザーは、基本的な比較演算子 (、、) のみを使用でき `-eq` `-gt` `-lt` ます。 代入ステートメント、プロパティ参照、およびメソッド呼び出しは許可されません。

**LanguageMode** パラメーターの既定値は、 **SessionType** パラメーターの値によって異なります。

- 空-NoLanguage
- RestrictedRemoteServer-NoLanguage
- 既定値-FullLanguage

```yaml
Type: System.Management.Automation.PSLanguageMode
Parameter Sets: (All)
Aliases:
Accepted values: FullLanguage, RestrictedLanguage, NoLanguage, ConstrainedLanguage

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModulesToImport

セッション構成を使用するセッションに自動的にインポートされたモジュールおよびスナップインを指定します。

既定では、リモートセッションにインポートされるのは、 **Microsoft の PowerShell** のみですが、コマンドレットが除外されていない限り、ユーザーはコマンドレットとコマンドレットを使用して、 `Import-Module` `Add-PSSnapin` モジュールとスナップインをセッションに追加できます。

このパラメーターの値の各モジュールまたはスナップインを、文字列によって、またはハッシュ テーブルとして表すことができます。 モジュール文字列は、モジュールまたはスナップインの名前のみで構成されています。 モジュールハッシュテーブルには、 **ModuleName** 、 **ModuleVersion** 、および **GUID** キーを含めることができます。 **ModuleName** キーのみが必須です。

たとえば、次の値は文字列とハッシュ テーブルで構成されます。 任意の順序での文字列とハッシュ テーブルの組み合わせは有効です。

`'TroubleshootingPack', @{ModuleName='PSDiagnostics'; ModuleVersion='1.0.0.0';GUID='c61d6278-02a3-4618-ae37-a524d40a7f44'}`

コマンドレットの **ModulesToImport** パラメーターの値は、 `Register-PSSessionConfiguration` セッション構成ファイルの **ModulesToImport** キーの値よりも優先されます。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MountUserDrive

このセッション構成を使用して PSDrive を公開するセッションを構成し `User:` ます。 ユーザードライブは、接続しているユーザーごとに一意であり、ファイルシステムプロバイダーが公開されていない場合でも、ユーザーは PowerShell エンドポイントとの間でデータのコピーを行うことができます。 ユーザードライブのルートは、の下に作成され `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\DriveRoots\` ます。 このエンドポイントに接続するユーザーには、それぞれ `$env:USERDOMAIN_$env:USERNAME` という名前のフォルダーが作成されます。

ユーザードライブの内容はユーザーセッション間で保持されるため、自動的には削除されません。 既定では、ユーザーは最大 50 MB のデータをユーザードライブに保存できます。 これは、 **UserDriveMaximumSize** パラメーターを使用してカスタマイズできます。

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

### -Path

セッション構成ファイルのパスとファイル名を指定します。 ファイルには `.pssc` ファイル名拡張子が必要です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PowerShellVersion

セッション構成を使用するセッションの PowerShell エンジンのバージョンを指定します。 このパラメーターに指定できる値は、2.0 と3.0 です。 このパラメーターを省略すると、 **powershellversion** キーがコメントアウトされ、最新バージョンの PowerShell がセッションで実行されます。

コマンドレットの **psversion** パラメーターの値は、 `Register-PSSessionConfiguration` セッション構成ファイルの **powershellversion** キーの値よりも優先されます。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredGroups

このセッション構成を使用するセッションに接続するユーザーの条件付きアクセス規則を指定します。

ハッシュテーブル (' および ' または ') ごとに1つのキーのみを使用してルールの一覧を作成するためのハッシュテーブルを入力し、値をセキュリティグループ名または追加のハッシュテーブルの配列に設定します。

1つのグループのメンバーとしてユーザーを接続する必要がある例: `@{ And = 'MyRequiredGroup' }`

エンドポイントにアクセスするために、ユーザーがグループ A またはグループ B と C の両方に属している必要がある例を次に示します。 `@{ Or = 'GroupA', @{ And = 'GroupB', 'GroupC' } }`

```yaml
Type: System.Collections.IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RoleDefinitions

セキュリティグループ (またはユーザー) とロール機能の間のマッピングを指定します。 ユーザーには、セッションの作成時にグループメンバーシップに適用されるすべてのロール機能へのアクセスが許可されます。

キーがセキュリティグループの名前で、値がセキュリティグループで使用できるようにする必要があるロール機能の一覧を含むハッシュテーブルであるハッシュテーブルを入力します。

例: `@{'Contoso\Level 2 Helpdesk Users' = @{ RoleCapabilities = 'Maintenance', 'ADHelpDesk' }}`

```yaml
Type: System.Collections.IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunAsVirtualAccount

このセッション構成を使用して、コンピューターの (仮想) 管理者アカウントとして実行されるセッションを構成します。 このフィールドは、 **Groupmanagedserviceaccount** パラメーターと共に使用することはできません。

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

### -RunAsVirtualAccountGroups

セッション構成を使用するセッションが仮想アカウントとして実行されるときに、仮想アカウントに関連付けるセキュリティグループを指定します。 省略した場合、仮想アカウントは、他のすべてのコンピューター上のドメインコントローラーおよび管理者の Domain Admins に属しています。

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

### -SchemaVersion

セッション構成ファイル スキーマのバージョンを指定します。 既定値は "1.0.0.0" です。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptsToProcess

セッション構成を使用するセッションに、指定されたスクリプトを追加します。 スクリプトのパスとファイル名を入力します。 このパラメーターの値には、スクリプトファイル名の完全パスまたは絶対パスを指定する必要があります。

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

### -SessionType

セッション構成を使用して作成されるセッションの型を指定します。 既定値は Default です。 このパラメーターの有効値は、次のとおりです。

- Empty-既定では、モジュールはセッションに追加されません。 このコマンドレットのパラメーターを使用して、モジュール、関数、スクリプト、およびその他の機能をセッションに追加できます。 このオプションは、選択したコマンドを追加することによってカスタムセッションを作成するために用意されています。 空のセッションにコマンドを追加しないと、セッションは式に制限され、使用できない場合があります。
- 既定-セッションに対して、PowerShell のコアモジュールを追加します。 このモジュールには、 `Import-Module` このコマンドレットを明示的に禁止していない限り、ユーザーが他のモジュールをインポートするために使用できるコマンドレットが含まれています。
- RestrictedRemoteServer. には、、、、、 `Exit-PSSession` 、 `Get-Command` `Get-FormatData` `Get-Help` `Measure-Object` `Out-Default` 、および `Select-Object` の各プロキシ関数のみが含まれています。
  このコマンドレットのパラメーターを使用して、モジュール、関数、スクリプト、およびその他の機能をセッションに追加できます。

```yaml
Type: System.Management.Automation.Remoting.SessionType
Parameter Sets: (All)
Aliases:
Accepted values: Empty, RestrictedRemoteServer, Default

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TranscriptDirectory

このセッション構成を使用しているセッションのセッションのトランスクリプトを配置するディレクトリを指定します。

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

### -TypesToProcess

`.ps1xml`セッション構成を使用するセッションに、指定された種類のファイルを追加します。 種類のファイル名を入力します。 このパラメーターの値には、型のファイル名への完全パスまたは絶対パスを指定する必要があります。

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

### -UserDriveMaximumSize

このセッション構成を使用するセッションで公開されているユーザードライブの最大サイズを指定します。
省略した場合、各ドライブルートの既定のサイズ `User:` は 50 mb です。

このパラメーターは **MountUserDrive** と共に使用する必要があります。

```yaml
Type: System.Int64
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -変数の定義

セッション構成を使用するセッションに、指定された変数を追加します。 ハッシュ テーブルに次のキーを入力します。

- 名前-変数の名前。 このキーは必須です。
- 値-変数の値。 このキーは必須です。
- オプション-変数オプション。 このキーはオプションです。 既定値は **None** です。 このパラメーターに指定できる値は、None、ReadOnly、Constant、Private、または AllScope です。

例: `@{Name='WarningPreference';Value='SilentlyContinue';Options='AllScope'}`

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -VisibleAliases

セッションのエイリアスを、このパラメーターの値に指定されたエイリアスと、 **AliasDefinition** パラメーターに定義するエイリアスに制限します。 ワイルドカード文字がサポートされています。 既定では、PowerShell エンジンによって定義されたすべてのエイリアスとモジュールがエクスポートするすべてのエイリアスがセッションで表示されます。

例: `VisibleAliases='gcm', 'gp'`

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって、 `Import-Module` コマンドレットとその ipmo エイリアスがセッションから削除されます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -VisibleCmdlets

セッションのコマンドレットを、このパラメーターの値に指定されたコマンドレットに制限します。 ワイルドカード文字とモジュール修飾名がサポートされています。

既定では、セッション内のモジュールがエクスポートするすべてのコマンドレットが、セッションで表示されます。 **SessionType** および **ModulesToImport** パラメーターを使用して、どのモジュールとスナップインをセッションにインポートするかを決定します。 **ModulesToImport** 内のモジュールがコマンドレットを公開していない場合、適切なモジュールは自動読み込みを試行します。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって、 `Import-Module` コマンドレットとその ipmo エイリアスがセッションから削除されます。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -VisibleExternalCommands

セッションで実行できる外部バイナリ、スクリプト、およびコマンドを、このパラメーターの値で指定されているものに制限します。 ワイルドカード文字がサポートされています。

既定では、セッションに外部コマンドは表示されません。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって、 `Import-Module` セッションからコマンドレットとその ipmo エイリアスが削除されます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -VisibleFunctions

セッションの関数を、このパラメーターの値に指定された関数と、 **FunctionDefinition** パラメーターに定義する関数に制限します。 ワイルドカード文字がサポートされています。

既定では、セッション内のモジュールがエクスポートするすべての関数が、セッションで表示されます。 **SessionType** および **ModulesToImport** パラメーターを使用して、どのモジュールとスナップインをセッションにインポートするかを決定します。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって、 `Import-Module` コマンドレットとその ipmo エイリアスがセッションから削除されます。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -VisibleProviders

セッション内の PowerShell プロバイダーを、このパラメーターの値に指定されているものに制限します。
ワイルドカード文字がサポートされています。

既定では、セッション内のモジュールがエクスポートするすべてのプロバイダーが、セッションで表示されます。 セッションにインポートするモジュールを特定するには、 **Sessiontype** パラメーターと **ModulesToImport** パラメーターを使用します。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって `Import-Module` コマンドレットとそのエイリアスがセッションから削除され `ipmo` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットにオブジェクトを送ることはできません。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- **VisibleCmdlets** や **VisibleProviders** などのパラメーターでは、セッションに項目がインポートされません。 代わりに、セッションにインポートされた項目の中から選択します。 たとえば、VisibleProviders パラメーターの値が証明書プロバイダーであるにもかかわらず、 **ModulesToImport** パラメーターで証明書プロバイダーが含まれる **VisibleProviders** モジュールが指定されていない場合、証明書プロバイダーはセッションに表示されませ **ん。**
- `New-PSSessionConfigurationFile`**path** パラメーターで指定したパスに .pssc ファイル名拡張子を持つセッション構成ファイルを作成します。 セッション構成ファイルを使用してセッション構成を作成すると、このコマンドレットによって `Register-PSSessionConfiguration` 構成ファイルがコピーされ、ファイルのアクティブなコピーがディレクトリの **sessionconfig** サブディレクトリに保存され `$PSHOME` ます。

  セッション構成の **Configfilepath** プロパティには、アクティブなセッション構成ファイルの完全修飾パスが含まれています。 任意の `$PSHOME` テキストエディターを使用して、ディレクトリ内のアクティブな構成ファイルをいつでも変更できます。 加えた変更は、既存のセッションではなく、セッション構成を使用するすべての新しいセッションに影響します。

  編集されたセッション構成ファイルを使用する前に、コマンドレットを使用して、 `Test-PSSessionConfigurationFile` 構成ファイルのエントリが有効であることを確認します。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Test-PSSessionConfigurationFile](Test-PSSessionConfigurationFile.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)
