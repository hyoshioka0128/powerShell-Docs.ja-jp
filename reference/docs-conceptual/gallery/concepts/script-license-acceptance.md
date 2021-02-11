---
ms.date: 06/09/2017
title: スクリプトでのライセンス同意の必須化
description: この記事では、PowerShell ギャラリーで公開されているスクリプトのうち、エンド ユーザー ライセンスの受け入れを必要とするものの取り扱い方法について説明します。
ms.openlocfilehash: d82974810fd1e73ef8d9e5771fc430d0f7964e87
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92656074"
---
# <a name="requiring-license-acceptance-for-scripts"></a>スクリプトでのライセンス同意の必須化

ライセンス同意は、スクリプトではサポートされていません。 ただし、ライセンスへの同意が必要なモジュールにスクリプトが依存するシナリオはサポートされています。

PowerShellGet スクリプト コマンドでは、ユーザーがライセンスを確認した場合と同じ動作をするパラメーター **AcceptLicense** がサポートされています。 **AcceptLicense** を指定しない場合、ユーザーには依存モジュールの `license.txt` ファイルが示され、このライセンスに同意するように求められます。

## <a name="examples"></a>例

### <a name="example-1-install-script-with-dependencies-requiring-license-acceptance"></a>例 1: ライセンスへの同意が必要な依存関係があるスクリプトをインストールする

スクリプト "ScriptRequireLicenseAcceptance" は、モジュール "ModuleRequireLicenseAcceptance" に依存しています。 ユーザーにはライセンスへの同意が求められます。

```PowerShell
PS> Install-Script -Name ScriptRequireLicenseAcceptance

License Acceptance
MIT License 2.0
Copyright (c) 2016 PowerShell Team
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.

Do you accept the license terms for module 'ModuleRequireLicenseAcceptance'.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

### <a name="example-2-install-script-with-dependencies-requiring-license-acceptance-and--acceptlicense"></a>例 2: -AcceptLicense を使用してライセンスへの同意が必要な依存関係があるスクリプトをインストールする

スクリプト "ScriptRequireLicenseAcceptance" は、モジュール "ModuleRequireLicenseAcceptance" に依存しています。 -AcceptLicense を指定したため、ライセンスへの同意は求められません。

```PowerShell
PS> Install-Script -Name ScriptRequireLicenseAcceptance -AcceptLicense
```

## <a name="more-details"></a>詳細

- [モジュールに対するライセンス同意リクエストのサポート](module-license-acceptance.md)
- [PowerShellGallery でのライセンス同意リクエストのサポート](../how-to/working-with-packages/packages-that-require-license-acceptance.md)
- [Azure Automation へのデプロイに表示されるライセンス同意リクエスト](../how-to/working-with-packages/deploy-to-azure-automation.md)
