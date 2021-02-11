---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 05/16/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/update-help?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-Help
ms.openlocfilehash: f4aec907a41378bbf49f744b66c5662aa3404beb
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601200"
---
# Update-Help

## 概要
最新のヘルプ ファイルをダウンロードしてコンピューターにインストールします。

## SYNTAX

### パス (既定値)

```
Update-Help [[-Module] <String[]>] [-FullyQualifiedModule <ModuleSpecification[]>]
 [[-SourcePath] <String[]>] [-Recurse] [[-UICulture] <CultureInfo[]>] [-Credential <PSCredential>]
 [-UseDefaultCredentials] [-Force] [-Scope <UpdateHelpScope>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### LiteralPath

```
Update-Help [[-Module] <String[]>] [-FullyQualifiedModule <ModuleSpecification[]>]
 [-LiteralPath <String[]>] [-Recurse] [[-UICulture] <CultureInfo[]>] [-Credential <PSCredential>]
 [-UseDefaultCredentials] [-Force] [-Scope <UpdateHelpScope>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`Update-Help`コマンドレットにより、PowerShell モジュールの最新のヘルプファイルがダウンロードされ、コンピューターにインストールされます。 変更を有効にするには、PowerShell を再起動する必要はありません。 コマンドレットを使用して、 `Get-Help` 新しいヘルプファイルをすぐに表示できます。

`Update-Help` コンピューター上のヘルプファイルのバージョンを確認します。 モジュールのヘルプファイルがない場合、またはヘルプファイルが古くなっている場合は、に `Update-Help` よって最新のヘルプファイルがダウンロードされます。 ヘルプファイルは、インターネットまたはファイル共有からダウンロードしてインストールできます。

パラメーターを指定しない場合、は、 `Update-Help` セッション内のモジュールのヘルプファイルと、更新可能なヘルプをサポートするすべてのインストール済みモジュールのヘルプファイルを更新します。 インストールされているが現在のセッションに読み込まれていないモジュールが含まれます。 PowerShell モジュールは、環境変数に示されている場所に格納され `$env:PSModulePath` ます。 詳しくは、「[about_Updatable_Help](./About/about_Updatable_Help.md)」をご覧ください。

**Module** パラメーターを使用して、特定のモジュールのヘルプファイルを更新できます。 複数の言語およびロケールのヘルプファイルをダウンロードするには、 **UICulture** パラメーターを使用します。

また `Update-Help` 、インターネットに接続されていないコンピューターでを使用することもできます。 まず、コマンドレットを使用して、 `Save-Help` インターネットからヘルプファイルをダウンロードし、インターネットに接続されていないシステムからアクセスできる共有フォルダーに保存します。 次に、の **SourcePath** パラメーターを使用して `Update-Help` 、更新されたヘルプファイルを共有からダウンロードし、コンピューターにインストールします。

PowerShell プロファイルにコマンドレットを追加することで、ヘルプの更新を自動化でき `Update-Help` ます。 既定では、は `Update-Help` 各コンピューターで1日1回だけ実行されます。 1 日に 1 回という制限をオーバーライドするには、**Force** パラメーターを使用します。

この `Update-Help` コマンドレットは、Windows PowerShell 3.0 で導入されました。

> [!IMPORTANT]
> `Update-Help` PowerShell 6.0 以下で管理者特権が必要です。
> PowerShell 6.1 以降では、既定の **スコープ** がに設定さ `CurrentUser` れています。
> PowerShell 6.1 より前では、 **Scope** パラメーターは使用できませんでした。
>
> PowerShell コアモジュールのヘルプファイルを更新するには、コンピューターの Administrators グループのメンバーである必要があります。
>
> Powershell のコアモジュールを含む、PowerShell のインストールディレクトリ () のモジュールのヘルプファイルをダウンロードまたは更新するには、 `$PSHOME\Modules` [ **管理者として実行** ] オプションを使用して powershell を起動します。
> たとえば、`Start-Process pwsh.exe -Verb RunAs` のように指定します。

## 例

### 例 1: すべてのモジュールのヘルプファイルを更新する

コマンドレットは、更新 `Update-Help` 可能なヘルプをサポートするインストール済みモジュールのヘルプファイルを更新します。 ユーザーインターフェイス (UI) カルチャ言語は、オペレーティングシステムで設定されます。

```powershell
Update-Help
```

### 例 2: 指定したモジュールのヘルプファイルを更新する

`Update-Help`コマンドレットは、 **Microsoft. PowerShell** で始まるモジュール名のヘルプファイルのみを更新します。

```powershell
Update-Help -Module Microsoft.PowerShell*
```

### 例 3: さまざまな言語のヘルプファイルを更新する

この `Update-Help` コマンドレットは、すべてのモジュールの日本語 (ja-jp) と英語 (en-us) のヘルプファイルを更新します。

指定した UI カルチャのヘルプファイルがモジュールで提供されていない場合は、モジュールと UI カルチャのエラーメッセージが表示されます。 この例では、エラーメッセージは、モジュールの **Microsoft. PowerShell. ユーティリティ** に対して **ja-jp** ヘルプファイルが見つからなかったことを示しています。

```powershell
Update-Help -UICulture ja-JP, en-US
```

```Output
Update-Help : Failed to update Help for the module(s) 'Microsoft.PowerShell.Utility' with UI culture(s) {ja-JP}
No UI culture was found that matches the following pattern: ja-JP.
```

### 例 4: ファイル共有から複数のコンピューターのヘルプファイルを更新する

この例では、更新されたヘルプファイルをインターネットからダウンロードし、ファイル共有に保存します。 ユーザーの資格情報は、ファイル共有にアクセスして更新プログラムをインストールするためのアクセス許可を持っている必要があります。 ファイル共有を使用すると、ファイアウォールの背後にあるコンピューターや、インターネットに接続されていないコンピューターを更新することができます。

```powershell
Save-Help -DestinationPath \\Server01\Share\PSHelp -Credential Domain01\Admin01
Invoke-Command -ComputerName (Get-Content Servers.txt) -ScriptBlock {
     Update-Help -SourcePath \\Server01\Share\PSHelp -Credential Domain01\Admin01
}
```

この `Save-Help` コマンドは、更新可能なヘルプをサポートするすべてのモジュールの最新のヘルプファイルをダウンロードします。
**DestinationPath** パラメーターを指定すると、ファイル共有にファイルが保存され `\\Server01\Share\PSHelp` ます。 **Credential** パラメーターは、ファイル共有にアクセスする権限を持つユーザーを指定します。

`Invoke-Command`コマンドレットは、 `Update-Help` 複数のコンピューターでリモートコマンドを実行します。 **ComputerName** パラメーターは、 **Servers.txt** ファイルからリモートコンピューターの一覧を取得します。 **ScriptBlock** パラメーターはコマンドを実行 `Update-Help` し、 **SourcePath** パラメーターを使用して、更新されたヘルプファイルを含むファイル共有を指定します。 **Credential** パラメーターは、ファイル共有にアクセスしてリモートコマンドを実行できるユーザーを指定し `Update-Help` ます。

### 例 5: 更新されたヘルプファイルの一覧を取得する

`Update-Help`コマンドレットは、指定されたモジュールのヘルプを更新します。 コマンドレットでは、 **Verbose** 共通パラメーターを使用して、更新されたヘルプファイルの一覧を表示します。 **Verbose** を使用して、特定のモジュールのすべてのヘルプファイルまたはヘルプファイルの出力を表示できます。

**Verbose** パラメーターを指定しない場合、 `Update-Help` コマンドの結果は表示されません。 **Verbose** パラメーターの出力は、ヘルプファイルが更新されたこと、または最新バージョンがインストールされているかどうかを確認するのに役立ちます。

```powershell
Update-Help -Module Microsoft.PowerShell.Utility -Verbose
```

### 例 6: 更新可能なヘルプをサポートするモジュールを検索する

この例では、更新可能なヘルプをサポートするモジュールを一覧表示します。 このコマンドは、モジュールの **Helpinfouri** プロパティを使用して、更新可能なヘルプをサポートするモジュールを識別します。 **Helpinfouri** プロパティには、コマンドレットの実行時にリダイレクトされるアドレスが含まれてい `Update-Help` ます。

```powershell
Get-Module -ListAvailable | Where-Object -Property HelpInfoUri
```

```output
   Directory: C:\program files\powershell\6\Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Manifest   6.1.0.0    CimCmdlets                          Core      {Get-CimAssociatedInstance... }
Manifest   1.2.2.0    Microsoft.PowerShell.Archive        Desk      {Compress-Archive... }
Manifest   6.1.0.0    Microsoft.PowerShell.Diagnostics    Core      {Get-WinEvent, New-WinEvent}

    Directory: C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Manifest   2.0.1.0    Appx                                Core,Desk {Add-AppxPackage, ... }
Script     1.0.0.0    AssignedAccess                      Core,Desk {Clear-AssignedAccess, ... }
Manifest   1.0.0.0    BitLocker                           Core,Desk {Unlock-BitLocker, ... }
```

### 例 7: 更新されたヘルプファイルをインベントリする

この例では、スクリプトによって、 `Get-UpdateHelpVersion.ps1` 各モジュールの更新可能なヘルプファイルとそのバージョン番号のインベントリが作成されます。

このスクリプトは、モジュールの **Helpinfouri** プロパティを使用して、更新可能なヘルプをサポートするモジュールを識別します。 更新可能なヘルプをサポートするモジュールの場合、スクリプトはヘルプ情報ファイル (* helpinfo.xml) を検索して解析し、最新のバージョン番号を検索します。

このスクリプトでは、 **Pscustomobject** いうクラスとハッシュテーブルを使用して、カスタム出力オブジェクトを作成します。

```powershell
# Get-UpdateHelpVersion.ps1
Param(
    [parameter(Mandatory=$False)]
    [String[]]
    $Module
)
$HelpInfoNamespace = @{helpInfo='http://schemas.microsoft.com/powershell/help/2010/05'}

if ($Module) { $Modules = Get-Module $Module -ListAvailable | where {$_.HelpInfoUri} }
else { $Modules = Get-Module -ListAvailable | where {$_.HelpInfoUri} }

foreach ($mModule in $Modules)
{
    $mDir = $mModule.ModuleBase

    if (Test-Path $mdir\*helpinfo.xml)
    {
        $mName=$mModule.Name
        $mNodes = dir $mdir\*helpinfo.xml -ErrorAction SilentlyContinue |
            Select-Xml -Namespace $HelpInfoNamespace -XPath "//helpInfo:UICulture"
        foreach ($mNode in $mNodes)
        {
            $mCulture=$mNode.Node.UICultureName
            $mVer=$mNode.Node.UICultureVersion

            [PSCustomObject]@{"ModuleName"=$mName; "Culture"=$mCulture; "Version"=$mVer}
        }
    }
}
```

```Output
ModuleName                              Culture                                 Version
----------                              -------                                 -------
ActiveDirectory                         en-US                                   3.0.0.0
ADCSAdministration                      en-US                                   3.0.0.0
ADCSDeployment                          en-US                                   3.0.0.0
ADDSDeployment                          en-US                                   3.0.0.0
ADFS                                    en-US                                   3.0.0.0
```

## PARAMETERS

### -Credential

**SourcePath** によって指定されたファイルシステムの場所にアクセスする権限を持つユーザーの資格情報を指定します。 このパラメーターは、コマンドに **SourcePath** または **LiteralPath** パラメーターが使用されている場合に限り有効です。

**Credential** パラメーターを使用すると、 `Update-Help` リモートコンピューター上で **SourcePath** パラメーターを使用してコマンドを実行できます。 明示的な資格情報を指定することにより、リモートコンピューターでコマンドを実行し、アクセス拒否エラーが発生しないか、CredSSP 認証を使用して資格情報を委任することなく、3台目のコンピューター上のファイル共有にアクセスできます。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

このコマンドレットが1日に1回の制限に従っていないことを示し、バージョンチェックをスキップし、1 GB の制限を超えるファイルをダウンロードします。

このパラメーターを指定しないと、は `Update-Help` 24 時間ごとに1回だけ実行されます。 ダウンロードは、モジュールあたり 1 GB の圧縮されていないコンテンツに制限されており、ヘルプファイルはコンピューター上の既存のファイルより新しい場合にのみインストールされます。

1日に1回の制限により、ヘルプファイルをホストするサーバーが保護され、 `Update-Help` 接続やダウンロードが繰り返されるリソースコストを発生させることなく、PowerShell プロファイルにコマンドを追加することが実用的になります。

**Force** パラメーターを使用しないで、複数の UI カルチャ内のモジュールのヘルプを更新するには、次のように、同じコマンドの中にすべての UI カルチャを含めます。

`Update-Help -Module PSScheduledJobs -UICulture en-US, fr-FR, pt-BR`

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

### -FullyQualifiedModule

**Modul特定** のオブジェクトの形式で指定された名前を持つモジュールを指定します。
これらのモジュールについては、「 [modulの通知コンストラクター (Hashtable)](/dotnet/api/microsoft.powershell.commands.modulespecification.-ctor?view=powershellsdk-1.1.0#Microsoft_PowerShell_Commands_ModuleSpecification__ctor_System_Collections_Hashtable_)」の「解説」で説明されています。

たとえば、 **FullyQualifiedModule** パラメーターは、次の形式で指定されたモジュール名を受け入れます。

`@{ModuleName = "modulename"; ModuleVersion = "version_number"}`

または

`@{ModuleName = "modulename"; ModuleVersion = "version_number"; Guid = "GUID"}.`

**ModuleName** と **ModuleVersion** は必須ですが、**Guid** は省略可能です。

**モジュール** パラメーターと同じコマンドで **FullyQualifiedModule** パラメーターを指定することはできません。

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

### -LiteralPath

更新されたヘルプファイルをインターネットからダウンロードするのではなく、そのフォルダーを指定します。 コマンドレットを使用してヘルプファイルをディレクトリにダウンロードした場合は、このパラメーターまたは **SourcePath** を使用し `Save-Help` ます。

またはコマンドレットなどのディレクトリオブジェクトをにパイプライン `Get-Item` でき `Get-ChildItem` `Update-Help` ます。

**SourcePath** の値とは異なり、 **LiteralPath** の値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -モジュール

指定したモジュールのヘルプを更新します。 1つ以上のモジュール名または名前パターンをコンマ区切りのリストで入力するか、各行に1つのモジュール名を一覧表示するファイルを指定します。 ワイルドカード文字を使用できます。 コマンドレットからコマンドレットにモジュールをパイプラインすることができ `Get-Module` `Update-Help` ます。

指定するモジュールはコンピューターにインストールする必要がありますが、現在のセッションにインポートする必要はありません。 セッション内の任意のモジュール、または環境変数に示されている場所にインストールされている任意のモジュールを指定でき `$env:PSModulePath` ます。

(すべて) の値は、 `*` コンピューターにインストールされているすべてのモジュールのヘルプを更新しようとします。
更新可能なヘルプをサポートしていないモジュールが含まれます。 この値は、コマンドで更新可能なヘルプをサポートしていないモジュールが検出された場合にエラーを生成することがあります。 代わりに、 `Update-Help` パラメーターを指定せずにを実行します。

コマンドレットの **module** パラメーターは、 `Update-Help` モジュールファイルまたはモジュールマニフェストファイルの完全なパスを受け入れません。 場所にないモジュールのヘルプを更新するには、 `$env:PSModulePath` コマンドを実行する前に、現在のセッションにモジュールをインポートし `Update-Help` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Name

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -再帰

指定されたディレクトリ内のヘルプファイルに対して再帰的な検索を実行します。 このパラメーターは、コマンドで **SourcePath** パラメーターが使用されている場合にのみ有効です。

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

### -スコープ

ヘルプが更新されるシステムスコープを指定します。 **AllUsers** スコープでの更新には、Windows システムの管理者特権が必要です。 この `-Scope` パラメーターは、PowerShell Core バージョン6.1 で導入されました。

**CurrentUser** は、PowerShell 6.1 以降のヘルプファイルの既定のスコープです。 **AllUsers** は、すべてのユーザーのヘルプをインストールまたは更新するために指定できます。 `sudo`すべてのユーザーのヘルプを更新するには、Unix システムでの特権が必要です。 例: `sudo pwsh -c Update-Help`

指定できる値は次のとおりです。

- CurrentUser
- AllUsers

```yaml
Type: Microsoft.PowerShell.Commands.UpdateHelpScope
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: CurrentUser
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SourcePath

が `Update-Help` インターネットからダウンロードするのではなく、更新されたヘルプファイルを取得するファイルシステムフォルダーを指定します。 フォルダーのパスを入力します。 ファイル名またはファイル名拡張子を指定しないでください。 またはコマンドレットのようなフォルダーをにパイプライン `Get-Item` でき `Get-ChildItem` `Update-Help` ます。

既定では、は、 `Update-Help` 更新されたヘルプファイルをインターネットからダウンロードします。 コマンドレットを使用して、更新されたヘルプファイルをディレクトリにダウンロードした場合は、 **SourcePath** を使用し `Save-Help` ます。

**SourcePath** の既定値を指定するには、[**グループポリシー**]、[**コンピューターの構成**] にアクセスし、[update-help]**の既定のソースパスを設定** します。 このグループポリシー設定は、ユーザーがを使用して `Update-Help` インターネットからヘルプファイルをダウンロードできないようにします。
詳細については、「[about_Group_Policy_Settings](./About/about_Group_Policy_Settings.md)」をご覧ください。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases: Path

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UICulture

が `Update-Help` 更新されたヘルプファイルを取得するために使用する UI カルチャの値を指定します。 **Es**、カルチャオブジェクトを含む変数、 `Get-Culture` またはコマンドやコマンドなどのカルチャオブジェクトを取得するコマンドなど、1つまたは複数の言語コードを入力し `Get-UICulture` ます。 ワイルドカード文字は使用できません。また、 **de** などの部分言語コードを送信することはできません。

既定では、は、 `Update-Help` オペレーティングシステム用に設定された UI カルチャのヘルプファイルを取得します。 **UICulture** パラメーターを指定すると、は `Update-Help` 指定された UI カルチャに対してのみヘルプを検索します。

> [!NOTE]
> Ubuntu 18.04 によって既定のロケール設定がに変更されました `C.UTF.8` 。これは、認識されている UI カルチャではありません。 `Update-Help` このパラメーターを、のようなサポートされているロケールで使用しない限り、サイレントモードでヘルプをダウンロードでき `en-US` ません。 これは、サポートされていない値を使用するプラットフォームで発生する可能性があります。

**UICulture** パラメーターを使用するコマンドは、指定した UI カルチャのヘルプ ファイルをモジュールが提供する場合に限り、成功します。 指定された UI カルチャがサポートされていないためにコマンドが失敗した場合は、エラーメッセージが表示されます。

```yaml
Type: System.Globalization.CultureInfo[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseDefaultCredentials

は、 `Update-Help` 現在のユーザーの資格情報を使用して、インターネットダウンロードなどのコマンドを実行することを示します。 既定では、コマンドは明示的な資格情報がない状態で実行されます。

このパラメーターは、web ダウンロードで NT LAN Manager (NTLM)、negotiate、または Kerberos ベースの認証が使用されている場合にのみ有効です。

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

コマンドレットの実行前に確認を求めるメッセージが表示されます。

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

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

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

### DirectoryInfo

パイプを使用してディレクトリパスをにパイプすることができ `Update-Help` ます。

### PSModuleInfo (システム管理)

パイプを使用して、モジュールオブジェクトをコマンドレットからにパイプすることができ `Get-Module` `Update-Help` ます。

## 出力

### なし

`Update-Help` 出力を生成しません。

## 注

Powershell を使用してインストールされたコマンド、またはディレクトリ内の任意のモジュールを含む PowerShell コアモジュールのヘルプを更新するには、 `$PSHOME\Modules` **管理者として実行** するオプションを使用して powershell を起動します。

PowerShell コアモジュールのヘルプ、PowerShell と共にインストールされるコマンド、およびフォルダー内のモジュールのヘルプを更新できるのは、コンピューターの Administrators グループのメンバーだけ `$PSHOME\Modules` です。 ヘルプファイルを更新するアクセス許可がない場合は、オンラインでヘルプファイルを読むことができます。 たとえば、「 `Get-Help Update-Help -Online` 」のように入力します。

モジュールは、更新可能なヘルプの最小単位です。 特定のコマンドレットのヘルプを更新することはできません。 特定のコマンドレットを含むモジュールを検索するには、コマンドレットの **ModuleName** プロパティを使用し `Get-Command` ます。たとえば、のように `(Get-Command Update-Help).ModuleName` します。

ヘルプファイルはモジュールディレクトリにインストールされるため、 `Update-Help` コマンドレットはコンピューターにインストールされているモジュールに対してのみ、更新されたヘルプファイルをインストールできます。 ただし、この `Save-Help` コマンドレットを使用すると、コンピューターにインストールされていないモジュールのヘルプを保存できます。

に `Update-Help` よって、モジュールの更新されたヘルプファイルが見つからない場合、または指定された言語で更新されたヘルプが見つからない場合は、エラーメッセージを表示せずに、警告なしで続行されます。 状態や進捗状況の詳細を確認するには、**Verbose** パラメーターを使用します。

この `Update-Help` コマンドレットは、Windows PowerShell 3.0 で導入されました。 以前のバージョンの PowerShell では機能しません。 Windows PowerShell 2.0 と Windows PowerShell 3.0 の両方がインストールされているコンピューターでは、 `Update-Help` Windows powershell 3.0 セッションのコマンドレットを使用してヘルプファイルをダウンロードし、更新します。 ヘルプファイルは、Windows PowerShell 2.0 と Windows PowerShell 3.0 の両方で使用できます。

`Update-Help`およびコマンドレットは、次のポートを使用して `Save-Help` ヘルプファイルをダウンロードします。 HTTP の場合はポート80、HTTPS の場合はポート443を使用します。

`Update-Help` では、すべてのモジュールと PowerShell Core スナップインがサポートされています。その他のスナップインはサポートしていません。

環境変数に一覧表示されていない場所にあるモジュールのヘルプを更新するには、 `$env:PSModulePath` 現在のセッションにモジュールをインポートしてから、コマンドを実行し `Update-Help` ます。 `Update-Help`パラメーターを指定せずに実行するか、 **module** パラメーターを使用してモジュール名を指定します。 およびコマンドレットの **module** パラメーターは、 `Update-Help` `Save-Help` モジュールファイルまたはモジュールマニフェストファイルの完全パスを受け入れません。

モジュールはすべて、更新可能なヘルプをサポートできます。 作成したモジュールの更新可能なヘルプをサポートする手順については、「 [更新可能なヘルプのサポート](/powershell/scripting/developer/module/supporting-updatable-help)」を参照してください。

`Update-Help`および `Save-Help` コマンドレットは、Windows Preinstallation Environment (Windows PE) ではサポートされていません。

## 関連リンク

[Get-Culture](../Microsoft.PowerShell.Utility/Get-Culture.md)

[Get-Help](Get-Help.md)

[Get-Module](Get-Module.md)

[Get-UICulture](../Microsoft.PowerShell.Utility/Get-UICulture.md)

[Start-Job](Start-Job.md)

[Save-Help](Save-Help.md)
