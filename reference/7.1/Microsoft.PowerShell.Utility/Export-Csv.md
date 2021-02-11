---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 12/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-Csv
ms.openlocfilehash: f920130ec8354b61b0bb3617e061520271df0eed
ms.sourcegitcommit: 560a9f3c3148acab4655e91e8b07745ab74d5d26
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "96913238"
---
# Export-Csv

## 概要
オブジェクトを一連のコンマ区切り値 (CSV) 文字列に変換し、その文字列をファイルに保存します。

## SYNTAX

### 区切り記号 (既定値)

```
Export-Csv -InputObject <PSObject> [[-Path] <String>] [-LiteralPath <String>] [-Force] [-NoClobber]
 [-Encoding <Encoding>] [-Append] [[-Delimiter] <Char>] [-IncludeTypeInformation]
 [-NoTypeInformation] [-QuoteFields <String[]>] [-UseQuotes <QuoteKind>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### UseCulture

```
Export-Csv -InputObject <PSObject> [[-Path] <String>] [-LiteralPath <String>] [-Force] [-NoClobber]
 [-Encoding <Encoding>] [-Append] [-UseCulture] [-IncludeTypeInformation] [-NoTypeInformation]
 [-QuoteFields <String[]>] [-UseQuotes <QuoteKind>] [-WhatIf] [-Confirm]  [<CommonParameters>]
```

## Description

コマンドレットでは、 `Export-CSV` 送信するオブジェクトの CSV ファイルを作成します。 各オブジェクトは、オブジェクトのプロパティ値のコンマ区切りのリストを含む行です。 コマンドレットを使用して `Export-CSV` スプレッドシートを作成し、CSV ファイルを入力として受け取るプログラムとデータを共有することができます。

オブジェクトをコマンドレットに送信する前に書式設定しないでください `Export-CSV` 。 が `Export-CSV` 書式設定されたオブジェクトを受け取る場合、CSV ファイルにはオブジェクトのプロパティではなく、書式設定のプロパティが含まれます。 オブジェクトの選択したプロパティのみをエクスポートするには、コマンドレットを使用し `Select-Object` ます。

## 例

### 例 1: CSV ファイルにプロセスのプロパティをエクスポートする

この例では、特定のプロパティを持つ **プロセス** オブジェクトを選択し、そのオブジェクトを CSV ファイルにエクスポートします。

```powershell
Get-Process -Name WmiPrvSE | Select-Object -Property BasePriority,Id,SessionId,WorkingSet |
  Export-Csv -Path .\WmiData.csv -NoTypeInformation
Import-Csv -Path .\WmiData.csv
```

```Output
BasePriority Id    SessionId WorkingSet
------------ --    --------- ----------
8            976   0         20267008
8            2292  0         36786176
8            3816  0         30351360
8            8604  0         15011840
8            10008 0         8830976
8            11764 0         14237696
8            54632 0         9502720
```

`Get-Process`コマンドレットでは、**プロセス** オブジェクトを取得します。 **Name** パラメーターは、WmiPrvSE process オブジェクトのみを含むように出力をフィルター処理します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Select-Object` ます。 `Select-Object`**Property** パラメーターを使用して、プロセスオブジェクトのプロパティのサブセットを選択します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。 **Path** パラメーターは、WmiData.csv ファイルが現在のディレクトリに保存されることを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 この `Import-Csv` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 2: コンマ区切りファイルにプロセスをエクスポートする

この例では、 **プロセス** オブジェクトを取得し、オブジェクトを CSV ファイルにエクスポートします。

```powershell
Get-Process | Export-Csv -Path .\Processes.csv -NoTypeInformation
Get-Content -Path .\Processes.csv
```

```Output
"Name","SI","Handles","VM","WS","PM","NPM","Path","Parent","Company","CPU","FileVersion", ...
"ApplicationFrameHost","4","511","2203597099008","35364864","21979136","30048", ...
```

