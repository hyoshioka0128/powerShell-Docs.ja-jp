---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 09/11/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-psrolecapabilityfile?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-PSRoleCapabilityFile
ms.openlocfilehash: 3ef54618e065a5fca3d52415897b3042d6ba4c65
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605464"
---
# New-PSRoleCapabilityFile

## 概要
セッション構成を通じて公開される一連の機能を定義するファイルを作成します。

## SYNTAX

```
New-PSRoleCapabilityFile [-Path] <String> [-Guid <Guid>] [-Author <String>] [-Description <String>]
 [-CompanyName <String>] [-Copyright <String>] [-ModulesToImport <Object[]>] [-VisibleAliases <String[]>]
 [-VisibleCmdlets <Object[]>] [-VisibleFunctions <Object[]>] [-VisibleExternalCommands <String[]>]
 [-VisibleProviders <String[]>] [-ScriptsToProcess <String[]>] [-AliasDefinitions <IDictionary[]>]
 [-FunctionDefinitions <IDictionary[]>] [-VariableDefinitions <Object>] [-EnvironmentVariables <IDictionary>]
 [-TypesToProcess <String[]>] [-FormatsToProcess <String[]>] [-AssembliesToLoad <String[]>]
 [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-PSRoleCapabilityFile` セッション構成ファイルを通じて公開できる一連のユーザー機能を定義するファイルを作成します。 これには、ユーザーが使用できるコマンドレット、関数、およびスクリプトの決定が含まれます。 機能ファイルは、セッション構成のプロパティと値のハッシュテーブルを含む、人間が判読できるテキストファイルです。 ファイルの拡張子は psrc で、複数のセッション構成で使用できます。

のすべてのパラメーター `New-PSRoleCapabilityFile` は省略可能です。 **path** パラメーターを除き、ファイルのファイルパスを指定します。 コマンドレットを実行するときにパラメーターを含めない場合、パラメーターの説明に示されている場合を除き、セッション構成ファイル内の対応するキーがコメントアウトされます。 たとえば、 **AssembliesToLoad** パラメーターを含めない場合、セッション構成ファイルのそのセクションはコメントアウトされます。

セッション構成でロール機能ファイルを使用するには、まず、有効な PowerShell モジュールフォルダーの **RoleCapabilities** サブフォルダーにファイルを配置します。 次に、PowerShell セッション構成 (.pssc) ファイルの **Roledefinitions** フィールドで、名前を指定してファイルを参照します。

このコマンドレットは、Windows PowerShell 5.0 で導入されました。

## 例

### 例 1: 空のロール機能ファイルを作成する

この例では、既定値 (空白) を使用する新しいロール機能ファイルを作成します。 後でこのファイルをテキストエディターで編集して、これらの構成設定を変更できます。

```powershell
New-PSRoleCapabilityFile -Path ".\ExampleFile.psrc"
```

### 例 2: ロール機能ファイルを作成し、ユーザーがサービスと VDI コンピューターを再起動できるようにする

この例では、ユーザーが特定の名前パターンに一致するサービスとコンピューターを再起動できるようにする、サンプルのロール機能ファイルを作成します。 名前のフィルター処理は、 **Validatepattern** パラメーターを正規表現に設定することによって定義され `VDI\d+` ます。

```powershell
$roleParameters = @{
    Path = ".\Maintenance.psrc"
    Author = "User01"
    CompanyName = "Fabrikam Corporation"
    Description = "This role enables users to restart any service and restart any VDI computer."
    ModulesToImport = "Microsoft.PowerShell.Core"
    VisibleCmdlets = "Restart-Service", @{
                      Name = "Restart-Computer"
                      Parameters = @{ Name = "ComputerName"; ValidatePattern = "VDI\d+" }
    }
}
New-PSRoleCapabilityFile @roleParameters
```

## PARAMETERS

### -エイリアスの定義

ロール機能ファイルを使用するセッションに、指定されたエイリアスを追加します。 ハッシュ テーブルに次のキーを入力します。

- 名前。 エイリアスの名前。 このキーは必須です。
- Value。 エイリアスが表すコマンド。 このキーは必須です。
- 説明 エイリアスを説明するテキスト文字列。 このキーはオプションです。
- オプション。 エイリアスのオプション。 このキーはオプションです。 既定値は None です。 このパラメーターに指定できる値は、None、ReadOnly、Constant、Private、または AllScope です。

例: `@{Name="hlp";Value="Get-Help";Description="Gets help";Options="ReadOnly"}`

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

ロール機能ファイルを使用するセッションに読み込むアセンブリを指定します。

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

ロール機能ファイルを作成したユーザーを指定します。

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

ロール機能ファイルを作成した会社を識別します。
既定値は Unknown です。

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

### -Copyright

ロール機能ファイルの著作権を指定します。 このパラメーターを省略すると、は `New-PSRoleCapabilityFile` **Author** パラメーターの値を使用して著作権情報ステートメントを生成します。

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

ロール機能ファイルの説明を指定します。

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

このロール機能ファイルを公開するセッションの環境変数を指定します。 キーが環境変数名で、値が環境変数値のハッシュ テーブルを入力します。

例: `EnvironmentVariables=@{TestShare="\\\\Server01\TestShare"}`

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

### -FormatsToProcess

ロール機能ファイルを使用するセッションで実行される書式設定ファイル (types.ps1xml) を指定します。 このパラメーターの値は、書式設定ファイルの完全パスまたは絶対パスである必要があります。

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

### -FunctionDefinitions

ロール機能を公開するセッションに、指定された関数を追加します。 ハッシュ テーブルに次のキーを入力します。

