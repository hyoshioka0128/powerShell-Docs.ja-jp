---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-warning?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Warning
ms.openlocfilehash: 18c168e894989fea8b26fe000a624f91d7345332
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604963"
---
# Write-Warning

## 概要
警告メッセージを書き込みます。

## SYNTAX

```
Write-Warning [-Message] <String> [<CommonParameters>]
```

## Description

コマンドレットでは、 `Write-Warning` 警告メッセージを PowerShell ホストに書き込みます。 警告への応答は、ユーザーの変数の値と、 `$WarningPreference` **warnings action** 共通パラメーターの使用によって異なります。

## 例

### 例 1: 警告メッセージを記述する

このコマンドは、"WARNING: This is only a test warning." というメッセージを表示します。

```powershell
Write-Warning "This is only a test warning."
```

### 例 2: Write-Warning に文字列を渡す

このコマンドは、パイプライン演算子 () を使用してに文字列を送信できることを示して `|` `Write-Warning` います。
このコマンドに示すように、変数に文字列を保存することも、文字列を直接にパイプすることもでき `Write-Warning` ます。

```powershell
$w = "This is only a test warning."
$w | Write-Warning
```

### 例 3: $WarningPreference 変数を設定し、警告を記述する

この例は、コマンドの変数の値の効果を示して `$WarningPreference` `Write-Warning` います。

```powershell
PS> $WarningPreference
Continue
PS> Write-Warning "This is only a test warning."
This is only a test warning.
PS> $WarningPreference = "SilentlyContinue"
PS> Write-Warning "This is only a test warning."
PS> $WarningPreference = "Stop"
PS> Write-Warning "This is only a test warning."
WARNING: This is only a test message.
Write-Warning : Command execution stopped because the shell variable "WarningPreference" is set to Stop.
At line:1 char:14
     + Write-Warning <<<<  "This is only a test message."
```

最初のコマンドは、変数の既定値であるを表示し `$WarningPreference` `Continue` ます。 この結果、警告メッセージが表示された後も実行が継続されます。

変数の値を変更すると、 `$WarningPreference` コマンドの効果が `Write-Warning` 再度変更されます。 値がの場合、 `SilentlyContinue` 警告は表示されません。 値がの場合、 `Stop` 警告が表示され、コマンドの実行が停止します。

変数の詳細については `$WarningPreference` 、「 [about_Preference_Variables](../Microsoft.Powershell.Core/About/about_Preference_Variables.md)」を参照してください。

### 例 4: Warnings アクションパラメーターを設定し、警告を書き込む

この例は、コマンドに対する **Warnings action** 共通パラメーターの効果を示して `Write-Warning` います。 任意のコマンドレットと共に **Warnings action** 共通パラメーターを使用して、そのコマンドによって生成された警告に対して PowerShell がどのように応答するかを決定できます。 **Warnings action** 共通パラメーターは、その特定のコマンドのの値のみをオーバーライドし `$WarningPreference` ます。

```powershell
PS> Write-Warning "This is only a test warning." -WarningAction Inquire
WARNING: This is only a test warning.
Confirm
Continue with this operation?
 [Y] Yes  [A] Yes to All  [H] Halt Command  [S] Suspend  [?] Help (default is "Y"):
```

このコマンドは、 `Write-Warning` コマンドレットを使用して警告を表示します。 値が Inquire の **Warnings action** 共通パラメーターは、コマンドで警告が表示されたときにユーザーにプロンプトを表示するようシステムに指示します。

**Warnings action** 共通パラメーターの詳細については、「 [about_CommonParameters](../Microsoft.Powershell.Core/About/about_CommonParameters.md)」を参照してください。

## PARAMETERS

### -メッセージ
警告メッセージを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: Msg

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、警告を含む文字列をにパイプすることができ `Write-Warning` ます。

## 出力

### なし

`Write-Warning` 警告ストリームにのみ書き込みます。 その他の出力は生成しません。

## 注

変数の既定値 `$WarningPreference` はです `Continue` 。これにより、警告が表示され、コマンドの実行が続行されます。 などのユーザー設定変数の有効な値を確認するには、 `$WarningPreference` "abc" などのランダムな文字の文字列に設定します。 結果として得られるエラーメッセージには、有効な値が表示されます。

## 関連リンク

[about_Output_Streams](../Microsoft.PowerShell.Core/About/about_Output_Streams.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Write-Debug](Write-Debug.md)

[Write-Error](Write-Error.md)

[Write-Host](Write-Host.md)

[Write-Information](Write-Information.md)

[Write-Output](Write-Output.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)
