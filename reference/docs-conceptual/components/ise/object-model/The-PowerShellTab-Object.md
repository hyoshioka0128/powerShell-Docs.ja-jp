---
ms.date: 06/05/2017
keywords: PowerShell, コマンドレット
title: PowerShellTab オブジェクト
ms.assetid: a9b58556-951b-4f48-b3ae-b351b7564360
ms.openlocfilehash: 577e2aaaddf3071801816d9ae91dbf0006dd5072
ms.sourcegitcommit: 00ff76d7d9414fe585c04740b739b9cf14d711e1
ms.translationtype: MTE95
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2018
ms.locfileid: "53402445"
---
# <a name="the-powershelltab-object"></a><span data-ttu-id="bf927-103">PowerShellTab オブジェクト</span><span class="sxs-lookup"><span data-stu-id="bf927-103">The PowerShellTab Object</span></span>

<span data-ttu-id="bf927-104">**PowerShellTab** オブジェクトは、Windows PowerShell ランタイム環境を表します。</span><span class="sxs-lookup"><span data-stu-id="bf927-104">The **PowerShellTab** object represents a Windows PowerShell runtime environment.</span></span>

## <a name="methods"></a><span data-ttu-id="bf927-105">メソッド</span><span class="sxs-lookup"><span data-stu-id="bf927-105">Methods</span></span>

### <a name="invoke-script-"></a><span data-ttu-id="bf927-106">Invoke\( Script \)</span><span class="sxs-lookup"><span data-stu-id="bf927-106">Invoke\( Script \)</span></span>

<span data-ttu-id="bf927-107">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-107">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-108">指定したスクリプトを [PowerShell] タブで実行します。</span><span class="sxs-lookup"><span data-stu-id="bf927-108">Runs the given script in the PowerShell tab.</span></span>

> [!NOTE]
> <span data-ttu-id="bf927-109">このメソッドは、その他の [PowerShell] タブでのみ機能し、実行元となる [PowerShell] タブでは機能しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-109">This method only works on other PowerShell tabs, not the PowerShell tab from which it is run.</span></span> <span data-ttu-id="bf927-110">このコマンドはオブジェクトや値を返しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-110">It does not return any object or value.</span></span> <span data-ttu-id="bf927-111">このコードによって変数が変更されると、その変更はコマンドが呼び出されたタブで保持されます。</span><span class="sxs-lookup"><span data-stu-id="bf927-111">If the code modifies any variable, then those changes persist on the tab against which the command was invoked.</span></span>

<span data-ttu-id="bf927-112">**Script** - System.Management.Automation.ScriptBlock または文字列。実行するスクリプト ブロック。</span><span class="sxs-lookup"><span data-stu-id="bf927-112">**Script** - System.Management.Automation.ScriptBlock or String The script block to run.</span></span>

```powershell
# Manually create a second PowerShell tab before running this script.
# Return to the first PowerShell tab and type the following command
$psISE.PowerShellTabs[1].Invoke({dir})
```

### <a name="invokesynchronous-script-usenewscope-millisecondstimeout-"></a><span data-ttu-id="bf927-113">InvokeSynchronous\( Script, \[useNewScope\], millisecondsTimeout \)</span><span class="sxs-lookup"><span data-stu-id="bf927-113">InvokeSynchronous\( Script, \[useNewScope\], millisecondsTimeout \)</span></span>

<span data-ttu-id="bf927-114">Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-114">Supported in Windows PowerShell ISE 3.0 and later, and not present in earlier versions.</span></span>

<span data-ttu-id="bf927-115">指定したスクリプトを [PowerShell] タブで実行します。</span><span class="sxs-lookup"><span data-stu-id="bf927-115">Runs the given script in the PowerShell tab.</span></span>

> [!NOTE]
> <span data-ttu-id="bf927-116">このメソッドは、その他の [PowerShell] タブでのみ機能し、実行元となる [PowerShell] タブでは機能しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-116">This method only works on other PowerShell tabs, not the PowerShell tab from which it is run.</span></span> <span data-ttu-id="bf927-117">スクリプト ブロックが実行され、スクリプトから返される値はコマンドを呼び出した実行環境に返されます。</span><span class="sxs-lookup"><span data-stu-id="bf927-117">The script block is run and any value that is returned from the script is returned to the run environment from which you invoked the command.</span></span> <span data-ttu-id="bf927-118">コマンドの実行よりも長くかかる場合、 **millesecondsTimeout**値を指定し、コマンドは、例外で失敗します。操作がタイムアウトしました</span><span class="sxs-lookup"><span data-stu-id="bf927-118">If the command takes longer to run than the **millesecondsTimeout** value specifies, then the command fails with an exception: "The operation has timed out."</span></span>

