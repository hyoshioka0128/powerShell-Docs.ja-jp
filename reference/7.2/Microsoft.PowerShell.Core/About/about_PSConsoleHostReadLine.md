---
description: PowerShell がコンソールプロンプトで入力を読み取る方法をカスタマイズする方法について説明します。
Locale: en-US
ms.date: 01/04/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_psconsolehostreadline?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PSConsoleHostReadLine
ms.openlocfilehash: 2f69cd4c0c8f65f4a963eae561647d6de30a067e
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600606"
---
# <a name="about_psconsolehostreadline"></a>about_PSConsoleHostReadLine

## <a name="short-description"></a>概要
PowerShell がコンソールプロンプトで入力を読み取る方法をカスタマイズする方法について説明します。

## <a name="long-description"></a>詳細説明

Windows PowerShell 3.0 以降では、PSConsoleHostReadLine という名前の関数を記述できます。この関数は、コンソールの入力の既定の処理方法をオーバーライドします。

### <a name="examples"></a>例

次の例では、メモ帳を起動し、ユーザーが作成したテキストファイルから入力を取得します。

```powershell
function PSConsoleHostReadLine
{
  $inputFile = Join-Path $env:TEMP PSConsoleHostReadLine
  Set-Content $inputFile "PS > "

  # Notepad opens. Enter your command in it, save the file, and then exit.
  notepad $inputFile | Out-Null
  $userInput = Get-Content $inputFile
  $resultingCommand = $userInput.Replace("PS >", "")
  $resultingCommand
}
```

### <a name="remarks"></a>REMARKS

既定では、PowerShell はコンソールから入力を読み取ります。このモードでは、Windows コンソールサブシステムがすべての keypresses、F7 メニュー、およびその他の入力を処理します。 Enter キーまたは Tab キーを押すと、その時点までに入力したテキストが PowerShell によって取得されます。 Ctrl + R、Ctrl + A、Ctrl + E、またはその他のキーを押したことを、Enter キーまたは Tab キーを押す前に確認することはできません。Windows PowerShell 3.0 では、PSConsoleHostReadLine 関数によってこの問題が解決されます。 PowerShell コンソールホストで PSConsoleHostReadline という名前の関数を定義すると、PowerShell は "調理モード" 入力機構ではなく、その関数を呼び出します。

### <a name="see-also"></a>関連項目

[about_Prompts](about_Prompts.md)

