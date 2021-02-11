---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/out-printer?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-Printer
ms.openlocfilehash: bc16c7129dff2f2982d1756d5642d86481cd573d
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94344356"
---
# Out-Printer

## 概要
プリンターに出力を送信します。

## SYNTAX

```
Out-Printer [[-Name] <String>] [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

`Out-Printer`コマンドレットは、出力を既定のプリンターまたは代替プリンター (指定されている場合) に送信します。

> [!NOTE]
> このコマンドレットは、PowerShell 7 で再導入されました。 このコマンドレットは、Windows デスクトップをサポートする Windows システムでのみ使用できます。

## 例

### 例 1-既定のプリンターに印刷するファイルを送信する

この例では、に Path パラメーターがない場合でも、ファイルを印刷する方法を示し `Out-Printer` ます。 **Path**

```powershell
Get-Content -Path ./readme.txt | Out-Printer
```

`Get-Content`現在のディレクトリ内のファイルの内容を取得 `readme.txt` し、その内容をにパイプして `Out-Printer` 、既定のプリンターに送信します。

### 例 2: リモートプリンターに文字列を出力する

この例 `Hello, World` では、Server01 上の **Prt-6b カラー** プリンターに出力します。

```powershell
"Hello, World" | Out-Printer -Name "\\Server01\Prt-6B Color"
```

**Name** パラメーターは、既定値ではなく、特定のプリンターを選択します。

### 例 3-既定のプリンターにヘルプトピックを印刷する

この例では、のヘルプトピックの完全バージョンを出力し `Get-CimInstance` ます。

```powershell
$H = Get-Help -Full Get-CimInstance
Out-Printer -InputObject $H
```

`Get-Help` のヘルプトピックの完全バージョンを取得 `Get-CimInstance` し、変数に格納し `$H` ます。 **InputObject** パラメーターは、の値 `$H` をに渡し `Out-Printer` ます。

## PARAMETERS

### -InputObject

プリンターに送信するオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

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

### -Name

指定されたプリンターに出力を送信します。 パラメーター **名は省略可能です。**

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: PrinterName

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをにパイプすることができ `Out-Printer` ます。

## 出力

### なし

`Out-Printer` はオブジェクトを返しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

動詞を含むコマンドレットは、 `Out` オブジェクトを書式設定しません。 それらをレンダリングして、指定された表示先に送信するだけです。 書式設定されていないオブジェクトをコマンドレットに送信すると、コマンドレットによって、 `Out` レンダリングされる前に書式設定コマンドレットに送信されます。

`Out-Printer` プリンターにデータを送信しますが、パイプラインに出力オブジェクトを生成しません。 パイプを使用しての出力をに渡した場合 `Out-Printer` `Get-Member` 、は `Get-Member` オブジェクトが指定されていないことを報告します。

## 関連リンク

[Out-File](Out-File.md)

[Out-String](Out-String.md)
