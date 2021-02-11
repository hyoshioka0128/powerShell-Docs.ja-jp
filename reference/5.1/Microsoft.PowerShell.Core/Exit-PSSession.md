---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 02/08/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/exit-pssession?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Exit-PSSession
ms.openlocfilehash: 1e0c8413a37a9b8877b6af9089ac311459ada20d
ms.sourcegitcommit: 3a1d80e27438976101f216b8c3d623c61b868db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2021
ms.locfileid: "99975109"
---
# Exit-PSSession

## 構文
リモート コンピューターとの対話型セッションを終了します。

## SYNTAX

```
Exit-PSSession [<CommonParameters>]
```

## 説明

コマンドレットは `Exit-PSSession` 、コマンドレットを使用して開始した対話型セッションを終了し `Enter-PSSession` ます。

また、キーワードを使用して、 `exit` 対話型セッションを終了することもできます。 効果は、を使用する場合と同じです `Exit-PSSession` 。

## 例

### 例 1: 対話型セッションを開始および停止する

```powershell
PS C:\> Enter-PSSession -ComputerName Server01
Server01\PS> Exit-PSSession
PS C:\>
```

これらのコマンドは、Server01 リモート コンピューターで対話型セッションを開始および停止します。

### 例 2: PSSession オブジェクトを使用して対話型セッションを開始および停止する

```powershell
PS C:\> $s = New-PSSession -ComputerName Server01
PS C:\> Enter-PSSession -Session $s
Server01\PS> Exit-PSSession
PS C:\> $s
Id Name            ComputerName    State    ConfigurationName
-- ----            ------------    -----    -----------------
1  Session1        Server01        Opened   Microsoft.PowerShell
```

これらのコマンドは、Windows PowerShell セッション (**PSSession**) を使用する Server01 コンピューターとの対話型セッションを開始および停止します。

対話型セッションは Windows PowerShell セッションを使用して開始されたため、対話型セッションが終了しても **PSSession** は引き続き利用できます。 _ComputerName_ パラメーターを使用すると、 `Enter-PSSession` 対話型セッションが終了したときに閉じられる一時的なセッションがによって作成されます。

最初のコマンドは、コマンドレットを使用して、 `New-PSSession` Server01 コンピューター上に **PSSession** を作成します。 このコマンドは、 **PSSession** を変数に保存し `$s` ます。

2番目のコマンドは、を使用し `Enter-PSSession` て、の **PSSession** を使用して対話型セッションを開始し `$s` ます。

3番目のコマンドは、を使用し `Exit-PSSession` て対話型セッションを停止します。

最後のコマンドは、変数内の **PSSession** を表示し `$s` ます。 **State** プロパティは、 **PSSession** がまだ開いており、使用可能であることを示しています。

### 例 3: Exit キーワードを使用してセッションを停止する

```powershell
PS C:\> Enter-PSSession -ComputerName Server01
Server01\PS> exit
PS C:\>
```

この例では、キーワードを使用して、 `exit` を使用して開始された対話型セッションを停止し `Enter-PSSession` ます。 キーワードは、 `exit` を使用する場合と同じ効果があり `Exit-PSSession` ます。

## パラメーター

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## ノート

このコマンドレットは、共通パラメーターのみを受け取ります。

## 関連リンク

[Connect-PSSession](Connect-PSSession.md)

[Disconnect-PSSession](Disconnect-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)

[Receive-PSSession](Receive-PSSession.md)

[Remove-PSSession](Remove-PSSession.md)