- 名前。 関数の名前です。 このキーは必須です。
- Process. 関数の本体。 スクリプト ブロックを入力します。 このキーは必須です。
- オプション。 関数のオプション。 このキーはオプションです。 既定値は None です。 このパラメーターに指定できる値は、None、ReadOnly、Constant、Private、または AllScope です。

次に例を示します。

`@{Name="Get-PowerShellProcess";ScriptBlock={Get-Process PowerShell};Options="AllScope"}`

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

### -Guid

ロール機能ファイルの一意の識別子を指定します。 このパラメーターを省略すると、に `New-PSRoleCapabilityFile` よってファイルの GUID が生成されます。 PowerShell で新しい GUID を作成するには、「」と入力 `[guid]::NewGuid()` します。

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

### -ModulesToImport

ロール機能ファイルを使用するセッションに自動的にインポートされるモジュールを指定します。 既定では、一覧表示されているモジュールのすべてのコマンドが表示されます。 **VisibleCmdlets** または **VisibleFunctions** と共に使用すると、指定したモジュールから表示されるコマンドを制限できます。

このパラメーターの値で使用される各モジュールは、文字列またはハッシュテーブルで表すことができます。 モジュール文字列は、モジュールの名前のみで構成されています。 モジュールハッシュテーブルには、 **ModuleName**、 **ModuleVersion**、および **GUID** キーを含めることができます。 **ModuleName** キーのみが必須です。

たとえば、次の値は文字列とハッシュ テーブルで構成されます。 任意の順序での文字列とハッシュ テーブルの組み合わせは有効です。

`"TroubleshootingPack", @{ModuleName="PSDiagnostics"; ModuleVersion="1.0.0.0";GUID="c61d6278-02a3-4618-ae37-a524d40a7f44"}`

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

### -Path

ロール機能ファイルのパスとファイル名を指定します。 ファイルにはファイル `.psrc` 名拡張子が必要です。

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

### -ScriptsToProcess

ロール機能ファイルを使用するセッションに追加するスクリプトを指定します。 スクリプトのパスとファイル名を入力します。 このパラメーターの値には、スクリプトファイル名の完全パスまたは絶対パスを指定する必要があります。

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

### -TypesToProcess

ロール機能ファイルを使用するセッションに追加する型ファイル (types.ps1xml) を指定します。 種類のファイル名を入力します。 このパラメーターの値には、型のファイル名の完全パスまたは絶対パスを指定する必要があります。

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

### -変数の定義

ロール機能ファイルを使用するセッションに追加する変数を指定します。 ハッシュ テーブルに次のキーを入力します。

- 名前。 変数名。 このキーは必須です。
- Value。 変数の値。 このキーは必須です。
- オプション。 変数のオプション。 このキーはオプションです。 既定値は None です。 このパラメーターに指定できる値は、None、ReadOnly、Constant、Private、または AllScope です。

例: `@{Name="WarningPreference";Value="SilentlyContinue";Options="AllScope"}`

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

セッションのエイリアスを、このパラメーターの値に指定されたエイリアスと、エイリアス **定義** パラメーターで定義した別名に限定します。 ワイルドカード文字がサポートされています。
既定では、PowerShell エンジンによって定義されたすべてのエイリアスとモジュールがエクスポートするすべてのエイリアスがセッションで表示されます。

たとえば、使用可能なエイリアスを gm と gcm に限定するには、次の構文を使用します。 `VisibleAliases="gcm", "gp"`

ロール機能ファイルに **表示** されるパラメーターが含まれている場合、PowerShell は `Import-Module` コマンドレットとその `ipmo` エイリアスをセッションから削除します。

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

既定では、セッション内のモジュールがエクスポートするすべてのコマンドレットが、セッションで表示されます。 **SessionType** および **ModulesToImport** パラメーターを使用して、どのモジュールとスナップインをセッションにインポートするかを決定します。 **ModulesToImport** 内のモジュールがコマンドレットを公開していない場合、は `New-PSRoleCapabilityFile` 適切なモジュールの読み込みを試みます。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって `Import-Module` コマンドレットとそのエイリアスがセッションから削除され `ipmo` ます。

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

### -VisibleExternalCommands

セッションで実行できる外部バイナリ、スクリプト、およびコマンドを、このパラメーターの値で指定されているものに制限します。

既定では、このセッションでは外部コマンドは表示されません。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって `Import-Module` コマンドレットとそのエイリアスがセッションから削除され `ipmo` ます。

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

### -VisibleFunctions

セッションの関数を、このパラメーターの値に指定された関数と、 **Functiondefinitions** パラメーターで定義したすべての関数に制限します。 ワイルドカード文字がサポートされています。

既定では、セッションのモジュールによってエクスポートされたすべての関数は、そのセッションで表示されます。 セッションにインポートするモジュールを特定するには、 **Sessiontype** パラメーターと **ModulesToImport** パラメーターを使用します。

セッション構成ファイルに **表示** されているパラメーターがある場合は、PowerShell によって `Import-Module` コマンドレットとそのエイリアスがセッションから削除され `ipmo` ます。

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

既定では、セッションのモジュールによってエクスポートされたすべてのプロバイダーがセッションで表示されます。 セッションにインポートするモジュールを特定するには、 **Sessiontype** パラメーターと **ModulesToImport** パラメーターを使用します。

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

## 出力

## 注

## 関連リンク

[New-PSSessionConfigurationFile](New-PSSessionConfigurationFile.md)

[Get-PSSessionCapability](Get-PSSessionCapability.md)