<span data-ttu-id="bf927-119">**Script** - System.Management.Automation.ScriptBlock または文字列。実行するスクリプト ブロック。</span><span class="sxs-lookup"><span data-stu-id="bf927-119">**Script** - System.Management.Automation.ScriptBlock or String The script block to run.</span></span>

<span data-ttu-id="bf927-120">**\[useNewScope\]** - 省略可能なブール値で、既定は **$true** です。**$true** に設定されると、そのコマンドを実行する新しいスコープが作成されます。</span><span class="sxs-lookup"><span data-stu-id="bf927-120">**\[useNewScope\]** -  Optional Boolean that defaults to **$true** If set to **$true**, then a new scope is created within which to run the command.</span></span> <span data-ttu-id="bf927-121">コマンドで指定されている [PowerShell] タブのランタイム環境は変更されません。</span><span class="sxs-lookup"><span data-stu-id="bf927-121">It does not modify the runtime environment of the PowerShell tab that is specified by the command.</span></span>

<span data-ttu-id="bf927-122">**\[millisecondsTimeout\]** - **500** を既定値とする省略可能な整数。</span><span class="sxs-lookup"><span data-stu-id="bf927-122">**\[millisecondsTimeout\]** -  Optional integer that defaults to **500**.</span></span>
<span data-ttu-id="bf927-123">指定した時間内にコマンドが完了しない場合、コマンドによって **TimeoutException** が生成され、"処理がタイムアウトになりました。" というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bf927-123">If the command does not finish within the specified time, then the command generates a **TimeoutException** with the message "The operation has timed out."</span></span>

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

## <a name="properties"></a><span data-ttu-id="bf927-124">プロパティ</span><span class="sxs-lookup"><span data-stu-id="bf927-124">Properties</span></span>

### <a name="addonsmenu"></a><span data-ttu-id="bf927-125">AddOnsMenu</span><span class="sxs-lookup"><span data-stu-id="bf927-125">AddOnsMenu</span></span>

<span data-ttu-id="bf927-126">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-126">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-127">[PowerShell] タブのアドオン メニューを取得する読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-127">The read-only property that gets the Add-ons menu for the PowerShell tab.</span></span>

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

### <a name="caninvoke"></a><span data-ttu-id="bf927-128">CanInvoke</span><span class="sxs-lookup"><span data-stu-id="bf927-128">CanInvoke</span></span>