コマンドレットでは、 `Get-Process` **プロセス** オブジェクトを取得します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。
**Path** パラメーターは、Processes.csv ファイルが現在のディレクトリに保存されることを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 3: セミコロンで区切られたファイルにプロセスをエクスポートする

この例では、 **プロセス** オブジェクトを取得し、セミコロン区切り記号を使用してオブジェクトをファイルにエクスポートします。

```powershell
Get-Process | Export-Csv -Path .\Processes.csv -Delimiter ';' -NoTypeInformation
Get-Content -Path .\Processes.csv
```

```Output
"Name";"SI";"Handles";"VM";"WS";"PM";"NPM";"Path";"Parent";"Company";"CPU";"FileVersion"; ...
"ApplicationFrameHost";"4";"509";"2203595321344";"34807808";"21770240";"29504"; ...
```

コマンドレットでは、 `Get-Process` **プロセス** オブジェクトを取得します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。
**Path** パラメーターは、Processes.csv ファイルが現在のディレクトリに保存されることを指定します。 **Delimiter** パラメーターでは、文字列値を区切るためのセミコロンを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 4: 現在のカルチャのリスト区切り記号を使用してプロセスをエクスポートする

この例では、 **プロセス** オブジェクトを取得し、オブジェクトをファイルにエクスポートします。 区切り記号は、現在のカルチャのリスト区切り記号です。

```powershell
(Get-Culture).TextInfo.ListSeparator
Get-Process | Export-Csv -Path .\Processes.csv -UseCulture -NoTypeInformation
Get-Content -Path .\Processes.csv
```

```Output
"Name","SI","Handles","VM","WS","PM","NPM","Path","Parent","Company","CPU","FileVersion", ...
"ApplicationFrameHost","4","511","2203597099008","35364864","21979136","30048", ...
```

この `Get-Culture` コマンドレットは、入れ子になったプロパティの **TextInfo** と **listseparator** を使用し、現在のカルチャの既定のリスト区切り記号を表示します。 コマンドレットでは、 `Get-Process` **プロセス** オブジェクトを取得します。
プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。 **Path** パラメーターは、Processes.csv ファイルが現在のディレクトリに保存されることを指定します。 **UseCulture** パラメーターは、現在のカルチャの既定のリスト区切り記号を区切り記号として使用します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 5: 型情報を含むプロセスをエクスポートする

この例では、 **#TYPE** ヘッダー情報を CSV ファイルに含める方法について説明します。 PowerShell 6.0 より前のバージョンでは、 **#TYPE** ヘッダーが既定値です。

```powershell
Get-Process | Export-Csv -Path .\Processes.csv -IncludeTypeInformation
Get-Content -Path .\Processes.csv
```

```Output
#TYPE System.Diagnostics.Process
"Name","SI","Handles","VM","WS","PM","NPM","Path","Company","CPU","FileVersion", ...
"ApplicationFrameHost","4","507","2203595001856","35139584","20934656","29504", ...
```

コマンドレットでは、 `Get-Process` **プロセス** オブジェクトを取得します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。
**Path** パラメーターは、Processes.csv ファイルが現在のディレクトリに保存されることを指定します。 **Includetypeinformation** には、CSV 出力に **#TYPE** 情報ヘッダーが含まれています。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 6: CSV ファイルにオブジェクトをエクスポートして追加する

この例では、オブジェクトを CSV ファイルにエクスポートし、 **Append** パラメーターを使用してオブジェクトを既存のファイルに追加する方法について説明します。

```powershell
$AppService = (Get-Service -DisplayName *Application* | Select-Object -Property DisplayName, Status)
$AppService | Export-Csv -Path .\Services.Csv -NoTypeInformation
Get-Content -Path .\Services.Csv
$WinService = (Get-Service -DisplayName *Windows* | Select-Object -Property DisplayName, Status)
$WinService | Export-Csv -Path ./Services.csv -NoTypeInformation -Append
Get-Content -Path .\Services.Csv
```

