---
ms.date: 06/12/2017
title: アイテムの作成と公開
description: この記事では、スクリプトまたはモジュールを準備して PowerShell ギャラリーに公開するまでの仕組みと重要な手順について説明します
ms.openlocfilehash: be846799aff71d38bdd0c98b3f43eaee5aef7798
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662512"
---
# <a name="creating-and-publishing-an-item"></a>アイテムの作成と公開

PowerShell ギャラリーとは、広範な PowerShell ユーザー コミュニティを通じて、安定した PowerShell モジュール、スクリプト、DSC (Desired State Configuration) リソースを公開し、共有する場所です。

この記事では、スクリプトまたはモジュールを準備して PowerShell ギャラリーに公開するまでの仕組みと重要な手順について説明します。 [公開ガイドライン](../../concepts/publishing-guidelines.md)を確認して、公開したアイテムがより多くの PowerShell ギャラリー ユーザーに受け入れられるようにする方法を理解しておくことを強くお勧めします。

アイテムを PowerShell ギャラリーに公開するための最小要件は以下の通りです。

- PowerShell ギャラリー アカウントを持っていて、API キーが関連付けられている
- 必要なメタデータがアイテムに含まれていることを確認する
- 事前検証ツールを使用して、アイテムが公開できる状態であることを確認する
- Publish-Module コマンドおよび Publish-Script コマンドを使用して、アイテムを PowerShell ギャラリーに公開する
- アイテムに関する質問や懸念事項に答える

PowerShell ギャラリーでは、PowerShell モジュールと PowerShell スクリプトを受け付けています。 ここでスクリプトとは、大規模モジュールの一部ではなく、1 つのファイルである PowerShell スクリプトを意味しています。

## <a name="powershell-gallery-account-and-api-key"></a>PowerShell ギャラリー アカウントと API キー

PowerShell ギャラリー アカウントの設定方法については、「[Creating a PowerShell Gallery Account (PowerShell ギャラリー アカウントを作成する)](creating-an-account.md)」をご覧ください。

アカウントを作成したら、アイテムの公開に必要な API キーを取得できます。 アカウントでサインインすると、PowerShell ギャラリー ページの上部で [Register]\(登録\) の代わりにユーザー名が表示されます。 ユーザー名をクリックすると、[My Account]\(マイ アカウント\) ページに移動します。そこで API キーを確認できます。

> [!IMPORTANT]
> API キーは、ログイン情報およびパスワードと同等にセキュリティ保護する必要があります。 このキーを使用すると、PowerShell ギャラリーで所有しているあらゆるアイテムを、自分だけでなく他のユーザーも更新できます。 キーは定期的に更新することをお勧めします。[My Account]\(マイ アカウント\) ページの [Reset Key]\(キーをリセットする\) を使用して更新できます。

## <a name="required-metadata-for-items-published-to-the-powershell-gallery"></a>PowerShell ギャラリーで公開するアイテムに必要なメタデータ

PowerShell ギャラリーでは、スクリプトまたはモジュールのマニフェストに含まれているメタデータ フィールドから抽出した情報を、ギャラリーのユーザーに提供します。 PowerShell ギャラリーに公開するアイテムを作成または変更するには、アイテムのマニフェストに含める情報についていくつかの要件があります。 [公開ガイドライン](../../concepts/publishing-guidelines.md)のアイテム メタデータに関するセクションを確認して、アイテムを使ってユーザーに対して最適な情報を提供する方法を理解することを強くお勧めします。

[New-ModuleManifest](/powershell/module/microsoft.powershell.core/new-modulemanifest) および [New-ScriptFileInfo](/powershell/module/PowerShellGet/New-ScriptFileInfo) コマンドレットは、すべてのマニフェスト要素のプレースホルダーが用意されたマニフェスト テンプレートを作成します。

どちらのマニフェストにも、公開に重要な 2 つのセクションがあります。主キー データ、および PrivateData の PSData 領域です。 PowerShell モジュール マニフェストにおける主キー データとは、PrivateData セクション外のすべてです。 主キーのセットは、使用している PowerShell のバージョンに関連付けられており、未定義はサポートされません。 PrivateData は新しいキーの追加をサポートしているので、PowerShell ギャラリーに固有の要素は PSData にあります。

PowerShell ギャラリーに公開するアイテムについて提供する最も重要なマニフェスト要素は次のとおりです。

