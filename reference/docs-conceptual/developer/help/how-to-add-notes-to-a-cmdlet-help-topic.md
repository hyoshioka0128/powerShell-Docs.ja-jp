---
ms.date: 10/20/2020
ms.topic: reference
title: コマンドレットのヘルプ トピックに注を追加する方法
description: コマンドレットのヘルプ トピックに注を追加する方法
ms.openlocfilehash: 61363c26ab0172277d1332ed1e9de080d8da200c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659564"
---
# <a name="how-to-add-notes-to-a-cmdlet-help-topic"></a>コマンドレットのヘルプ トピックに注を追加する方法

このセクションでは、PowerShell コマンドレットのヘルプ トピックに **注** セクションを追加する方法について説明します。 **注** セクションは、パラメーターの詳細な説明など、他の構造化されたセクションには当てはまりにくい詳細を説明するために使用します。 その内容には、特定のプロバイダーでのコマンドレットの動作、コマンドレットの独自かつ重要な使用方法、または想定されるエラー条件の回避方法に関するコメントなどを含めることができます。

**注** セクションは、1 つの `<maml:alertset>` ノードを使用して定義されます。 注セクションに追加できる注の数に制限はありません。 注ごとに、`<maml:alertset>` ノードに `<maml:alert>` タグのペアを追加します。 それぞれの注の内容は、一連の `<maml:para>` タグ内に追加します。 スペースに対しては空の `<maml:para>` タグを使用します。

```xml
<maml:alertSet>
  <maml:title>Optional title for Note</maml:title>
  <maml:alert>
    <maml:para>Note 1</maml:para>
    <maml:para>Note a</maml:para>
  </maml:alert>
  <maml:alert>
    <maml:para>Note 2</maml:para>
  </maml:alert>
</maml:alertSet>
```
