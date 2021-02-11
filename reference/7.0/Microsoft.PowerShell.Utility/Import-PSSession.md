---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/import-pssession?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-PSSession
ms.openlocfilehash: c64a59300cdaffe71de04c7843bf644df49530d5
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390118"
---
# Import-PSSession

## 概要
別のセッションから現在のセッションにコマンドをインポートします。

## SYNTAX

```
Import-PSSession [-Prefix <String>] [-DisableNameChecking] [[-CommandName] <String[]>] [-AllowClobber]
 [-ArgumentList <Object[]>] [-CommandType <CommandTypes>] [-Module <String[]>]
 [-FullyQualifiedModule <ModuleSpecification[]>] [[-FormatTypeName] <String[]>]
 [-Certificate <X509Certificate2>] [-Session] <PSSession> [<CommonParameters>]
```

## Description

コマンドレットは、コマンド `Import-PSSession` レット、関数、エイリアスなどのコマンドを、ローカルコンピューターまたはリモートコンピューター上の PSSession から現在のセッションにインポートします。 コマンド `Get-Command` レットが PSSession で検索できる任意のコマンドをインポートできます。

コマンドを使用し `Import-PSSession` て、Microsoft Exchange Server シェルなどのカスタマイズされたシェルから、または、現在のセッションに含まれていない Windows PowerShell モジュールやスナップインなどの要素を含むセッションからコマンドをインポートします。

コマンドをインポートするには、まず、コマンドレットを使用して `New-PSSession` PSSession を作成します。 次に、コマンドレットを使用し `Import-PSSession` てコマンドをインポートします。 既定では、は、 `Import-PSSession` 現在のセッションのコマンドと同じ名前のコマンドを除くすべてのコマンドをインポートします。 すべてのコマンドをインポートするには、 **AllowClobber** パラメーターを使用します。

インポートしたコマンドは、セッションのコマンドを使用する場合と同じように使用できます。 インポートしたコマンドを使用すると、インポートした部分のコマンドがインポート元のセッションで暗黙的に実行されます。 ただし、リモート操作はすべて Windows PowerShell によって処理されます。 リモート操作に注意を払う必要はありませんが、もう一方のセッション (PSSession) への接続を開いたままにする必要があります。 接続を閉じると、インポートしたコマンドを利用できなくなります。

インポートされたコマンドはローカルコマンドよりも実行に時間がかかる場合があるため、 `Import-PSSession` インポートされたすべてのコマンドに **AsJob** パラメーターを追加します。 このパラメーターを使用すると、コマンドを Windows PowerShell のバックグラウンド ジョブとして実行することができます。 詳細については、「[about_Jobs](../Microsoft.PowerShell.Core/about/about_Jobs.md)」を参照してください。

を使用すると `Import-PSSession` 、Windows PowerShell は、インポートされたコマンドをセッションにのみ存在する一時モジュールに追加し、モジュールを表すオブジェクトを返します。 今後のセッションで使用できる永続的なモジュールを作成するには、 `Export-PSSession` コマンドレットを使用します。

この `Import-PSSession` コマンドレットは、Windows PowerShell の暗黙的なリモート処理機能を使用します。 現在のセッションにコマンドをインポートすると、元のセッションまたは元のコンピューターの同様のセッションで、コマンドが暗黙的に実行されます。

Windows PowerShell 3.0 以降では、コマンドレットを使用して、 `Import-Module` リモートセッションから現在のセッションにモジュールをインポートできます。 この機能では、暗黙的なリモート処理が使用されます。 これは、を使用して、 `Import-PSSession` リモートセッションから選択したモジュールを現在のセッションにインポートすることと同じです。

## 例

### 例 1: PSSession からすべてのコマンドをインポートする

```
PS C:\> $S = New-PSSession -ComputerName Server01
PS C:\> Import-PSSession -Session $S
```

このコマンドは、Server01 コンピューターの PSSession から現在のセッションに、現在のセッションのコマンドと同じ名前のコマンドを除くすべてのコマンドをインポートします。

