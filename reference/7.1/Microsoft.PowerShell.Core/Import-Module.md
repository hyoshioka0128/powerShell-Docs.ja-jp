---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 12/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-Module
ms.openlocfilehash: d041d63c7af185e62a902ebaa362809cddfb4659
ms.sourcegitcommit: 7b376314e7640c39a53aac9f0db8bb935514a960
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2020
ms.locfileid: "96564580"
---
# Import-Module

## 概要
現在のセッションにモジュールを追加します。

## SYNTAX

### 名前 (既定値)

```
Import-Module [-Global] [-Prefix <String>] [-Name] <String[]> [-Function <String[]>]
 [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>] [-Force] [-SkipEditionCheck]
 [-PassThru] [-AsCustomObject] [-MinimumVersion <Version>] [-MaximumVersion <String>]
 [-RequiredVersion <Version>] [-ArgumentList <Object[]>] [-DisableNameChecking] [-NoClobber]
 [-Scope <String>]  [<CommonParameters>]
```

### PSSession

```
Import-Module [-Global] [-Prefix <String>] [-Name] <String[]> [-Function <String[]>]
 [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>] [-Force] [-SkipEditionCheck]
 [-PassThru] [-AsCustomObject] [-MinimumVersion <Version>] [-MaximumVersion <String>]
 [-RequiredVersion <Version>] [-ArgumentList <Object[]>] [-DisableNameChecking] [-NoClobber]
 [-Scope <String>] -PSSession <PSSession>  [<CommonParameters>]
```

### CimSession

```
Import-Module [-Global] [-Prefix <String>] [-Name] <String[]> [-Function <String[]>]
 [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>] [-Force] [-SkipEditionCheck]
 [-PassThru] [-AsCustomObject] [-MinimumVersion <Version>] [-MaximumVersion <String>]
 [-RequiredVersion <Version>] [-ArgumentList <Object[]>] [-DisableNameChecking] [-NoClobber]
 [-Scope <String>] -CimSession <CimSession> [-CimResourceUri <Uri>] [-CimNamespace <String>]
 [<CommonParameters>]
```

### UseWindowsPowerShell

```
Import-Module [-Name] <string[]> -UseWindowsPowerShell [-Global] [-Prefix <string>]
 [-Function <string[]>] [-Cmdlet <string[]>] [-Variable <string[]>] [-Alias <string[]>]
 [-Force] [-PassThru] [-AsCustomObject] [-MinimumVersion <version>] [-MaximumVersion <string>]
 [-RequiredVersion <version>] [-ArgumentList <Object[]>] [-DisableNameChecking] [-NoClobber]
 [-Scope <string>] [<CommonParameters>]
```

### FullyQualifiedName

```
Import-Module [-Global] [-Prefix <String>] [-FullyQualifiedName] <ModuleSpecification[]>
 [-Function <String[]>] [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>] [-Force]
 [-SkipEditionCheck] [-PassThru] [-AsCustomObject] [-ArgumentList <Object[]>] [-DisableNameChecking]
 [-NoClobber] [-Scope <String>]  [<CommonParameters>]
```

### FullyQualifiedNameAndPSSession

```
Import-Module [-Global] [-Prefix <String>] [-FullyQualifiedName] <ModuleSpecification[]>
 [-Function <String[]>] [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>] [-Force]
 [-SkipEditionCheck] [-PassThru] [-AsCustomObject] [-ArgumentList <Object[]>] [-DisableNameChecking]
 [-NoClobber] [-Scope <String>] -PSSession <PSSession>  [<CommonParameters>]
```

### FullyQualifiedNameAndUseWindowsPowerShell

```
Import-Module [-FullyQualifiedName] <ModuleSpecification[]> -UseWindowsPowerShell [-Global]
 [-Prefix <string>] [-Function <string[]>] [-Cmdlet <string[]>] [-Variable <string[]>]
 [-Alias <string[]>] [-Force] [-PassThru] [-AsCustomObject] [-ArgumentList <Object[]>]
 [-DisableNameChecking] [-NoClobber] [-Scope <string>] [<CommonParameters>]
```

### アセンブリ

```
Import-Module [-Global] [-Prefix <String>] [-Assembly] <Assembly[]> [-Function <String[]>]
 [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>] [-Force] [-SkipEditionCheck]
 [-PassThru] [-AsCustomObject] [-ArgumentList <Object[]>] [-DisableNameChecking] [-NoClobber]
 [-Scope <String>]  [<CommonParameters>]
```

### ModuleInfo

```
Import-Module [-Global] [-Prefix <String>] [-Function <String[]>] [-Cmdlet <String[]>]
 [-Variable <String[]>] [-Alias <String[]>] [-Force] [-SkipEditionCheck] [-PassThru]
 [-AsCustomObject] [-ModuleInfo] <PSModuleInfo[]> [-ArgumentList <Object[]>] [-DisableNameChecking]
 [-NoClobber] [-Scope <String>]  [<CommonParameters>]
```

## Description

`Import-Module`コマンドレットでは、現在のセッションに1つ以上のモジュールを追加します。 PowerShell 3.0 以降では、モジュール内のコマンドまたはプロバイダーを使用すると、インストールされているモジュールが自動的にセッションにインポートされます。 ただし、このコマンドを使用してモジュールをインポートすることもでき `Import-Module` ます。
自動モジュールインポートは、ユーザー設定変数を使用して無効にすることができ `$PSModuleAutoloadingPreference` ます。 変数の詳細については `$PSModuleAutoloadingPreference` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

モジュールは、PowerShell で使用できるメンバーを含むパッケージです。 メンバーには、コマンドレット、プロバイダー、スクリプト、関数、変数、およびその他のツールとファイルが含まれます。 モジュールをインポートすると、モジュールのメンバーをセッションで使用できます。 モジュールの詳細については、「 [about_Modules](About/about_Modules.md)」を参照してください。

既定では、は、モジュールによっ `Import-Module` てエクスポートされるすべてのメンバーをインポートしますが、 **エイリアス**、 **関数**、 **コマンドレット**、および **変数** パラメーターを使用して、インポートするメンバーを制限することができます。 **NoClobber** パラメーターは、 `Import-Module` 現在のセッションのメンバーと同じ名前を持つメンバーをでインポートできないようにします。