<span data-ttu-id="bf927-129">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-129">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-130">スクリプトを [Invoke( Script )](#invoke-script-) メソッドで呼び出すことが可能な場合は **$true** を返す、読み取り専用のブール型プロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-130">The read-only Boolean property that returns a **$true** value if a script can be invoked with the [Invoke( Script )](#invoke-script-) method.</span></span>

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

### <a name="consolepane"></a><span data-ttu-id="bf927-131">Consolepane</span><span class="sxs-lookup"><span data-stu-id="bf927-131">Consolepane</span></span>

<span data-ttu-id="bf927-132">Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-132">Supported in Windows PowerShell ISE 3.0 and later, and not present in earlier versions.</span></span>  <span data-ttu-id="bf927-133">Windows PowerShell ISE 2.0 では、このパラメータの名前は **CommandPane** でした。</span><span class="sxs-lookup"><span data-stu-id="bf927-133">In Windows PowerShell ISE 2.0 this was named **CommandPane**.</span></span>

<span data-ttu-id="bf927-134">コンソール ウィンドウの [editor](The-ISEEditor-Object.md) オブジェクトを取得する読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-134">The read-only property that gets the Console pane [editor](The-ISEEditor-Object.md) object.</span></span>

```powershell
# Gets the Console Pane editor.
$psISE.CurrentPowerShellTab.ConsolePane
```

### <a name="displayname"></a><span data-ttu-id="bf927-135">表示名</span><span class="sxs-lookup"><span data-stu-id="bf927-135">DisplayName</span></span>

<span data-ttu-id="bf927-136">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-136">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-137">[PowerShell] タブに表示されるテキストを取得または設定する読み取り/書き込みプロパティです。既定では、タブ名は "PowerShell #" となり、「#」には番号が入ります。</span><span class="sxs-lookup"><span data-stu-id="bf927-137">The read-write property that gets or sets the text that is displayed on the PowerShell tab. By default, tabs are named "PowerShell #", where the # represents a number.</span></span>

```powershell
$newTab = $psISE.PowerShellTabs.Add()
# Change the DisplayName of the new PowerShell tab.
$newTab.DisplayName = 'Brand New Tab'
```

### <a name="expandedscript"></a><span data-ttu-id="bf927-138">ExpandedScript</span><span class="sxs-lookup"><span data-stu-id="bf927-138">ExpandedScript</span></span>

<span data-ttu-id="bf927-139">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-139">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-140">スクリプト ウィンドウが展開されているか非表示かどうかを特定する読み取り/書き込みブール型プロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-140">The read-write Boolean property that determines whether the Script pane is expanded or hidden.</span></span>

```powershell
# Toggle the expanded script property to see its effect.
$psISE.CurrentPowerShellTab.ExpandedScript = !$psISE.CurrentPowerShellTab.ExpandedScript
```

### <a name="files"></a><span data-ttu-id="bf927-141">ファイル</span><span class="sxs-lookup"><span data-stu-id="bf927-141">Files</span></span>

<span data-ttu-id="bf927-142">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-142">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-143">[PowerShell] タブで開かれている[スクリプト ファイルのコレクション](The-ISEFileCollection-Object.md)を取得する読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-143">The read-only property that gets the [collection of script files](The-ISEFileCollection-Object.md) that are open in the PowerShell tab.</span></span>

```powershell
$newFile = $psISE.CurrentPowerShellTab.Files.Add()
$newFile.Editor.Text = "a`r`nb"
# Gets the line count
$newFile.Editor.LineCount
```

### <a name="output"></a><span data-ttu-id="bf927-144">出力</span><span class="sxs-lookup"><span data-stu-id="bf927-144">Output</span></span>

<span data-ttu-id="bf927-145">この機能は、Windows PowerShell ISE 2.0 に存在しますが、それよりも後のバージョンの ISE では削除されているか、名前が変更されています。</span><span class="sxs-lookup"><span data-stu-id="bf927-145">This feature is present in Windows PowerShell ISE 2.0, but was removed or renamed in later versions of the ISE.</span></span>  <span data-ttu-id="bf927-146">Windows PowerShell ISE 2.0 より後のバージョンでは、**ConsolePane** オブジェクトを同じ目的で使用できます。</span><span class="sxs-lookup"><span data-stu-id="bf927-146">In later versions of Windows PowerShell ISE, you can use the **ConsolePane** object for the same purposes.</span></span>

<span data-ttu-id="bf927-147">現在の[エディター](The-ISEEditor-Object.md)の出力ウィンドウが取得する読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-147">The read-only property that gets the Output pane of the current [editor](The-ISEEditor-Object.md).</span></span>

```powershell
# Clears the text in the Output pane.
$psISE.CurrentPowerShellTab.output.clear()
```

### <a name="prompt"></a><span data-ttu-id="bf927-148">ダイアログを表示する</span><span class="sxs-lookup"><span data-stu-id="bf927-148">Prompt</span></span>

<span data-ttu-id="bf927-149">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-149">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-150">現在のプロンプト テキストを取得する読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-150">The read-only property that gets the current prompt text.</span></span> <span data-ttu-id="bf927-151">注: **Prompt** 関数は、ユーザーのプロファイルでオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="bf927-151">Note: the **Prompt** function can be overridden by the user'™s profile.</span></span> <span data-ttu-id="bf927-152">結果が単純な文字列以外の場合、このプロパティは何も返しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-152">If the result is other than a simple string, then this property returns nothing.</span></span>

```powershell
# Gets the current prompt text.
$psISE.CurrentPowerShellTab.Prompt
```

### <a name="showcommands"></a><span data-ttu-id="bf927-153">ShowCommands</span><span class="sxs-lookup"><span data-stu-id="bf927-153">ShowCommands</span></span>

<span data-ttu-id="bf927-154">Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-154">Supported in Windows PowerShell ISE 3.0 and later, and not present in earlier versions.</span></span>

<span data-ttu-id="bf927-155">コマンド ウィンドウが現在表示されているかどうかを示す読み取り/書き込みプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-155">The read-write property that indicates if the Commands pane is currently displayed.</span></span>

```powershell
# Gets the current status of the Commands pane and stores it in the $a variable
$a = $psISE.CurrentPowerShellTab.ShowCommands
# if $a is $false, then turn the Commands pane on by changing the value to $true
if (!$a) {$psISE.CurrentPowerShellTab.ShowCommands = $true}
```

### <a name="statustext"></a><span data-ttu-id="bf927-156">StatusText</span><span class="sxs-lookup"><span data-stu-id="bf927-156">StatusText</span></span>

<span data-ttu-id="bf927-157">Windows PowerShell ISE 2.0 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="bf927-157">Supported in Windows PowerShell ISE 2.0 and later.</span></span>

<span data-ttu-id="bf927-158">**PowerShellTab** ステータス テキストを取得する読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-158">The read-only property that gets the **PowerShellTab** status text.</span></span>

```powershell
# Gets the current status text,
$psISE.CurrentPowerShellTab.StatusText
```

### <a name="horizontaladdontoolspaneopened"></a><span data-ttu-id="bf927-159">HorizontalAddOnToolsPaneOpened</span><span class="sxs-lookup"><span data-stu-id="bf927-159">HorizontalAddOnToolsPaneOpened</span></span>

<span data-ttu-id="bf927-160">Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-160">Supported in Windows PowerShell ISE 3.0 and later, and not present in earlier versions.</span></span>

<span data-ttu-id="bf927-161">水平方向のアドオン ツール ウィンドウが現在開いているかどうかを示す読み取り 専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-161">The read-only property that indicates whether the horizontal Add-Ons tool pane is currently open.</span></span>

```powershell
# Gets the current state of the horizontal Add-ons tool pane.
$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened
```

### <a name="verticaladdontoolspaneopened"></a><span data-ttu-id="bf927-162">VerticalAddOnToolsPaneOpened</span><span class="sxs-lookup"><span data-stu-id="bf927-162">VerticalAddOnToolsPaneOpened</span></span>

<span data-ttu-id="bf927-163">Windows PowerShell ISE 3.0 以降でサポートされており、それよりも前のバージョンには存在しません。</span><span class="sxs-lookup"><span data-stu-id="bf927-163">Supported in Windows PowerShell ISE 3.0 and later, and not present in earlier versions.</span></span>

<span data-ttu-id="bf927-164">垂直方向のアドオン ツール ウィンドウが現在開いているかどうかを示す読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bf927-164">The read-only property that indicates whether the vertical Add-Ons tool pane is currently open.</span></span>

```powershell
# Turns on the Commands pane
$psISE.CurrentPowerShellTab.ShowCommands = $true
# Gets the current state of the vertical Add-ons tool pane.
$psISE.CurrentPowerShellTab.HorizontalAddOnToolsPaneOpened
```

## <a name="see-also"></a><span data-ttu-id="bf927-165">参照</span><span class="sxs-lookup"><span data-stu-id="bf927-165">See Also</span></span>

- [<span data-ttu-id="bf927-166">PowerShellTabCollection オブジェクト</span><span class="sxs-lookup"><span data-stu-id="bf927-166">The PowerShellTabCollection Object</span></span>](The-PowerShellTabCollection-Object.md)
- [<span data-ttu-id="bf927-167">Windows PowerShell ISE スクリプト オブジェクト モデルの目的</span><span class="sxs-lookup"><span data-stu-id="bf927-167">Purpose of the Windows PowerShell ISE Scripting Object Model</span></span>](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [<span data-ttu-id="bf927-168">ISE オブジェクト モデルの階層</span><span class="sxs-lookup"><span data-stu-id="bf927-168">The ISE Object Model Hierarchy</span></span>](The-ISE-Object-Model-Hierarchy.md)