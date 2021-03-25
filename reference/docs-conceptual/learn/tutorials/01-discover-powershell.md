---
title: PowerShell を知る
ms.date: 03/12/2021
ms.custom: chnoring
ms.reviewer: sewhee
description: PowerShell の概要と、PowerShell を知るために使用されるいくつかの重要なコマンドについて学びます。
ms.openlocfilehash: 34bbd465a918224c203e7243834e7fb3a3a6070c
ms.sourcegitcommit: 16a02ae47d1a85b01692101aa0aa6e91e1ba398e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2021
ms.locfileid: "104729997"
---
# <a name="discover-powershell"></a>PowerShell を知る

PowerShell は、コマンド ライン シェルとスクリプト言語が一体化されたものです。 PowerShell は Windows から始まりました。 管理タスクについてタスクの自動化を支援することを目的としていましたが、クロス プラットフォームとして拡張され、さまざまなタスクに使用できるようになりました。

PowerShell が他と違うのは、テキストではなく .NET オブジェクトを受け入れて返すことです。
これにより、"_パイプライン_" で、さまざまなコマンドを連続して簡単に接続できます。

> [!NOTE]
> このチュートリアル シリーズでは、パイプラインについてさらに詳しく説明します。

そのような場合でも、結果を少し "_マッサージ_" (変換または操作) しなければならないことがあります。

## <a name="what-can-powershell--be-used-for"></a>PowerShell の用途

PowerShell の用途は、Windows 専用だった頃に比べて広がっています。 Windows タスクの自動化にも使用されますが、今日では、次のようなさまざまなタスクで使用できます。

- **クラウド管理**。 PowerShell は、クラウドのリソースを管理するために使用できます。 たとえば、クラウドのリソースに関する情報を取得したり、新しいリソースを更新または配置したりできます。
- **CI/CD**。 継続的インテグレーション/継続的配置パイプラインの一部として使用することもできます。
- **Active Directory と Exchange のタスクの自動化**。 使用することで、Active Directory のユーザーや、Exchange のメールボックスの作成など、Windows でのほぼすべてのタスクを自動化できます。

用途の範囲は他にも多数ありますが、上記の一覧から PowerShell が長い歴史を経てきたことがわかります。

## <a name="who-uses-powershell"></a>PowerShell を使用するユーザー

PowerShell は非常に強力であるため、さまざまな役割で働く多くの人が、使用によるベネフィットを得られます。 従来、PowerShell はシステム管理者ロールで使用されていましたが、現在は DevOps、Cloud Ops、さらには開発者も使用しています。

## <a name="powershell-cmdlets"></a>PowerShell コマンドレット

PowerShell には、何百ものコマンドがプレインストールされています。 PowerShell コマンドは cmdlet と呼ばれ、"コマンドレット" と発音します。

各コマンドレットの名前は、`Get-Process` のように、"動詞-名詞" のペアで構成されます。 この名前付け規則のおかげで、コマンドレットの動作を簡単に理解できます。 また、探しているコマンドを簡単に見つけられます。 使用するコマンドレットを検索するときは、動詞または名詞でフィルターできます。

### <a name="using-cmdlets-to-explore-powershell"></a>コマンドレットを使用して PowerShell を探索する

PowerShell を初めて選択したときは、学習することが多く見えるため、難しいと思われるかもしれません。
しかし、PowerShell は、必要に応じて少しずつ学習できるように設計されています。

PowerShell には、PowerShell をよく知るのに役立つコマンドレットが含まれています。 これら 4 つのコマンドレットを使用して、使用可能なコマンド、実行内容、および操作の対象となる型を確認できます。

- `Get-Verb`. このコマンドを実行すると、ほとんどのコマンドが従う動詞の一覧が返されます。
  また、応答には、これらの動詞の動作が記述されています。 ほとんどのコマンドがこの名前付け規則に従うため、コマンドの動作 (適切なコマンドの選択に役立ちます)、さらにはコマンドを作成する場合の名前付けに関する期待値が設定されます。
- `Get-Command`. このコマンドでは、コンピューターにインストールされているすべてのコマンドの一覧が取得されます。
- `Get-Member`. オブジェクトベースの出力に対して動作し、コマンドで使用できるオブジェクト、プロパティ、およびメソッドを検出できます。
- `Get-Help`. コマンドの名前を引数として指定してこのコマンドを呼び出すと、コマンドのさまざまな部分について説明するヘルプ ページが表示されます。

これらのコマンドを使用して、PowerShell について知る必要があるほとんどすべてのことを確認できます。