`Import-Module` 現在のセッションにのみモジュールをインポートします。 すべての新しいセッションにモジュールをインポートするには、 `Import-Module` PowerShell プロファイルにコマンドを追加します。 プロファイルの詳細については、「[about_Profiles](About/about_Profiles.md)」を参照してください。

リモートコンピューター上に **PSSession** を作成することによって、PowerShell リモート処理が有効になっているリモート Windows コンピューターを管理できます。 次に、の **PSSession** パラメーター `Import-Module` を使用して、リモートコンピューターにインストールされているモジュールをインポートします。 現在のセッションでインポートされたコマンドを使用すると、コマンドはリモートコンピューター上で暗黙的に実行されます。

Windows PowerShell 3.0 以降では、を使用して `Import-Module` Common Information Model (CIM) モジュールをインポートできます。 CIM モジュールは、コマンドレット定義 XML (CDXML) ファイルのコマンドレットを定義します。 この機能を使用すると、C++ で記述されたものなど、マネージコード以外のアセンブリに実装されているコマンドレットを使用できます。

Windows オペレーティングシステムを実行していないコンピューターを含め、PowerShell リモート処理が有効になっていないリモートコンピューターの場合は、の **CIMSession** パラメーターを使用して、 `Import-Module` リモートコンピューターから CIM モジュールをインポートできます。 インポートされたコマンドは、リモートコンピューター上で暗黙的に実行されます。 **CIMSession** は、リモートコンピューター上の WINDOWS MANAGEMENT INSTRUMENTATION (WMI) に接続します。

## 例

### 例 1: モジュールのメンバーを現在のセッションにインポートする

この例では、 **Psdiagnostics** モジュールのメンバーを現在のセッションにインポートします。

```powershell
Import-Module -Name PSDiagnostics
```

### 例 2: モジュールパスで指定されたすべてのモジュールをインポートする

この例では、環境変数によって指定されたパスにある使用可能なすべてのモジュール `$env:PSModulePath` を現在のセッションにインポートします。

```powershell
Get-Module -ListAvailable | Import-Module
```

### 例 3: 現在のセッションに複数のモジュールのメンバーをインポートする

この例では、 **Psdiagnostics** および **Dism** モジュールのメンバーを現在のセッションにインポートします。

```powershell
$m = Get-Module -ListAvailable PSDiagnostics, Dism
Import-Module -ModuleInfo $m
```

`Get-Module`コマンドレットは、 **Psdiagnostics** および **Dism** モジュールを取得し、変数にオブジェクトを保存し `$m` ます。 セッションにまだインポートされていないモジュールを取得する場合は、 **ListAvailable** パラメーターが必要です。

の **Moduleinfo** パラメーター `Import-Module` は、モジュールを現在のセッションにインポートするために使用されます。

### 例 4: パスによって指定されたすべてのモジュールをインポートする

この例では、明示的なパスを使用して、インポートするモジュールを識別します。

```powershell
Import-Module -Name c:\ps-test\modules\test -Verbose
```

```Output
VERBOSE: Loading module from path 'C:\ps-test\modules\Test\Test.psm1'.
VERBOSE: Exporting function 'my-parm'.
VERBOSE: Exporting function 'Get-Parameter'.
VERBOSE: Exporting function 'Get-Specification'.
VERBOSE: Exporting function 'Get-SpecDetails'.
```

**Verbose** パラメーターを使用すると、は、 `Import-Module` モジュールを読み込むときに進行状況を報告します。
**Verbose**、 **PassThru**、または **ascustomobject** 各パラメーターを指定しない `Import-Module` と、モジュールをインポートしても出力は生成されません。

### 例 5: セッションにインポートされたモジュールメンバーを制限する

この例では、セッションにインポートされるモジュールメンバーと、セッションに対するこのコマンドの影響を制限する方法を示します。 **関数** パラメーターは、モジュールからインポートされるメンバーを制限します。 **エイリアス**、**変数**、および **コマンドレット** パラメーターを使用して、モジュールがインポートする他のメンバーを制限することもできます。

`Get-Module`コマンドレットは、 **psdiagnostics** モジュールを表すオブジェクトを取得します。 **ExportedCmdlets** プロパティは、すべてがインポートされていない場合でも、モジュールがエクスポートするすべてのコマンドレットを一覧表示します。

```powershell
Import-Module PSDiagnostics -Function Disable-PSTrace, Enable-PSTrace
(Get-Module PSDiagnostics).ExportedCommands
```

```Output
Key                          Value
---                          -----
Disable-PSTrace              Disable-PSTrace
Disable-PSWSManCombinedTrace Disable-PSWSManCombinedTrace
Disable-WSManTrace           Disable-WSManTrace
Enable-PSTrace               Enable-PSTrace
Enable-PSWSManCombinedTrace  Enable-PSWSManCombinedTrace
Enable-WSManTrace            Enable-WSManTrace
Get-LogProperties            Get-LogProperties
Set-LogProperties            Set-LogProperties
Start-Trace                  Start-Trace
Stop-Trace                   Stop-Trace
```

```powershell
Get-Command -Module PSDiagnostics
```

```Output
CommandType     Name                 Version    Source
-----------     ----                 -------    ------
Function        Disable-PSTrace      6.1.0.0    PSDiagnostics
Function        Enable-PSTrace       6.1.0.0    PSDiagnostics
```

コマンドレットの **module** パラメーターを使用すると、 `Get-Command` **psdiagnostics** モジュールからインポートされたコマンドが表示されます。 結果によって、およびコマンドレットだけがインポートされたことが確認さ `Disable-PSTrace` `Enable-PSTrace` れます。

### 例 6: モジュールのメンバーをインポートし、プレフィックスを追加する

