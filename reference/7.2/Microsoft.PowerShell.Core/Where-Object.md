---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/19/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Where-Object
ms.openlocfilehash: 667a503d67ac9a9a0f41187cbac079d946247618
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600578"
---
# Where-Object

## 概要
プロパティの値に基づいて、コレクションからオブジェクトを選択します。

## SYNTAX

### EqualSet (既定値)

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] [-EQ]
 [<CommonParameters>]
```

### ScriptBlockSet

```
Where-Object [-InputObject <PSObject>] [-FilterScript] <ScriptBlock> [<CommonParameters>]
```

### MatchSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -Match
 [<CommonParameters>]
```

### CaseSensitiveEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CEQ
 [<CommonParameters>]
```

### NotEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -NE
 [<CommonParameters>]
```

### CaseSensitiveNotEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CNE
 [<CommonParameters>]
```

### GreaterThanSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -GT
 [<CommonParameters>]
```

### CaseSensitiveGreaterThanSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CGT
 [<CommonParameters>]
```

### 設定しない

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -LT
 [<CommonParameters>]
```

### CaseSensitiveLessThanSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CLT
 [<CommonParameters>]
```

### GreaterOrEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -GE
 [<CommonParameters>]
```

### CaseSensitiveGreaterOrEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CGE
 [<CommonParameters>]
```

### LessOrEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -LE
 [<CommonParameters>]
```

### CaseSensitiveLessOrEqualSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CLE
 [<CommonParameters>]
```

### 数字セット

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -Like
 [<CommonParameters>]
```

### CaseSensitiveLikeSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CLike
 [<CommonParameters>]
```

### Not記法セット

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -NotLike
 [<CommonParameters>]
```

### CaseSensitiveNotLikeSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CNotLike
 [<CommonParameters>]
```

### CaseSensitiveMatchSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CMatch
 [<CommonParameters>]
```

### NotMatchSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -NotMatch
 [<CommonParameters>]
```

### CaseSensitiveNotMatchSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CNotMatch
 [<CommonParameters>]
```

### 集合集合

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -Contains
 [<CommonParameters>]
```

### CaseSensitiveContainsSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CContains
 [<CommonParameters>]
```

### Not集合セット

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -NotContains
 [<CommonParameters>]
```

### CaseSensitiveNotContainsSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CNotContains
 [<CommonParameters>]
```

### ペン

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -In
 [<CommonParameters>]
```

### CaseSensitiveInSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CIn
 [<CommonParameters>]
```

### NotInSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -NotIn
 [<CommonParameters>]
```

### CaseSensitiveNotInSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -CNotIn
 [<CommonParameters>]
```

### IsSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -Is
 [<CommonParameters>]
```

### IsNotSet

```
Where-Object [-InputObject <PSObject>] [-Property] <String> [[-Value] <Object>] -IsNot
 [<CommonParameters>]
