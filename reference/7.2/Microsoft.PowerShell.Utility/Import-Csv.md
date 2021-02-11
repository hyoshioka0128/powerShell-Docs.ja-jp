---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/19/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/import-csv?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-Csv
ms.openlocfilehash: 819591c6004d6185958376ac8f84c42b9bfbf460
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600979"
---
# Import-Csv

## 概要
コンマ区切り値 (CSV) ファイルの項目から、テーブルに似たカスタムオブジェクトを作成します。

## SYNTAX

### デリミターパス (既定)

```
Import-Csv [[-Delimiter] <Char>] [-Path] <String[]> [-Header <String[]>] [-Encoding <Encoding>]
 [<CommonParameters>]
```

### DelimiterLiteralPath

```
Import-Csv [[-Delimiter] <Char>] -LiteralPath <String[]> [-Header <String[]>] [-Encoding <Encoding>]
 [<CommonParameters>]
```

### CulturePath

```
Import-Csv [-Path] <String[]> -UseCulture [-Header <String[]>] [-Encoding <Encoding>]
 [<CommonParameters>]
```

### アルゴリズムの反復

```
Import-Csv -LiteralPath <String[]> -UseCulture [-Header <String[]>] [-Encoding <Encoding>]
 [<CommonParameters>]
```

## Description

`Import-Csv`コマンドレットでは、CSV ファイルの項目からテーブルに似たカスタムオブジェクトを作成します。 CSV ファイルの各列がカスタムオブジェクトのプロパティになり、行の項目がプロパティ値になります。 `Import-Csv` コマンドレットによって生成されたファイルを含む、任意の CSV ファイルで動作 `Export-Csv` します。

コマンドレットのパラメーターを使用して、 `Import-Csv` 列ヘッダー行と項目区切り記号を指定したり、 `Import-Csv` 現在のカルチャの区切り記号を項目の区切り記号として使用するように指定したりできます。

また、 `ConvertTo-Csv` `ConvertFrom-Csv` コマンドレットとコマンドレットを使用して、オブジェクトを CSV 文字列 (およびその逆) に変換することもできます。 これらのコマンドレットは、およびコマンドレットと同じですが、 `Export-CSV` `Import-Csv` ファイルを処理しない点が異なります。

CSV ファイルのヘッダー行エントリに空の値または null 値が含まれている場合は、PowerShell によって既定のヘッダー行の名前が挿入され、警告メッセージが表示されます。

PowerShell 6.0 以降では、 `Import-Csv` W3C 拡張ログファイル形式がサポートされるようになりました。

## 例

### 例 1: プロセスオブジェクトをインポートする

この例では、プロセスオブジェクトの CSV ファイルをエクスポートしてインポートする方法を示します。

```powershell
Get-Process | Export-Csv -Path .\Processes.csv
$P = Import-Csv -Path .\Processes.csv
$P | Get-Member
```

```Output
   TypeName: System.Management.Automation.PSCustomObject

Name                       MemberType   Definition
----                       ----------   ----------
Equals                     Method       bool Equals(System.Object obj)
GetHashCode                Method       int GetHashCode()
GetType                    Method       type GetType()
ToString                   Method       string ToString()
BasePriority               NoteProperty string BasePriority=8
Company                    NoteProperty string Company=Microsoft Corporation
...
```

```powershell
$P | Format-Table
```

```Output
Name                   SI Handles VM            WS        PM        NPM    Path
----                   -- ------- --            --        --        ---    ----
ApplicationFrameHost   4  407     2199293489152 15884288  15151104  23792  C:\WINDOWS\system32\ApplicationFrameHost.exe
...
wininit                0  157     2199112204288 4591616   1630208   10376
winlogon               4  233     2199125549056 7659520   2826240   10992  C:\WINDOWS\System32\WinLogon.exe
WinStore.App           4  846     873435136     33652736  26607616  55432  C:\Program Files\WindowsApps\Microsoft.WindowsStore_11712.1001.13.0_x64__8weky...
WmiPrvSE               0  201     2199100219392 8830976   3297280   10632  C:\WINDOWS\system32\wbem\wmiprvse.exe
WmiPrvSE               0  407     2199157727232 18509824  12922880  16624  C:\WINDOWS\system32\wbem\wmiprvse.exe
WUDFHost               0  834     2199310204928 51945472  87441408  24984  C:\Windows\System32\WUDFHost.exe
```

