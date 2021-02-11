---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 09/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-help?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Help
ms.openlocfilehash: 82721b3d079c795e0ce6fcae1fba0eab93344b52
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599915"
---
# Get-Help

## 概要
PowerShell コマンドと概念に関する情報を表示します。

## SYNTAX

### AllUsersView (既定値)

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Full] [-Component <String[]>]
 [-Functionality <String[]>] [-Role <String[]>] [<CommonParameters>]
```

### DetailedView

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] -Detailed
 [-Component <String[]>] [-Functionality <String[]>] [-Role <String[]>] [<CommonParameters>]
```

### 例

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] -Examples
 [-Component <String[]>] [-Functionality <String[]>] [-Role <String[]>] [<CommonParameters>]
```

### パラメーター

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] -Parameter <String[]>
 [-Component <String[]>] [-Functionality <String[]>] [-Role <String[]>] [<CommonParameters>]
```

### オンライン

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Component <String[]>]
 [-Functionality <String[]>] [-Role <String[]>] -Online [<CommonParameters>]
```

### ShowWindow

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Component <String[]>]
 [-Functionality <String[]>] [-Role <String[]>] -ShowWindow [<CommonParameters>]
```

## Description

コマンド `Get-Help` レットでは、コマンドレット、関数、Common Information Model (CIM) コマンド、ワークフロー、プロバイダー、エイリアス、スクリプトなど、PowerShell の概念とコマンドに関する情報を表示します。

PowerShell コマンドレットのヘルプを表示するには、「」のように、 `Get-Help` コマンドレット名の後に「」と入力し `Get-Help Get-Process` ます。

PowerShell の概念説明ヘルプ記事は、 **about_Comparison_Operators** などの **about_** から始まります。 すべての **about_** 記事を表示するには、「」と入力 `Get-Help about_*` します。 特定の記事を表示するには、のように「 `Get-Help about_<article-name>` 」と入力し `Get-Help about_Comparison_Operators` ます。

PowerShell プロバイダーのヘルプを表示するには、 `Get-Help` プロバイダー名の後に「」と入力します。 たとえば、証明書プロバイダーのヘルプを表示するには、「」と入力 `Get-Help Certificate` します。

`help`または `man` を入力して、一度に1つのテキスト画面を表示することもできます。 または、はと `<cmdlet-name> -?` 同じです `Get-Help` が、コマンドレットに対してのみ機能します。

`Get-Help` コンピューター上のヘルプファイルから表示されるヘルプコンテンツを取得します。 ヘルプファイルがない場合は、 `Get-Help` コマンドレットに関する基本的な情報のみが表示されます。 一部の PowerShell モジュールには、ヘルプファイルが含まれています。 PowerShell 3.0 以降では、Windows オペレーティングシステムに付属のモジュールにヘルプファイルは含まれていません。 PowerShell 3.0 でモジュールのヘルプファイルをダウンロードまたは更新するには、 `Update-Help` コマンドレットを使用します。

Microsoft Docs で PowerShell のヘルプドキュメントをオンラインで表示することもできます。ヘルプファイルのオンラインバージョンを取得するには、のように、 **online** パラメーターを使用し `Get-Help Get-Process -Online` ます。 PowerShell のドキュメントをすべて読み取るには、Microsoft Docs [powershell のドキュメント](/powershell)を参照してください。

`Get-Help`ヘルプ記事の正確な名前、またはヘルプ記事に固有の単語を入力すると、によって `Get-Help` その記事の内容が表示されます。 コマンドエイリアスの正確な名前を指定すると、によって `Get-Help` 元のコマンドのヘルプが表示されます。 複数のヘルプ記事のタイトルに表示される単語または単語のパターンを入力すると、 `Get-Help` 一致するタイトルの一覧が表示されます。 ヘルプ記事のタイトルに表示されていないテキストを入力すると、 `Get-Help` そのテキストを内容に含む記事の一覧がに表示されます。

