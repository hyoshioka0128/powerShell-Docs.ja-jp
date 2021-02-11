---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-verbose?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Verbose
ms.openlocfilehash: 177e32106f37c59593bbecac13843f6603327cc9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599542"
---
# Write-Verbose

## 概要
テキストを詳細メッセージ ストリームに書き込みます。

## SYNTAX

```
Write-Verbose [-Message] <String> [<CommonParameters>]
```

## Description

`Write-Verbose`コマンドレットは、PowerShell の詳細メッセージストリームにテキストを書き込みます。 通常、詳細メッセージストリームは、コマンド処理に関する詳細な情報を提供するために使用されます。

既定では、詳細メッセージストリームは表示されませんが、変数の値を変更する `$VerbosePreference` か、任意のコマンドで **verbose** 共通パラメーターを使用して表示できます。

## 例

### 例 1: ステータスメッセージを書き込む

```powershell
Write-Verbose -Message "Searching the Application Event Log."
Write-Verbose -Message "Searching the Application Event Log." -Verbose
```

これらのコマンドは、 `Write-Verbose` コマンドレットを使用してステータスメッセージを表示します。 既定では、メッセージは表示されません。

2番目のコマンドは、 **verbose** 共通パラメーターを使用します。これにより、変数の値に関係なく、詳細なメッセージが表示され `$VerbosePreference` ます。

### 例 2: $VerbosePreference を設定してステータスメッセージを書き込む

```powershell
$VerbosePreference = "Continue"
Write-Verbose "Copying file $filename"
```

これらのコマンドは、 `Write-Verbose` コマンドレットを使用してステータスメッセージを表示します。 既定では、メッセージは表示されません。

最初のコマンドは、ユーザー設定変数に Continue の値を割り当て `$VerbosePreference` ます。 既定値のでは、詳細メッセージが表示さ `SilentlyContinue` れません。 2 番目のコマンドは、詳細メッセージを書き込みます。

## PARAMETERS

### -メッセージ

表示するメッセージを指定します。 このパラメーターは必須です。 また、パイプを使用してメッセージ文字列をにパイプすることもでき `Write-Verbose` ます。

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用して、メッセージを含む文字列をにパイプ処理でき `Write-Verbose` ます。

## 出力

### なし

`Write-Verbose` 詳細メッセージストリームにのみ書き込みます。

## 注

- 詳細メッセージは、コマンドが **Verbose** 共通パラメーターを使用する場合のみ返されます。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。
- Windows PowerShell のバックグラウンドジョブとリモートコマンドでは、 `$VerbosePreference` ジョブセッションとリモートセッションの変数によって、既定で詳細メッセージが表示されるかどうかが決まります。
  変数の詳細については `$VerbosePreference` 、「 [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

## 関連リンク

[about_Output_Streams](../Microsoft.PowerShell.Core/About/about_Output_Streams.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Write-Debug](Write-Debug.md)

[Write-Error](Write-Error.md)

[Write-Host](Write-Host.md)

[Write-Information](Write-Information.md)

[Write-Output](Write-Output.md)

[Write-Progress](Write-Progress.md)

[Write-Warning](Write-Warning.md)