このコマンドでは **CommandName** パラメーターを使用しないため、インポートされたコマンドに必要な書式データもすべてインポートされます。

### 例 2: 特定の文字列で終わるコマンドをインポートする

```
PS C:\> $S = New-PSSession https://ps.testlabs.com/powershell
PS C:\> Import-PSSession -Session $S -CommandName *-test -FormatTypeName *
PS C:\> New-Test -Name Test1
PS C:\> Get-Test test1 | Run-Test
```

これらのコマンドは、PSSession からローカル セッションに名前が "-test" で終わるコマンドをインポートします。その後、インポートしたコマンドレットを使用する方法も示します。

最初のコマンドは、コマンドレットを使用して `New-PSSession` PSSession を作成します。 このメソッドは、PSSession を変数に保存し `$S` ます。

2番目のコマンドは、コマンドレットを使用して、 `Import-PSSession` の PSSession から現在のセッションにコマンドをインポートし `$S` ます。 **CommandName** パラメーターを使用して "Test" という名詞を含むコマンドを指定し、 **FormatTypeName** パラメーターを使用して Test コマンドの書式データをインポートします。

3 番目と 4 番目のコマンドは、現在のセッションでインポートされたコマンドを使用します。 インポートされたコマンドは、現在のセッションに実際に追加されるため、ローカル構文を使用して実行できます。 インポートされた `Invoke-Command` コマンドを実行するためにコマンドレットを使用する必要はありません。

### 例 3: PSSession からコマンドレットをインポートする

```
PS C:\> $S1 = New-PSSession -ComputerName s1
PS C:\> $S2 = New-PSSession -ComputerName s2
PS C:\> Import-PSSession -Session s1 -Type cmdlet -Name New-Test, Get-Test -FormatTypeName *
PS C:\> Import-PSSession -Session s2 -Type Cmdlet -Name Set-Test -FormatTypeName *
PS C:\> New-Test Test1 | Set-Test -RunType Full
```

この例では、ローカルのコマンドレットを使用するのと同じように、インポートされたコマンドレットを使用できることを示します。

これらのコマンドは、Server01 コンピューターの PSSession からコマンドレットとコマンドレットをインポートし、 `New-Test` `Get-Test` `Set-Test` Server02 コンピューターの pssession からコマンドレットをインポートします。

別々の PSSession からコマンドレットをインポートした場合でも、パイプを使用して 1 つのコマンドレットから別のコマンドレットにオブジェクトを渡すことができます。エラーにはなりません。

### 例 4: インポートされたコマンドをバックグラウンドジョブとして実行する

```
PS C:\> $S = New-PSSession -ComputerName Server01
PS C:\> Import-PSSession -Session $S -CommandName *-test* -FormatTypeName *
PS C:\> $batch = New-Test -Name Batch -AsJob
PS C:\> Receive-Job $batch
```

この例では、インポートしたコマンドをバックグラウンド ジョブとして実行する方法を示します。

インポートされたコマンドはローカルコマンドよりも実行に時間がかかる場合があるため、 `Import-PSSession` インポートされたすべてのコマンドに **AsJob** パラメーターを追加します。 **AsJob** パラメーターを使用すると、コマンドをバックグラウンド ジョブとして実行できます。

最初のコマンドは、Server01 コンピューター上に PSSession を作成し、その変数に PSSession オブジェクトを保存し `$S` ます。

2番目のコマンドは、を使用して、 `Import-PSSession` の PSSession から現在のセッションにテストコマンドレットをインポートし `$S` ます。

3番目のコマンドは、インポートされたコマンドレットの **AsJob** パラメーターを使用し `New-Test` て、 `New-Test` バックグラウンドジョブとしてコマンドを実行します。 このコマンドは、を返すジョブオブジェクトを `New-Test` 変数に保存し `$batch` ます。

