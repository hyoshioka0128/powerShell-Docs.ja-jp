---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/23/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-pssession?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-PSSession
ms.openlocfilehash: 5d5841720c6187863902a929632e15d1687685e1
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94389404"
---
# Export-PSSession

## 概要

別のセッションからコマンドをエクスポートし、PowerShell モジュールに保存します。

## SYNTAX

### すべて

```
Export-PSSession [-OutputModule] <String> [-Force] [-Encoding <Encoding>]
 [[-CommandName] <String[]>] [-AllowClobber] [-ArgumentList <Object[]>]
 [-CommandType <CommandTypes>] [-Module <String[]>] [-FullyQualifiedModule <ModuleSpecification[]>]
 [[-FormatTypeName] <String[]>] [-Certificate <X509Certificate2>] [-Session] <PSSession>
 [<CommonParameters>]
```

## Description

コマンド `Export-PSSession` レットは、ローカルコンピューターまたはリモートコンピューター上の別の powershell セッション (PSSession) からコマンドレット、関数、エイリアス、およびその他のコマンドの種類を取得し、powershell モジュールに保存します。 モジュールのコマンドを現在のセッションに追加するには、コマンドレットを使用し `Import-Module` ます。

`Import-PSSession`別の PSSession から現在のセッションにコマンドをインポートするとは異なり、は `Export-PSSession` モジュールにコマンドを保存します。 コマンドは、現在のセッションにはインポートされません。

コマンドをエクスポートするには、コマンドレットを使用して、 `New-PSSession` エクスポートするコマンドを含む PSSession を作成します。 次に、 `Export-PSSession` コマンドレットを使用してコマンドをエクスポートします。

コマンド名の競合を防ぐため、の既定で `Export-PSSession` は、現在のセッションに存在するコマンドを除き、すべてのコマンドがエクスポートされます。 **CommandName** パラメーターを使用して、エクスポートするコマンドを指定できます。

`Export-PSSession`コマンドレットでは、PowerShell の暗黙的なリモート処理機能を使用します。 現在のセッションにコマンドをインポートすると、元のセッションまたは元のコンピューターの同様のセッションで、コマンドが暗黙的に実行されます。

## 例

### 例 1: PSSession からコマンドをエクスポートする

この例では、ローカルコンピューターから Server01 コンピューターに新しい PSSession を作成します。 現在のセッションに存在するコマンドを除くすべてのコマンドは、ローカルコンピューター上の Server01 という名前のモジュールにエクスポートされます。 エクスポートには、コマンドの書式設定データが含まれています。

```powershell
$S = New-PSSession -ComputerName Server01
Export-PSSession -Session $S -OutputModule Server01
```

この `New-PSSession` コマンドは、Server01 コンピューター上に PSSession を作成します。 PSSession は変数に格納され `$S` ます。 この `Export-PSSession` コマンドは、 `$S` 変数のコマンドをエクスポートし、Server01 モジュールにデータを書式設定します。

### 例 2: Get コマンドと Set コマンドをエクスポートする

この例では、 `Get` サーバーからすべてのコマンドとコマンドをエクスポートし `Set` ます。

```powershell
$S = New-PSSession -ConnectionUri https://exchange.microsoft.com/mailbox -Credential exchangeadmin01@hotmail.com -Authentication Negotiate
Export-PSSession -Session $S -Module exch* -CommandName Get-*, Set-* -FormatTypeName * -OutputModule $PSHOME\Modules\Exchange -Encoding ASCII
```

これらのコマンドは `Get` 、 `Set` リモートコンピューター上の Microsoft exchange Server スナップインからローカルコンピューター上のディレクトリの Exchange モジュールに、コマンドとコマンドをエクスポートし `$PSHOME\Modules` ます。
ディレクトリにモジュールを配置 `$PSHOME\Modules` すると、コンピューターのすべてのユーザーがそのモジュールにアクセスできるようになります。

### 例 3: リモートコンピューターからコマンドをエクスポートする

この例では、リモートコンピューター上の PSSession からコマンドレットをエクスポートし、ローカルコンピューター上のモジュールに保存します。 モジュールのコマンドレットが現在のセッションに追加され、使用できるようになります。

