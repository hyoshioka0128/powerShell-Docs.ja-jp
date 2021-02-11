---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/save-help?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Save-Help
ms.openlocfilehash: cafc462f7278b41d27c789ebf12d5abf076ff49e
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390645"
---
# Save-Help

## 概要
最新のヘルプ ファイルをダウンロードしてファイル システム ディレクトリに保存します。

## SYNTAX

### パス (既定値)

```
Save-Help [-DestinationPath] <String[]> [[-Module] <PSModuleInfo[]>]
 [-FullyQualifiedModule <ModuleSpecification[]>] [[-UICulture] <CultureInfo[]>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-Force] [-Scope <UpdateHelpScope>]
 [<CommonParameters>]
```

### LiteralPath

```
Save-Help -LiteralPath <String[]> [[-Module] <PSModuleInfo[]>]
 [-FullyQualifiedModule <ModuleSpecification[]>] [[-UICulture] <CultureInfo[]>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-Force] [-Scope <UpdateHelpScope>]
 [<CommonParameters>]
```

## Description

`Save-Help`コマンドレットは、PowerShell モジュールの最新のヘルプファイルをダウンロードし、指定したディレクトリに保存します。 この機能を使用すると、インターネットにアクセスできないコンピューター上のヘルプ ファイルを更新でき、複数のコンピューター上のヘルプ ファイルを更新しやすくなります。

Windows PowerShell 3.0 では、 `Save-Help` ローカルコンピューターにインストールされているモジュールに対してのみ動作しました。 リモートコンピューターからモジュールをインポートしたり、PowerShell リモート処理を使用してリモートコンピューターから **PSModuleInfo** オブジェクトへの参照を取得したりすることはできましたが、 **Helpinfouri** プロパティは保持されず、 `Save-Help` リモートモジュールのヘルプでは機能しません。

Windows PowerShell 4.0 では、 **Helpinfouri** プロパティが PowerShell リモート処理によって保持されます。これにより、 `Save-Help` リモートコンピューターにインストールされているモジュールでを使用できるようになります。 また、インターネットにアクセスできないコンピューターでを実行して、 **PSModuleInfo** オブジェクトをディスクまたはリムーバブルメディアに保存することもでき `Export-Clixml` ます。その場合、インターネットにアクセスできるコンピューターにオブジェクトをインポートし、 `Save-Help` **PSModuleInfo** オブジェクトでを実行します。 保存されたヘルプは、USB ドライブなどのリムーバブル記憶域メディアを使用してリモートコンピューターに転送できます。 を実行して、リモートコンピューターにヘルプをインストールでき `Update-Help` ます。 このプロセスを使用すれば、どのようなネットワーク アクセスも実行できないコンピューターにヘルプをインストールできます。

保存されたヘルプファイルをインストールするには、コマンドレットを実行し `Update-Help` ます。 その **SourcePath** パラメーターを追加して、ヘルプファイルを保存したフォルダーを指定します。

パラメーターを指定しない場合、コマンドは、 `Save-Help` セッション内のすべてのモジュールの最新のヘルプと、 **PSModulePath** 環境変数に示されている場所にコンピューターにインストールされているモジュールの最新のヘルプをダウンロードします。 この操作により、更新可能なヘルプをサポートしていないモジュールは警告なしにスキップされます。

コマンドレットでは、 `Save-Help` 対象フォルダー内のヘルプファイルのバージョンを確認します。 新しいヘルプファイルが使用可能な場合、このコマンドレットは、インターネットから最新のヘルプファイルをダウンロードし、フォルダーに保存します。 `Save-Help`コマンドレットはコマンドレットと同じように動作し `Update-Help` ますが、ダウンロードされたキャビネット (.cab) ファイルを保存し、コンピューターにインストールするのではなく、ダウンロードしたキャビネット (.cab) ファイルを保存する点が異なります。

