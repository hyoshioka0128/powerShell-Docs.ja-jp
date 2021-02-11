---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/10/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-Json
ms.openlocfilehash: e1bab9250269dadf0d899f9e172e8a4a8387271d
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388129"
---
# ConvertFrom-Json

## 概要
JSON 形式の文字列をカスタム オブジェクトに変換します。

## SYNTAX

```
ConvertFrom-Json [-InputObject] <String> [<CommonParameters>]
```

## Description

この `ConvertFrom-Json` コマンドレットは、json (JavaScript Object Notation) 形式の文字列を、json 文字列内の各フィールドのプロパティを持つカスタム **Pscustomobject** オブジェクトに変換します。 JSON は、オブジェクトのテキスト表現を提供する Web サイトで広く使用されています。 JSON 標準では、 **Pscustomobject** 禁止されている使用は禁止されていません。 たとえば、JSON 文字列に重複するキーが含まれている場合、このコマンドレットでは最後のキーのみが使用されます。 次の他の例を参照してください。

任意のオブジェクトから JSON 文字列を生成するには、コマンドレットを使用し `ConvertTo-Json` ます。

このコマンドレットは、PowerShell 3.0 で導入されました。

> [!NOTE]
> このコマンドレットは、コメント付きの JSON をサポートしていません。

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

## PARAMETERS

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、JSON 文字列をにパイプすることができ `ConvertFrom-Json` ます。

## 出力

### PSCustomObject

## 注

`ConvertFrom-Json`コマンドレットは、 [JavaScriptSerializer クラス](/dotnet/api/system.web.script.serialization.javascriptserializer)を使用して実装されます。

## 関連リンク

[JavaScript および .NET での JavaScript Object Notation (JSON) の概要](/previous-versions/dotnet/articles/bb299886(v=msdn.10))

[ConvertTo-Json](ConvertTo-Json.md)

[Invoke-WebRequest](Invoke-WebRequest.md)

[Invoke-RestMethod](Invoke-RestMethod.md)
