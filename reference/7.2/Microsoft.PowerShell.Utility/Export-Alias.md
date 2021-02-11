---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-alias?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-Alias
ms.openlocfilehash: 9da204513ed4a17eb8dc20481b878a4a783534f6
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599911"
---
# Export-Alias

## 概要
現在定義されているエイリアスに関する情報をファイルにエクスポートします。

## SYNTAX

### ByPath (既定値)

```
Export-Alias [-Path] <String> [[-Name] <String[]>] [-PassThru] [-As <ExportAliasFormat>] [-Append] [-Force]
 [-NoClobber] [-Description <String>] [-Scope <String>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Export-Alias -LiteralPath <String> [[-Name] <String[]>] [-PassThru] [-As <ExportAliasFormat>] [-Append]
 [-Force] [-NoClobber] [-Description <String>] [-Scope <String>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Export-Alias`コマンドレットは、現在のセッションのエイリアスをファイルにエクスポートします。
出力ファイルが存在しない場合は、コマンドレットにより作成されます。

`Export-Alias` では、特定のスコープまたはすべてのスコープのエイリアスをエクスポートできます。また、データを CSV 形式で生成したり、セッションまたは PowerShell プロファイルに追加できる一連の Set-Alias コマンドとしてエクスポートしたりすることができます。

## 例

### 例 1: エイリアスをエクスポートする

```powershell
Export-Alias -Path "alias.csv"
```

このコマンドは、現在のエイリアス情報を現在のディレクトリ内の Alias.csv というファイルにエクスポートします。

### 例 2: エクスポートファイルが既に存在する場合を除き、エイリアスをエクスポートする

```powershell
Export-Alias -Path "alias.csv" -NoClobber
```

このコマンドは、現在のセッションのエイリアスを Alias.csv ファイルにエクスポートします。

**NoClobber** パラメーターが指定されているため、現在のディレクトリに Alias.csv ファイルが既に存在する場合、コマンドは失敗します。

### 例 3: ファイルにエイリアスを追加する

```powershell
Export-Alias -Path "alias.csv" -Append -Description "Appended Aliases" -Force
```

このコマンドは、現在のセッションのエイリアスを Alias.csv ファイルに追加します。

このコマンドは、 **description** パラメーターを使用して、ファイルの先頭のコメントに説明を追加します。

また、このコマンドは、読み取り専用属性を持っている場合でも、 **Force** パラメーターを使用して既存の Alias.csv ファイルを上書きします。

### 例 4: エイリアスをスクリプトとしてエクスポートする

```powershell
Export-Alias -Path "alias.ps1" -As Script
Add-Content -Path $Profile -Value (Get-Content alias.ps1)
$S = New-PSSession -ComputerName Server01
Invoke-Command -Session $S -FilePath .\alias.ps1
```

この例では、によって生成されるスクリプトファイル形式を使用する方法を示し `Export-Alias` ます。

このコマンドは、セッションのエイリアスを Alias.ps1 ファイルにエクスポートします。
As パラメーターを使用してスクリプトの値を指定する **と** 、各エイリアスの Set-Alias コマンドを含むファイルが生成されます。

2 番目のコマンドは、Alias.ps1 ファイル内のエイリアスを CurrentUser-CurrentHost プロファイルに追加します。
プロファイルへのパスは、変数に保存され `$Profile` ます。
このコマンドは、コマンドレットを使用して、 `Get-Content` Alias.ps1 ファイルからエイリアスを取得し、コマンドレットを使用して `Add-Content` それらをプロファイルに追加します。
詳細については、「about_Profiles」を参照してください。

3番目と4番目のコマンドは、Alias.ps1 ファイルのエイリアスを Server01 コンピューター上のリモートセッションに追加します。
3番目のコマンドは、 `New-PSSession` コマンドレットを使用してセッションを作成します。
4番目のコマンドは、コマンドレットの **FilePath** パラメーターを使用して、 `Invoke-Command` 新しいセッションで Alias.ps1 ファイルを実行します。

## PARAMETERS

### -追加

このコマンドレットが、指定されたファイルの既存の内容を上書きするのではなく、指定したファイルに出力を追加することを示します。

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

### -As

出力形式を指定します。
CSV が既定値です。
このパラメーターの有効値は、次のとおりです。

- 市区.
コンマ区切り値 (CSV) 形式です。
- スクリプティング。
エクスポートさ `Set-Alias` れた各エイリアスに対してコマンドを作成します。
ファイル名拡張子 .ps1 を使用して出力ファイルに名前を付ける場合は、任意のセッションにエイリアスを追加するスクリプトとして実行できます。

```yaml
Type: Microsoft.PowerShell.Commands.ExportAliasFormat
Parameter Sets: (All)
Aliases:
Accepted values: Csv, Script

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description

エクスポートするファイルの説明を指定します。
ファイルの先頭のヘッダー情報に続いて、説明がコメントとして表示されます。

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

ユーザーに確認せずに、直ちにコマンドを実行します。

読み取り専用の属性がファイルに設定されている場合でも、出力ファイルを上書きします。

既定では、 `Export-Alias` 読み取り専用または非表示の属性が設定されている場合、またはコマンドで **NoClobber** パラメーターが使用されている場合を除き、は警告なしにファイルを上書きします。
コマンドで両方を使用する場合、 **NoClobber** パラメーターは **Force** パラメーターよりも優先されます。

**Force** パラメーターは、 `Export-Alias` hidden 属性を持つファイルを強制的に上書きすることはできません。

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

出力ファイルのパスを指定します。
**Path** と異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。
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
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

エクスポートするエイリアスの配列として、名前を指定します。
ワイルドカードを使用できます。

既定で `Export-Alias` は、はセッションまたはスコープ内のすべてのエイリアスをエクスポートします。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -NoClobber

`Export-Alias`コマンドで **Force** パラメーターが使用されている場合でも、このコマンドレットによってファイルが上書きされないことを示します。

**NoClobber** パラメーターを省略すると、 `Export-Alias` ファイルに対して読み取り専用属性が設定されていない限り、は警告を表示せずに既存のファイルを上書きします。
*NoClobber* は、読み取り専用 `Export-Alias` 属性を使用してファイルを上書きすることを許可する Force パラメーターよりも優先されます。

*NoClobber* では、 **Append** パラメーターによって既存のファイルにコンテンツが追加されるのを防ぐことはできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NoOverwrite

Required: False
Position: Named
Default value: None
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

### -Path

出力ファイルのパスを指定します。
ワイルドカードは許可されていますが、結果のパスの値を 1 つのファイル名に解決する必要があります。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -スコープ

エイリアスのエクスポート元となるスコープを指定します。
このパラメーターの有効値は、次のとおりです。

- グローバル
- ローカル
- スクリプト
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数で、0は現在のスコープ、1はその親)

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

### [なし] :

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### None または system.servicemodel. エイリアス情報

**Passthru** パラメーターを使用すると、によって、 `Export-Alias` エイリアスを表す system.string オブジェクトが返され **ます。**
それ以外の場合、このコマンドレットによる出力はありません。

## 注

* Export-Aliases の結果がファイルに返されるのみです。

## 関連リンク

[Get-Alias](Get-Alias.md)

[Import-Alias](Import-Alias.md)

[New-Alias](New-Alias.md)

[Set-Alias](Set-Alias.md)

