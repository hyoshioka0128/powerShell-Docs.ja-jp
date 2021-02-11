---
ms.date: 09/12/2016
ms.topic: reference
title: ヘルプ ファイルを更新する方法
description: ヘルプ ファイルを更新する方法
ms.openlocfilehash: 19bf501cf91b1eb5dabb334c2179953590b40232
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649580"
---
# <a name="how-to-update-help-files"></a>ヘルプ ファイルを更新する方法

このトピックでは、更新可能なヘルプをサポートするモジュールのヘルプファイルを更新する方法について説明します。

## <a name="updating-help-files"></a>ヘルプファイルの更新

ヘルプファイルの更新には、エラーの修正、概念の明確化、よく寄せられる質問への回答、新しいファイルの追加、新しい例の追加など、さまざまな理由があります。

ヘルプファイルを更新するには:

1. ファイルを変更します。
1. ファイルを他の UI カルチャに変換します。
1. 各 UI カルチャのモジュールについて、すべてのヘルプファイル (新規、変更済み、および変更されていない) を収集します。
1. XML スキーマに対してファイルを検証します。
1. 各 UI カルチャの CAB ファイルをリビルドします。
1. HelpInfo XML ファイルで、各 UI カルチャの CAB ファイルのバージョン番号をインクリメントします。
1. HelpInfo XML ファイルの **Helpcontenturi** 要素の値によって指定された場所に新しい CAB ファイルをアップロードします。 古い CAB ファイルを新しい CAB ファイルに置き換えます。
1. 更新された HelpInfo XML ファイルを、モジュールマニフェストの **Helpinfouri** キーによって指定された場所にアップロードします。 古い HelpInfo XML ファイルを新しいファイルに置き換えます。