```powershell
$S = New-PSSession -ComputerName Server01 -Credential Server01\User01
Export-PSSession -Session $S -OutputModule TestCmdlets -Type Cmdlet -CommandName *test* -FormatTypeName *
Remove-PSSession $S
Import-Module TestCmdlets
Get-Help Test*
Test-Files
```

`New-PSSession`このコマンドは、Server01 コンピューター上に PSSession を作成し、変数に保存し `$S` ます。 コマンドは、 `Export-PSSession` の PSSession から `$S` ローカルコンピューター上の testcmdlets モジュールへのテストで名前が始まるコマンドレットをエクスポートします。

`Remove-PSSession`コマンドレットでは、の PSSession を `$S` 現在のセッションから削除します。 このコマンドは、セッションからインポートされたコマンドを使用するために PSSession をアクティブにする必要がないことを示しています。 コマンドレットでは、 `Import-Module` testcmdlets モジュール内のコマンドレットを現在のセッションに追加します。 コマンドは、任意のセッションでいつでも実行できます。

`Get-Help`コマンドレットは、名前が Test で始まるコマンドレットのヘルプを取得します。 モジュール内のコマンドを現在のセッションに追加した後は、コマンドレットとコマンドレットを使用して、インポートした `Get-Help` `Get-Command` コマンドについて学習できます。 `Test-Files`コマンドレットが Server01 コンピューターからエクスポートされ、セッションに追加されました。 `Test-Files`コマンドレットは、コマンドがインポートされたコンピューター上のリモートセッションで実行されます。 PowerShell は、TestCmdlets モジュールに格納されている情報からセッションを作成します。

### 例 4: 現在のセッションでコマンドを Export および上書きする

この例では、変数に格納されているコマンドを現在のセッションにエクスポートします。

```powershell
Export-PSSession -Session $S -AllowClobber -OutputModule AllCommands
```

この `Export-PSSession` コマンドは、変数内の PSSession から現在のセッションにすべてのコマンドおよびすべての書式設定データをエクスポートし `$S` ます。 **AllowClobber** パラメーターには、現在のセッションのコマンドと同じ名前のコマンドが含まれています。

### 例 5: 閉じた PSSession からコマンドをエクスポートする

この例では、エクスポートされたコマンドを作成した PSSession が閉じているときに、特別なオプションを使用してエクスポートされたコマンドを実行する方法を示します。

モジュールをインポートするときに元のリモートセッションが閉じている場合、モジュールは、元のコンピューターに接続している開いているリモートセッションを使用します。 発信元のコンピューターに現在のセッションがない場合は、モジュールによってセッションが再確立されます。

リモートセッションで特別なオプションを指定してエクスポートされたコマンドを実行するには、モジュールをインポートする前に、これらのオプションを使用してリモートセッションを作成する必要があります。 `New-PSSession` **Sessionoption** パラメーターを指定してコマンドレットを使用する

```powershell
$Options = New-PSSessionOption -NoMachineProfile
$S = New-PSSession -ComputerName Server01 -SessionOption $Options
Export-PSSession -Session $S -OutputModule Server01
Remove-PSSession $S
New-PSSession -ComputerName Server01 -SessionOption $Options
Import-Module Server01
```

コマンドレットでは、 `New-PSSessionOption` **Pssessionoption** オブジェクトを作成し、そのオブジェクトを変数に保存し `$Options` ます。 この `New-PSSession` コマンドは、Server01 コンピューター上に PSSession を作成します。
**Sessionoption** パラメーターは、に格納されているオブジェクトを使用し `$Options` ます。 セッションは変数に格納され `$S` ます。

コマンド `Export-PSSession` レットは、の PSSession から `$S` Server01 モジュールにコマンドをエクスポートします。
この `Remove-PSSession` コマンドレットは、変数内の PSSession を削除し `$S` ます。

コマンドレットでは、 `New-PSSession` Server01 コンピューターに接続する新しい PSSession を作成します。 **Sessionoption** パラメーターは、に格納されているオブジェクトを使用し `$Options` ます。 `Import-Module`コマンドレットは、Server01 モジュールからコマンドをインポートします。 モジュール内のコマンドは、Server01 コンピューター上の PSSession で実行されます。

## PARAMETERS

### -AllowClobber