`Get-Help` サポートされているすべての言語およびロケールのヘルプ記事を取得できます。 `Get-Help` は、最初に、Windows に設定されているロケールでヘルプファイルを検索し、次に、親ロケール (たとえば、pt の **pt** **-BR**) で、次にフォールバックロケールでヘルプファイルを検索します。 PowerShell 3.0 以降では、フォールバックロケールでヘルプが見つからない場合、では、 `Get-Help` エラーメッセージを返すか、自動生成されたヘルプを表示する前に、英語 ( **en-us**) のヘルプ記事が検索されます。

コマンド構文ダイアグラムに表示されるシンボルの詳細については `Get-Help` 、「 [about_Command_Syntax](./About/about_Command_Syntax.md)」を参照してください。
**Required** や **Position** などのパラメーター属性の詳細については、「 [about_Parameters](./About/about_Parameters.md)」を参照してください。

>[!NOTE]
> PowerShell 3.0 および PowerShell 4.0 では、 `Get-Help` モジュールが現在のセッションにインポートされ **て** いない限り、はモジュール内の記事を見つけることができません。 これは既知の問題です。 モジュール内の記事 **に関する情報** を取得するには、コマンドレットを使用するか、 `Import-Module` モジュールに含まれているコマンドレットを実行して、モジュールをインポートします。

## 例

### 例 1: コマンドレットに関する基本的なヘルプ情報を表示する

これらの例では、コマンドレットに関する基本的なヘルプ情報が表示さ `Format-Table` れます。

```powershell
Get-Help Format-Table
Get-Help -Name Format-Table
Format-Table -?
```

`Get-Help <cmdlet-name>` は、コマンドレットの最も簡単な既定の構文です `Get-Help` 。 **Name** パラメーターは省略できます。

構文は `<cmdlet-name> -?` コマンドレットに対してのみ機能します。

### 例 2: 基本的な情報を1ページずつ表示する

これらの例では、コマンドレットの基本的なヘルプ情報 `Format-Table` が1ページずつ表示されます。

```powershell
help Format-Table
man Format-Table
Get-Help Format-Table | Out-Host -Paging
```

`help` は、 `Get-Help` コマンドレットを内部的に実行し、結果を1ページずつ表示する関数です。

`man` 関数のエイリアスです `help` 。

`Get-Help Format-Table` オブジェクトをパイプラインの下に送信します。 `Out-Host -Paging` パイプラインからの出力を受け取り、一度に1ページずつ表示します。 詳細については、「 [Out Host](Out-Host.md)」を参照してください。

### 例 3: コマンドレットの詳細情報を表示する

これらの例では、コマンドレットの詳細なヘルプ情報が表示さ `Format-Table` れます。

```powershell
Get-Help Format-Table -Detailed
Get-Help Format-Table -Full
```

**詳細** パラメーターには、パラメーターの説明と例を含むヘルプ記事の詳細ビューが表示されます。

**Full** パラメーターは、パラメーターの説明、例、入力オブジェクトと出力オブジェクトの種類、および追加のメモを含むヘルプ記事の完全なビューを表示します。

**詳細** パラメーターと **完全** パラメーターは、コンピューターにヘルプファイルがインストールされているコマンドに対してのみ有効です。 これらのパラメーターは、概念 (**about_**) のヘルプ記事には有効ではありません。

### 例 4: パラメーターを使用して、コマンドレットの選択した部分を表示する

これらの例では、コマンドレットのヘルプの選択部分を表示し `Format-Table` ます。

```powershell
Get-Help Format-Table -Examples
Get-Help Format-Table -Parameter *
Get-Help Format-Table -Parameter GroupBy
```

**例** のパラメーターには、ヘルプファイルの **名前** と **概要**、およびすべての例が表示されます。 **例のパラメーターは** スイッチパラメーターであるため、例の番号を指定することはできません。

