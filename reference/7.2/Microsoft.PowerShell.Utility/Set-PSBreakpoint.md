---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/24/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/set-psbreakpoint?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-PSBreakpoint
ms.openlocfilehash: 5e2bdf435bf7cb9da3f31712c346beff29a11baf
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600411"
---
# Set-PSBreakpoint

## 概要
行、コマンド、または変数にブレークポイントを設定します。

## SYNTAX

### Line (既定値)

```
Set-PSBreakpoint [-Action <ScriptBlock>] [[-Column] <Int32>] [-Line] <Int32[]> [-Script] <String[]>
 [<CommonParameters>]
```

### コマンド

```
Set-PSBreakpoint [-Action <ScriptBlock>] -Command <String[]> [[-Script] <String[]>] [<CommonParameters>]
```

### 変数

```
Set-PSBreakpoint [-Action <ScriptBlock>] [[-Script] <String[]>] -Variable <String[]>
 [-Mode <VariableAccessMode>] [<CommonParameters>]
```

## Description

`Set-PSBreakpoint`コマンドレットは、現在のセッションで実行されているスクリプトまたは任意のコマンドでブレークポイントを設定します。 を使用すると、 `Set-PSBreakpoint` 別のブレークポイントで停止したときに、スクリプトを実行したり、コマンドを実行したり、デバッグ中にブレークポイントを設定したりすることができます。

`Set-PSBreakpoint` リモートコンピューターにブレークポイントを設定することはできません。 リモート コンピューターでスクリプトをデバッグするには、スクリプトをローカル コンピューターにコピーしてからローカルでデバッグします。

各 `Set-PSBreakpoint` コマンドは、次の3種類のブレークポイントのいずれかを作成します。

- 行のブレークポイント-特定の行と列の座標にブレークポイントを設定します。
- コマンドのブレークポイント-コマンドおよび関数にブレークポイントを設定します。
- 変数のブレークポイント-変数にブレークポイントを設定します。

1つのコマンドで複数の行、コマンド、または変数にブレークポイントを設定でき `Set-PSBreakpoint` ますが、各 `Set-PSBreakpoint` コマンドで設定できるブレークポイントの種類は1つだけです。

ブレークポイントでは、PowerShell は一時的に実行を停止し、デバッガーに制御を与えます。 コマンドプロンプトがに変わり、 `DBG\>` 一連のデバッガーコマンドを使用できるようになります。 ただし、 **Action** パラメーターを使用して、ブレークポイントの条件、ログ記録や診断などの追加のタスクを実行する命令などの代替応答を指定できます。

`Set-PSBreakpoint`コマンドレットは、PowerShell スクリプトのデバッグ用に設計されたいくつかのコマンドレットの1つです。
PowerShell デバッガーの詳細については、「 [about_Debuggers](../Microsoft.PowerShell.Core/About/about_Debuggers.md)」を参照してください。

## 例

### 例 1: 行にブレークポイントを設定する

この例では、Sample.ps1 スクリプトの5行目にブレークポイントを設定します。 スクリプトを実行すると、5行目が実行される直前に実行が停止します。

```powershell
Set-PSBreakpoint -Script "sample.ps1" -Line 5
```

```output
Column     : 0
Line       : 5
Action     :
Enabled    : True
HitCount   : 0
Id         : 0
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1
```

新しいブレークポイントを行番号で設定すると、コマンドレットによって、 `Set-PSBreakpoint` ブレークポイント ID とヒットカウントを含む行ブレークポイントオブジェクト (system.string) が生成さ **れます**。

### 例 2: 関数にブレークポイントを設定する

この例では、Sample.ps1 コマンドレットの関数にコマンドのブレークポイントを作成し `Increment` ます。 スクリプトは、指定した関数を呼び出すたびに、直前に実行を停止します。

```powershell
Set-PSBreakpoint -Command "Increment" -Script "sample.ps1"
```

```Output
Command    : Increment
Action     :
Enabled    : True
HitCount   : 0
Id         : 1
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1
```

結果として、コマンドのブレークポイント オブジェクトが作成されます。 スクリプトを実行する前に、 **ヒットカウント** プロパティの値は0です。

### 例 3: 変数にブレークポイントを設定する

この例では、Sample.ps1 スクリプトの **サーバー** 変数にブレークポイントを設定します。 この例では、 **Mode** パラメーターを **ReadWrite** の値と共に使用して、変数の値が読み取られたときと値が変更される直前に実行を停止します。

