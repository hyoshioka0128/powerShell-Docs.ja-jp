---
ms.date: 09/11/2018
title: パッケージの手動ダウンロード
description: PowerShell ギャラリーからパッケージを手動でダウンロードする方法について説明します。
ms.openlocfilehash: 50cd51d970bf21f8e957e60ceed2e98f306b57ab
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662287"
---
# <a name="manual-package-download"></a>パッケージの手動ダウンロード

PowerShell ギャラリーでは、PowerShellGet コマンドレットを使用せずに、Web サイトから直接パッケージのダウンロードをサポートしています。 どのパッケージも NuGet パッケージ (`.nupkg`) ファイルとしてダウンロードでき、その後、内部リポジトリにコピーできます。

> [!NOTE]
> パッケージの手動ダウンロードは、`Install-Module` コマンドレットの代替手段を意図したものでは **ありません** 。
> パッケージのダウンロードでは、モジュールまたはスクリプトはインストールされません。 NuGet パッケージのダウンロードには依存関係は含まれていません。 次の手順は、参照目的にのみ提供されています。

## <a name="using-manual-download-to-acquire-a-package"></a>手動ダウンロードを使用してパッケージを取得

次に示すように、各ページには [手動ダウンロード] のリンクがあります。

![インストール オプションが記載されているパッケージ表示ページ](media/manual-download/packagedisplaypagewithpseditions.png)

手動でダウンロードするには、 **[Download the raw nupkg file]\(raw nupk ファイルのダウンロード\)** をクリックします。 パッケージのコピーがブラウザーのダウンロード フォルダーに `<name>.<version>.nupkg` という名前でコピーされます。

NuGet パッケージは、パッケージの内容に関する情報を含む追加のファイルが含まれた ZIP アーカイブです。 Internet Explorer などの一部のブラウザーでは、ファイル拡張子が `.nupkg` から `.zip` に自動的に置き換えられます。 パッケージを展開するには、必要に応じて `.nupkg` ファイルの名前を `.zip` に変更してから、ローカル フォルダーに内容を抽出します。

NuGet パッケージ ファイルには、元のパッケージ化されたコードの一部ではない次の **NuGet に固有の要素** が含まれています。

- `_rels` という名前のフォルダーには、依存関係の一覧表示する `.rels` ファイルが含まれています
- `package` という名前のフォルダーには、NuGet に固有のデータが含まれています
- `[Content_Types].xml` という名前のファイルには、PowerShellGet などの拡張機能が NuGet でどのように機能するかが説明されています
- `<name>.nuspec` という名前のファイルには、メタデータの大部分が含まれています

## <a name="installing-powershell-modules-from-a-nuget-package"></a>NuGet パッケージから PowerShell モジュールをインストールする

> [!NOTE]
> これらの手順を実行しても、`Install-Module` を実行した場合と同じ結果には **なりません** 。 これらの手順は、最小要件を満たします。 これらは、`Install-Module` の代替手段を意図したものではありません。
> `Install-Module` によって実行されるいくつかの手順は含まれていません。

最も簡単な方法は、フォルダーから NuGet に固有の要素を削除することです。 要素を削除すると、パッケージの作成者によって作成された PowerShell コードが残ります。
NuGet 固有の要素の一覧については、[手動ダウンロードを使用したパッケージの取得](#using-manual-download-to-acquire-a-package)に関するページを参照してください。

手順は次のとおりです。

1. インターネットからダウンロードされた NuGet パッケージ (`.nupkg`) ファイルのブロックを、たとえば、`Unblock-File -Path C:\Downloads\module.nupkg` コマンドレットを使用して解除します。
1. NuGet パッケージの内容をローカル フォルダーに抽出します。
1. フォルダーから NuGet に固有の要素を削除します。
1. フォルダーの名前を変更します。 既定のフォルダー名は通常、`<name>.<version>` です。 モジュールがプレリリース バージョンとしてタグ付けされている場合は、バージョンに `-prerelease` を含めることができます。 フォルダーの名前をモジュールの名前だけに変更します。 たとえば、`azurerm.storage.5.0.4-preview` を `azurerm.storage` にします。
1. フォルダーを `$env:PSModulePath value` 内のフォルダーのいずれかにコピーします。 `$env:PSModulePath` は、PowerShell がモジュールを検索する、セミコロンで区切られたパスのセットです。

> [!IMPORTANT]
> 手動ダウンロードには、モジュールで必要な依存関係は何も含まれていません。 パッケージに依存関係がある場合は、このモジュールが正常に動作するために、それらをシステムにインストールする必要があります。 PowerShell ギャラリーには、パッケージで必要なすべての依存関係が表示されます。

## <a name="installing-powershell-scripts-from-a-nuget-package"></a>NuGet パッケージから PowerShell スクリプトをインストールする

> [!NOTE]
> これらの手順を実行しても、`Install-Script` を実行した場合と同じ結果には **なりません** 。 これらの手順は、最小要件を満たします。 これらは、`Install-Script` の代替手段を意図したものではありません。

最も簡単な方法は、NuGet パッケージを抽出してから、スクリプトを直接使用することです。

手順は次のとおりです。

1. インターネットからダウンロードされた NuGet パッケージ (`.nupkg`) ファイルのブロックを、たとえば、`Unblock-File -Path C:\Downloads\package.nupkg` コマンドレットを使用して解除します。
1. NuGet パッケージの内容を抽出します。
1. フォルダー内の `.PS1` ファイルは、この場所から直接使用することができます。
1. フォルダー内の NuGet に固有の要素を削除できます。

NuGet 固有の要素の一覧については、[手動ダウンロードを使用したパッケージの取得](#using-manual-download-to-acquire-a-package)に関するページを参照してください。

> [!IMPORTANT]
> 手動ダウンロードには、モジュールで必要な依存関係は何も含まれていません。 パッケージに依存関係がある場合は、このモジュールが正常に動作するために、それらをシステムにインストールする必要があります。 PowerShell ギャラリーには、パッケージで必要なすべての依存関係が表示されます。
