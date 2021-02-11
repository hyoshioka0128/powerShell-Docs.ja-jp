---
ms.date: 06/05/2017
title: PowerShellTab オブジェクト
description: PowerShellTab オブジェクトは、Windows PowerShell ランタイム環境を表します。
ms.openlocfilehash: ac89875e408a41a92d7e3d1a83a849466296c3c6
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663387"
---
# <a name="the-powershelltab-object"></a>PowerShellTab オブジェクト

**PowerShellTab** オブジェクトは、Windows PowerShell ランタイム環境を表します。

## <a name="methods"></a>メソッド

### <a name="invoke-script-"></a>Invoke\( Script \)

Windows PowerShell ISE 2.0 以降でサポートされています。

指定したスクリプトを [PowerShell] タブで実行します。

> [!NOTE]
> このメソッドは、その他の [PowerShell] タブでのみ機能し、実行元となる [PowerShell] タブでは機能しません。 このコマンドはオブジェクトや値を返しません。 このコードによって変数が変更されると、その変更はコマンドが呼び出されたタブで保持されます。

**Script** - System.Management.Automation.ScriptBlock または文字列。実行するスクリプト ブロック。

```powershell
# Manually create a second PowerShell tab before running this script.
# Return to the first PowerShell tab and type the following command
$psISE.PowerShellTabs[1].Invoke({dir})
```

### <a name="invokesynchronous-script-usenewscope-millisecondstimeout-"></a>InvokeSynchronous\( Script, \[useNewScope\], millisecondsTimeout \)

Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。

指定したスクリプトを [PowerShell] タブで実行します。

> [!NOTE]
> このメソッドは、その他の [PowerShell] タブでのみ機能し、実行元となる [PowerShell] タブでは機能しません。 スクリプト ブロックが実行され、スクリプトから返される値はコマンドを呼び出した実行環境に返されます。 コマンドが **millesecondsTimeout** の値で指定した時間内に完了しない場合、コマンドが失敗して次の例外が発生します。"操作がタイムアウトしました"。

**Script** - System.Management.Automation.ScriptBlock または文字列。実行するスクリプト ブロック。

**\[useNewScope\]** - 省略可能なブール値で、既定は `$true` です。`$true` に設定されると、そのコマンドを実行する新しいスコープが作成されます。 コマンドで指定されている [PowerShell] タブのランタイム環境は変更されません。

**\[millisecondsTimeout\]** - **500** を既定値とする省略可能な整数。
指定した時間内にコマンドが完了しない場合、コマンドによって **TimeoutException** が生成され、"処理がタイムアウトになりました。" というメッセージが表示されます。

```powershell
# Create a new PowerShell tab and then switch back to the first
$psISE.PowerShellTabs.Add()
$psISE.PowerShellTabs.SetSelectedPowerShellTab($psISE.PowerShellTabs[0])

# Invoke a simple command on the other tab, in its own scope
$psISE.PowerShellTabs[1].InvokeSynchronous('$x=1', $false)
# You can switch to the other tab and type '$x' to see that the value is saved there.

# This example sets a value in the other tab (in a different scope)
# and returns it through the pipeline to this tab to store in $a
$a = $psISE.PowerShellTabs[1].InvokeSynchronous('$z=3;$z')
$a

# This example runs a command that takes longer than the allowed timeout value
# and measures how long it runs so that you can see the impact
Measure-Command {$psISE.PowerShellTabs[1].InvokeSynchronous('sleep 10', $false, 5000)}
```

## <a name="properties"></a>Properties

### <a name="addonsmenu"></a>AddOnsMenu

Windows PowerShell ISE 2.0 以降でサポートされています。

[PowerShell] タブのアドオン メニューを取得する読み取り専用のプロパティです。

```powershell
# Clear the Add-ons menu if one exists.
$psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Clear()
# Create an AddOns menu with an accessor.
# Note the use of "_"  as opposed to the "&" for mapping to the fast key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add('_Process', {Get-Process}, 'Alt+P')
# Add a nested menu.
$parentAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add('Parent', $null, $null)
$parentAdded.SubMenus.Add('_Dir', {dir}, 'Alt+D')
# Show the Add-ons menu on the current PowerShell tab.
$psISE.CurrentPowerShellTab.AddOnsMenu
```

### <a name="caninvoke"></a>CanInvoke

Windows PowerShell ISE 2.0 以降でサポートされています。