現在のセッションのコマンドと同じ名前でも、指定されたコマンドをエクスポートします。

現在のセッションのコマンドと同じ名前のコマンドをエクスポートすると、エクスポートされたコマンドは元のコマンドを非表示にしたり置き換えたりします。 詳細については、「[about_Command_Precedence](../Microsoft.PowerShell.Core/About/about_Command_Precedence.md)」(コマンドの優先順位について) を参照してください。

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

### -ArgumentList

指定された引数 (パラメーター値) を使用して生成されたコマンドのバリアントをエクスポートします。

たとえば、証明書内のコマンドのバリアントをエクスポートするには `Get-Item` (Cert:)の PSSession のドライブに `$S` 、「」と入力 `Export-PSSession -Session $S -Command Get-Item -ArgumentList cert:` します。

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

### -証明書

を作成するモジュールで、フォーマットファイル (* .Format.ps1xml) またはスクリプトモジュールファイル (hbase-runner.psm1) の署名に使用されるクライアント証明書を指定し `Export-PSSession` ます。 証明書が格納されている変数を入力するか、証明書を取得するコマンドまたは式を入力します。

証明書を検索するには、コマンドレットを使用する `Get-PfxCertificate` か、 `Get-ChildItem` 証明書 (Cert:) のコマンドレットを使用します。駆動. 証明書が無効な場合、または証明書に十分な権限がない場合、コマンドは失敗します。

```yaml
Type: System.Security.Cryptography.X509Certificates.X509Certificate2
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CommandName

指定した名前または名前パターンのコマンドのみをエクスポートします。 ワイルドカードを使用できます。 **CommandName** またはそのエイリアスである **Name** を使用します。

既定では、は、 `Export-PSSession` 現在のセッションのコマンドと同じ名前を持つコマンドを除き、PSSession からすべてのコマンドをエクスポートします。 これにより、現在のセッションのコマンドによってコマンドが非表示になったり置き換えられたりするのを防ぐことができます。 他のコマンドを表示または置換するコマンドも含めて、すべてのコマンドをエクスポートするには、 **AllowClobber** パラメーターを使用します。

**CommandName** パラメーターを使用する場合、 **formattypename** パラメーターを使用しない限り、コマンドの書式設定ファイルはエクスポートされません。 同様に、 **Formattypename** パラメーターを使用する場合、 **CommandName** パラメーターを使用しない限り、コマンドはエクスポートされません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Name

Required: False
Position: 2
Default value: All commands in the session.
Accept pipeline input: False
Accept wildcard characters: True
```

### -CommandType

指定された型のコマンド オブジェクトのみをエクスポートします。 **CommandType** またはそのエイリアスの **Type** を使用します。

このパラメーターに指定できる値は次のとおりです。

- エイリアス. 現在のセッションのすべての PowerShell エイリアス。
- すべて。 すべてのコマンドの型。 これは、と同じです `Get-Command -Name *` 。
- アプリケーション をクリックします。 Path 環境変数 () に一覧表示されているパス内の PowerShell ファイル以外のすべてのファイル ( `$env:path` .txt、.exe、.dll ファイルなど)。
- コマンドレット. 現在のセッションのコマンドレット。 コマンドレットが既定値です。
- 構成。 PowerShell 構成。 詳細については、「 [about_Session_Configurations](../Microsoft.PowerShell.Core/About/about_Session_Configurations.md)」を参照してください。
- ExternalScript。 Path 環境変数 () に示されているパス内のすべての ps1 ファイル。 `$env:path`
- フィルターと関数。 すべての PowerShell 関数。
- スクリプティング。 現在のセッションのスクリプト ブロック。
- 稟議. PowerShell ワークフロー。 詳細については、「 [about_Workflows](/powershell/module/PSWorkflow/About/about_Workflows)」を参照してください。

```yaml
Type: System.Management.Automation.CommandTypes
Parameter Sets: (All)
Aliases: Type
Accepted values: Alias, All, Application, Cmdlet, Configuration, ExternalScript, Filter, Function, Script, Workflow

Required: False
Position: Named
Default value: All commands in the session.
Accept pipeline input: False
Accept wildcard characters: False
```

### -Encoding

ターゲット ファイルのエンコードの種類を指定します。 既定値は `utf8NoBOM` です。

