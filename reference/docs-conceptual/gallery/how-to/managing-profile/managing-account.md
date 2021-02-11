---
ms.date: 09/05/2018
title: PowerShell ギャラリーのアカウント設定
description: この記事では、PowerShell ギャラリーでのアカウント プロファイルの設定について説明します
ms.openlocfilehash: ab0e09aae6fea26ac6d85a35e30c8d06f31b121a
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662744"
---
# <a name="powershell-gallery-account-settings"></a>PowerShell ギャラリーのアカウント設定

PowerShell ギャラリー アカウントは、ID にリンクされている公開されている名前です。 その ID は、`user@hotmail.com` または `user@outlook.com` のような Microsoft ID か、Azure Active Directory (AAD) アカウントのいずれかになります。

PowerShell ギャラリーには、次のアカウント設定が用意されています。

- PowerShell ギャラリー アカウントに関連付けられている電子メール アカウント
- PowerShell ギャラリーから送信された電子メール通知のオプション
- PowerShell ギャラリー アカウントに関連付けられているユーザー アカウント
- PowerShell ギャラリー アカウントに関連付けられている画像

## <a name="email-address"></a>電子メール アドレス

電子メール アドレスは、PowerShell ギャラリーの通知の送信先です。 ログイン アカウントと一致している必要はありません。 アクセス権がある任意の電子メール アカウントを使用することができます。 PowerShell ギャラリーから直接、電子メール アドレスが他のユーザーに提供されることはありません。

![電子メール アドレスの変更](media/managing-account/PSGallery_AcccountEmailAddress.png)

新しい電子メール アドレスを入力すると、PowerShell ギャラリーによって確認メールがそのアドレスに送信されます。 確認メールには、PowerShell ギャラリーに戻って変更プロセスを完了するためのリンクが含まれています。 確認プロセスを完了するまで、すべての通知が変更前のアドレスに送信されます。

## <a name="email-notifications"></a>メール通知

PowerShell ギャラリーには、次の通知オプションが用意されています。

- Users can contact me through the PowerShell Gallery\(ユーザーが PowerShell ギャラリーを通じて連絡できるようにする\)
- Notify me when an item is pushed to the PowerShell Gallery using my account\(自分のアカウントを使用して PowerShell ギャラリーにパッケージがプッシュされたときに通知を受け取る\)

![電子メール アドレス オプションの選択](media/managing-account/PSGallery_AccountEmailOptions.png)

このページで説明したように、PowerShell ギャラリーからの重要な通知を無効にすることはできません。
チェックの内容は次のとおりです

- セキュリティ通知
- PowerShell ギャラリー管理者からのアカウント管理の通知
- 送信したものに対して PowerShell ギャラリーで実行されたテストに関する通知

## <a name="change-your-login-account"></a>ログイン アカウントの変更

ログイン アカウントを変更するには、現在のアカウントでサインインしている必要があります。 変更を完了するには、次の手順を実行します。

![ログイン アカウントの設定を変更する](media/managing-account/PSGallery_LoginAccountSettings.png)

1. **[アカウントの変更]** をクリックします。 ログイン アカウントの変更が PowerShell ギャラリーでのそのアカウントのすべての使用に適用されることを説明するポップアップ ウィンドウが表示されます。 情報を確認して、 **[OK]** をクリックして続行します。

   ![変更の確認 - OK/キャンセル](media/managing-account/PSGallery_LoginAccountChange-1.png)

2. _新しいアカウント_ を使用してサインインするように求められます。

   ![新しいアカウントでサインインする](media/managing-account/PSGallery_LoginAccountChange-2.png)

3. **[次へ]** をクリックすると、現在のアカウントを使用してサインインしていることを知らせるメッセージが表示されます。
   **[Sign out and sign in with a different account]\(サインアウトして別のアカウントでサインインする\)** をクリックします。

   ![サインアウトして別のアカウントでサインインする](media/managing-account/PSGallery_LoginAccountChange-3.png)

4. 新しいアカウントのパスワードを入力します。 パスワードを入力すると、ログイン アカウントが更新されたことを示す [アカウント設定] ページに戻ります。

## <a name="enable-two-factor-authentication-2fa"></a>2 要素認証 (2FA) の有効化

PowerShell ギャラリーに手動で公開するすべてのユーザーには、2 要素認証を使用することをお勧めします。 2 要素認証を有効にするには、ログイン アカウントに少なくとも 2 つの認証形式が登録されている必要があります。 1 つはパスワードで、他の形式には以下を指定できます。

- テキスト メッセージを受信できる電話番号
- 携帯電話の Microsoft Authenticator などの登録済みの認証子アプリケーション

これらの認証形式は、ご自分の AAD アカウント情報または Microsoft ID アカウントのセキュリティ設定で構成する必要があります。

2FA を有効にすると、PowerShell ギャラリーにサインインするたびに、構成済みの認証形式を使用した認証を求められます。

> [!IMPORTANT]
> PowerShell ギャラリー サイトで 2 要素認証を有効にした場合、ログイン アカウントのすべての使用に対して 2FA を有効にする必要はありません。 詳細については、「[2 段階認証について](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)」を参照してください。

## <a name="change-your-profile-picture"></a>プロファイル写真の変更

PowerShell ギャラリーでは、プロファイルに関連付けられている画像の保存および表示を Gravatar に依存しています。 プロファイル画像を更新するには、[Gravatar.com](http://www.gravatar.com/) にアクセスします。
