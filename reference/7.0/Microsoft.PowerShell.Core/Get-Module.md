---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 12/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-module?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Module
ms.openlocfilehash: 971f6efcbdec06ed5733e005ef0e95452d6c31a0
ms.sourcegitcommit: 7b376314e7640c39a53aac9f0db8bb935514a960
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2020
ms.locfileid: "96564522"
---
# Get-Module

## 概要
現在のセッションでインポートされたモジュール、または PSModulePath からインポートできるモジュールを一覧表示します。

## SYNTAX

### 読み込み済み (既定値)

```
Get-Module [[-Name] <String[]>] [-FullyQualifiedName <ModuleSpecification[]>] [-All] [<CommonParameters>]
```

### 利用可能

```
Get-Module [[-Name] <String[]>] [-FullyQualifiedName <ModuleSpecification[]>] [-All] [-ListAvailable]
 [-PSEdition <String>] [-SkipEditionCheck] [-Refresh] [<CommonParameters>]
```

### PsSession

```
Get-Module [[-Name] <String[]>] [-FullyQualifiedName <ModuleSpecification[]>] [-ListAvailable]
 [-PSEdition <String>] [-SkipEditionCheck] [-Refresh] -PSSession <PSSession> [<CommonParameters>]
```

### CimSession

```
Get-Module [[-Name] <String[]>] [-FullyQualifiedName <ModuleSpecification[]>] [-ListAvailable]
 [-SkipEditionCheck] [-Refresh] -CimSession <CimSession> [-CimResourceUri <Uri>] [-CimNamespace <String>]
 [<CommonParameters>]
```

## Description

`Get-Module`コマンドレットにより、インポートされた、またはインポート可能な powershell モジュールが powershell セッションに一覧表示されます。 パラメーターを指定しない場合、 `Get-Module` 現在のセッションにインポートされているモジュールを取得します。 **ListAvailable** パラメーターは、PSModulePath 環境変数 () で指定されたパスからインポートできるモジュールを一覧表示するために使用され `$env:PSModulePath` ます。

を返すモジュールオブジェクトに `Get-Module` は、モジュールに関する重要な情報が含まれています。 また、 `Import-Module` コマンドレットやコマンドレットなど、他のコマンドレットにモジュールオブジェクトをパイプ処理することもでき `Remove-Module` ます。

`Get-Module` モジュールを一覧表示しますが、インポートしません。 Windows PowerShell 3.0 以降では、モジュールのコマンドを使用すると、モジュールは自動的にインポートされますが、 `Get-Module` コマンドによって自動インポートがトリガーされることはありません。 コマンドレットを使用して、モジュールをセッションにインポートすることもでき `Import-Module` ます。

Windows PowerShell 3.0 以降では、リモートセッションからローカルセッションにモジュールを取得してインポートできます。 この方法では、PowerShell の暗黙的なリモート処理機能を使用します。これは、コマンドレットを使用することと同じです `Import-PSSession` 。 別のセッションからインポートされたモジュールでコマンドを使用すると、リモートセッションでコマンドが暗黙的に実行されます。 この機能を使用すると、ローカルセッションからリモートコンピューターを管理できます。

また、Windows PowerShell 3.0 以降では、およびを使用して `Get-Module` `Import-Module` COMMON INFORMATION MODEL (CIM) モジュールを取得およびインポートできます。 CIM モジュールは、コマンドレット定義 XML (CDXML) ファイルのコマンドレットを定義します。 この機能を使用すると、C++ で記述されたものなど、マネージコード以外のアセンブリに実装されているコマンドレットを使用できます。

暗黙的なリモート処理を使用すると、PowerShell リモート処理が有効になっているリモートコンピューターを管理できます。
リモートコンピューター上に **pssession** を作成し、の **pssession** パラメーターを使用して、 `Get-Module` リモートセッションの PowerShell モジュールを取得します。 リモートセッションからモジュールをインポートすると、インポートしたコマンドはリモートコンピューター上のセッションで実行されます。

同様の戦略を使用して、PowerShell リモート処理が有効になっていないコンピューターを管理できます。
これには、Windows オペレーティングシステムを実行していないコンピューターや、powershell を搭載していても PowerShell リモート処理が有効になっていないコンピューターが含まれます。