```Output
"DisplayName","Status"
"Application Layer Gateway Service","Stopped"
"Application Identity","Running"
"Windows Audio Endpoint Builder","Running"
"Windows Audio","Running"
"Windows Event Log","Running"
```

コマンドレットでは、 `Get-Service` サービスオブジェクトを取得します。 **DisplayName** パラメーターは、word アプリケーションを含むサービスを返します。 サービスオブジェクトは、パイプラインを介してコマンドレットに送信され `Select-Object` ます。 `Select-Object`**Property** パラメーターを使用して、 **DisplayName** プロパティと **Status** プロパティを指定します。 `$AppService`変数は、オブジェクトを格納します。

オブジェクトは、パイプラインを介して `$AppService` コマンドレットに送信され `Export-Csv` ます。 `Export-Csv` サービスオブジェクトを一連の CSV 文字列に変換します。 **Path** パラメーターは、Services.csv ファイルが現在のディレクトリに保存されることを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

`Get-Service`および `Select-Object` コマンドレットは、word ウィンドウを含むサービスに対して繰り返されます。 変数には、 `$WinService` サービスオブジェクトが格納されます。 コマンドレットでは、 `Export-Csv` **Append** パラメーターを使用して、 `$WinService` オブジェクトが既存の Services.csv ファイルに追加されることを指定します。 `Get-Content`コマンドレットは、追加されたデータを含む更新されたファイルを表示するために繰り返されます。

### 例 7: パイプライン内の Format コマンドレットで予期しない結果が発生する

この例は、パイプライン内で format コマンドレットを使用しないことが重要な理由を示しています。 予期しない出力を受信した場合は、パイプライン構文のトラブルシューティングを行います。

```powershell
Get-Date | Select-Object -Property DateTime, Day, DayOfWeek, DayOfYear |
 Export-Csv -Path .\DateTime.csv -NoTypeInformation
Get-Content -Path .\DateTime.csv
```

```Output
"DateTime","Day","DayOfWeek","DayOfYear"
"Wednesday, January 2, 2019 14:59:34","2","Wednesday","2"
```

```powershell
Get-Date | Format-Table -Property DateTime, Day, DayOfWeek, DayOfYear |
 Export-Csv -Path .\FTDateTime.csv -NoTypeInformation
Get-Content -Path .\FTDateTime.csv
```

```Output
"ClassId2e4f51ef21dd47e99d3c952918aff9cd","pageHeaderEntry","pageFooterEntry","autosizeInfo", ...
"033ecb2bc07a4d43b5ef94ed5a35d280",,,,"Microsoft.PowerShell.Commands.Internal.Format. ...
"9e210fe47d09416682b841769c78b8a3",,,,,
"27c87ef9bbda4f709f6b4002fa4af63c",,,,,
"4ec4f0187cb04f4cb6973460dfe252df",,,,,
"cf522b78d86c486691226b40aa69e95c",,,,,
```

`Get-Date`コマンドレットでは、 **DateTime** オブジェクトを取得します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Select-Object` ます。 `Select-Object`**Property** パラメーターを使用して、オブジェクトのプロパティのサブセットを選択します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` オブジェクトを CSV 形式に変換します。 **Path** パラメーターは、DateTime.csv ファイルが現在のディレクトリに保存されることを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 `Get-Content`コマンドレットでは、 **Path** パラメーターを使用して、現在のディレクトリにある CSV ファイルを表示します。

`Format-Table`パイプライン内でコマンドレットを使用してプロパティを選択すると、予期しない結果が返されます。 `Format-Table` テーブル形式オブジェクトを、 `Export-Csv` **DateTime** オブジェクトではなく、コマンドレットに送信します。 `Export-Csv` テーブル形式オブジェクトを一連の CSV 文字列に変換します。 コマンドレットにより、 `Get-Content` テーブル形式オブジェクトを含む CSV ファイルが表示されます。

### 例 8: Force パラメーターを使用して読み取り専用ファイルを上書きする

