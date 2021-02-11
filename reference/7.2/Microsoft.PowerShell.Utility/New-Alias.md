---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-alias?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Alias
ms.openlocfilehash: 7f226af3cb221543cdae88930f661e2343d954b9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602780"
---
# New-Alias

## 概要
新しいエイリアスを作成します。

## SYNTAX

```
New-Alias [-Name] <String> [-Value] <String> [-Description <String>] [-Option <ScopedItemOptions>] [-PassThru]
 [-Scope <String>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

**新しい** alias コマンドレットは、現在の PowerShell セッションで新しいエイリアスを作成します。
**新しいエイリアス** を使用して作成されたエイリアスは、セッションを終了した後、または PowerShell を閉じた後は保存されません。
Export-Alias コマンドレットを使用して、エイリアス情報をファイルに保存できます。
後で、 **インポートエイリアス** を使用して、保存されたエイリアス情報を取得できます。

## 例

### 例 1: コマンドレットのエイリアスを作成する

```
PS C:\> New-Alias -Name "List" Get-ChildItem
```

このコマンドは、Get-ChildItem コマンドレットを表す、alias という名前のエイリアスを作成します。

### 例 2: コマンドレットの読み取り専用エイリアスを作成する

```
PS C:\> New-Alias -Name "W" -Value Get-WmiObject -Description "quick wmi alias" -Option ReadOnly
PS C:\> Get-Alias -Name "W" | Format-List *
```

このコマンドは、Get-WmiObject コマンドレットを表す W という名前のエイリアスを作成します。
エイリアスとして "クイック wmi エイリアス" という説明が作成され、読み取り専用になります。
コマンドの最後の行は、Get-Alias を使用して新しいエイリアスを取得し、パイプを使用して Format-List に渡します。これによりすべての情報が表示されます。

## PARAMETERS

### -Description

エイリアスの説明を指定します。
任意の文字列を入力できます。
説明にスペースが含まれる場合は、二重引用符で囲みます。

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

### -Force

という名前のエイリアスが既に存在する場合に、コマンドレットが Set-Alias のように動作することを示します。

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

### -Name

新しいエイリアスを指定します。
エイリアス名には任意の英数字を使用できますが、最初の文字を数字にすることはできません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Option

エイリアスの **Options** プロパティの値を指定します。
有効な値は次のとおりです。

- None: 別名には制約がありません (既定値)
- ReadOnly: エイリアスは削除できますが、 **Force** パラメーターを使用しない限り変更できません
- 定数: エイリアスを削除または変更することはできません
- プライベート: エイリアスは現在のスコープでのみ使用できます
- AllScope: エイリアスは、作成された新しいスコープにコピーされます。
- 未指定: オプションが指定されていません

セッション内のすべてのエイリアスの **Options** プロパティを表示するには、「」と入力 `Get-Alias | Format-Table -Property Name, Options -AutoSize` します。

```yaml
Type: System.Management.Automation.ScopedItemOptions
Parameter Sets: (All)
Aliases:
Accepted values: None, ReadOnly, Constant, Private, AllScope, Unspecified

Required: False
Position: Named
Default value: [System.Management.Automation.ScopedItemOptions]::None
Accept pipeline input: False
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

### -スコープ

新しいエイリアスのスコープを指定します。
このパラメーターの有効値は、次のとおりです。

- グローバル
- ローカル
- スクリプト
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数。0は現在のスコープ、1はその親)。

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

### -Value

エイリアスを作成するコマンドレットまたはコマンド要素の名前を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
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

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### None または system.servicemodel. エイリアス情報

*Passthru* パラメーターを使用すると、新しいエイリアスを表す system.string **オブジェクトが****新しいエイリアス** によって生成されます。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

* 新しい別名を作成するには、またはを使用し `Set-Alias` `New-Alias` ます。 エイリアスを変更するには、を使用 `Set-Alias` します。 別名を削除するには、を使用 `Remove-Item` します。

## 関連リンク

[Export-Alias](Export-Alias.md)

[Get-Alias](Get-Alias.md)

[Import-Alias](Import-Alias.md)

[Set-Alias](Set-Alias.md)

