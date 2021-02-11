---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/clear-variable?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Clear-Variable
ms.openlocfilehash: 0381b48097f75d3195a82a67225cd8d6759e7433
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600095"
---
# Clear-Variable

## 概要
変数の値を削除します。

## SYNTAX

```
Clear-Variable [-Name] <String[]> [-Include <String[]>] [-Exclude <String[]>] [-Force] [-PassThru]
 [-Scope <String>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

**Clear variable** コマンドレットは、変数に格納されているデータを削除しますが、変数は削除しません。
その結果、変数の値は NULL (空) です。
変数に指定されたデータ型またはオブジェクト型がある場合、このコマンドレットは変数に格納されているオブジェクトの型を保持します。

## 例

### 例 1: 検索文字列で始まるグローバル変数の値を削除する

```
PS C:\> Clear-Variable my* -Scope Global
```

このコマンドは、名前が my で始まるグローバル変数の値を削除します。

### 例 2: 親スコープではなく、子スコープ内の変数をクリアする

```
PS C:\> $a=3
PS C:\> &{ Clear-Variable a }
PS C:\> $a
3
```

これらのコマンドは、子スコープ内の変数をクリアしても親スコープ内の変数はクリアされないことを示します。
最初のコマンドは、変数 $A の値を3に設定します。
2番目のコマンドは、invoke 演算子 (&) を使用して、新しいスコープで **変数のクリア** コマンドを実行します。
子スコープ内で (存在しない場合も) 変数がクリアされますが、ローカル スコープではクリアされません。
$A の値を取得する3番目のコマンドは、値3が影響を受けないことを示します。

### 例 3: 指定された変数の値を削除する

```
PS C:\> Clear-Variable -Name "Processes"
```

このコマンドは、Processes という名前の変数の値を削除します。
コマンドレットによって操作が完了した後も、プロセスという名前の変数は存在しますが、値は null になります。

## PARAMETERS

### -除外

このコマンドレットが操作で省略する項目の配列を指定します。
このパラメーターの値は、 *Name* パラメーターを修飾します。
「s*」などの名前要素またはパターンを入力します。
ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Force

読み取り専用である場合でも、コマンドレットが変数をオフにできるようにします。
Force パラメーターを使用しても、コマンドレットは定数をクリアできません。

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

### -Include

このコマンドレットによって操作に含まれる項目の配列を指定します。
このパラメーターの値は、 *Name* パラメーターを修飾します。
「s*」などの名前要素またはパターンを入力します。
ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Name

クリアする変数の名前を指定します。
ワイルドカードを使用できます。
このパラメーターは必須ですが、パラメーター名 ("Name") は省略可能です。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
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

このエイリアスが有効なスコープを指定します。

このパラメーターの有効値は、次のとおりです。

- グローバル
- ローカル
- スクリプト

現在のスコープに対して相対的な数値 (0 ~ スコープの数) を使用することもできます。ここで、0は現在のスコープ、1はその親です。
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

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### None または system.string です。

*PassThru* パラメーターを使用すると、このコマンドレットは、クリアされた変数を表す **system.string オブジェクトを** 生成します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

* 変数を値と共に削除するには、Remove-Variable また Remove-Item を使用します。

  このコマンドレットは、 *Force* パラメーターを使用した場合でも、定数またはシステムによって所有されている変数の値を削除しません。

  クリアする変数が存在しない場合、コマンドレットの効果はありません。
null 値を持つ変数は作成されません。

  また、 **Clear 変数** は、その組み込みエイリアスである clv を使用して参照することもできます。
詳細については、「about_Aliases」を参照してください。

*

## 関連リンク

[Get-Variable](Get-Variable.md)

[New-Variable](New-Variable.md)

[Remove-Variable](Remove-Variable.md)

[変数の設定](Set-Variable.md)