**Parameter** パラメーターには、指定されたパラメーターの説明だけが表示されます。 アスタリスク () のワイルドカード文字のみを指定すると `*` 、すべてのパラメーターの説明が表示されます。
**パラメーター** で **GroupBy** などのパラメーター名を指定すると、そのパラメーターに関する情報が表示されます。

これらのパラメーターは、概念 (**about_**) のヘルプ記事には有効ではありません。

### 例 5: オンラインバージョンのヘルプを表示する

この例では、 `Format-Table` 既定の web ブラウザーでコマンドレットのヘルプ記事のオンラインバージョンを表示します。

```powershell
Get-Help Format-Table -Online
```

### 例 6: ヘルプシステムに関するヘルプを表示する

パラメーターを指定せずにコマンドレットを実行すると、 `Get-Help` PowerShell ヘルプシステムに関する情報が表示されます。

```powershell
Get-Help
```

### 例 7: 使用可能なヘルプ記事を表示する

この例では、お使いのコンピューターで利用可能なすべてのヘルプ記事の一覧を表示します。

```powershell
Get-Help *
```

### 例 8: 概念説明の記事の一覧を表示する

この例では、PowerShell ヘルプに含まれる概念説明の記事の一覧を示します。 これらの記事はすべて **about_** 文字で始まります。 特定のヘルプファイルを表示するには、たとえば「」と入力 `Get-Help \<about_article-name\>` `Get-Help about_Signing` します。

コンピューターにヘルプファイルがインストールされている概念説明の記事のみが表示されます。 PowerShell 3.0 でヘルプファイルをダウンロードしてインストールする方法の詳細については、「 [update-help](Update-Help.md)」を参照してください。

```powershell
Get-Help about_*
```

### 例 9: コマンドレットのヘルプで単語を検索する

この例は、コマンドレットのヘルプ記事で単語を検索する方法を示しています。

```powershell
Get-Help Add-Member -Full | Out-String -Stream | Select-String -Pattern Clixml
```

```Output
the Export-Clixml cmdlet to save the instance of the object, including the additional members...
can use the Import-Clixml cmdlet to re-create the instance of the object from the information...
Export-Clixml
Import-Clixml
```

`Get-Help` は、 **完全な** パラメーターを使用してのヘルプ情報を取得し `Add-Member` ます。 **MamlCommandHelpInfo** オブジェクトは、パイプラインを介して送信されます。 `Out-String`**Stream** パラメーターを使用して、オブジェクトを文字列に変換します。 `Select-String`**Pattern** パラメーターを使用して、 **export-clixml** 文字列を検索します。

### 例 10: 単語を含む記事の一覧を表示する

この例では、 **リモート処理** という単語を含む記事の一覧を表示します。

どの記事のタイトルにも表示されない単語を入力すると、 `Get-Help` その単語を含む記事の一覧がに表示されます。

```powershell
Get-Help -Name remoting
```

```Output
Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Install-PowerShellRemoting.ps1    External                            Install-PowerShellRemoting.ps1
Disable-PSRemoting                Cmdlet    Microsoft.PowerShell.Core Prevents remote users...
Enable-PSRemoting                 Cmdlet    Microsoft.PowerShell.Core Configures the computer...
```

### 例 11: プロバイダー固有のヘルプを表示する

この例では、のプロバイダー固有のヘルプを取得する2つの方法を示し `Get-Item` ます。 これらのコマンドは、 `Get-Item` PowerShell SQL Server プロバイダーの **DataCollection** ノードでコマンドレットを使用する方法を説明するヘルプを表示します。

最初の例では、path パラメーターを使用して `Get-Help`  、SQL Server プロバイダーのパスを指定します。
プロバイダーのパスが指定されているため、任意のパスの場所からコマンドを実行できます。

