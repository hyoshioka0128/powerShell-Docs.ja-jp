---
ms.date: 07/29/2020
keywords: powershell,コマンドレット
ms.topic: how-to
title: PowerShell ドキュメントの使用方法
description: この記事では、検索のフィルター処理やバージョンの選択など、このサイトの機能を使用する方法について説明します。
ms.openlocfilehash: 4779e6e4b17c461d71e9d613d1184b9ce2e7ab7b
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "98216084"
---
# <a name="how-to-use-the-powershell-documentation"></a>PowerShell ドキュメントの使用方法

PowerShell のオンライン ドキュメントへようこそ。 このサイトには、次のバージョンの PowerShell のコマンドレット リファレンスが含まれています。

- PowerShell 7.2 (プレリリース)
- PowerShell 7.1 (現在)
- PowerShell 7.0 (LTS)
- PowerShell 5.1

## <a name="finding-articles-and-selecting-a-version"></a>記事の検索とバージョンの選択

ドキュメントでコンテンツを検索する方法が 2 とおりあります。最も簡単な方法は、バージョン セレクターの下にあるフィルター ボックスを使用することです。 記事のタイトルに表示される単語を入力してください。 一致する記事の一覧がページに表示されます。 その一覧からサイト全体を検索するオプションを選択することもできます。

既定では、このサイトには PowerShell の最新リリース バージョンのドキュメントが表示されます。 コマンドレットの中には、PowerShell のバージョンによって動作が異なるものがあります。 使用している PowerShell のバージョンのドキュメントを表示していることを確認してください。

ページの上部にあるバージョン ピッカーを使用し、目的の PowerShell のバージョンを選択します。

![バージョン ピッカーの使用](media/how-to-use-docs/version-search.gif)

`$PSversionTable.PSVersion` の値を調べることで、お客様が使用している PowerShell のバージョンを確認できます。 次の例は、Windows PowerShell 5.1 に対する出力を示しています。

```powershell
$PSVersionTable.PSVersion
```

```Output
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      19041  1
```

PowerShell を初めて使用する場合に、コマンドの構文を理解するためにヘルプが必要な場合は、「[コマンド構文について](/powershell/module/microsoft.powershell.core/about/about_command_syntax)」を参照してください。

## <a name="finding-articles-for-previous-versions"></a>以前のバージョンの記事の検索

PowerShell の以前のバージョンのドキュメントは、[以前のバージョン](https://aka.ms/PSLegacyDocs)のサイトにアーカイブされています。

このサイトには、次のトピックのドキュメントが含まれています。

- PowerShell 3.0
- PowerShell 4.0
- PowerShell 5.0
- PowerShell 6
- PowerShell ワークフロー
- PowerShell Web Access
