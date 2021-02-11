---
ms.date: 12/31/2019
title: ISEMenuItem オブジェクト
description: ISEMenuItem オブジェクトは Microsoft.PowerShell.Host.ISE.ISEMenuItem クラスのインスタンスです。 **[アドオン]** メニューにあるすべてのオブジェクトは、ISEMenuItem クラスのインスタンスです。
ms.openlocfilehash: 15036e3551687a21dfbe50834a89247c80949656
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663439"
---
# <a name="the-isemenuitem-object"></a>ISEMenuItem オブジェクト

**ISEMenuItem** オブジェクトは **Microsoft.PowerShell.Host.ISE.ISEMenuItem** クラスのインスタンスです。
**[アドオン]** メニューにあるすべてのオブジェクトは、 **Microsoft.PowerShell.Host.ISE.ISEMenuItem** クラスのインスタンスです。

## <a name="properties"></a>Properties

### <a name="displayname"></a>DisplayName

Windows PowerShell ISE 2.0 以降でサポートされています。

メニュー項目の名前を表示する読み取り専用プロパティ。

```powershell
# Get the display name of the Add-ons menu item
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Add('_Process', {Get-Process}, 'Alt+P')
$psISE.CurrentPowerShellTab.AddOnsMenu.DisplayName
```

### <a name="action"></a>アクション

Windows PowerShell ISE 2.0 以降でサポートされています。

スクリプトのブロックを取得する読み取り専用プロパティ。 メニュー項目をクリックすると、アクションが呼び出されます。

```powershell
# Get the action associated with the first submenu item.
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Add('_Process', {Get-Process}, 'Alt+P')
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus[0].Action

# Invoke the script associated with the first submenu item
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus[0].Action.Invoke()
```

### <a name="shortcut"></a>ショートカット

Windows PowerShell ISE 2.0 以降でサポートされています。

メニュー項目の Windows 入力用ショートカット キーを取得する読み取り専用プロパティ。

```powershell
# Get the shortcut for the first submenu item.
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Add('_Process', {Get-Process}, 'Alt+P')
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus[0].Shortcut
```

### <a name="submenus"></a>Submenus

Windows PowerShell ISE 2.0 以降でサポートされています。

メニュー項目の[サブメニューの一覧](The-ISEMenuItemCollection-Object.md)を取得する読み取り専用プロパティ。

```powershell
# List the submenus of the Add-ons menu
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Add('_Process', {Get-Process}, 'Alt+P')
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus
```

## <a name="scripting-example"></a>スクリプトの例

[アドオン] メニューとそのスクリプト可能なプロパティの使用をさらに理解するには、次のスクリプトの例に目を通してください。

```powershell
# This is a scripting example that shows the use of the Add-ons menu.
# Clear the Add-ons menu if any entries currently exist
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()

# Add an Add-ons menu item with an shortcut and fast access key.
# Note the use of "_"  as opposed to the "&" for mapping to the fast access key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add('_Process', {Get-Process}, 'Alt+P')
# Add a nested menu - a parent and a child submenu item.
$parentAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Add('Parent', $null, $null)
$parentAdded.SubMenus.Add('_Dir', {dir}, 'Alt+D')
```

## <a name="see-also"></a>参照

- [ISEMenuItemCollection オブジェクト](The-ISEMenuItemCollection-Object.md)
- [Windows PowerShell ISE スクリプト オブジェクト モデルの目的](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [ISE オブジェクト モデルの階層](The-ISE-Object-Model-Hierarchy.md)
