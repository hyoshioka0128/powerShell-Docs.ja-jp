---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 11/02/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-markdown?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-Markdown
ms.openlocfilehash: c694e73e69c1e51ecf88f445684923ef5f19113f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601811"
---
# ConvertFrom-Markdown

## 概要
文字列またはファイルの内容を **Markdowninfo** オブジェクトに変換します。

## SYNTAX

### PathParamSet (既定)

```
ConvertFrom-Markdown [-Path] <String[]> [-AsVT100EncodedString] [<CommonParameters>]
```

### LiteralParamSet

```
ConvertFrom-Markdown -LiteralPath <String[]> [-AsVT100EncodedString] [<CommonParameters>]
```

### InputObjParamSet

```
ConvertFrom-Markdown -InputObject <PSObject> [-AsVT100EncodedString] [<CommonParameters>]
```

## Description

このコマンドレットは、指定されたコンテンツを **Markdowninfo** に変換します。 **Path** パラメーターにファイルパスを指定すると、ファイルの内容が変換されます。 出力オブジェクトには、次の3つのプロパティがあります。

- **Token** プロパティには、変換されたオブジェクトの抽象構文ツリー (AST) があります。
- **Html** プロパティには、指定した入力の html 変換が含まれます。
- **AsVT100EncodedString** パラメーターが指定されている場合、 **VT100EncodedString** プロパティには、ANSI (VT100) エスケープシーケンスで変換された文字列が含まれます。

このコマンドレットは、PowerShell 6.1 で導入されました。

## 例

### 例 1: Markdown コンテンツを含むファイルを HTML に変換する

```powershell
ConvertFrom-Markdown -Path .\README.md
```

**Markdowninfo** オブジェクトが返されます。 **Tokens** プロパティには、ファイルの変換された内容の AST が含まれてい `README.md` ます。 **Html** プロパティには、html で変換されたファイルの内容があり `README.md` ます。

### 例 2: Markdown コンテンツを含むファイルを VT100 でエンコードされた文字列に変換する

```powershell
ConvertFrom-Markdown -Path .\README.md -AsVT100EncodedString
```

**Markdowninfo** オブジェクトが返されます。 **Tokens** プロパティには、ファイルの変換された内容の AST が含まれてい `README.md` ます。 **VT100EncodedString** プロパティには、VT100 でエンコードされた、ファイルの内容が変換された文字列があり `README.md` ます。

### 例 3: Markdown コンテンツを含む入力オブジェクトを VT100 でエンコードされた文字列に変換する

```powershell
Get-Item .\README.md | ConvertFrom-Markdown -AsVT100EncodedString
```

**Markdowninfo** オブジェクトが返されます。 からの **FileInfo** オブジェクト `Get-Item` は、VT100 でエンコードされた文字列に変換されます。 **Tokens** プロパティには、ファイルの変換された内容の AST が含まれてい `README.md` ます。 **VT100EncodedString** プロパティには、VT100 でエンコードされた、ファイルの内容が変換された文字列があり `README.md` ます。

### 例 4: Markdown コンテンツを含む文字列を VT100 でエンコードされた文字列に変換する

```powershell
"**Bold text**" | ConvertFrom-Markdown -AsVT100EncodedString
```

**Markdowninfo** オブジェクトが返されます。 指定された文字列は、VT100 でエンコードされた `**Bold text**` 文字列に変換され、 **VT100EncodedString** プロパティで使用できます。

## PARAMETERS

### -AsVT100EncodedString

出力を VT100 エスケープコードを使用して文字列としてエンコードするかどうかを指定します。

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

### -InputObject

変換するオブジェクトを指定します。 **System.string 型の** オブジェクトを指定すると、文字列が変換されます。 System.object 型のオブジェクトを指定すると、オブジェクトによって指定されたファイルの内容が変換され **ます。** 他の種類のオブジェクトの場合、エラーが発生します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: InputObjParamSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

変換するファイルへのパスを指定します。

```yaml
Type: System.String[]
Parameter Sets: LiteralParamSet
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

変換するファイルへのパスを指定します。

```yaml
Type: System.String[]
Parameter Sets: PathParamSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

## 出力

### Microsoft... Markdownrender

## 注

## 関連リンク

[Markdown パーサー](https://github.com/lunet-io/markdig)

[ANSI エスケープコード](https://wikipedia.org/wiki/ANSI_escape_code)

