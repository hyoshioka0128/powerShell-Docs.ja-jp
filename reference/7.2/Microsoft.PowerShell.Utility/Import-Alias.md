---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/import-alias?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-Alias
ms.openlocfilehash: 20bc5d141b68cab19374afbe44b3472c72bf69bc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601793"
---
# Import-Alias

## 概要
ファイルからエイリアス一覧をインポートします。

## SYNTAX

### ByPath (既定値)

```
Import-Alias [-Path] <String> [-Scope <String>] [-PassThru] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Import-Alias -LiteralPath <String> [-Scope <String>] [-PassThru] [-Force] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`Import-Alias`コマンドレットは、ファイルからエイリアス一覧をインポートします。

Windows PowerShell 3.0 以降では、セキュリティ機能として、 `Import-Alias` 既定で既存のエイリアスは上書きされません。
既存のエイリアスを上書きするには、エイリアス ファイルが安全であることを確認した後で、**Force** パラメーターを使用します。

## 例

### 例 1: ファイルからエイリアスをインポートする

```powershell
Import-Alias test.txt
```

このコマンドは、test.txt という名前のファイルからエイリアス情報をインポートします。

## PARAMETERS

### -Force

既に定義されているか、または読み取り専用になっているエイリアスをコマンドレットがインポートできるようにします。
次のコマンドを使用して、現在定義されているエイリアスに関する情報を表示します。

`Get-Alias | Select-Object Name, Options`

対応するエイリアスは、読み取り専用である場合、**Options** プロパティの値に表示されます。

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

### -LiteralPath

エクスポートされたエイリアス情報を含むファイルのパスを指定します。
**Path** パラメーターと異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。
ワイルドカードとして解釈される文字はありません。
パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。
単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。
既定では、このコマンドレットによる出力はありません。

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

### -Path

エクスポートされたエイリアス情報を含むファイルのパスを指定します。
ワイルドカードは許可されますが、1 つの名前に解決される必要があります。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -スコープ

エイリアスのインポート先のスコープを指定します。
このパラメーターの有効値は、次のとおりです。

- グローバル
- ローカル
- スクリプト
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数。0は現在のスコープ、1はその親)

既定値は Local です。
詳細については、「about_Scopes」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
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

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

パイプを使用して、パスを含む文字列をにパイプすることができ `Import-Alias` ます。

## 出力

### None または system.servicemodel. エイリアス情報

**Passthru** パラメーターを使用すると、によって、 `Import-Alias` エイリアスを表す system.string オブジェクトが返され **ます。**
それ以外の場合、このコマンドレットによる出力はありません。

## 注

## 関連リンク

[Export-Alias](Export-Alias.md)

[Get-Alias](Get-Alias.md)

[New-Alias](New-Alias.md)

[Set-Alias](Set-Alias.md)