2番目の例では、を使用し `Set-Location` て、SQL Server プロバイダーのパスに移動します。 この場所から、プロバイダー固有のヘルプを取得するために、 **Path** パラメーターは必要ありません `Get-Help` 。

```powershell
Get-Help Get-Item -Path SQLSERVER:\DataCollection
```

```Output
NAME

    Get-Item

SYNOPSIS

    Gets a collection of Server objects for the local computer and any computers

    to which you have made a SQL Server PowerShell connection.
    ...
```

```powershell
Set-Location SQLSERVER:\DataCollection
SQLSERVER:\DataCollection> Get-Help Get-Item
```

```Output
NAME

    Get-Item

SYNOPSIS

    Gets a collection of Server objects for the local computer and any computers

    to which you have made a SQL Server PowerShell connection.
    ...
```

### 例 12: スクリプトのヘルプを表示する

この例では、のヘルプを取得し `MyScript.ps1 script` ます。 関数とスクリプトのヘルプを記述する方法の詳細については、「 [about_Comment_Based_Help](./About/about_Comment_Based_Help.md)」を参照してください。

```powershell
Get-Help -Name C:\PS-Test\MyScript.ps1
```

## PARAMETERS

### -カテゴリ

指定したカテゴリの項目とそのエイリアスに対してのみヘルプを表示します。 概念説明の記事は、 **HelpFile** カテゴリに含まれています。

このパラメーターに指定できる値は次のとおりです。

- エイリアス
- コマンドレット
- プロバイダー
- 全般
- よくあるご質問
- 用語集
- HelpFile
- ScriptCommand
- Function
- Assert
- ExternalScript
- すべて
- DefaultHelp
- ワークフロー
- DscResource
- インスタンス
- 構成

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:
Accepted values: Alias, Cmdlet, Provider, General, FAQ, Glossary, HelpFile, ScriptCommand, Function, Filter, ExternalScript, All, DefaultHelp, Workflow, DscResource, Class, Configuration

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -コンポーネント

**Exchange** など、指定されたコンポーネント値を持つコマンドを表示します。 コンポーネント名を入力します。
ワイルドカード文字を使用できます。 このパラメーターは、概念 (**About_**) ヘルプの表示には影響しません。

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

### -Detailed

基本的なヘルプの表示に加えてパラメーターの説明と例を表示します。 このパラメーターは、ヘルプファイルがコンピューターにインストールされている場合にのみ有効です。 概念 (**About_**) のヘルプの表示には効果がありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DetailedView
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Examples

名前、概要、例のみを表示します。 例のみを表示するには、「」と入力 `(Get-Help \<cmdlet-name\>).Examples` します。

このパラメーターは、ヘルプファイルがコンピューターにインストールされている場合にのみ有効です。 概念 (**About_**) のヘルプの表示には効果がありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Examples
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Full

コマンドレットのヘルプ記事全体を表示します。 **完全** には、パラメーターの説明と属性、例、入力オブジェクトと出力オブジェクトの種類、および追加のメモが含まれています。

このパラメーターは、ヘルプファイルがコンピューターにインストールされている場合にのみ有効です。 概念 (**About_**) のヘルプの表示には効果がありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AllUsersView
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -機能

指定した機能を持つ項目のヘルプを表示します。 機能を入力します。 ワイルドカード文字を使用できます。 このパラメーターは、概念 (**About_**) ヘルプの表示には影響しません。

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

### -Name

指定したコマンドまたは概念に関するヘルプを取得します。 コマンドレット、関数、プロバイダー、スクリプト、またはワークフローの名前 (など)、またはのような概念記事の名前 (など)、 `Get-Member` またはエイリアス (など) を入力し `about_Objects` `ls` ます。 コマンドレットとプロバイダー名にはワイルドカード文字を使用できますが、ワイルドカード文字を使用して関数のヘルプおよびスクリプトのヘルプ記事の名前を検索することはできません。

