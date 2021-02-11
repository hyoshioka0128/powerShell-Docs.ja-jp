---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/format-wide?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Format-Wide
ms.openlocfilehash: ba61c2d24d85256c55a7409fc368de4407aad5a9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605433"
---
# Format-Wide

## 概要
各オブジェクトの 1 つのプロパティのみを表示する幅の広いテーブルに、オブジェクトを書式設定して表示します。

## SYNTAX

```
Format-Wide [[-Property] <Object>] [-AutoSize] [-Column <int>] [-GroupBy <Object>] [-View <string>]
  [-ShowError] [-DisplayError] [-Force] [-Expand <string>] [-InputObject <psobject>]
  [<CommonParameters>]
```

## Description

コマンドレットでは、オブジェクトを `Format-Wide` 各オブジェクトの1つのプロパティのみを表示する幅の広いテーブルとして書式設定します。 **プロパティ** パラメーターを使用して、表示するプロパティを指定できます。

## 例

### 例 1: 現在のディレクトリ内のファイルの名前を書式設定する

このコマンドは、現在のディレクトリにあるファイルの名前を画面上に 3 列で表示します。

```powershell
Get-ChildItem | Format-Wide -Column 3
```

Get-Childitem コマンドレットは、ディレクトリ内の各ファイルを表すオブジェクトを取得します。 パイプライン演算子 (|) は、パイプラインを介してファイルオブジェクトをに渡し `Format-Wide` 、出力用に書式設定します。 **Column** パラメーターは、列の数を指定します。

### 例 2: レジストリキーの名前を書式設定する

このコマンドは、HKEY_CURRENT_USER\Software\Microsoft キーにあるレジストリ キーの名前を表示します。

```powershell
Get-ChildItem HKCU:\software\microsoft | Format-Wide -Property pschildname -AutoSize
```

Get-Childitem コマンドレットは、キーを表すオブジェクトを取得します。 このパスは、HKCU:、PowerShell レジストリプロバイダーによって公開されているドライブの1つ、キーのパスの順に指定します。 パイプライン演算子 (|) は、パイプラインを介してレジストリキーオブジェクトをに渡し `Format-Wide` ます。これにより、出力用に書式が設定されます。 **Property** パラメーターはプロパティの名前を指定し、 **AutoSize** パラメーターは、読みやすくするために列を調整します。

### 例 3: 形式エラーのトラブルシューティング

次の例では、 **displayerror** パラメーターまたは **showerror** パラメーターを式に追加した結果を示します。

```powershell
PS /> Get-Date | Format-Wide { $_ / $null } -DisplayError


#ERR

PS /> Get-Date | Format-Wide { $_ / $null } -ShowError


Failed to evaluate expression " $_ / $null ".
+ CategoryInfo          : InvalidArgument: (12/21/2018 8:18:01 AM:PSObject) [], RuntimeException
+ FullyQualifiedErrorId : PSPropertyExpressionError
```

## PARAMETERS

### -AutoSize

データの幅に基づいて列のサイズと数を調整します。 既定では、列のサイズと数は、ビューによって決まります。 **AutoSize** パラメーターと **Column** パラメーターを同じコマンドで使用することはできません。

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

### -列

表示する列の数を指定します。 **AutoSize** パラメーターと **Column** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisplayError

コマンド ラインでのエラーを表示します。 このパラメーターはほとんど使用されませんが、コマンドで式を書式設定するときに、 `Format-Wide` 式が動作していないと思われる場合は、デバッグのために使用できます。

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

### -展開

コレクション内のオブジェクトに加えてコレクション オブジェクトを書式設定します。 このパラメーターは、ICollection (System.Collections) インターフェイスをサポートするオブジェクトを書式設定するために用意されています。 既定値は **Enumonly** です。

有効な値は次のとおりです。

- EnumOnly: コレクション内のオブジェクトのプロパティを表示します。
- CoreOnly: コレクションオブジェクトのプロパティを表示します。
- Both: コレクションオブジェクトのプロパティと、コレクション内のオブジェクトのプロパティを表示します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: CoreOnly, EnumOnly, Both

Required: False
Position: Named
Default value: EnumOnly
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

このコマンドレットが、コマンドの実行を妨げている制限をオーバーライドすることを示します。これは、変更によってセキュリティが損なわれないようにするためだけです。 たとえば、**Force** を指定すると、読み取り専用属性がオーバーライドされるか、ファイル パスを完成させるためにディレクトリが作成されますが、ファイルのアクセス許可は変更されません。

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

### -GroupBy

共有プロパティまたは値に基づき、グループ単位で出力を書式設定します。 式または出力のプロパティを入力します。

**GroupBy** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 名前 (またはラベル)- `<string>`
- 式 `<string>` または `<script block>`
- FormatString `<string>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

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

### -InputObject

書式設定するオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -プロパティ

表示するオブジェクト プロパティと、その表示順序を指定します。
ワイルドカードを使用できます。

このパラメーターを省略した場合、表示されるプロパティは、表示されるオブジェクトに依存します。 パラメーター名 "Property" は省略可能です。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 式 `<string>` または `<script block>`
- FormatString `<string>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ShowError

パイプラインを使用してエラーを送信します。 このパラメーターはほとんど使用されませんが、コマンドで式を書式設定するときに、 `Format-Wide` 式が動作していないと思われる場合は、デバッグのために使用できます。

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

### -ビュー

代替テーブル形式またはビューの名前を指定します。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをにパイプすることができ `Format-Wide` ます。

## 出力

### Microsoft. PowerShell. 内部形式

`Format-Wide` テーブルを表す書式オブジェクトを返します。

## 注

また、組み込みエイリアスであるを参照することもでき `Format-Wide` `fw` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

**GroupBy** パラメーターは、オブジェクトが並べ替えられていることを前提としています。 を使用し `Sort-Object` てオブジェクトをグループ化する前に、を使用し `Format-Custom` ます。

**View** パラメーターを使用すると、テーブルの代替形式を指定できます。 PowerShell ディレクトリ内のファイルで定義されているビューを使用することも、 `*.format.PS1XML` 新しい types.ps1xml ファイルに独自のビューを作成して、コマンドレットを使用して powershell に追加することもできます `Update-FormatData` 。

**View** パラメーターの代替ビューでは、テーブル形式を使用する必要があります。そうでない場合、コマンドは失敗します。 代替ビューが一覧の場合は、を使用 `Format-List` します。 代替ビューが一覧や表でない場合、Format-Custom を使用します。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[Format-Custom](Format-Custom.md)

[Format-Hex](Format-Hex.md)

[Format-List](Format-List.md)

[Format-Table](Format-Table.md)
