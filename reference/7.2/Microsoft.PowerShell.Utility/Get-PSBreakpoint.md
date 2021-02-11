---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 05/15/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-psbreakpoint?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSBreakpoint
ms.openlocfilehash: 78691b806fe68aaa8d9e502d28e6f3967867f95b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600980"
---
# Get-PSBreakpoint

## 概要
現在のセッションで設定されたブレークポイントを取得します。

## SYNTAX

### スクリプト (既定値)

```
Get-PSBreakpoint [-Script <String[]>] [<CommonParameters>]
```

### 変数

```
Get-PSBreakpoint [-Script <String[]>] -Variable <String[]> [<CommonParameters>]
```

### コマンド

```
Get-PSBreakpoint [-Script <String[]>] -Command <String[]> [<CommonParameters>]
```

### 型

```
Get-PSBreakpoint [-Script <String[]>] [-Type] <BreakpointType[]> [<CommonParameters>]
```

### Id

```
Get-PSBreakpoint [-Id] <Int32[]> [<CommonParameters>]
```

## Description

**Get PSBreakPoint ポイント** コマンドレットは、現在のセッションで設定されているブレークポイントを取得します。
コマンドレットのパラメーターを使用して、特定のブレークポイントを取得できます。

ブレークポイントとは、実行を一時的に停止して命令を調べることができるようにするための、コマンドまたはスクリプト内のポイントです。
**Get-PSBreakpoint ポイント** は、PowerShell スクリプトとコマンドのデバッグ用に設計されたいくつかのコマンドレットの1つです。 PowerShell デバッガーの詳細については、「about_Debuggers」を参照してください。

## 例

### 例 1: すべてのスクリプトおよび関数のすべてのブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint
```

このコマンドは、現在のセッション内のすべてのスクリプトおよび関数に設定されているすべてのブレークポイントを取得します。

### 例 2: ID によってブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint -Id 2
Function   :
IncrementAction     :
Enabled    :
TrueHitCount   : 0
Id         : 2
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1
```

このコマンドは、ブレークポイント ID が 2 のブレークポイントを取得します。

### 例 3: パイプを使用して ID を Get-PSBreakpoint にする

```
PS C:\> $B = Set-PSBreakpoint -Script "sample.ps1" -Command "Increment"
PS C:\> $B.Id | Get-PSBreakpoint
```

これらのコマンドは、ブレークポイント ID をパイプを使ってブレークポイントを取得する方法を示し **ています**。

最初のコマンドは、Set-PSBreakpoint コマンドレットを使用して、Sample.ps1 スクリプトの Increment 関数にブレークポイントを作成します。 ブレークポイントオブジェクトを $B 変数に保存します。

2番目のコマンドは、ドット演算子 (.) を使用して、$B 変数内のブレークポイントオブジェクトの Id プロパティを取得します。 パイプライン演算子 (|) を使用して、ID を **Get PSBreakpoint ポイント** コマンドレットに送信します。

結果として **、指定** した ID のブレークポイントが取得されます。

### 例 4: 指定したスクリプトファイル内のブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint -Script "Sample.ps1, SupportScript.ps1"
```

このコマンドは、Sample.ps1 ファイルと SupportScript.ps1 ファイル内のすべてのブレークポイントを取得します。

このコマンドは、他のスクリプトまたはセッションの関数で設定されている可能性のある他のブレークポイントを取得しません。

### 例 5: 指定したコマンドレットのブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint -Command "Read-Host, Write-Host" -Script "Sample.ps1"
```

このコマンドは、Sample.ps1 ファイル内の Read-Host コマンドまたは Write-Host コマンドに設定されているすべてのコマンド ブレークポイントを取得します。

### 例 6: 指定したファイル内のコマンドのブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint -Type Command -Script "Sample.ps1"
```

このコマンドは、Sample.ps1 ファイル内のすべてのコマンド ブレークポイントを取得します。

### 例 7: 変数によってブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint -Variable "Index, Swap"
```

このコマンドは、現在のセッションの $Index と $Swap 変数に設定されているブレークポイントを取得します。

### 例 8: ファイル内のすべての行と変数のブレークポイントを取得する

```
PS C:\> Get-PSBreakpoint -Type Line, Variable -Script "Sample.ps1"
```

このコマンドは、Sample.ps1 スクリプト内のすべての行ブレークポイントと変数ブレークポイントを取得します。

## PARAMETERS

### -Command

指定したコマンド名に設定されているコマンドのブレークポイントの配列を指定します。
コマンドレットや関数の名前などのコマンド名を入力します。

```yaml
Type: System.String[]
Parameter Sets: Command
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

このコマンドレットが取得するブレークポイント Id を指定します。
ID をコンマ区切りリスト形式で入力します。
パイプを使用してブレークポイント Id を **取得** することもできます。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -スクリプト

ブレークポイントを含むスクリプトの配列を指定します。
1つまたは複数のスクリプトファイルのパス (省略可能) と名前を入力します。
パスを省略した場合、現在のディレクトリが既定の場所になります。

```yaml
Type: System.String[]
Parameter Sets: Script, Variable, Command, Type
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Type

このコマンドレットが取得するブレークポイントの型の配列を指定します。
1 つまたは複数の型を入力します。
このパラメーターの有効値は、次のとおりです。

- 行
- コマンド
- 変数

パイプを使用してブレークポイント **の** 種類を取得することもできます。

```yaml
Type: Microsoft.PowerShell.Commands.BreakpointType[]
Parameter Sets: Type
Aliases:
Accepted values: Line, Variable, Command

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Variable

指定した変数名に設定されている変数のブレークポイントの配列を指定します。
ドル記号を付けずに変数名を入力します。

```yaml
Type: System.String[]
Parameter Sets: Variable
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「about_CommonParameters」 (https://go.microsoft.com/fwlink/?LinkID=113216) ) を参照してください。

## 入力

### System.string、Microsoft. PowerShell. BreakpointType

パイプを使用して、ブレークポイントの Id とブレークポイントの種類を **取得** することができます。

## 出力

### システム.... ブレークポイント

**Get-PSBreakPoint ポイント** は、セッション内のブレークポイントを表すオブジェクトを返します。

## 注

* 使用することができます、 **取得-PSBreakpoint ポイント** またはそのエイリアス "gbp" です。

## 関連リンク

[Disable-PSBreakpoint](Disable-PSBreakpoint.md)

[Enable-PSBreakpoint](Enable-PSBreakpoint.md)

[Get-PSCallStack](Get-PSCallStack.md)

[Remove-PSBreakpoint](Remove-PSBreakpoint.md)

[Set-PSBreakpoint](Set-PSBreakpoint.md)