まず、リモートコンピューターに CIM セッションを作成します。 CIM セッションは、リモートコンピューター上の Windows Management Instrumentation (WMI) に接続します。 次に、の **CIMSession** パラメーターを使用して `Get-Module` 、CIM セッションから cim モジュールを取得します。 コマンドレットを使用して CIM モジュールをインポートし、インポートされたコマンドを実行すると、 `Import-Module` コマンドはリモートコンピューター上で暗黙的に実行されます。 この WMI と CIM の戦略を使用して、リモート コンピューターを管理できます。

## 例

### 例 1: 現在のセッションにインポートされたモジュールを取得する

```powershell
Get-Module
```

このコマンドは、現在のセッションにインポートされているモジュールを取得します。

### 例 2: インストールされているモジュールと使用可能なモジュールを取得する

```powershell
Get-Module -ListAvailable
```

このコマンドは、コンピューターにインストールされていて現在のセッションにインポートできるモジュールを取得します。

`Get-Module`**$env:P SModulePath** 環境変数で指定されたパスで使用可能なモジュールを検索します。 **PSModulePath** の詳細については、「[about_Modules](About/about_Modules.md)」および「[about_Environment_Variables](About/about_Environment_Variables.md)」を参照してください。

### 例 3: エクスポートされたすべてのファイルを取得する

```powershell
Get-Module -ListAvailable -All
```

このコマンドは、すべての利用可能なモジュールのすべてのエクスポートされたファイルを取得します。

### 例 4: 完全修飾名でモジュールを取得する

```powershell
$FullyQualifedName = @{ModuleName="Microsoft.PowerShell.Management";ModuleVersion="3.1.0.0"}
Get-Module -FullyQualifiedName $FullyQualifedName | Format-Table -Property Name,Version
```

```Output
Name                             Version
----                             -------
Microsoft.PowerShell.Management  3.1.0.0
```

このコマンドは、 **FullyQualifiedName** パラメーターを使用してモジュールの完全修飾名を指定することによって、**管理** モジュールを取得します。 次に、結果をコマンドレットにパイプして、 `Format-Table` 結果を列見出しとして **名前** と **バージョン** を含むテーブルとして書式設定します。

### 例 5: モジュールのプロパティを取得する

```powershell
Get-Module | Get-Member -MemberType Property | Format-Table Name
```

```Output
Name
----
AccessMode
Author
ClrVersion
CompanyName
Copyright
Definition
Description
DotNetFrameworkVersion
ExportedAliases
ExportedCmdlets
ExportedCommands
ExportedFormatFiles
ExportedFunctions
ExportedTypeFiles
ExportedVariables
ExportedWorkflows
FileList
Guid
HelpInfoUri
LogPipelineExecutionDetails
ModuleBase
ModuleList
ModuleType
Name
NestedModules
OnRemove
Path
PowerShellHostName
PowerShellHostVersion
PowerShellVersion
PrivateData
ProcessorArchitecture
RequiredAssemblies
RequiredModules
RootModule
Scripts
SessionState
Version
```

このコマンドは、を返す **PSModuleInfo** オブジェクトのプロパティを取得し `Get-Module` ます。 モジュール ファイルごとに 1 つのオブジェクトがあります。

プロパティを使用して、モジュール オブジェクトを書式設定したり、フィルター処理したりできます。 プロパティの詳細については、「 [PSModuleInfo properties](/dotnet/api/system.management.automation.psmoduleinfo)」を参照してください。

出力には、Windows PowerShell 3.0 で導入された **作成者** や **CompanyName** などの新しいプロパティが含まれています。

### 例 6: すべてのモジュールを名前でグループ化する

```powershell
Get-Module -ListAvailable -All | Format-Table -Property Name, Moduletype, Path -Groupby Name
```

```Output
Name: AppLocker

Name      ModuleType Path
----      ---------- ----
AppLocker   Manifest C:\Windows\system32\WindowsPowerShell\v1.0\Modules\AppLocker\AppLocker.psd1


   Name: Appx

Name ModuleType Path
---- ---------- ----
Appx   Manifest C:\Windows\system32\WindowsPowerShell\v1.0\Modules\Appx\en-US\Appx.psd1
Appx   Manifest C:\Windows\system32\WindowsPowerShell\v1.0\Modules\Appx\Appx.psd1
Appx     Script C:\Windows\system32\WindowsPowerShell\v1.0\Modules\Appx\Appx.psm1


   Name: BestPractices

Name          ModuleType Path
----          ---------- ----
BestPractices   Manifest C:\Windows\system32\WindowsPowerShell\v1.0\Modules\BestPractices\BestPractices.psd1


   Name: BitsTransfer

Name         ModuleType Path
----         ---------- ----
BitsTransfer   Manifest C:\Windows\system32\WindowsPowerShell\v1.0\Modules\BitsTransfer\BitsTransfer.psd1
```