スクリプトを [Invoke( Script )](#invoke-script-) メソッドで呼び出すことが可能な場合は `$true` を返す、読み取り専用のブール型プロパティです。

```powershell
# CanInvoke will be false if the PowerShell
# tab is running a script that takes a while, and you
# check its properties from another PowerShell tab. It is
# always false if checked on the current PowerShell tab.
# Manually create a second PowerShell tab before running this script.
# Return to the first tab and type
$secondTab = $psISE.PowerShellTabs[1]
$secondTab.CanInvoke
$secondTab.Invoke({sleep 20})
$secondTab.CanInvoke
```

### <a name="consolepane"></a>ConsolePane

Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。 Windows PowerShell ISE 2.0 では、このパラメータの名前は **CommandPane** でした。

コンソール ウィンドウの [editor](The-ISEEditor-Object.md) オブジェクトを取得する読み取り専用のプロパティです。

```powershell
# Gets the Console Pane editor.
$psISE.CurrentPowerShellTab.ConsolePane
```

### <a name="displayname"></a>DisplayName

Windows PowerShell ISE 2.0 以降でサポートされています。

[PowerShell] タブに表示されるテキストを取得または設定する読み取り/書き込みプロパティです。既定では、タブ名は "PowerShell #" となり、「#」には番号が入ります。

```powershell
$newTab = $psISE.PowerShellTabs.Add()
# Change the DisplayName of the new PowerShell tab.
$newTab.DisplayName = 'Brand New Tab'
```

### <a name="expandedscript"></a>ExpandedScript

Windows PowerShell ISE 2.0 以降でサポートされています。

スクリプト ウィンドウが展開されているか非表示かどうかを特定する読み取り/書き込みブール型プロパティです。

```powershell
# Toggle the expanded script property to see its effect.
$psISE.CurrentPowerShellTab.ExpandedScript = !$psISE.CurrentPowerShellTab.ExpandedScript
```

### <a name="files"></a>ファイル

Windows PowerShell ISE 2.0 以降でサポートされています。

[PowerShell] タブで開かれている[スクリプト ファイルのコレクション](The-ISEFileCollection-Object.md)を取得する読み取り専用のプロパティです。

```powershell
$newFile = $psISE.CurrentPowerShellTab.Files.Add()
$newFile.Editor.Text = "a`r`nb"
# Gets the line count
$newFile.Editor.LineCount
```

### <a name="output"></a>出力

この機能は、Windows PowerShell ISE 2.0 に存在しますが、それよりも後のバージョンの ISE では削除されているか、名前が変更されています。 Windows PowerShell ISE 2.0 より後のバージョンでは、 **ConsolePane** オブジェクトを同じ目的で使用できます。

現在の[エディター](The-ISEEditor-Object.md)の出力ウィンドウが取得する読み取り専用のプロパティです。

```powershell
# Clears the text in the Output pane.
$psISE.CurrentPowerShellTab.output.clear()
```

### <a name="prompt"></a>Prompt

Windows PowerShell ISE 2.0 以降でサポートされています。

現在のプロンプト テキストを取得する読み取り専用のプロパティです。 注: **Prompt** 関数は、ユーザーのプロファイルでオーバーライドできます。 結果が単純な文字列以外の場合、このプロパティは何も返しません。

```powershell
# Gets the current prompt text.
$psISE.CurrentPowerShellTab.Prompt
```

### <a name="showcommands"></a>ShowCommands

Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。

コマンド ウィンドウが現在表示されているかどうかを示す読み取り/書き込みプロパティです。

```powershell
# Gets the current status of the Commands pane and stores it in the $a variable
$a = $psISE.CurrentPowerShellTab.ShowCommands
# if $a is $false, then turn the Commands pane on by changing the value to $true
if (!$a) {$psISE.CurrentPowerShellTab.ShowCommands = $true}
```

### <a name="statustext"></a>StatusText

Windows PowerShell ISE 2.0 以降でサポートされています。

**PowerShellTab** ステータス テキストを取得する読み取り専用のプロパティです。

```powershell
# Gets the current status text,
$psISE.CurrentPowerShellTab.StatusText
```

### <a name="horizontaladdontoolspaneopened"></a>HorizontalAddOnToolsPaneOpened

Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。

水平方向のアドオン ツール ウィンドウが現在開いているかどうかを示す読み取り専用のプロパティです。

```powershell
# Gets the current state of the horizontal Add-ons tool pane.
$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened
```

### <a name="verticaladdontoolspaneopened"></a>VerticalAddOnToolsPaneOpened

Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。

垂直方向のアドオン ツール ウィンドウが現在開いているかどうかを示す読み取り専用のプロパティです。

```powershell
# Turns on the Commands pane
$psISE.CurrentPowerShellTab.ShowCommands = $true
# Gets the current state of the vertical Add-ons tool pane.
$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened
```

## <a name="see-also"></a>参照

- [PowerShellTabCollection オブジェクト](The-PowerShellTabCollection-Object.md)
- [Windows PowerShell ISE スクリプト オブジェクト モデルの目的](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [ISE オブジェクト モデルの階層](The-ISE-Object-Model-Hierarchy.md)
