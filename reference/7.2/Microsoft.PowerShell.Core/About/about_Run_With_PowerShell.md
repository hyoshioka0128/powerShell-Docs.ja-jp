---
description: PowerShell を使用して実行する機能を使用して、ファイルシステムドライブからスクリプトを実行する方法について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_run_with_powershell?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Run_With_PowerShell
ms.openlocfilehash: 7070fa2bc8bb30e83f59e3d1193faa3a8495f109
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600611"
---
# <a name="about-run-with-powershell"></a>PowerShell を使用した実行について

## <a name="short-description"></a>概要
"PowerShell で実行" 機能を使用して、ファイルシステムドライブからスクリプトを実行する方法について説明します。

## <a name="long-description"></a>詳細説明

Windows PowerShell 3.0 以降では、"PowerShell で実行" 機能を使用して、Windows 8 および Windows Server 2012 のエクスプローラーと、以前のバージョンの Windows のエクスプローラーからスクリプトを実行できます。

"PowerShell を使用して実行" 機能は、必要なパラメーターを持たず、コマンドプロンプトに出力を返さないスクリプトを実行するように設計されています。

"PowerShell を使用して実行" 機能を使用すると、PowerShell コンソールウィンドウが少しだけ表示されます。 これと対話することはできません。

"PowerShell で実行" 機能を使用するには、次のようにします。

エクスプローラー (または Windows エクスプローラー) で、スクリプトファイルの名前を右クリックし、[PowerShell で実行] を選択します。

"PowerShell を使用して実行" 機能は、実行ポリシーがバイパスの PowerShell セッションを開始し、スクリプトを実行して、セッションを閉じます。

次の形式のコマンドを実行します。

```
PowerShell.exe -File <FileName> -ExecutionPolicy Bypass
```

"PowerShell で実行" は、スクリプトが実行されるセッション (PowerShell プロセスの現在のインスタンス) に対してのみバイパス実行ポリシーを設定します。
この機能では、コンピューターまたはユーザーの実行ポリシーは変更されません。

"PowerShell を使用して実行" 機能は、AllSigned 実行ポリシーによってのみ影響を受けます。 コンピューターまたはユーザーに対して AllSigned 実行ポリシーが有効な場合は、"PowerShell で実行" は署名されたスクリプトのみを実行します。 "PowerShell で実行" は、他の実行ポリシーの影響を受けません。 詳細については、「[about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

トラブルシューティングに関する注意: PowerShell コマンドを使用して実行すると、実行ポリシーの変更を確認するメッセージが表示される場合があります。

## <a name="see-also"></a>関連項目

[about_Execution_Policies](about_Execution_Policies.md)

[about_Group_Policy_Settings](about_Group_Policy_Settings.md)

[about_Scripts](about_Scripts.md)

