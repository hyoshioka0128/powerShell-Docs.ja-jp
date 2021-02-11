---
ms.date: 09/13/2016
ms.topic: reference
title: 書式設定ファイルを作成する方法 (.format.ps1xml)
description: 書式設定ファイルを作成する方法 (.format.ps1xml)
ms.openlocfilehash: 5bbc1ba40bfccf13636abc0f0751938aa724b761
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651993"
---
# <a name="how-to-create-a-formatting-file-formatps1xml"></a>書式設定ファイルを作成する方法 (.format.ps1xml)

このトピックでは、書式設定ファイル (.format.ps1xml) を作成する方法について説明します。

> [!NOTE]
> また、Windows PowerShell によって提供されるいずれかのファイルのコピーを作成することによって、フォーマットファイルを作成することもできます。 既存のファイルのコピーを作成する場合は、既存のデジタル署名を削除し、独自の署名を新しいファイルに追加します。

### <a name="to-create-a-formatps1xml-file"></a>.format.ps1xml ファイルを作成します。

1. メモ帳などのテキストエディターを使用して、テキストファイル (.txt) を作成します。

2. 次の行を書式設定ファイルにコピーします。

   ```xml
   <?xml version="1.0" encoding="utf-8" ?>
   <Configuration>
   <ViewDefinitions>
   </ViewDefinitions>
   </Configuration>
   ```

   - タグによっ `<Configuration></Configuration>` て、ルートノードが定義され `Configuration` ます。 その他のすべての XML タグは、このノード内で囲まれます。

   - タグによっ `<ViewDefinitions></ViewDefinitions>` てノードが定義され `ViewDefinitions` ます。 すべてのビューは、このノード内で定義されます。

3. Windows PowerShell のインストールフォルダー、モジュールフォルダー、またはモジュールフォルダーのサブフォルダーにファイルを保存します。 ファイルを保存するときは、次の名前形式を  `MyFile.format.ps1xml` 使用します。 フォーマットファイルでは、拡張機能を使用する必要があり `.format.ps1xml` ます。

   これで、書式設定ファイルにビューを追加する準備ができました。 書式設定ファイルで定義できるビューの数に制限はありません。 オブジェクトごとに1つのビュー、同じオブジェクトに対して複数のビュー、または複数のオブジェクトで使用される1つのビューを追加できます。

## <a name="see-also"></a>参照

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
