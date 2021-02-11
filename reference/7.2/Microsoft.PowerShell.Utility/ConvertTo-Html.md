---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertto-html?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertTo-Html
ms.openlocfilehash: ffe7fe7c44258435c7ec9ddd2bab9ebeb9f6c465
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600107"
---
# ConvertTo-Html

## 概要
.NET オブジェクトを、Web ブラウザーで表示できる HTML に変換します。

## SYNTAX

### ページ (既定)

```
ConvertTo-Html [-InputObject <PSObject>] [[-Property] <Object[]>] [[-Body] <String[]>]
 [[-Head] <String[]>] [[-Title] <String>] [-As <String>] [-CssUri <Uri>] [-PostContent <String[]>]
 [-PreContent <String[]>] [-Meta <Hashtable>] [-Charset <String>] [-Transitional]
 [<CommonParameters>]
```

### Fragment

```
ConvertTo-Html [-InputObject <PSObject>] [[-Property] <Object[]>] [-As <String>] [-Fragment]
 [-PostContent <String[]>] [-PreContent <String[]>] [<CommonParameters>]
```

## Description

`ConvertTo-Html`コマンドレットは、Web ブラウザーに表示できる HTML に .net オブジェクトを変換します。 このコマンドレットを使用すると、Web ページにコマンドの出力を表示できます。

のパラメーターを使用すると、 `ConvertTo-Html` オブジェクトのプロパティを選択したり、テーブルまたは一覧の形式を指定したり、HTML ページのタイトルを指定したり、オブジェクトの前後にテキストを追加したり、厳密な DTD ページではなくテーブルまたはリストフラグメントだけを返すことができます。

複数のオブジェクトをに送信すると `ConvertTo-Html` 、PowerShell によって、送信する最初のオブジェクトのプロパティに基づいてテーブル (または一覧) が作成されます。 残りのオブジェクトに指定したプロパティのいずれかがない場合は、そのオブジェクトのプロパティ値が空のセルになります。 残りのオブジェクトに追加のプロパティがある場合は、これらのプロパティ値は、ファイルには含まれません。

## 例

### 例 1: 日付を表示する web ページを作成する

```powershell
ConvertTo-Html -InputObject (Get-Date)
```

このコマンドは、現在の日付のプロパティを表示する HTML ページを作成します。 **InputObject** パラメーターを使用してコマンドの結果を `Get-Date` コマンドレットに送信し `ConvertTo-Html` ます。

### 例 2: PowerShell エイリアスを表示する web ページを作成する

```powershell
Get-Alias | ConvertTo-Html | Out-File aliases.htm
Invoke-Item aliases.htm
```

このコマンドは、現在のコンソールで PowerShell エイリアスを一覧表示する HTML ページを作成します。

コマンドは、コマンド `Get-Alias` レットを使用してエイリアスを取得します。 パイプライン演算子 () を使用して、 `|` エイリアスをコマンドレットに送信します。これにより、 `ConvertTo-Html` HTML ページが作成されます。 また、コマンドレットを使用して、 `Out-File` HTML コードをファイルに送信し `aliases.htm` ます。

### 例 3: PowerShell イベントを表示する web ページを作成する

```powershell
`Get-EventLog` -LogName "Windows PowerShell" | ConvertTo-Html | Out-File pslog.htm
```

このコマンド `pslog.htm` は、ローカルコンピューターの Windows PowerShell イベントログにイベントを表示するという名前の HTML ページを作成します。

コマンドレットを使用して `Get-EventLog` Windows PowerShell ログ内のイベントを取得し、パイプライン演算子 () を使用して `|` イベントをコマンドレットに送信し `ConvertTo-Html` ます。 また、コマンドレットを使用して、 `Out-File` HTML コードをファイルに送信し `pslog.htm` ます。

また、コマンドレットを使用して、 `Out-File` HTML コードをファイルに送信し `pslog.htm` ます。

### 例 4: プロセスを表示する web ページを作成する

```powershell
Get-Process |
  ConvertTo-Html -Property Name, Path, Company -Title "Process Information" |
    Out-File proc.htm
Invoke-Item proc.htm
```

これらのコマンドは、ローカル コンピューター上のプロセスの名前、パス、および会社を一覧表示する HTML ページを作成し、開きます。

最初のコマンドは、 `Get-Process` コマンドレットを使用して、コンピューター上で実行されているプロセスを表すオブジェクトを取得します。 このコマンドは、パイプライン演算子 () を使用して、 `|` プロセスオブジェクトをコマンドレットに送信し `ConvertTo-Html` ます。

このコマンドは、 **Property** パラメーターを使用して、テーブルに含めるプロセスオブジェクトの3つのプロパティを選択します。 このコマンドは、 **title** パラメーターを使用して、HTML ページのタイトルを指定します。 また、コマンドレットを使用して、 `Out-File` 生成された HTML を Proc.htm という名前のファイルに送信します。

2番目のコマンドは、 `Invoke-Item` コマンドレットを使用して、 `Proc.htm` 既定のブラウザーでを開きます。

### 例 5: サービスオブジェクトを表示する web ページを作成する

