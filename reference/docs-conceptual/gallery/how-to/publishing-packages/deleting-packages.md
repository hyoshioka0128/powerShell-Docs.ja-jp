---
ms.date: 06/12/2017
title: PowerShell ギャラリーからパッケージを削除する方法
description: PowerShell ギャラリーからパッケージを削除する方法
ms.openlocfilehash: e02fad61ce8ab808ba9fdf4ab16b9ae236a49e80
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662576"
---
# <a name="deleting-packages"></a>パッケージを削除する

PowerShell ギャラリーでは、パッケージを完全に削除することはできません。パッケージの依存関係がなくなるためです。

代わりに、PowerShell ギャラリーでは、Web サイトのパッケージ管理ページでパッケージを "リストから外す" ことができます。 パッケージがリストから外されると、PowerShell ギャラリーでも、PowerShellGet コマンドを使用しても、検索やパッケージ リストにそのパッケージが表示されなくなります。
ただし、正しいバージョンを指定すればダウンロードできます。正しいバージョンを指定することで、自動スクリプトが引き続き動作します。

パッケージを削除しなければならない、例外的な状況になった場合、PowerShell ギャラリー チームが手動で削除します。 たとえば、著作権の侵害や有害なコンテンツがあれば、それはアイテムを削除する正当な理由となります。 その場合、[PowerShell ギャラリー](https://www.PowerShellGallery.com)からサポート要求を提出してください。