4番目のコマンドは、コマンドレットを使用して、 `Receive-Job` 変数内のジョブの結果を取得し `$batch` ます。

### 例 5: Windows PowerShell モジュールからコマンドレットと関数をインポートする

```
PS C:\> $S = New-PSSession -ComputerName Server01
PS C:\> Invoke-Command -Session $S {Import-Module TestManagement}
PS C:\> Import-PSSession -Session $S -Module TestManagement
```

この例では、リモート コンピューターの Windows PowerShell モジュールから現在のセッションにコマンドレットと関数をインポートする方法を示します。

最初のコマンドは、Server01 コンピューター上に PSSession を作成し、変数に保存し `$S` ます。

2番目のコマンドは、コマンドレットを使用して、 `Invoke-Command` `Import-Module` の PSSession でコマンドを実行し `$S` ます。

通常、モジュールは Windows PowerShell プロファイルのコマンドによってすべてのセッションに追加され `Import-Module` ますが、プロファイルは PSSessions では実行されません。

3番目のコマンドは、の **module** パラメーターを使用して、 `Import-PSSession` モジュール内のコマンドレットと関数を現在のセッションにインポートします。

### 例 6: 一時ファイルにモジュールを作成する

```
PS C:\> Import-PSSession $S -CommandName Get-Date, SearchHelp -FormatTypeName * -AllowClobber

Name              : tmp_79468106-4e1d-4d90-af97-1154f9317239_tcw1zunz.ttf
Path              : C:\Users\User01\AppData\Local\Temp\tmp_79468106-4e1d-4d90-af97-1154f9317239_tcw1
zunz.ttf\tmp_79468106-4e1d-4d90-af97-1154f9317239_
tcw1zunz.ttf.psm1
Description       : Implicit remoting for http://server01.corp.fabrikam.com/wsman
Guid              : 79468106-4e1d-4d90-af97-1154f9317239
Version           : 1.0
ModuleBase        : C:\Users\User01\AppData\Local\Temp\tmp_79468106-4e1d-4d90-af97-1154f9317239_tcw1
zunz.ttf
ModuleType        : Script
PrivateData       : {ImplicitRemoting}
AccessMode        : ReadWrite
ExportedAliases   : {}
ExportedCmdlets   : {}
ExportedFunctions : {[Get-Date, Get-Date], [SearchHelp, SearchHelp]}
ExportedVariables : {}
NestedModules     : {}
```

この例では、が `Import-PSSession` ディスク上の一時ファイルにモジュールを作成することを示しています。 また、現在のセッションにインポートする前に、すべてのコマンドを関数に変換します。

このコマンドは、コマンドレットを使用して、 `Import-PSSession` `Get-Date` 現在のセッションにコマンドレットと searchhelp 関数をインポートします。

この `Import-PSSession` コマンドレットは、一時モジュールを表す **PSModuleInfo** オブジェクトを返します。 **Path** プロパティの値は、によって `Import-PSSession` スクリプトモジュール (hbase-runner.psm1) ファイルが一時的な場所に作成されたことを示します。 **ExportedFunctions** プロパティは、 `Get-Date` コマンドレットと searchhelp 関数が両方とも関数としてインポートされたことを示しています。

### 例 7: インポートされたコマンドによって非表示になっているコマンドを実行する

```
PS C:\> Import-PSSession $S -CommandName Get-Date -FormatTypeName * -AllowClobber

PS C:\> Get-Command Get-Date -All

CommandType   Name       Definition
-----------   ----       ----------
Function      Get-Date   ...
Cmdlet        Get-Date   Get-Date [[-Date] <DateTime>] [-Year <Int32>] [-Month <Int32>]

PS C:\> Get-Date
09074

PS C:\> (Get-Command -Type Cmdlet -Name Get-Date).PSSnapin.Name
Microsoft.PowerShell.Utility

PS C:\> Microsoft.PowerShell.Utility\Get-Date
Sunday, March 15, 2009 2:08:26 PM
```

