---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 05/20/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-command?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Command
ms.openlocfilehash: 1d54082ee313c0e8d4ee7911f89da150aeba9d55
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601802"
---
# Get-Command

## 概要
すべてのコマンドを取得します。

## SYNTAX

### 設定します (既定)。

```
Get-Command [-Verb <String[]>] [-Noun <String[]>] [-Module <String[]>]
 [-FullyQualifiedModule <ModuleSpecification[]>] [-TotalCount <Int32>] [-Syntax] [-ShowCommandInfo]
 [[-ArgumentList] <Object[]>] [-All] [-ListImported] [-ParameterName <String[]>]
 [-ParameterType <PSTypeName[]>] [<CommonParameters>]
```

### AllCommandSet

```
Get-Command [[-Name] <String[]>] [-Module <String[]>]
 [-FullyQualifiedModule <ModuleSpecification[]>] [-CommandType <CommandTypes>] [-TotalCount <Int32>]
 [-Syntax] [-ShowCommandInfo] [[-ArgumentList] <Object[]>] [-All] [-ListImported]
 [-ParameterName <String[]>] [-ParameterType <PSTypeName[]>] [-UseFuzzyMatching]
 [-UseAbbreviationExpansion] [<CommonParameters>]
```

## Description

コマンドレットは、コマンド `Get-Command` レット、エイリアス、関数、フィルター、スクリプト、アプリケーションなど、コンピューターにインストールされているすべてのコマンドを取得します。 `Get-Command` 他のセッションからインポートされた PowerShell モジュールとコマンドからコマンドを取得します。 現在のセッションにインポートされているコマンドのみを取得するには、**ListImported** パラメーターを使用します。

パラメーターを指定しない場合、 `Get-Command` コンピューターにインストールされているすべてのコマンドレット、関数、およびエイリアスを取得します。 `Get-Command *` Path 環境変数 () 内のすべての非 PowerShell ファイルを含むすべての種類のコマンドを取得し `$env:Path` ます。この変数は、アプリケーションコマンドの種類で一覧表示されます。

`Get-Command` ワイルドカード文字を使用せずにコマンドの正確な名前を使用すると、コマンドを含むモジュールが自動的にインポートされ、コマンドをすぐに使用できるようになります。 モジュールの自動インポートを有効または無効にして構成するには、ユーザー設定変数を使用し `$PSModuleAutoLoadingPreference` ます。 詳細については、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

`Get-Command``Get-Help`ヘルプトピックから情報を取得するとは異なり、コマンドコードから直接データを取得します。

Windows PowerShell 5.0 以降では、コマンドレットの結果に `Get-Command` より、既定で **バージョン** 列が表示されます。 新しい **Version** プロパティが **commandinfo** クラスに追加されました。

## 例

### 例 1: コマンドレット、関数、およびエイリアスを取得する

このコマンドは、コンピューターにインストールされている PowerShell コマンドレット、関数、およびエイリアスを取得します。

```powershell
Get-Command
```

### 例 2: 現在のセッションでコマンドを取得する

このコマンドは、**ListImported** パラメーターを使用して、現在のセッション内のコマンドのみを取得します。

```powershell
Get-Command -ListImported
```

### 例 3: コマンドレットを取得し、順番に表示する

このコマンドは、すべてのコマンドレットを取得し、それらをコマンドレット名内の名詞によるアルファベット順で並べ替え、その後、名詞に基づくグループとして表示します。 この表示は、タスクのコマンドレットの検索に役立ちます。

```powershell
Get-Command -Type Cmdlet | Sort-Object -Property Noun | Format-Table -GroupBy Noun
```

### 例 4: モジュール内のコマンドを取得する

このコマンドは、 **Module** パラメーターを使用して、Microsoft. Powershell および Microsoft. Powershell. Utility モジュール内のコマンドを取得します。

```powershell
Get-Command -Module Microsoft.PowerShell.Security, Microsoft.PowerShell.Utility
```

### 例 5: コマンドレットに関する情報を取得する

