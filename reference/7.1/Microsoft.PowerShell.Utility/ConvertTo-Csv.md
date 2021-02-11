---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 12/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertto-csv?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertTo-Csv
ms.openlocfilehash: 7ad8dc837bd843c2df48587ad809d9f65a4cf8a7
ms.sourcegitcommit: 560a9f3c3148acab4655e91e8b07745ab74d5d26
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "96913221"
---
# ConvertTo-Csv

## 概要
.NET オブジェクトを一連の文字区切り値 (CSV) 文字列に変換します。

## SYNTAX

### 区切り記号 (既定値)

```
ConvertTo-Csv [-InputObject] <PSObject> [[-Delimiter] <Char>] [-IncludeTypeInformation]
 [-NoTypeInformation] [-QuoteFields <String[]>] [-UseQuotes <QuoteKind>] [<CommonParameters>]
```

### UseCulture

```
ConvertTo-Csv [-InputObject] <PSObject> [-UseCulture] [-IncludeTypeInformation] [-NoTypeInformation]
 [-QuoteFields <String[]>] [-UseQuotes <QuoteKind>] [<CommonParameters>]
```

## Description

`ConvertTo-CSV`コマンドレットは、送信したオブジェクトを表す一連のコンマ区切り値 (CSV) 文字列を返します。 その後、コマンドレットを使用して `ConvertFrom-Csv` 、CSV 文字列からオブジェクトを再作成できます。 CSV から変換されたオブジェクトは、プロパティ値とメソッドを含まない元のオブジェクトの文字列値です。

コマンドレットを使用して、 `Export-Csv` オブジェクトを CSV 文字列に変換できます。 `Export-CSV` はと似てい `ConvertTo-CSV` ますが、CSV 文字列をファイルに保存する点が異なります。

コマンドレットには、 `ConvertTo-CSV` コンマ以外の区切り記号を指定するパラメーターや、現在のカルチャを区切り記号として使用するパラメーターがあります。

## 例

### 例 1: オブジェクトを CSV に変換する

この例では、 **プロセス** オブジェクトを CSV 文字列に変換します。

```powershell
Get-Process -Name pwsh | ConvertTo-Csv -NoTypeInformation
```

```Output
"Name","SI","Handles","VM","WS","PM","NPM","Path","Parent","Company","CPU","FileVersion", ...
"pwsh","8","950","2204001161216","100925440","59686912","67104", ...
```

コマンドレットでは、 `Get-Process` **process** オブジェクトを取得し、 **Name** パラメーターを使用して PowerShell プロセスを指定します。 プロセスオブジェクトは、パイプラインの下にコマンドレットに送信され `ConvertTo-CSV` ます。 `ConvertTo-CSV`コマンドレットでは、オブジェクトを CSV 文字列に変換します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。

### 例 2: DateTime オブジェクトを CSV に変換する

この例では、 **DateTime** オブジェクトを CSV 文字列に変換します。

```powershell
$Date = Get-Date
ConvertTo-Csv -InputObject $Date -Delimiter ';' -NoTypeInformation
```

```Output
"DisplayHint";"DateTime";"Date";"Day";"DayOfWeek";"DayOfYear";"Hour";"Kind";"Millisecond";"Minute";"Month";"Second";"Ticks";"TimeOfDay";"Year"
"DateTime";"Friday, January 4, 2019 14:40:51";"1/4/2019 00:00:00";"4";"Friday";"4";"14";"Local";"711";"40";"1";"51";"636822096517114991";"14:40:51.7114991";"2019"
```

`Get-Date`このコマンドレットは、 **DateTime** オブジェクトを取得し、変数に保存し `$Date` ます。 この `ConvertTo-Csv` コマンドレットは、 **DateTime** オブジェクトを文字列に変換します。 **InputObject** パラメーターは、変数に格納されている **DateTime** オブジェクトを使用し `$Date` ます。 **Delimiter** パラメーターでは、文字列値を区切るためのセミコロンを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。

### 例 3: PowerShell イベントログを CSV に変換する

この例では、PowerShell の Windows イベントログを一連の CSV 文字列に変換します。

```powershell
(Get-Culture).TextInfo.ListSeparator
Get-WinEvent -LogName 'PowerShellCore/Operational' | ConvertTo-Csv -UseCulture -NoTypeInformation
```

```Output
,
"Message","Id","Version","Qualifiers","Level","Task","Opcode","Keywords","RecordId", ...
"Error Message = System error""4100","1",,"3","106","19","0","31716","PowerShellCore", ...
```

この `Get-Culture` コマンドレットは、入れ子になったプロパティの **TextInfo** と **listseparator** を使用し、現在のカルチャの既定のリスト区切り記号を表示します。 `Get-WinEvent`コマンドレットは、イベントログオブジェクトを取得し、 **LogName** パラメーターを使用してログファイル名を指定します。 イベントログオブジェクトは、パイプラインを介してコマンドレットに送信され `ConvertTo-Csv` ます。 `ConvertTo-Csv`コマンドレットは、イベントログオブジェクトを一連の CSV 文字列に変換します。 **UseCulture** パラメーターは、現在のカルチャの既定のリスト区切り記号を区切り記号として使用します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。

### 例 4: 2 つの列を囲む引用符を使用して CSV に変換する