```

### Not

```
Where-Object [-InputObject <PSObject>] [-Property] <String> -Not [<CommonParameters>]
```

## Description

コマンドレットでは、 `Where-Object` 渡されるオブジェクトのコレクションから、特定のプロパティ値を持つオブジェクトを選択します。 たとえば、コマンドレットを使用して、特定の `Where-Object` 日付以降に作成されたファイル、特定の ID を持つイベント、または特定のバージョンの Windows を使用するコンピューターを選択できます。

Windows PowerShell 3.0 以降では、コマンドを構築する方法は2つあり `Where-Object` ます。

- **スクリプトブロック**。 スクリプト ブロックを使用すると、プロパティ名、比較演算子、プロパティ値を指定できます。 `Where-Object` スクリプトブロックステートメントが true であるすべてのオブジェクトを返します。

  たとえば、次のコマンドは、通常の優先度クラスのプロセスを取得します。つまり、"優先度" **クラス** プロパティの値が normal であるプロセスを取得します。

  `Get-Process | Where-Object {$_.PriorityClass -eq "Normal"}`

  すべての PowerShell 比較演算子は、スクリプトブロック形式で有効です。 比較演算子の詳細については、「 [about_Comparison_Operators](./About/about_Comparison_Operators.md)」を参照してください。

- **比較ステートメント**。 比較ステートメントを使用すると、自然言語のように記述することができます。 比較ステートメントは Windows PowerShell 3.0 で導入されました。

  たとえば、次のコマンドは、priority クラスが Normal のプロセスも取得します。 これらのコマンドは同等であり、置き換えて使用することができます。

  `Get-Process | Where-Object -Property PriorityClass -eq -Value "Normal"`

  `Get-Process | Where-Object PriorityClass -eq "Normal"`

  Windows PowerShell 3.0 以降では、 `Where-Object` コマンドのパラメーターとして比較演算子が追加さ `Where-Object` れています。 すべての演算子は、特に指定しない限り、大文字と小文字を区別しません。 Windows PowerShell 3.0 より前では、PowerShell 言語の比較演算子は、スクリプトブロックでのみ使用できました。

に1つの **プロパティ** を指定すると `Where-Object` 、プロパティの値はブール式として扱われます。 **Length** の値が0でない場合、式は **True** に評価されます。 例: `('hi', '', 'there') | Where-Object Length`

前の例は、機能的には次のように相当します。

- `('hi', '', 'there') | Where-Object Length -GT 0`
- `('hi', '', 'there') | Where-Object {$_.Length -gt 0}`

## 例

### 例 1: 停止したサービスを取得する

これらのコマンドは、現在停止しているすべてのサービスの一覧を取得します。 `$_`自動変数は、コマンドレットに渡される各オブジェクトを表し `Where-Object` ます。

最初のコマンドは、スクリプトブロック形式を使用します。2番目のコマンドは、比較ステートメント形式を使用します。 コマンドは等価であり、同じ意味で使用することができます。

```powershell
Get-Service | Where-Object {$_.Status -eq "Stopped"}
Get-Service | where Status -eq "Stopped"
```

### 例 2: ワーキングセットに基づいてプロセスを取得する

これらのコマンドは、ワーキングセットが250メガバイト (KB) を超えるプロセスを一覧表示します。 Scriptblock とステートメントの構文は同等であり、同じように使用できます。

```powershell
Get-Process | Where-Object {$_.WorkingSet -GT 250MB}
Get-Process | Where-Object WorkingSet -GT (250MB)
```

### 例 3: プロセス名に基づいてプロセスを取得する

これらのコマンドは、文字で始まる **ProcessName** プロパティ値を持つプロセスを取得し `p` ます。 **Match** 演算子を使用すると、正規表現の一致を使用できます。

Scriptblock とステートメントの構文は同等であり、同じように使用できます。

```powershell
Get-Process | Where-Object {$_.ProcessName -Match "^p.*"}
Get-Process | Where-Object ProcessName -Match "^p.*"
```

### 例 4: 比較ステートメントの形式を使用する

この例では、コマンドレットの新しい比較ステートメント形式を使用する方法を示し `Where-Object` ます。

最初のコマンドは、比較ステートメント形式を使用しています。
このコマンドでは、エイリアスは使用されず、すべてのパラメーターにパラメーター名が使用されています。

2 番目のコマンドは、比較コマンドの形式をより自然に使用しています。 `where`エイリアスはコマンドレット名の代わりに使用され、 `Where-Object` すべての省略可能なパラメーター名は省略されます。

```powershell
Get-Process | Where-Object -Property Handles -GE -Value 1000
Get-Process | where Handles -GE 1000
```

### 例 5: プロパティに基づいてコマンドを取得する

この例では、true または false の項目、または指定したプロパティの任意の値を持つ項目を返すコマンドを記述する方法を示します。 各例では、コマンドのスクリプトブロック形式と比較ステートメント形式の両方を示しています。

```powershell
# Use Where-Object to get commands that have any value for the OutputType property of the command.
# This omits commands that do not have an OutputType property and those that have an OutputType property, but no property value.
Get-Command | where OutputType
Get-Command | where {$_.OutputType}
```

```powershell
# Use Where-Object to get objects that are containers.
# This gets objects that have the **PSIsContainer** property with a value of $True and excludes all others.
Get-ChildItem | where PSIsContainer
Get-ChildItem | where {$_.PSIsContainer}
```

```powershell
# Finally, use the Not operator (!) to get objects that are not containers.
# This gets objects that do have the **PSIsContainer** property and those that have a value of $False for the **PSIsContainer** property.
Get-ChildItem | where {!$_.PSIsContainer}
# You cannot use the Not operator (!) in the comparison statement format of the command.
Get-ChildItem | where PSIsContainer -eq $False
```

### 例 6: 複数の条件を使用する

```powershell
Get-Module -ListAvailable | where {($_.Name -notlike "Microsoft*" -and $_.Name -notlike "PS*") -and $_.HelpInfoUri}
```

この例では、 `Where-Object` 複数の条件を持つコマンドを作成する方法を示します。

このコマンドは、更新可能なヘルプ機能をサポートしている非コア モジュールを取得します。 このコマンドは、コマンドレットの **ListAvailable** パラメーターを使用して、 `Get-Module` コンピューター上のすべてのモジュールを取得します。 パイプライン演算子 () は、 `|` モジュールを `Where-Object` コマンドレットに送信します。このコマンドレットは、名前が Microsoft または PS で始まらないモジュールを取得し、 **Helpinfouri** プロパティの値を設定します。これにより、モジュールの更新されたヘルプファイルの検索場所を PowerShell に指示します。 比較ステートメントは、 **and** 論理演算子によって結合されます。

この例は、スクリプト ブロックのコマンド形式を使用しています。 論理演算子 ( **and** や **Or** など) は、スクリプトブロックでのみ有効です。 コマンドの比較ステートメント形式で使用することはできません `Where-Object` 。

- PowerShell の論理演算子の詳細については、「 [about_Logical_Operators](./About/about_logical_operators.md)」を参照してください。
- 更新可能なヘルプ機能の詳細については、「 [about_Updatable_Help](./About/about_Updatable_Help.md)」を参照してください。

## PARAMETERS

### -CContains

オブジェクトのプロパティ値が指定した値と完全に一致する場合に、このコマンドレットがコレクションからオブジェクトを取得することを示します。 この操作では、大文字と小文字が区別されます。

例: `Get-Process | where ProcessName -CContains "svchost"`

**Ccontains** は値のコレクションを参照し、指定された値と完全に一致する項目がコレクションに格納されている場合は true になります。 入力が単一のオブジェクトの場合、PowerShell はそれを1つのオブジェクトのコレクションに変換します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveContainsSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CEQ

プロパティ値が指定した値と同じ場合に、このコマンドレットによってオブジェクトが取得されることを示します。
この操作では、大文字と小文字が区別されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveEqualSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CGE

プロパティ値が指定した値以上の場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveGreaterOrEqualSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CGT

プロパティ値が指定した値よりも大きい場合に、このコマンドレットによってオブジェクトが取得されることを示します。
この操作では、大文字と小文字が区別されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveGreaterThanSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CIn

プロパティ値に指定した値が含まれている場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

例: `Get-Process | where -Value "svchost" -CIn ProcessName`

**CIn** は **ccontains** に似ていますが、プロパティと値の位置が逆になる点が異なります。 たとえば、次のステートメントはどちらも true です。

`"abc", "def" -CContains "abc"`

`"abc" -CIn "abc", "def"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveInSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CLE

