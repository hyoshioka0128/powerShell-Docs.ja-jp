---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/format-custom?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Format-Custom
ms.openlocfilehash: ff4b2dda3f12fa34fc518c6a180f3213ba873d90
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600982"
---
# Format-Custom

## 概要
カスタマイズされたビューを使用して、出力を書式設定します。

## SYNTAX

```
Format-Custom [[-Property] <Object[]>] [-Depth <Int32>] [-GroupBy <Object>] [-View <String>]
 [-ShowError] [-DisplayError] [-Force] [-Expand <String>] [-InputObject <PSObject>]
 [<CommonParameters>]
```

## Description

`Format-Custom`コマンドレットは、代替ビューで定義されているように、コマンドの出力を書式設定します。
`Format-Custom` は、テーブルまたはリストだけではないビューを表示するように設計されています。 PowerShell で定義されたビューを使用することも、新しいファイルに独自のビューを作成 `format.ps1xml` して、コマンドレットを使用して powershell に追加することもできます `Update-FormatData` 。

## 例

### 例 1: カスタムビューを使用して出力の書式を設定する

```powershell
Get-Command Start-Transcript | Format-Custom -View MyView
```

このコマンドは、 `Start-Transcript` ユーザーが作成したカスタムビューである MyView ビューで定義されている形式で、コマンドレットに関する情報を書式設定します。 このコマンドを正常に実行するには、最初に新しい TYPES.PS1XML ファイルを作成し、 **Myview** ビューを定義してから、コマンドを使用して `Update-FormatData` Types.ps1xml ファイルを PowerShell に追加する必要があります。

### 例 2: 既定のビューで出力を書式設定する

```powershell
Get-Process Winlogon | Format-Custom
```

このコマンドは、 **Winlogon** プロセスに関する情報をカスタマイズされた代替ビューで書式設定します。
コマンドは **View** パラメーターを使用しないため、では `Format-Custom` 既定のカスタムビューを使用してデータを書式設定します。

### 例 3: 形式エラーのトラブルシューティング

次の例では、 **displayerror** パラメーターまたは **showerror** パラメーターを式に追加した結果を示します。

```powershell
PC /> Get-Date | Format-Custom DayOfWeek,{ $_ / $null } -DisplayError

class DateTime
{
  DayOfWeek = Friday
   $_ / $null  = #ERR
}


PC /> Get-Date | Format-Custom DayOfWeek,{ $_ / $null } -ShowError

class DateTime
{
  DayOfWeek = Friday
   $_ / $null  =
}

Failed to evaluate expression " $_ / $null ".
+ CategoryInfo          : InvalidArgument: (12/21/2018 8:01:04 AM:PSObject) [], RuntimeException
+ FullyQualifiedErrorId : PSPropertyExpressionError
```

## PARAMETERS

### -深さ

表示する列の数を指定します。

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

コマンド ラインでのエラーを表示します。 このパラメーターはほとんど使用されませんが、コマンドで式を書式設定するときに、 `Format-Custom` 式が動作していないと思われる場合は、デバッグのために使用できます。

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

コレクション内のオブジェクトに加えてコレクション オブジェクトを書式設定します。 このパラメーター **は、system.string インターフェイスをサポート** するオブジェクトを書式設定するように設計されています。 既定値は **Enumonly** です。

有効な値は次のとおりです。

- EnumOnly: コレクション内のオブジェクトのプロパティを表示します。
- CoreOnly: コレクションオブジェクトのプロパティを表示します。
- Both: コレクションオブジェクトのプロパティとコレクション内のオブジェクトを表示します。

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

すべてのエラー情報を表示することをコマンドレットに指示します。 **Displayerror** パラメーターまたは **showerror** パラメーターと共に使用します。 既定では、エラー オブジェクトがエラーまたは表示ストリームに書き込まれるとき、エラー情報の一部のみが表示されます。

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

このパラメーターを省略した場合、表示されるプロパティは、表示されるオブジェクトに依存します。 パラメーター名の **プロパティ** は省略可能です。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 式 `<string>` または `<script block>`
- デプス `<int32>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ShowError

パイプラインを使用してエラーを送信します。 このパラメーターはほとんど使用されませんが、コマンドで式を書式設定するときに、 `Format-Custom` 式が動作していないと思われる場合は、デバッグのために使用できます。

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

代替形式またはビューの名前を指定します。 このパラメーターを省略すると、は `Format-Custom` 既定のカスタムビューを使用します。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

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

パイプを使用して任意のオブジェクトをにパイプすることができ `Format-Custom` ます。

## 出力

### Microsoft. PowerShell. 内部形式

`Format-Custom` 表示を表す書式オブジェクトを返します。

## 注

`Format-Custom` は、テーブルまたはリストだけではないビューを表示するように設計されています。 代替テーブルビューを表示するには、を使用 `Format-Table` します。 代替のリストビューを表示するには、を使用し `Format-List` ます。

また、組み込みエイリアスであるを参照することもでき `Format-Custom` `fc` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

**GroupBy** パラメーターは、オブジェクトが並べ替えられていることを前提としています。 を使用してオブジェクトをグループ化する前に、を使用して `Format-Custom` オブジェクトを `Sort-Object` 並べ替えます。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[Format-Hex](Format-Hex.md)

[Format-List](Format-List.md)

[Format-Table](Format-Table.md)

[Format-Wide](Format-Wide.md)

[Get-Process](../Microsoft.PowerShell.Management/Get-Process.md)