この例では、 **DateTime** オブジェクトを CSV 文字列に変換します。

```powershell
Get-Date | ConvertTo-Csv -QuoteFields "DateTime","Date"
```

```Output
DisplayHint,"DateTime","Date",Day,DayOfWeek,DayOfYear,Hour,Kind,Millisecond,Minute,Month,Second,Ticks,TimeOfDay,Year
DateTime,"Thursday, August 22, 2019 11:27:34 AM","8/22/2019 12:00:00 AM",22,Thursday,234,11,Local,569,27,8,34,637020700545699784,11:27:34.5699784,2019
```

### 例 4: 必要な場合にのみ引用符を使用して CSV に変換する

この例では、 **DateTime** オブジェクトを CSV 文字列に変換します。

```powershell
Get-Date | ConvertTo-Csv -UseQuotes AsNeeded
```

```Output
DisplayHint,DateTime,Date,Day,DayOfWeek,DayOfYear,Hour,Kind,Millisecond,Minute,Month,Second,Ticks,TimeOfDay,Year
DateTime,"Thursday, August 22, 2019 11:31:00 AM",8/22/2019 12:00:00 AM,22,Thursday,234,11,Local,713,31,8,0,637020702607132640,11:31:00.7132640,2019
```

## PARAMETERS

### -区切り記号

CSV 文字列内のプロパティ値を区切る区切り記号を指定します。 既定値はコンマ ( `,` ) です。 コロン () などの文字を入力し `:` ます。 セミコロン () を指定するには、 `;` 単一引用符で囲みます。

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

### -IncludeTypeInformation

このパラメーターを使用する場合、出力の最初の行には **#TYPE** が含まれ、その後にオブジェクト型の完全修飾名が続きます。 たとえば、 **#TYPE** のようにします。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: ITI

Required: False
Position: Named
Default value: #TYPE <Object>
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

CSV 文字列に変換されるオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。 パイプを使用してオブジェクトをにパイプ処理することもでき `ConvertTo-CSV` ます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -NoTypeInformation

出力から **#TYPE** 情報ヘッダーを削除します。 このパラメーターは、PowerShell 6.0 の既定値になり、下位互換性のために用意されています。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NTI

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseCulture

現在のカルチャの区切り記号を項目の区切り記号として使用します。 カルチャのリスト区切り記号を検索するには、コマンドを使用し `(Get-Culture).TextInfo.ListSeparator` ます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: UseCulture
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -QuoteFields

引用符で囲む必要がある列の名前を指定します。 このパラメーターを使用する場合は、指定した列のみが引用符で囲まれます。 このパラメーターは、PowerShell 7.0 で追加されました。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: QF

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Usotes

CSV ファイルで引用符を使用するかどうかを指定します。 次のいずれかの値になります。

- なし-何も引用しない
- すべてを常に引用符で囲む (既定の動作)
- AsNeeded-区切り文字を含む quote フィールドのみ

このパラメーターは、PowerShell 7.0 で追加されました。

```yaml
Type: Microsoft.PowerShell.Commands.BaseCsvWritingCommand+QuoteKind
Parameter Sets: (All)
Aliases: UQ

Required: False
Position: Named
Default value: Always
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して、拡張型システム (実行) アダプターを持つ任意のオブジェクトをにパイプでき `ConvertTo-CSV` ます。

## 出力

### System.String

CSV 出力は、文字列のコレクションとして返されます。

## 注

CSV 形式では、各オブジェクトはそのプロパティ値のコンマ区切りリストで表されます。 プロパティ値は、オブジェクトの **ToString ()** メソッドを使用して文字列に変換されます。 文字列は、プロパティ値の名前で表されます。 `ConvertTo-CSV` では、オブジェクトのメソッドはエクスポートされません。

CSV 文字列は次のように出力されます。

- **Includetypeinformation** を使用する場合、最初の文字列は **#TYPE** で構成され、その後にオブジェクト型の完全修飾名が続きます。 たとえば、 **#TYPE** のようにします。
- **Includetypeinformation** が使用されていない場合、最初の文字列には列ヘッダーが含まれます。 ヘッダーには、コンマ区切りのリストとして、最初のオブジェクトのプロパティ名が含まれます。
- 残りの文字列には、各オブジェクトのプロパティ値のコンマ区切りのリストが含まれます。

PowerShell 6.0 以降では、の既定の動作で `ConvertTo-CSV` は、 **#TYPE** 情報が CSV に含められず、 **notypeinformation** が暗黙的に使用されます。 **Includetypeinformation** を使用して **#TYPE** 情報を含め、PowerShell 6.0 より前のの既定の動作をエミュレートすることができ `ConvertTo-CSV` ます。

複数のオブジェクトをに送信すると `ConvertTo-CSV` 、は、 `ConvertTo-CSV` 最初に送信したオブジェクトのプロパティに基づいて文字列を並べ替えます。 残りのオブジェクトに指定されたプロパティのいずれかがない場合、そのオブジェクトのプロパティ値は、連続する2つのコンマで表される Null になります。 残りのオブジェクトに追加のプロパティがある場合、これらのプロパティ値は無視されます。

## 関連リンク

[ConvertFrom-Csv](ConvertFrom-Csv.md)

[エクスポート-Csv](Export-Csv.md)

[Import-Csv](Import-Csv.md)