プロパティ値が指定した値以下の場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveLessOrEqualSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CLike

プロパティ値がワイルドカード文字を含む値と一致する場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

例: `Get-Process | where ProcessName -CLike "*host"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveLikeSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CLT

プロパティ値が指定した値より小さい場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveLessThanSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CMatch

プロパティ値が指定した正規表現に一致する場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。 入力がスカラーの場合は、一致した値が自動変数に保存され `$Matches` ます。

例: `Get-Process | where ProcessName -CMatch "Shell"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveMatchSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CNE

プロパティ値が指定した値と異なる場合に、このコマンドレットによってオブジェクトが取得されることを示します。
この操作では、大文字と小文字が区別されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveNotEqualSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CNotContains

オブジェクトのプロパティ値が指定した値と完全に一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

例: `Get-Process | where ProcessName -CNotContains "svchost"`

**Notcontains** と **cnotcontains** は値のコレクションを参照し、指定された値と完全に一致する項目がコレクションに含まれていない場合に true になります。 入力が単一のオブジェクトの場合、PowerShell はそれを1つのオブジェクトのコレクションに変換します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveNotContainsSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CNotIn

プロパティ値が指定した値と完全に一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

例: `Get-Process | where -Value "svchost" -CNotIn -Property ProcessName`

**Notin** および **cnotin** 演算子は、プロパティと値の位置が逆になる点を除い **て、Notin** および **cnotin** と似ています。 たとえば、次のステートメントは true です。

`"abc", "def" -CNotContains "Abc"`

`"abc" -CNotIn "Abc", "def"`

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveNotInSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CNotLike

プロパティ値がワイルドカード文字を含む値と一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。

例: `Get-Process | where ProcessName -CNotLike "*host"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveNotLikeSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CNotMatch

プロパティ値が指定した正規表現と一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。 この操作では、大文字と小文字が区別されます。 入力がスカラーの場合は、一致した値が自動変数に保存され `$Matches` ます。

例: `Get-Process | where ProcessName -CNotMatch "Shell"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CaseSensitiveNotMatchSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Contains