- スクリプト名またはモジュール名 - これらは、スクリプトの .PS1 の名前、またはモジュールの .PSD1 の名前から取得されます。
- バージョン - これは必須の主キーで、形式は SemVer ガイドラインに従う必要があります。 詳細についてはベスト プラクティスをご覧ください。
- 作成者 - これは必須の主キーで、アイテムに関連付けられる名前を含みます。 下記の作成者と所有者をご覧ください。
- 説明 - これは必須の主キーで、このアイテムの実行内容と使用にあたっての要件を簡単に説明するのに使用します。
- ProjectURI - これは強くお勧めする PSData 内の URI フィールドで、アイテムの開発を行う Github リポジトリや同様の場所へのリンクを提供します。
- タグ - PSEditions およびプラットフォームとの互換性に基づいて、パッケージにタグ付けすることを強くお勧めします。 詳細については、[公開のガイドライン](../../concepts/publishing-guidelines.md#tag-your-package-with-the-compatible-pseditions-and-platforms)を参照してください。

PowerShell ギャラリーでは、アイテムの作成者と所有者は関連する概念ですが、常に一致するわけではありません。 アイテムの所有者とは、PowerShell ギャラリー アカウントを持ち、アイテムを管理するアクセス許可を有するユーザーです。 アイテムを更新できる所有者が多数いることもあります。 所有者は PowerShell ギャラリーからのみ使用されるもので、アイテムがあるシステムから別のシステムにコピーされると失われます。 作成者は、マニフェスト データに組み込まれている文字列のため、常にアイテムに含まれます。 Microsoft 製品からのアイテムに関する推奨事項は次のとおりです。

- 複数の所有者を設定し、そのうち少なくとも 1 つは、そのアイテムを作成するチームの名前にする。
- 作成者は、よく知られているチーム名 (Azure SDK チームなど) または Microsoft Corporation とする。

## <a name="pre-validate-your-item"></a>アイテムを事前検証する

PowerShell ギャラリーにアイテムを公開する前に、コードに対して実行する必要があるツールがいくつかあります。

- PowerShell ギャラリー内にある [PowerShell Script Analyzer](https://www.powershellgallery.com/packages/PSScriptAnalyzer/)
- モジュールには、PowerShell の一部である Test-ModuleManifest
- スクリプトには、PowerShell Get に付属している Test-ScriptFileInfo

[PowerShell Script Analyzer](https://www.powershellgallery.com/packages/PSScriptAnalyzer/) は静的コード分析ツールで、コードをスキャンして、PowerShell のコーディングの基本的なガイドラインを満たしていることを確認します。 このツールはコードに含まれる一般的で重大な問題を特定するもので、アイテムを公開できるようにするために、開発中に定期的に実行する必要があります。 PowerShell Script Analyzer は、エラー、警告、および情報として特定した問題を一覧で提供します。 エラーはすべて、PowerShell ギャラリーに公開する前に対処する必要があります。 警告は確認する必要があり、大半を処理する必要があります。 PowerShell Script Analyzer は、PowerShell ギャラリーでアイテムが公開または更新されるたびに実行されます。 ギャラリー運用チームは、エラーが検出されると対処のためにアイテムの所有者に連絡します。

アイテムに含まれるマニフェスト情報が PowerShell ギャラリー インフラストラクチャで読み取れない場合、公開することはできません。
[Test-ModuleManifest](/powershell/module/microsoft.powershell.core/test-modulemanifest) は、インストールするとそのモジュールが使用できなくなるような、一般的な問題を検出します。 これはすべてのモジュールについて、PowerShell ギャラリーに公開する前に実行する必要があります。

同様に、[Test-ScriptFileInfo](/powershell/module/PowerShellGet/test-scriptfileinfo) はスクリプトに含まれるメタデータを検証するもので、すべてのスクリプト (モジュールとは別に公開された) で、PowerShell ギャラリーに公開する前に実行する必要があります。

## <a name="publishing-items"></a>アイテムを公開する

PowerShell ギャラリーにアイテムを公開するには、[Publish-Script](/powershell/module/PowerShellGet/publish-script) または [Publish-Module](/powershell/module/PowerShellGet/publish-module) を使用する必要があります。 これらのコマンドは、どちらも以下が必要です。

- 公開するアイテムへのパス。 モジュールの場合、モジュールの名前が付いたフォルダーを使用します。 同じモジュールの複数のバージョンを含むフォルダーを指定する場合は、RequiredVersion を指定する必要があります。
- Nuget API キー。 これは、PowerShell ギャラリーの [My Account]\(マイ アカウント\)ページにある API キーです。

コマンド ラインのその他のオプションは、そのほとんどが公開するアイテムのマニフェスト データに含まれるので、コマンドで指定する必要はありません。

エラーを回避するには、公開の前に、-Whatif -Verbose を使用してコマンドを実行することを強くお勧めします。 これによって、かなりの時間を節約することができます。PowerShell ギャラリーに公開するたびに、アイテムのマニフェスト セクションでバージョン番号を更新する必要があるからです。

次に例を示します。

* `Publish-Module -Path ".\MyModule" -NugetAPIKey "GUID" -WhatIf -Verbose`
* `Publish-Script -Path ".\MyScriptFile.PS1" -NugetAPIKey "GUID" -WhatIf -Verbose`

出力を注意深く確認し、エラーや警告がない場合は、-Whatif を付けずにコマンドを再実行します。

PowerShell ギャラリーに公開されるすべてのアイテムは、ウイルスのスキャンが行われ、PowerShell Script Analyzer を使用して分析されます。 その時点で発生した問題は、解決するよう公開者に送り返されます。

PowerShell ギャラリーにアイテムを公開したら、アイテムに関するフィードバックがないか注視する必要があります。

- 公開に使用したアカウントに関連付けられている電子メール アドレスをチェックするようにします。 ユーザーおよび PowerShell ギャラリー オペレーション チームは、そのアカウントを使用して、フィードバックや、PowerShell Script Analyzer またはウイルス対策スキャンからの問題を提供します。 [使用条件](https://www.powershellgallery.com/policies/Terms)に記載されているように、電子メール アカウントが無効である、または、アカウントに深刻な問題が報告されたのに長期間にわたり未解決のままである場合は、アイテムは放棄されたとみなされ、PowerShell ギャラリーから削除されます。