`Get-Process`コマンドレットは、プロセスオブジェクトをパイプラインからに送信し `Export-Csv` ます。 `Export-Csv`コマンドレットは、プロセスオブジェクトを CSV 文字列に変換し、その文字列を Processes.csv ファイルに保存します。 `Import-Csv`コマンドレットは、Processes.csv ファイルから CSV 文字列をインポートします。
文字列は変数に保存され `$P` ます。 変数は、インポートされた `$P` `Get-Member` CSV 文字列のプロパティを表示するコマンドレットにパイプラインから送信されます。 `$P`変数はパイプラインの下にコマンドレットに送信され `Format-Table` 、オブジェクトが表示されます。

### 例 2: 区切り記号を指定する

この例では、コマンドレットの **Delimiter** パラメーターを使用する方法を示し `Import-Csv` ます。

```powershell
Get-Process | Export-Csv -Path .\Processes.csv -Delimiter :
$P = Import-Csv -Path .\Processes.csv -Delimiter :
$P | Format-Table
```

`Get-Process`コマンドレットは、パイプラインのプロセスオブジェクトをに送信し `Export-Csv` ます。 `Export-Csv`コマンドレットは、プロセスオブジェクトを CSV 文字列に変換し、その文字列を Processes.csv ファイルに保存します。
**Delimiter** パラメーターは、コロン区切り記号を指定するために使用されます。 `Import-Csv`コマンドレットは、Processes.csv ファイルから CSV 文字列をインポートします。 文字列は変数に保存され `$P` ます。 To `$P` 変数は、パイプラインを介してコマンドレットに送信され `Format-Table` ます。

### 例 3: 区切り記号の現在のカルチャを指定する

この例では、UseCulture パラメーターを指定してコマンドレットを使用する方法を示し `Import-Csv` ます。 

```powershell
(Get-Culture).TextInfo.ListSeparator
Get-Process | Export-Csv -Path .\Processes.csv -UseCulture
Import-Csv -Path .\Processes.csv -UseCulture
```

この `Get-Culture` コマンドレットは、入れ子になったプロパティの **TextInfo** と **listseparator** を使用して、現在のカルチャの既定のリスト区切り記号を取得します。 `Get-Process`コマンドレットは、パイプラインのプロセスオブジェクトをに送信し `Export-Csv` ます。 `Export-Csv`コマンドレットは、プロセスオブジェクトを CSV 文字列に変換し、その文字列を Processes.csv ファイルに保存します。 **UseCulture** パラメーターは、現在のカルチャの既定のリスト区切り記号を使用します。 `Import-Csv`コマンドレットは、Processes.csv ファイルから CSV 文字列をインポートします。

### 例 4: インポートされたオブジェクトのプロパティ名を変更する

この例では、の **Header** パラメーターを使用して `Import-Csv` 、結果としてインポートされるオブジェクトのプロパティの名前を変更する方法を示します。

```powershell
Start-Job -ScriptBlock { Get-Process } | Export-Csv -Path .\Jobs.csv -NoTypeInformation
$Header = 'State', 'MoreData', 'StatusMessage', 'Location', 'Command', 'StateInfo', 'Finished', 'InstanceId', 'Id', 'Name', 'ChildJobs', 'BeginTime', 'EndTime', 'JobType', 'Output', 'Error', 'Progress', 'Verbose', 'Debug', 'Warning', 'Information'
# Delete the default header from file
$A = Get-Content -Path .\Jobs.csv
$A = $A[1..($A.Count - 1)]
$A | Out-File -FilePath .\Jobs.csv
$J = Import-Csv -Path .\Jobs.csv -Header $Header
$J
```

```Output
State         : Running
MoreData      : True
StatusMessage :
Location      : localhost
Command       : Get-Process
StateInfo     : Running
Finished      : System.Threading.ManualResetEvent
InstanceId    : a259eb63-6824-4b97-a033-305108ae1c2e
Id            : 1
Name          : Job1
ChildJobs     : System.Collections.Generic.List`1[System.Management.Automation.Job]
BeginTime     : 12/20/2018 18:59:57
EndTime       :
JobType       : BackgroundJob
Output        : System.Management.Automation.PSDataCollection`1[System.Management.Automation.PSObject]
Error         : System.Management.Automation.PSDataCollection`1[System.Management.Automation.ErrorRecord]
Progress      : System.Management.Automation.PSDataCollection`1[System.Management.Automation.ProgressRecord]
Verbose       : System.Management.Automation.PSDataCollection`1[System.Management.Automation.VerboseRecord]
Debug         : System.Management.Automation.PSDataCollection`1[System.Management.Automation.DebugRecord]
Warning       : System.Management.Automation.PSDataCollection`1[System.Management.Automation.WarningRecord]
Information   : System.Management.Automation.PSDataCollection`1[System.Management.Automation.InformationRecord]
```