このパラメーターに指定できる値は次のとおりです。

- `ascii`: ASCII (7 ビット) 文字セットのエンコーディングを使用します。
- `bigendianunicode`: ビッグエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `bigendianutf32`: ビッグエンディアンのバイト順を使用して、32 UTF-8 形式でエンコードします。
- `oem`: MS-DOS およびコンソールプログラムの既定のエンコードを使用します。
- `unicode`: リトルエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `utf7`: UTF-7 形式でエンコードします。
- `utf8`: UTF-8 形式でエンコードします。
- `utf8BOM`: バイト順マーク (BOM) を使用して UTF-8 形式でエンコードします。
- `utf8NoBOM`: バイトオーダーマーク (BOM) を使用せずに UTF-8 形式でエンコードします。
- `utf32`:32 UTF-8 形式でエンコードします。

PowerShell 6.2 以降では、 **Encoding** パラメーターを使用して、登録されているコードページの数値 id (など) `-Encoding 1251` や、登録されているコードページの文字列名 (など) を使用することもでき `-Encoding "windows-1251"` ます。 詳細については、 [コードページ](/dotnet/api/system.text.encoding.codepage?view=netcore-2.2)の .net ドキュメントを参照してください。

```yaml
Type: System.Text.Encoding
Parameter Sets: (All)
Aliases:
Accepted values: ASCII, BigEndianUnicode, BigEndianUTF32, OEM, Unicode, UTF7, UTF8, UTF8BOM, UTF8NoBOM, UTF32

Required: False
Position: Named
Default value: UTF8NoBOM
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ファイルが読み取り専用属性を持つ場合でも、1 つ以上の既存の出力ファイルを上書きします。

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

### -FormatTypeName

指定された Microsoft .NET Framework 型の書式設定命令をエクスポートします。 型名を入力します。 既定では、は、 `Export-PSSession` .NET Framework の名前空間に含まれてい **System.Management.Automation** ないすべての型の書式設定命令をエクスポートします。

このパラメーターの値は、コマンドがインポートされるセッション内のコマンドによって返される型の名前である必要があり `Get-FormatData` ます。 リモートセッションのすべての書式設定データを取得するには、「」と入力 `*` します。

**Formattypename** パラメーターを使用する場合、 **CommandName** パラメーターを使用しない限り、コマンドはエクスポートされません。

**CommandName** パラメーターを使用する場合、 **formattypename** パラメーターを使用しない限り、コマンドの書式設定ファイルはエクスポートされません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
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
Accept pipeline input: False
Accept wildcard characters: False
```

### -モジュール

指定した PowerShell スナップインおよびモジュールのコマンドのみをエクスポートします。 スナップインとモジュールの名前を入力します。 ワイルドカードは使用できません。

詳細については、「」および about_PSSnapins を参照してください `Import-Module` 。 [about_PSSnapins](/powershell/module/microsoft.powershell.core/about/about_pssnapins?view=powershell-5.1)

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PSSnapin

Required: False
Position: Named
Default value: All commands in the session.
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutputModule

によって作成されたモジュールのパスと名前 (省略可能) を指定し `Export-PSSession` ます。 既定のパスは、`$home\Documents\WindowsPowerShell\Modules` です。 このパラメーターは必須です。

モジュールサブディレクトリまたはが作成したファイルが `Export-PSSession` 既に存在する場合、コマンドは失敗します。 既存のファイルを上書きするには、 **Force** パラメーターを使用します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: PSPath, ModuleName