この例では、インポートされたコマンドによって非表示になったコマンドを実行する方法を示します。

最初のコマンドは、 `Get-Date` 変数の PSSession からコマンドレットをインポートし `$S` ます。 現在のセッションにはコマンドレットが含まれているため、 `Get-Date` コマンドでは **AllowClobber** パラメーターが必要です。

2番目のコマンドは、コマンドレットの **all** パラメーターを使用して、 `Get-Command` 現在のセッションのすべてのコマンドを取得し `Get-Date` ます。 出力には、セッションに元のコマンドレットと関数が含まれていることが示されて `Get-Date` `Get-Date` います。 関数は、 `Get-Date` の PSSession でインポートされたコマンドレットを実行し `Get-Date` `$S` ます。

3番目のコマンドは、コマンドを実行し `Get-Date` ます。 関数はコマンドレットよりも優先されるため、Windows PowerShell はインポートされた関数を実行します `Get-Date` 。これにより、ユリウス暦の日付が返されます。

4 番目と 5 番目のコマンドは、修飾名を使用して、インポートされたコマンドによって非表示になったコマンドを実行する方法を示しています。

4番目のコマンドは、元のコマンドレットを現在のセッションに追加した Windows PowerShell スナップインの名前を取得し `Get-Date` ます。

5番目のコマンドは、コマンドレットのスナップインによって修飾された名前を使用して、 `Get-Date` コマンドを実行し `Get-Date` ます。

コマンドの優先順位と非表示のコマンドの詳細については、「[about_Command_Precedence](../Microsoft.PowerShell.Core/about/about_Command_Precedence.md)」を参照してください。

### 例 8: 名前に特定の文字列が含まれているコマンドをインポートする

```
PS C:\> Import-PSSession -Session $S -CommandName **Item** -AllowClobber
```

このコマンドは、の PSSession からの項目を含む名前を持つコマンドをインポート `$S` します。 このコマンドには、 **CommandName** パラメーターは含まれていますが、 **formattypedata** パラメーターは含まれていないため、コマンドのみがインポートされます。

このコマンドは、を使用して `Import-PSSession` リモートコンピューターでコマンドを実行し、現在のセッションでコマンドの書式データを既に持っている場合に使用します。

### 例 9: モジュールパラメーターを使用して、セッションにインポートされたコマンドを検出する

```
PS C:\> $M = Import-PSSession -Session $S -CommandName *bits* -FormatTypeName *bits*
PS C:\> Get-Command -Module $M
CommandType     Name
-----------     ----
Function        Add-BitsFile
Function        Complete-BitsTransfer
Function        Get-BitsTransfer
Function        Remove-BitsTransfer
Function        Resume-BitsTransfer
Function        Set-BitsTransfer
Function        Start-BitsTransfer
Function        Suspend-BitsTransfer
```

このコマンドは、の **Module** パラメーターを使用し `Get-Command` て、コマンドによってセッションにインポートされたコマンドを確認する方法を示して `Import-PSSession` います。

最初のコマンドは、コマンドレットを使用して、 `Import-PSSession` 変数の PSSession から "bits" を含む名前のコマンドをインポートし `$S` ます。 `Import-PSSession`コマンドは一時モジュールを返し、モジュールは変数に保存され `$m` ます。

2番目のコマンドは、コマンドレットを使用し `Get-Command` て、変数内のモジュールによってエクスポートされたコマンドを取得し `$M` ます。

**Module** パラメーターは、モジュール名用に設計されており、文字列値を受け取ります。 ただし、モジュール オブジェクトを渡すと、Windows PowerShell は、モジュール オブジェクトの **ToString** メソッドを使用して、モジュール名を返します。

コマンドは、 `Get-Command` "" に相当し `Get-Command $M.Name` ます。

## PARAMETERS

### -AllowClobber

このコマンドレットが、現在のセッションのコマンドと同じ名前を持つ場合でも、指定したコマンドをインポートすることを示します。