この例では、空の読み取り専用ファイルを作成し、 **Force** パラメーターを使用してファイルを更新します。

```powershell
New-Item -Path .\ReadOnly.csv -ItemType File
Set-ItemProperty -Path .\ReadOnly.csv -Name IsReadOnly -Value $true
Get-Process | Export-Csv -Path .\ReadOnly.csv -NoTypeInformation
```

```Output
Export-Csv : Access to the path 'C:\ReadOnly.csv' is denied.
At line:1 char:15
+ Get-Process | Export-Csv -Path .\ReadOnly.csv -NoTypeInformation
+               ~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : OpenError: (:) [Export-Csv], UnauthorizedAccessException
+ FullyQualifiedErrorId : FileOpenFailure,Microsoft.PowerShell.Commands.ExportCsvCommand
```

```powershell
Get-Process | Export-Csv -Path .\ReadOnly.csv -NoTypeInformation -Force
Get-Content -Path .\ReadOnly.csv
```

```Output
"Name";"SI";"Handles";"VM";"WS";"PM";"NPM";"Path";"Parent";"Company";"CPU";"FileVersion"; ...
"ApplicationFrameHost";"4";"509";"2203595321344";"34807808";"21770240";"29504"; ...
```

この `New-Item` コマンドレットは、 **Path** パラメーターと **ItemType** パラメーターを使用して、現在のディレクトリに ReadOnly.csv ファイルを作成します。 コマンドレットでは、 `Set-ItemProperty` **Name** パラメーターと **Value** パラメーターを使用して、ファイルの **IsReadOnly** プロパティを true に変更します。 コマンドレットでは、 `Get-Process` **プロセス** オブジェクトを取得します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Export-Csv` ます。 `Export-Csv` プロセスオブジェクトを一連の CSV 文字列に変換します。 **Path** パラメーターは、ReadOnly.csv ファイルが現在のディレクトリに保存されることを指定します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。 出力には、アクセスが拒否されたためにファイルが書き込まれていないことが示されています。

**Force** パラメーターをコマンドレットに追加して、 `Export-Csv` エクスポートにファイルへの書き込みを強制します。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 9: Force パラメーターを Append と共に使用する

この例では、 **Force** パラメーターと **Append** パラメーターを使用する方法を示します。 これらのパラメーターを組み合わせると、一致しないオブジェクトプロパティを CSV ファイルに書き込むことができます。

```powershell
$Content = [PSCustomObject]@{Name = 'PowerShell Core'; Version = '6.0'}
$Content | Export-Csv -Path .\ParmFile.csv -NoTypeInformation
$AdditionalContent = [PSCustomObject]@{Name = 'Windows PowerShell'; Edition = 'Desktop'}
$AdditionalContent | Export-Csv -Path .\ParmFile.csv -NoTypeInformation -Append
```

```Output
Export-Csv : Cannot append CSV content to the following file: ParmFile.csv.
The appended object does not have a property that corresponds to the following column:
Version. To continue with mismatched properties, add the -Force parameter, and then retry
 the command.
