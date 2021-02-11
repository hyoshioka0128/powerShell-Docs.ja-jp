---
ms.date: 06/12/2017
title: WMF 5.0 のアンインストール
description: この記事では、旧バージョンの Windows から WMF をアンインストールする方法について説明します。
ms.openlocfilehash: d8078ea918db2c1cf9a7ddd6ea8d1413b593c0ff
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92660808"
---
# <a name="uninstallation-instructions"></a>アンインストール手順

## <a name="using-command-prompt"></a>コマンド プロンプトを使用

1. **コマンド プロンプト** を開きます。
2. [Windows Update スタンドアロン ランチャー](https://support.microsoft.com/kb/934307)を次のように実行します。

Windows Server 2012 R2 および Windows 8.1:

```powershell
wusa /uninstall /kb:3134758
```

Windows Server 2012:

```powershell
wusa /uninstall /kb:3134759
```

Windows Server 2008 R2 SP1 および Windows 7 SP1:

```powershell
wusa /uninstall /kb:3134760
```

## <a name="using-control-panel"></a>コントロール パネルを使用

1. **[コントロール パネル]** を開きます。
2. **[プログラム]** を開き、 **[プログラムのアンインストール]** を開きます。
3. **[インストールされた更新プログラムを表示]** をクリックします。
4. インストールされた更新プログラムの一覧から **[Windows Management Framework 5.0]** を選択します。 これは *KB3134758* 、 *KB3134759* 、または *KB3134760* に対応しています。 **[アンインストール]** をクリックします。