このコマンドは、インポートされたモジュールファイルと使用可能なモジュールファイルの両方を取得し、モジュール名でグループ化します。 これにより、それぞれのスクリプトによってエクスポートされるモジュール ファイルを確認できます。

### 例 7: モジュールマニフェストの内容を表示する

これらのコマンドは、Windows PowerShell **BitsTransfer** モジュールのモジュールマニフェストの内容を表示します。

モジュールにマニフェストファイルを含める必要はありません。 マニフェストファイルがある場合、マニフェストファイルはバージョン番号を含めるためにのみ必要です。 ただし、マニフェスト ファイルは、多くの場合、モジュール、その要件、およびその内容に関する有用な情報を提供します。

```powershell
# First command
$m = Get-Module -list -Name BitsTransfer

# Second command
Get-Content $m.Path
```

```Output
@ {
    GUID               = "{8FA5064B-8479-4c5c-86EA-0D311FE48875}"
    Author             = "Microsoft Corporation"
    CompanyName        = "Microsoft Corporation"
    Copyright          = "Microsoft Corporation. All rights reserved."
    ModuleVersion      = "1.0.0.0"
    Description        = "Windows PowerShell File Transfer Module"
    PowerShellVersion  = "2.0"
    CLRVersion         = "2.0"
    NestedModules      = "Microsoft.BackgroundIntelligentTransfer.Management"
    FormatsToProcess   = "FileTransfer.Format.ps1xml"
    RequiredAssemblies = Join-Path $psScriptRoot "Microsoft.BackgroundIntelligentTransfer.Management.Interop.dll"
}
```

最初のコマンドは、BitsTransfer モジュールを表す PSModuleInfo オブジェクトを取得します。 変数にオブジェクトを保存し `$m` ます。

2番目のコマンドは、コマンドレットを使用して、 `Get-Content` 指定されたパスのマニフェストファイルの内容を取得します。 ここでは、ドット付き表記を使用してマニフェスト ファイルへのパスを取得し、オブジェクトの Path プロパティに格納しています。 出力には、モジュール マニフェストの内容が表示されます。

### 例 8: モジュールディレクトリ内のファイルを一覧表示する

```powershell
dir (Get-Module -ListAvailable FileTransfer).ModuleBase
```

```Output
Directory: C:\Windows\system32\WindowsPowerShell\v1.0\Modules\FileTransfer
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        12/16/2008  12:36 PM            en-US
-a---        11/19/2008  11:30 PM      16184 FileTransfer.Format.ps1xml
-a---        11/20/2008  11:30 PM       1044 FileTransfer.psd1
-a---        12/16/2008  12:20 AM     108544 Microsoft.BackgroundIntelligentTransfer.Management.Interop.dll
```

このコマンドは、モジュールのディレクトリ内のファイルを一覧表示します。 これは、インポートの前にモジュールの内容を確認するためのもう 1 つの方法です。 モジュールによっては、ヘルプ ファイルやモジュールについて記述した ReadMe ファイルが含まれている場合があります。

### 例 9: コンピューターにインストールされているモジュールを取得する

```powershell
$s = New-PSSession -ComputerName Server01

Get-Module -PSSession $s -ListAvailable
```

これらのコマンドは、Server01 コンピューターにインストールされているモジュールを取得します。

最初のコマンドは、コマンドレットを使用して、 `New-PSSession` Server01 コンピューター上に **PSSession** を作成します。 このコマンドは、 **PSSession** を $s 変数に保存します。

2番目のコマンドは、の **pssession** パラメーターと **ListAvailable** パラメーターを使用して、 `Get-Module` 変数内の **pssession** のモジュールを取得し `$s` ます。

モジュールを他のセッションからコマンドレットにパイプする場合 `Import-Module` 、は `Import-Module` 暗黙的なリモート処理機能を使用して、モジュールを現在のセッションにインポートします。 これは、コマンドレットを使用することと同じです `Import-PSSession` 。 現在のセッションに含まれるモジュールのコマンドレットを使用することができますが、これらのコマンドレットを使用するコマンドは、実際にはリモート セッションで実行されます。 詳細については、[`Import-Module`](Import-Module.md) および [`Import-PSSession`](../Microsoft.PowerShell.Utility/Import-PSSession.md) を参照してください。

