---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-debug?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Debug
ms.openlocfilehash: 3d23f76dbf8e1c9a21805a4914038c8c4f319852
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602784"
---
# Write-Debug

## 概要
デバッグ メッセージをコンソールに出力します。

## SYNTAX

```
Write-Debug [-Message] <String> [<CommonParameters>]
```

## Description

`Write-Debug`コマンドレットは、スクリプトまたはコマンドからデバッグメッセージをホストに書き込みます。

既定では、デバッグメッセージはコンソールに表示されませんが、 **debug** パラメーターまたは変数を使用して表示でき `$DebugPreference` ます。

## 例

### 例 1: $DebugPreference を理解する

この例では、デバッグメッセージを書き込みます。

```powershell
Write-Debug "Cannot open file."
```

の既定値 `$DebugPreference` は **SilentlyContinue** です。 このため、メッセージはコンソールに表示されません。

### 例 2: $DebugPreference の値を変更する

この例では、変数の値を変更した場合の影響を示し `$DebugPreference` ます。 まず、の現在の値を表示 `$DebugPreference` し、デバッグメッセージの書き込みを試みます。 次に、の値を `$DebugPreference` [ **続行**] に変更します。これにより、デバッグメッセージを表示できるようになります。

```
PS> $DebugPreference
SilentlyContinue
PS> Write-Debug "Cannot open file."
PS>
PS> $DebugPreference = "Continue"
PS> Write-Debug "Cannot open file."
DEBUG: Cannot open file.
```

の詳細について `$DebugPreference` は、「 [about_Preference_Variables](/powershell/module/Microsoft.PowerShell.Core/About/about_Preference_Variables)」を参照してください。

### 例 3: Debug パラメーターを使用して $DebugPreference をオーバーライドする

関数は、 `Test-Debug` 変数の値を `$DebugPreference` PowerShell ホストおよびデバッグストリームに書き込みます。 この例では、 **Debug** パラメーターを使用して値をオーバーライドし `$DebugPreference` ます。

```powershell
function Test-Debug {
    [CmdletBinding()]
    param()
    Write-Debug ('$DebugPreference is ' + $DebugPreference)
    Write-Host ('$DebugPreference is ' + $DebugPreference)
}
```

```
PS> Test-Debug
$DebugPreference is SilentlyContinue

PS> Test-Debug -Debug
DEBUG: $DebugPreference is Continue
$DebugPreference is Continue
PS> $DebugPreference
SilentlyContinue
```

`$DebugPreference` **Debug** パラメーターを使用すると、の値が変更されることに注意してください。 この変更は、関数のスコープにのみ影響します。 値は関数の外部には影響しません。

**Debug** 共通パラメーターの詳細については、「 [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## PARAMETERS

### -メッセージ

コンソールに出力するデバッグ メッセージを指定します。

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

パイプを使用して、デバッグメッセージを含む文字列をにパイプすることができ `Write-Debug` ます。

## 出力

### なし

`Write-Debug` は、デバッグストリームにのみ書き込みます。 パイプラインにオブジェクトを書き込むことはありません。

## 注

## 関連リンク

[about_Output_Streams](../Microsoft.PowerShell.Core/About/about_Output_Streams.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Write-Error](Write-Error.md)

[Write-Host](Write-Host.md)

[Write-Output](Write-Output.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)
