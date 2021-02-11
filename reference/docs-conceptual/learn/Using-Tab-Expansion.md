---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: タブ拡張の使用
description: PowerShell でタブ拡張機能を使用する方法について説明します。
ms.openlocfilehash: 658cdf5ddf78bbd6dd431c2170cd5ff643e6bf95
ms.sourcegitcommit: 7f712e12ec5b3f3f3e695da804b050ea0ce58b3a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "94661343"
---
# <a name="using-tab-expansion"></a>タブ拡張の使用

コマンド ライン シェルは多くの場合、長いファイルやコマンドの名前を自動補完する手段を備えており、コマンド入力を迅速化したり、ヒントを与えたりします。 PowerShell では、<kbd>Tab</kbd> キーを押してファイル名やコマンドレット名を入力することができます。

> [!NOTE]
> タブ拡張は、内部関数 TabExpansion または TabExpansion2 によって制御されています。 この関数は変更やオーバーライドができるため、この説明は既定の PowerShell 構成の動作についてのガイドとなっています。

使用可能な選択肢からファイル名またはパスを自動的に入力するには、名前の一部を入力して <kbd>Tab</kbd> キーを押します。 PowerShell は自動的に名前を拡張し、最初に見つかった一致する項目を表示します。 <kbd>Tab</kbd> キーを繰り返し押すと、すべての利用可能な選択肢が順番に表示されます。

コマンドレット名のタブ拡張は若干異なります。 コマンドレット名にタブ拡張を使用するには、名前の最初の部分全体 (動詞) とそれに続くハイフンを入力します。 部分一致するよう、名前をより詳細に入力することもできます。 たとえば、「`get-co`」と入力して <kbd>Tab</kbd> キーを押すと、これが PowerShell によって `Get-Command` コマンドレットに自動的に展開されます (文字の大文字と小文字の区別もその標準形式に変更されることに注目してください)。 もう一度 <kbd>Tab</kbd> キーを押すと、PowerShell によってこれが他の一致する唯一のコマンドレット名 `Get-Content` に置き換えられます。

> [!NOTE]
> また、PowerShell 7.0 より、<kbd>Tab</kbd> キーで省略形のコマンドレットや関数が完全な形で示されます。 たとえば、`i-psdf<tab>` では `Import-PowerShellDataFile` が返されます。

同じ行にタブ拡張を繰り返し使用できます。 たとえば、`Get-Content` コマンドレットの名前に対してタブ展開を使用するには、次のように入力できます。

```
PS> Get-Con<Tab>
```

<kbd>Tab</kbd> キーを押すと、コマンドは次のように拡張されます。

```
PS> Get-Content
```

次に、アクティブ セットアップのログ ファイルのパスを部分的に指定し、もう一度タブ拡張を使用できます。

```
PS> Get-Content c:\windows\acts<Tab>
```

<kbd>Tab</kbd> キーを押すと、コマンドは次のように拡張されます。

```
PS> Get-Content C:\windows\actsetup.log
```

> [!NOTE]
> タブ拡張プロセスの 1 つの制限は、タブがあると、必ず単語の補完を試みていると解釈されることです。 コマンドの例をコピーして PowerShell コンソールに貼り付ける場合は、サンプルにタブが含まれていないことを確認ください。タブが含まれていると結果は予測できず、ほぼ確実に意図したとおりにはなりません。