Required: True
Position: 1
Default value: $home\Documents\WindowsPowerShell\Modules
Accept pipeline input: False
Accept wildcard characters: False
```

### -セッション

コマンドのエクスポート元の PSSession を指定します。 セッションオブジェクトを含む変数、またはコマンドなどのセッションオブジェクトを取得するコマンドを入力し `Get-PSSession` ます。 このパラメーターは必須です。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してオブジェクトをにパイプすることはできません `Export-PSSession` 。

## 出力

### システム. IO. FileInfo

`Export-PSSession` 作成したモジュールを構成するファイルの一覧を返します。

## 注

`Export-PSSession` は、PowerShell リモート処理インフラストラクチャに依存しています。 このコマンドレットを使用するには、リモート用にコンピューターを構成する必要があります。 詳細については、「[about_Remote_Requirements](../Microsoft.PowerShell.Core/About/about_Remote_Requirements.md)」を参照してください。

を使用して `Export-PSSession` PowerShell プロバイダーをエクスポートすることはできません。

エクスポートされたコマンドは、エクスポート元の PSSession で暗黙的に実行されます。 コマンドをリモートで実行する方法の詳細は、PowerShell によって完全に処理されます。 ローカル コマンドを実行するのと同じように、エクスポートしたコマンドを実行できます。

`Export-ModuleMember` は、エクスポートするモジュールの PSSession に関する情報をキャプチャして保存します。 モジュールをインポートするときに、コマンドのエクスポート元の PSSession が閉じられていて、同じコンピューターへのアクティブな PSSessions がない場合、モジュール内のコマンドは PSSession を再作成しようとします。 PSSession を再作成しようとして失敗した場合、エクスポートされたコマンドは実行されません。

モジュールでキャプチャして保存するセッション情報には、 `Export-ModuleMember` ユーザー設定変数で指定したセッションオプションや、、、 `$PSSessionOption` **SessionOption** `New-PSSession` `Enter-PSSession` またはコマンドレットの sessionoption パラメーターを使用したセッションオプションは含まれません `Invoke-Command` 。 モジュールをインポートするときに、元の PSSession が閉じている場合、モジュールは同じコンピューターに対する別の利用可能な PSSession を使用します。 インポートされたコマンドを正しい構成のセッションで実行するには、必要なオプションで PSSession を作成してから、モジュールをインポートするようにしてください。

エクスポートするコマンドを見つけるには、コマンドレットを使用して `Export-PSSession` `Invoke-Command` `Get-Command` PSSession でコマンドを実行します。 コマンドの書式設定データを取得して保存するには、コマンドレットとコマンドレットを使用し `Get-FormatData` `Export-FormatData` ます。 コマンドを実行すると、、、、およびからのエラーメッセージが表示される場合があり `Invoke-Command` `Get-Command` `Get-FormatData` `Export-FormatData` `Export-PSSession` ます。 また、、、 `Export-PSSession` `Get-Command` `Get-FormatData` `Select-Object` 、およびコマンドレットを含まないセッションからコマンドをエクスポートすることはできません `Get-Help` 。

`Export-PSSession` コマンドレットを使用して、 `Write-Progress` コマンドの進行状況を表示します。 コマンドが実行中の場合、進行状況バーが表示されます。

エクスポートされたコマンドには、ユーザー インターフェイスを備えたプログラム (たとえば、メモ帳) を起動することができないなど、他のリモート コマンドと同じ制限があります。

PowerShell プロファイルは PSSessions では実行されないため、プロファイルによってセッションに追加されたコマンドはでは使用できません `Export-PSSession` 。 プロファイルからコマンドをエクスポートするには、コマンドを使用して、コマンドをエクスポートする前に、コマンドを使用し `Invoke-Command` て PSSession でプロファイルを手動で実行します。

`Export-PSSession`コマンドが書式設定データをインポートしない場合でも、を作成するモジュールには書式設定ファイルが含まれる場合があります。 コマンドで書式データがインポートされない場合、作成される書式ファイルに書式データは含まれません。

## 関連リンク

[about_Command_Precedence](../Microsoft.PowerShell.Core/About/about_Command_Precedence.md)

[about_PSSessions](../Microsoft.PowerShell.Core/About/about_PSSessions.md)

[about_PSSnapins](/powershell/module/microsoft.powershell.core/about/about_pssnapins?view=powershell-5.1)

[about_Remote_Requirements](../Microsoft.PowerShell.Core/About/about_Remote_Requirements.md)

[Import-Module](../Microsoft.PowerShell.Core/Import-Module.md)

[Import-PSSession](Import-PSSession.md)

[Invoke-Command](../Microsoft.PowerShell.Core/Invoke-Command.md)

[New-PSSession](../Microsoft.PowerShell.Core/New-PSSession.md)

[New-PSSessionOption](../Microsoft.PowerShell.Core/New-PSSessionOption.md)

[Remove-PSSession](../Microsoft.PowerShell.Core/Remove-PSSession.md)