現在のセッションにあるコマンドと同じ名前のコマンドをインポートする場合、インポートされたコマンドを非表示にするか、元のコマンドを置き換えます。 詳細については、「[about_Command_Precedence](../Microsoft.PowerShell.Core/about/about_Command_Precedence.md)」(コマンドの優先順位について) を参照してください。

既定で `Import-PSSession` は、は、現在のセッションのコマンドと同じ名前のコマンドをインポートしません。

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

指定された引数 (パラメーター値) を使用した結果として得られるコマンドの配列を指定します。

たとえば、 `Get-Item` 証明書 (Cert:) のコマンドのバリアントをインポートするには、の PSSession のドライブに `$S` 、「」と入力 `Import-PSSession -Session $S -Command Get-Item -ArgumentList cert:` します。

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

が作成する一時モジュール内のフォーマットファイル (* .Format.ps1xml) またはスクリプトモジュールファイル (hbase-runner.psm1) の署名に使用されるクライアント証明書を指定し `Import-PSSession` ます。

証明書が格納されている変数を入力するか、証明書を取得するコマンドまたは式を入力します。

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

指定された名前または名前パターンのコマンドを指定します。 ワイルドカードを使用できます。 **CommandName** またはそのエイリアスである **Name** を使用します。

既定では、は `Import-PSSession` セッションからすべてのコマンドをインポートします。ただし、現在のセッションのコマンドと同じ名前を持つコマンドは除きます。 そのため、インポート先のセッションでインポートしたコマンドが非表示になることや、置き換えられることがありません。 非表示になったり、他のコマンドを置き換えたりするコマンドを含め、すべてのコマンドをインポートするには、 **AllowClobber** パラメーターを使用します。

**CommandName** パラメーターを使用する場合は、 **FormatTypeName** パラメーターを使用しない限り、コマンドの書式ファイルはインポートされません。 同様に、 **FormatTypeName** パラメーターを使用する場合は、 **CommandName** パラメーターを使用しない限り、コマンドはインポートされません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Name

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CommandType

コマンドオブジェクトの種類を指定します。 既定値は Cmdlet です。 **CommandType** またはそのエイリアスの **Type** を使用します。 このパラメーターの有効値は、次のとおりです。

- エイリアス. リモート セッションの Windows PowerShell エイリアス。
- すべて。 リモート セッションのコマンドレットと関数。
- アプリケーション をクリックします。 リモートセッションの Path 環境変数 () に一覧表示されているパス内のファイル Windows-PowerShell 以外のすべてのファイル ( `$env:path` .txt、.exe、.dll ファイルなど)。
- コマンドレット. リモート セッションのコマンドレット。 "Cmdlet" が既定値です。
- ExternalScript。 リモートセッションの Path 環境変数 () に一覧表示されているパス内の ps1 ファイル。 `$env:path`
- フィルターと関数。 リモート セッションの Windows PowerShell 関数。
- スクリプティング。 リモート セッションのスクリプト ブロック。