オブジェクトのプロパティ値の項目が指定した値と完全に一致する場合に、このコマンドレットによってオブジェクトが取得されることを示します。

例: `Get-Process | where ProcessName -Contains "Svchost"`

プロパティ値に1つのオブジェクトが含まれている場合、PowerShell はそれを1つのオブジェクトのコレクションに変換します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ContainsSet
Aliases: IContains

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EQ

プロパティ値が指定した値と同じ場合に、このコマンドレットによってオブジェクトが取得されることを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: EqualSet
Aliases: IEQ

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilterScript

オブジェクトをフィルター処理するために使用するスクリプト ブロックを指定します。 スクリプトブロックを中かっこ () で囲み `{}` ます。

パラメーター名 **Filterscript** は省略可能です。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ScriptBlockSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -GE

プロパティ値が指定した値以上の場合に、このコマンドレットによってオブジェクトが取得されることを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: GreaterOrEqualSet
Aliases: IGE

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -GT

プロパティ値が指定した値よりも大きい場合に、このコマンドレットによってオブジェクトが取得されることを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: GreaterThanSet
Aliases: IGT

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -In

プロパティ値が指定した値と一致する場合に、このコマンドレットによってオブジェクトが取得されることを示します。
次に例を示します。

`Get-Process | where -Property ProcessName -in -Value "Svchost", "TaskHost", "WsmProvHost"`

**Value** パラメーターの値が単一のオブジェクトの場合、PowerShell はそれを1つのオブジェクトのコレクションに変換します。

オブジェクトのプロパティ値が配列の場合、PowerShell は参照の等価性を使用して一致を判定します。 `Where-Object`**Property** パラメーターの値と **値** の値がオブジェクトの同じインスタンスである場合にのみ、オブジェクトを返します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: InSet
Aliases: IIn

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

フィルター処理されるオブジェクトを指定します。 パイプを使用してオブジェクトをにパイプすることもでき `Where-Object` ます。

で **inputobject** パラメーターを使用すると、 `Where-Object` コマンドの結果をにパイプするのではなく、 `Where-Object` **inputobject** 値が1つのオブジェクトとして扱われます。 これは、値が、などのコマンドの結果であるコレクションである場合にも当てはまり `-InputObject (Get-Process)` ます。 **InputObject** は配列またはオブジェクトのコレクションから個々のプロパティを返すことができないため、を使用し `Where-Object` て、定義されたプロパティに特定の値を持つオブジェクトのコレクションをフィルター処理する場合は、 `Where-Object` このトピックの例に示すように、パイプラインでを使用することをお勧めします。

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

### -が

プロパティ値が指定された .NET 型のインスタンスの場合に、このコマンドレットがオブジェクトを取得することを示します。 型名を角かっこで囲みます。

たとえば、`Get-Process | where StartTime -Is [DateTime]`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: IsSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IsNot

プロパティ値が指定した .NET 型のインスタンスでない場合に、このコマンドレットによってオブジェクトが取得されることを示します。

たとえば、`Get-Process | where StartTime -IsNot [DateTime]`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: IsNotSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LE

プロパティ値が指定した値以下の場合に、このコマンドレットによってオブジェクトが取得されることを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: LessOrEqualSet
Aliases: ILE

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Like

プロパティ値がワイルドカード文字を含む値と一致する場合に、このコマンドレットによってオブジェクトが取得されることを示します。

例: `Get-Process | where ProcessName -Like "*host"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: LikeSet
Aliases: ILike

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LT

プロパティ値が指定した値よりも小さい場合に、このコマンドレットによってオブジェクトが取得されることを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: LessThanSet
Aliases: ILT

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -一致

プロパティ値が指定した正規表現に一致する場合に、このコマンドレットによってオブジェクトが取得されることを示します。 入力がスカラーの場合は、一致した値が自動変数に保存され `$Matches` ます。

例: `Get-Process | where ProcessName -Match "shell"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: MatchSet
Aliases: IMatch

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NE

プロパティ値が指定した値と異なる場合に、このコマンドレットによってオブジェクトが取得されることを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NotEqualSet
Aliases: INE

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Not

プロパティが存在しない場合、または値が null または false の場合に、このコマンドレットによってオブジェクトが取得されることを示します。

例: `Get-Service | where -Not "DependentServices"`

このパラメーターは、Windows PowerShell 6.1 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Not
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotContains

プロパティ値の項目が指定した値と完全に一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。

例: `Get-Process | where ProcessName -NotContains "Svchost"`