保存されたモジュール用ヘルプはそれぞれ、UI カルチャごとに、ヘルプ情報 (HelpInfo XML) ファイル 1 つと、ヘルプ ファイルのキャビネット (.cab) ファイル 1 つで構成されます。 キャビネットファイルからヘルプファイルを抽出する必要はありません。 `Update-Help`コマンドレットは、ヘルプファイルを抽出し、XML の安全性を検証した後、モジュールフォルダーの言語固有のサブフォルダーにヘルプファイルとヘルプ情報ファイルをインストールします。

PowerShell のインストールフォルダー () にモジュールのヘルプファイルを保存するに `$pshome\Modules` は、[管理者として実行] オプションを使用して powershell を起動します。 これらのモジュールのヘルプ ファイルをダウンロードするには、そのコンピューターの Administrators グループのメンバーである必要があります。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: DhcpServer モジュールのヘルプを保存する

```powershell
# Option 1: Run Invoke-Command to get the PSModuleInfo object for the remote DHCP Server module,
# save the PSModuleInfo object in the variable $m, and then run Save-Help.

$m = Invoke-Command -ComputerName RemoteServer -ScriptBlock { Get-Module -Name DhcpServer -ListAvailable }
Save-Help -Module $m -DestinationPath "C:\SavedHelp"


# Option 2: Open a PSSession--targeted at the remote computer that is running the DhcpServer
# module--to get the PSModuleInfo object for the remote module, and then run Save-Help.

$s = New-PSSession -ComputerName "RemoteServer"
$m = Get-Module -PSSession $s -Name "DhcpServer" -ListAvailable
Save-Help -Module $m -DestinationPath "C:\SavedHelp"


# Option 3: Open a CIM session--targeted at the remote computer that is running the DhcpServer
# module--to get the PSModuleInfo object for the remote module, and then run Save-Help.

$c = New-CimSession -ComputerName "RemoteServer"
$m = Get-Module -CimSession $c -Name "DhcpServer" -ListAvailable
Save-Help -Module $m -DestinationPath "C:\SavedHelp"
```

この例では、を使用し `Save-Help` て、ローカルコンピューターに **DhcpServer** モジュールまたは DHCP サーバーの役割をインストールせずに、インターネットに接続されたクライアントコンピューターから **DhcpServer** モジュールのヘルプを保存する3つの方法を示します。

### 例 2: DhcpServer モジュールのヘルプをインストールする

```powershell
# First, run Export-CliXml to export the PSModuleInfo object to a shared folder or to removable media.

$m = Get-Module -Name "DhcpServer" -ListAvailable
Export-CliXml -Path "E:\UsbFlashDrive\DhcpModule.xml" -InputObject $m

# Next, transport the removable media to a computer that has Internet access, and then import the
# PSModuleInfo object with Import-CliXml. Run Save-Help to save the Help for the imported DhcpServer
# module PSModuleInfo object.

$deserialized_m = Import-CliXml "E:\UsbFlashDrive\DhcpModule.xml"
Save-Help -Module $deserialized_m -DestinationPath "E:\UsbFlashDrive\SavedHelp"

# Finally, transport the removable media back to the computer that does not have network access, and
# then install the help by running Update-Help.

Update-Help -Module DhcpServer -SourcePath "E:\UsbFlashDrive\SavedHelp"
```

この例では、例1でインターネットにアクセスできないコンピューターの **DhcpServer** モジュールのヘルプをインストールする方法を示します。

### 例 3: すべてのモジュールのヘルプを保存する

```powershell
Save-Help -DestinationPath "\\Server01\FileShare01"
```

このコマンドは、ローカル コンピューター上の Windows に設定された UI カルチャに該当するすべてのモジュールの最新のヘルプ ファイルをダウンロードします。 ヘルプファイルはフォルダーに保存され `\\Server01\Fileshare01` ます。

### 例 4: コンピューターのモジュールのヘルプを保存する

```powershell
Save-Help -Module ServerManager -DestinationPath "\\Server01\FileShare01" -Credential Domain01/Admin01
```

