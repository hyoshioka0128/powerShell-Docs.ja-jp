---
ms.date: 03/27/2018
title: PowerShell ギャラリー GDPR コンプライアンス
description: この記事では、PowerShell ギャラリーから個人データを削除する方法を説明します。これは、GDPR の下での義務を遂行するのに役立ちます。
ms.openlocfilehash: bd2537f48367a9b6ee3a9d70380b1f32d0a1a651
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661152"
---
# <a name="powershell-gallery-gdpr-compliance"></a>PowerShell ギャラリー GDPR コンプライアンス

## <a name="overview"></a>概要

2018 年 5 月に、ヨーロッパの個人情報保護法、一般データ保護規制 (GDPR) が有効になりました。 GDPR は、欧州連合 (EU) 内で人々に製品やサービスを提供したり、EU 居住者に関連するデータを収集および分析したりする企業、政府機関、非営利組織やその他の組織を対象とする新しいルールです。 GDPR は現住所に関わらず適用されます。

> [!NOTE]
> この記事は、PowerShell ギャラリーから個人データを削除する手順を提供するため、GDPR の下での義務の遂行に役立ちます。 GDPR に関する一般情報については、[Service Trust Portal の GDPR セクション](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)をご覧ください。

## <a name="personally-identifiable-data"></a>個人を特定できるデータ

PowerShell ギャラリーには、ユーザーから提供され、個人情報が含まれる可能性がある、次の情報が格納されます。

- PowerShell ギャラリーのアカウント
- PowerShell ギャラリーで公開されるパッケージ
- PowerShell ギャラリー チームとの電子メールのやり取り

ほとんどのユーザーは、PowerShell ギャラリーのアカウントを作成しません。 パッケージを公開する予定がない場合や、PowerShell ギャラリーで "所有者に連絡" 機能を使用しない場合は、アカウントは必要ありません。 ユーザーから開始された電子メールのやり取りを除き、PowerShell ギャラリーにはアカウントを作成していないユーザーの個人を特定できるデータは格納されません。

PowerShell ギャラリーのアカウントを作成したユーザーは、PowerShell ギャラリーにパッケージを公開することができます。 このパッケージは、PowerShell コードになると予想されますが、個人情報を含むその他の情報が含まれることがあります。 次の情報は、PowerShell ギャラリーに公開されたすべてのパッケージを取得する方法を示しています。

## <a name="dsr-export-of-powershell-gallery-data"></a>PowerShell ギャラリー データの DSR エクスポート

次のセクションは、PowerShell ギャラリーがデータ サブジェクト要求 (DSR) をどのようにサポートするかを、PowerShell ギャラリーに格納された情報をエクスポートする方法や、この情報の削除を依頼する方法について説明しています。

### <a name="email"></a>Email

電子メールのやり取りには、次のいずれかが含まれます。

- コード分析のスキャンで PowerShell ギャラリーに公開されたパッケージに関する問題検出された場合に、PowerShell ギャラリーのパッケージの所有者に送信された電子メール
- いずれかのユーザーによって、"お問い合わせ先" ページの電子メール アドレス ([cgadmin@microsoft.com](mailto:cgadmin@microsoft.com)) を使用して PowerShell ギャラリー チームに送信された電子メール
- PowerShell ギャラリーの "所有者に連絡" 機能を使用して PowerShell ギャラリーのパッケージの所有者に電子メールを送信する登録済みユーザー

悪意のあるコードが PowerShell ギャラリーで検出された場合に想定されるセキュリティ調査をサポートするため、PowerShell ギャラリーとの間で送受信された電子メールには、90 日間のアイテム保持ポリシーが適用されます。 このポリシーにより、電子メールは 90 日後に削除されます。

90 日以内であれば、ご自分の電子メール アドレスと PowerShell ギャラリーとの間で送受信されたすべてのメールのコピーを要求することができます。 このやり取りを要求するには、次の件名のメールを [cgadmin@microsoft.com](mailto:cgadmin@microsoft.com) に送信します: "DSR Request for emails relating to this account" (このアカウントに関連のあるメールの DSR 要求)。 メッセージの本文には、要求する情報を記述します (例: "Please send all emails sent to or received from this email address" (このメール アドレスで送受信されたすべてのメールを送ってください))。ご自分の電子メール アドレスに関する、要求から 90 日以内のすべてのメールが 7 営業日以内に送信されます。

### <a name="powershell-gallery-account-information"></a>PowerShell ギャラリーのアカウント情報

PowerShell ギャラリーのアカウントを作成している場合、次の手順を実行すると PowerShell ギャラリーに格納されているすべての個人情報を確認することができます。

1. PowerShell ギャラリーにサインインし、ユーザー名をクリックします
2. PowerShell ギャラリーのアカウントで使用される電子メール アドレスを示す [アカウント] ページが表示されます

PowerShell ギャラリーで複数のアカウントを作成した場合は、アカウントごとにこの手順を繰り返す必要があります。

### <a name="packages-in-the-powershell-gallery"></a>PowerShell ギャラリーのパッケージ

PowerShell ギャラリーに公開されたパッケージのエクスポートを迅速に行うため、PowerShell ギャラリーに "GetPSGalleryItemsForAuthor" のスクリプトを公開しました。 このスクリプトにより、パッケージに格納された作成者情報に基づき、PowerShell ギャラリーに配置されたすべてのパッケージのすべてのバージョンのコピーがエクスポートされます。

> [!NOTE]
> 作成者は、自分のパッケージを公開するときに、パッケージ マニフェストに格納されます。 作成者が PowerShell ギャラリーで使用するアカウントと同じ ID である保証はありません。 [作成者] フィールドで他の値を使用する場合は、このスクリプトを使用するときに、その値を指定する必要があります。

スクリプトは次の PowerShell コマンドを使用してダウンロードできます。

```powershell
Save-Script Get-repository psgallery
```

これで、次の PowerShell コマンドを実行してスクリプトを直接実行できるようになります。

```powershell
# cd <local folder location>
.\GetPSGalleryItemsForAuthor.ps1
```

作成者と、自分のシステム上でパッケージを保存するフォルダーを指定するように求められます。

## <a name="deleting-personal-data-from-the-powershell-gallery"></a>PowerShell ギャラリーから個人データを削除する

自分の PowerShell ギャラリーのアカウント、または PowerShell ギャラリーで所有しているパッケージを削除するには、次の件名のメールを cgadmin@microsoft.com に送信します: "GDPR Request for items relating to this account" (このアカウントに関連のある項目の GDPR 要求)。 メッセージの本文には、削除を依頼する情報を記述します。 次に例を示します。

- パッケージ "パッケージ名" のバージョン x.y.z を削除してください
- パッケージ "パッケージ名" のすべてのバージョンを削除してください
- 私の PowerShell ギャラリー アカウントを削除してください

PowerShell ギャラリーの管理者が 7 営業日以内に返信します。
指定したパッケージは、要求が送信された後、30 日以内に削除されます。
