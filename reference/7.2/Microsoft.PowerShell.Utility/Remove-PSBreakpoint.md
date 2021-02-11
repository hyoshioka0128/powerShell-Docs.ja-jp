---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/remove-psbreakpoint?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-PSBreakpoint
ms.openlocfilehash: a56b9041c0ae70def7df619f2a4d265cb631fe7b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600784"
---
# Remove-PSBreakpoint

## 概要
現在のコンソールからブレークポイントを削除します。

## SYNTAX

### ブレークポイント (既定値)

```
Remove-PSBreakpoint [-Breakpoint] <Breakpoint[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Id

```
Remove-PSBreakpoint [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description
Delete **PSBreakpoint** ポイントコマンドレットは、ブレークポイントを削除します。
ブレークポイント　オブジェクトまたはブレークポイント ID を入力します。

ブレークポイントを削除すると、ブレークポイント オブジェクトは使用できないか、機能不能となります。
ブレークポイント オブジェクトを変数を保存した場合、参照は存続するが、ブレークポイントは機能しません。

**削除-PSBreakpoint ポイントは、** PowerShell スクリプトのデバッグ用に設計されたいくつかのコマンドレットの1つです。
PowerShell デバッガーの詳細については、「about_Debuggers」を参照してください。

## 例

### 例 1: すべてのブレークポイントを削除する

```
PS C:\> Get-PSBreakpoint | Remove-PSBreakpoint
```

このコマンドは、現在のコンソール内のすべてのブレークポイントを削除します。

### 例 2: 指定したブレークポイントを削除する

```
PS C:\> $B = Set-PSBreakpoint -Script "sample.ps1" -Variable "Name"
PS C:\> $B | Remove-PSBreakpoint
```

このコマンドは、ブレークポイントを削除します。

最初のコマンドでは、Set-PSBreakpoint コマンドレットを使用して、Sample.ps1 スクリプトの Name 変数にブレークポイントを作成しています。
次に、ブレークポイントオブジェクトを $B 変数に保存します。

2番目のコマンドは、 **削除 PSBreakpoint ポイント** コマンドレットを使用して、新しいブレークポイントを削除します。
パイプライン演算子 (|) を使用して、$B 変数内のブレークポイントオブジェクトを、 **削除 PSBreakpoint ポイント** コマンドレットに送信します。

このコマンドの結果、スクリプトを実行した場合、停止することなく完了するまで実行されます。
また、 **Get PSBreakpoint ポイント** コマンドレットは、このブレークポイントを返しません。

### 例 3: ID によってブレークポイントを削除する

```
PS C:\> Remove-PSBreakpoint -Id 2
```

このコマンドは、ブレークポイント ID が 2 のブレークポイントを削除します。

### 例 4: 関数を使用してすべてのブレークポイントを削除する

```
PS C:\> function del-psb { get-psbreakpoint | remove-psbreakpoint }
```

この単純な関数は、現在のコンソール内のすべてのブレークポイントを削除します。
Get-PSBreakpoint コマンドレットを使用してブレークポイントを取得します。
次に、パイプライン演算子 (|) を使用して、ブレークポイントを **削除する、PSBreakpoint ポイント** コマンドレットにブレークポイントを送信します。

そのため、 `del-psb` 長いコマンドの代わりにを入力できます。

関数を保存するには、PowerShell プロファイルに追加します。

## PARAMETERS

### -ブレークポイント
削除するブレークポイントを指定します。
ブレークポイントオブジェクトを含む変数、または、ブレークポイントオブジェクトを取得するコマンド ( **Get PSBreakpoint** ポイントコマンドなど) を入力します。
パイプを使用して、ブレークポイントオブジェクトを **削除** することもできます。

```yaml
Type: System.Management.Automation.Breakpoint[]
Parameter Sets: Breakpoint
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Id
このコマンドレットがブレークポイントを削除するブレークポイント Id を指定します。

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

### システム.... ブレークポイント
パイプを使用してブレークポイントオブジェクトを **削除し、-PSBreakpoint ポイントを削除** できます。

## 出力

### なし
このコマンドレットは出力を生成しません。

## 注

## 関連リンク

[Disable-PSBreakpoint](Disable-PSBreakpoint.md)

[Enable-PSBreakpoint](Enable-PSBreakpoint.md)

[Get-PSBreakpoint](Get-PSBreakpoint.md)

[Get-PSCallStack](Get-PSCallStack.md)

[Set-PSBreakpoint](Set-PSBreakpoint.md)

