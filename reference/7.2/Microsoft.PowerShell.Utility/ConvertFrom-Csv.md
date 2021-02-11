---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 12/21/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-csv?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-Csv
ms.openlocfilehash: 951a1e4ecc46b401ae066bc3b138f264a4313e65
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604946"
---
# ConvertFrom-Csv

## 概要
コンマ区切り値 (CSV) 形式のオブジェクトのプロパティを元のオブジェクトの CSV バージョンに変換します。

## SYNTAX

### 区切り記号 (既定値)

```
ConvertFrom-Csv [[-Delimiter] <Char>] [-InputObject] <PSObject[]> [-Header <String[]>]
 [<CommonParameters>]
```

### UseCulture

```
ConvertFrom-Csv -UseCulture [-InputObject] <PSObject[]> [-Header <String[]>] [<CommonParameters>]
```

## Description

`ConvertFrom-Csv`コマンドレットでは、コマンドレットによって生成される CSV 可変長文字列からオブジェクトを作成し `ConvertTo-Csv` ます。

このコマンドレットのパラメーターを使用して、結果として生成されるオブジェクトのプロパティ名を決定する列ヘッダー行を指定したり、項目の区切り記号を指定したり、現在のカルチャの区切り記号を区切り記号として使用するようにこのコマンドレットに指示したりできます。

によって作成されるオブジェクトは、 `ConvertFrom-Csv` 元のオブジェクトの CSV バージョンです。 CSV オブジェクトのプロパティ値は、元のオブジェクトのプロパティ値の文字列バージョンです。 オブジェクトの CSV バージョンはメソッドを持ちません。

また、 `Export-Csv` `Import-Csv` コマンドレットとコマンドレットを使用して、オブジェクトをファイル内の CSV 文字列に変換することもできます。 これらのコマンドレットは、 `ConvertTo-Csv` `ConvertFrom-Csv` コマンドレットとコマンドレットと同じですが、CSV 文字列をファイルに保存する点が異なります。

## 例

### 例 1: ローカルコンピューター上のプロセスを CSV 形式に変換する

この例では、ローカルコンピューター上のプロセスを CSV 形式に変換してから、オブジェクト形式に復元する方法を示します。

```powershell
$P = Get-Process | ConvertTo-Csv
$P | ConvertFrom-Csv
```

`Get-Process`コマンドレットは、パイプラインの下にあるプロセスをに送信し `ConvertTo-Csv` ます。 コマンドレットでは、 `ConvertTo-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。 コマンドレットにより、 `ConvertFrom-Csv` csv 文字列が元のプロセスオブジェクトの csv バージョンに変換されます。 CSV 文字列は変数に保存され `$P` ます。

### 例 2: データオブジェクトを CSV 形式に変換してから CSV オブジェクト形式に変換する

この例では、データオブジェクトを CSV 形式に変換した後、CSV オブジェクト形式に変換する方法を示します。

```powershell
$Date = Get-Date | ConvertTo-Csv -Delimiter ';'
ConvertFrom-Csv -InputObject $Date -Delimiter ';'
```

最初のコマンドは、を使用して、 `Get-Date` パイプラインの現在の日付と時刻をに送信し `ConvertTo-Csv` ます。 この `ConvertTo-Csv` コマンドレットは、date オブジェクトを一連の CSV 文字列に変換します。
**区切り** 記号パラメーターは、セミコロンの区切り記号を指定するために使用されます。 文字列は変数に保存され `$Date` ます。

### 例 3: header パラメーターを使用してプロパティの名前を変更する

この例では、の **Header** パラメーターを使用して `ConvertFrom-Csv` 、結果としてインポートされるオブジェクトのプロパティの名前を変更する方法を示します。

```powershell
$J = Start-Job -ScriptBlock { Get-Process } | ConvertTo-Csv  -NoTypeInformation
$Header = 'State', 'MoreData', 'StatusMessage', 'Location', 'Command', 'StateInfo', 'Finished', 'InstanceId', 'Id', 'Name', 'ChildJobs', 'BeginTime', 'EndTime', 'JobType', 'Output', 'Error', 'Progress', 'Verbose', 'Debug', 'Warning', 'Information'
# Delete the default header from $J
$J = $J[1..($J.count - 1)]
$J | ConvertFrom-Csv -Header $Header
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

