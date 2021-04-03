---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/02/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-alias?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Alias
ms.openlocfilehash: f3f5f58060fb3ad0b5102eb46fe07859b426ed9c
ms.sourcegitcommit: c91f79576bc54e162bcc7adf78026417b2776687
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106273898"
---
# New-Alias

## 構文
新しいエイリアスを作成します。

## 構文

```
New-Alias [-Name] <String> [-Value] <String> [-Description <String>] [-Option <ScopedItemOptions>] [-PassThru]
 [-Scope <String>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## 説明

コマンドレットにより、 `New-Alias` 現在の PowerShell セッションで新しいエイリアスが作成されます。 を使用して作成された別名 `New-Alias` は、セッションを終了した後、または PowerShell を閉じた後は保存されません。
コマンドレットを使用し `Export-Alias` て、エイリアス情報をファイルに保存できます。 後でを使用し `Import-Alias` て、保存されたエイリアス情報を取得できます。

## 例

### 例 1: コマンドレットのエイリアスを作成する

```
New-Alias -Name "List" Get-ChildItem
```

このコマンドは、Get-ChildItem コマンドレットを表す、alias という名前のエイリアスを作成します。

### 例 2: コマンドレットの読み取り専用エイリアスを作成する

```
New-Alias -Name "C" -Value Get-ChildItem -Description "quick gci alias" -Option ReadOnly
Get-Alias -Name "C" | Format-List *
```

このコマンド `C` は、コマンドレットを表すという名前のエイリアスを作成し `Get-ChildItem` ます。 エイリアスとして "クイック wmi エイリアス" という説明が作成され、読み取り専用になります。 コマンドの最後の行では、を使用して新しいエイリアスを取得し、パイプを使用して Format-List にパイプを使用して `Get-Alias` 、すべての情報を表示します。

## パラメーター

### -Description

エイリアスの説明を指定します。 任意の文字列を入力できます。 説明にスペースが含まれる場合は、二重引用符で囲みます。

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

は `Set-Alias` 、という名前のエイリアスが既に存在する場合と同じように動作することを示します。

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

新しいエイリアスを指定します。 エイリアス名には任意の英数字を使用できますが、最初の文字を数字にすることはできません。

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

- `None`: 別名には制約がありません (既定値)
- `ReadOnly`: エイリアスは削除できますが、 **Force** パラメーターを使用しない限り、変更することはできません。
- `Constant`: エイリアスを削除または変更することはできません
- `Private`: エイリアスは現在のスコープでのみ使用できます
- `AllScope`: 作成された新しいスコープにエイリアスがコピーされます。
- `Unspecified`: オプションが指定されていません

これらの値はフラグベースの列挙体として定義されます。 このパラメーターを使用すると、複数の値を組み合わせて複数のフラグを設定できます。 値は、値の配列として **オプション** パラメーターに渡すことも、その値のコンマ区切りの文字列として渡すこともできます。 コマンドレットでは、バイナリまたは演算を使用して値を結合します。 配列として値を渡すのが最も簡単なオプションであり、値に対してタブ補完を使用することもできます。

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

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

新しいエイリアスのスコープを指定します。 このパラメーターの有効値は、次のとおりです。

- `Global`
- `Local`
- `Script`
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数 `0` 。は現在のスコープで、 `1` はその親です)。

`Local` は既定値です。 詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

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

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### None または system.servicemodel. エイリアス情報

**Passthru** パラメーターを使用すると、に `New-Alias` よって、新しいエイリアスを表す system.string オブジェクトが生成さ **れます。** それ以外の場合、このコマンドレットによる出力はありません。

## Notes

- 新しい別名を作成するには、またはを使用し `Set-Alias` `New-Alias` ます。 エイリアスを変更するには、を使用 `Set-Alias` します。 別名を削除するには、を使用 `Remove-Item` します。

## 関連リンク

[Export-Alias](Export-Alias.md)

[Get-Alias](Get-Alias.md)

[Import-Alias](Import-Alias.md)

[Set-Alias](Set-Alias.md)