このコマンドは、 **ServerManager** モジュールの最新のヘルプファイルをダウンロードし、フォルダーに保存し `\\Server01\Fileshare01` ます。

モジュールをコンピューターにインストールする際、モジュールが現在のセッションにインポートされていない場合でも、モジュール名を **Module** パラメーターの値として入力できます。

このコマンドは、 **Credential** パラメーターを使用して、ファイル共有への書き込みアクセス許可を持つユーザーの資格情報を指定します。

### 例 5: 別のコンピューターにモジュールのヘルプを保存する

```powershell
Invoke-Command -ComputerName Server02 {Get-Module -Name CustomSQL -ListAvailable} | Save-Help -DestinationPath \\Server01\FileShare01 -Credential Domain01\Admin01
```

これらのコマンドは、 **Customsql** モジュールの最新のヘルプファイルをダウンロードし、フォルダーに保存し `\\Server01\Fileshare01` ます。

**Customsql** モジュールがコンピューターにインストールされていないため、このシーケンスには、 `Invoke-Command` Server02 コンピューターから customsql モジュールのモジュールオブジェクトを取得し、そのモジュールオブジェクトをコマンドレットにパイプするコマンドが含まれてい `Save-Help` ます。

コンピューターにモジュールがインストールされていない場合、には `Save-Help` モジュールオブジェクトが必要です。これには、最新のヘルプファイルの場所に関する情報が含まれます。

### 例 6: モジュールのヘルプを複数の言語で保存する

```powershell
Save-Help -Module Microsoft.PowerShell* -UICulture de-DE, en-US, fr-FR, ja-JP -DestinationPath "D:\Help"
```

このコマンドは、4つの異なる UI カルチャの PowerShell コアモジュールのヘルプを保存します。 これらのロケールの言語パックは、コンピューターにインストールされている必要はありません。

`Save-Help` では、モジュールの所有者が翻訳されたファイルをインターネットで使用できるようにする場合にのみ、さまざまな UI カルチャのモジュールのヘルプファイルをダウンロードできます。

### 例 7: 1 日に複数回ヘルプを保存する

```powershell
Save-Help -Force -DestinationPath "\\Server3\AdminShare\Help"
```

このコマンドは、コンピューターにインストールされているすべてのモジュールのヘルプを保存します。 このコマンドは、 **Force** コマンド `Save-Help` レットが24時間ごとに複数回ダウンロードできないようにするルールを上書きする Force パラメーターを指定します。

**Force** パラメーターは、1 GB の制限もオーバーライドし、バージョンのチェックを回避します。
そのため、対象のフォルダーのバージョンよりも前のバージョンであっても、ファイルをダウンロードできます。

コマンドは、コマンドレットを使用して、 `Save-Help` 指定したフォルダーにヘルプファイルをダウンロードして保存します。
**Force** パラメーターは、1日に複数回コマンドを実行する必要がある場合に必要です `Save-Help` 。

## PARAMETERS

### -Credential

ユーザー資格情報を指定します。 このコマンドレットは、 **DestinationPath** パラメーターで指定したファイルシステムの場所にアクセスする権限を持つユーザーの資格情報を使用して、コマンドを実行します。 このパラメーターは、コマンドで **DestinationPath** パラメーターまたは **LiteralPath** パラメーターが使用されている場合にのみ有効です。

このパラメーターを使用すると `Save-Help` 、リモートコンピューターで **DestinationPath** パラメーターを使用するコマンドを実行できます。 明示的な資格情報を指定することにより、リモートコンピューターでコマンドを実行し、アクセス拒否エラーが発生しないか、CredSSP 認証を使用して資格情報を委任することなく、3台目のコンピューター上のファイル共有にアクセスできます。

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
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DestinationPath

ヘルプファイルを保存するフォルダーのパスを指定します。 ファイル名やファイル名拡張子を指定しないでください。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases: Path

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

このコマンドレットが1日に1回の制限に従っていないことを示し、バージョンチェックをスキップし、1 GB の制限を超えるファイルをダウンロードします。

