---
description: PowerShell コマンドプロンプトでコマンドを編集する方法について説明します。
Locale: en-US
ms.date: 07/10/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_line_editing?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Line_Editing
ms.openlocfilehash: 2b9a320b92bc0a61efc9f9ed75078b17cdd9cbc9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605432"
---
# <a name="about-line-editing"></a>行の編集について

## <a name="short-description"></a>簡単な説明

PowerShell コマンドプロンプトでコマンドを編集する方法について説明します。

## <a name="long-description"></a>長い説明

Powershell コンソールには、PowerShell コマンドプロンプトでコマンドを編集するための便利なショートカットキーがあります。

### <a name="add-a-line"></a>線を追加する

線を追加するには、Shift キーを押し<kbd>ながら</kbd> + <kbd>enter</kbd>キーを押します。

複数の行を追加できます。 追加の各行は `>>` 、継続のプロンプトで始まります。 <kbd>Enter</kbd>キーを押してコマンドを実行します。

### <a name="move-left-and-right"></a>左右に移動

カーソルを1文字左に移動するには、 <kbd>左矢印</kbd>キーを押します。

カーソルを1単語左に移動するには、 <kbd>ctrl</kbd> + <kbd>左方向</kbd>キーを押します。

カーソルを1文字右に移動するには、 <kbd>右矢印</kbd>キーを押します。

カーソルを1ワード右に移動するには、 <kbd>ctrl</kbd>キーを押しながら + <kbd>右方向</kbd>キーを押します。

### <a name="move-to-a-lines-beginning-or-end"></a>行の先頭または末尾に移動する

行の先頭に移動するには、[ <kbd>ホーム</kbd>] を押します。

行の末尾に移動するには、[ <kbd>終了</kbd>] をクリックします。

行が追加された場合は、 <kbd>Home</kbd> キーまたは <kbd>end</kbd> キーを2回押して、行の先頭または末尾に移動します。

### <a name="delete-characters"></a>文字の削除

カーソルの位置の背後にある文字を削除するには、 <kbd>Backspace</kbd>キーを押します。

カーソルの位置にある文字を削除するには、 <kbd>del キーを</kbd>押します。

### <a name="delete-characters-from-a-line"></a>行から文字を削除する

カーソルの位置から行末までのすべての文字を削除するには、 <kbd>ctrl</kbd> + <kbd>end</kbd>キーを押します。

カーソルの位置から行頭までのすべての文字を削除するには、 <kbd>ctrl</kbd> + <kbd>Home</kbd>キーを押します。

行が追加された場合、現在の行と追加された行から文字が削除されます。

### <a name="insert-and-overstrike-mode"></a>Insert および上書きモード

上書きモードに変更するには、[ <kbd>挿入</kbd>] をクリックします。 挿入モードに戻るには、もう一度 <kbd>insert</kbd> キーを押します。

### <a name="tab-completion"></a>Tab 補完機能

コマンドレット名、パラメーター、またはパスを完了するには、 <kbd>tab</kbd> キーを押します。 値の一覧をスクロールするには、 <kbd>tab</kbd> キーをもう一度押します。

## <a name="see-also"></a>関連項目

[about_Command_Syntax](about_Command_Syntax.md)

[about_Path_Syntax](about_Path_Syntax.md)

[about_PSReadline](../../PSReadline/About/about_PSReadline.md)