コマンドレットでは、 `Start-Job` 実行するバックグラウンドジョブを開始し `Get-Process` ます。 ジョブオブジェクトは、パイプラインからに送信され、 `ConvertTo-Csv` CSV 文字列に変換されます。 **Notypeinformation** パラメーターは、CSV 出力から型情報ヘッダーを削除し、PowerShell Core では省略可能です。 変数には、 `$Header` **HasPSJobTypeName data**、 **jobstateinfo**、 **Psbegintime**、 **psendtime**、およびの既定値を置き換えるカスタムヘッダーが含まれています。 この `$J` 変数には CSV 文字列が含まれており、既定のヘッダーを削除するために使用されます。 コマンドレットにより、 `ConvertFrom-Csv` CSV 文字列が **Pscustomobject** 変換され、 **Header** パラメーターを使用して変数が適用され `$Header` ます。

### 例 4: サービスオブジェクトの CSV 文字列を変換する

この例では、UseCulture パラメーターを指定してコマンドレットを使用する方法を示し `ConvertFrom-Csv` ます。 

```powershell
(Get-Culture).TextInfo.ListSeparator
$Services = (Get-Service | ConvertTo-Csv)
ConvertFrom-Csv -InputObject $Services -UseCulture
```

この `Get-Culture` コマンドレットは、入れ子になったプロパティの **TextInfo** と **listseparator** を使用して、現在のカルチャの既定のリスト区切り記号を取得します。 `Get-Service`コマンドレットは、サービスオブジェクトをパイプラインからに送信し `ConvertTo-Csv` ます。 は、 `ConvertTo-Csv` サービスオブジェクトを一連の CSV 文字列に変換します。 CSV 文字列は変数に格納され `$Services` ます。 コマンドレットでは、 `ConvertFrom-Csv` **InputObject** パラメーターを使用して、変数から CSV 文字列を変換し `$Services` ます。 **UseCulture** パラメーターは、現在のカルチャの既定のリスト区切り記号を使用します。

**UseCulture** パラメーターを使用する場合は、現在のカルチャの既定のリスト区切り記号が、CSV 文字列で使用されている区切り記号と一致していることを確認してください。 それ以外の場合、は `ConvertFrom-Csv` CSV 文字列からオブジェクトを生成できません。

## PARAMETERS

### -区切り記号

CSV 文字列内のプロパティ値を区切る区切り記号を指定します。
既定値はコンマ (,) です。

コロン (:) などの文字を入力します。
セミコロン (;) を指定するには単一引用符で囲みます。

ファイルに実際の文字列区切り記号以外の文字を指定すると、は `ConvertFrom-Csv` csv 文字列からオブジェクトを作成できず、csv 文字列が返されます。

```yaml
Type: System.Char
Parameter Sets: Delimiter
Aliases:

Required: False
Position: 1
Default value: comma (,)
Accept pipeline input: False
Accept wildcard characters: False
```

### -ヘッダー

インポートされた文字列の代替列ヘッダー行を指定します。 列ヘッダーは、によって作成されるオブジェクトのプロパティ名を決定し `ConvertFrom-Csv` ます。

列ヘッダーをコンマ区切りのリストとして入力します。 ヘッダー文字列は引用符で囲まないでください。 各列ヘッダーを単一引用符で囲みます。

入力した列ヘッダーの数がデータ列の数を下回る場合は、残りのデータ列は破棄されます。 データ列よりも多くの列ヘッダーを入力すると、追加の列ヘッダーが空のデータ列で作成されます。

**Header** パラメーターを使用する場合は、CSV 文字列から列ヘッダー文字列を省略します。 それ以外の場合、このコマンドレットはヘッダー行の項目から追加のオブジェクトを作成します。

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

### -InputObject

オブジェクトに変換する CSV 文字列を指定します。 CSV 文字列が格納されている変数を入力するか、CSV 文字列を取得するコマンドまたは式を入力します。 また、CSV 文字列をにパイプ処理することもでき `ConvertFrom-Csv` ます。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
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
Parameter Sets: UseCulture
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

このコマンドレットには、CSV 文字列をパイプすることができます。

## 出力

### システム管理. PSObject

このコマンドレットは、CSV 文字列のプロパティによって記述されたオブジェクトを返します。

## 注

インポートされたオブジェクトはオブジェクト型の CSV バージョンであるため、オブジェクト型の非 CSV バージョンを書式設定する PowerShell の型書式設定エントリによって認識および書式設定されません。

CSV 形式では、各オブジェクトは、オブジェクトのプロパティ値のコンマ区切りリストで表されます。 プロパティ値は、オブジェクトの **ToString ()** メソッドを使用して文字列に変換されるため、プロパティ値の名前で表されます。 このコマンドレットでは、オブジェクトのメソッドはエクスポートされません。

## 関連リンク

[ConvertTo-Csv](ConvertTo-Csv.md)

[エクスポート-Csv](Export-Csv.md)

[Import-Csv](Import-Csv.md)