### 例 10: Windows オペレーティングシステムを実行していないコンピューターを管理する

この例のコマンドを使用すると、Windows オペレーティングシステムを実行していないリモートコンピューターの記憶域システムを管理できます。
この例では、コンピューターの管理者によってモジュール検出用の WMI プロバイダーがインストールされているため、CIM コマンドでプロバイダー用の既定値を使用できます。

```powershell
$cs = New-CimSession -ComputerName RSDGF03
Get-Module -CimSession $cs -Name Storage | Import-Module
Get-Command Get-Disk
```

```Output
CommandType     Name                  ModuleName
-----------     ----                  ----------
Function        Get-Disk              Storage
```

```powershell
Get-Disk
```

```Output
Number Friendly Name              OperationalStatus          Total Size Partition Style
------ -------------              -----------------          ---------- ---------------
0      Virtual HD ATA Device      Online                          40 GB MBR
```

最初のコマンドは、 `New-CimSession` コマンドレットを使用して、RSDGF03 リモートコンピューター上にセッションを作成します。 このセッションは、リモート コンピューター上の WMI に接続します。 このコマンドは、CIM セッションを変数に保存し `$cs` ます。

2番目のコマンドは、変数の CIM セッションを使用して、 `$cs` `Get-Module` RSDGF03 コンピューターでコマンドを実行します。 ここでは、Name パラメーターを使用して、Storage モジュールを指定しています。 このコマンドは、パイプライン演算子 (|) を使用して、ストレージモジュールをコマンドレットに送信します。これにより `Import-Module` 、ローカルセッションにインポートされます。

3番目のコマンドは、 `Get-Command` ストレージモジュールのコマンドでコマンドレットを実行し `Get-Disk` ます。
CIM モジュールをローカルセッションにインポートすると、PowerShell は CIM モジュールを表す CDXML ファイルを PowerShell スクリプトに変換します。これは、ローカルセッションの関数として表示されます。

4番目のコマンドは、コマンドを実行し `Get-Disk` ます。 コマンドはローカル セッションで入力されますが、暗黙的にインポート先のリモート コンピューター上で実行されます。 コマンドは、リモート コンピューターからオブジェクトを取得してローカル セッションに返します。

## PARAMETERS

### -All

このコマンドレットが各モジュールフォルダー内のすべてのモジュールを取得することを示します。これには、入れ子になったモジュール、マニフェスト (.psd1) ファイル、スクリプトモジュール (hbase-runner.psm1) ファイル、およびバイナリモジュール (.dll) ファイルが含まれます。 このパラメーターを指定しない場合、 `Get-Module` 各モジュールフォルダー内の既定のモジュールのみを取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Loaded, Available
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -CimNamespace

CIM モジュールを公開する代替 CIM プロバイダーの名前空間を指定します。 既定値は、モジュール検出用の WMI プロバイダーの名前空間です。

Windows オペレーティングシステムを実行していないコンピューターやデバイスから CIM モジュールを取得するには、このパラメーターを使用します。

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

Windows オペレーティングシステムを実行していないコンピューターやデバイスから CIM モジュールを取得するには、このパラメーターを使用します。

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

リモート コンピューター上の CIM セッションを指定します。 CIM セッションを含む変数、または [CimSession](/powershell/module/cimcmdlets/get-cimsession) コマンドなどの cim セッションを取得するコマンドを入力します。

`Get-Module` は、CIM セッション接続を使用して、リモートコンピューターからモジュールを取得します。 コマンドレットを使用してモジュールをインポート `Import-Module` し、現在のセッションでインポートされたモジュールのコマンドを使用すると、コマンドは実際にはリモートコンピューター上で実行されます。

このパラメーターを使用すると、Windows オペレーティングシステムを実行していないコンピューターやデバイス、PowerShell を搭載しているが PowerShell リモート処理が有効になっていないコンピューターからモジュールを取得できます。

**CimSession** パラメーターは、**CIMSession** 内のすべてのモジュールを取得します。 ただし、インポートできるのは、CIM ベースのモジュールとコマンドレット定義 XML (CDXML) ベースのモジュールのみです。

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

### -FullyQualifiedName

