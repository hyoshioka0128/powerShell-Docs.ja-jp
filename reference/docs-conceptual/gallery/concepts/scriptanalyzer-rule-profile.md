---
ms.date: 06/12/2017
title: ギャラリーの ScriptAnalyzer 規則プロファイル
description: PowerShell ScriptAnalyzer が PowerShell ギャラリーとどのように統合されるかについて説明します。
ms.openlocfilehash: 3af710e8811f0fabfb02f5317d5b4ff9c320f29a
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92646763"
---
# <a name="scriptanalyzer-rule-profile-for-gallery"></a>ギャラリーの ScriptAnalyzer 規則プロファイル

PowerShell ギャラリーに公開されるパッケージの品質を確保するために、[PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) 規則を実行し、送信されたスクリプトに違反がないか確認します。

ScriptAnalyzer [GitHub ページ](https://github.com/PowerShell/PSScriptAnalyzer/blob/development/Engine/Settings/PSGallery.psd1)で実行されているルールの一覧を参照できます。
実行されている規則について懸念がある場合は、PowerShell ギャラリー管理者に問い合わせるか、ScriptAnalyzer の問題が未解決であるという設定をしてください。

今後リリースされるギャラリーでは、ScriptAnalyzer の結果が、個別のパッケージのページに表示されます。 パッケージの所有者が、公開されたパッケージ内に重大なエラーがないことを確認することをお勧めします。