この例では、 **Psdiagnostics** モジュールを現在のセッションにインポートし、メンバー名にプレフィックスを追加して、プレフィックスの付いたメンバー名を表示します。 の **prefix** パラメーターは、 `Import-Module` モジュールからインポートされるすべてのメンバーに **x** プレフィックスを追加します。 プレフィックスは、現在のセッションのメンバーにのみ適用されます。 モジュールは変更されません。 **PassThru** パラメーターは、インポートされたモジュールを表すモジュールオブジェクトを返します。

```powershell
Import-Module PSDiagnostics -Prefix x -PassThru
```

```Output
ModuleType Version    Name               ExportedCommands
---------- -------    ----               ----------------
Script     6.1.0.0    PSDiagnostics      {Disable-xPSTrace, Disable-xPSWSManCombinedTrace, Disable-xW...
```

```powershell
Get-Command -Module PSDiagnostics
```

```Output
CommandType     Name                                   Version    Source
-----------     ----                                   -------    ------
Function        Disable-xPSTrace                       6.1.0.0    PSDiagnostics
Function        Disable-xPSWSManCombinedTrace          6.1.0.0    PSDiagnostics
Function        Disable-xWSManTrace                    6.1.0.0    PSDiagnostics
Function        Enable-xPSTrace                        6.1.0.0    PSDiagnostics
Function        Enable-xPSWSManCombinedTrace           6.1.0.0    PSDiagnostics
Function        Enable-xWSManTrace                     6.1.0.0    PSDiagnostics
Function        Get-xLogProperties                     6.1.0.0    PSDiagnostics
Function        Set-xLogProperties                     6.1.0.0    PSDiagnostics
Function        Start-xTrace                           6.1.0.0    PSDiagnostics
Function        Stop-xTrace                            6.1.0.0    PSDiagnostics
```

`Get-Command` モジュールからインポートされたメンバーを取得します。 出力から、モジュール メンバーにプレフィックスが正しく追加されたことがわかります。

### 例 7: カスタムオブジェクトを取得して使用する

この例では、によって返されるカスタムオブジェクトを取得して使用する方法を示し `Import-Module` ます。

カスタム オブジェクトには、インポートされた各モジュール メンバーを表す合成メンバーが含まれます。 たとえば、モジュールのコマンドレットと関数は、カスタム オブジェクトのスクリプト メソッドに変換されます。

カスタムオブジェクトは、スクリプト作成に役立ちます。 また、インポートされた複数のオブジェクトが同じ名前を持つ場合にも役立ちます。 オブジェクトのスクリプト メソッドを使用すると、モジュール名を含め、インポートされたメンバーの完全修飾名を指定した場合と同じ効果が得られます。

**Ascustomobject** "パラメーター" は、スクリプトモジュールをインポートするときにのみ使用できます。 使用 `Get-Module` できるモジュールがスクリプトモジュールであるかどうかを判断するには、を使用します。

```powershell
Get-Module -List | Format-Table -Property Name, ModuleType -AutoSize
```

```Output
Name          ModuleType
----          ----------
Show-Calendar     Script
BitsTransfer    Manifest
PSDiagnostics   Manifest
TestCmdlets       Script
...
```

```powershell
$a = Import-Module -Name Show-Calendar -AsCustomObject -Passthru
$a | Get-Member
```

```Output
    TypeName: System.Management.Automation.PSCustomObject
Name          MemberType   Definition
----          ----------   ----------
Equals        Method       bool Equals(System.Object obj)
GetHashCode   Method       int GetHashCode()
GetType       Method       type GetType()
ToString      Method       string ToString()
Show-Calendar ScriptMethod System.Object Show-Calendar();
```

```powershell
$a."Show-Calendar"()
```

`Show-Calendar`スクリプトモジュールは、 **Ascustomobject** ユーザーパラメーターを使用してインポートされ、カスタムオブジェクトを要求し、 **PassThru** パラメーターを使用してオブジェクトを返します。 生成されたカスタムオブジェクトは変数に保存され `$a` ます。

`$a`変数をコマンドレットにパイプ処理して、保存された `Get-Member` オブジェクトのプロパティとメソッドを表示します。 出力には、 `Show-Calendar` スクリプトメソッドが示されています。

スクリプトメソッドを呼び出すには、 `Show-Calendar` 名前にハイフンが含まれているため、メソッド名を引用符で囲む必要があります。

### 例 8: 同じセッションにモジュールを再インポートする

この例では、  `Import-Module` モジュールを同じセッションに再インポートときにの Force パラメーターを使用する方法を示します。 **Force** パラメーターは、読み込まれたモジュールを削除し、再度インポートします。

```powershell
Import-Module PSDiagnostics
Import-Module PSDiagnostics -Force -Prefix PS
```

最初のコマンドは、 **Psdiagnostics** モジュールをインポートします。 2 番目のコマンドは、このモジュールを今度は **Prefix** パラメーターを使用してインポートします。

**Force** パラメーターを指定しない場合、セッションには各 **psdiagnostics** コマンドレットの2つのコピーが含まれます。1つは標準名、もう1つはプレフィックス付きの名前を使用します。

### 例 9: インポートされたコマンドによって非表示になっているコマンドを実行する

この例は、インポートされたコマンドで非表示になったコマンドを実行する方法を示しています。 **Testmodule** モジュールには、年と日を返すという名前の関数が含まれてい `Get-Date` ます。

```powershell
Get-Date
```

```Output
Thursday, August 15, 2019 2:26:12 PM
```

```powershell
Import-Module TestModule
Get-Date
```

```Output
19227
```

```powershell
Get-Command Get-Date -All | Format-Table -Property CommandType, Name, ModuleName -AutoSize
```

```Output
CommandType     Name         ModuleName
-----------     ----         ----------
Function        Get-Date     TestModule
Cmdlet          Get-Date     Microsoft.PowerShell.Utility
```

```powershell
Microsoft.PowerShell.Utility\Get-Date
```

```Output
Thursday, August 15, 2019 2:28:31 PM
```

最初の `Get-Date` コマンドレットは、現在の日付の **DateTime** オブジェクトを返します。 **Testmodule** モジュールをインポートすると、は年 `Get-Date` と日を返します。

