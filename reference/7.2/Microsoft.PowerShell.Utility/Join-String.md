---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Module Name: Microsoft.PowerShell.Utility
ms.date: 12/12/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/join-string?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Join-String
ms.openlocfilehash: f737c8025f9fda3611a44177bd19e928f596d0aa
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602176"
---
# Join-String

## 概要
パイプラインのオブジェクトを1つの文字列に結合します。

## SYNTAX

### 既定値 (既定値)

```
Join-String [[-Property] <PSPropertyExpression>] [[-Separator] <String>] [-OutputPrefix <String>]
 [-OutputSuffix <String>] [-UseCulture] [-InputObject <PSObject[]>] [<CommonParameters>]
```

### SingleQuote

```
Join-String [[-Property] <PSPropertyExpression>] [[-Separator] <String>] [-OutputPrefix <String>]
 [-OutputSuffix <String>] [-SingleQuote] [-UseCulture] [-InputObject <PSObject[]>]
 [<CommonParameters>]
```

### DoubleQuote

```
Join-String [[-Property] <PSPropertyExpression>] [[-Separator] <String>] [-OutputPrefix <String>]
 [-OutputSuffix <String>] [-DoubleQuote] [-UseCulture] [-InputObject <PSObject[]>]
 [<CommonParameters>]
```

### フォーマット

```
Join-String [[-Property] <PSPropertyExpression>] [[-Separator] <String>] [-OutputPrefix <String>]
 [-OutputSuffix <String>] [-FormatString <String>] [-UseCulture] [-InputObject <PSObject[]>]
 [<CommonParameters>]
```

## Description

この `Join-String` コマンドレットは、パイプラインオブジェクトのテキストを1つの文字列に結合 (または結合) します。

パラメーターが指定されていない場合、パイプラインオブジェクトは文字列に変換され、既定の区切り記号と結合され `$OFS` ます。

プロパティ名を指定することにより、プロパティの値が文字列に変換され、文字列に結合されます。

スクリプトブロックは、プロパティ名の代わりに使用できます。 スクリプトブロックの結果は、結果を形成するために結合される前に文字列に変換されます。 オブジェクトのプロパティのテキスト、または文字列に変換されたオブジェクトの結果を組み合わせることができます。

このコマンドレットは、PowerShell 6.2 で導入されました。

## 例

### 例 1: ディレクトリ名を結合する

<!-- markdownlint-disable MD038 -->
この例では、ディレクトリ名を結合し、出力を二重引用符で囲み、ディレクトリ名をコンマとスペース ( `, ` ) で区切ります。 出力は文字列オブジェクトです。

```powershell
Get-ChildItem -Directory C:\ | Join-String -Property Name -DoubleQuote -Separator ', '
```

```Output
"PerfLogs", "Program Files", "Program Files (x86)", "Users", "Windows"
```

`Get-ChildItem`**directory** パラメーターを使用して、ドライブのすべてのディレクトリ名を取得し `C:\` ます。
オブジェクトは、パイプラインの下位に送信され `Join-String` ます。 **Property** パラメーターは、ディレクトリ名を指定します。 **DoubleQuote** パラメーターは、ディレクトリ名を二重引用符で囲みます。
**Separator** パラメーターは、ディレクトリ名を区切るためにコンマとスペース () を使用することを指定し `, ` ます。

`Get-ChildItem`オブジェクトは **DirectoryInfo** であり、 `Join-String` オブジェクトを **system.string** に変換します。

### 例 2: プロパティの部分文字列を使用してディレクトリ名を結合する

この例では、substring メソッドを使用して、ディレクトリ名の最初の4文字を取得し、その出力を単一引用符で囲み、ディレクトリ名をセミコロン () で区切り `;` ます。

```powershell
Get-ChildItem -Directory C:\ | Join-String -Property {$_.Name.SubString(0,4)} -SingleQuote -Separator ';'
```

```Output
'Perf';'Prog';'Prog';'User';'Wind'
```

`Get-ChildItem`**directory** パラメーターを使用して、ドライブのすべてのディレクトリ名を取得し `C:\` ます。
オブジェクトは、パイプラインの下位に送信され `Join-String` ます。

**プロパティ** パラメーターのスクリプトブロックでは、自動変数 () を使用して、 `$_` 各オブジェクトの **Name** プロパティの部分文字列を指定します。 部分文字列は、各ディレクトリ名の最初の4文字を取得します。 部分文字列は、文字の開始位置と終了位置を指定します。 **Singlequote** パラメーターは、ディレクトリ名を単一引用符で囲みます。 **Separator** パラメーターは、ディレクトリ名を区切るためにセミコロン () を使用するように指定し `;` ます。

自動変数と部分文字列の詳細については、「 [about_Automatic_Variables](../microsoft.powershell.core/about/about_automatic_variables.md) 」および「 [Substring](/dotnet/api/system.string.substring)」を参照してください。

### 例 3: 結合出力を別の行に表示する

この例では、サービス名をそれぞれ別の行に結合し、タブでインデントします。

```powershell
Get-Service -Name se* | Join-String -Property Name -Separator "`r`n`t" -OutputPrefix "Services:`n`t"
```

```Output
Services:
    seclogon
    SecurityHealthService
    SEMgrSvc
    SENS
    Sense
    SensorDataService
    SensorService
    SensrSvc
    SessionEnv
