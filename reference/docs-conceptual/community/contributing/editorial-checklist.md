---
title: 編集チェックリスト
description: PowerShell ドキュメントを編集する際のルールの概要を示します。
ms.date: 03/05/2020
ms.topic: conceptual
ms.openlocfilehash: b5baf7366239084779d34e23f218e5e6222ed1a3
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "81624739"
---
# <a name="editors-checklist"></a>編集者のチェックリスト

ここでは、新しい記事を書くときや既存の記事を更新するときに適用されるルールの概要を示します。 これらのルールの詳しい説明と例については、共同作成者ガイドの他の記事をご覧ください。

## <a name="metadata"></a>Metadata

- `ms.date`: **MM/DD/YYYY** 形式
  - 重要な更新または事実に基づく更新がある場合は日付を変更する
    - 記事の再構成
    - 事実誤認の修正
    - 新しい情報の追加
  - 更新が重要でない場合は日付を変更しない
    - 誤字や書式設定の修正
- `title`: スペースを含め、43 から 59 文字の一意の文字列
  - サイト識別子を含めない - 自動生成されます
  - 文の先頭文字を大文字にする - 先頭文字と固有名詞のみを大文字にします
- `description`:スペースを含め、115 から 145 文字 - この要約は検索結果に表示されます

## <a name="formatting"></a>書式設定

- 段落内にインラインで表示されるバッククォート構文要素
  - コマンドレット名: `Verb-Noun`
  - 変数: `$counter`
  - 構文例: `Verb-Noun -Parameter`
  - ファイル パス: `C:\Program Files\PowerShell`、`/usr/bin/pwsh`
  - ドキュメント内でクリック可能であることを意図していない URL
  - プロパティまたはパラメーターの値
- プロパティ名、パラメーター、名前、クラス名、モジュール名、エンティティ名、オブジェクト名、型名には太字を使用する
  - 太字は、強調ではなく、セマンティック マークアップに使用
  - 太字 - アスタリスク (`**`) を使用
- 斜体 - アンダースコア (`_`) を使用
  - セマンティック マークアップではなく、強調にのみ使用
- 100 列 (**about_Topics** の場合は 80 列) で改行する
- ハード タブを使用しない - スペースのみを使用します
- 行の末尾にスペースを含めない

### <a name="headers"></a>ヘッダー

- H1 が最初 - H1 は記事ごとに 1 つだけ使用します
- [ATX ヘッダー](https://github.github.com/gfm/#atx-headings)だけを使用する
- すべてのヘッダーの先頭文字を大文字にする
- レベルをスキップしない - H2 をスキップして H3 を使用することはできません
- 最大の深さを H3 または H4 にする
- 前後に空白行を挿入する
- PlatyPS では、そのスキーマの特定のヘッダーが適用される - ヘッダーを追加または削除しないでください

### <a name="code-blocks"></a>コード ブロック

- 前後に空白行を挿入する
- タグ付きのコード フェンスを使用する - **powershell**、**Output**、または他の適切な言語 ID
- タグなしのフェンス - 構文ブロックまたはその他のシェル
- 読者が **[コピー]** ボタンを使用することを意図していないシンプルな例を除き、Output は別のコード ブロックに配置する
- [サポートされている言語](/contribute/code-in-docs#supported-languages)の一覧を参照する

### <a name="lists"></a>リスト

- 適切にインデントする
- 最初の項目の前と最後の項目の後に空白行を挿入する
- 箇条書き - 強調と混同しやすいアスタリスク (`*`) ではなく、ハイフン (`-`) を使用する
- 番号付きリストの場合、すべての番号に "1." を使用する

## <a name="terminology"></a>用語

- PowerShell、Windows PowerShell、PowerShell Core
- 「[Product Terminology (製品用語)](powershell-style-guide.md#product-terminology)」を参照する

## <a name="cmdlet-reference-examples"></a>コマンドレット リファレンスの例

- コマンドレット リファレンスには、例を少なくとも 1 つ含める必要がある
- 例には、使用方法を示すのにちょうどよいコードを使用する
- PowerShell 構文
  - コマンドレットとパラメーターのフル ネームを使用する - エイリアスは使用しないでください
  - コマンド ラインが長すぎる場合は、パラメーターにスプラッティングを使用する
  - 行継続のバッククォートの使用は避ける - 必要な場合にのみ使用してください
- 例に必要な場合を除き、PowerShell プロンプト (`PS>`) を削除または簡略化する
- コマンドレット リファレンスの例は、次の PlatyPS スキーマに従う必要がある

  ~~~Markdown
  ### Example 1 - Descriptive title

  Zero or more short descriptive paragraphs explaining the context of the example followed by one or
  more code blocks. Recommend at least one and no more than two.

  ```powershell
  ... one or more PowerShell code statements ...
  ```

  ```Output
  Example output of the code above.
  ```

  Zero or more optional follow up paragraphs that explain the details of the code and output.
  ~~~

- コード ブロックの間に段落を入れない - 説明的な内容はすべてコード ブロックの前後に配置する必要があります

## <a name="linking-to-other-documents"></a>他のドキュメントへのリンク

- ドキュメント セットの外部またはコマンドレット リファレンスと概念の間にリンクを設定する
  - docs.microsoft.com にリンクする場合は、相対 URL を使用する (`https://docs.microsoft.com/en-us` を削除する)
  - Microsoft プロパティの URL にロケールを含めない (例: URL から `/en-us` を削除する)
  - ターゲット サイトで無効である場合を除き、外部 Web サイトへのすべての URL で HTTPS を使用する
- ドキュメント セット内
  - ファイル パスへのリンク (例: `../folder/file.md`)
  - すべてのファイル パスでスラッシュ (`/`) 文字を使用する
- 画像リンクには一意の代替テキストが必要
