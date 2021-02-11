---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-alias?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Alias
ms.openlocfilehash: 7b6f639d1c5685e341260c056a1dd0a17fe9f46d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600092"
---
# Get-Alias

## 概要
現在のセッションのエイリアスを取得します。

## SYNTAX

### 既定値 (既定値)

```
Get-Alias [[-Name] <String[]>] [-Exclude <String[]>] [-Scope <String>] [<CommonParameters>]
```

### 定義

```
Get-Alias [-Exclude <String[]>] [-Scope <String>] [-Definition <String[]>] [<CommonParameters>]
```

## Description

**Get Alias** コマンドレットは、現在のセッションのエイリアスを取得します。
これには、組み込みエイリアス、設定またはインポートしたエイリアス、および PowerShell プロファイルに追加したエイリアスが含まれます。

既定では、 **Get エイリアス** はエイリアスを受け取り、コマンド名を返します。
*定義* パラメーターを使用すると、 **Get エイリアス** はコマンド名を受け取り、そのエイリアスを返します。

Windows PowerShell 3.0 以降では、必要な情報を簡単に見つけられるように、 **Get エイリアス** によって、ハイフンではないエイリアス名が形式で表示され `<alias> -> <definition>` ます。

## 例

### 例 1: 現在のセッションのすべてのエイリアスを取得する

```
PS C:\> Get-Alias

CommandType     Name
-----------     ----
Alias           % -> ForEach-Object
Alias           ? -> Where-Object
Alias           ac -> Add-Content
Alias           asnp -> Add-PSSnapin
Alias           cat -> Get-Content
Alias           cd -> Set-Location
Alias           chdir -> Set-Location
Alias           clc -> Clear-Content
Alias           clear -> Clear-Host
Alias           clhy -> Clear-History
...
```

このコマンドは、現在のセッションのすべてのエイリアスを取得します。

出力には、 `<alias> -> <definition>` Windows PowerShell 3.0 で導入された形式が示されています。
通常、ハイフン付きのエイリアスは、ニックネームの代わりに、コマンドレットと関数の推奨される名前になるため、この形式はハイフンを含まないエイリアスのみに使用されます。

### 例 2: 名前を指定してエイリアスを取得する

```powershell
Get-Alias -Name gp*, sp* -Exclude *ps
```

このコマンドは、gp または sp で始まるすべてのエイリアスを取得します。ただし、末尾が ps であるエイリアスは除きます。

### 例 3: コマンドレットのエイリアスを取得する

```powershell
Get-Alias -Definition Get-ChildItem
```

このコマンドは、Get-ChildItem コマンドレットのエイリアスを取得します。

既定では、 **Get alias** コマンドレットは、エイリアスがわかっている場合に項目名を取得します。
*定義* パラメーターは、項目名がわかっている場合にエイリアスを取得します。

### 例 4: プロパティによってエイリアスを取得する

```powershell
Get-Alias | Where-Object {$_.Options -Match "ReadOnly"}
```

このコマンドは、Options プロパティの値が ReadOnly であるすべてのエイリアスを取得します。
このコマンドは、ReadOnly オプションがあるため、PowerShell に組み込まれているエイリアスを簡単に見つけることができます。

Options は、 **エイリアス** が取得する別名情報オブジェクトの1つのプロパティにすぎません。
エイリアス情報オブジェクトのすべてのプロパティとメソッドを検索するには、「」と入力 `Get-Alias | get-member` します。

### 例 5: 名前でエイリアスを取得し、文字の先頭でフィルター処理する

```powershell
Get-Alias -Definition "*-PSSession" -Exclude e* -Scope Global
```

この例では、"e" で始まるエイリアスを除き、"-PSSession" で終わる名前を持つコマンドのエイリアスを取得します。

このコマンドは、 *scope* パラメーターを使用して、グローバルスコープでコマンドを適用します。
これは、セッション内のエイリアスを取得するときに、スクリプトで役立ちます。

## PARAMETERS

### -Definition

指定した項目のエイリアスを取得します。
コマンドレット、関数、スクリプト、ファイル、または実行可能ファイルの名前を入力します。

このパラメーターは、エイリアスオブジェクトの Definition プロパティで項目名を検索するため、 *定義* と呼ばれます。

```yaml
Type: System.String[]
Parameter Sets: Definition
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -除外

指定した項目を除外します。
このパラメーターの値は、Name パラメーターと Definition パラメーターを修飾します。
名前、要素、または「s*」のようなパターンを入力します。
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

このコマンドレットが取得するエイリアスを指定します。
ワイルドカードを使用できます。
既定では、は、 `Get-Alias` 現在のセッションに対して定義されているすべてのエイリアスを取得します。
パラメーター **名は省略可能です。**
パイプを使用してエイリアス名をにパイプすることもでき `Get-Alias` ます。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases:

Required: False
Position: 0
Default value: All aliases
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -スコープ

このコマンドレットがエイリアスを取得するスコープを指定します。
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
Default value: Local
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用してエイリアス名を **取得** することができます。

## 出力

### システム管理. エイリアス情報

**Get エイリアス** は、各エイリアスを表すオブジェクトを返します。
**Get エイリアス** はすべてのエイリアスに対して同じオブジェクトを返しますが、PowerShell は矢印ベースの形式を使用して、ハイフンではないエイリアスの名前を表示します。

## 注

- 新しいエイリアスを作成するには、Set-Alias または New-Alias を使用します。 エイリアスを削除するには、Remove-Item を使用します。
- 矢印に基づくエイリアス名の形式は、ハイフンを含むエイリアスには使用されません。 これらは、一般的な省略形またはニックネームの代わりに、コマンドレットと関数の推奨される代替名となる可能性が高くなります。

## 関連リンク

[Export-Alias](Export-Alias.md)

[Import-Alias](Import-Alias.md)

[New-Alias](New-Alias.md)

[Set-Alias](Set-Alias.md)

[エイリアス プロバイダー](../Microsoft.PowerShell.Core/About/about_Alias_Provider.md)

[about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)