```yaml
Type: System.Management.Automation.CommandTypes
Parameter Sets: (All)
Aliases: Type
Accepted values: Alias, Function, Filter, Cmdlet, ExternalScript, Application, Script, Workflow, Configuration, All

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableNameChecking

このコマンドレットが、許可されていない動詞または禁止された文字を含んでいる名前を持つコマンドレットまたは関数をインポートしたときに警告するメッセージを抑制することを示します。

既定では、インポートしたモジュールが、名前に許可されていない動詞を持つコマンドレットまたは関数をエクスポートすると、Windows PowerShell によって次の警告メッセージが表示されます。

"警告: インポートされたコマンド名には、許可されていない動詞が含まれているため、検出できない可能性があります。 詳細については Verbose パラメーターを使用し、承認された `Get-Verb` 動詞の一覧を表示するには「」と入力してください。

このメッセージは、単なる警告です。 実際には、非準拠のコマンドを含む、すべてのモジュールがインポートされます。 モジュール ユーザーにメッセージが表示されますが、名前付けの問題はモジュール作成者が解決する必要があります。

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

指定された Microsoft .NET Framework 型の書式設定命令を指定します。
型名を入力します。
ワイルドカードを使用できます。

このパラメーターの値は、コマンドがインポートされるセッション内のコマンドによって返される型の名前である必要があり `Get-FormatData` ます。 リモートセッションのすべての書式設定データを取得するには、「」と入力 `*` します。

コマンドに **CommandName** パラメーターまたは **formattypename** パラメーターが含まれていない場合は、 `Import-PSSession` `Get-FormatData` リモートセッションでコマンドによって返されるすべての .NET Framework 型の書式設定命令をインポートします。

**FormatTypeName** パラメーターを使用する場合は、 **CommandName** パラメーターを使用しない限り、コマンドはインポートされません。

同様に、 **CommandName** パラメーターを使用する場合は、 **FormatTypeName** パラメーターを使用しない限り、コマンドの書式ファイルはインポートされません。

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

PowerShell SDK の Modulの **特定の**[コンストラクター (Hashtable)](/dotnet/api/microsoft.powershell.commands.modulespecification.-ctor#Microsoft_PowerShell_Commands_ModuleSpecification__ctor_System_Collections_Hashtable_)の「解説」で説明されている名前を持つモジュールを指定します。 たとえば、 **FullyQualifiedModule** パラメーターは、次の形式で指定されたモジュール名を受け入れます。

- `@{ModuleName = "modulename"; ModuleVersion = "version_number"}` または
- `@{ModuleName = "modulename"; ModuleVersion = "version_number"; Guid = "GUID"}`.

**ModuleName** と **ModuleVersion** は必須ですが、 **Guid** は省略可能です。

**モジュール** パラメーターと同じコマンドで **FullyQualifiedModule** パラメーターを指定することはできません。 2つのパラメーターは相互に排他的です。

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

Windows PowerShell スナップインおよびモジュールのコマンドの配列とを指定します。 スナップインとモジュールの名前を入力します。 ワイルドカードは使用できません。

`Import-PSSession` スナップインからプロバイダーをインポートできません。

詳細については、「[about_PSSnapins](/powershell/module/Microsoft.PowerShell.Core/About/about_PSSnapins)」と「[about_Modules](../Microsoft.PowerShell.Core/About/about_Modules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PSSnapin

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Prefix

インポートされたコマンドの名前の名詞のプレフィックスを指定します。

セッションに同じ名前の別のコマンドが存在する場合に発生する名前の競合を回避するには、このパラメーターを使用します。

たとえば、プレフィックス Remote を指定してからコマンドレットをインポートした場合、 `Get-Date` このコマンドレットはセッションではとして認識され、 `Get-RemoteDate` 元のコマンドレットと混同されることはありません `Get-Date` 。

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

### -セッション

コマンドレットのインポート元の **PSSession** を指定します。 セッションオブジェクトを含む変数、 `New-PSSession` またはコマンドやコマンドなどのセッションオブジェクトを取得するコマンドを入力し `Get-PSSession` ます。 指定できるセッションは 1 つだけです。 このパラメーターは必須です。

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

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### PSModuleInfo (システム管理)

`Import-PSSession` およびコマンドレットが返すのと同じモジュールオブジェクトを返し `New-Module` `Get-Module` ます。
ただし、インポートされたモジュールは一時的なもので、現在のセッションにのみ存在します。 ディスクにパーマネントモジュールを作成するには、 `Export-PSSession` コマンドレットを使用します。

## 注

- `Import-PSSession` は、PowerShell リモート処理インフラストラクチャに依存しています。 このコマンドレットを使用するには、WS-Management リモート処理用にコンピューターを構成する必要があります。 詳細については、「 [about_Remote](../Microsoft.PowerShell.Core/about/about_Remote.md) 」および「 [about_Remote_Requirements](../Microsoft.PowerShell.Core/about/about_Remote_Requirements.md)」を参照してください。
- `Import-PSSession` では、変数または PowerShell プロバイダーはインポートされません。
- 現在のセッションのコマンドと同じ名前のコマンドをインポートすると、インポートされたコマンドによって、現在のセッションのエイリアス、関数、およびコマンドレットが非表示になったり、現在のセッションの関数や変数が置き換えられたりすることがあります。 名前の競合を回避するには、 **Prefix** パラメーターを使用します。 詳細については、「[about_Command_Precedence](../Microsoft.PowerShell.Core/about/about_Command_Precedence.md)」(コマンドの優先順位について) を参照してください。
- `Import-PSSession` すべてのコマンドをインポート前に関数に変換します。 そのため、インポートされたコマンドの動作は、元のコマンドの種類が維持されている場合と少し異なります。 たとえば、PSSession からコマンドレットをインポートした後、モジュールまたはスナップインから同じ名前のコマンドレットをインポートした場合は、関数はコマンドレットよりも優先されるため、既定では PSSession からインポートしたコマンドレットが常に実行されます。 また、同じ名前のエイリアスが存在するセッションにエイリアスをインポートした場合は、エイリアスは関数よりも優先されるため、元のエイリアスが常に使用されます。 詳細については、「[about_Command_Precedence](../Microsoft.PowerShell.Core/about/about_Command_Precedence.md)」(コマンドの優先順位について) を参照してください。
- `Import-PSSession` コマンドレットを使用して、 `Write-Progress` コマンドの進行状況を表示します。 コマンドが実行中の場合、進行状況バーが表示されます。
- インポートするコマンドを見つけるには、コマンドレットを使用して `Import-PSSession` `Invoke-Command` `Get-Command` PSSession でコマンドを実行します。 コマンドの書式設定データを取得するには、コマンドレットを使用し `Get-FormatData` ます。 コマンドを実行すると、これらのコマンドレットからのエラーメッセージが表示することがあり `Import-PSSession` ます。 また、、、 `Import-PSSession` `Get-Command` `Get-FormatData` `Select-Object` 、およびコマンドレットを含まない PSSession からコマンドをインポートすることはできません `Get-Help` 。
- インポートされたコマンドには、ユーザー インターフェイスを備えたプログラム (たとえば、メモ帳) を起動できないなど、他のリモート コマンドと同じ制限があります。
- Windows PowerShell プロファイルは PSSessions では実行されないため、プロファイルによってセッションに追加されたコマンドはでは使用できません `Import-PSSession` 。 プロファイルからコマンドをインポートするには、コマンドを使用して、コマンドをインポートする前に、コマンドを使用し `Invoke-Command` て PSSession でプロファイルを手動で実行します。
- `Import-PSSession`コマンドで書式データがインポートされない場合でも、によって作成される一時モジュールには書式設定ファイルが含まれる場合があります。 コマンドで書式データがインポートされない場合、作成される書式ファイルに書式データは含まれません。
- を使用するには `Import-PSSession` 、現在のセッションの実行ポリシーを Restricted または AllSigned にすることはできません。作成される一時モジュールには、 `Import-PSSession` これらのポリシーで禁止されている未署名のスクリプトファイルが含まれているためです。 `Import-PSSession`ローカルコンピューターの実行ポリシーを変更せずにを使用するには、の **Scope** パラメーターを使用して、 `Set-ExecutionPolicy` 1 つのプロセスに対してより制限の緩い実行ポリシーを設定します。
- Windows PowerShell 2.0 では、別のセッションからインポートしたコマンドのヘルプ トピックに、 **Prefix** パラメーターを使用して割り当てたプレフィックスが含まれません。 Windows PowerShell 2.0 でインポートしたコマンドのヘルプを取得するには、元の (プレフィックスなしの) コマンド名を使用します。

## 関連リンク

[Export-PSSession](Export-PSSession.md)