このパラメーターを指定しない場合、各 `Save-Help` モジュールに対して24時間ごとに1つのコマンドのみが許可されます。ダウンロードはモジュールごとに 1 GB の非圧縮コンテンツに制限され、モジュールのヘルプファイルはコンピューター上のファイルよりも新しい場合にのみインストールされます。

1日に1回の制限により、ヘルプファイルをホストするサーバーが保護され、PowerShell プロファイルにコマンドを追加できるようになり `Save-Help` ます。

**Force** パラメーターを指定せずに複数の ui カルチャのモジュールのヘルプを保存するには、次のように、すべての ui カルチャを同じコマンドに含めます。`Save-Help -Module PSScheduledJobs -UICulture en-US, fr-FR, pt-BR`

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

### -FullyQualifiedModule

**Modul特定** のオブジェクトの形式で指定された名前を持つモジュールを指定します。 「 [Modulの認識コンストラクター (Hashtable)](/dotnet/api/microsoft.powershell.commands.modulespecification.-ctor#Microsoft_PowerShell_Commands_ModuleSpecification__ctor_System_Collections_Hashtable_)」の「解説」を参照してください。

たとえば、 **FullyQualifiedModule** パラメーターは、次のいずれかの形式で指定されたモジュール名を受け入れます。

- `@{ModuleName = "modulename"; ModuleVersion = "version_number"}`
- `@{ModuleName = "modulename"; ModuleVersion = "version_number"; Guid = "GUID"}`

**ModuleName** と **ModuleVersion** は必須ですが、 **Guid** は省略可能です。 **モジュール** パラメーターと同じコマンドで **FullyQualifiedModule** パラメーターを指定することはできません。 2つのパラメーターは相互に排他的です。

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

コピー先フォルダーのパスを指定します。 **DestinationPath** パラメーターの値とは異なり、 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -モジュール

このコマンドレットがヘルプをダウンロードするモジュールを指定します。 1つ以上のモジュール名または名前のパターンを、コンマ区切りのリスト、または各行に1つのモジュール名を持つファイルに入力します。 ワイルドカード文字を使用できます。 パイプを使用して、モジュールオブジェクトをコマンドレットからにパイプすることもでき `Get-Module` `Save-Help` ます。

既定では、は `Save-Help` 更新可能なヘルプをサポートし、 **PSModulePath** 環境変数に示されている場所にローカルコンピューターにインストールされているすべてのモジュールのヘルプをダウンロードします。

コンピューターにインストールされていないモジュールのヘルプを保存するには、 `Get-Module` リモートコンピューターでコマンドを実行します。 次に、結果として得られるモジュールオブジェクトをコマンドレットにパイプするか、モジュール `Save-Help` オブジェクトを **モジュール** または **InputObject** パラメーターの値として送信します。

指定したモジュールがコンピューターにインストールされている場合は、モジュール名またはモジュール オブジェクトを入力できます。 モジュールがコンピューターにインストールされていない場合は、コマンドレットによって返されるようなモジュールオブジェクトを入力する必要があり `Get-Module` ます。

コマンドレットの **module** パラメーターは、 `Save-Help` モジュールファイルまたはモジュールマニフェストファイルの完全なパスを受け入れません。 **PSModulePath** の場所にないモジュールのヘルプを保存するには、コマンドを実行する前に、現在のセッションにモジュールをインポートし `Save-Help` ます。

"*" (すべて) の値は、コンピューターにインストールされているすべてのモジュールのヘルプを更新しようとします。
これには、更新可能なヘルプをサポートしていないモジュールも含まれます。 この値は、更新可能なヘルプをサポートしていないモジュールがコマンドによって検出された場合にエラーを生成することがあります。

```yaml
Type: System.Management.Automation.PSModuleInfo[]
Parameter Sets: (All)
Aliases: Name

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -UICulture

このコマンドレットが更新されたヘルプファイルを取得する UI カルチャの値を指定します。 1つまたは複数の言語コードを入力し `es-ES` ます。たとえば、カルチャオブジェクトを含む変数や、またはコマンドなどのカルチャオブジェクトを取得するコマンドを入力し `Get-Culture` `Get-UICulture` ます。

ワイルドカード文字は使用できません。 "De" などの部分言語コードを指定しないでください。

既定で `Save-Help` は、は、Windows またはそのフォールバックカルチャに設定されている UI カルチャのヘルプファイルを取得します。
**UICulture** パラメーターを指定すると、は、 `Save-Help` フォールバックカルチャではなく、指定された UI カルチャに対してのみヘルプを検索します。

```yaml
Type: System.Globalization.CultureInfo[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: Current UI culture
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseDefaultCredentials

このコマンドレットが、web ダウンロードなどのコマンドを、現在のユーザーの資格情報を使用して実行することを示します。 既定では、コマンドは明示的な資格情報がない状態で実行されます。

このパラメーターは、Web ダウンロードが、NTLM、Negotiate、または Kerberos ベース認証を使用する場合に限り有効です。

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

### -スコープ

このパラメーターは、このコマンドレットでは何も行いません。

```yaml
Type: Microsoft.PowerShell.Commands.UpdateHelpScope
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### PSModuleInfo (システム管理)

パイプを使用して、モジュールオブジェクトを `Get-Module` コマンドレットからの **module** パラメーターに渡し `Save-Help` ます。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

- モジュールのヘルプを [$pshome] \ [モジュール] フォルダーに保存するには、[管理者として実行] オプションを使用して PowerShell を起動します。 コンピューターの Administrators グループのメンバーだけが、$pshome \ modules フォルダー内のモジュールのヘルプをダウンロードできます。
- 保存されたモジュール用ヘルプはそれぞれ、UI カルチャごとに、ヘルプ情報 (HelpInfo XML) ファイル 1 つと、ヘルプ ファイルのキャビネット (.cab) ファイル 1 つで構成されます。 キャビネットファイルからヘルプファイルを抽出する必要はありません。 コマンドレットにより、 `Update-Help` ヘルプファイルが抽出され、XML が検証された後、モジュールフォルダーの言語固有のサブフォルダーにヘルプファイルとヘルプ情報ファイルがインストールされます。
- コマンドレットでは、 `Save-Help` コンピューターにインストールされていないモジュールのヘルプを保存できます。 ただし、ヘルプファイルはモジュールフォルダーにインストールされるため、 `Update-Help` コマンドレットでは、コンピューターにインストールされているモジュールに対してのみ更新されたヘルプファイルをインストールできます。
- に `Save-Help` よって、モジュールの更新されたヘルプファイルが見つからない場合、または指定された言語で更新されたヘルプファイルが見つからない場合は、エラーメッセージを表示せずに、警告なしで続行されます。 コマンドによって保存されたファイルを確認するには、 **Verbose** パラメーターを指定します。
- モジュールは、更新可能なヘルプの最小単位です。 特定のコマンドレットのヘルプは保存できません。モジュール内のすべてのコマンドレットに対してのみ使用できます。 特定のコマンドレットを含むモジュールを見つけるには、 **ModuleName** プロパティをコマンドレットと共に使用します。次に例を示します。 `Get-Command``(Get-Command \<cmdlet-name\>).ModuleName`
- `Save-Help` では、すべてのモジュールと PowerShell Core スナップインがサポートされています。その他のスナップインはサポートしていません。
- `Update-Help`およびコマンドレットは、次のポートを使用して `Save-Help` ヘルプファイルをダウンロードします。 HTTP の場合はポート80、HTTPS の場合はポート443を使用します。
- `Update-Help`および `Save-Help` コマンドレットは、Windows プレインストール環境 (Windows PE) ではサポートされていません。

## 関連リンク

[Get-Help](Get-Help.md)

[Get-Module](Get-Module.md)

[Update-Help](Update-Help.md)
