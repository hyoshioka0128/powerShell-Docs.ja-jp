---
description: Powershell の新しいバージョンがリリースされたことをユーザーに通知します。
Locale: en-US
ms.date: 01/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_update_notifications?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Update_Notifications
ms.openlocfilehash: 1a9f8cfec15f83566fdb77175dcc1aed6d9e8c34
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605435"
---
# <a name="about-update-notifications"></a>更新通知について

## <a name="short-description"></a>概要

Powershell の新しいバージョンがリリースされたことをユーザーに通知します。

## <a name="long-description"></a>詳細説明

Powershell 7.0 以降では、powershell は更新通知を使用して、PowerShell への更新が存在することをユーザーに警告します。 PowerShell は、新しいバージョンが利用可能かどうかを、1 日に 1 回オンライン サービスに問い合わせます。

> [!NOTE]
> 特定の24時間の期間に最初のセッション中に更新チェックが行われますが、パフォーマンス上の理由から、通知は後続のセッションの開始時にのみ表示されます。 また、パフォーマンス上の理由から、セッションが開始されてから少なくとも3秒が経過するまでチェックは開始されません。

既定では、PowerShell は、そのバージョン/分岐に応じて、2 つの異なる通知チャネルのいずれかをサブスクライブします。 サポートされている一般公開 (GA) バージョンの PowerShell では、更新された GA リリースの通知のみが返されます。 プレビューおよびリリース候補 (RC) リリースでは、プレビュー、RC、GA リリースに対する更新プログラムが通知されます。

更新通知動作は、`POWERSHELL_UPDATECHECK` 環境変数を使用して変更できます。 サポートされている値を次に示します。

- `Off` 更新通知機能をオフにします。
- `Default` が次のように定義されていません `POWERSHELL_UPDATECHECK` 。
  - GA リリースでは、GA リリースに対する更新プログラムが通知されます
  - プレビュー/RC リリースでは、GA およびプレビュー リリースに対する更新プログラムが通知されます
- `LTS` 長期的なサービス (LTS) GA リリースの更新についてのみ通知します

次のエンドポイントは、各チャネルの最新バージョンを決定するために現在使用されています。

- `LTS`: https://aka.ms/pwsh-buildinfo-lts
- `Stable`: https://aka.ms/pwsh-buildinfo-stable
- `Preview`: https://aka.ms/pwsh-buildinfo-preview

更新通知には、PowerShell を自動的に更新する方法は用意されていません。 今後、PowerShell 内から更新するための手順や機能が増えている場合がありますが、現在は PowerShell のインストールに使用したのと同じインストールメカニズムを使用して更新する必要があります。

