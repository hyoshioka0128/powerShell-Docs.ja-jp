---
ms.date: 12/31/2019
title: ISEMenuItemCollection オブジェクト
description: ISEMenuItemCollection オブジェクトは、ISEMenuItem オブジェクトのコレクションです。
ms.openlocfilehash: cd86768d13b1326a8f35c44f0391ab60669cee4f
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92655998"
---
# <a name="the-isemenuitemcollection-object"></a>ISEMenuItemCollection オブジェクト

**ISEMenuItemCollection** オブジェクトは、 **ISEMenuItem** オブジェクトのコレクションです。 これは **Microsoft.PowerShell.Host.ISE.ISEMenuItemCollection** クラスのインスタンスです。 例としては、Windows PowerShell&reg; Integrated Scripting Environment (ISE) の **[アドオン]** メニューをカスタマイズするために使用される `$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus` オブジェクトです。

## <a name="method"></a>方法

### <a name="addstring-displayname-systemmanagementautomationscriptblock-action-systemwindowsinputkeygesture-shortcut-"></a>Add\(string DisplayName, System.Management.Automation.ScriptBlock Action, System.Windows.Input.KeyGesture Shortcut \)

Windows PowerShell ISE 2.0 以降でサポートされています。

メニュー項目をコレクションに追加します。

**DisplayName** 追加するメニューの表示名。

**Action** このメニュー項目に関連付けられるアクションを指定する **System.Management.Automation.ScriptBlock** オブジェクト。

**Shortcut** アクションのキーボード ショートカット。

**Returns** 追加した **ISEMenuItem** オブジェクト。

```powershell
# Create an Add-ons menu with an fast access key and a shortcut.
# Note the use of "_"  as opposed to the "&" for mapping to the fast access key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Add('_Process', {Get-Process}, 'Alt+P')
```

### <a name="clear"></a>Clear\(\)

Windows PowerShell ISE 2.0 以降でサポートされています。

メニュー項目からすべてのサブメニューを削除します。

```powershell
# Remove all custom submenu items from the AddOns menu
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()
```

## <a name="see-also"></a>参照

- [ISEMenuItem オブジェクト](The-ISEMenuItem-Object.md)
- [Windows PowerShell ISE スクリプト オブジェクト モデルの目的](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [ISE オブジェクト モデルの階層](The-ISE-Object-Model-Hierarchy.md)
