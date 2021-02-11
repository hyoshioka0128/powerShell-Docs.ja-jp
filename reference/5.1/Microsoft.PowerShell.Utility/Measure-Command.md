---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 12/09/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/measure-command?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Measure-Command
ms.openlocfilehash: 4fb4553a75bf6cd5e458cc7d87c37e9a13ce0ea2
ms.sourcegitcommit: 165d10405d9db3a68c417a239d3181378fd02b9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "96935937"
---
# Measure-Command

## 概要
スクリプト ブロックとコマンドレットの実行に要する時間を計測します。

## SYNTAX

```
Measure-Command [-InputObject <PSObject>] [-Expression] <ScriptBlock> [<CommonParameters>]
```

## Description

コマンドレットでは、 `Measure-Command` スクリプトブロックまたはコマンドレットを内部で実行し、操作の実行時間を指定して、実行時間を返します。

> [!NOTE]
> スクリプトブロックは、 `Measure-Command` 子スコープではなく、現在のスコープで実行されます。

## 例

### 例 1: コマンドを測定する

この例では、 `Get-EventLog` Windows PowerShell イベントログ内のイベントを取得するコマンドの実行にかかる時間を計測します。

```powershell
Measure-Command { Get-EventLog "windows powershell" }
```

### 例 2: Measure-Command の2つの出力を比較する

最初のコマンドは、 `Get-ChildItem` **Path** パラメーターを使用して `.txt` `C:\Windows` ディレクトリとそのサブディレクトリ内のファイルのみを取得する再帰コマンドを処理するためにかかる時間を計測します。

2番目のコマンドは、プロバイダー固有の ' パラメーターを使用する再帰コマンドの処理にかかる時間を測定し `Get-ChildItem` ます。

これらのコマンドは、PowerShell コマンドでプロバイダー固有のフィルターを使用した値を表示します。

```powershell
Measure-Command { Get-ChildItem -Path C:\Windows\*.txt -Recurse }
```

```Output
Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 8
Milliseconds      : 618
Ticks             : 86182763
TotalDays         : 9.9748568287037E-05
TotalHours        : 0.00239396563888889
TotalMinutes      : 0.143637938333333
TotalSeconds      : 8.6182763
TotalMilliseconds : 8618.2763
```

```powershell
Measure-Command {Get-ChildItem C:\Windows -Filter "*.txt" -Recurse}
```

```Output
Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 1
Milliseconds      : 140
Ticks             : 11409189
TotalDays         : 1.32050798611111E-05
TotalHours        : 0.000316921916666667
TotalMinutes      : 0.019015315
TotalSeconds      : 1.1409189
TotalMilliseconds : 1140.9189
```

### 例 3: 入力を Measure-Command にパイプする

パイプ処理されたオブジェクト `Measure-Command` は、 **Expression** パラメーターに渡されるスクリプトブロックで使用できます。 スクリプトブロックは、パイプラインのオブジェクトごとに1回実行されます。

```powershell
# Perform a simple operation to demonstrate the InputObject parameter
# Note that no output is displayed.
10, 20, 50 | Measure-Command -Expression { for ($i=0; $i -lt $_; $i++) {$i} }
```

```Output
Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 0
Milliseconds      : 12
Ticks             : 122672
TotalDays         : 1.41981481481481E-07
TotalHours        : 3.40755555555556E-06
TotalMinutes      : 0.000204453333333333
TotalSeconds      : 0.0122672
TotalMilliseconds : 12.2672
```

### 例 4: 測定されたコマンドの出力を表示する

に式の出力を表示するに `Measure-Command` は、パイプを使用し `Out-Default` ます。

```powershell
# Perform the same operation as above adding Out-Default to every execution.
# This will show that the ScriptBlock is in fact executing for every item.
10, 20, 50 | Measure-Command -Expression {for ($i=0; $i -lt $_; $i++) {$i}; "$($_)" | Out-Default }
```

```Output
10
20
50


Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 0
Milliseconds      : 11
Ticks             : 113745
TotalDays         : 1.31649305555556E-07
TotalHours        : 3.15958333333333E-06
TotalMinutes      : 0.000189575
TotalSeconds      : 0.0113745
TotalMilliseconds : 11.3745
```

### 例 5: 子スコープで実行を測定する

`Measure-Command` 現在のスコープでスクリプトブロックを実行します。これにより、スクリプトブロックは現在のスコープ内の値を変更できます。 現在のスコープが変更されないようにするには、スクリプトブロックを中かっこ () で囲み、 `{}` 呼び出し演算子 () を使用して `&` 子スコープでブロックを実行する必要があります。

```powershell
$foo = 'Value 1'
$null = Measure-Command { $foo = 'Value 2' }
$foo
$null = Measure-Command { & { $foo = 'Value 3' } }
$foo
```

```Output
Value 2
Value 2
```

呼び出し演算子の詳細については、「 [about_Operators](../Microsoft.PowerShell.Core/About/about_Operators.md#call-operator-)」を参照してください。

## PARAMETERS

### -式

時間の計測する式を指定します。 式を中かっこ () で囲み `{}` ます。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

**InputObject** パラメーターにバインドされたオブジェクトは、 **Expression** パラメーターに渡されるスクリプトブロックに対する省略可能な入力です。 スクリプトブロック内では、を使用して、 `$_` パイプライン内の現在のオブジェクトを参照できます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用してオブジェクトをにパイプすることができ `Measure-Command` ます。

## 出力

### System.TimeSpan

`Measure-Command` 結果を表す期間オブジェクトを返します。

## 注

## 関連リンク

[Invoke-Command](../Microsoft.PowerShell.Core/Invoke-Command.md)

[Trace-Command](Trace-Command.md)
