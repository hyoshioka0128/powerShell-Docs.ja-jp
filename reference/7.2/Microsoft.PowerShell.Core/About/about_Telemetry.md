---
description: PowerShell で収集されたテレメトリと、オプトアウトする方法について説明します。
Locale: en-US
ms.date: 08/09/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_telemetry?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Telemetry
ms.openlocfilehash: 677d43b405fdc92e6b68d6e903847b11cb671a2c
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602412"
---
# <a name="about-telemetry"></a>テレメトリについて

## <a name="short-description"></a>概要

PowerShell で収集されたテレメトリと、オプトアウトする方法について説明します。

## <a name="long-description"></a>詳細説明

PowerShell は、基本的なテレメトリデータを Microsoft に送信します。
このデータにより、PowerShell が使用されている環境をより深く理解し、新機能と修正の優先順位を付けることができます。
次のテレメトリポイントが記録されます。

- PowerShell の種類で開始 (API vs コンソール)
- 一意の PowerShell 使用状況のカウント
- 次の実行の種類の数。
  - アプリケーション (ネイティブコマンド)
  - ExternalScript
  - スクリプト
  - Function
  - コマンドレット
- PowerShell に付属している、Microsoft が所有する試験的な機能または試験的な機能の数
- ホストされているセッションの数
- Microsoft 所有のモジュールが読み込まれた回数

このデータには、OS 名、OS バージョン、PowerShell のバージョン、および配布チャネルが含まれます。

このテレメトリをオプトアウトするには、環境変数 POWERSHELL_TELEMETRY_OPTOUT を true、yes、または1に設定します。