### <a name="verb"></a>動詞

動詞は、PowerShell の重要な概念です。 これは、ほとんどのコマンドレットが従う名前付け基準です。 また、独自のコマンドを記述した場合に従うことが期待されている名前付け基準でもあります。 "_動詞_" は実行しようとしている内容、読み取ろうとしているデータ、変更しようとしている内容を示しています。 PowerShell には、標準化された動詞の一覧があります。 使用可能なすべての動詞の完全な一覧を取得するには、`Get-Verb` コマンドレットを使用します。

```powershell
Get-Verb
```

実行すると、動詞の長い一覧が出力されます。 その応答から、そのような動詞が何を行うかについて、さらなるコンテキストが表示されるということがわかります。 こちらが、出力の先頭行です。

```output
Verb        AliasPrefix Group          Description
----        ----------- -----          -----------
Add         a           Common         Adds a resource to a container, or attaches an item to ano…
```

## <a name="find-commands-with-get-command"></a>Get-Command を使用してコマンドを検索する

`Get-Command` コマンドレットでは、システムにインストールされている使用可能なすべてのコマンドの一覧が返されます。
ただし、返される一覧は非常に大きくなります。 コマンドを簡単に見つけられるように、返される情報の量を制限することができます。 応答をフィルターするには、いずれかのパラメーターを使用するか、ヘルパー コマンドレットを使用します。

### <a name="filter-on-name"></a>名前でフィルターする

さまざまなパラメーターを使用して、`Get-Command` の出力をフィルターできます。 この方法でのフィルターは、コマンドの特定のプロパティに対してクエリを実行するということです。 つまり、フィルターの対象となるプロパティを指定し、照合する文字列を指定するということです。 したがって、次のような比較になります。

```powershell
Get-Command -Name '*Process'
```

この時点で、フィルターでは指定された文字列引数と完全に一致するかどうかが試行されます。
より柔軟にする場合は、比較で、パターン マッチングを行うワイルドカード `*` を使用できます。 次のコードでは、名前が process で終わるすべてのコマンドが検索されます。

上記のパラメーター `-Name` はフィルターに使用されます。 `-Name` とは別に、`-ParameterName` や `-Type` などにもフィルターを設定できます。

### <a name="filtering-on-noun-and-verb"></a>名詞と動詞に対するフィルター

`-Name` でのフィルターについて確認し、他にもフィルターを適用できるパラメーターがあることがわかりました。 動詞と名詞にも、フィルターを適用できます。 そのようなフィルターでは、コマンドの名前の一部が対象となります。

- **動詞をフィルターする**。 コマンドの名前の動詞部分は、左端の部分です。 コマンド `Get-Process` の動詞部分は `Get` です。 動詞部分をフィルターするには、次のように `-Verb` をパラメーターとして指定します。

   ```powershell
   Get-Command -Verb 'Get'
   ```

   上記のコマンドでは、動詞部分が `Get` であるすべてのコマンドが一覧表示されます。

- **名詞をフィルターする**。 コマンドの右端の部分が、名詞部分です。 動詞は、`Get-Verb` の呼び出しから返された動詞の中にある必要があります。名詞は任意のものにすることができます。 コマンド `Get-Process` の、名詞部分は `Process` です。 名詞をフィルターするには、次のように `-Noun` をパラメーターと文字列引数として指定します。

   ```powershell
   Get-Command -Noun U*
   ```

動詞 (または名詞) のみを使用してフィルターすると、やはり結果が大きくなる場合があります。 検索を絞り込むには、下の例のように 2 つのパラメーターを組み合わせることをお勧めします。

```powershell
Get-Command -Verb Get -Noun U*
```

上記の結果は、次のようになります。

```output
CommandType     Name                         Version    Source
-----------     ----                         -------    ------
Cmdlet          Get-UICulture                7.0.0.0    Microsoft.PowerShell.Utility
Cmdlet          Get-Unique                   7.0.0.0    Microsoft.PowerShell.Utility
Cmdlet          Get-Uptime                   7.0.0.0    Microsoft.PowerShell.Utility
Cmdlet          Get-UsageAggregates          2.0.0      Az.Billing
```

動詞とそれによって呼び出されるものを知ることで、出力がかなり絞り込まれました。

### <a name="use-helper-cmdlets-to-filter-results"></a>ヘルパー コマンドレットを使用して結果をフィルターする

