---
ms.date: 09/11/2018
title: PowerShell ギャラリー アカウントを作成する
description: この記事では、PowerShell ギャラリーのユーザー アカウント要件について説明します
ms.openlocfilehash: 24c7531e61128415a284d1b438b43f3af0d1053a
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662609"
---
# <a name="creating-a-powershell-gallery-account"></a>PowerShell ギャラリー アカウントを作成する

PowerShell ギャラリーに何らかのアイテムを公開する前に、PowerShell ギャラリー アカウントを作成する必要があります。
PowerShell ギャラリー アカウントは、電子メール対応のログイン アカウントにリンクする必要があります。 このアカウントには、Azure Active Directory アカウントまたは outlook.com や hotmail.com の電子メール アカウントのような Microsoft ID を指定できます。

PowerShell ギャラリー アカウントを作成するには、 [https://PowerShellGallery.com](https://PowerShellGallery.com) にアクセスして、次の図のように、 **[サインイン]** をクリックします。

![新規アカウントの作成](media/creating-an-account/CreateAccount-Register.png)

Azure Active Directory アカウントを使用するには、 **[職場または学校アカウント]** を選択し、ご利用のアカウントでサインインします。 Microsoft ID を使用するには、 **[個人用アカウント]** を選択してサインインします。

次に、PowerShell ギャラリーで使用するユーザー名を作成するように求められます。 利用規約とプライバシー ポリシーを確認したら、ユーザー名を入力して **[登録]** をクリックします。

> [!NOTE]
> アカウント名は、一度作成すると変更できません。 詳細については、「[Managing Package Owners](managing-package-owners.md)」 (パッケージの所有者を管理する) を参照してください。

## <a name="recommended-practices-for-powershell-gallery-accounts"></a>PowerShell ギャラリー アカウントの推奨プラクティス

PowerShell ギャラリー アカウントで使用される電子メール アカウントを積極的に監視することが重要です。 この電子メール アドレスは、PowerShell ギャラリー パッケージの所有者とのすべての通信で使用されます。 PowerShell ギャラリーの運用チームがパッケージの所有者に連絡がとれない場合、パッケージを削除するよう求められる場合があります。

PowerShell ギャラリーに公開を行う組織が、その目的で一意の外部アカウントを作成することは頻繁にあります。 電子メールの転送を使用して、組織内のアドレスに通知を転送することをお勧めします。

1 つのパッケージに複数の所有者が関連付けられている場合は、PowerShell ギャラリーのすべての通知がすべての所有者に送信されます。 詳細については、「[Managing Package Owners](managing-package-owners.md)」 (パッケージの所有者を管理する) を参照してください。