環境変数に示されているパスにないスクリプトのヘルプを表示するには `$env:Path` 、スクリプトのパスとファイル名を入力します。

ヘルプ記事の正確な名前を入力すると、に `Get-Help` その記事の内容が表示されます。

複数のヘルプ記事のタイトルに表示される単語または単語のパターンを入力すると、 `Get-Help` 一致するタイトルの一覧が表示されます。

ヘルプ記事のタイトルに一致しないテキストを入力すると、 `Get-Help` そのテキストを内容に含む記事の一覧がに表示されます。

などの概念記事の名前は、 `about_Objects` 英語以外のバージョンの PowerShell でも英語で入力する必要があります。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Online

ヘルプ記事のオンラインバージョンを既定のブラウザーに表示します。 このパラメーターは、コマンドレット、関数、ワークフロー、およびスクリプトのヘルプ記事に対してのみ有効です。 リモートセッションでは、 **Online** パラメーターをと共に使用することはできません `Get-Help` 。

記述するヘルプ記事でこの機能をサポートする方法の詳細については、「 [about_Comment_Based_Help](./About/about_Comment_Based_Help.md)」、「 [オンラインヘルプのサポート](/powershell/scripting/developer/module/supporting-online-help)」、および「 [PowerShell コマンドレットのヘルプの作成](/powershell/scripting/developer/help/writing-help-for-windows-powershell-cmdlets)」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Online
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Parameter

指定したパラメーターの詳しい説明のみを表示します。 ワイルドカードを使用できます。 このパラメーターは、概念 (**About_**) ヘルプの表示には影響しません。