の **all** パラメーターを使用すると、 `Get-Command` セッション内のすべてのコマンドが表示され `Get-Date` ます。 結果には、セッションに2つの `Get-Date` コマンド ( **testmodule** モジュールの関数と、 **Microsoft. PowerShell. ユーティリティ** モジュールのコマンドレット) があることが示されています。

関数はコマンドレットよりも優先されるため、 `Get-Date` **testmodule** モジュールの関数はコマンドレットではなく、を実行し `Get-Date` ます。 の元のバージョンを実行するには、 `Get-Date` コマンド名をモジュール名で修飾する必要があります。

PowerShell でのコマンドの優先順位の詳細については、「 [about_Command_Precedence](about/about_Command_Precedence.md)」を参照してください。

### 例 10: モジュールの最小バージョンをインポートする

この例では、 **PowerShellGet** モジュールをインポートします。 の **MinimumVersion** パラメーターを使用して `Import-Module` 、バージョン2.0.0 以上のモジュールのみをインポートします。

```powershell
Import-Module -Name PowerShellGet -MinimumVersion 2.0.0
```

また、 **RequiredVersion** パラメーターを使用して、モジュールの特定のバージョンをインポートしたり、キーワードの **Module** パラメーターと **version** パラメーターを使用して、 `#Requires` スクリプト内でモジュールの特定のバージョンを要求したりすることもできます。

### 例 11: 完全修飾名を使用してインポートする

この例では、FullyQualifiedName を使用して、モジュールの特定のバージョンをインポートします。

```powershell
PS> Get-Module -ListAvailable PowerShellGet | Select-Object Name, Version

Name          Version
----          -------
PowerShellGet 2.2.1
PowerShellGet 2.1.3
PowerShellGet 2.1.2
PowerShellGet 1.0.0.1

PS> Import-Module -FullyQualifiedName @{ModuleName = 'PowerShellGet'; ModuleVersion = '2.1.3' }
```

### 例 12: 完全修飾パスを使用してインポートする

この例では、完全修飾パスを使用して、モジュールの特定のバージョンをインポートします。

```powershell
PS> Get-Module -ListAvailable PowerShellGet | Select-Object Path

Path
----
C:\Program Files\PowerShell\Modules\PowerShellGet\2.2.1\PowerShellGet.psd1
C:\program files\powershell\6\Modules\PowerShellGet\PowerShellGet.psd1
C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\2.1.2\PowerShellGet.psd1
C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\1.0.0.1\PowerShellGet.psd1

PS> Import-Module -Name 'C:\Program Files\PowerShell\Modules\PowerShellGet\2.2.1\PowerShellGet.psd1'
```

### 例 13: リモートコンピューターからモジュールをインポートする

この例では、コマンドレットを使用して、 `Import-Module` リモートコンピューターからモジュールをインポートする方法を示します。
このコマンドは、PowerShell の暗黙的なリモート処理機能を使用します。

別のセッションからモジュールをインポートすると、現在のセッションでコマンドレットを使用することができます。
ただし、コマンドレットを使用するコマンドは、リモートセッションで実行されます。

```powershell
$s = New-PSSession -ComputerName Server01
Get-Module -PSSession $s -ListAvailable -Name NetSecurity
```

```Output
ModuleType Name             ExportedCommands
---------- ----             ----------------
Manifest   NetSecurity      {New-NetIPsecAuthProposal, New-NetIPsecMainModeCryptoProposal, New-Ne...
```

```powershell
Import-Module -PSSession $s -Name NetSecurity
Get-Command -Module NetSecurity -Name Get-*Firewall*
```

```Output
CommandType     Name                                               ModuleName
-----------     ----                                               ----------
Function        Get-NetFirewallAddressFilter                       NetSecurity
Function        Get-NetFirewallApplicationFilter                   NetSecurity
Function        Get-NetFirewallInterfaceFilter                     NetSecurity
Function        Get-NetFirewallInterfaceTypeFilter                 NetSecurity
Function        Get-NetFirewallPortFilter                          NetSecurity
Function        Get-NetFirewallProfile                             NetSecurity
Function        Get-NetFirewallRule                                NetSecurity
Function        Get-NetFirewallSecurityFilter                      NetSecurity
Function        Get-NetFirewallServiceFilter                       NetSecurity
Function        Get-NetFirewallSetting                             NetSecurity
```

```powershell
Get-NetFirewallRule -DisplayName "Windows Remote Management*" |
  Format-Table -Property DisplayName, Name -AutoSize
```

```Output
DisplayName                                              Name
-----------                                              ----
Windows Remote Management (HTTP-In)                      WINRM-HTTP-In-TCP
Windows Remote Management (HTTP-In)                      WINRM-HTTP-In-TCP-PUBLIC
Windows Remote Management - Compatibility Mode (HTTP-In) WINRM-HTTP-Compat-In-TCP
```

`New-PSSession` Server01 コンピューターへのリモートセッション (**PSSession**) を作成します。 **PSSession** は変数に保存され `$s` ます。

