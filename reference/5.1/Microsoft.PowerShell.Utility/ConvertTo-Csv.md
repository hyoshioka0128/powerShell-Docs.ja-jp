---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 1/7/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertto-csv?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertTo-Csv
ms.openlocfilehash: be590368539f396f0aac694e9565674393543f2c
ms.sourcegitcommit: 560a9f3c3148acab4655e91e8b07745ab74d5d26
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "96913204"
---
# ConvertTo-Csv

## 概要
.NET オブジェクトを一連のコンマ区切り値 (CSV) 文字列に変換します。

## SYNTAX

### 区切り記号 (既定値)

```
ConvertTo-Csv [-InputObject] <psobject> [[-Delimiter] <char>] [-NoTypeInformation]
 [<CommonParameters>]
```

### UseCulture

```
ConvertTo-Csv [-InputObject] <psobject> [-UseCulture] [-NoTypeInformation] [<CommonParameters>]
```

## Description

`ConvertTo-CSV`コマンドレットは、送信したオブジェクトを表す一連のコンマ区切り値 (CSV) 文字列を返します。 その後、コマンドレットを使用して `ConvertFrom-Csv` 、CSV 文字列からオブジェクトを再作成できます。 CSV から変換されたオブジェクトは、プロパティ値とメソッドを含まない元のオブジェクトの文字列値です。

コマンドレットを使用して、 `Export-Csv` オブジェクトを CSV 文字列に変換できます。 `Export-CSV` はと似てい `ConvertTo-CSV` ますが、CSV 文字列をファイルに保存する点が異なります。

コマンドレットには、 `ConvertTo-CSV` コンマ以外の区切り記号を指定するパラメーターや、現在のカルチャを区切り記号として使用するパラメーターがあります。

## 例

### 例 1: オブジェクトを CSV に変換する

この例では、 **プロセス** オブジェクトを CSV 文字列に変換します。

```powershell
Get-Process -Name 'PowerShell' | ConvertTo-Csv -NoTypeInformation
```

```Output
"Name","SI","Handles","VM","WS","PM","NPM","Path","Company","CPU","FileVersion", ...
"powershell","11","691","2204036739072","175943680","132665344","33312", ...
```

コマンドレットでは、 `Get-Process` **process** オブジェクトを取得し、 **Name** パラメーターを使用して PowerShell プロセスを指定します。 プロセスオブジェクトは、パイプラインの下にコマンドレットに送信され `ConvertTo-CSV` ます。 `ConvertTo-CSV`コマンドレットでは、オブジェクトを CSV 文字列に変換します。 **Notypeinformation** パラメーターは、CSV 出力から **#TYPE** 情報ヘッダーを削除します。

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

`Get-Date`このコマンドレットは、 **DateTime** オブジェクトを取得し、変数に保存し `$Date` ます。 この `ConvertTo-Csv` コマンドレットは、 **DateTime** オブジェクトを文字列に変換します。 **InputObject** パラメーターは、変数に格納されている **DateTime** オブジェクトを使用し `$Date` ます。 **Delimiter** パラメーターでは、文字列値を区切るためのセミコロンを指定します。 **Notypeinformation** パラメーターは、CSV 出力から **#TYPE** 情報ヘッダーを削除します。

### 例 3: PowerShell イベントログを CSV に変換する

この例では、PowerShell の Windows イベントログを一連の CSV 文字列に変換します。

```powershell
(Get-Culture).TextInfo.ListSeparator
Get-WinEvent -LogName 'Windows PowerShell' | ConvertTo-Csv -UseCulture -NoTypeInformation
```

```Output
,
"Message","Id","Version","Qualifiers","Level","Task","Opcode","Keywords","RecordId", ...
"Error Message = System error","403",,"0","4","4",,"36028797018963968","46891","PowerShell", ...
```

この `Get-Culture` コマンドレットは、入れ子になったプロパティの **TextInfo** と **listseparator** を使用し、現在のカルチャの既定のリスト区切り記号を表示します。 `Get-WinEvent`コマンドレットは、イベントログオブジェクトを取得し、 **LogName** パラメーターを使用してログファイル名を指定します。 イベントログオブジェクトは、パイプラインを介してコマンドレットに送信され `ConvertTo-Csv` ます。 `ConvertTo-Csv`コマンドレットは、イベントログオブジェクトを一連の CSV 文字列に変換します。 **UseCulture** パラメーターは、現在のカルチャの既定のリスト区切り記号を区切り記号として使用します。 **Notypeinformation** パラメーターは、CSV 出力から **#TYPE** 情報ヘッダーを削除します。

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

- 既定では、最初の文字列には **#TYPE** 情報ヘッダーの後にオブジェクトの種類の完全修飾名が含まれます。 たとえば、 **#TYPE** のようにします。
- **Notypeinformation** が使用されている場合、最初の文字列には列ヘッダーが含まれます。 ヘッダーには、コンマ区切りのリストとして、最初のオブジェクトのプロパティ名が含まれます。
- 残りの文字列には、各オブジェクトのプロパティ値のコンマ区切りのリストが含まれます。

複数のオブジェクトをに送信すると `ConvertTo-CSV` 、は、 `ConvertTo-CSV` 最初に送信したオブジェクトのプロパティに基づいて文字列を並べ替えます。 残りのオブジェクトに指定されたプロパティのいずれかがない場合、そのオブジェクトのプロパティ値は、連続する2つのコンマで表される Null になります。 残りのオブジェクトに追加のプロパティがある場合、これらのプロパティ値は無視されます。

## 関連リンク

[ConvertFrom-Csv](ConvertFrom-Csv.md)

[エクスポート-Csv](Export-Csv.md)

[Import-Csv](Import-Csv.md)
