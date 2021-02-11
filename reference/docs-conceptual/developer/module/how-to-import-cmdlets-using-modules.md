---
ms.date: 08/28/2019
ms.topic: reference
title: モジュールを使用してコマンドレットをインポートする方法
description: モジュールを使用してコマンドレットをインポートする方法
ms.openlocfilehash: 485a4be4d2accaf050a6536e7f92a0673f62a30b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657280"
---
# <a name="how-to-import-cmdlets-using-modules"></a>モジュールを使用してコマンドレットをインポートする方法

この記事では、バイナリモジュールを使用して PowerShell セッションにコマンドレットをインポートする方法について説明します。

> [!NOTE]
> モジュールのメンバーには、コマンドレット、プロバイダー、関数、変数、別名などを含めることができます。 スナップインには、コマンドレットとプロバイダーのみを含めることができます。

## <a name="how-to-load-cmdlets-using-a-module"></a>モジュールを使用してコマンドレットを読み込む方法

1. コマンドレットが実装されているアセンブリファイルと同じ名前のモジュールフォルダーを作成します。 この手順では、Windows フォルダーにモジュールフォルダーが作成され `system32` ます。

   `%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules\mymodule`

1. `PSModulePath`環境変数に新しいモジュールフォルダーへのパスが含まれていることを確認します。 既定では、システムフォルダーは環境変数に既に追加されてい `PSModulePath` ます。 を表示するには `PSModulePath` 、「」と入力 `$env:PSModulePath` します。

1. コマンドレットアセンブリをモジュールフォルダーにコピーします。

1. モジュールのルートフォルダーにモジュールマニフェストファイル () を追加 `.psd1` します。 PowerShell はモジュールマニフェストを使用してモジュールをインポートします。 詳細については、「 [PowerShell モジュールマニフェストを記述する方法](../module/how-to-write-a-powershell-module-manifest.md)」を参照してください。

1. 次のコマンドを実行して、コマンドレットをセッションに追加します。

   `Import-Module [Module_Name]`

   この手順は、コマンドレットをテストするために使用できます。 これにより、アセンブリ内のすべてのコマンドレットがセッションに追加されます。 モジュールの詳細については、「 [Windows PowerShell モジュールの記述](../module/writing-a-windows-powershell-module.md)」を参照してください。

## <a name="see-also"></a>関連項目

[PowerShell モジュール マニフェストを記述する方法](../module/how-to-write-a-powershell-module-manifest.md)

[PowerShell モジュールをインポートする](../module/importing-a-powershell-module.md)

[Import-Module](/powershell/module/Microsoft.PowerShell.Core/Import-Module)

[モジュールのインストール](../module/installing-a-powershell-module.md)

[PSModulePath インストール パスを変更する](../module/modifying-the-psmodulepath-installation-path.md)

[Windows PowerShell コマンドレットの記述](../cmdlet/cmdlet-overview.md)