パラメーターを使用してフィルターする以外に、コマンドを使用してこのタスクを実行することもできます。 フィルターとして機能するコマンドをいくつか次に示します。

- `Select-Object`. これは、1 つまたは複数のオブジェクトから特定のプロパティを選択するのに役立つ、非常に汎用性の高いコマンドです。 さらに、このパラメーターでは、返される応答を制限することもできます。 `Select-Object` を使用して限られた数のレコードを要求する場合の例を示します。

   ```powershell
   Get-Command | Select-Object -First 3
   ```

   先頭からカウントされた最初の 3 つのコマンドが上記の結果になります。 結果は次のようになります。

   ```output
   CommandType     Name                                               Version    Source
   -----------     ----                                               -------    ------
   Alias           Add-AdlAnalyticsDataSource                         1.0.2      Az.DataLakeAnalytics
   Alias           Add-AdlAnalyticsFirewallRule                       1.0.2      Az.DataLakeAnalytics
   Alias           Add-AdlStoreFirewallRule                           1.3.0      Az.DataLakeStore
   ```

   このコマンドでは他にも多くのことができるので、さらに詳しく調べる価値があります ([Select-Object のドキュメント](/powershell/module/microsoft.powershell.utility/select-object))

- `Where-Object`. Where-Object は、プロパティの値に基づいてコレクションからオブジェクトを選択する場合に役立ちます。 コマンドは式を受け取り、そこでは、どの列をどの値と照合するかを表現できます。 `ProcessName` が `p` で始まるすべてのプロセス オブジェクトを検索するには、`Where-Object` を次のように使用することができます。

  ```powershell
  Get-Process | Where-Object {$_.ProcessName -Like "p*"}
  ```

  上記の `Get-Process` コマンドレットでは、プロセス オブジェクトのコレクションが生成されます。 応答をフィルターするには、コマンド `Where-Object` を "_パイプ_" します。 パイプは、2 つ以上のコマンドがパイプ文字 `|` を介して接続されることを意味します。 1 つのコマンドからの出力が、左から右に読み取られるときに、次のコマンドの入力として機能するということです。 `Where-Object` では、フィルターに式が使用されます。 式自体は、`-Like` 演算子と、ワイルドカード式である文字列引数を使用します。

## <a name="explore-objects-with-get-member"></a>Get-Member を使用してオブジェクトを探索する

必要なコマンドレットを見つけたら、何が出力されるかについて詳しく確認してみましょう。 出力は、次のようないくつかの理由で興味深いものです。

- **スタンドアロン**。 1 つのコマンドレットを実行するだけで、出力を何らかの種類のレポートに表示することができます。 自分にとって意味のある出力をコマンドが生成するかどうか、またはそれを変更する必要があるかどうかをご確認ください。
- **パイプラインで使用される場合**。 多くの場合、PowerShell では、パイプライン内の複数のコマンドを接続して、データをフェッチし、フィルターして、最終的に変換することができます。 コマンドがパイプラインに適合するようにするには、どのような入力と出力が生成されるかを確認する必要があります。 これは、コマンドの出力が別のコマンドの入力として使用されることを意味します。

`Get-Member` コマンドレットでは、結果のプロパティとメソッドが表示されます。 さらに、オブジェクトの型も表示されます。 検査する出力を `Get-Member` にパイプします。

```powershell
Get-Process | Get-Member
```

結果には、`TypeName` として戻り値の型と、オブジェクトのすべてのプロパティとメソッドが表示されます。 このような結果の抜粋を次に示します。

```output
TypeName: System.Diagnostics.Process

Name        MemberType     Definition
----        ----------     ----------
Handles     AliasProperty  Handles = Handlecount
Name        AliasProperty  Name = ProcessName
```

通常、オブジェクトには数多くのプロパティとメソッドがあり、探しているものを簡単に見つけられるように、結果をフィルターできます。 パラメーター `-MemberType` を使用して、たとえば、次の例のように、すべてのメソッドが表示されるように指定できます。

```powershell
Get-Process | Get-Member -MemberType Method
```

応答が返された場合、PowerShell は通常、一部のプロパティのみを表示します。 上記の応答では `Name`、`MemberType` と `Definition` が表示されました。 この表示を変更するには、コマンドレット `Select-Object` を選択できます。 `Select-Object` では表示する列を指定できます。 これには、列の名前、コンマ区切りのリスト、またはワイルドカード `*` を指定できます。 `Select-Object` を使用して `Name` と `Definition` を取得する例を示します。

```powershell
Get-Process | Get-Member | Select-Object Name, Definition
```