PSSession パラメーターを指定してを実行 `Get-Module` すると、 **netsecurity** モジュールがリモートコンピューターにインストールされ、使用可能であることが示されます。  このコマンドは、コマンドレットを使用して `Invoke-Command` `Get-Module` リモートセッションでコマンドを実行することと同じです。 例: (`Invoke-Command $s {Get-Module -ListAvailable -Name NetSecurity`

`Import-Module` **PSSession** パラメーターを指定してを実行すると、リモートコンピューターから **netsecurity** モジュールが現在のセッションにインポートされます。 コマンド `Get-Command` レットを使用すると、 **netsecurity** モジュールから **get** および include **Firewall** で始まるコマンドを取得できます。 出力によって、モジュールとそのコマンドレットが現在のセッションにインポートされたことが確認されます。

次に、 `Get-NetFirewallRule` コマンドレットは、Server01 コンピューター上のファイアウォール規則を取得 Windows リモート管理します。 これは、コマンドレットを使用して `Invoke-Command` `Get-NetFirewallRule` リモートセッションで実行することと同じです。

### 例 14: Windows オペレーティングシステムを使用せずにリモートコンピューター上の記憶域を管理する

この例では、コンピューターの管理者がモジュール検出 WMI プロバイダーをインストールしています。これにより、プロバイダー用に設計された CIM コマンドを使用できるようになります。

コマンドレットでは、 `New-CimSession` RSDGF03 という名前のリモートコンピューター上にセッションを作成します。 セッションは、リモートコンピューター上の WMI サービスに接続します。 CIM セッションは変数に保存され `$cs` ます。
`Import-Module` の **CimSession** を使用して、 `$cs` RSDGF03 コンピューターから **ストレージ** CIM モジュールをインポートします。

`Get-Command`コマンドレットは、 `Get-Disk` **ストレージ** モジュール内のコマンドを表示します。 CIM モジュールをローカルセッションにインポートすると、PowerShell によって各コマンドの CDXML ファイルが PowerShell スクリプトに変換されます。 PowerShell スクリプトは、ローカルセッションの関数として表示されます。

`Get-Disk`はローカルセッションで入力されますが、コマンドレットは、インポート元のリモートコンピューター上で暗黙的に実行されます。 コマンドを実行すると、リモートコンピューターからローカルセッションにオブジェクトが返されます。

```powershell
$cs = New-CimSession -ComputerName RSDGF03
Import-Module -CimSession $cs -Name Storage
# Importing a CIM module, converts the CDXML files for each command into PowerShell scripts.
# These appear as functions in the local session.
Get-Command Get-Disk
```

```Output
CommandType     Name                  ModuleName
-----------     ----                  ----------
Function        Get-Disk              Storage
```

```powershell
# Use implicit remoting to query disks on the remote computer from which the module was imported.
Get-Disk
```

```Output
Number Friendly Name           OperationalStatus  Total Size Partition Style
------ -------------           -----------------  ---------- ---------------
0      Virtual HD ATA Device   Online                  40 GB MBR
```

## PARAMETERS

### -エイリアス

このコマンドレットがモジュールから現在のセッションにインポートするエイリアスを指定します。 エイリアスのコンマ区切りのリストを入力します。 ワイルドカード文字を使用できます。

一部のモジュールは、モジュールをインポートすると、選択されたエイリアスをセッションに自動的にエクスポートします。
このパラメーターを使用すると、エクスポートされたエイリアスの中から選択できます。

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

### -ArgumentList

コマンドの実行中にスクリプトモジュールに渡される引数の配列、またはパラメーター値を指定し `Import-Module` ます。 このパラメーターは、スクリプトモジュールをインポートする場合にのみ有効です。

**ArgumentList** パラメーターは、別名 **args** を使用して参照することもできます。 **ArgumentList** の動作の詳細については、「 [about_Splatting](about/about_Splatting.md#splatting-with-arrays)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsCustomObject 場合

このコマンドレットが、インポートされたモジュールメンバーを表すメンバーを持つカスタムオブジェクトを返すことを示します。 このパラメーターは、スクリプト モジュールに対してのみ有効です。

**Ascustomobject** パラメーターを使用すると、 `Import-Module` モジュールメンバーがセッションにインポートされ、その後、 オブジェクトではなく **pscustomobject** オブジェクトが返されます。 カスタム オブジェクトを変数に保存し、ドット表記を使用してメンバーを呼び出すことができます。

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

### -Assembly

アセンブリオブジェクトの配列を指定します。 このコマンドレットは、指定したアセンブリオブジェクトに実装されているコマンドレットとプロバイダーをインポートします。 アセンブリ オブジェクトが保存されている変数か、アセンブリ オブジェクトを作成するコマンドを入力します。 パイプを使用してアセンブリオブジェクトをにパイプすることもでき `Import-Module` ます。

このパラメーターを使用すると、指定されたアセンブリによって実装されたコマンドレットとプロバイダーのみがインポートされます。 モジュールに他のファイルが含まれている場合は、インポートされないため、モジュールの重要なメンバーが不足している可能性があります。 モジュールのデバッグとテスト、またはモジュールの作成者によって使用するように指示された場合は、このパラメーターを使用します。

```yaml
Type: System.Reflection.Assembly[]
Parameter Sets: Assembly
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -CimNamespace

CIM モジュールを公開する代替 CIM プロバイダーの名前空間を指定します。 既定値は、モジュール検出用の WMI プロバイダーの名前空間です。

Windows オペレーティングシステムを実行していないコンピューターやデバイスから CIM モジュールをインポートするには、このパラメーターを使用します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: CimSession
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CimResourceUri

CIM モジュールの代替の場所を指定します。 既定値は、リモートコンピューター上のモジュール検出 WMI プロバイダーのリソース URI です。

Windows オペレーティングシステムを実行していないコンピューターやデバイスから CIM モジュールをインポートするには、このパラメーターを使用します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Uri
Parameter Sets: CimSession
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CimSession

リモート コンピューター上の CIM セッションを指定します。 CIM セッションを含む変数、または [CimSession](../CimCmdlets/Get-CimSession.md) コマンドなどの cim セッションを取得するコマンドを入力します。

`Import-Module` CIM セッション接続を使用して、リモートコンピューターから現在のセッションにモジュールをインポートします。 現在のセッションでインポートされたモジュールのコマンドを使用すると、コマンドはリモートコンピューター上で実行されます。

このパラメーターを使用して、Windows オペレーティングシステムを実行していないコンピューターやデバイス、PowerShell を搭載しているが PowerShell リモート処理が有効になっていない Windows コンピューターからモジュールをインポートできます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession
Parameter Sets: CimSession
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -コマンドレット

このコマンドレットがモジュールから現在のセッションにインポートするコマンドレットの配列を指定します。
ワイルドカード文字を使用できます。

一部のモジュールは、モジュールをインポートすると、選択されたコマンドレットをセッションに自動的にエクスポートします。
このパラメーターを使用すると、エクスポートされたコマンドレットの中から選択できます。

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

### -DisableNameChecking

このコマンドレットが、許可されていない動詞または禁止された文字を含んでいる名前を持つコマンドレットまたは関数をインポートしたときに警告するメッセージを抑制することを示します。

既定では、インポートしたモジュールによって、名前に許可されていない動詞を持つコマンドレットまたは関数がエクスポートされると、PowerShell によって次の警告メッセージが表示されます。

> 警告: インポートされたコマンド名には、検出できない可能性がある未承認の動詞が含まれています。 詳細については Verbose パラメーターを使用するか、「Get-Verb」と入力して承認されている動詞の一覧を参照してください。

このメッセージは、単なる警告です。 実際には、非準拠のコマンドを含む、すべてのモジュールがインポートされます。 モジュール ユーザーにメッセージが表示されますが、名前付けの問題はモジュール作成者が解決する必要があります。

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

### -Force

このパラメーターを指定すると、現在のモジュールの上にモジュールが読み込まれるか、再度読み込まれます。

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

### -FullyQualifiedName

モジュールの完全修飾名をハッシュテーブルとして指定します。 値には、文字列とハッシュテーブルの組み合わせを指定できます。 ハッシュテーブルには、次のキーがあります。

- `ModuleName` - **必須** モジュール名を指定します。
- `GUID` - **省略可能** モジュールの GUID を指定します。
- 以下の3つのキーのいずれかを指定する **必要** もあります。 これらのキーを一緒に使用することはできません。
  - `ModuleVersion` -モジュールの許容される最小バージョンを指定します。
  - `RequiredVersion` -モジュールの正確な必須バージョンを指定します。
  - `MaximumVersion` -モジュールの許容される最大バージョンを指定します。

```yaml
Type: Microsoft.PowerShell.Commands.ModuleSpecification[]
Parameter Sets: FullyQualifiedName, FullyQualifiedNameAndPSSession, FullyQualifiedNameAndWinCompat
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -関数

このコマンドレットがモジュールから現在のセッションにインポートする関数の配列を指定します。
ワイルドカード文字を使用できます。 一部のモジュールは、モジュールをインポートすると、選択された関数をセッションに自動的にエクスポートします。 このパラメーターを使用すると、エクスポートされた関数の中から選択できます。

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

### -グローバル

このコマンドレットがモジュールをグローバルセッション状態にインポートして、セッションのすべてのコマンドで使用できるようにすることを示します。

既定では、 `Import-Module` コマンドプロンプト、スクリプトファイル、または scriptblock からコマンドレットを呼び出すと、すべてのコマンドがグローバルセッション状態にインポートされます。

別のモジュールから呼び出されると、コマンドレットは、 `Import-Module` モジュール内のコマンド (入れ子になったモジュールのコマンドを含む) を呼び出し元のモジュールのセッション状態にインポートします。

> [!TIP]
> モジュール内からを呼び出すことは避けてください `Import-Module` 。 代わりに、親モジュールのマニフェストで入れ子になったモジュールとしてターゲットモジュールを宣言します。 入れ子になったモジュールを宣言すると、依存関係の発見可能性が向上します。

**Global** パラメーターは、値が **Global** である **Scope** パラメーターと同じ効果が得られます。

モジュールによってエクスポートされるコマンドを制限するには、 `Export-ModuleMember` スクリプトモジュールでコマンドを使用します。

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

### -MaximumVersion

最大バージョンを指定します。 このコマンドレットは、指定した値以下のバージョンのモジュールのみをインポートします。 バージョンが修飾していない場合は、 `Import-Module` エラーを返します。

```yaml
Type: System.String
Parameter Sets: Name, PSSession, CimSession, WinCompat
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MinimumVersion

最小バージョンを指定します。 このコマンドレットは、指定した値以上のバージョンのモジュールのみをインポートします。 **MinimumVersion** パラメーター名またはそのエイリアスである **Version** を使用します。 バージョンが修飾されていない場合、は `Import-Module` エラーを生成します。

正確なバージョンを指定するには、**RequiredVersion** パラメーターを使用します。 また、 **#Requires** キーワードの **Module** パラメーターと **Version** パラメーターを使用して、スクリプト内でモジュールの特定のバージョンを要求することもできます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Version
Parameter Sets: Name, PSSession, CimSession, WinCompat
Aliases: Version

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModuleInfo

インポートするモジュールオブジェクトの配列を指定します。 モジュールオブジェクトを含む変数、またはモジュールオブジェクトを取得するコマンド (など) を入力し `Get-Module -ListAvailable` ます。 パイプを使用してモジュールオブジェクトをにパイプすることもでき `Import-Module` ます。

```yaml
Type: System.Management.Automation.PSModuleInfo[]
Parameter Sets: ModuleInfo
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

インポートするモジュールの名前を指定します。 モジュールの名前、または、、、またはファイルなどのモジュール内のファイルの名前を入力し `.psd1` `.psm1` `.dll` `.ps1` ます。 ファイル パスは省略可能です。 ワイルドカード文字は使用できません。 パイプを使用してモジュール名とファイル名をにパイプすることもでき `Import-Module` ます。

パスを省略した場合、は `Import-Module` 環境変数に保存されているパスでモジュールを検索し `$env:PSModulePath` ます。

可能な限り、モジュール名のみを指定してください。 ファイル名を指定した場合は、そのファイルに実装されているメンバーのみがインポートされます。 モジュールに他のファイルが含まれている場合は、インポートされないため、モジュールの重要なメンバーが不足している可能性があります。

> [!NOTE]
> スクリプト ( `.ps1` ) ファイルはモジュールとしてインポートできますが、スクリプトファイルは通常、スクリプトモジュールファイル () ファイルのようには構成されていません `.psm1` 。 スクリプトファイルをインポートしても、モジュールとして使用できることは保証されません。 詳細については、「 [about_Modules](about/about_Modules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: Name, PSSession, CimSession, WinCompat
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: True
```

### -NoClobber

現在のセッションの既存のコマンドと同じ名前のコマンドがインポートされないようにします。 既定では、 `Import-Module` エクスポートされたすべてのモジュールコマンドがインポートされます。

同じ名前を持つコマンドは、セッション内のコマンドを非表示にしたり置き換えることができます。 セッションでのコマンド名の競合を回避するには、**Prefix** パラメーターまたは **NoClobber** パラメーターを使用します。 名前の競合およびコマンドの優先順位の詳細については、[about_Modules](about/about_Modules.md) の「モジュールと名前の競合」および [about_Command_Precedence](about/about_Command_Precedence.md) を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NoOverwrite

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -Prefix

インポートされたモジュールメンバーの名前の名詞にこのコマンドレットが追加するプレフィックスを指定します。

セッションに同じ名前の別のメンバーが存在する場合に発生する名前の競合を回避するには、このパラメーターを使用します。 このパラメーターによってモジュールが変更されることはなく、モジュールが自身で使用するためにインポートするファイルには影響しません。 これらは、入れ子になったモジュールと呼ばれます。 このコマンドレットは、現在のセッションのメンバーの名前にのみ影響します。

たとえば、プレフィックス UTC を指定してからコマンドレットをインポートした場合、 `Get-Date` このコマンドレットはセッションではとして認識され、 `Get-UTCDate` 元のコマンドレットと混同されることはありません `Get-Date` 。

このパラメーターの値は、モジュールの **DefaultCommandPrefix** プロパティ (既定のプレフィックスを指定する) よりも優先されます。

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

### -PSSession

このコマンドレットが現在のセッションにモジュールをインポートするときに使用する、PowerShell のユーザー管理セッション (**PSSession**) を指定します。 **Pssession** を含む変数、またはコマンドなどの **pssession** を取得するコマンドを入力し `Get-PSSession` ます。

別のセッションから現在のセッションにモジュールをインポートする際、ローカル モジュールのコマンドレットを使用するのと同じように、モジュールのコマンドレットを現在のセッションで使用できます。 リモートコマンドレットを使用するコマンドはリモートセッションで実行されますが、リモート処理の詳細は PowerShell によってバックグラウンドで管理されます。

このパラメーターは、PowerShell の暗黙的なリモート処理機能を使用します。 これは、コマンドレットを使用して、セッションから特定のモジュールをインポートすることと同じです `Import-PSSession` 。

`Import-Module` 別のセッションから PowerShell コアモジュールをインポートすることはできません。 PowerShell コアモジュールには、Microsoft. PowerShell で始まる名前が付いています。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: PSSession, FullyQualifiedNameAndPSSession
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredVersion

このコマンドレットによってインポートされるモジュールのバージョンを指定します。 バージョンがインストールされていない場合は、によって `Import-Module` エラーが生成されます。

既定では、は `Import-Module` バージョン番号をチェックせずにモジュールをインポートします。

最小バージョンを指定するには、**MinimumVersion** パラメーターを使用します。 また、 **#Requires** キーワードの **Module** パラメーターと **Version** パラメーターを使用して、スクリプト内でモジュールの特定のバージョンを要求することもできます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

Windows オペレーティングシステムの既存のリリースに含まれているモジュールをインポートするために、 **RequiredVersion** を使用するスクリプトは、windows オペレーティングシステムの将来のリリースでは自動的には実行されません。 これは、Windows オペレーティングシステムの将来のリリースにおける PowerShell モジュールのバージョン番号が、Windows オペレーティングシステムの既存のリリースにおけるモジュールのバージョン番号よりも大きいためです。

```yaml
Type: System.Version
Parameter Sets: Name, PSSession, CimSession, WinCompat
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -スコープ

このコマンドレットがモジュールをインポートするスコープを指定します。

このパラメーターの有効値は、次のとおりです。

- **グローバル**。 セッション内のすべてのコマンドに使用できます。 **Global** パラメーターと同じ効果が得られます。
- **ローカル**。 現在のスコープでのみ使用できます。

既定では、 `Import-Module` コマンドプロンプト、スクリプトファイル、または scriptblock からコマンドレットを呼び出すと、すべてのコマンドがグローバルセッション状態にインポートされます。 パラメーターを使用し `-Scope Local` て、モジュールの内容をスクリプトまたは scriptblock スコープにインポートできます。

別のモジュールから呼び出された場合、コマンドレットは、 `Import-Module` 入れ子になったモジュールのコマンドを含むモジュール内のコマンドを呼び出し元のセッション状態にインポートします。 またはを指定する `-Scope Global` と `-Global` 、このコマンドレットによってモジュールがグローバルセッション状態にインポートされ、セッションのすべてのコマンドで使用できるようになります。

**Global** パラメーターは、値が **Global** である **Scope** パラメーターと同じ効果が得られます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Local, Global

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Variable

このコマンドレットがモジュールから現在のセッションにインポートする変数の配列を指定します。
変数のリストを入力します。 ワイルドカード文字を使用できます。

一部のモジュールは、モジュールをインポートすると、選択された変数をセッションに自動的にエクスポートします。
このパラメーターを使用すると、エクスポートされた変数の中から選択できます。

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

### -SkipEditionCheck

フィールドのチェックをスキップし `CompatiblePSEditions` ます。

モジュールが `"$($env:windir)\System32\WindowsPowerShell\v1.0\Modules"` マニフェストフィールドにを指定していない場合に、モジュールディレクトリから PowerShell Core にモジュールを読み込むことができ `Core` `CompatiblePSEditions` ます。

別のパスからモジュールをインポートする場合、このスイッチは何も実行しません。これは、チェックが実行されないためです。 Linux と macOS では、このスイッチは何も行いません。

詳細については、「 [about_PowerShell_Editions](About/about_PowerShell_Editions.md)」を参照してください。

> [!WARNING]
> `Import-Module -SkipEditionCheck` でも、モジュールのインポートに失敗する可能性があります。 成功した場合でも、モジュールからコマンドを呼び出すと、互換性のない API を使用しようとすると、後でエラーが発生する可能性があります。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Name, PSSession, CimSession, FullyQualifiedName, FullyQualifiedNameAndPSSession, Assembly, ModuleInfo
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseWindowsPowerShell

Windows PowerShell の互換性機能を使用してモジュールを読み込みます。 詳細については、「 [about_Windows_PowerShell_Compatibility](About/about_Windows_PowerShell_Compatibility.md) 」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: WinCompat, FullyQualifiedNameAndWinCompat
Aliases: UseWinPS

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.string、PSModuleInfo、システムのアセンブリを指定します。

パイプを使用して、モジュール名、モジュールオブジェクト、またはアセンブリオブジェクトをこのコマンドレットに渡します。

## 出力

### なし、PSModuleInfo、またはシステムの管理.......

既定で `Import-Module` は、は出力を生成しません。 **PassThru** パラメーターを指定すると、このコマンドレットは、モジュールを表す **PSModuleInfo** オブジェクトを生成します。 **Ascustomobject** いうパラメーターを指定すると、 **pscustomobject** オブジェクトが生成されます。

## 注

- モジュールをインポートする前に、モジュールがローカルコンピューターにインストールされている必要があります。 つまり、モジュールディレクトリは、ローカルコンピューターからアクセスできるディレクトリにコピーする必要があります。 詳細については、「 [about_Modules](About/about_Modules.md)」を参照してください。

  また、**PSSession** パラメーターと **CIMSession** パラメーターを使用すれば、リモート コンピューターにインストールされているモジュールをインポートすることもできます。 ただし、これらのモジュールのコマンドレットを使用するコマンドは、リモートコンピューター上のリモートセッションで実行されます。

- セッションに同じ名前と同じ種類のメンバーをインポートする場合、PowerShell は、既定で最後にインポートされたメンバーを使用します。 変数と別名は置き換えられ、元の変数にはアクセスできません。 関数、コマンドレット、およびプロバイダーは、新しいメンバーによって単純にシャドウされます。 これらのコマンド名には、スナップイン、モジュール、または関数パスの名前を指定することによってアクセスできます。

- モジュールからインポートされたコマンドの書式設定データを更新するには、 `Update-FormatData` コマンドレットを使用します。 `Update-FormatData` では、モジュールからインポートされたセッションのコマンドの書式設定データも更新されます。 モジュールのフォーマットファイルが変更された場合は、コマンドを実行して、インポートした `Update-FormatData` コマンドの書式設定データを更新できます。 モジュールをもう一度インポートする必要はありません。

- Windows PowerShell 3.0 以降では、PowerShell でインストールされるコアコマンドはモジュールにパッケージ化されています。 Windows PowerShell 2.0、およびそれ以降のバージョンの PowerShell で古いスタイルのセッションを作成するホストプログラムでは、コアコマンドはスナップイン (**PSSnapins**) にパッケージ化されます。 例外は、常にスナップインである、 **PowerShell です**。 また、コマンドレットによって開始されるようなリモートセッション `New-PSSession` は、コアスナップインを含む古いスタイルのセッションです。

  コアモジュールで新しいスタイルのセッションを作成する **CreateDefault2** メソッドの詳細については、 [CreateDefault2 メソッド](/dotnet/api/system.management.automation.runspaces.initialsessionstate.createdefault2)を参照してください。

- Windows PowerShell 2.0 では、モジュールオブジェクトの一部のプロパティ値 ( **ExportedCmdlets** や **nestedmodules** プロパティ値など) は、モジュールがインポートされるまで設定されませんでした。

- Windows PowerShell 3.0 + と互換性のない混合モードのアセンブリを含むモジュールをインポートしようとすると、 `Import-Module` 次のようなエラーメッセージが返されます。

  > Import-Module: 混合モードのアセンブリは、ランタイムのバージョン ' v v2.0.50727 ' に対してビルドされており、追加の構成情報がないと、4.0 ランタイムに読み込むことができません。

  このエラーは、Windows PowerShell 2.0 用に設計されたモジュールに、少なくとも1つのモジュールアセンブリが含まれている場合に発生します。 C++ や C# などのマネージコードと非マネージコードの両方を含む、モジュールが混在するアセンブリ。

  混合モードアセンブリを含むモジュールをインポートするには、次のコマンドを使用して Windows PowerShell 2.0 を起動し、コマンドを再試行し `Import-Module` ます。

  `PowerShell.exe -Version 2.0`

- CIM セッション機能を使用するには、リモート コンピューターにおいて、WS-Management リモート処理と Common Information Model (CIM) 標準の Microsoft 実装である Windows Management Instrumentation (WMI) が必要です。 さらに、モジュール検出用の WMI プロバイダーまたは同じ基本機能を備えた代替 CIM プロバイダーもコンピューターに必要です。

  CIM セッション機能は、Windows オペレーティングシステムを実行していないコンピューターと PowerShell を搭載している Windows コンピューターで使用できますが、PowerShell リモート処理は有効になっていません。

  また、CIM パラメーターを使用して、ローカルコンピューターを含め、PowerShell リモート処理が有効になっているコンピューターから CIM モジュールを取得することもできます。 ローカルコンピューターに CIM セッションを作成すると、PowerShell は WMI ではなく DCOM を使用してセッションを作成します。

- 既定では、は、 `Import-Module` 子孫スコープから呼び出された場合でも、グローバルスコープ内のモジュールをインポートします。 最上位レベルのスコープとすべての子孫のスコープは、モジュールのエクスポートされた要素にアクセスできます。

  子孫スコープでは、 `-Scope Local` そのスコープとそのすべての子孫スコープへのインポートが制限されます。 親スコープでは、インポートされたメンバーは表示されません。

  > [!NOTE]
  > `Get-Module` 現在のセッションで読み込まれたすべてのモジュールを表示します。 これには、子孫スコープにローカルに読み込まれたモジュールが含まれます。 `Get-Command -Module modulename`現在のスコープに読み込まれているメンバーを確認するには、を使用します。

  `Import-Module` モジュール内のクラス定義と列挙型定義を読み込みません。 `using module`スクリプトの先頭にあるステートメントを使用します。 これにより、クラスと列挙型の定義を含むモジュールがインポートされます。 詳細については、「 [about_Using](About/about_Using.md)」を参照してください。

## 関連リンク

[about_Modules](about/about_Modules.md)

[Export-ModuleMember](Export-ModuleMember.md)

[Get-Module](Get-Module.md)

[New-Module](New-Module.md)

[Remove-Module](Remove-Module.md)

[about_PowerShell_Editions](About/about_PowerShell_Editions.md)
