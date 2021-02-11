---
ms.openlocfilehash: 61a70db9ae7a38d6731f1cefb011072c12d1e39a
ms.sourcegitcommit: 077488408c820c860131382324bdd576d0edf52a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/24/2020
ms.locfileid: "95514917"
---
# <a name="microsoft-open-source-code-of-conduct"></a>Microsoft オープン ソース倫理規定

> 更新:2020/11/02

このプロジェクトは、「[Microsoft のオープン ソースの倫理規定](https://opensource.microsoft.com/codeofconduct/)」を採用しています。 詳細については[論理規定についてのよくある質問](https://opensource.microsoft.com/codeofconduct/faq/)をご覧ください。また、追加の質問やコメントがある場合は[opencode@microsoft.com](mailto:opencode@microsoft.com)にお問い合わせください。

[live-badge]: https://powershell.visualstudio.com/PowerShell-Docs/_apis/build/status/PowerShell-Docs-CI?branchName=live
[staging-badge]: https://powershell.visualstudio.com/PowerShell-Docs/_apis/build/status/PowerShell-Docs-CI?branchName=staging

## <a name="build-status"></a>ビルドの状態

|          ライブ ブランチ          |           ステージング ブランチ            |
| :---------------------------- | :---------------------------------- |
| [![live-badge][]][live-badge] | [![staging-badge][]][staging-badge] |

## <a name="powershell-documentation"></a>PowerShell ドキュメント

公式の PowerShell ドキュメントが格納される PowerShell-Docs リポジトリへようこそ。

## <a name="repository-structure"></a>リポジトリの構造

次の一覧は、このリポジトリ内の主なフォルダーについて説明しています。

- `.github` - このリポジトリで GitHub によって使用される構成設定が含まれています
- `.vscode` - Visual Studio Code (VS Code) の構成設定と推奨される拡張機能が含まれています
- `assets` - ドキュメントにリンクされているダウンロード可能なファイルが含まれています
- `reference` - [docs.microsoft.com]([https://docs.microsoft.com/powershell/scripting/) に発行されたドキュメントが含まれています。 これには、リファレンスと概念のコンテンツの両方が含まれます。
  - `5.1` - コマンドレット リファレンスと PowerShell 5.1 に関するトピックが含まれています
  - `7.0` - コマンドレット リファレンスと PowerShell 7.0 に関するトピックが含まれています
  - `7.1` - コマンドレット リファレンスと PowerShell 7.1 に関するトピックが含まれています
  - `7.2` - コマンドレット リファレンスと PowerShell 7.2 (プレビュー) に関するトピックが含まれています
  - `bread` - 階層リンク ナビゲーションに使用される TOC が含まれています
  - `docs-conceptual` - ドキュメント サイトに公開されている概念説明の記事が含まれています。 一般に、フォルダー構造は目次 (TOC) を反映しています。
  - `mapping` - ビルド システムで使用されるバージョン マッピング構成が含まれています
  - `media` - ドキュメントで使用される画像ファイルが含まれています。 `docs-conceptual` のコンテンツ全体にメディア フォルダーがあります。 ドキュメントの画像の使用方法については、共同作成者ガイドを参照してください。
  - `module` - モジュール ブラウザーのページの Markdown ソースが含まれています
- `tests` - ビルド システムで使用される Pester テストが含まれています
- `tools` - ビルド システムで使用されるその他のツールが含まれています

> 注: 参照コンテンツ (番号付きフォルダー内) は、ドキュメント サイトの Web ページと、PowerShell で使用される更新可能なヘルプを作成するために使用されます。
> `docs-conceptual` フォルダー内の記事は、ドキュメント Web サイトにのみ公開されています。

## <a name="contributing"></a>寄与

このリポジトリへの公開投稿は、_ステージング_ ブランチへの [pull request](https://help.github.com/articles/using-pull-requests/) から行ってください。
pull request が受け入れられるには、[貢献者使用許諾契約書](https://cla.microsoft.com/)に署名する必要があります。 これは 1 回限りの要件です。

投稿の詳細については、[共同作成者ガイド](https://aka.ms/PSDocsContributor)を参照してください。 共同作成者ガイドには、ドキュメント、お勧めのツール、スタイルや書式設定に関する要求を投稿する方法についての詳細情報が記載されています。 異なるバージョン間で一貫性のあるドキュメントを保つため、問題と Pull Request のテンプレートを使用してください。

## <a name="licenses"></a>ライセンス

このプロジェクトには、2 つのライセンス ファイルがあります。 MIT ライセンスは、このリポジトリに含まれるコードに適用されます。 Creative Commons ライセンスは、ドキュメントに適用されます。
