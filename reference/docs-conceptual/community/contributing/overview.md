---
title: PowerShell ドキュメントへの投稿
description: この記事では、PowerShell ドキュメントに投稿するために必要な手順について説明します。
ms.date: 12/09/2020
ms.topic: conceptual
ms.openlocfilehash: 9fbdafa023eac80340437f30d2d6925a1a4ed3cb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "99599509"
---
# <a name="contributing-to-powershell-documentation"></a>PowerShell ドキュメントへの投稿

PowerShell のサポートにご協力いただき、ありがとうございます。

共同作成者ガイドは、Microsoft でドキュメントを作成するために使用するツールとプロセスについて説明する記事のコレクションです。 これらのガイドの一部では、[docs.microsoft.com][docs] に公開されているすべてのドキュメント セットに共通する情報について説明しています。 ガイドの一部は、PowerShell のドキュメントを記述する方法に特有のものです。

一般的な記事については、一元管理された[共同作成者ガイド][contribute]で確認できます。 PowerShell 固有のガイドは、ここで参照できます。

## <a name="ways-to-contribute"></a>作成協力の方法

投稿するには、次の 2 つの方法があります。 どちらの方法も、Microsoft にとって価値があります。

- [イシューの報告][file-an-issue]により、Microsoft は弊社のドキュメントの問題や矛盾点を特定することができます。 場合によっては、イシューは解決が難しく、調査がさらに必要になることがあります。 イシューのプロセスにより、私たちは問題について会話し、十分な解決策を策定することができます。

- コンテンツを追加または変更するための[プル要求の送信](pull-requests.md)は、より複雑なプロセスです。
  次の情報では、ドキュメントにコンテンツを送信するためのツール、プロセス、基準について説明します。

## <a name="prepare-to-make-a-contribution"></a>投稿するための準備

ドキュメントの共同作成には、GitHub アカウントが必要です。 次のチェックリストを使用して、投稿を行うために必要なツールをインストールして構成します。

1. [GitHub にサインアップします](/contribute/get-started-setup-github)
1. [Git ツールと Markdown ツールをインストールします](/contribute/get-started-setup-tools)
1. [Docs Authoring パックをインストールします](/contribute/how-to-write-docs-auth-pack)
1. [Posh-Git をインストールします][posh-git] (推奨されますが、必須ではありません)
1. [ローカル Git リポジトリを設定します](/contribute/get-started-setup-local)
1. [Git と GitHub の基礎を確認します](/contribute/git-github-fundamentals)

## <a name="get-started-writing-docs"></a>ドキュメントの作成を開始する

ドキュメントに変更を加えるには、次の 2 つの方法があります。

1. [既存のドキュメントへのクイック編集](/contribute/#quick-edits-to-existing-documents)
   - 軽微な修正、誤字の修正、または小さな追加
1. [ドキュメント用の完全な GitHub ワークフロー](/contribute/how-to-write-workflows-major)
   - 大きな変更、複数のバージョン、画像の追加または変更、または新しい記事の投稿

また、一元化された共同作成者ガイドの「[書き込みの要点](/contribute/style-quick-start)」も参照してください。 もう 1 つの優れたリソースは、[Microsoft ライティング スタイル ガイド][style-guide]です。 Microsoft ライティング スタイル ガイドの目的は、エディター、テクニカル ライター、開発者、マーケティング担当者など IT に関するすべての人の記述するコンテンツを向上させることです。

パブリック リポジトリに含まれているドキュメントとコード例に対する軽微な修正や明確化は、[docs.microsoft.com の使用条件][terms-of-use]の対象になります。

重要な変更を行う場合は、完全な GitHub ワークフローを使用します。 Microsoft の従業員でない場合は、重要な変更によって、オンラインの [Contribution Licensing Agreement (CLA)][cla] を送信するように求めるコメントがプル要求に生成されます。 プル要求が正しく確認され受理されるように、オンライン フォームへのご記入をお願いいたします。

## <a name="code-of-conduct"></a>倫理規定

docs.microsoft.com に公開されるすべてのリポジトリには、「[Microsoft オープン ソース倫理規定](https://opensource.microsoft.com/codeofconduct/)」または「[.NET Foundation 倫理規定](https://dotnetfoundation.org/code-of-conduct)」が適用されます。 詳細については、[倫理規定に関する FAQ](https://opensource.microsoft.com/codeofconduct/faq/) を参照してください。

## <a name="next-steps"></a>次のステップ

次の記事では、PowerShell ドキュメントに固有の情報を紹介します。 一元化された共同作成者ガイドのガイダンスと重複している箇所については、それらの規則が PowerShell コンテンツの場合ではどのように異なるかについて示しています。

以下のドキュメントを確認してください。

- [イシューを報告する方法](file-an-issue.md)
- [ドキュメントの作成を開始する](get-started-writing.md)
- [プル要求の送信](pull-requests.md)
- [PowerShell ドキュメントのスタイル ガイド](powershell-style-guide.md)

その他のリソース

- [編集のチェックリスト](editorial-checklist.md)
- [イシューを管理する方法](managing-issues.md)
- [プル要求を管理する方法](managing-pull-requests.md)

<!--link refs-->
[cla]: https://cla.microsoft.com/
[contribute]: /contribute/
[docs]: https://docs.microsoft.com/
[file-an-issue]: file-an-issue.md
[posh-git]: https://www.powershellgallery.com/packages/posh-git
[psdocs]: /powershell
[style-guide]: /style-guide/welcome/
[terms-of-use]: /legal/termsofuse