### <a name="search-by-type"></a>型で検索する

目的のコマンドを検索する別の方法として、同じ型に対して動作するすべてのコマンドを検索する方法もあります。 `Get-Member` を使用すると、次のように、戻り値の型が応答の最初の行として取得されます。

```output
TypeName: System.Diagnostics.Process
```

この型を使用して、次のようにコマンドを検索できるようになりました。

```powershell
Get-Command -ParameterType Process
```

上記を呼び出した結果のリストには、`Process` 型に対してのみ動作するコマンドが含まれます。

```output
CommandType     Name                         Version    Source
-----------     ----                         -------    ------
Cmdlet          Debug-Process                7.0.0.0    Microsoft.PowerShell.Managem…
Cmdlet          Enter-PSHostProcess          7.1.0.0    Microsoft.PowerShell.Core
Cmdlet          Get-Process                  7.0.0.0    Microsoft.PowerShell.Managem…
Cmdlet          Get-PSHostProcessInfo        7.1.0.0    Microsoft.PowerShell.Core
Cmdlet          Stop-Process                 7.0.0.0    Microsoft.PowerShell.Managem…
Cmdlet          Wait-Process                 7.0.0.0    Microsoft.PowerShell.Managem…
```

ご覧のように、コマンドの種類を知っておくと、興味深いコマンドの後に検索を大幅に絞り込むことができます。

## <a name="exercise---calling-your-first-command"></a>演習 - 最初のコマンドの呼び出し

この演習では、最初のコマンドを実行する方法について学習します。

1. `pwsh` と入力して、PowerShell コンソールを起動します。

   ```powershell
   pwsh
   ```

1. 次の `$PSVersionTable.PSVersion` を実行します。

   ```powershell
   $PSVersionTable.PSVersion
   ```

   出力の例を次に示します。

   ```output
   Major  Minor  Patch  PreReleaseLabel BuildLabel
   -----  -----  -----  --------------- ----------
   7      1      0
   ```

   おめでとうございます。これで、最初のコマンドを正常に実行し、システムにインストールされている PowerShell のバージョンに関する情報を取得できるようになりました。

## <a name="exercise---find-related-commands"></a>演習 - 関連するコマンドを見つける

この演習の目標は、コマンドについて詳しく理解することです。 これにより、操作対象の型や、同じ型に対して実行される他の類似するコマンドなどもわかります。

1. PowerShell シェルが確実に開始されているようにします
1. コマンド `Get-Process` を実行します。

   ```powershell
   Get-Process | Get-Member | Select-Object TypeName -Unique
   ```

   次のような内容が出力されます。

   ```output
   TypeName
   --------
   System.Diagnostics.Process
   --------
   ```

   戻り値は、コマンド `Get-Command` によって返される型です。 この時点で、他にどのようなコマンドがこれらの型に対して動作するかを確認できます。

1. コマンド `Get-Command` を実行します。

   ```powershell
   Get-Command -ParameterType Process
   ```

   次のような内容が出力されます。

   ```output
   CommandType     Name                         Version    Source
    -----------     ----                        -------    ------
    Cmdlet          Debug-Process               7.0.0.0    Microsoft.PowerShell.Managem…
    Cmdlet          Enter-PSHostProcess         7.1.0.0    Microsoft.PowerShell.Core
    Cmdlet          Get-Process                 7.0.0.0    Microsoft.PowerShell.Managem…
    Cmdlet          Get-PSHostProcessInfo       7.1.0.0    Microsoft.PowerShell.Core
    Cmdlet          Stop-Process                7.0.0.0    Microsoft.PowerShell.Managem…
    Cmdlet          Wait-Process                7.0.0.0    Microsoft.PowerShell.Managem…
   ```

   これで、同じ型 `Process` で動作する他のコマンドがわかりました。
   最初に `Get-Member` から始めると、次にどのコマンドについて確認すべきかを理解できるでしょう。

## <a name="summary"></a>まとめ

この最初のパートでは、PowerShell の概要と、使用できる範囲について学習しました。 コマンドレット、特に `Get-Command`、`Get-Verb`、`Get-Member` について学習しました。 これらのコマンドレットから学習方法がわかるため、知っておくことが重要です。 次のパートでは、強力なヘルプ システムの使用方法について説明します。

### <a name="additional-resources"></a>その他のリソース

- [Get-Command](xref:Microsoft.PowerShell.Core.Get-Command)
- [Get-Member](xref:Microsoft.PowerShell.Utility.Get-Member)