このコマンドは、コマンドレットに関する情報を取得し `Get-AppLockerPolicy` ます。 また、**AppLocker** モジュールをインポートして、**AppLocker** モジュール内のすべてのコマンドを現在のセッションに追加します。

```powershell
Get-Command Get-AppLockerPolicy
```

モジュールが自動的にインポートされる場合、結果は Import-Module コマンドレットを使用した場合と同じになります。
モジュールは、コマンド、型、および書式設定ファイルを追加し、セッションでスクリプトを実行できます。 モジュールの自動インポートを有効化、無効化、および構成するには、ユーザー設定変数を使用し `$PSModuleAutoLoadingPreference` ます。 詳細については、「 [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

### 例 6: コマンドレットの構文を取得する

このコマンドは、 **ArgumentList** パラメーターと **構文** パラメーターを使用して、 `Get-ChildItem` Cert: ドライブで使用されているときにコマンドレットの構文を取得します。 Cert: ドライブは、証明書プロバイダーによってセッションに追加される PowerShell ドライブです。

```powershell
Get-Command  -Name Get-Childitem -Args Cert: -Syntax
```

出力に表示される構文と、 **Args** (**ArgumentList**) パラメーターを省略したときに表示される構文を比較すると、 **証明書プロバイダー** によって動的パラメーター **codesigningcert** がコマンドレットに追加されることがわかります `Get-ChildItem` 。

証明書プロバイダーの詳細については、「 [about_Certificate_Provider](../Microsoft.PowerShell.Security/About/about_Certificate_Provider.md)」を参照してください。

### 例 7: 動的パラメーターを取得する

この例のコマンドは、関数を使用して、 `Get-DynamicParameters` 証明書プロバイダーが `Get-ChildItem` Cert: ドライブで使用されているときにコマンドレットに追加する動的パラメーターを取得します。

```powershell
function Get-DynamicParameters
{
    param ($Cmdlet, $PSDrive)
    (Get-Command -Name $Cmdlet -ArgumentList $PSDrive).ParameterSets | ForEach-Object {$_.Parameters} | Where-Object { $_.IsDynamic } | Select-Object -Property Name -Unique
}
Get-DynamicParameters -Cmdlet Get-ChildItem -PSDrive Cert:
```

```Output
Name
----
CodeSigningCert
```

`Get-DynamicParameters`この例の関数は、コマンドレットの動的パラメーターを取得します。 これは、前の例で使用されたメソッドの代替です。 動的パラメーターは、別のコマンドレットまたはプロバイダーによって、コマンドレットに追加できます。

### 例 8: すべての型のすべてのコマンドを取得する

このコマンドは、 **Path** 環境変数 () のパスにある実行可能ファイルを含め、ローカルコンピューター上のすべての種類のすべてのコマンドを取得し `$env:path` ます。

```powershell
Get-Command *
```

このコマンドは、**FileInfo** オブジェクト (System.IO.FileInfo) ではなく、各ファイルの **ApplicationInfo** オブジェクト (System.Management.Automation.ApplicationInfo) を返します。

### 例 9: パラメーター名と型を使用してコマンドレットを取得する

このコマンドは、名前に Auth を含むパラメーターがあり、その型が **Authenticationmechanism** であるコマンドレットを取得します。

```powershell
Get-Command -ParameterName *Auth* -ParameterType AuthenticationMechanism
```

ユーザーの認証に使用されるメソッドを指定するためのコマンドレットを検索するために、次のようなコマンドを使用できます。

**ParameterType** パラメーターは、パラメーターが同じ名前の場合でも、**AuthenticationMechanism** 値を受け取るパラメーターと、**AuthenticationLevel** パラメーターを受け取るパラメーターを区別します。

### 例 10: エイリアスを取得する

この例では、 `Get-Command` コマンドレットをエイリアスと共に使用する方法を示します。

```powershell
Get-Command Name dir
```

```Output
CommandType     Name                                               ModuleName
-----------     ----                                               ----------
Alias           dir -> Get-ChildItem
```

通常はコマンドレットと関数で使用され `Get-Command` ますが、スクリプト、関数、エイリアス、および実行可能ファイルも取得します。

コマンドの出力は、エイリアスの **Name** プロパティの値の特別な表示を示しています。 この表示は、エイリアスとコマンドのフルネームを示しています。

### 例 11: 別名から構文を取得する

この例では、エイリアスの標準名と共に構文を取得する方法を示します。

このコマンドの出力では、標準名のラベルが付いたエイリアスと、その後に構文が表示されます。

```powershell
Get-Command -Name dir -Syntax
```

```Output
dir (alias) -> Get-ChildItem

dir [[-Path] <string[]>] [[-Filter] <string>] [-Include <string[]>] [-Exclude <string[]>] [-Recurse] [-Depth <uint>] [-Force] [-Name] [-Attributes <FlagsExpression[FileAttributes]>] [-FollowSymlink] [-Directory] [-File] [-Hidden] [-ReadOnly] [-System] [<CommonParameters>]

dir [[-Filter] <string>] -LiteralPath <string[]> [-Include <string[]>] [-Exclude <string[]>] [-Recurse] [-Depth <uint>] [-Force] [-Name] [-Attributes <FlagsExpression[FileAttributes]>] [-FollowSymlink] [-Directory] [-File] [-Hidden] [-ReadOnly] [-System] [<CommonParameters>]
```

### 例 12: メモ帳コマンドのすべてのインスタンスを取得する

この例では、コマンドレットの **all** パラメーターを使用して、 `Get-Command` ローカルコンピューター上のコマンドのすべてのインスタンスを表示し `Notepad` ます。

```powershell
Get-Command Notepad -All | Format-Table CommandType, Name, Definition
```

```Output
CommandType     Name           Definition
-----------     ----           ----------
Application     notepad.exe    C:\WINDOWS\system32\notepad.exe
Application     NOTEPAD.EXE    C:\WINDOWS\NOTEPAD.EXE
```

**All** パラメーターは、セッションで同じ名前のコマンドが複数ある場合に役立ちます。

Windows PowerShell 3.0 以降では、セッションに同じ名前の複数のコマンドが含まれる場合、既定では、 `Get-Command` コマンド名を入力したときに実行されるコマンドのみが取得されます。 **All** パラメーターを使用して、指定した `Get-Command` 名前のすべてのコマンドを取得し、実行の優先順位で返します。 一覧で最初のコマンドではなく、他のコマンドを実行するには、コマンドへの完全修飾パスを入力します。

コマンドの優先順位の詳細については、「 [about_Command_Precedence](About/about_Command_Precedence.md)」を参照してください。

### 例 13: コマンドレットを含むモジュールの名前を取得する

このコマンドは、コマンドレットが生成されたモジュールの名前を取得し `Get-Date` ます。
コマンドは、すべてのコマンドの **ModuleName** プロパティを使用します。

```powershell
(Get-Command Get-Date).ModuleName
```

```Output
Microsoft.PowerShell.Utility
```

このコマンド形式は、セッションにインポートされていない場合でも、PowerShell モジュールのコマンドに対して機能します。

### 例 14: 出力の種類がのコマンドレットと関数を取得する

```powershell
Get-Command -Type Cmdlet | Where-Object OutputType | Format-List -Property Name, OutputType
```

このコマンドは、出力の種類とオブジェクトの種類を返す、コマンドレットと関数を取得します。

コマンドの最初の部分は、すべてのコマンドレットを取得します。 パイプライン演算子 () は、コマンドレットをコマンドレットに送信します。このコマンドレットは、 `|` `Where-Object` **OutputType** プロパティが設定されているコマンドレットだけを選択します。 もう1つのパイプライン演算子は、選択したコマンドレットオブジェクトをコマンドレットに送信します。このコマンドレットは、 `Format-List` リスト内の各コマンドレットの名前と出力の種類を表示します。

**CommandInfo** オブジェクトの **OutputType** プロパティは、コマンドレット コードがコマンドレットの **OutputType** 属性を定義する場合にのみ、NULL 以外の値を保持します。

### 例 15: 特定のオブジェクトの種類を入力として受け取るコマンドレットを取得する

```powershell
Get-Command -ParameterType (((Get-NetAdapter)[0]).PSTypeNames)
```

```Output
CommandType     Name                                               ModuleName
-----------     ----                                               ----------
Function        Disable-NetAdapter                                 NetAdapter
Function        Enable-NetAdapter                                  NetAdapter
Function        Rename-NetAdapter                                  NetAdapter
Function        Restart-NetAdapter                                 NetAdapter
Function        Set-NetAdapter                                     NetAdapter
```

このコマンドは、ネット アダプター オブジェクトを入力として受け取るコマンドレットを検索します。 このコマンド形式を使用して、いずれかのコマンドによって返されるオブジェクトの種類を受け取るコマンドレットを検索できます。

コマンドは、すべてのオブジェクトの **PSTypeNames** 組み込みプロパティを使用して、オブジェクトを記述する型を取得します。 ネット アダプターの **PSTypeNames** プロパティを取得し、ネット アダプターのコレクションの **PSTypeNames** プロパティを除外するために、コマンドは、配列表記を使用して、コマンドレットが返す最初のネット アダプターを取得します。

### 例 16: あいまい一致を使用してコマンドを取得する

この例では、コマンドの名前は、意図的に ' get-commnd ' として指定されています。
スイッチを使用すると `-UseFuzzyMatching` 、コマンドレットは、一致と見なさ `Get-Command` れたシステム上の他のネイティブコマンドが最も一致したものであると判断しました。

```powershell
Get-Command get-commnd -UseFuzzyMatching
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-Command                                        6.2.0.0    Microsoft.PowerShell.Core
Application     getconf                                            0.0.0.0    /usr/bin/getconf
Application     command                                            0.0.0.0    /usr/bin/command
```

## PARAMETERS

### -All

このコマンドレットが、同じ名前を持つ同じ種類のコマンドを含む、すべてのコマンドを取得することを示します。 既定では、 `Get-Command` コマンド名を入力したときに実行されるコマンドのみが取得されます。

複数のコマンドが同じ名前の場合に実行するコマンドを PowerShell で選択する方法の詳細については、「 [about_Command_Precedence](About/about_Command_Precedence.md)」を参照してください。
名前の競合が原因で既定で実行されないモジュール修飾コマンド名と実行中のコマンドの詳細については、「 [about_Modules](About/about_Modules.md)」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

Windows PowerShell 2.0 では、 `Get-Command` 既定ですべてのコマンドを取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ArgumentList

引数の配列を指定します。 このコマンドレットは、指定されたパラメーター ("arguments") と共に使用された場合に、コマンドレットまたは関数に関する情報を取得します。 **ArgumentList** のエイリアスは、**Args** です。

特定の他のパラメーターが使用されている場合にのみ使用可能な動的パラメーターを検出するには、 **ArgumentList** の値を動的パラメーターをトリガーするパラメーターに設定します。

プロバイダーがコマンドレットに追加する動的パラメーターを検出するには、 **ArgumentList** パラメーターの値を、WSMan:、HKLM:、Cert: などのプロバイダードライブのパスに設定します。 コマンドが PowerShell プロバイダーコマンドレットの場合は、各コマンドでパスを1つだけ入力します。 プロバイダーのコマンドレットは、 **ArgumentList** の値の最初のパスの動的パラメーターのみを返します。 プロバイダーコマンドレットの詳細については、「 [about_Providers](About/about_Providers.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases: Args

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CommandType

このコマンドレットが取得するコマンドの種類を指定します。 1 つまたは複数のコマンドの種類を入力します。 **CommandType** またはそのエイリアスの **Type** を使用します。 既定では、は `Get-Command` すべてのコマンドレット、関数、およびエイリアスを取得します。

このパラメーターの有効値は、次のとおりです。

- エイリアス. すべての PowerShell コマンドのエイリアスを取得します。 詳細については、「 [about_Aliases](About/about_Aliases.md)」を参照してください。
- すべて。 すべてのコマンドの種類を取得します。 このパラメーター値は、に相当し `Get-Command *` ます。
- アプリケーション をクリックします。 .Txt、.exe、.dll ファイルなど、 **Path** 環境変数 ($env:p a) に一覧表示されているパス内の PowerShell 以外のファイルを取得します。 **Path** 環境変数の詳細については、「about_Environment_Variables」を参照してください。
- コマンドレット. すべてのコマンドレットを取得します。
- ExternalScript。 **Path** 環境変数 ($env:path) に列挙されるパス内のすべての .ps1 ファイルを取得します。
- フィルターと関数。 すべての PowerShell advanced および simple 関数とフィルターを取得します。
- スクリプティング。 すべてのスクリプト ブロックを取得します。 PowerShell スクリプト (ps1 ファイル) を取得するには、ExternalScript 値を使用します。

```yaml
Type: System.Management.Automation.CommandTypes
Parameter Sets: AllCommandSet
Aliases: Type
Accepted values: Alias, Function, Filter, Cmdlet, ExternalScript, Application, Script, Workflow, Configuration, All

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -FullyQualifiedModule

**Modulの** 特定の [コンストラクター (Hashtable)](/dotnet/api/microsoft.powershell.commands.modulespecification.-ctor?view=powershellsdk-1.1.0#Microsoft_PowerShell_Commands_ModuleSpecification__ctor_System_Collections_Hashtable_)の **「解説**」で説明されている、名前付きモジュールを指定します。
たとえば、 **FullyQualifiedModule** パラメーターは、次のいずれかの形式で指定されたモジュール名を受け入れます。

- `@{ModuleName = "modulename"; ModuleVersion = "version_number"}`
- `@{ModuleName = "modulename"; ModuleVersion = "version_number"; Guid = "GUID"}`

**ModuleName** と **ModuleVersion** は必須ですが、**Guid** は省略可能です。

**モジュール** パラメーターと同じコマンドで **FullyQualifiedModule** パラメーターを指定することはできません。 2つのパラメーターは相互に排他的です。

```yaml
Type: Microsoft.PowerShell.Commands.ModuleSpecification[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ListImported

このコマンドレットが現在のセッションのコマンドのみを取得することを示します。

PowerShell 3.0 以降では、既定では、 `Get-Command` 現在のセッションのコマンドに限定されるのではなく、インストールされているすべてのコマンドを取得します。 PowerShell 2.0 では、現在のセッションのコマンドのみを取得します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -モジュール

モジュールの配列を指定します。 このコマンドレットは、指定されたモジュールからのコマンドを取得します。
モジュールまたはモジュールオブジェクトの名前を入力します。

このパラメーターは文字列値を受け取りますが、このパラメーターの値は、およびコマンドレットが返すオブジェクトなどの **PSModuleInfo** オブジェクトにすることもでき `Get-Module` `Import-PSSession` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PSSnapin

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Name

名前の配列を指定します。 このコマンドレットは、指定された名前のコマンドのみを取得します。 名前または名前パターンを入力します。 ワイルドカード文字を使用できます。

同じ名前のコマンドを取得するには、**All** パラメーターを使用します。 2つのコマンドの名前が同じ場合、既定では、 `Get-Command` コマンド名を入力したときに実行されるコマンドを取得します。

```yaml
Type: System.String[]
Parameter Sets: AllCommandSet
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -名詞

コマンドの名詞の配列を指定します。 このコマンドレットは、指定された名詞を含む名前を持つコマンドレット、関数、およびエイリアスを含むコマンドを取得します。 1 つまたは複数の名詞または名詞のパターンを入力します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: CmdletSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -ParameterName

パラメーター名の配列を指定します。 このコマンドレットは、指定されたパラメーターを持つセッション内のコマンドを取得します。 パラメーター名またはパラメーターの別名を入力します。 ワイルドカード文字がサポートされています。

**ParameterName** および **ParameterType**　パラメーターは、現在のセッションのコマンドのみを検索します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

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

### -ParameterType

パラメーター名の配列を指定します。 このコマンドレットは、指定された型のパラメーターを持つ、セッション内のコマンドを取得します。 パラメーターの型のフルネームまたは部分的な名前を入力します。 ワイルドカード文字がサポートされています。

**ParameterName** および **ParameterType**　パラメーターは、現在のセッションのコマンドのみを検索します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSTypeName[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ShowCommandInfo

このコマンドレットによってコマンド情報が表示されることを示します。

このパラメーターは、Windows PowerShell 5.0 で導入されました。

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

### -構文

このコマンドレットが、コマンドに関する次の指定されたデータのみを取得することを示します。

- エイリアス. 標準の名前と構文を取得します。
- レット. 構文を取得します。
- 関数とフィルター。 関数定義を取得します。
- スクリプト、アプリケーション、またはファイル。 パスとファイル名を取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -TotalCount

取得するコマンドの数を指定します。 このパラメーターを使用すると、コマンドの出力を制限できます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -UseAbbreviationExpansion

コマンド内の文字の照合を使用して、コマンド内の大文字で検索することを示します。 たとえば、は、 `i-psdf` `Import-PowerShellDataFile` 検索する各文字が結果の大文字と一致すると見なされます。 この種類の一致を使用すると、ワイルドカードの結果は一致しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AllCommandSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -UseFuzzyMatching

コマンドを検索するときに、あいまい一致アルゴリズムを使用することを示します。 出力の順序は、最も近い一致から最も一致しないものになります。 ワイルドカードは、ワイルドカード文字を含む可能性のあるコマンドを照合しようとするため、あいまい一致では使用しないでください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AllCommandSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -動詞

コマンド動詞の配列を指定します。 このコマンドレットは、指定された動詞を含む名前を持つコマンドレット、関数、およびエイリアスを含むコマンドを取得します。 1 つまたは複数の動詞または動詞パターンを入力します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: CmdletSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用してコマンド名をこのコマンドレットに渡します。

## 出力

### システムの管理. CommandInfo

このコマンドレットは、 **Commandinfo** クラスから派生したオブジェクトを返します。 返されるオブジェクトの型は、が取得するコマンドの型によって異なり `Get-Command` ます。

### システム管理. エイリアス情報

エイリアスを表します。

### システム管理. ApplicationInfo

アプリケーションとファイルを表します。

### システムの管理.......

コマンドレットを表します。

### システム管理. FunctionInfo

関数とフィルターを表します。

## 注

* セッションで同じ名前を持つ複数のコマンドを使用できる場合は、 `Get-Command` コマンド名を入力すると実行されるコマンドが返されます。 同じ名前を持つコマンドを実行順序で表示するには、 **All** パラメーターを使用します。 詳細については、「[about_Command_Precedence](../Microsoft.PowerShell.Core/About/about_Command_Precedence.md)」(コマンドの優先順位について) を参照してください。
* モジュールが自動的にインポートされる場合、結果はコマンドレットを使用した場合と同じになり `Import-Module` ます。 モジュールは、コマンド、型、および書式設定ファイルを追加し、セッションでスクリプトを実行できます。
  モジュールの自動インポートを有効化、無効化、および構成するには、ユーザー設定変数を使用し `$PSModuleAutoLoadingPreference` ます。 詳細については、「 [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

## 関連リンク

[Export-PSSession](../Microsoft.PowerShell.Utility/Export-PSSession.md)

[Get-Help](Get-Help.md)

[Get-Member](../Microsoft.PowerShell.Utility/Get-Member.md)

[Get-PSDrive](../Microsoft.PowerShell.Management/Get-PSDrive.md)

[Import-PSSession](../Microsoft.PowerShell.Utility/Import-PSSession.md)

[about_Command_Precedence](About/about_Command_Precedence.md)

