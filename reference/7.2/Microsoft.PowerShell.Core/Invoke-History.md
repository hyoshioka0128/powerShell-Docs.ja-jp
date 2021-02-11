---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 05/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-history?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-History
ms.openlocfilehash: 7fb4341a84706f7d31d463bb527a1a31f387c2ae
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602014"
---
# Invoke-History

## 概要
セッションの履歴からコマンドを実行します。

## SYNTAX

```
Invoke-History [[-Id] <String>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンド `Invoke-History` レットは、セッションの履歴からコマンドを実行します。 コマンドを表すオブジェクトを Get-History からに渡すことも `Invoke-History` 、 **Id** 番号を使用して現在の履歴内のコマンドを特定することもできます。 コマンドの識別番号を調べるには、コマンドレットを使用し `Get-History` ます。

セッション履歴は、 **Psreadline** モジュールによって保持されている履歴とは別に管理されます。
どちらの履歴も、 **Psreadline** が読み込まれたセッションで使用できます。 このコマンドレットは、セッション履歴でのみ機能します。 詳細については、「」、「 [about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)」を参照してください。

## 例

### 例 1: 履歴で最新のコマンドを実行する

この例では、セッション履歴の最後のコマンド、または最新のコマンドを実行します。 このコマンドは `r` 、のエイリアスであるとして省略でき `Invoke-History` ます。

```powershell
Invoke-History
```

### 例 2: 指定した ID を持つコマンドを実行する

この例では、 **Id** 132 のセッション履歴でコマンドを実行します。 **Id** パラメーターの名前は省略可能であるため、このコマンドは `Invoke-History 132` 、、 `ihy 132` 、またはのように省略できます `r 132` 。

```powershell
Invoke-History -Id 132
```

### 例 3: コマンドテキストを使用して最新のコマンドを実行する

この例では、セッション履歴の最新のコマンドを実行し `Get-Process` ます。 **Id** パラメーターに文字を入力すると、は、 `Invoke-History` 最後のコマンドで始まるパターンに一致する最初のコマンドを実行します。

```powershell
Invoke-History -Id get-pr
```

> [!NOTE]
> パターンマッチングでは大文字と小文字が区別されませんが、パターンは行の先頭と一致します。

### 例 4: 履歴から一連のコマンドを実行する

この例では、16 ~ 24 のコマンドを実行します。 **Id** 値は1つしか表示できないため、コマンドは、コマンドレットを使用して、 `ForEach-Object` `Invoke-History` 各 **id** 値に対してコマンドを1回実行します。

```powershell
16..24 | ForEach {Invoke-History -Id $_ }
```

### 例 5

この例では、コマンド 255 (249 ~ 255) で終わる、履歴内の7つのコマンドを実行します。 `Get-History`コマンドレットを使用して、コマンドを取得します。 **Id** 値は1つしか表示できないため、コマンドは、コマンドレットを使用して、 `ForEach-Object` `Invoke-History` 各 **id** 値に対してコマンドを1回実行します。

```powershell
Get-History -Id 255 -Count 7 | ForEach {Invoke-History -Id $_.Id}
```

## PARAMETERS

### -Id

履歴内のコマンドの **Id** を指定します。 コマンドの **Id** 番号、またはコマンドの最初のいくつかの文字を入力できます。

文字を入力すると、は `Invoke-History` 最新のコマンドと最初に一致します。 このパラメーターを省略すると、 `Invoke-History` は最後のコマンド、または最新のコマンドを実行します。 コマンドの **Id** 番号を検索するには、コマンドレットを使用し `Get-History` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
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

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

このコマンドレットには、履歴 **Id** をパイプすることができます。

## 出力

### なし

このコマンドレットは出力を生成しませんが、実行するコマンドによって出力が生成される場合があり `Invoke-History` ます。

## 注

セッション履歴は、セッション中に入力したコマンドの一覧です。 セッション履歴は、実行の順序、状態、コマンドの開始時刻と終了時刻を表します。 各コマンドを入力すると、PowerShell によってそのコマンドが履歴に追加され、再利用できるようになります。 セッションの履歴の詳細については、「 [about_History](About/about_History.md)」を参照してください。

また、組み込みのエイリアスであるとを参照することもでき `Invoke-History` `r` `ihy` ます。 詳細については、「 [about_Aliases](About/about_Aliases.md)」を参照してください。

## 関連リンク

[Add-History](Add-History.md)

[Clear-History](Clear-History.md)

[Get-History](Get-History.md)

