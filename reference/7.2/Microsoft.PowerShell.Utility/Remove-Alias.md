---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/24/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/remove-alias?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-Alias
ms.openlocfilehash: 0ce13bef77e9ef9647809336aed650833dab51f3
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605431"
---
# Remove-Alias

## 概要
現在のセッションからエイリアスを削除します。

## SYNTAX

### 既定値 (既定値)

```
Remove-Alias [-Name] <String[]> [-Scope <String>] [-Force] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Remove-Alias` 現在の PowerShell セッションからエイリアスを削除します。 **オプション** プロパティを **ReadOnly** に設定してエイリアスを削除するには、 **Force** パラメーターを使用します。

`Remove-Alias`コマンドレットは、PowerShell 6.0 で導入されました。

## 例

### 例 1-エイリアスを削除する

この例で `del` は、コマンドレットを表すという名前のエイリアスを削除 `Remove-Item` します。

```powershell
Remove-Alias -Name del
```

### 例 2-すべての非定数エイリアスを削除する

この例では、 **Options** プロパティが **Constant** に設定されているエイリアスを除き、現在の PowerShell セッションからすべてのエイリアスを削除します。 コマンドを実行すると、他の PowerShell セッションまたは新しい PowerShell セッションでエイリアスを使用できるようになります。

```powershell
Get-Alias | Where-Object { $_.Options -NE "Constant" } | Remove-Alias -Force
```

`Get-Alias` PowerShell セッションのすべてのエイリアスを取得し、オブジェクトをパイプラインの下に送信します。
`Where-Object` はスクリプトブロックを使用し、自動変数 ( `$_` ) および **オプション** プロパティは現在のパイプラインオブジェクトを表します。 **NE** (等しくない) パラメーターは、 **Options** 値が **Constant** に設定されていないオブジェクトを選択します。 `Remove-Alias`**Force** パラメーターを使用して、PowerShell セッションから読み取り専用エイリアスを含むエイリアスを削除します。

## PARAMETERS

### -Force

コマンドレットによってエイリアスが削除されることを示します。これには、 **オプション** プロパティが **ReadOnly** に設定されているエイリアスも含まれます。 **Force** パラメーターは、 **Option** プロパティが **Constant** に設定されたエイリアスを削除することはできません。

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

### -Name

削除するエイリアスの名前を指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -スコープ

指定されたスコープのエイリアスのみに影響します。 既定のスコープは **Local** です。 詳細については、「 [about_Scopes](../microsoft.powershell.core/about/about_scopes.md)」を参照してください。

このパラメーターの有効値は、次のとおりです。

- グローバル
- ローカル
- スクリプト
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数。0は現在のスコープ、1はその親)

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Local
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String[]

パイプを使用してエイリアスオブジェクトを **削除** することができます。

## 出力

### なし

このコマンドレットは出力を返しません。

## 注

変更は現在のスコープにのみ影響します。 すべてのセッションからエイリアスを削除するには、PowerShell プロファイルに **削除エイリアス** コマンドを追加します。

詳細については、「 [about_Aliases](../microsoft.powershell.core/about/about_aliases.md)」を参照してください。

## 関連リンク

[about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)

[Export-Alias](Export-Alias.md)

[Get-Alias](Get-Alias.md)

[Import-Alias](Import-Alias.md)

[New-Alias](New-Alias.md)

[Set-Alias](Set-Alias.md)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)