```powershell
Get-Service | ConvertTo-Html -CssUri "test.css"
```

```Output
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"  "https://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html>
<head>
<title>HTML TABLE</title>
<link rel="stylesheet" type="text/css" href="test.css" />
...
```

このコマンドは、コマンドレットが返すサービスオブジェクトの HTML ページを作成し `Get-Service` ます。 このコマンドは、 **CssUri** パラメーターを使用して、HTML ページのカスケードスタイルシートを指定します。

**CssUri** パラメーターを指定すると `<link rel="stylesheet" type="text/css" href="test.css">` 、結果の HTML にタグが追加されます。 このタグの HREF 属性には、スタイル シートの名前が含まれています。

### 例 6: サービスオブジェクトを表示する web ページを作成する

```powershell
Get-Service | ConvertTo-Html -As LIST | Out-File services.htm
```

このコマンドは、コマンドレットが返すサービスオブジェクトの HTML ページを作成し `Get-Service` ます。 このコマンドは、 **As** パラメーターを使用してリスト形式を指定します。 コマンドレットは、 `Out-File` 結果の HTML をファイルに送信し `Services.htm` ます。

### 例 7: 現在の日付の web テーブルを作成する

```powershell
Get-Date | ConvertTo-Html -Fragment
```

```Output
<table>
<colgroup>...</colgroup>
<tr><th>DisplayHint</th><th>DateTime</th><th>Date</th><th>Day</th><th>DayOfWeek</th><th>DayOfYear</th><th>Hour</th>
<th>Kind</th><th>Millisecond</th><th>Minute</th><th>Month</th><th>Second</th><th>Ticks</th><th>TimeOfDay</th><th>Year</th></tr>
<tr><td>DateTime</td><td>Monday, May 05, 2008 10:40:04 AM</td><td>5/5/2008 12:00:00 AM</td><td>5</td><td>Monday</td>
<td>126</td><td>10</td><td>Local</td><td>123</td><td>40</td><td>5</td><td>4</td><td>633455808041237213</td><td>10:40:04.12
37213</td><td>2008</td></tr>
</table>
```

このコマンドは `ConvertTo-Html` 、を使用して、現在の日付の HTML テーブルを生成します。 コマンドは、コマンド `Get-Date` レットを使用して現在の日付を取得します。 パイプライン演算子 (|) を使用して、結果をコマンドレットに送信し `ConvertTo-Html` ます。

コマンドには、 `ConvertTo-Html` 出力を HTML テーブルに制限する **Fragment** パラメーターが含まれています。 その結果、`<HEAD>` や `<BODY>` タグなど、HTML ページの他の要素が省略されます。

### 例 8: PowerShell イベントを表示するための web ページを作成する

```powershell
Get-EventLog -Log "Windows PowerShell" | ConvertTo-Html -Property id, level, task
```

このコマンドは、 `Get-EventLog` コマンドレットを使用して、Windows PowerShell イベントログからイベントを取得します。

パイプライン演算子 () を使用して、イベントを `|` コマンドレットに送信し `ConvertTo-Html` ます。これにより、イベントが HTML 形式に変換されます。

この `ConvertTo-Html` コマンドは、 **Property** パラメーターを使用して、イベントの **ID**、 **Level**、および **Task** プロパティのみを選択します。

### 例 9: 指定したサービスを表示する web ページを作成する

```powershell
$htmlParams = @{
  Title = "Windows Services: Server01"
  Body = Get-Date
  PreContent = "<P>Generated by Corporate IT</P>"
  PostContent = "For details, contact Corporate IT."
}
Get-Service A* |
  ConvertTo-Html @htmlParams |
    Out-File Services.htm
Invoke-Item Services.htm
```

このコマンドは、で始まるコンピューター上のサービスを表示する Web ページを作成して開きます。の **Title**、 **Body**、 **precontent**、および **postcontent** パラメーターを使用して `ConvertTo-Html` 、出力をカスタマイズします。

コマンドの最初の部分では、コマンドレットを使用して `Get-Service` 、で始まるコンピューター上のサービスを取得します。このコマンドは、パイプライン演算子 () を使用して `|` 結果を `ConvertTo-Html` コマンドレットに送信します。 また、コマンドレットを使用して、 `Out-File` 出力を Services.htm ファイルに送信します。

セミコロン () は、 `;` 最初のコマンドを終了し、2番目のコマンドを開始します。このコマンドは、コマンドレットを使用して、 `Invoke-Item` 既定の `Services.htm` ブラウザーでファイルを開きます。

### 例 10: HTML のメタプロパティと文字セットを設定する

```powershell
Get-Service | ConvertTo-HTML -Meta @{
  refresh=10
  author="Author's Name"
  keywords="PowerShell, HTML, ConvertTo-HTML"
} -Charset "UTF-8"
```

このコマンドは、更新、作成者、およびキーワードのメタタグを含む web ページの HTML を作成します。
ページの文字セットが UTF-8 に設定されています

### 例 11: HTML を XHTML 遷移 DTD に設定する

```powershell
Get-Service | ConvertTo-HTML -Transitional
```