**Modul特定** のオブジェクトの形式で指定された名前を持つモジュールを指定します。 「 [Modulの認識コンストラクター (Hashtable)](/dotnet/api/microsoft.powershell.commands.modulespecification.-ctor#Microsoft_PowerShell_Commands_ModuleSpecification__ctor_System_Collections_Hashtable_)」の「解説」を参照してください。

たとえば、 **FullyQualifiedModule** パラメーターは、次のいずれかの形式で指定されたモジュール名を受け入れます。

- `@{ModuleName = "modulename"; ModuleVersion = "version_number"}`
- `@{ModuleName = "modulename"; ModuleVersion = "version_number"; Guid = "GUID"}`

**ModuleName** と **ModuleVersion** は必須ですが、**Guid** は省略可能です。 **モジュール** パラメーターと同じコマンドで **FullyQualifiedModule** パラメーターを指定することはできません。 2つのパラメーターは相互に排他的です。

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

### -ListAvailable

このコマンドレットがインストールされているすべてのモジュールを取得することを示します。 `Get-Module`**PSModulePath** 環境変数に示されているパス内のモジュールを取得します。 このパラメーターを指定しない場合、 `Get-Module` **PSModulePath** 環境変数に一覧表示されているモジュールと、現在のセッションに読み込まれているモジュールのみが取得されます。 **ListAvailable** は、**PSModulePath** 環境変数に含まれていないモジュールが現在のセッションに読み込まれている場合でも、これらのモジュールに関する情報を返しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Available, PsSession, CimSession
Aliases:

Required: True (Available), False (PsSession, CimSession)
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

このコマンドレットが取得するモジュールの名前または名前パターンを指定します。 ワイルドカード文字を使用できます。 パイプを使用して名前をにパイプすることもでき `Get-Module` ます。 **Name** パラメーターと同じコマンドで **FullyQualifiedName** パラメーターを指定することはできません。

**名前** には、モジュール GUID を値として使用できません。
GUID を指定してモジュールを返すには、代わりに **FullyQualifiedName** を使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: True
```

### -PSEdition

指定された PowerShell のエディションをサポートするモジュールを取得します。

このパラメーターの有効値は、次のとおりです。

- デスクトップ
- コア

Get-Module コマンドレットは、指定された値の **PSModuleInfo** オブジェクトの **CompatiblePSEditions** プロパティを確認し、その値が設定されているモジュールだけを返します。

> [!NOTE]
>
> - **Desktop Edition:** .NET Framework 上に構築され、Windows の完全フットプリント エディション (Server Core、Windows Desktop など) で実行される PowerShell のバージョンをターゲットとするスクリプトおよびモジュールと互換性があります。
> - **コア エディション:** .NET Core 上に構築されており、Nano Server や Windows IoT などの Windows の縮小エディションで実行する PowerShell のバージョンを対象とするスクリプトおよびモジュールとの互換性を提供します。

```yaml
Type: System.String
Parameter Sets: PsSession, Available
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PSSession

指定されたユーザー管理の PowerShell セッション (**PSSession**) 内のモジュールを取得します。 セッションを含む変数、コマンドなどのセッションを取得するコマンド、またはコマンドなどのセッションを作成するコマンドを入力し `Get-PSSession` `New-PSSession` ます。

セッションがリモートコンピューターに接続されている場合は、 **ListAvailable** パラメーターを指定する必要があります。

`Get-Module` **Pssession** パラメーターを使用するコマンドは、コマンドレットを使用して `Invoke-Command` `Get-Module -ListAvailable` **pssession** でコマンドを実行することと同じです。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: PsSession
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -更新

このコマンドレットがインストールされているコマンドのキャッシュを更新することを示します。 コマンドのキャッシュは、セッションの開始時に作成されます。 この `Get-Command` コマンドレットを使用すると、セッションにインポートされていないモジュールからコマンドを取得できます。

このパラメーターは、セッションが開始された時点からモジュールの内容が変化する開発およびテスト シナリオ用に用意されています。

コマンドで **Refresh** パラメーターを指定する場合は、 **ListAvailable** を指定する必要があります。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Available, PsSession, CimSession
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipEditionCheck

フィールドのチェックをスキップし `CompatiblePSEditions` ます。

既定では、Get-Module は、 `%windir%\System32\WindowsPowerShell\v1.0\Modules` フィールドにを指定していないディレクトリ内のモジュールを省略 `Core` `CompatiblePSEditions` します。 このスイッチが設定されている場合、 `Core` Powershell Core と互換性のない Windows powershell モジュールパスにあるモジュールが返されるように、を指定せずにモジュールが含まれるようになります。

MacOS と Linux では、このパラメーターは何も行いません。

詳細については、「 [about_PowerShell_Editions](About/about_PowerShell_Editions.md) 」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Available, PsSession, CimSession
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

### System.String

パイプを使用してモジュール名をこのコマンドレットに渡します。

## 出力

### PSModuleInfo (システム管理)

このコマンドレットは、モジュールを表すオブジェクトを返します。
**ListAvailable** パラメーターを指定すると、は `Get-Module` **moduleinfogrouping** オブジェクトを返します。これは、同じプロパティとメソッドを持つ **PSModuleInfo** オブジェクトの一種です。

## 注

- Windows PowerShell 3.0 以降では、PowerShell に含まれるコアコマンドはモジュールにパッケージ化されています。 この例外は、スナップイン (**add-pssnapin**) である、 **PowerShell のコア** です。 既定では、**Microsoft.PowerShell.Core** スナップインのみがセッションに追加されます。 モジュールは初回使用時に自動的にインポートされ、コマンドレットを使用して `Import-Module` インポートできます。

- Windows PowerShell 2.0、およびそれ以降のバージョンの PowerShell で古いスタイルのセッションを作成するホストプログラムでは、コアコマンドはスナップイン (**PSSnapins**) にパッケージ化されます。 例外は、常にスナップインである、 **PowerShell です**。 また、コマンドレットによって開始されるようなリモートセッション `New-PSSession` は、コアスナップインを含む古いスタイルのセッションです。

  コアモジュールで新しいスタイルのセッションを作成する **CreateDefault2** メソッドの詳細については、「 [CreateDefault2 メソッド](/dotnet/api/system.management.automation.runspaces.initialsessionstate.createdefault2)」を参照してください。

- `Get-Module`**PSModulePath** 環境変数の値 ($Env:P SModulePath) に格納されている場所のモジュールのみを取得します。 コマンドレットでは、 `Import-Module` 他の場所にモジュールをインポートできますが、コマンドレットを使用してモジュールを取得することはできません `Get-Module` 。

- また、PowerShell 3.0 以降では、を返すオブジェクトに新しいプロパティが追加されました `Get-Module` 。これにより、モジュールをインポートする前でも簡単に学習できるようになります。 インポートする前に、すべてのプロパティが設定されます。 これには、モジュールによってエクスポートされるコマンドを一覧表示する **ExportedCommands**、 **ExportedCmdlets** 、 **ExportedFunctions** の各プロパティが含まれます。

- **ListAvailable** パラメーターは、適切な形式のモジュールのみを取得します。つまり、基本名がモジュールフォルダーの名前と同じファイルを少なくとも1つ含むフォルダーです。 基本名は、ファイル名拡張子のない名前です。 異なる名前を持つファイルを含むフォルダーは、コンテナーと見なされますが、モジュールではありません。

  DLL ファイルとして実装されていてもモジュールフォルダーには含まれていないモジュールを取得するには、 **ListAvailable** と **All** の両方のパラメーターを指定します。

- CIM セッション機能を使用するには、リモート コンピューターにおいて、WS-Management リモート処理と Common Information Model (CIM) 標準の Microsoft 実装である Windows Management Instrumentation (WMI) が必要です。 さらに、モジュール検出用の WMI プロバイダーまたは同じ基本機能を備えた代替 WMI プロバイダーもコンピューターに必要です。

  CIM セッション機能は、Windows オペレーティングシステムを実行していないコンピューターと PowerShell を搭載している Windows コンピューターで使用できますが、PowerShell リモート処理は有効になっていません。

  また、CIM パラメーターを使用して、PowerShell リモート処理が有効になっているコンピューターから CIM モジュールを取得することもできます。 これには、ローカルコンピューターが含まれます。 ローカルコンピューターに CIM セッションを作成すると、PowerShell は WMI ではなく DCOM を使用してセッションを作成します。

## 関連リンク

[Get-CimSession](../CimCmdlets/Get-CimSession.md)

[New-CimSession](../CimCmdlets/New-CimSession.md)

[about_Modules](About/about_Modules.md)

[Get-PSSession](Get-PSSession.md)

[Import-Module](Import-Module.md)

[Import-PSSession](../Microsoft.PowerShell.Utility/Import-PSSession.md)

[New-PSSession](New-PSSession.md)

[Remove-Module](Remove-Module.md)

[about_PowerShell_Editions](About/about_PowerShell_Editions.md)