At line:1 char:22
+ $AdditionalContent | Export-Csv -Path .\ParmFile.csv -NoTypeInformation -Append
+                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : InvalidData: (Version:String) [Export-Csv], InvalidOperationException
+ FullyQualifiedErrorId : CannotAppendCsvWithMismatchedPropertyNames,Microsoft.PowerShell. ...
```

```powershell
$AdditionalContent | Export-Csv -Path .\ParmFile.csv -NoTypeInformation -Append -Force
Import-Csv -Path .\ParmFile.csv
```

```Output
Name               Version
----               -------
PowerShell Core    6.0
Windows PowerShell
```

式は、 **Name** プロパティと **Version** プロパティを持つ **pscustomobject** 作成します。 値は変数に格納され `$Content` ます。 変数は、パイプラインを介して `$Content` コマンドレットに送信され `Export-Csv` ます。 `Export-Csv`**Path** パラメーターを使用して、ParmFile.csv ファイルを現在のディレクトリに保存します。 **Notypeinformation** パラメーターを指定すると、CSV 出力から **#TYPE** 情報ヘッダーが削除され、PowerShell 6 では不要になります。

もう1つの式は、 **Name** プロパティと **Edition** プロパティを使用して **pscustomobject** 作成します。 値は変数に格納され `$AdditionalContent` ます。 変数は、パイプラインを介して `$AdditionalContent` コマンドレットに送信され `Export-Csv` ます。 ファイルにデータを追加するには、 **Append** パラメーターを使用します。 **バージョン** と **エディション** のプロパティ名が一致しないため、追加に失敗します。

`Export-Csv`コマンドレット **force** パラメーターを使用して、エクスポートにファイルへの書き込みを強制します。 **Edition** プロパティは破棄されます。 この `Import-Csv` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリにあるファイルを表示します。

### 例 10: 2 つの列を囲む引用符を使用して CSV にエクスポートする

この例では、 **DateTime** オブジェクトを CSV 文字列に変換します。

```powershell
Get-Date | Export-Csv  -QuoteFields "DateTime","Date" -Path .\FTDateTime.csv
Get-Content -Path .\FTDateTime.csv
```

```Output
DisplayHint,"DateTime","Date",Day,DayOfWeek,DayOfYear,Hour,Kind,Millisecond,Minute,Month,Second,Ticks,TimeOfDay,Year
DateTime,"Thursday, August 22, 2019 11:27:34 AM","8/22/2019 12:00:00 AM",22,Thursday,234,11,Local,569,27,8,34,637020700545699784,11:27:34.5699784,2019
```

### 例 11: 必要な場合にのみ引用符を使用して CSV にエクスポートする

この例では、 **DateTime** オブジェクトを CSV 文字列に変換します。

```powershell
Get-Date | Export-Csv  -UseQuotes AsNeeded -Path .\FTDateTime.csv
Get-Content -Path .\FTDateTime.csv
```

```Output
DisplayHint,DateTime,Date,Day,DayOfWeek,DayOfYear,Hour,Kind,Millisecond,Minute,Month,Second,Ticks,TimeOfDay,Year
DateTime,"Thursday, August 22, 2019 11:31:00 AM",8/22/2019 12:00:00 AM,22,Thursday,234,11,Local,713,31,8,0,637020702607132640,11:31:00.7132640,2019
```

## PARAMETERS

### -追加

このパラメーターを使用して、 `Export-CSV` 指定したファイルの末尾に CSV 出力を追加します。 このパラメーターを指定しないと、に `Export-CSV` よってファイルの内容が警告なしで置き換えられます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

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

### -区切り記号

プロパティの値を区切る区切り記号を指定します。 既定値はコンマ ( `,` ) です。 コロン () などの文字を入力し `:` ます。 セミコロン () を指定するには `;` 、引用符で囲みます。

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

### -Encoding

エクスポートされた CSV ファイルのエンコード方式を指定します。 既定値は `utf8NoBOM` です。

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

### -Force

このパラメーター `Export-Csv` を使用すると、は **読み取り専用** 属性でファイルを上書きできます。

**Force** パラメーターと **Append** パラメーターを組み合わせると、一致しないプロパティを含むオブジェクトを CSV ファイルに書き込むことができます。 一致するプロパティのみがファイルに書き込まれます。 一致しないプロパティは破棄されます。

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

### -IncludeTypeInformation

このパラメーターを使用する場合、CSV 出力の最初の行には **#TYPE** が含まれ、その後にオブジェクト型の完全修飾名が続きます。 たとえば、 **#TYPE** のようにします。

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

CSV 文字列としてエクスポートするオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。 パイプを使用してオブジェクトをにパイプ処理することもでき `Export-CSV` ます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -LiteralPath

CSV 出力ファイルのパスを指定します。 **Path** と異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符を使用します。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: PSPath, LP

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoClobber

が既存のファイルを上書きしないようにするには、このパラメーターを使用し `Export-CSV` ます。 既定では、指定されたパスにファイルが存在する場合、は `Export-CSV` 警告なしにファイルを上書きします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NoOverwrite

Required: False
Position: Named
Default value: None
Accept pipeline input: False
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

### -Path

CSV 出力ファイルを保存する場所を指定する必須のパラメーターです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
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

コマンドレットが処理または変更されないようにします。 出力には、コマンドレットが実行された場合に何が起こるかが示されます。

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

### -QuoteFields

引用符で囲む必要がある列の名前を指定します。 このパラメーターを使用する場合は、指定された列だけが引用符で囲まれます。 このパラメーターは、PowerShell 7.0 で追加されました。

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

パイプを使用して、拡張型システム (実行) アダプターを持つ任意のオブジェクトをにパイプでき `Export-CSV` ます。

## 出力

### System.String

CSV リストが Path パラメーターに指定されたファイルに送られます。

## 注

コマンドレットは、送信した `Export-CSV` オブジェクトを一連の CSV 文字列に変換し、指定したテキストファイルに保存します。 を使用し `Export-CSV -IncludeTypeInformation` て csv ファイルにオブジェクトを保存した後、コマンドレットを使用して、 `Import-Csv` csv ファイル内のテキストからオブジェクトを作成できます。

CSV ファイルでは、各オブジェクトは、オブジェクトのプロパティ値のコンマ区切りリストで表されます。 プロパティ値は、 **ToString ()** メソッドを使用して文字列に変換されます。 文字列は、プロパティ値の名前で表されます。 `Export-CSV -IncludeTypeInformation` では、オブジェクトのメソッドはエクスポートされません。

CSV 文字列は次のように出力されます。

- **Includetypeinformation** を使用する場合、最初の文字列には **#TYPE** 情報ヘッダーの後にオブジェクトの種類の完全修飾名が含まれます。
  たとえば、 **#TYPE** のようにします。
- **Includetypeinformation** が使用されていない場合、最初の文字列には列ヘッダーが含まれます。 ヘッダーには、コンマ区切りのリストとして、最初のオブジェクトのプロパティ名が含まれます。
- 残りの文字列には、各オブジェクトのプロパティ値のコンマ区切りのリストが含まれます。

PowerShell 6.0 以降では、の既定の動作で `Export-CSV` は、 **#TYPE** 情報が CSV に含められず、 **notypeinformation** が暗黙的に使用されます。 **Includetypeinformation** を使用して **#TYPE** 情報を含め、PowerShell 6.0 より前のの既定の動作をエミュレートすることができ `Export-CSV` ます。

複数のオブジェクトをに送信すると `Export-CSV` 、は、 `Export-CSV` 最初に送信したオブジェクトのプロパティに基づいてファイルを整理します。 指定したプロパティのいずれかが残りのオブジェクトに含まれない場合、そのオブジェクトのプロパティ値は連続する 2 つのコンマで表される null になります。 残りのオブジェクトに追加のプロパティがある場合は、これらのプロパティ値は、ファイルには含まれません。

コマンドレットを使用して、 `Import-Csv` ファイル内の CSV 文字列からオブジェクトを再作成することができます。 結果のオブジェクトは、メソッドのない、プロパティ値の文字列形式で構成された元のオブジェクトの CSV バージョンです。

`ConvertTo-Csv` `ConvertFrom-Csv` コマンドレットおよびコマンドレットは、オブジェクトを csv 文字列と csv 文字列に変換します。 `Export-CSV` はと同じですが `ConvertTo-CSV` 、CSV 文字列をファイルに保存する点が異なります。

## 関連リンク

[ConvertFrom-Csv](ConvertFrom-Csv.md)

[ConvertTo-Csv](ConvertTo-Csv.md)

[Format-Table](Format-Table.md)

[Import-Csv](Import-Csv.md)

[Select-Object](Select-Object.md)
