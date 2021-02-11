---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-host?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Host
ms.openlocfilehash: 01d045a6254b40161462bf36976fdbf57f205705
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599303"
---
# Write-Host

## 概要

カスタマイズした出力をホストに書き込みます。

## SYNTAX

```
Write-Host [[-Object] <Object>] [-NoNewline] [-Separator <Object>] [-ForegroundColor <ConsoleColor>]
 [-BackgroundColor <ConsoleColor>] [<CommonParameters>]
```

## Description

`Write-Host`コマンドレットの主な目的は、-(ホスト) から表示専用の出力を生成することです。たとえば、[読み取りホスト](Read-Host.md)との組み合わせでユーザーに入力を求める場合などに色付きのテキストを印刷することができます。
`Write-Host`[ToString ()](/dotnet/api/system.object.tostring)メソッドを使用して出力を書き込みます。 これに対し、パイプラインにデータを出力するには、 [書き込み出力](Write-Output.md) または暗黙の出力を使用します。

パラメーターを使用してテキストの色を指定できます。また、パラメーターを使用して背景色を指定することもでき `ForegroundColor` `BackgroundColor` ます。 Separator パラメーターを使用すると、個々の表示オブジェクトを区切るための文字列を指定できます。 具体的な結果は、PowerShell をホストしているプログラムによって異なります。

> [!NOTE]
> Windows PowerShell 5.0 以降では、を `Write-Host` 使用して、 `Write-Information` `Write-Host` 情報ストリームに出力を出力することができます。 これにより、後方互換性を維持しながら、を使用して書き込まれたデータを **キャプチャ** または **抑制** でき `Write-Host` ます。
>
> ユーザー `$InformationPreference` 設定変数と `InformationAction` 共通パラメーターは、メッセージには影響しません `Write-Host` 。 この規則の例外はです。これにより `-InformationAction Ignore` 、出力が実質的に抑制さ `Write-Host` れます。 (「例5」を参照)

## 例

### 例 1: 新しい行を追加せずにコンソールに書き込む

```powershell
Write-Host "no newline test " -NoNewline
Write-Host "second string"
```

```output
no newline test second string
```

このコマンドは、パラメーターを使用して "no 改行テスト" という文字列を表示し `NoNewline` ます。

2番目の文字列が書き込まれますが、文字列を区切る改行がないため、最初の文字列と同じ行に記述されます。

### 例 2: コンソールに書き込んで区切り記号を含める

```powershell
Write-Host (2,4,6,8,10,12) -Separator ", +2= "
```

```output
2, +2= 4, +2= 6, +2= 8, +2= 10, +2= 12
```

このコマンドは、2 ~ 12 の偶数を表示します。 **Separator** パラメーターを使用して、文字列 `, +2= ` (コンマ、スペース、、 `+` 、 `2` `=` 、スペース) を追加します。

### 例 3: 異なるテキストと背景の色で記述する

```powershell
Write-Host (2,4,6,8,10,12) -Separator ", -> " -ForegroundColor DarkGreen -BackgroundColor White
```

```output
2, -> 4, -> 6, -> 8, -> 10, -> 12
```

このコマンドは、2 ~ 12 の偶数を表示します。 この例では、パラメーターを使用して `ForegroundColor` 濃い緑のテキストを出力し、パラメーターを使用して `BackgroundColor` 白い背景を表示しています。

### 例 4: 異なるテキストと背景の色で記述する

```powershell
Write-Host "Red on white text." -ForegroundColor red -BackgroundColor white
```

```output
Red on white text.
```

このコマンドは、文字列 "Red on white text" を表示します。 テキストは、パラメーターで定義されているように、赤色で表示され `ForegroundColor` ます。 背景は、パラメーターで定義されているように白に `BackgroundColor` なります。

### 例 5: Write-Host からの出力を抑制する

```powershell
# The following two statements can be used to effectively suppress output from Write-Host
Write-Host "I won't print" -InformationAction Ignore
Write-Host "I won't print" 6>$null
```

```output

```

これらのコマンドは、コマンドレットの出力を効果的に抑制 `Write-Host` します。 最初の例では、パラメーターを値と共に使用して、 `InformationAction` `Ignore` 情報ストリームへの出力を抑制しています。
2番目の例では、コマンドの情報ストリームを変数にリダイレクト `$null` し、その結果を抑制します。

## PARAMETERS

### -BackgroundColor

背景色を指定します。 既定値はありません。 このパラメーターの有効値は、次のとおりです。

- Black
- 濃い青
- 濃い緑
- 濃いシアン
- 濃い赤
- 濃いマゼンタ
- 濃い黄色
- グレー
- 濃い灰色
- ブルー
- [緑]
- シアン
- [赤]
- 赤紫
- 黄
- 白

```yaml
Type: System.ConsoleColor
Parameter Sets: (All)
Aliases:
Accepted values: Black, DarkBlue, DarkGreen, DarkCyan, DarkRed, DarkMagenta, DarkYellow, Gray, DarkGray, Blue, Green, Cyan, Red, Magenta, Yellow, White

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ForegroundColor

文字の色を指定します。 既定値はありません。 このパラメーターの有効値は、次のとおりです。

- Black
- 濃い青
- 濃い緑
- 濃いシアン
- 濃い赤
- 濃いマゼンタ
- 濃い黄色
- グレー
- 濃い灰色
- ブルー
- [緑]
- シアン
- [赤]
- 赤紫
- 黄
- 白

```yaml
Type: System.ConsoleColor
Parameter Sets: (All)
Aliases:
Accepted values: Black, DarkBlue, DarkGreen, DarkCyan, DarkRed, DarkMagenta, DarkYellow, Gray, DarkGray, Blue, Green, Cyan, Red, Magenta, Yellow, White

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -なし Wline

入力オブジェクトの文字列形式が連結され、出力が形成されます。 出力文字列間にスペースや改行は挿入されません。 最後の出力文字列の後に改行は追加されません。

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

### -オブジェクト

ホストに表示するオブジェクト。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases: Msg, Message

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Separator

ホストによって表示されるオブジェクト間に挿入する区切り文字列を指定します。

```yaml
Type: System.Object
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

### System.Object

ホストに書き込むオブジェクトはパイプ処理で渡すことができます。

## 出力

### なし

`Write-Host` オブジェクトをホストに送信します。 このコマンドはオブジェクトを返しません。 ただし、ホストは、に送信するオブジェクトを表示し `Write-Host` ます。

## 注

- コレクションをホストに書き込む場合、コレクションの要素は、1つの空白で区切られた同じ行に出力されます。 これは、 **Separator** パラメーターを使用してオーバーライドできます。

- プロパティを持つオブジェクトなどの非プリミティブデータ型では、予期しない結果が発生し、意味のある出力が得られない場合があります。 たとえば、 `Write-Host @{a = 1; b = 2}` は `System.Collections.DictionaryEntry System.Collections.DictionaryEntry` ホストに出力します。

## 関連リンク

[Clear-Host](../Microsoft.PowerShell.Core/Clear-Host.md)

[Out-Host](../Microsoft.PowerShell.Core/Out-Host.md)

[Write-Debug](Write-Debug.md)

[Write-Error](Write-Error.md)

[Write-Output](Write-Output.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)
