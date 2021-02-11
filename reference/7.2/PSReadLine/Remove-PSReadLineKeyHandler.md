---
external help file: Microsoft.PowerShell.PSReadLine2.dll-Help.xml
Locale: en-US
Module Name: PSReadLine
ms.date: 12/07/2018
online version: https://docs.microsoft.com/powershell/module/psreadline/remove-psreadlinekeyhandler?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-PSReadLineKeyHandler
ms.openlocfilehash: d280eba2e717ccfb0b896d62f42079390d1db848
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601398"
---
# Remove-PSReadLineKeyHandler

## 概要
キーバインドを削除します。

## SYNTAX

```
Remove-PSReadLineKeyHandler [-Chord] <String[]> [-ViMode <ViMode>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Remove-PSReadLineKeyHandler` 指定されたキーバインドを削除します。

## 例

### 例 1: バインディングを削除する

```powershell
Remove-PSReadLineKeyHandler -Chord Ctrl+B
```

このコマンドは、キーの組み合わせまたはコードからバインドを削除し `Ctrl+B` ます。 この `Ctrl+B` 記事では、コードを作成し `Set-PSReadLineKeyHandler` ます。

## PARAMETERS

### -コード

削除するキーまたはキーのシーケンスの配列を指定します。 単一のバインドは、1つの文字列を使用して指定します。 バインドが一連のキーである場合は、次の例のように、キーをコンマで区切ります。

`Ctrl+x,Ctrl+l`

このパラメーターは、文字列の配列を受け入れます。 各文字列は個別のバインドであり、1つのバインドのキーのシーケンスではありません。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Key

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ViMode

バインドが適用される vi モードを指定します。 使用可能な値: Insert、Command。

```yaml
Type: Microsoft.PowerShell.ViMode
Parameter Sets: (All)
Aliases:
Accepted values: Insert, Command

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### なし

## 注

## 関連リンク

[Get-PSReadLineKeyHandler](Get-PSReadLineKeyHandler.md)

[Get-PSReadLineOption](Get-PSReadLineOption.md)

[設定-PSReadLineOption](Set-PSReadLineOption.md)

[設定-PSReadLineKeyHandler](Set-PSReadLineKeyHandler.md)

