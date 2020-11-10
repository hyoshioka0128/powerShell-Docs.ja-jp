---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/exit-pssession?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Exit-PSSession
ms.openlocfilehash: efe0e6c9287b3595988aa3ffc520ce46699cafda
ms.sourcegitcommit: 9b28fb9a3d72655bb63f62af18b3a5af6a05cd3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/07/2020
ms.locfileid: "93212067"
---
# Exit-PSSession

## 概要
リモート コンピューターとの対話型セッションを終了します。

## SYNTAX

```
Exit-PSSession [<CommonParameters>]
```

## Description
**終了 PSSession** コマンドレットは、Enter-PSSession コマンドレットを使用して開始した対話型セッションを終了します。

また、 **Exit** キーワードを使用して、対話型セッションを終了することもできます。
効果は、 **終了 PSSession** を使用する場合と同じです。

## 例

### 例 1: 対話型セッションを開始および停止する

```
PS C:\> Enter-PSSession -computername Server01
Server01\PS> Exit-PSSession
PS C:\>
```

これらのコマンドは、Server01 リモート コンピューターで対話型セッションを開始および停止します。

### 例 2: PSSession オブジェクトを使用して対話型セッションを開始および停止する

```
PS C:\> $s = New-PSSession -ComputerName Server01
PS C:\> Enter-PSSession -Session $s
Server01\PS> Exit-PSSession
PS C:\> $s
Id Name            ComputerName    State    ConfigurationName
-- ----            ------------    -----    -----------------
1  Session1        Server01        Opened   Microsoft.PowerShell
```

これらのコマンドは、Windows PowerShell セッション ( **PSSession** ) を使用する Server01 コンピューターとの対話型セッションを開始および停止します。

対話型セッションは Windows PowerShell セッションを使用して開始されたため、対話型セッションが終了しても **PSSession** は引き続き利用できます。
*ComputerName* パラメーターを使用する場合、「 **-PSSession** 」と入力すると、対話型セッションが終了したときに閉じられる一時的なセッションが作成されます。

最初のコマンドは、New-PSSession コマンドレットを使用して、Server01 コンピューター上に **PSSession** を作成します。
このコマンドは、 **PSSession** を $s 変数に保存します。

2番目のコマンドは、 **Enter-pssession** を使用して、$S の **PSSession** を使用して対話型セッションを開始します。

3番目のコマンドは、 **出口** を使用して対話型セッションを停止します。

最後のコマンドは、$s 変数内の **PSSession** を表示します。
**State** プロパティは、 **PSSession** がまだ開いており、使用可能であることを示しています。

### 例 3: Exit キーワードを使用してセッションを停止する

```
PS C:\> Enter-PSSession -computername Server01
Server01\PS> exit
PS C:\>
```

この例では、 **Exit** キーワードを使用して、 **入力 PSSession** を使用して開始された対話型セッションを停止します。
**Exit** キーワードは、 **出口** を使用する場合と同じ効果があります。

## PARAMETERS

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### なし
このコマンドレットによる戻り値はありません。

## 注

* このコマンドレットは、共通パラメーターのみを受け取ります。


## 関連リンク

[Connect-PSSession](Connect-PSSession.md)

[Disconnect-PSSession](Disconnect-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)

[Receive-PSSession](Receive-PSSession.md)

[Remove-PSSession](Remove-PSSession.md)