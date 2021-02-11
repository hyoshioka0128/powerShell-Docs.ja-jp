---
ms.date: 09/12/2016
ms.topic: reference
title: HelpInfo XML バージョン番号を設定する方法
description: HelpInfo XML バージョン番号を設定する方法
ms.openlocfilehash: 9ef1940ca05d8aa9b04770013287490b71c8065a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92658829"
---
# <a name="how-to-set-helpinfo-xml-version-numbers"></a>HelpInfo XML バージョン番号を設定する方法

このトピックでは、更新可能なヘルプ情報ファイル ("HelpInfo XML ファイル" と呼ばれます) でバージョン番号を設定および増やす方法について説明します。

## <a name="how-to-set-helpinfo-xml-version-numbers"></a>HelpInfo XML バージョン番号を設定する方法

HelpInfo XML ファイルのバージョン番号は、更新可能なヘルプの操作にとって重要です。 [Update-help](/powershell/module/Microsoft.PowerShell.Core/Update-Help)および HelpInfo コマンドレットは、リモートの xml ファイル内の ui カルチャのバージョン番号が、ローカルの HelpInfo XML の ui カルチャのバージョン番号より大きい場合、またはローカルの HelpInfo xml ファイルがない場合にのみ、新しいヘルプファイルを[ダウンロードします](/powershell/module/Microsoft.PowerShell.Core/Save-Help)。

HelpInfo XML ファイルは、Microsoft .NET Framework の system.xml クラスで定義されている4つの部分から構成されるバージョン番号を使用 **します。** 形式は `N1.N2.N3.N4` です。 モジュールの作成者は、 **System. version** クラスで許可されている任意のバージョン番号付けスキームを使用できます。 更新可能なヘルプでは、ui カルチャ用の CAB ファイルの新しいバージョンが、HelpInfo XML ファイルの **Helpcontenturi** 要素で指定されている場所にアップロードされたときに、ui カルチャのバージョン番号が増えるだけで済みます。

次の例は、バージョンが2.15.0.10 の場合のドイツ語 (de) UI カルチャの HelpInfo XML ファイルの要素を示しています。

```xml
<UICulture>
  <UICultureName>de-DE</UICultureName>
  <UICultureVersion>2.15.0.10</UICultureVersion>
</UICulture>
```

UI カルチャのバージョン番号は、その UI カルチャの CAB ファイルのバージョンを反映しています。 バージョン番号は CAB ファイル全体に適用されます。 CAB ファイル内のファイルごとに異なるバージョン番号を設定することはできません。 各 UI カルチャのバージョン番号は個別に評価され、モジュールがサポートする他の UI カルチャのバージョン番号とは関係がありません。
