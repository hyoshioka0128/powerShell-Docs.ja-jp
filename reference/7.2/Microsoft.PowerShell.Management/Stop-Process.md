---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/stop-process?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Stop-Process
ms.openlocfilehash: 8c9e520f1f19444fd538fabee99a48ec08c69992
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602200"
---
# Stop-Process

## 概要
実行中のプロセスを 1 つ以上停止します。

## SYNTAX

### Id (既定値)

```
Stop-Process [-Id] <Int32[]> [-PassThru] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### 名前

```
Stop-Process -Name <String[]> [-PassThru] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObject

```
Stop-Process [-InputObject] <Process[]> [-PassThru] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

**Stop Process** コマンドレットは、実行中の1つ以上のプロセスを停止します。
プロセスは、プロセス名またはプロセス ID (PID) で指定することも、プロセスオブジェクトを **停止プロセス** に渡すこともできます。
**停止プロセス** は、ローカルコンピューター上で実行されているプロセスでのみ機能します。

Windows Vista 以降のバージョンの Windows オペレーティングシステムでは、現在のユーザーが所有していないプロセスを停止するには、[管理者として実行] オプションを使用して PowerShell を起動する必要があります。
また、 *Confirm* パラメーターを指定しない限り、確認のプロンプトは表示されません。

## 例

### 例 1: プロセスのすべてのインスタンスを停止する

```
PS C:\> Stop-Process -Name "notepad"
```

このコマンドを実行すると、コンピューター上の Notepad プロセスのインスタンスがすべて停止されます。
メモ帳の各インスタンスは、独自のプロセスで実行されます。
*Name* パラメーターを使用してプロセスを指定し、そのすべてが同じ名前を持っています。
*Id* パラメーターを使用して同じプロセスを停止する場合は、メモ帳の各インスタンスのプロセス id を一覧表示する必要があります。

### 例 2: プロセスの特定のインスタンスを停止する

```
PS C:\> Stop-Process -Id 3952 -Confirm -PassThru
Confirm
Are you sure you want to perform this action?
Performing operation "Stop-Process" on Target "notepad (3952)".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):y
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
41       2      996       3212    31            3952 notepad
```

このコマンドを実行すると、Notepad プロセスの特定のインスタンスが停止されます。
プロセス ID 3952 を使用してプロセスを識別しています。
*Confirm* パラメーターは、プロセスを停止する前にプロンプトを表示するように PowerShell に指示します。
このプロンプトには ID に加えてプロセス名が含まれているため、これがベストプラクティスです。
*PassThru* パラメーターは、プロセスオブジェクトを表示用のフォーマッタに渡します。
このパラメーターを指定しないと、 **プロセスの停止** コマンドの後に表示されません。

### 例 3: プロセスを停止し、停止したことを検出する

```
PS C:\> calc
PS C:\> $p = Get-Process -Name "calc"
PS C:\> Stop-Process -InputObject $p
PS C:\> Get-Process | Where-Object {$_.HasExited}
```

この一連のコマンドは、Calc プロセスを開始および停止し、停止したプロセスを検出します。

最初のコマンドは、電卓のインスタンスを起動します。

2番目のコマンドは、[ **取得プロセス** ] を使用して、Calc プロセスを表すオブジェクトを取得し、$p 変数に格納します。

3番目のコマンドは、Calc 処理を停止します。
*InputObject* パラメーターを使用して、オブジェクトを **停止プロセス** に渡します。

最後のコマンドは、コンピューター上の、以前は実行中で現在は停止しているすべてのプロセスを取得します。
コンピューター上のすべてのプロセスを取得するには、 **Get Process** を使用します。
パイプライン演算子 (|) は、Where-Object コマンドレットに結果を渡します。このコマンドレットは、 **Hasexited** $True プロパティの値が指定されているものを選択します。
**Hasexited** 、プロセスオブジェクトの1つのプロパティにすぎません。
すべてのプロパティを検索するには、「」と入力 `Get-Process | Get-Member` します。

### 例 4: 現在のユーザーが所有していないプロセスを停止する

```
PS C:\> Get-Process -Name "lsass" | Stop-Process

Stop-Process : Cannot stop process 'lsass (596)' because of the following error: Access is denied
At line:1 char:34
+ Get-Process -Name "lsass" | Stop-Process <<<<

[ADMIN]: PS C:\> Get-Process -Name "lsass" | Stop-Process

Warning!
Are you sure you want to perform this action?
Performing operation 'Stop-Process' on Target 'lsass(596)'
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
[ADMIN]: PS C:\> Get-Process -Name "lsass" | Stop-Process -Force
[ADMIN]: PS C:\>
```

これらのコマンドは、 *Force* を使用して、ユーザーが所有していないプロセスを停止した場合の影響を示します。

最初のコマンドは、 **Get process** を使用して Lsass プロセスを取得します。
パイプライン演算子は、プロセスを停止 **プロセス** に送信して停止します。
サンプル出力に示されているように、最初のコマンドはアクセス拒否メッセージで失敗します。このプロセスは、コンピューターの Administrator グループのメンバーによってのみ停止される可能性があるためです。

[管理者として実行] オプションを使用して PowerShell を開き、コマンドを繰り返すと、確認を求めるメッセージが表示されます。

2番目のコマンドは、プロンプトを抑制する *Force* を指定します。
その結果、プロセスは確認なしで停止されます。

## PARAMETERS

### -Force

確認メッセージを表示せずに、指定されたプロセスを停止します。
既定では、現在のユーザーが所有していないプロセスを停止する前に、 **停止処理** によって確認メッセージが表示されます。

プロセスの所有者を見つけるには、コマンドレットを使用して、 `Get-CimInstance` プロセスを表す **Win32_Process** オブジェクトを取得し、オブジェクトの **GetOwner** メソッドを使用します。

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

### -Id

停止するプロセスのプロセス Id を指定します。
複数の ID を指定するには、ID をコンマで区切ります。
プロセスの PID を検索するには、「」と入力 `Get-Process` します。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InputObject

停止するプロセスオブジェクトを指定します。
オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Diagnostics.Process[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

停止するプロセスのプロセス名を指定します。
複数のプロセス名をコンマで区切って入力することも、ワイルドカード文字を使用することもできます。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases: ProcessName

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -PassThru

プロセスを表すオブジェクトを返します。
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
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Diagnostics.Process

パイプを使用してプロセスオブジェクトをこのコマンドレットに渡します。

## 出力

### なし、システム。診断プロセス

このコマンドレットは、 *PassThru* パラメーターを指定した場合に、停止されたプロセスを表す **system.string オブジェクトを返します。**
それ以外の場合、このコマンドレットによる出力はありません。

## 注

* また、組み込みエイリアスである **kill** と **spps** を使用して、**停止プロセス** を参照することもできます。 詳細については、「about_Aliases」を参照してください。

  Windows PowerShell では、Windows Management Instrumentation (WMI) **Win32_Process** オブジェクトのプロパティとメソッドを使用することもできます。
詳細については、「」および「WMI SDK」を参照してください `Get-CimInstance` 。

* プロセスを停止すると、プロセスに依存するプロセスやサービスが停止する可能性があることに注意してください。
場合によっては、プロセスを停止したとき、Windows が停止する場合もあります。

## 関連リンク

[Debug-Process](Debug-Process.md)

[Get-Process](Get-Process.md)

[Start-Process](Start-Process.md)

[Stop-Process](Stop-Process.md)

[Wait-Process](Wait-Process.md)