```powershell
Set-PSBreakpoint -Script "sample.ps1" -Variable "Server" -Mode ReadWrite
```

### 例 4: 指定したテキストで始まるすべてのコマンドにブレークポイントを設定する

この例では、"write" で始まる Sample.ps1 スクリプト内のすべてのコマンドにブレークポイントを設定します (など) `Write-Host` 。

```powershell
Set-PSBreakpoint -Script Sample.ps1 -Command "write*"
```

### 例 5: 変数の値に応じてブレークポイントを設定する

この例 `DiskTest` では、変数の値が2より大きい場合にのみ、Test.ps1 スクリプトの関数で実行を停止し `$Disk` ます。

```powershell
Set-PSBreakpoint -Script "test.ps1" -Command "DiskTest" -Action { if ($Disk -gt 2) { break } }
```

**アクション** の値は、関数内の変数の値をテストするスクリプトブロックです `$Disk` 。

アクションは、 `break` 条件が満たされた場合に、キーワードを使用して実行を停止します。 代替 (および既定値) は **続行** されます。

### 例 6: 関数にブレークポイントを設定する

この例では、関数にブレークポイントを設定 `CheckLog` します。 このコマンドにはスクリプトが指定されていないため、現在のセッションで実行されているすべてにブレークポイントが設定されます。 デバッガーは、関数が宣言されているときではなく、呼び出されたときに中断します。

```
PS> Set-PSBreakpoint -Command "checklog"
Id       : 0
Command  : checklog
Enabled  : True
HitCount : 0
Action   :

function CheckLog {
>> get-eventlog -log Application |
>> where {($_.source -like "TestApp") -and ($_.Message -like "*failed*")}
>>}
>>
PS> Checklog
DEBUG: Hit breakpoint(s)
DEBUG:  Function breakpoint on 'prompt:Checklog'
```

### 例 7: 複数行にブレークポイントを設定する

この例では、Sample.ps1 スクリプトに3つの行のブレークポイントを設定します。 スクリプトで指定した各行の列 2 にブレークポイントが 1 つ設定されます。 **Action** パラメーターで指定されたアクションは、すべてのブレークポイントに適用されます。

```powershell
PS C:\> Set-PSBreakpoint -Script "sample.ps1" -Line 1, 14, 19 -Column 2 -Action {&(log.ps1)}
```

```Output
Column     : 2
Line       : 1
Action     :
Enabled    : True
HitCount   : 0
Id         : 6
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1


Column     : 2
Line       : 14
Action     :
Enabled    : True
HitCount   : 0
Id         : 7
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1


Column     : 2
Line       : 19
Action     :
Enabled    : True
HitCount   : 0
Id         : 8
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1
```

## PARAMETERS

### -Action

中断するのではなく、各ブレークポイントで実行するコマンドを指定します。 コマンドを含むスクリプト ブロックを入力します。 このパラメーターを使用して、条件付きブレークポイントを設定することや、テストやログの記録など、他のタスクを実行することができます。

このパラメーターを省略し、アクションを指定しない場合、ブレークポイントに達すると実行が停止し、デバッガーが開始されます。

**Action** パラメーターを使用すると、アクションスクリプトブロックが各ブレークポイントで実行されます。 スクリプト ブロックに Break キーワードが含まれない場合、実行は停止しません。 スクリプト ブロックに Continue キーワードを使用した場合、次のブレークポイントに達する前に実行が再開されます。

詳細については、「 [about_Script_Blocks](../Microsoft.PowerShell.Core/About/about_Script_Blocks.md)、 [about_Break](../Microsoft.PowerShell.Core/About/about_Break.md)、および [about_Continue](../Microsoft.PowerShell.Core/About/about_Continue.md)」を参照してください。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -列

実行を停止するスクリプト ファイル内の列の列番号を指定します。 列番号を 1 つのみ入力します。 既定値は 列 1 です。

列の値は、ブレークポイントを指定するために **Line** パラメーターの値と共に使用されます。 **Line** パラメーターに複数の行が指定されている場合、 **column** パラメーターは、指定した各行の指定された列にブレークポイントを設定します。 PowerShell は、指定された行と列の位置にある文字を含むステートメントまたは式の前に実行を停止します。

列は、最上位の左余白の先頭が列番号 1 でカウントされます (0 ではありません)。 スクリプトに存在しない列を指定した場合、エラーにはなりませんが、ブレークポイントが実行されることはありません。

