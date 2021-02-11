---
ms.date: 09/12/2016
ms.topic: reference
title: プロバイダーのヘルプ トピックに関連項目を追加する方法
description: プロバイダーのヘルプ トピックに関連項目を追加する方法
ms.openlocfilehash: df0b14ba84e04baf404081944ef62ef6745d74b2
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667659"
---
# <a name="how-to-add-a-see-also-section-to-a-provider-help-topic"></a>プロバイダーのヘルプ トピックに関連項目を追加する方法

このセクションでは、プロバイダーヘルプトピックの **「関連項目」** セクションにデータを設定する方法について説明します。

「関連項目」セクションは、プロバイダーに関連するトピックの一覧で構成され **て** います。また、ユーザーがプロバイダーをよりよく理解し、使用するために役立つ場合もあります。 トピックの一覧には、Windows PowerShell のヘルプトピック (コマンドレットのヘルプ、プロバイダーのヘルプ、および概念説明の "about") を含めることができます。 また、現在のプロバイダーのヘルプトピックのオンラインバージョンを含む、書籍、紙、およびオンラインのトピックへの参照を含めることもできます。

オンライントピックを参照するときは、プレーンテキストで URI または検索語句を指定します。 コマンドレットでは、 `Get-Help` 一覧のどのトピックにもリンクまたはリダイレクトされません。 また、 `Online` コマンドレットのパラメーターは、 `Get-Help` プロバイダーのヘルプでは機能しません。

**「参照**」セクションは、 `RelatedLinks` 要素とそれに含まれるタグから作成されます。
タグを追加する方法を次の XML に示します。

### <a name="to-add-see-also-topics"></a>追加するには、「関連項目」も参照してください。

1. ファイルの `<AssemblyName>.dll-help.xml` 要素内に `providerHelp` 要素を追加 `RelatedLinks` します。 要素は、 `RelatedLinks` 要素の最後の要素である必要があり `providerHelp` ます。 `RelatedLinks`各プロバイダーヘルプトピックでは、1つの要素のみが許可されます。

   次に例を示します。

    ```xml
    <providerHelp>
        <RelatedLinks>
        </RelatedLinks>
    </providerHelp>
    ```

1. **「関連** 項目」セクションの各トピックについて、要素内に `RelatedLinks` 要素を追加し `navigationLink` ます。 次に、各 `navigationLink` 要素内に1つの `linkText` 要素と1つの要素を追加し `uri` ます。 要素を使用していない場合は `uri` 、空の要素 () として追加でき \<uri/> ます。

   次に例を示します。

    ```xml
    <providerHelp>
        <RelatedLinks>
            <navigationLink>
                <linkText> </linkText>
                <uri> </uri>
            </navigationLink>
        </RelatedLinks>
    </providerHelp>
    ```

1. タグの間にトピック名を入力し `linkText` ます。 URI を指定する場合は、タグの間に入力し `uri` ます。 現在のプロバイダーのヘルプトピックのオンラインバージョンを示すには、タグの間に、 `linkText` トピック名の代わりに「online version:」と入力します。 通常、"オンラインバージョン:" リンクは、「関連項目」の一覧の最初のトピックです。

   次の例には、3つのトピックも含まれています。 最初のは、現在のトピックのオンラインバージョンを参照します。 2番目のコマンドレットは、Windows PowerShell コマンドレットのヘルプトピックを参照します。 3番目のトピックでは、別のオンライントピックを参照します。

    ```xml
    <providerHelp>
        <RelatedLinks>
            <navigationLink>
                <linkText> Online version: </linkText>
                <uri>http://www.fabrikam.com/help/myFunction.htm</uri>
            </navigationLink>
            <navigationLink>
                <linkText> about_functions </linkText>
                <uri/>
            </navigationLink>
            <navigationLink>
                <linkText> Windows PowerShell Getting Started Guide </linkText>
                <uri>https://go.microsoft.com/fwlink/?LinkID=89597<uri/>
            </navigationLink>
        </RelatedLinks>
    </providerHelp>
    ```
