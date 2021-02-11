---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/25/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-date?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Date
ms.openlocfilehash: bb7f169e0c01c73bb4e834375f341bf795e37ce7
ms.sourcegitcommit: f5986121386c81acddcf324eb0526d7d092bcc8f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/20/2021
ms.locfileid: "98584666"
---
# Get-Date

## 概要
現在の日付と時刻を取得します。

## SYNTAX

### net (既定値)

```
Get-Date [[-Date] <DateTime>] [-Year <Int32>] [-Month <Int32>] [-Day <Int32>] [-Hour <Int32>]
 [-Minute <Int32>] [-Second <Int32>] [-Millisecond <Int32>] [-DisplayHint <DisplayHintType>]
 [-Format <String>] [<CommonParameters>]
```

### UFormat

```
Get-Date [[-Date] <DateTime>] [-Year <Int32>] [-Month <Int32>] [-Day <Int32>] [-Hour <Int32>]
 [-Minute <Int32>] [-Second <Int32>] [-Millisecond <Int32>] [-DisplayHint <DisplayHintType>]
 [-UFormat <String>] [<CommonParameters>]
```

## Description

この `Get-Date` コマンドレットは、現在の日付または指定した日付を表す **DateTime** オブジェクトを取得します。 `Get-Date` では、日付と時刻をいくつかの .NET 形式と UNIX 形式で書式設定できます。 を使用し `Get-Date` て、日付または時刻の文字列を生成し、他のコマンドレットまたはプログラムに文字列を送信することができます。

`Get-Date` コンピューターのカルチャ設定を使用して、出力形式を決定します。 コンピューターの設定を表示するには、を使用 `(Get-Culture).DateTimeFormat` します。

## 例

### 例 1: 現在の日付と時刻を取得する

この例では、は `Get-Date` 現在のシステム日付と時刻を表示します。 出力は、長い日付形式と長い時刻形式です。

```powershell
Get-Date
```

```Output
Tuesday, June 25, 2019 14:53:32
```

### 例 2: 現在の日付と時刻の要素を取得する

この例 `Get-Date` では、を使用して、日付要素または時刻要素を取得する方法を示します。 パラメーターでは、引数 **Date**、 **Time**、または **DateTime** を使用します。

```powershell
Get-Date -DisplayHint Date
```

```Output
Tuesday, June 25, 2019
```

`Get-Date`**Displayhint** パラメーターを **date** 引数と共に使用して、日付のみを取得します。

### 例 3: .NET 書式指定子を使用して日付と時刻を取得する

この例では、.NET 形式指定子を使用して、出力の形式をカスタマイズします。 出力は **文字列** オブジェクトです。

```powershell
Get-Date -Format "dddd MM/dd/yyyy HH:mm K"
```

```Output
Tuesday 06/25/2019 16:17 -07:00
```

`Get-Date`**format** パラメーターを使用して、複数の書式指定子を指定します。

この例で使用される .NET 形式指定子は、次のように定義されています。

| 指定子 | 定義 |
| --- | --- |
| `dddd` | 曜日-完全名 |
| `MM` | 月の番号 |
| `dd` | 月の通算日-2 桁 |
| `yyyy` | 4桁表記の年 |
| `HH:mm` | 24時間形式の時刻-秒なし |
| `K` | 世界協定時刻 (UTC) からのタイムゾーンオフセット |

.NET 書式指定子の詳細については、「 [カスタム日時書式指定文字列](/dotnet/standard/base-types/custom-date-and-time-format-strings?view=netframework-4.8)」を参照してください。

### 例 4: UFormat 指定子を使用して日付と時刻を取得する

この例では、出力の形式をカスタマイズするために、いくつかの **Uformat** 書式指定子が使用されています。
出力は **文字列** オブジェクトです。

```powershell
Get-Date -UFormat "%A %m/%d/%Y %R %Z"
```

```Output
Tuesday 06/25/2019 16:19 -07
```

`Get-Date`**uformat** パラメーターを使用して、複数の書式指定子を指定します。

この例で使用されている **Uformat** 書式指定子は、次のように定義されています。

| 指定子 | 定義 |
| --- | --- |
| `%A` | 曜日-完全名 |
| `%m` | 月の番号 |
| `%d` | 月の通算日-2 桁 |
| `%Y` | 4桁表記の年 |
| `%R` | 24時間形式の時刻-秒なし |
| `%Z` | 世界協定時刻 (UTC) からのタイムゾーンオフセット |

