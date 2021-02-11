---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/debug-runspace?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Debug-Runspace
ms.openlocfilehash: 3f01b7624ffcbca472b09bdb83a7440f3526db43
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603229"
---
# Debug-Runspace

## 概要
実行空間を使用して、対話型デバッグセッションを開始します。

## SYNTAX

### RunspaceParameterSet (既定値)

```
Debug-Runspace [-Runspace] <Runspace> [-BreakAll] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameParameterSet

```
Debug-Runspace [-Name] <String> [-BreakAll] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### IdParameterSet

```
Debug-Runspace [-Id] <Int32> [-BreakAll] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceIdParameterSet

```
Debug-Runspace [-InstanceId] <Guid> [-BreakAll] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Debug-Runspace`コマンドレットは、ローカルまたはリモートのアクティブな実行空間を使用して、対話型デバッグセッションを開始します。 最初にを実行してデバッグする実行空間を見つけて、 `Get-Process` powershell に関連付けられているプロセスを見つけ、そのプロセス `Enter-PSHostProcess` にアタッチする **id** パラメーターで指定されたプロセス ID を使用して、 `Get-Runspace` powershell ホストプロセス内の実行空間を一覧表示することができます。

デバッグする実行空間を選択すると、実行空間で現在コマンドまたはスクリプトが実行されている場合、またはスクリプトがブレークポイントで停止した場合、PowerShell は実行空間に対してリモートデバッガーセッションを開きます。 リモートセッションスクリプトがデバッグされるのと同じ方法で実行空間スクリプトをデバッグできます。

PowerShell ホストプロセスにアタッチできるのは、そのプロセスを実行しているコンピューターの管理者である場合、またはデバッグするスクリプトを実行している場合のみです。 また、現在の PowerShell セッションを実行しているホストプロセスを入力することはできません。 別の PowerShell セッションを実行しているホストプロセスのみを入力できます。

## 例

### 例 1: リモートの実行空間をデバッグする

```
PS C:\> Get-Process -ComputerName "WS10TestServer" -Name "*powershell*"

Handles      WS(K)   VM(M)      CPU(s)    Id  ProcessName
-------      -----   -----      ------    --  -----------
    377      69912     63     2.09      2420  powershell
    399     123396    829     4.48      1152  powershell_ise

PS C:\> Enter-PSSession -ComputerName "WS10TestServer"
[WS10TestServer]:PS C:\> Enter-PSHostProcess -Id 1152
[WS10TestServer:][Process:1152]: PS C:\Users\Test\Documents> Get-Runspace

Id Name            ComputerName    Type          State         Availability
-- ----            ------------    ----          -----         ------------
 1 Runspace1       WS10TestServer  Remote        Opened        Available
 2 RemoteHost      WS10TestServer  Remote        Opened        Busy

PS C:\> [WS10TestServer][Process:1152]: PS C:\Users\Test\Documents> Debug-Runspace -Id 2

Hit Line breakpoint on 'C:\TestWFVar1.ps1:83'
At C:\TestWFVar1.ps1:83 char:1
+ $scriptVar = "Script Variable"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[Process:1152]: [RSDBG: 2]: PS C:\> >
```

この例では、リモートコンピューターで開いている実行空間 WS10TestServer をデバッグします。 コマンドの最初の行で、リモートコンピューターでを実行し、 `Get-Process` Windows PowerShell ホストプロセスをフィルター処理します。 この例では、プロセス ID 1152、Windows PowerShell ISE ホストプロセスをデバッグします。

2番目のコマンドでは、を実行し `Enter-PSSession` て、WS10TestServer でリモートセッションを開きます。 3番目のコマンドでは、を実行し、 `Enter-PSHostProcess` 最初のコマンド1152で取得したホストプロセスの ID を指定して、リモートサーバー上で実行されている Windows PowerShell ISE ホストプロセスにアタッチします。

4番目のコマンドでは、を実行して、プロセス ID 1152 の使用可能な実行空間を一覧表示し `Get-Runspace` ます。
ビジー状態の実行空間の ID 番号をメモしておきます。デバッグするスクリプトを実行しています。

最後のコマンドでは、スクリプトを実行している開いている実行空間のデバッグ TestWFVar1.ps1 を開始し、を実行して、id パラメーターを追加することによって、実行 `Debug-Runspace` 空間を id (2) で識別します。  スクリプトにブレークポイントがあるため、デバッガーが開きます。

## PARAMETERS

### -Id

実行空間の ID 番号を指定します。 実行 `Get-Runspace` 空間 id を表示するには、を実行します。

```yaml
Type: System.Int32
Parameter Sets: IdParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InstanceId

実行空間をインスタンス ID で指定します。この GUID は、を実行して表示でき `Get-Runspace` ます。

```yaml
Type: System.Guid
Parameter Sets: InstanceIdParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

実行空間を名前で指定します。 実行 `Get-Runspace` 空間の名前を表示するには、を実行します。

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -実行空間

実行空間オブジェクトを指定します。 このパラメーターに値を指定する最も簡単な方法は、フィルター処理されたコマンドの結果を含む変数を指定することです `Get-Runspace` 。

```yaml
Type: System.Management.Automation.Runspaces.Runspace
Parameter Sets: RunspaceParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
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
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

### -BreakAll

{{BreakAll の説明を入力}}

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. 実行空間

パイプを使用してコマンドの結果をデバッグすることができ `Get-Runspace` **ます。**

## 出力

## 注

`Debug-Runspace` Opened 状態の実行空間で機能します。 実行空間の状態が opened から別の状態に変わった場合、実行空間は実行中の一覧から自動的に削除されます。 実行空間は、次の条件を満たしている場合にのみ、実行中の一覧に追加されます。

- 呼び出し元である場合は、つまり、GUID ID があり `Invoke-Command` ます。
- から送信されている場合は。 `Debug-Runspace` GUID ID を持つ場合は `Debug-Runspace` 。
- PowerShell ワークフローから送信され、そのワークフロージョブ ID が現在のアクティブデバッガーワークフロージョブ ID と同じである場合。

## 関連リンク

[about_Debuggers](../Microsoft.PowerShell.Core/About/about_Debuggers.md)

[Debug-Job](../Microsoft.PowerShell.Core/Debug-Job.md)

[Get-Runspace](Get-Runspace.md)

[Get-Process](../Microsoft.PowerShell.Management/Get-Process.md)

[Enter-PSHostProcess](../Microsoft.PowerShell.Core/Enter-PSHostProcess.md)

[Enter-PSSession](../Microsoft.PowerShell.Core/Enter-PSSession.md)

