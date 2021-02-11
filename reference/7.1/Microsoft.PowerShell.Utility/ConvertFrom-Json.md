---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/19/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-Json
ms.openlocfilehash: 062d29d033e0a84837a1593fb96df0c3a00dc392
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94389421"
---
# ConvertFrom-Json

## 概要
JSON 形式の文字列をカスタムオブジェクトまたはハッシュテーブルに変換します。

## SYNTAX

```
ConvertFrom-Json [-InputObject] <String> [-AsHashtable] [-Depth <Int32>] [-NoEnumerate] [<CommonParameters>]
```

## Description

この `ConvertFrom-Json` コマンドレットは、json (JavaScript Object Notation) 形式の文字列を、json 文字列内の各フィールドのプロパティを持つカスタム **Pscustomobject** オブジェクトに変換します。 JSON は、オブジェクトのテキスト表現を提供する Web サイトで広く使用されています。 JSON 標準では、 **Pscustomobject** 禁止されている使用は禁止されていません。 たとえば、JSON 文字列に重複するキーが含まれている場合、このコマンドレットでは最後のキーのみが使用されます。 次の他の例を参照してください。

任意のオブジェクトから JSON 文字列を生成するには、コマンドレットを使用し `ConvertTo-Json` ます。

このコマンドレットは、PowerShell 3.0 で導入されました。

> [!NOTE]
> PowerShell 6 以降では、このコマンドレットは、コメント付きの JSON をサポートしています。 受け入れられるコメントは、2つのスラッシュ () で開始され `//` ます。 コメントはデータ内では表示されません。また、PowerShell 5.1 の場合と同様に、データを破損させたりエラーをスローしたりすることなくファイルに書き込むことができます。

## 例

### 例 1: DateTime オブジェクトを JSON オブジェクトに変換する

このコマンドは、コマンドレットとコマンドレットを使用して、 `ConvertTo-Json` `ConvertFrom-Json` **DateTime** オブジェクトを `Get-Date` コマンドレットから JSON オブジェクトに変換し、 **pscustomobject** 実行します。

```powershell
Get-Date | Select-Object -Property * | ConvertTo-Json | ConvertFrom-Json
```

```Output
DisplayHint : 2
DateTime    : Friday, January 13, 2012 8:06:31 PM
Date        : 1/13/2012 8:00:00 AM
Day         : 13
DayOfWeek   : 5
DayOfYear   : 13
Hour        : 20
Kind        : 2
Millisecond : 400
Minute      : 6
Month       : 1
Second      : 31
Ticks       : 634620819914009002
TimeOfDay   : @{Ticks=723914009002; Days=0; Hours=20; Milliseconds=400; Minutes=6; Seconds=31; TotalDays=0.83786343634490734; TotalHours=20.108722472277776; TotalMilliseconds=72391400.900200009; TotalMinutes=1206.5233483366667;TotalSeconds=72391.4009002}
Year        : 2012
```

この例では、コマンドレットを使用して、 `Select-Object` **DateTime** オブジェクトのすべてのプロパティを取得します。 コマンドレットを使用し `ConvertTo-Json` て、 **DateTime** オブジェクトを json オブジェクトとして書式設定された文字列に変換し、コマンドレットを使用して `ConvertFrom-Json` json 形式の文字列を **pscustomobject** オブジェクトに変換します。

### 例 2: web サービスから JSON 文字列を取得し、それを PowerShell オブジェクトに変換する

このコマンドは、コマンドレットを使用して `Invoke-WebRequest` web サービスから json 文字列を取得し、コマンドレットを使用して `ConvertFrom-Json` json コンテンツを PowerShell で管理できるオブジェクトに変換します。

```powershell
# Ensures that Invoke-WebRequest uses TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$j = Invoke-WebRequest 'https://api.github.com/repos/PowerShell/PowerShell/issues' | ConvertFrom-Json
```

`Invoke-RestMethod`JSON コンテンツをオブジェクトに自動的に変換するコマンドレットを使用することもできます。

### 例 3: JSON 文字列をカスタムオブジェクトに変換する

この例では、コマンドレットを使用して `ConvertFrom-Json` JSON ファイルを PowerShell カスタムオブジェクトに変換する方法を示します。

```powershell
Get-Content JsonFile.JSON | ConvertFrom-Json
```

このコマンドは Get-Content コマンドレットを使用して、JSON ファイル内の文字列を取得します。 次に、パイプライン演算子を使用して、区切られた文字列をコマンドレットに送信し `ConvertFrom-Json` 、それをカスタムオブジェクトに変換します。

