---
title: イシューを管理する方法
description: この記事では、PowerShell-Docs チームがイシューを管理する方法について説明します。
ms.date: 12/09/2020
ms.topic: conceptual
ms.openlocfilehash: 72267137a2657f51e5f616113adf92d80647acad
ms.sourcegitcommit: 61765d08321623743dc5db5367160f6982fe7857
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2020
ms.locfileid: "99602165"
---
# <a name="how-we-manage-issues"></a>イシューを管理する方法

この記事では、PowerShell-Docs リポジトリでイシューを管理する方法について説明します。 この記事は、PowerShell-Docs チームのメンバーの作業を支援することを目的としています。 ここで公開されているので、パブリックコントリビューターのプロセスの透明性を提供します。

## <a name="sources-of-issues"></a>イシューのソース

- コミュニティの共同作成者
- 内部の共同作成者
- ソーシャル メディア チャネルからのコメントの転記
- ドキュメント フィードバック フォームによるフィードバック

## <a name="response-time-targets"></a>目標対応時間

新しい問題の80% は、3営業日以内に終了します。

- トリアージ済み-1 営業日以内
- 10営業日以内の修正または変更

### <a name="labeling--milestones"></a>ラベル付けとマイルストーン

#### <a name="label-types"></a>ラベルの種類

|   Type   | 説明                                                         |
| -------- | ------------------------------------------------------------------- |
| 領域     | イシューを議論する、PowerShell またはドキュメントの部分を示すために使用します。<br>機能の所有者が機能に関するイシューを見つけるのに役立ちます。 |
| 問題    | 問題の種類を示します。                                         |
| 優先度 | 問題の優先度を示します。 値の範囲 0 (高)-4 (低)  |
| Status   | 作業項目の状態または作業項目が閉じられた理由を示します。          |
| タグ      | 追加の分類のためにに使用されるラベル                        |
| 待機中  | 他のイベントまたは他のイベントで待機していることを示します。         |

#### <a name="milestones"></a>Milestones

イシューと PR は、適切なマイルストーンでタグ付けする必要があります。 問題が特定のバージョンに適用されない場合、マイルストーンは使用されません。 PowerShell コードベースにまだマージされていない変更に関する Pr および関連する問題は、 **今後** のマイルストーンに割り当てる必要があります。 コードの変更がマージされたら、マイルストーンを適切なバージョンに変更します。

|    マイルストーン     |                    説明                     |
| ---------------- | -------------------------------------------------- |
| 7.0.0            | PowerShell 7.0 に関連する作業項目               |
| 7.1.0            | PowerShell 7.1 に関連する作業項目               |
| 7.2.0            | PowerShell 7.2 に関連する作業項目               |
| 予定           | 将来のバージョンの PowerShell の作業項目          |

## <a name="triage-process"></a>トリアージ プロセス

PowerShell docs チームメンバーは、問題を毎日確認し、新しい問題が発生したときにトリアージします。 チームは週に1回、トリアージが必要な問題、または未解決のままの問題について話し合います。

### <a name="misplaced-product-feedback"></a>間違って送られた製品フィードバック

- 顧客を正しいフィードバックチャネルにリダイレクトするコメントを入力します。
- 省略可能:イシューを適切な製品フィードバックの場所にコピーし、コピーした項目へのリンクを追加して、イシューを閉じます。 イシューを UserVoice にコピーしないでください。

  PowerShell の問題の既定の場所は [https://github.com/PowerShell/PowerShell/issues/new/choose](https://github.com/PowerShell/PowerShell/issues/new/choose) です。

  次のサブジェクト領域には、さまざまな問題があります。

  | 件名 |                                                     製品フィードバックの URL                                                     |
  | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
  | dsc      | [https://windowsserver.uservoice.com/forums/301869-powershell](https://windowsserver.uservoice.com/forums/301869-powershell) |
  | ギャラリー  | [https://github.com/powershell/powershellgallery/issues/new](https://github.com/powershell/powershellgallery/issues/new)     |
  | jea      | [https://github.com/powershell/jea/issues/new](https://github.com/powershell/jea/issues/new)                                 |
  | wmf      | [https://windowsserver.uservoice.com/forums/301869-powershell](https://windowsserver.uservoice.com/forums/301869-powershell) |

### <a name="support-requests"></a>サポート要求

- サポートに関する質問が単純な場合は、質問に丁寧に答えてイシューを閉じます。
- 質問が複雑な場合や、送信者からさらに多くの質問が返ってきた場合は、フォーラムおよびサポート チャネルにリダイレクトします。 フォーラムにリダイレクトする際の推奨されるテキストは次のとおりです。

  ```Markdown
  > This is not the right forum for these kinds of questions. Try posting your question in a
  > community support forum. For a list of community forums see:
  > https://docs.microsoft.com/powershell/scripting/community/community-support
  ```

### <a name="code-of-conduct-violations"></a>行動規範違反

- 必要に応じて、イシューを編集し、不快感を与えるコンテンツを削除します。
- そのイシューがスパムであることを示すコメントを入力し、イシューを閉じ、それ以上コメントできないようにロックします。
- 各違反については、週次のトリアージで説明し、さらにアクションを実行する必要があるかどうかを判断する必要があります (GitHub または Microsoft 法務への報告)。
