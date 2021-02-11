---
ms.date: 09/12/2016
ms.topic: reference
title: コマンドレットのヘルプ トピックに関連リンクを追加する方法
description: コマンドレットのヘルプ トピックに関連リンクを追加する方法
ms.openlocfilehash: 7f1baefea69310bdf835c52461f8d3f49c4d94e8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92662005"
---
# <a name="how-to-add-related-links-to-a-cmdlet-help-topic"></a>コマンドレットのヘルプ トピックに関連リンクを追加する方法

このセクションでは、PowerShell コマンドレットのヘルプトピックに関連するその他のコンテンツへの参照を追加する方法について説明します。 これらの参照はコマンドウィンドウに表示されるため、参照先のコンテンツに直接リンクされません。

PowerShell に含まれているコマンドレットのヘルプトピックでは、これらのリンクは、PowerShell に関連しない他のコマンドレット、概念説明コンテンツ ( `about_` )、およびその他のドキュメントやヘルプファイルを参照しています。

次の XML は、関連トピックへの2つの参照を含む、関係 **リンク** ノードを追加する方法を示しています。

```xml
<maml:relatedLinks>
  <maml:navigationLink>
    <maml:linkText>Topic-name</maml:linkText>
  </maml:navigationLink>
  <maml:navigationLink>
    <maml:linkText>Topic-name</maml:linkText>
  </maml:navigationLink>
</ maml:relatedLinks >
```