```yaml
Type: System.Int32
Parameter Sets: Line
Aliases:

Required: False
Position: 2
Default value: 1
Accept pipeline input: False
Accept wildcard characters: False
```

### -Command

コマンドのブレークポイントを設定します。 コマンドレット名 (など) `Get-Process` 、または関数名を入力します。 ワイルドカードを使用できます。

各コマンドの各インスタンスが実行される直前に、実行が停止します。 コマンドが関数の場合、関数が呼び出されるたびに、BEGIN、PROCESS、および END セクションごとに実行が停止します。

```yaml
Type: System.String[]
Parameter Sets: Command
Aliases: C

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Line

スクリプトに行のブレークポイントを設定します。 コンマで区切って 1 つまたは複数の行番号を入力します。 PowerShell は、指定された各行で始まるステートメントを実行する直前に停止します。

行は、スクリプト ファイルの最上位の左余白の先頭が行番号 1 でカウントされます (0 ではありません)。
空白行を指定した場合、次の空白でない行の前で実行を停止します。 行が範囲外にある場合、ブレークポイントがヒットすることはありません。

```yaml
Type: System.Int32[]
Parameter Sets: Line
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Mode

変数のブレークポイントをトリガーするアクセスモードを指定します。 既定値は **Write** です。

このパラメーターは、 **Variable** パラメーターがコマンドで使用されている場合にのみ有効です。 このモードは、コマンドに設定されたブレークポイントすべてに適用されます。 このパラメーターの有効値は、次のとおりです。

- **書き込み** -新しい値が変数に書き込まれる直前に実行を停止します。
- **読み取り** -変数が読み取られたとき、つまり、割り当て、表示、または使用されるときに、変数の値にアクセスしたときに実行を停止します。 Read モードでは、変数の値が変更されたときは実行を停止しません。
- **ReadWrite** -変数の読み取りまたは書き込み時に実行を停止します。

```yaml
Type: System.Management.Automation.VariableAccessMode
Parameter Sets: Variable
Aliases:
Accepted values: Read, Write, ReadWrite

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -スクリプト

このコマンドレットによってブレークポイントが設定されるスクリプトファイルの配列を指定します。 1 つまたは複数のスクリプト ファイルのパスとファイル名を入力します。 ファイルが現在のディレクトリにある場合、パスを省略することができます。
ワイルドカードを使用できます。

既定では、変数のブレークポイントとコマンドのブレークポイントは、現在のセッションで実行されているコマンドに設定されます。 このパラメーターは、行のブレークポイントを設定するときにのみ必要です。

```yaml
Type: System.String[]
Parameter Sets: Line, Command, Variable
Aliases:

Required: True (Line), False (Command, Variable)
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Variable

このコマンドレットでブレークポイントを設定する変数の配列を指定します。 ドル記号を付けずに変数のコンマ区切りリストを入力し `$` ます ()。

**Mode** パラメーターを使用して、ブレークポイントをトリガーするアクセスモードを決定します。 既定のモードは Write であり、新しい値が変数に書き込まれる直前に実行を停止します。

```yaml
Type: System.String[]
Parameter Sets: Variable
Aliases: V

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
入力をにパイプすることはできません `Set-PSBreakpoint` 。

## 出力

### ブレークポイントオブジェクト (system.string、system......................... CommandBreakpoint ポイント、

`Set-PSBreakpoint` 設定されている各ブレークポイントを表すオブジェクトを返します。

## 注

- `Set-PSBreakpoint` リモートコンピューターにブレークポイントを設定することはできません。 リモート コンピューターでスクリプトをデバッグするには、スクリプトをローカル コンピューターにコピーしてからローカルでデバッグします。
- 複数の行、コマンド、または変数にブレークポイントを設定すると、によっ `Set-PSBreakpoint` て各エントリのブレークポイントオブジェクトが生成されます。
- コマンド プロンプトで関数または変数にブレークポイントを設定すると、関数または変数を作成する前または後に、ブレークポイントを設定できます。

## 関連リンク

[Disable-PSBreakpoint](Disable-PSBreakpoint.md)

[Enable-PSBreakpoint](Enable-PSBreakpoint.md)

[Get-PSBreakpoint](Get-PSBreakpoint.md)

[Get-PSCallStack](Get-PSCallStack.md)

[Remove-PSBreakpoint](Remove-PSBreakpoint.md)

