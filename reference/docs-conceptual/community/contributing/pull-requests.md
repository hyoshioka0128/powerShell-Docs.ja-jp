---
title: プルリクエストを送信する方法
description: この記事では、PowerShell-Docs リポジトリにプルリクエストを送信する方法について説明します。
ms.date: 12/09/2020
ms.topic: conceptual
ms.openlocfilehash: 1a21c25e19189aec4f48ad034147b02f4f804f9d
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "99599690"
---
# <a name="how-to-submit-pull-requests"></a>プルリクエストを送信する方法

コンテンツを変更するには、フォークからプルリクエスト (PR) を送信します。 プルリクエストをマージする前に、レビューする必要があります。 最良の結果を得るために、プルリクエストを送信する前に[編集チェックリスト](editorial-checklist.md)を確認してください。

## <a name="using-git-branches"></a>Git ブランチの使用

PowerShell-Docs の既定の分岐は `staging` 分岐です。 作業ブランチで行われた変更は、 `staging` 発行される前に分岐にマージされます。 ブランチは、 `staging` `live` 平日の午後 3:00 (太平洋標準時) に分岐にマージされます。 この `live` 分岐には、docs.microsoft.com に公開されているコンテンツが含まれています。

変更を開始する前に、PowerShell-Docs リポジトリのローカルコピーに作業ブランチを作成します。 ローカルで作業している場合は、作業ブランチを作成する前にローカルリポジトリを同期してください。 作業ブランチは、ブランチの更新後のコピーから作成する必要があり `staging` ます。

すべてのプルリクエストは、`staging` ブランチをターゲットにする必要があります。 ブランチに変更を送信しないで `live` ください。
`staging` ブランチで行われた変更が `live` にマージされ、`live` に対する変更が上書きされます。

## <a name="make-the-pull-request-process-work-better-for-everyone"></a>プルリクエストプロセスをすべてのユーザーに対して効果的に機能させる

PR をよりシンプルで焦点を絞ったものにすると、レビューとマージに要する時間を短縮できます。

### <a name="avoid-pull-requests-that-update-large-numbers-of-files-or-contain-unrelated-changes"></a>大量のファイルを更新したり、関連のない変更を含むプルリクエストを回避する

関連性のない変更を含む PR を作成しないようにします。 既存の記事の軽微な更新は、新しい記事や大規模な改訂から切り離します。 このような変更には、別の作業ブランチで対応します。

一括変更では、多数の変更されたファイルを含む PR が作成されます。 PR の変更されたファイルを最大 50 個に制限します。 大規模な PR はレビューするのが難しく、エラーを含む可能性が高くなります。

### <a name="renaming-or-deleting-files"></a>ファイル名の変更またはファイルの削除

ファイルの名前を変更したり、ファイルを削除したりする場合は、PR に関連した問題が発生している必要があります。 そのイシューでは、ファイルの名前変更または削除の必要性について話し合う必要があります。

コンテンツの追加または変更を、ファイル名の変更やファイルの削除と一緒にしないでください。 名前を変更または削除したファイルは、グローバルリダイレクトファイルに追加する必要があります。 可能であれば、名前の変更または削除されたコンテンツ (TOC ファイルを含む) にリンクされているすべてのファイルを更新します。

## <a name="docs-pr-validation-service"></a>Docs PR 検証サービス

Docs PR 検証サービスは、変更に対して検証規則を実行する GitHub アプリです。 検証サービスによって報告されたエラーまたは警告を修正する必要があります。

動作は次のとおりです。

1. PR を送信します。
1. PR の状態を示す GitHub コメント内に、リポジトリで有効になっている "チェック" の状態が表示されます。 この例では、"コミットの検証" と "OpenPublishing. ビルド" の2つのチェックが有効になっています。

   ![検証の状態 - 一部のチェックに失敗しました](media/pull-requests/validation-failed.png)

   コミットの検証に失敗した場合でも、ビルドは検証を通過できます。

1. **[詳細]** をクリックして、詳細情報を確認します。
1. [詳細] ページには、失敗したすべての検証チェックと、問題を修正する方法に関する情報が表示されます。
1. 検証が成功すると、PR に次のコメントが追加されます。

   ![検証の状態: 成功](media/pull-requests/build-validation.png)

> [!NOTE]
> 外部 (Microsoft の従業員以外) の共同作成者の場合、詳細なビルド レポートやプレビュー リンクにアクセスすることはできません。

PR を確認すると、変更を加えたり、検証警告メッセージを修正したりすることが求められる場合があります。 PowerShell-Docs チームは、検証エラーと編集要件を理解するのに役立ちます。

## <a name="next-steps"></a>次のステップ

[PowerShell-Docs スタイル ガイド](powershell-style-guide.md)

## <a name="additional-resources"></a>その他のリソース

[プルリクエストを管理する方法](managing-pull-requests.md)

<!--link refs-->
[fork]: /contribute/get-started-setup-local#fork-the-repository
