---
description: 関数について説明 `Prompt` し、カスタム関数を作成する方法を示し `Prompt` ます。
Locale: en-US
ms.date: 04/15/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_prompts?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Prompts
ms.openlocfilehash: 3887df636c3ad486a4dbe72fffdc8acd92d2b3e9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599287"
---
# <a name="about-prompts"></a>プロンプトについて

## <a name="short-description"></a>簡単な説明
関数について説明 `Prompt` し、カスタム関数を作成する方法を示し `Prompt` ます。

## <a name="long-description"></a>長い説明

Powershell コマンドプロンプトで、PowerShell でコマンドを実行する準備ができたことが示されます。

```
PS C:\>
```

PowerShell プロンプトは、組み込み関数によって決定され `Prompt` ます。 独自の関数を作成 `Prompt` して PowerShell プロファイルに保存することで、プロンプトをカスタマイズできます。

## <a name="about-the-prompt-function"></a>Prompt 関数について

関数は、 `Prompt` PowerShell プロンプトの外観を決定します。
PowerShell には組み込み関数が用意さ `Prompt` れていますが、独自の関数を定義することでオーバーライドでき `Prompt` ます。

`Prompt`関数の構文は次のとおりです。

```powershell
function Prompt { <function-body> }
```

関数は、 `Prompt` オブジェクトを返す必要があります。 文字列として書式設定された文字列またはオブジェクトを返すことをお勧めします。 推奨される最大長は80文字です。

たとえば、次の関数は、 `Prompt` "Hello, World" という文字列と、その後に右山かっこ () を返し `>` ます。

```powershell
PS C:\> function prompt {"Hello, World > "}
Hello, World >
```

### <a name="getting-the-prompt-function"></a>Prompt 関数を取得する

関数を取得するに `Prompt` は、 `Get-Command` コマンドレットを使用するか、 `Get-Item` 関数ドライブのコマンドレットを使用します。

次に例を示します。

```powershell
PS C:\> Get-Command Prompt

CommandType     Name      ModuleName
-----------     ----      ----------
Function        prompt
```

プロンプトの値を設定するスクリプトを取得するには、ドットメソッドを使用して関数の **ScriptBlock** プロパティを取得し `Prompt` ます。

次に例を示します。

```powershell
(Get-Command Prompt).ScriptBlock
```

```Output
"PS $($executionContext.SessionState.Path.CurrentLocation)$('>' * ($nestedPromptLevel + 1)) "
# .Link
# https://go.microsoft.com/fwlink/?LinkID=225750
# .ExternalHelp System.Management.Automation.dll-help.xml
```

すべての関数と同様に、 `Prompt` 関数はドライブに格納され `Function:` ます。
現在の関数を作成するスクリプトを表示するには `Prompt` 、次のように入力します。

```powershell
(Get-Item function:prompt).ScriptBlock
```

### <a name="the-default-prompt"></a>既定のプロンプト

既定のプロンプトは、関数が `Prompt` エラーを生成した場合、またはオブジェクトを返さない場合にのみ表示されます。

既定の PowerShell プロンプトは次のとおりです。

```
PS>
```

たとえば、次のコマンドは関数を `Prompt` に設定し `$null` ますが、これは無効です。 その結果、既定のプロンプトが表示されます。

```powershell
PS C:\> function prompt {$null}
PS>
```

PowerShell には組み込みのプロンプトが付属しているため、通常、既定のプロンプトは表示されません。

### <a name="built-in-prompt"></a>組み込みのプロンプト

PowerShell には組み込み関数が含まれてい `Prompt` ます。

```powershell
function prompt {
    $(if (Test-Path variable:/PSDebugContext) { '[DBG]: ' }
      else { '' }) + 'PS ' + $(Get-Location) +
        $(if ($NestedPromptLevel -ge 1) { '>>' }) + '> '
}
```

関数は、コマンドレットを使用して、 `Test-Path` `$PSDebugContext` 自動変数が設定されているかどうかを確認します。 が設定されている場合は、デバッグモードになり、次のように `$PSDebugContext` `[DBG]:` プロンプトに追加されます。

```Output
[DBG]: PS C:\ps-test>
```

`$PSDebugContext`が設定されていない場合、関数は `PS` プロンプトにを追加します。
また、関数は、コマンドレットを使用して、 `Get-Location` 現在のファイルシステムのディレクトリの場所を取得します。 次に、右山かっこ () を追加し `>` ます。

次に例を示します。

```Output
PS C:\ps-test>
```

入れ子になったプロンプトでは、関数によって2つの山かっこ () がプロンプトに追加され `>>` ます。 (自動変数の値が1より大きい場合は、入れ子になったプロンプトが表示され `$NestedPromptLevel` ます)。

たとえば、入れ子になったプロンプトでデバッグする場合、プロンプトは次のように表示されます。

