---
ms.date: 06/12/2017
title: 検索結果のフィルター処理
description: この記事では、PowerShell ギャラリーでコンテンツをフィルター処理するために使用されるユーザー インターフェイスについて説明します。
ms.openlocfilehash: a769daae903e614b96be1056e3ff14eca41970bd
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94389829"
---
# <a name="filtering-search-results"></a>検索結果のフィルター処理

[[パッケージ] タブ](https://www.powershellgallery.com/packages)には、PowerShell ギャラリーで利用可能なすべてのパッケージが表示されます。

パッケージのフィルター、並べ替え、検索には、複数の方法があります。 特定のパッケージに関する詳細情報を表示するには、そのパッケージをクリックします。

## <a name="filter-by"></a>フィルター条件

[フィルター条件] の下にあるドロップダウンでは、結果を次の条件でフィルター処理できます。

- プレリリースを含める
- 安定版パッケージのみ

"プレリリース" と "安定版パッケージ" の詳細については、PowerShell チームのブログの「[Prerelease Versioning Added to PowerShellGet and PowerShell Gallery (PowerShellGet と PowerShell ギャラリーにプレリリースのバージョン管理が追加)](https://devblogs.microsoft.com/powershell/prerelease-versioning-added-to-powershellget-and-powershell-gallery/)」をご覧ください。

ドロップダウンの下にあるチェック ボックスでは、結果を次の条件でフィルター処理できます。

- パッケージの種類
  - Module
  - スクリプト
- Categories
  - コマンドレット
  - DSC リソース
  - 機能
  - ロール機能
  - ワークフロー

PowerShell ギャラリー内のモジュールのみを表示するには、[パッケージの種類] の [モジュール] をオンにします。 同様に、PowerShell ギャラリー内のスクリプトのみを表示するには、[パッケージの種類] の [スクリプト] をオンにします。

> [!NOTE]
> フィルターは包含です。 例:[コマンドレット] または [関数] (またはその両方) をオンにした場合、コマンドレットと関数の両方を含むパッケージが表示されます。 選択されていない場合、パッケージは表示されません。 同様に、すべてのカテゴリが選択されている場合は、これらのカテゴリのいずれかを含むパッケージのみが表示されます。 **これらのどのカテゴリにも属していないパッケージは表示されません。**

## <a name="sort-by"></a>並べ替え条件

[並べ替え条件] ドロップダウンでは、結果を次の条件で並べ替えることができます。

- [人気度] - 人気度はダウンロード数によって決定します
- [A - Z] - パッケージ名のアルファベット順です
- [最近使ったパッケージ] - 公開日に応じてパッケージが表示されます

## <a name="search-box"></a>[検索] ボックス

[検索] ボックスでは、キーワードでパッケージを検索することができます。
詳細については、「[ギャラリー検索構文](search-syntax.md)」をご覧ください。
