---
ms.date: 09/12/2016
ms.topic: reference
title: 表示されるデータの書式を設定する
description: 表示されるデータの書式を設定する
ms.openlocfilehash: 40f6b3b4fa36062ee0bad3f197ad159f571445c8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667863"
---
# <a name="formatting-displayed-data"></a>表示されるデータの書式を設定する

リスト、テーブル、またはワイドビュー内の個々のデータポイントを表示する方法を指定できます。 ビューの項目を定義するときに要素を使用することも、要素を使用して `FormatString` `ScriptBlock` `FormatString` データに対してメソッドを呼び出すこともできます。

## <a name="using-the-formatstring-element"></a>FormatString 要素の使用

次の例では、system.string `TotalProcessorTime` オブジェクトのプロパティの[](/dotnet/api/System.Diagnostics.Process)値が FormatString 要素を使用して書式設定されています。 `TotalProcessorTime`プロパティ

```
<TableColumnItem>
  <PropertyName>TotalProcessorTime</PropertyName>
  <FormatString>{0:MMM}{0:dd}{0:HH}:{0:mm}</FormatString>
</TableColumnItem>
```
