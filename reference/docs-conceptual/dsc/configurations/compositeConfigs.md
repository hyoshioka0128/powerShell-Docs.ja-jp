---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: 構成を入れ子にする
description: DSC を使用すると、構成を別の構成の中に入れ子にすることで複合構成を作成できます。
ms.openlocfilehash: d7a81cb9673126e92e9185aacf19c5c7c17da8ca
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92667421"
---
# <a name="nesting-dsc-configurations"></a>DSC 構成を入れ子にする

入れ子構成 (複合構成とも呼ぶ) とは、別の構成内でリソースとして呼び出される構成です。 両方の構成は、同じファイルで定義する必要があります。

簡単な例を見てみましょう。

```powershell
Configuration FileConfig
{
    param (
        [Parameter(Mandatory = $true)]
        [String] $CopyFrom,

        [Parameter(Mandatory = $true)]
        [String] $CopyTo
    )

    Import-DscResource -ModuleName PSDesiredStateConfiguration

    File FileTest
    {
        SourcePath = $CopyFrom
        DestinationPath = $CopyTo
        Ensure = 'Present'
    }
}

Configuration NestedFileConfig
{
    Node localhost
    {
        FileConfig NestedConfig
        {
            CopyFrom = 'C:\Test\TestFile.txt'
            CopyTo = 'C:\Test2'
        }
    }
}
```

この例では、`FileConfig` は **CopyFrom** および **CopyTo** という 2 つの必須パラメーターを受け取り、それらは `File` リソース ブロック内の **SourcePath** プロパティおよび **DestinationPath** プロパティの値として使用されます。 `NestedConfig` 構成は `FileConfig` をリソースとして呼び出します。 `NestedConfig` リソース ブロック内のプロパティ ( **CopyFrom** と **CopyTo** ) は、`FileConfig` 構成のパラメーターです。

現在、DSC では、入れ子になった構成内に構成を入れ子にすることはサポートされていません。 構成を入れ子にできるレイヤーの深さは 1 つだけです。

## <a name="see-also"></a>参照

- [複合リソース: リソースとしての DSC 構成の使用](../resources/authoringResourceComposite.md)