有効な **Uformat** 書式指定子の一覧については、「 [メモ](#notes) 」を参照してください。

### 例 5: 年の日付を取得する

この例では、プロパティを使用して、年の数値を取得します。

グレゴリオ暦には、366日を持つ閏年を除き、365日が含まれています。 たとえば、2020年12月31日は366日です。

```powershell
(Get-Date -Year 2020 -Month 12 -Day 31).DayOfYear
```

```Output
366
```

`Get-Date` は、 **年**、 **月**、 **日** という3つのパラメーターを使用して日付を指定します。 このコマンドは、結果が **DayofYear** プロパティによって評価されるように、かっこで囲まれています。

### 例 6: 夏時間に合わせて日付を調整するかどうかを確認する

この例では、ブール型のメソッドを使用して、日付が夏時間によって調整されているかどうかを確認します。

```powershell
$DST = Get-Date
$DST.IsDaylightSavingTime()
```

```Output
True
```

変数は、 `$DST` の結果を格納し `Get-Date` ます。 `$DST`**IsDaylightSavingTime** メソッドを使用して、日付が夏時間に合わせて調整されているかどうかをテストします。

### 例 7: 現在の時刻を UTC 時刻に変換する

この例では、現在の時刻が UTC 時刻に変換されます。 時刻の変換には、システムのロケールの UTC オフセットが使用されます。 「 [Notes](#notes) 」セクションの表に、有効な **uformat** 書式指定子の一覧を示します。

```powershell
Get-Date -UFormat "%A %B/%d/%Y %T %Z"
$Time = Get-Date
$Time.ToUniversalTime()
```

```Output
Wednesday June/26/2019 10:45:26 -07

Wednesday, June 26, 2019 17:45:26
```

`Get-Date`**uformat** パラメーターを書式指定子と共に使用して、現在のシステムの日付と時刻を表示します。 書式指定子 **% Z** は、 **-07** の UTC オフセットを表します。

変数には、 `$Time` 現在のシステムの日付と時刻が格納されます。 `$Time`**ToUniversalTime ()** メソッドを使用して、時刻をコンピューターの UTC オフセットに基づいて変換します。

### 例 8: タイムスタンプを作成する

この例では、書式指定子によって、ディレクトリ名のタイムスタンプ **文字列** オブジェクトが作成されます。 タイムスタンプには、日付、時刻、および UTC オフセットが含まれます。

```powershell
$timestamp = Get-Date -Format o | ForEach-Object { $_ -replace ":", "." }
New-Item -Path C:\Test\$timestamp -Type Directory
```

```Output
    Directory: C:\Test

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         6/27/2019    07:59                2019-06-27T07.59.24.4603750-07.00
```

変数は、 `$timestamp` コマンドの結果を格納し `Get-Date` ます。 `Get-Date`**format パラメーターを** 小文字の書式指定子と共に使用して、 `o` タイムスタンプ **文字列** オブジェクトを作成します。 オブジェクトは、パイプライン内でに送信され `ForEach-Object` ます。 **ScriptBlock** には、 `$_` 現在のパイプラインオブジェクトを表す変数が含まれています。 タイムスタンプ文字列は、ピリオドで置き換えられたコロンで区切られます。

`New-Item`**Path** パラメーターを使用して、新しいディレクトリの場所を指定します。 パスには、 `$timestamp` ディレクトリ名として変数が含まれます。 **型** パラメーターは、ディレクトリが作成されることを指定します。

## PARAMETERS

### -日付

日付と時刻を指定します。 Time は省略可能で、指定されていない場合は00:00:00 を返します。

システムロケールの標準形式で日付と時刻を入力します。

たとえば、米国英語の場合は、次のようになります。

`Get-Date -Date "6/25/2019 12:30:22"` 2019年6月25日火曜日、12:30:22 を返します

```yaml
Type: System.DateTime
Parameter Sets: (All)
Aliases: LastWriteTime

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -日

表示されている月の日にちを指定します。 1 ～ 31 の値を入力します。

指定された値が月の日数よりも大きい場合、PowerShell はその月に日数を加算します。 たとえば、には `Get-Date -Month 2 -Day 31` **2 月31日** ではなく **3 月 3** 日が表示されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisplayHint

表示する日付と時刻の要素を決定します。

許容される値は次のとおりです。

- **日付**: 日付のみを表示します
- **Time**: 時刻のみを表示します
- **DateTime**: 日付と時刻を表示します。

```yaml
Type: Microsoft.PowerShell.Commands.DisplayHintType
Parameter Sets: (All)
Aliases:
Accepted values: Date, Time, DateTime

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -形式

書式指定子で示される Microsoft .NET Framework 形式で日付と時刻を表示します。
**Format** パラメーターは、**文字列** オブジェクトを出力します。

使用可能な .NET 書式指定子の一覧については、「 [カスタム日時書式指定文字列](/dotnet/standard/base-types/custom-date-and-time-format-strings?view=netframework-4.8)」を参照してください。

**Format** パラメーターを使用する場合は、 `Get-Date` 日付を表示するために必要な **DateTime** オブジェクトのプロパティのみが取得されます。 その結果、**DateTime** オブジェクトの一部のプロパティとメソッドを利用できなくなる場合があります。

PowerShell 5.0 以降では、 **Format** パラメーターの値として次の追加形式を使用できます。

- **Filedate**。 現在の日付を現地時刻で表した、ファイルまたはパスによるわかりやすい表現。 形式は、 `yyyyMMdd` (大文字と小文字を区別します。4桁の年、2桁の月、および2桁の日) を使用します。 以下に例を示します。
  20190627.

- **Filedateuniversal**。 現在の日付を世界協定時刻 (UTC) で表した、ファイルまたはパスのフレンドリ表現。 形式は、 `yyyyMMddZ` (大文字と小文字を区別します。4桁の年、2桁の月、2桁の日、および UTC インジケーターとしての文字を使用し `Z` ます)。 例: 20190627Z。

- **Filedatetime**。 ローカル時刻の現在の日付と時刻を24時間形式で表した、ファイルまたはパスのフレンドリ表現。 形式は、 `yyyyMMddTHHmmssffff` (大文字と小文字を区別し、4桁の年、2桁の月、2桁の日、時刻の区切り記号、2桁の時間、2桁の分、2桁の秒 `T` 、および4桁のミリ秒) を使用します。 例: 20190627T0840107271。

- **FileDateTimeUniversal**。 現在の日付と時刻を、世界協定時刻 (UTC) で24時間形式で表現したもの。 形式は、 `yyyyMMddTHHmmssffffZ` (大文字と小文字を区別する、4桁の年、2桁の月、2桁の日、時刻の区切り記号、2桁の時間、2桁の分、2桁の `T` 秒、4桁のミリ秒、および `Z` UTC インジケーターとしての文字) です。 例: 20190627T1540500718Z。

```yaml
Type: System.String
Parameter Sets: net
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Hour

表示する時を指定します。 0 ~ 23 の値を入力してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ミリ秒

日付のミリ秒を指定します。 0 ~ 999 の値を入力してください。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -分

表示する分を指定します。 0 ~ 59 の値を入力してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Month

表示する月を指定します。 1 ~ 12 の値を入力してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Second

表示する秒を指定します。 0 ~ 59 の値を入力してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UFormat

UNIX 形式で日付と時刻を表示します。 **Uformat** パラメーターを指定すると、文字列オブジェクトが出力されます。

**Uformat** 指定子の前には、パーセント記号 () が付きます。たとえば、、、など `%` `%m` `%d` `%Y` です。 [Notes](#notes)セクションには、有効な **uformat 指定子** のテーブルが含まれています。

**Uformat** パラメーターを使用する場合は、 `Get-Date` 日付を表示するために必要な **DateTime** オブジェクトのプロパティのみが取得されます。 その結果、**DateTime** オブジェクトの一部のプロパティとメソッドを利用できなくなる場合があります。

```yaml
Type: System.String
Parameter Sets: UFormat
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -年

表示する年を指定します。 1 ~ 9999 の値を入力してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
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

### パイプラインの入力

`Get-Date` パイプラインの入力を受け入れます。 たとえば、`Get-ChildItem | Get-Date` のようにします。

## 出力

### System.string または System.string

`Get-Date`**format** パラメーターと **uformat** パラメーターが使用されている場合を除き、 **DateTime** オブジェクトを返します。 **Format** パラメーターまたは **uformat** パラメーターは、**文字列** オブジェクトを返します。

**DateTime** オブジェクトが、文字列入力を必要とするなどのコマンドレットに送信されると、 `Add-Content` PowerShell はオブジェクトを **string** オブジェクトに変換します。

メソッドは、 `(Get-Date).ToString()` **DateTime** オブジェクトを **String** オブジェクトに変換します。

オブジェクトのプロパティとメソッドを表示するには、オブジェクトをパイプライン内でに送信し `Get-Member` ます。
たとえば、`Get-Date | Get-Member` のようにします。

## 注

**DateTime** オブジェクトは、システムロケールの長い日付形式と長い時刻形式です。

有効な **Uformat 指定子** は、次の表に示されています。

> [!IMPORTANT]
> 新しいバージョンの PowerShell では、追加の **Uformat** 指定子が追加されています。 たとえば、は `%F` powershell 6.2 で追加されたため、Windows powershell 5.1 以前では使用できません。 複数のバージョンの PowerShell で実行するように設計されたスクリプトで **Uformat** 指定子を使用する場合は、この点に注意してください。

| 書式指定子 |                                 意味                     |         例          |
| ---- | ----------------------------------------------------------------------- | ------------------------ |
| `%A` | 曜日-完全名                                             | 月曜日                   |
| `%a` | 曜日-省略名                                      | Mon                      |
| `%B` | 月の名前-完全                                                       | January                  |
| `%b` | 月の名前-略称                                                | 1 月                      |
| `%C` | 単位                                                                 | 2019の場合は20              |
| `%c` | 日付と時刻-省略形                                             | Thu 6 月 27 08:44:18 2019 |
| `%D` | Mm/dd/yy 形式の日付                                                 | 06/27/19                 |
| `%d` | 月の通算日-2 桁                                             | 05                       |
| `%e` | 月の通算日-1 桁のみの場合はスペースで始まります。           | \<space\>5/5               |
| `%G` | ' Y ' と同じ                                                             |                          |
| `%g` | ' Y ' と同じ                                                             |                          |
| `%H` | 24時間形式の時間                                                  | 17                       |
| `%h` | ' B ' と同じ                                                             |                          |
| `%I` | 12時間形式の時間                                                  | 05                       |
| `%j` | 通算日 ( `0` PowerShell 6 以降では、先行修正は含まれません) | 1-366                    |
| `%k` | ' H ' と同じ                                                             |                          |
| `%l` | ' I ' と同じ (大文字の I)                                              | 05                       |
| `%M` | 分                                                                 | 35                       |
| `%m` | 月の番号                                                            | 06                       |
| `%n` | 改行文字                                                       |                          |
| `%p` | AM または PM                                                                |                          |
| `%R` | 24時間形式の時刻-秒なし                                      | 17:45                    |
| `%r` | 12時間形式の時間                                                  | 09:15:36 AM              |
| `%S` | Seconds                                                                 | 05                       |
| `%s` | 00:00:00 1970 年1月1日以降の経過秒数                          | 1150451174.95705         |
| `%t` | 水平タブ文字                                                |                          |
| `%T` | 24 時間制の時刻                                                  | 17:45:52                 |
| `%U` | ' W ' と同じ                                                             |                          |
| `%u` | 曜日-数字                                                | 日曜日 = 0               |
| `%V` | 年の通算週                                                        | 01-53                    |
| `%w` | ' U ' と同じ                                                             |                          |
| `%W` | 年の通算週                                                        | 00-52                    |
| `%X` | ' T ' と同じ                                                             |                          |
| `%x` | ロケールの標準形式の日付                                      | 英語-米国の場合は06/27/19  |
| `%Y` | 4桁表記の年                                                  | 2019                     |
| `%y` | 2桁表記の年                                                  | 19                       |
| `%Z` | 世界協定時刻 (UTC) からのタイムゾーンオフセット                   | -07                      |

## 関連リンク

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Get-Culture](Get-Culture.md)

[Get-Member](Get-Member.md)

[New-Item](../Microsoft.PowerShell.Management/New-Item.md)

[New-TimeSpan](New-TimeSpan.md)

[Set-Date](Set-Date.md)