**Notcontains** は値のコレクションを参照し、指定された値と完全に一致する項目がコレクションに含まれていない場合は true になります。 入力が単一のオブジェクトの場合、PowerShell はそれを1つのオブジェクトのコレクションに変換します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NotContainsSet
Aliases: INotContains

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotIn

プロパティ値が指定した値と完全に一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。

例: `Get-Process | where -Value "svchost" -NotIn -Property ProcessName`

**Value** の値が単一のオブジェクトの場合、PowerShell はそれを1つのオブジェクトのコレクションに変換します。

オブジェクトのプロパティ値が配列の場合、PowerShell は参照の等価性を使用して一致を判定します。 `Where-Object`**プロパティ** の値と **値** の値がオブジェクトの同じインスタンスでない場合にのみ、オブジェクトを返します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NotInSet
Aliases: INotIn

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotLike

プロパティ値がワイルドカード文字を含む値と一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。

例: `Get-Process | where ProcessName -NotLike "*host"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NotLikeSet
Aliases: INotLike

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotMatch

プロパティ値が指定した正規表現と一致しない場合に、このコマンドレットによってオブジェクトが取得されることを示します。 入力がスカラーの場合は、一致した値が自動変数に保存され `$Matches` ます。

例: `Get-Process | where ProcessName -NotMatch "PowerShell"`

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NotMatchSet
Aliases: INotMatch

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -プロパティ

オブジェクトのプロパティの名前を指定します。 パラメーター名 ( **Property**) は省略可能です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: EqualSet, LessOrEqualSet, CaseSensitiveEqualSet, NotEqualSet, CaseSensitiveNotEqualSet, GreaterThanSet, CaseSensitiveGreaterThanSet, LessThanSet, CaseSensitiveLessThanSet, GreaterOrEqualSet, CaseSensitiveGreaterOrEqualSet, CaseSensitiveLessOrEqualSet, LikeSet, CaseSensitiveLikeSet, NotLikeSet, CaseSensitiveNotLikeSet, MatchSet, CaseSensitiveMatchSet, NotMatchSet, CaseSensitiveNotMatchSet, ContainsSet, CaseSensitiveContainsSet, NotContainsSet, CaseSensitiveNotContainsSet, InSet, CaseSensitiveInSet, NotInSet, CaseSensitiveNotInSet, IsSet, IsNotSet, Not
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Value

プロパティ値を指定します。 パラメーター名 ( **Value**) は省略可能です。 このパラメーターでは、次の比較パラメーターと共にワイルドカード文字を使用できます。

- **CLike**
- **CNotLike**
- **Like**
- **NotLike**

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Object
Parameter Sets: EqualSet, LessOrEqualSet, CaseSensitiveEqualSet, NotEqualSet, CaseSensitiveNotEqualSet, GreaterThanSet, CaseSensitiveGreaterThanSet, LessThanSet, CaseSensitiveLessThanSet, GreaterOrEqualSet, CaseSensitiveGreaterOrEqualSet, CaseSensitiveLessOrEqualSet, LikeSet, CaseSensitiveLikeSet, NotLikeSet, CaseSensitiveNotLikeSet, MatchSet, CaseSensitiveMatchSet, NotMatchSet, CaseSensitiveNotMatchSet, ContainsSet, CaseSensitiveContainsSet, NotContainsSet, CaseSensitiveNotContainsSet, InSet, CaseSensitiveInSet, NotInSet, CaseSensitiveNotInSet, IsSet, IsNotSet
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用してオブジェクトをこのコマンドレットに渡します。

## 出力

### Object

このコマンドレットは、入力オブジェクトセットから選択された項目を返します。

## 注

Windows PowerShell 4.0 以降では `Where` 、 `ForEach` コレクションで使用するためのメソッドとメソッドが追加されました。

これらの新しいメソッドの詳細については、こちらを参照してください [about_arrays](./About/about_Arrays.md)

## 関連リンク

[Compare-Object](../Microsoft.PowerShell.Utility/Compare-Object.md)

[ForEach-Object](ForEach-Object.md)

[Group-Object](../Microsoft.PowerShell.Utility/Group-Object.md)

[Measure-Object](../Microsoft.PowerShell.Utility/Measure-Object.md)

[New-Object](../Microsoft.PowerShell.Utility/New-Object.md)

[Select-Object](../Microsoft.PowerShell.Utility/Select-Object.md)

[Sort-Object](../Microsoft.PowerShell.Utility/Sort-Object.md)

[Tee-Object](../Microsoft.PowerShell.Utility/Tee-Object.md)