```Output
[DBG] PS C:\ps-test>>>
```

### <a name="changes-to-the-prompt"></a>プロンプトの変更

`Enter-PSSession`コマンドレットは、リモートコンピューターの名前を現在の関数に付加し `Prompt` ます。 コマンドレットを使用し `Enter-PSSession` てリモートコンピューターとのセッションを開始すると、コマンドプロンプトが変更され、リモートコンピューターの名前が表示されます。 次に例を示します。

```Output
PS Hello, World> Enter-PSSession Server01
[Server01]: PS Hello, World>
```

その他の PowerShell ホストアプリケーションと代替シェルには、独自のカスタムコマンドプロンプトが含まれている場合があります。

と自動変数の詳細については `$PSDebugContext` `$NestedPromptLevel` 、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

### <a name="how-to-customize-the-prompt"></a>プロンプトをカスタマイズする方法

プロンプトをカスタマイズするには、新しい関数を記述し `Prompt` ます。 関数は保護されていないため、上書きできます。

関数を記述するに `Prompt` は、次のように入力します。

```powershell
function prompt { }
```

次に、中かっこの間に、プロンプトを作成するコマンドまたは文字列を入力します。

たとえば、次のプロンプトにはコンピューター名が含まれています。

```powershell
function prompt {"PS [$env:COMPUTERNAME]> "}
```

Server01 コンピューターでは、プロンプトは次のようになります。

```Output
PS [Server01] >
```

次の `Prompt` 関数には、現在の日付と時刻が含まれています。

```powershell
function prompt {"$(Get-Date)> "}
```

プロンプトは次のようになります。

```Output
03/15/2012 17:49:47>
```

既定の関数を変更することもでき `Prompt` ます。

たとえば、次の変更された関数は、 `Prompt` `[ADMIN]:` [ **管理者として実行** ] オプションを使用して powershell を開いたときに、組み込みの powershell プロンプトにを追加します。

```powershell
function prompt {
  $identity = [Security.Principal.WindowsIdentity]::GetCurrent()
  $principal = [Security.Principal.WindowsPrincipal] $identity
  $adminRole = [Security.Principal.WindowsBuiltInRole]::Administrator

  $(if (Test-Path variable:/PSDebugContext) { '[DBG]: ' }
    elseif($principal.IsInRole($adminRole)) { "[ADMIN]: " }
    else { '' }
  ) + 'PS ' + $(Get-Location) +
    $(if ($NestedPromptLevel -ge 1) { '>>' }) + '> '
}
```

[ **管理者として実行** ] オプションを使用して PowerShell を起動すると、次のようなプロンプトが表示されます。

```Output
[ADMIN]: PS C:\ps-test>
```

次の関数は、次の `Prompt` コマンドの履歴 ID を表示します。 コマンドの履歴を表示するには、コマンドレットを使用し `Get-History` ます。

```powershell
function prompt {
   # The at sign creates an array in case only one history item exists.
   $history = @(Get-History)
   if($history.Count -gt 0)
   {
      $lastItem = $history[$history.Count - 1]
      $lastId = $lastItem.Id
   }

   $nextCommand = $lastId + 1
   $currentDirectory = Get-Location
   "PS: $nextCommand $currentDirectory >"
}
```

次のプロンプトでは、 `Write-Host` コマンドレットとコマンドレットを使用して、 `Get-Random` 色をランダムに変更するプロンプトを作成します。 `Write-Host`は現在のホストアプリケーションに書き込みますが、オブジェクトを返しません。この関数には、ステートメントが含まれてい `Return` ます。 使用しない場合、PowerShell では既定のプロンプトであるを使用し `PS>` ます。

```powershell
function prompt {
    $color = Get-Random -Min 1 -Max 16
    Write-Host ("PS " + $(Get-Location) +">") -NoNewLine `
     -ForegroundColor $Color
    return " "
}
```

### <a name="saving-the-prompt-function"></a>Prompt 関数を保存しています

任意の関数と同様に、 `Prompt` 関数は現在のセッションにのみ存在します。 `Prompt`今後のセッションのために関数を保存するには、PowerShell プロファイルに追加します。 プロファイルの詳細については、「[about_Profiles](about_Profiles.md)」を参照してください。

## <a name="see-also"></a>関連項目

[Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)

[Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)

[Get-History](xref:Microsoft.PowerShell.Core.Get-History)

[Get-Random](xref:Microsoft.PowerShell.Utility.Get-Random)

[Write-Host](xref:Microsoft.PowerShell.Utility.Write-Host)

[about_Profiles](about_Profiles.md)

[about_Functions](about_Functions.md)

[about_Scopes](about_Scopes.md)

[about_Debuggers](about_Debuggers.md)

[about_Automatic_Variables](about_Automatic_Variables.md)

