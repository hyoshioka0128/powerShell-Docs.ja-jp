---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/debug-process?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Debug-Process
ms.openlocfilehash: 660d2b4845df8091ff8b69b28c4e7695af557122
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602773"
---
# Debug-Process

## 概要
ローカル コンピューター上で実行中の 1 つ以上のプロセスをデバッグします。

## SYNTAX

### 名前 (既定値)

```
Debug-Process [-Name] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Id

```
Debug-Process [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObject

```
Debug-Process -InputObject <Process[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Debug-Process`コマンドレットは、ローカルコンピューター上で実行中の1つ以上のプロセスにデバッガーをアタッチします。
プロセスは、プロセス名またはプロセス ID (PID) で指定することも、パイプを使用してこのコマンドレットに処理することもできます。

このコマンドレットは、プロセスに対して現在登録されているデバッガーをアタッチします。 このコマンドレットを使用する前に、デバッガーがダウンロードされていて適切に設定されていることを確認します。

## 例

### 例 1: コンピューター上のプロセスにデバッガーをアタッチする

```
PS C:\> Debug-Process -Name "Windows Powershell"
```

このコマンドは、コンピューターの PowerShell プロセスにデバッガーを結合します。

### 例 2: 指定した文字列で始まるすべてのプロセスにデバッガーをアタッチする

```
PS C:\> Debug-Process -Name "SQL*"
```

このコマンドは、名前が SQL で始まるすべてのプロセスにデバッガーをアタッチします。

### 例 3: デバッガーを複数のプロセスにアタッチする

```
PS C:\> Debug-Process "Winlogon", "Explorer", "Outlook"
```

このコマンドは、Winlogon、Explorer、Outlook の各プロセスにデバッガーを結合します。

### 例 4: デバッガーを複数のプロセス Id にアタッチする

```
PS C:\> Debug-Process -Id 1132, 2028
```

このコマンドは、プロセス ID 1132 および 2028 のプロセスにデバッガーを結合します。

### 例 5: Get-Process を使用してプロセスを取得し、デバッガーをアタッチする

```
PS C:\> Get-Process "Windows PowerShell" | Debug-Process
```

このコマンドは、コンピューターの PowerShell プロセスにデバッガーを結合します。 コマンドレットを使用して `Get-Process` コンピューター上の PowerShell プロセスを取得し、パイプライン演算子 () を使用して `|` プロセスをコマンドレットに送信し `Debug-Process` ます。

特定の PowerShell プロセスを指定するには、の ID パラメーターを使用し `Get-Process` ます。

### 例 6: ローカルコンピューター上の現在のプロセスにデバッガーをアタッチする

```
PS C:\> $PID | Debug-Process
```

このコマンドは、コンピューターの現在の PowerShell プロセスにデバッガーを結合します。

このコマンドは、 `$PID` 現在の PowerShell プロセスのプロセス ID を含む自動変数を使用します。 次に、パイプライン演算子 () を使用して、 `|` プロセス ID をコマンドレットに送信し `Debug-Process` ます。

自動変数の詳細については `$PID` 、「about_Automatic_Variables」を参照してください。

### 例 7: InputObject パラメーターを使用するプロセスにデバッガーをアタッチする

```
PS C:\> $P = Get-Process "Windows PowerShell"
PS C:\> Debug-Process -InputObject $P
```

このコマンドは、ローカル コンピューターの PowerShell プロセスにデバッガーを結合します。

最初のコマンドは、 `Get-Process` コマンドレットを使用して、コンピューター上の PowerShell プロセスを取得します。 結果として得られるプロセスオブジェクトをという名前の変数に保存し `$P` ます。

2番目のコマンドは、コマンドレットの **InputObject** パラメーターを使用して `Debug-Process` 、変数内のプロセスオブジェクトを送信し `$P` ます。

## PARAMETERS

### -Id

デバッグするプロセスのプロセス ID を指定します。 **Id** パラメーター名は省略可能です。

プロセスのプロセス ID を検索するには、「」と入力 `Get-Process` します。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases: PID, ProcessId

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InputObject

デバッグするプロセスを表すプロセス オブジェクトを指定します。 プロセスオブジェクトを含む変数、または、コマンドレットなどのプロセスオブジェクトを取得するコマンドを入力し `Get-Process` ます。 パイプを使用してプロセスオブジェクトをこのコマンドレットに送ることもできます。

```yaml
Type: System.Diagnostics.Process[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

デバッグするプロセスの名前を指定します。 同じ名前のプロセスが複数ある場合、このコマンドレットは、その名前を持つすべてのプロセスにデバッガーをアタッチします。 **Name** パラメーターは省略可能です。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases: ProcessName

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

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

### System.string, System.string, System.string, System.string,

パイプを使用して、プロセス ID (Int32)、プロセスオブジェクト (システム診断プロセス)、またはプロセス名 (文字列) をこのコマンドレットに渡します。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

このコマンドレットは、Windows Management Instrumentation (WMI) Win32_Process クラスの AttachDebugger メソッドを使用します。 このメソッドの詳細については、MSDN ライブラリの「 [Attachdebugger メソッド](https://go.microsoft.com/fwlink/?LinkId=143640) 」を参照してください。

## 関連リンク

[Debug-Process](Debug-Process.md)

[Get-Process](Get-Process.md)

[Start-Process](Start-Process.md)

[Stop-Process](Stop-Process.md)

[Wait-Process](Wait-Process.md)