コマンドレットでは、 `Start-Job` 実行するバックグラウンドジョブを開始し `Get-Process` ます。 ジョブオブジェクトは、パイプラインからコマンドレットに送信され、 `Export-Csv` CSV 文字列に変換されます。 **Notypeinformation** パラメーターは、CSV 出力から型情報ヘッダーを削除し、PowerShell Core では省略可能です。
変数には、 `$Header` **HasPSJobTypeName data**、 **jobstateinfo**、 **Psbegintime**、 **psendtime**、およびの既定値を置き換えるカスタムヘッダーが含まれています。 変数は、コマンドレットを使用して、 `$A` `Get-Content` Jobs.csv ファイルから CSV 文字列を取得します。 変数は、 `$A` ファイルから既定のヘッダーを削除するために使用されます。 この `Out-File` コマンドレットは、新しいバージョンの Jobs.csv ファイルを変数に保存し `$A` ます。 `Import-Csv`コマンドレットは、Jobs.csv ファイルをインポートし、 **Header** パラメーターを使用して変数を適用し `$Header` ます。 変数には、インポートされた `$J` **Pscustomobject** 変数が含まれ、PowerShell コンソールにオブジェクトが表示されます。

### 例 5: CSV ファイルを使用してカスタムオブジェクトを作成する

この例では、CSV ファイルを使用して PowerShell でカスタムオブジェクトを作成する方法を示します。

```powershell
Get-Content -Path .\Links.csv
```

```Output
113207,about_Aliases
113208,about_Arithmetic_Operators
113209,about_Arrays
113210,about_Assignment_Operators
113212,about_Automatic_Variables
113213,about_Break
113214,about_Command_Precedence
113215,about_Command_Syntax
144309,about_Comment_Based_Help
113216,about_CommonParameters
113217,about_Comparison_Operators
113218,about_Continue
113219,about_Core_Commands
113220,about_Data_Section
```

```powershell
$A = Import-Csv -Path .\Links.csv -Header 'LinkID', 'TopicTitle'
$A | Get-Member
```

```Output
   TypeName: System.Management.Automation.PSCustomObject

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
LinkID      NoteProperty string LinkID=113207
TopicTitle  NoteProperty string TopicTitle=about_Aliases
```

```powershell
$A | Where-Object -Property TopicTitle -Like '*alias*'
```

```Output
LinkID TopicTitle
------ ----------
113207 about_Aliases
```

Links.csv ファイルを作成するには、出力に示されている値を使用し `Get-Content` ます。

コマンドレットにより、 `Get-Content` Links.csv ファイルが表示されます。 この `Import-Csv` コマンドレットは、Links.csv ファイルをインポートします。 **Header** パラメーターは、 **LinkId** と **トピックタイトル** のプロパティ名を指定します。 オブジェクトは変数に格納され `$A` ます。 `Get-Member`コマンドレットは、**ヘッダー** パラメーターのプロパティ名を表示します。 コマンドレットでは、 `Where-Object` **エイリアス** を含む "**トピックのタイトル**" プロパティを使用してオブジェクトを選択します。

### 例 6: 値が不足している CSV をインポートする

この例は、 `Import-Csv` CSV ファイルのヘッダー行に null 値または空の値が含まれている場合に、PowerShell のコマンドレットがどのように応答するかを示しています。 `Import-Csv` が返すオブジェクトのプロパティ名になる、欠落しているヘッダー行の既定の名前に置き換え `Import-Csv` ます。

```powershell
Get-Content -Path .\Projects.csv
```

```Output
ProjectID,ProjectName,,Completed
13,Inventory,Redmond,True
440,,FarEast,True
469,Marketing,Europe,False
```

```powershell
Import-Csv -Path .\Projects.csv
```

```Output
WARNING: One or more headers were not specified. Default names starting with "H" have been used in place of any missing headers.

ProjectID ProjectName H1      Completed
--------- ----------- --      ---------
13        Inventory   Redmond True
440                   FarEast True
469       Marketing   Europe  False
```

```powershell
(Import-Csv -Path .\Projects.csv).H1
```

```Output
WARNING: One or more headers were not specified. Default names starting with "H" have been used in place of any missing headers.
Redmond
FarEast
Europe
```

Projects.csv ファイルを作成するには、例の出力に示されている値を使用し `Get-Content` ます。

コマンドレットにより、 `Get-Content` Projects.csv ファイルが表示されます。 ヘッダー行の **ProjectName** と **Completed** の間に値がありません。 この `Import-Csv` コマンドレットは、Projects.csv ファイルをインポートし、 **H1** が既定のヘッダー名であるため、警告メッセージを表示します。 この `(Import-Csv -Path
.\Projects.csv).H1` コマンドは、 **H1** プロパティ値を取得し、警告を表示します。

## PARAMETERS

### -区切り記号