```

`Get-Service`**Name** パラメーターをと共に使用して、で始まるサービスを指定し `se*` ます。 アスタリスク ( `*` ) は、任意の文字のワイルドカードです。

オブジェクトは、 `Join-String` **プロパティ** パラメーターを使用してサービス名を指定するにパイプラインで送信されます。 **Separator** パラメーターは、キャリッジリターン ( `` `r `` )、改行 ( `` `n `` )、およびタブ () を表す3つの特殊文字を指定し `` `t `` ます。 **Outputprefix** は、ラベル **サービス** を挿入します。これには、出力の最初の行の前に新しい行とタブが挿入されます。

特殊文字の詳細については、「 [about_Special_Characters](..//microsoft.powershell.core/about/about_special_characters.md)」を参照してください。

### 例 4: オブジェクトからクラス定義を作成する

この例では、既存のオブジェクトをテンプレートとして使用して、PowerShell クラス定義を生成します。

このコードサンプルでは、スプラッティングを使用して、行の長さを減らし、読みやすさを向上させます。 詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md)」を参照してください。

```powershell
$obj = [pscustomobject] @{Name = "Joe"; Age = 42}
$parms = @{
  Property = "Name"
  FormatString = '  ${0}'
  OutputPrefix = "class {`n"
  OutputSuffix = "`n}`n"
  Separator = "`n"
}
$obj.PSObject.Properties | Join-String @parms
```

```Output
class {
  $Name
  $Age
}
```

## PARAMETERS

### -DoubleQuote

各パイプラインオブジェクトの文字列値を二重引用符で囲みます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DoubleQuote
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -FormatString

各項目の書式設定方法を指定する書式指定文字列。

```yaml
Type: System.String
Parameter Sets: Format
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

結合するテキストを指定します。 テキストが格納されている変数を入力するか、文字列に結合するオブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -OutputPrefix

出力文字列の前に挿入されるテキスト。 文字列には、キャリッジリターン ( `` `r `` )、改行 ( `` `n `` )、タブ () などの特殊文字を含めることができ `` `t `` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: op

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutputSuffix

出力文字列に追加されるテキスト。 文字列には、キャリッジリターン ( `` `r `` )、改行 ( `` `n `` )、タブ () などの特殊文字を含めることができ `` `t `` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: os

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -プロパティ

パイプラインオブジェクトをテキストに投影するプロパティの名前、またはプロパティ式。

```yaml
Type: Microsoft.PowerShell.Commands.PSPropertyExpression
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Separator

各パイプラインオブジェクトのテキストの間に挿入されるコンマやセミコロンなどのテキストまたは文字。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SingleQuote

各パイプラインオブジェクトの文字列値を単一引用符で囲みます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: SingleQuote
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseCulture

現在のカルチャの区切り記号を項目の区切り記号として使用します。 カルチャのリスト区切り記号を検索するには、コマンドを使用し `(Get-Culture).TextInfo.ListSeparator` ます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

## 出力

### System.String

## 注

## 関連リンク

[about_Automatic_Variables](../microsoft.powershell.core/about/about_automatic_variables.md)

[about_Special_Characters](..//microsoft.powershell.core/about/about_special_characters.md)

[出現](/dotnet/api/system.string.substring)

