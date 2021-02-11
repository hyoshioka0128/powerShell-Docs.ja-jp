---
title: PowerShell ドキュメントへの投稿を開始する
description: この記事では、PowerShell ドキュメントの共同作成者として作業を開始する方法の概要を説明します。
ms.date: 12/09/2020
ms.topic: conceptual
ms.openlocfilehash: ddcbf79de1ab05b901ce1abd67f65b2524ed164d
ms.sourcegitcommit: 61765d08321623743dc5db5367160f6982fe7857
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2020
ms.locfileid: "99602387"
---
# <a name="get-started-contributing-to-powershell-documentation"></a>PowerShell ドキュメントへの投稿を開始する

この記事では、PowerShell ドキュメントの共同作成者として作業を開始する方法の概要を説明します。

## <a name="powershell-docs-structure"></a>PowerShell-Docs の構造

[PowerShell Docs リポジトリ][psdocs]は、リファレンスと概念という2つのコンテンツグループに分かれています。

### <a name="reference-content"></a>リファレンス コンテンツ

参照コンテンツは、PowerShell に付属するコマンドレットの PowerShell コマンドレットリファレンスです。
[参照][ref]は、バージョンフォルダー (5.1、7.0、7.1、および 7.2) で収集されます。 このコンテンツには、PowerShell に付属するモジュールに対してのみコマンドレットリファレンスが含まれています。 このコンテンツは、コマンドレットによって表示されるヘルプ情報を作成するためにも使用され `Get-Help` ます。

### <a name="conceptual-content"></a>概念的コンテンツ

概念説明のドキュメントには、次のコンテンツが含まれています。

- リリース ノート
- セットアップ手順
- サンプルスクリプトと操作方法に関する記事
- DSC のドキュメント
- SDK のドキュメント

[概念説明のドキュメント][conceptual]は、バージョン別に分類されていません。 すべてのバージョンの PowerShell について、すべての記事が表示されます。 バージョン固有の記事の作成に取り組んでいます。 この機能がドキュメントで利用できるようになると、このガイドは適切な情報で更新されます。

> [!NOTE]
> 概念的な記事を追加、削除、または名前変更した場合はいつでも、TOC を更新してプル要求に含める必要があります。

## <a name="creating-new-articles"></a>新しい記事の作成

投稿する新しいドキュメントについては、GitHub の問題を作成する必要があります。 既存の問題を確認して、作業を複製していないことを確認します。 割り当てられた問題はと見なされ `in progress` ます。 問題に対して共同作業を行う場合は、問題に割り当てられている担当者にお問い合わせください。

PowerShell [RFC プロセス][rfc]と同様に、コンテンツを書き込む前に問題を作成します。 この問題により、PowerShell-Docs チームによって却下された作業の時間と労力を無駄にすることがなくなります。 この問題により、コンテンツのスコープと、PowerShell ドキュメントに含まれる内容を確認できます。 すべての記事が目次 (目次) に含まれている必要があります。 提案された TOC の場所は、問題のディスカッションに含まれている必要があります。

> [!NOTE]
> 参照コンテンツの TOC は、公開システムによって自動生成されます。 TOC を更新する必要はありません。

## <a name="updating-existing-articles"></a>既存のアーティクルの更新

該当する場合、コマンドレットのリファレンス記事は、このリポジトリに保持されているすべてのバージョンの PowerShell で重複しています。 コマンドレット参照またはアーティクルに関する問題を報告する場合は、 `About_` 問題の影響を受けるバージョンを指定する必要があります。 GitHub の issue テンプレートには、バージョンのチェックリストが含まれています。 チェックボックスを使用して、影響を受けるコンテンツのバージョンを指定します。

他のバージョンでも同じ変更が必要かどうかを確認するには、記事のすべてのバージョンを確認してください。 ファイルの各バージョンに適切な変更を適用します。

## <a name="localized-content"></a>ローカライズされたコンテンツ

PowerShell ドキュメントは英語で書かれており、17個の他の言語に翻訳されています。 英語のコンテンツは、という名前の GitHub リポジトリに格納され `MicrosoftDocs/PowerShell-Docs` ます。 ローカライズされたコンテンツは、言語ごとに個別のリポジトリに格納されます。 リポジトリでは、同じベース名が使用されますが、ロケール名が末尾に追加されます。 たとえば、には `MicrosoftDocs/PowerShell-Docs.de-de` ドイツ語に翻訳されたコンテンツが含まれています。

一般に、問題と変更は英語のリポジトリに送信する必要があります。 すべての翻訳は最初に英語のコンテンツから開始されます。 人間と機械の両方の翻訳を使用します。

| Translation メソッド  |                              Languages                               |
| ------------------- | -------------------------------------------------------------------- |
| 人間による翻訳   | de、es、fr-fr、it-IT、ja-jp、ko、韓国、pt-BR、ru-RU、zh-tw、zh-tw、または、-TW |
| 機械翻訳 | CS-CZ, hu-HU, nl-NL, pl-PL, pt-PT, sv-SE, tr-TR                      |

機械翻訳によって翻訳されたコンテンツは、単語の選択や文法が必ずしも正しいとは限りません。 記事の技術的な詳細ではなく、任意の言語の翻訳でエラーが見つかった場合は、ローカライズされたリポジトリで問題を開きます。 翻訳が間違っていると思われる理由を説明します。 Microsoft のローカライズチームは、これらの問題を確認して対応しています。

## <a name="next-steps"></a>次のステップ

GitHub で変更を送信するには、2つの一般的な方法があります。 両方の方法については、中央の共同作成者ガイドを参照してください。

1. GitHub web インターフェイスで [既存のドキュメントを簡単に編集](/contribute/#quick-edits-to-existing-documents) できます。
1. 新しい記事を追加したり、複数のファイルを更新したり、その他の大きな変更を行ったりするには、 [完全な GitHub ワークフロー][making-changes] を使用します。

変更を開始する前に、PowerShell-Docs リポジトリのフォークを作成する必要があります。 変更は、PowerShell ドキュメントのコピーの作業ブランチで行う必要があります。GitHub の **クイック編集** 方法を使用している場合は、これらの手順が処理されます。 **完全な GitHub ワークフロー** を使用している場合は、[ローカルで動作][fork]するように設定する必要があります。

どちらの方法も、プル要求 (PR) の作成で終了します。 詳細とベストプラクティスについて [は、「プル要求の送信](pull-requests.md) 」を参照してください。

<!--link refs-->
[conceptual]: https://github.com/MicrosoftDocs/PowerShell-Docs/tree/staging/reference/docs-conceptual
[fork]: /contribute/get-started-setup-local#fork-the-repository
[making-changes]: /contribute/how-to-write-workflows-major#making-your-changes
[psdocs]: https://github.com/MicrosoftDocs/PowerShell-Docs
[ref]: https://github.com/MicrosoftDocs/PowerShell-Docs/tree/staging/reference
[rfc]: https://github.com/PowerShell/powershell-rfc/blob/master/RFC0000-RFC-Process.md