CSV ファイル内のプロパティ値を区切る区切り記号を指定します。
既定値はコンマ (,) です。

コロン (:) などの文字を入力します。
セミコロン (;) を指定するには単一引用符で囲みます。

ファイルに実際の文字列区切り記号以外の文字を指定すると、は `Import-Csv` csv 文字列からオブジェクトを作成できず、csv 文字列が返されます。

```yaml
Type: System.Char
Parameter Sets: DelimiterPath, DelimiterLiteralPath
Aliases:

Required: False
Position: 1
Default value: comma (,)
Accept pipeline input: False
Accept wildcard characters: False
```

### -Encoding

インポートされた CSV ファイルのエンコードを指定します。 既定値は `utf8NoBOM` です。

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

> [!NOTE]
> **Utf-7** _ の使用は推奨されなくなりました。 PowerShell 7.1 では、 `utf7` _ *Encoding** パラメーターにを指定すると、警告が書き込まれます。

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

### -ヘッダー

インポートされたファイルの代替列ヘッダー行を指定します。 列ヘッダーは、によって作成されるオブジェクトのプロパティ名を決定し `Import-Csv` ます。

列ヘッダーをコンマ区切りのリストとして入力します。 ヘッダー文字列は引用符で囲まないでください。 各列ヘッダーを単一引用符で囲みます。

入力した列ヘッダーの数がデータ列の数を下回る場合は、残りのデータ列は破棄されます。 データ列よりも多くの列ヘッダーを入力すると、追加の列ヘッダーが空のデータ列で作成されます。

**Header** パラメーターを使用する場合は、CSV ファイルから元のヘッダー行を削除します。 それ以外の場合は、 `Import-Csv` ヘッダー行の項目から余分なオブジェクトを作成します。

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

### -LiteralPath

インポートする CSV ファイルのパスを指定します。 **Path** と異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: DelimiterLiteralPath, CultureLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

インポートする CSV ファイルのパスを指定します。
パイプを使用してパスをにパイプすることもでき `Import-Csv` ます。

```yaml
Type: System.String[]
Parameter Sets: DelimiterPath, CulturePath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -UseCulture

現在のカルチャの区切り記号を項目の区切り記号として使用します。 カルチャのリスト区切り記号を検索するには、コマンドを使用し `(Get-Culture).TextInfo.ListSeparator` ます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CulturePath, CultureLiteralPath
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、パスを含む文字列をにパイプすることができ `Import-Csv` ます。

## 出力

### Object

このコマンドレットは、CSV ファイル内のコンテンツによって記述されたオブジェクトを返します。

## 注

インポートされたオブジェクトはオブジェクト型の CSV バージョンであるため、オブジェクト型の非 CSV バージョンを書式設定する PowerShell の型書式設定エントリによって認識および書式設定されません。

コマンドの結果は、 `Import-Csv` テーブルに似たカスタムオブジェクトを形成する文字列のコレクションです。 各行は個別の文字列であるため、オブジェクトの **count** プロパティを使用して、テーブルの行をカウントできます。 列はオブジェクトのプロパティであり、行内の項目はプロパティの値です。

列ヘッダー行により、列の数と列名が決定されます。 列名は、オブジェクトのプロパティの名前でもあります。 **ヘッダー** パラメーターを使用して列ヘッダーを指定しない限り、最初の行は列ヘッダーとして解釈されます。 いずれかの行にヘッダー行よりも多くの値が含まれる場合、追加の値は無視されます。

列ヘッダー行に値が指定されていないか、null または空の値が含まれている場合、では、欠落している `Import-Csv` 列ヘッダーおよびプロパティ名に対して、 **H** の後に数字が使用されます。

CSV ファイルでは、各オブジェクトは、オブジェクトのプロパティ値のコンマ区切りリストで表されます。 プロパティ値は、オブジェクトの **ToString ()** メソッドを使用して文字列に変換されるので、プロパティ値の名前で表されます。 `Export-Csv` では、オブジェクトのメソッドはエクスポートされません。

`Import-Csv` では、W3C 拡張ログ形式もサポートされています。 で始まる行 `#` はコメントとして扱われ、コメントがで始まる場合 `#Fields:` や、区切られた列名の一覧が含まれている場合を除き、無視されます。 この場合、コマンドレットはこれらの列名を使用します。 これは、Windows IIS およびその他の web サーバーログの標準形式です。 詳細については、「 [拡張ログファイルの形式](https://www.w3.org/TR/WD-logfile.html)」を参照してください。

## 関連リンク

[ConvertFrom-Csv](ConvertFrom-Csv.md)

[ConvertTo-Csv](ConvertTo-Csv.md)

[エクスポート-Csv](Export-Csv.md)

[Get-Culture](Get-Culture.md)

