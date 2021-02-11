---
ms.date: 06/12/2017
title: ギャラリー検索構文
description: この記事では、PowerShell ギャラリーでのコンテンツ検索に使用されるユーザー インターフェイスについて説明します。
ms.openlocfilehash: 7ad486095647f99056b37c2ca1a50db099a166c0
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661362"
---
# <a name="gallery-search-syntax"></a>ギャラリー検索構文

[PowerShell ギャラリーの Web サイト](https://www.powershellgallery.com/)を使って、PowerShell ギャラリーを検索することができます。 PowerShell ギャラリーの Web サイトでは単語、フレーズ、キーワード表現を使用して検索結果を絞り込むテキスト検索ボックスが用意されています。

## <a name="search-by-keywords"></a>キーワードで検索

```Syntax
dsc azure sql
```

検索では、3 つのキーワードすべてを含む関連ドキュメントの検索が試みられ、一致するドキュメントが返されます。

## <a name="search-using-phrases-and-keywords"></a>フレーズとキーワードを使用して検索

```Syntax
"azure sql" deployment
```

引用符 ("") の間にフレーズを入力すると、個々のキーワードではなく、特定のフレーズの検索に変わります。 一致するドキュメントには通常、大文字と小文字のバリエーション (例: "Azure SQL" など) を含む "azure sql" そのままのフレーズを含み、また通常は 'deployment' という単語も含まれます。

## <a name="filtering-on-fields"></a>フィールドのフィルター処理

特定のパッケージ ID (または 'Id'、'id') を検索するか、検索語句の前にフィールド名を付けて他の特定のフィールドを検索します。

検索可能フィールドは現在、'Id'、'Version'、'Tags'、'Author'、'Owner'、'Functions'、'Cmdlets'、'DscResources'、'PowerShellVersion' です。

- ID とは、コンソールで使用する名前です。
- タイトルとは、検索結果のパッケージ ページの上部に表示される内容です。

## <a name="examples"></a>例

```Syntax
ID:PSReadline
```

ID に "PSReadline" が含まれるパッケージを検索します。

```Syntax
Id:"AzureRM.Profile"
```

ID フィールドで "AzureRM.Profile" のあるパッケージを検索する別の方法です。

'Id' フィルターは部分文字列の一致のため、次のように検索した場合、

```Syntax
Id:"azure"
```

これにより、"AzureRM.Profile" と "Azure.Storage" を含む結果が提供されます。

また、1 つのフィールドで複数のキーワードを検索することもできます。

```Syntax
id:azure tags:intellisense
```

また、二重引用符を使ってフレーズ検索を行うこともできます。

```Syntax
id:"azure.storage"
```

DSC タグのあるすべてのパッケージを検索します。

```Syntax
Tags:DSC
```

指定した関数のあるすべてのパッケージを検索します。

```Syntax
Functions:Get-TreeSize
```

指定したコマンドレットのあるすべてのパッケージを検索します。

```Syntax
Cmdlets:Get-AzureRmEnvironment
```

指定した DSC リソース名のあるすべてのパッケージを検索します。

```Syntax
DscResources:xArchive
```

指定した PowerShellVersion のあるすべてのパッケージを検索します

```Syntax
PowerShellVersion:2.0
```

最後に、'commands' など、サポートされていないフィールドを使用すると、単に無視され、すべてのフィールドが検索されます。 そのため、次のクエリ

```Syntax
commands:blobs storage
```

は次のクエリと同じものとして解釈されます。

```Syntax
blobs storage
```
