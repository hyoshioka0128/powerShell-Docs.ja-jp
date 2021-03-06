---
ms.date: 09/11/2018
contributor: JKeithB
keywords: ギャラリー, PowerShell, PSGallery
title: パッケージの手動ダウンロード
ms.openlocfilehash: af628f99befe50c16c2c0c60f1a352647af34ff4
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62075361"
---
# <a name="manual-package-download"></a>パッケージの手動ダウンロード

PowerShell ギャラリーでは、PowerShellGet コマンドレットを使用せずに、Web サイトから直接パッケージのダウンロードをサポートしています。 任意のパッケージを NuGet パッケージ (.nupkg) ファイルとしてダウンロードできます。その後、内部リポジトリにコピーできます。

> [!NOTE]
> パッケージの手動ダウンロードは、Install-module コマンドレットの代替手段を意図したものでは**ありません**。
> パッケージのダウンロードでは、モジュールまたはスクリプトはインストールされません。 NuGet パッケージのダウンロードには依存関係は含まれていません。 次の手順は、参照目的にのみ提供されています。

## <a name="using-manual-download-to-acquire-a-package"></a>手動ダウンロードを使用してパッケージを取得

次に示すように、各ページには [手動ダウンロード] のリンクがあります。

![手動ダウンロード](../../Images/packagedisplaypagewithpseditions.png)

手動でダウンロードするには、**[Download the raw nupkg file]\(raw nupk ファイルのダウンロード\)** をクリックします。 パッケージのコピーがブラウザーのダウンロード フォルダーに `<name>.<version>.nupkg` という名前でコピーされます。

NuGet パッケージは、パッケージの内容に関する情報を含む追加のファイルが含まれた ZIP アーカイブです。 Internet Explorer などの一部のブラウザーでは、ファイル拡張子が `.nupkg` から `.zip` に自動的に置き換えられます。 パッケージを展開するには、必要に応じて `.nupkg` ファイルの名前を `.zip` に変更してから、ローカル フォルダーに内容を抽出します。

NuGet パッケージ ファイルには、元のパッケージ化されたコードの一部ではない次の NuGet に固有の要素が含まれています。

- `_rels` という名前のフォルダーには、依存関係の一覧表示する `.rels` ファイルが含まれています
- `package` という名前のフォルダーには、NuGet に固有のデータが含まれています
- `[Content_Types].xml` という名前のファイルには、PowerShellGet などの拡張機能が NuGet でどのように機能するかが説明されています
- `<name>.nuspec` という名前のファイルには、メタデータの大部分が含まれています

## <a name="installing-powershell-modules-from-a-nuget-package"></a>NuGet パッケージから PowerShell モジュールをインストールする

> [!NOTE]
> これらの手順を実行しても、`Install-Module` を実行した場合と同じ結果には**なりません**。 これらの手順は、最小要件を満たします。 これらは、`Install-Module` の代替手段を意図したものではありません。 `Install-Module` によって実行されるいくつかの手順は含まれていません。

最も簡単な方法は、フォルダーから NuGet に固有の要素を削除することです。 これにより、パッケージの作成者によって作成された PowerShell コードが残ります。 手順は次のとおりです。

1. NuGet パッケージの内容をローカル フォルダーに抽出します。
2. フォルダーから NuGet に固有の要素を削除します。
3. フォルダーの名前を変更します。 既定のフォルダー名は通常、`<name>.<version>` です。 モジュールがプレリリース バージョンとしてタグ付けされされている場合は、バージョンに "-prerelease" を含めることができます。 フォルダーの名前をモジュールの名前だけに変更します。 たとえば、"azurerm.storage.5.0.4-preview" は "azurerm.storage" になります。
4. フォルダーを `$env:PSModulePath value` 内のフォルダーのいずれかにコピーします。 `$env:PSModulePath` はセミコロンで区切られたパスのセットです。ここで、PowerShell によってモジュールが検索されます。

> [!IMPORTANT]
> 手動ダウンロードには、モジュールで必要な依存関係は何も含まれていません。 パッケージに依存関係がある場合は、このモジュールが正常に動作するために、それらをシステムにインストールする必要があります。 PowerShell ギャラリーには、パッケージで必要なすべての依存関係が表示されます。

## <a name="installing-powershell-scripts-from-a-nuget-package"></a>NuGet パッケージから PowerShell スクリプトをインストールする

> [!NOTE]
> これらの手順を実行しても、`Install-Script` を実行した場合と同じ結果には**なりません**。 これらの手順は、最小要件を満たします。 これらは、`Install-Script` の代替手段を意図したものではありません。

最も簡単な方法は、NuGet パッケージを抽出してから、スクリプトを直接使用することです。 手順は次のとおりです。

1. NuGet パッケージの内容を抽出します。
2. フォルダー内の `.PS1` ファイルは、この場所から直接使用することができます。
3. フォルダー内の NuGet に固有の要素を削除できます。

> [!IMPORTANT]
> 手動ダウンロードには、モジュールで必要な依存関係は何も含まれていません。 パッケージに依存関係がある場合は、このモジュールが正常に動作するために、それらをシステムにインストールする必要があります。 PowerShell ギャラリーには、パッケージで必要なすべての依存関係が表示されます。