このコマンドは、返された HTML の DOCTYPE を XHTML 遷移 DTD に設定します。

## PARAMETERS

### -As

オブジェクトがテーブルまたは一覧のどちらで書式設定されるかを決定します。 有効な値は **テーブル** と **リスト** です。 既定値は **Table** です。

**テーブル** 値は、PowerShell テーブル形式に似た HTML テーブルを生成します。 ヘッダー行には、プロパティ名が表示されます。 各テーブル行は、オブジェクトを表し、各プロパティのオブジェクトの値を表示します。

**List** 値は、PowerShell リスト形式に似たオブジェクトごとに2列の HTML テーブルを生成します。 最初の列には、プロパティ名が表示されます。 2番目の列には、プロパティ値が表示されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Table, List

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -本文

開始の `<BODY>` タグの後に追加するテキストを指定します。 既定では、その位置にテキストがありません。

```yaml
Type: System.String[]
Parameter Sets: Page
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Charset

開始タグに追加するテキストを指定し `<charset>` ます。 既定では、その位置にテキストがありません。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: Page
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CssUri

HTML ファイルに適用されるカスケード スタイル シート (CSS) の Uniform Resource Identifier (URI) を指定します。 URI は、出力のスタイル シートのリンクに含まれます。

```yaml
Type: System.Uri
Parameter Sets: Page
Aliases: cu, uri

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Fragment

HTML テーブルのみを生成します。 HTML、HEAD、TITLE、および BODY タグは省略されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Fragment
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ヘッド

`<HEAD>` タグの内容を指定します。 既定値は、`<title\>HTML TABLE</title>` です。 **Head** パラメーターを使用すると、 **Title** パラメーターは無視されます。

```yaml
Type: System.String[]
Parameter Sets: Page
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

HTML で表されるオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

このパラメーターを使用して、コンピューター上のすべてのサービスなど、複数のオブジェクトを送信すると、によって、 `ConvertTo-Html` コレクションまたはオブジェクトの配列のプロパティを表示するテーブルが作成されます。 個々のオブジェクトのテーブルを作成するには、パイプライン演算子を使用してオブジェクトをにパイプ処理し `ConvertTo-Html` ます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Meta

開始タグに追加するテキストを指定し `<meta>` ます。 既定では、その位置にテキストがありません。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: Page
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostContent

終了の `</TABLE>` タグの後に追加するテキストを指定します。 既定では、その位置にテキストがありません。

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

### -事前コンテンツ

開始の `<TABLE>` タグの前に追加するテキストを指定します。 既定では、その位置にテキストがありません。

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

### -プロパティ

HTML 内のオブジェクトの指定したプロパティが含まれます。 **Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- Name (または label)- `<string>` (PowerShell 6.x で追加)
- 式 `<string>` または `<script block>`
- FormatString `<string>`
- 幅- `<int32>` -より大きい必要があります `0`
- Alignment-値には `Left` 、、 `Center` 、またはを指定できます。 `Right`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Title

HTML のファイルのタイトルを指定します。つまり、`<TITLE>` タグの間に表示されるテキストです。

```yaml
Type: System.String
Parameter Sets: Page
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -移行中

**Doctype** を **xhtml 遷移 dtd** に変更します。既定の **doctype** は **xhtml Strict dtd** です。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Page
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

パイプを使用して任意の .NET オブジェクトをにパイプすることができ `ConvertTo-Html` ます。

## 出力

### System.string または System.Xml.Xmlドキュメント

`ConvertTo-Html` 有効な HTML を構成する一連の文字列を返します。

## 注

このコマンドレットを使用するには、パイプを使用して1つまたは複数のオブジェクトをコマンドレットに渡します。または、 **InputObject** パラメーターを使用してオブジェクトを指定します。 入力が複数のオブジェクトで構成されている場合、これら 2 つのメソッドの出力は大きく異なります。

- パイプを使用して複数のオブジェクトをコマンドレットに渡した場合、PowerShell はオブジェクトをコマンドレットに一度に1つずつ送信します。 その結果、によって、 `ConvertTo-Html` 個々のオブジェクトを表示するテーブルが作成されます。 たとえば、コンピューター上のプロセスをにパイプする場合、 `ConvertTo-Html` 結果のテーブルにはすべてのプロセスが表示されます。

- **InputObject** パラメーターを使用して複数のオブジェクトを送信すると、は `ConvertTo-Html` これらのオブジェクトをコレクションまたは配列として受け取ります。 その結果、配列内の項目ではなく、配列とプロパティを表示するテーブルが作成されます。 たとえば、 **InputObject** を使用してコンピューター上のプロセスをに送信する場合、結果のテーブルには `ConvertTo-Html` オブジェクト配列とそのプロパティが表示されます。

  XHTML Strict DTD に準拠するために、DOCTYPE タグが適宜変更されます。

   `<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"   "https://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"\>`

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[ConvertTo-Csv](ConvertTo-Csv.md)

[ConvertTo-Json](ConvertTo-Json.md)

[ConvertTo-Xml](ConvertTo-Xml.md)

[Export-Clixml](Export-Clixml.md)

[Import-Clixml](Import-Clixml.md)