### 例 4: JSON 文字列をハッシュテーブルに変換する

このコマンドは、 `-AsHashtable` スイッチがコマンドの制限を克服できる例を示しています。

```powershell
'{ "key":"value1", "Key":"value2" }' | ConvertFrom-Json -AsHashtable
```

JSON 文字列には、大文字小文字のみが異なるキーを持つ2つのキーと値のペアが含まれています。 スイッチがない場合、コマンドはエラーをスローしました。

### 例 5: 1 つの要素配列をラウンドトリップする

このコマンドは、スイッチを使用して `-NoEnumerate` 1 つの要素 JSON 配列をラウンドトリップする例を示しています。

```powershell
Write-Output "With -NoEnumerate: $('[1]' | ConvertFrom-Json -NoEnumerate | ConvertTo-Json -Compress)"
Write-Output "Without -NoEnumerate: $('[1]' | ConvertFrom-Json | ConvertTo-Json -Compress)"
```

```Output
With -NoEnumerate: [1]
Without -NoEnumerate: 1
```

JSON 文字列には、1つの要素を持つ配列が含まれています。 スイッチがない場合は、JSON を PSObject に変換してから、コマンドを使用して変換すると、 `ConvertTo-Json` 1 つの整数になります。

## PARAMETERS

### -AsHashtable

JSON をハッシュテーブルオブジェクトに変換します。 このスイッチは、PowerShell 6.0 で導入されました。 いくつかのシナリオでは、コマンドレットのいくつかの制限を克服でき `ConvertFrom-Json` ます。

- 大文字と小文字の違いが異なるキーを持つリストが JSON に含まれている場合。 スイッチを指定しないと、これらのキーは同じキーと見なされるため、最後のキーのみが使用されます。
- JSON に空の文字列であるキーが含まれている場合。 スイッチを指定しないと、では、ハッシュテーブルでは許可されないため、コマンドレットでエラーがスロー `PSCustomObject` されます。 これが発生する可能性のあるユースケースの例としては、 `project.lock.json` ファイルがあります。
- ハッシュテーブルは、特定のデータ構造に対してより高速に処理できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -深さ

JSON 入力に許可されている最大の深さを取得または設定します。 既定では、1024です。

このパラメーターは、PowerShell 6.2 で導入されました。

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

### -InputObject

JSON オブジェクトに変換する JSON 文字列を指定します。 文字列が格納されている変数を入力するか、文字列を取得するコマンドまたは式を入力します。 パイプを使用して文字列をにパイプすることもでき `ConvertFrom-Json` ます。

**InputObject** パラメーターは必須ですが、その値に空の文字列を指定できます。 入力オブジェクトが空の文字列の場合、 `ConvertFrom-Json` は出力を生成しません。 **InputObject** 値をにすることはできません `$null` 。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -NoEnumerate

出力が列挙されないことを指定します。

このパラメーターを設定すると、すべての要素を個別に送信する代わりに、配列が単一のオブジェクトとして送信されます。 これにより、で JSON をラウンドトリップできることが保証さ `ConvertTo-Json` れます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、JSON 文字列をにパイプすることができ `ConvertFrom-Json` ます。

## 出力

### PSCustomObject

### System.Collections.Hashtable

## 注

このコマンドレットは [Newtonsoft Json.NET](https://www.newtonsoft.com/json)を使用して実装されます。

PowerShell 6 以降では、 `ConvertTo-Json` タイムスタンプとして書式設定された文字列を **DateTime** 値に変換しようとします。 変換後の値は、プロパティが次のよう `[datetime]` に設定されたインスタンスです `Kind` 。

- `Unspecified`入力文字列にタイムゾーン情報がない場合は。
- `Utc`タイムゾーン情報が末尾である場合は `Z` 。
- `Local`タイムゾーン情報が、のような末尾の UTC _オフセット_ として指定されている場合は `+02:00` 。 オフセットは、呼び出し元の構成されたタイムゾーンに適切に変換されます。 既定の出力書式設定では、元のタイムゾーンオフセットは示されません。

## 関連リンク

[JavaScript および .NET での JavaScript Object Notation (JSON) の概要](/previous-versions/dotnet/articles/bb299886(v=msdn.10))

[ConvertTo-Json](ConvertTo-Json.md)

[Invoke-WebRequest](Invoke-WebRequest.md)

[Invoke-RestMethod](Invoke-RestMethod.md)
