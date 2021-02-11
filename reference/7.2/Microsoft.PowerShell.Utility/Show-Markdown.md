---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/23/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/show-markdown?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Show-Markdown
ms.openlocfilehash: d14e6332a2da5c86c4f8ada0d110c64e080437e7
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603228"
---
# Show-Markdown

## 概要
VT100 エスケープシーケンスを使用するか、HTML を使用してブラウザーで、Markdown ファイルまたは文字列をコンソールに表示します。

## SYNTAX

### パス (既定値)

```
Show-Markdown [-Path] <String[]> [-UseBrowser] [<CommonParameters>]
```

### InputObject

```
Show-Markdown -InputObject <PSObject> [-UseBrowser] [<CommonParameters>]
```

### LiteralPath

```
Show-Markdown -LiteralPath <String[]> [-UseBrowser] [<CommonParameters>]
```

## Description

`Show-Markdown`コマンドレットは、ターミナルまたはブラウザーで人間が判読できる形式で Markdown を表示するために使用されます。

`Show-Markdown` 端末がレンダリングする VT100 エスケープシーケンスを含む文字列を返すことができます (VT100 エスケープシーケンスがサポートされている場合)。 これは主に、ターミナルで Markdown ファイルを表示するために使用されます。 また、AsVT100EncodedString パラメーターを指定することで、を使用してこの文字列を取得することもでき `ConvertFrom-Markdown` ます。 

`Show-Markdown` では、ブラウザーを開いて、Markdown の表示バージョンを表示することもできます。 HTML に変換し、HTML ファイルを既定のブラウザーで開くことで、Markdown を表示します。

`Show-Markdown`を使用してターミナルで Markdown を表示する方法を変更でき `Set-MarkdownOption` ます。

このコマンドレットは、PowerShell 6.1 で導入されました。

## 例

### 例 1: パスを指定する単純な例

```powershell
Show-Markdown -Path ./README.md
```

### 例 2: 文字列を指定する単純な例

```powershell
@"
# Show-Markdown

## Markdown

You can now interact with Markdown via PowerShell!

*stars*
__underlines__
"@ | Show-Markdown
```

### 例 2: ブラウザーで Markdown を開く

```powershell
Show-Markdown -Path ./README.md -UseBrowser
```

## PARAMETERS

### -InputObject

ターミナルに表示される Markdown 文字列。 サポートされている形式で渡さない場合 `Show-Markdown` は、によってエラーが生成されます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: InputObject
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

Markdown ファイルのパスを指定します。 Path パラメーターと異なり、LiteralPath の値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

表示する Markdown ファイルのパスを指定します。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -UseBrowser

Markdown 入力を HTML としてコンパイルし、既定のブラウザーで開きます。

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

### システム管理. PSObject

### System.String[]

## 出力

### System.String

## 注

## 関連リンク

[ConvertFrom-Markdown](ConvertFrom-Markdown.md)