```yaml
Type: System.String[]
Parameter Sets: Parameters
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Path

指定されたプロバイダー パスでのコマンドレットの動作を説明するヘルプを取得します。 PowerShell プロバイダーのパスを入力してください。

このパラメーターは、コマンドレットヘルプ記事のカスタマイズバージョンを取得します。このコマンドレットは、指定された PowerShell プロバイダーパスでのコマンドレットの動作を説明します。 このパラメーターは、プロバイダーコマンドレットに関するヘルプに対してのみ有効です。プロバイダーがヘルプファイルにプロバイダーコマンドレットのヘルプ記事のカスタムバージョンを含める場合にのみ有効です。 このパラメーターを使用するには、プロバイダーが含まれるモジュールのヘルプ ファイルをインストールします。

プロバイダーパスのカスタムコマンドレットのヘルプを表示するには、プロバイダーのパスの場所に移動し、コマンドを入力する `Get-Help` か、任意のパスの場所からの **path** パラメーターを使用して `Get-Help` プロバイダーのパスを指定します。 ヘルプ記事のプロバイダヘルプセクションで、カスタムコマンドレットのヘルプをオンラインで検索することもできます。

PowerShell プロバイダーの詳細については、「 [about_Providers](./About/about_Providers.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ロール

指定したユーザー ロール向けにカスタマイズされたヘルプを表示します。 ロールを入力します。 ワイルドカード文字を使用できます。

組織においてユーザーが果たすロールを入力します。 一部のコマンドレットでは、このパラメーターの値に基づいて、ヘルプ ファイルに異なるテキストが表示されます。 このパラメーターは、コア コマンドレットのヘルプには効果を持ちません。

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

### -ShowWindow

読みやすくするために、ウィンドウにヘルプ トピックを表示します。 このウィンドウには **、検索の検索機能** と、ヘルプトピックの選択したセクションのみを表示するオプションを含む、表示のオプションを設定できる **設定** ボックスが含まれています。

**ShowWindow** パラメーターは、コマンド (コマンドレット、関数、CIM コマンド、スクリプト) のヘルプトピックと **、記事の** 概念をサポートします。 プロバイダー ヘルプはサポートされません。

このパラメーターは、PowerShell 7.0 で再導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ShowWindow
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

オブジェクトをパイプラインからに送信することはできません `Get-Help` 。

## 出力

### ExtendedCmdletHelpInfo

`Get-Help`ヘルプファイルのないコマンドでを実行すると、は `Get-Help` 自動生成されたヘルプを表す **ExtendedCmdletHelpInfo** オブジェクトを返します。

### System.String

概念説明のヘルプ記事が表示された場合は、 `Get-Help` それを文字列として返します。

### MamlCommandHelpInfo

ヘルプファイルを含むコマンドを取得すると、は `Get-Help` **MamlCommandHelpInfo** オブジェクトを返します。

## 注

PowerShell 3.0 にはヘルプファイルが含まれていません。 が読み込まれたヘルプファイルをダウンロードしてインストールするには `Get-Help` 、コマンドレットを使用し `Update-Help` ます。 コマンドレットを使用して、 `Update-Help` PowerShell およびインストールするモジュールに付属するコアコマンドのヘルプファイルをダウンロードしてインストールすることができます。 また、このコマンドレットを使用してヘルプ ファイルを更新して、コンピューター上のヘルプを最新に保つこともできます。

PowerShell online に付属するコマンドに関するヘルプ記事は、 [Windows powershell を使用](/powershell/scripting/getting-started/getting-started-with-windows-powershell)したはじめにから開始することもできます。

`Get-Help` Windows オペレーティングシステムに対して設定されたロケール、またはそのロケールのフォールバック言語でヘルプを表示します。 プライマリまたはフォールバックロケールのヘルプファイルがない場合、は `Get-Help` コンピューターにヘルプファイルがないかのように動作します。 別のロケールのヘルプを表示するには、コントロールパネルの [ **地域** と **言語** ] を使用して設定を変更します。 Windows 10 の場合、 **設定**、 **時刻 & 言語**。

ヘルプの完全なビューには、パラメーターに関する情報の表が含まれています。 この表には、次のフィールドが含まれます。

- **[必須]** 。 パラメーターが必須 (true) であるか省略可能 (false) であるかを示します。

- **位置**。 パラメーターの名前が付けられているか、位置 (numeric) であるかを示します。 位置指定パラメーターは、コマンド内の特定の場所に指定する必要があります。

- **名前付き** では、パラメーター名が必須であることを示しますが、パラメーターはコマンド内の任意の場所で使用できます。

- **数値** は、パラメーター名が省略可能であることを示します。ただし、名前を省略した場合、パラメーターは数値で指定された場所にある必要があります。 たとえば、は、パラメーター名を省略した場合に、 `2` パラメーターが2番目または名前のないパラメーターである必要があることを示します。 パラメーター名を使用する場合は、コマンド内の任意の場所にパラメーターを指定できます。

- **既定値**。 コマンドにパラメーターを含めない場合に PowerShell が使用するパラメーター値または既定の動作。

- パイプラインの入力を受け入れます。 パイプラインを介してオブジェクトをパラメーターに送信できるかどうか (true)、またはできない (false) かどうかを示します。 **プロパティ名を** 指定すると、パイプラインオブジェクトには、パラメーター名と同じ名前のプロパティが必要になります。

- **ワイルドカード文字を受け入れ** ます。 パラメーターの値にアスタリスク ( `*` ) や疑問符 () などのワイルドカード文字を含めることができるかどうかを示し `?` ます。

## 関連リンク

[about_Command_Syntax](About/about_Command_Syntax.md)

[about_Comment_Based_Help](./About/about_Comment_Based_Help.md)

[Get-Command](Get-Command.md)

[更新可能なヘルプのサポート](/powershell/scripting/developer/module/supporting-updatable-help)

[Update-Help](Update-Help.md)

[コメントベースのヘルプ トピックを記述する](/powershell/scripting/developer/help/writing-comment-based-help-topics)

[PowerShell コマンドレットのヘルプの記述](/powershell/scripting/developer/help/writing-help-for-windows-powershell-cmdlets)

