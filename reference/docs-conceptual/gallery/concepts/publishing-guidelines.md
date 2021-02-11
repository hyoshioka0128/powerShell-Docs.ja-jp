---
ms.date: 06/12/2017
description: この記事では、PowerShell ギャラリーに公開されたパッケージが広く使用され、ユーザーに高い価値を提供していることを確認するための、推奨の手順について説明します。
title: PowerShell ギャラリーへの公開に関するガイドラインとベスト プラクティス
ms.openlocfilehash: 97af3761fad1efb849b7197761a3855c9f1b05a4
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391172"
---
# <a name="powershellgallery-publishing-guidelines-and-best-practices"></a>PowerShell ギャラリーへの公開に関するガイドラインとベスト プラクティス

この記事では、PowerShell ギャラリーでのマニフェスト データの処理方法に基づいて、また多くの PowerShell ギャラリー ユーザーからのフィードバックに基づいて、PowerShell ギャラリーに公開したパッケージが確実にユーザーに広く受け入れられ、高い価値をもたらすことができるように、Microsoft チームが行っている推奨の手順をご説明します。 このガイドラインに従ってパッケージを公開すると、そのパッケージは信用されてインストールされやすくなるとともに、より多くのユーザーに興味を持ってもらうことができます。

良質な PowerShell ギャラリーのパッケージを作成する上で大事なこと、オプションのマニフェスト設定で最も重要なこと、初期のレビュー担当者や [PowerShell Script Analyzer](https://aka.ms/psscriptanalyzer) からのフィードバックに基づいたコードの改善、モジュールのバージョン管理、ドキュメント、共有済みパッケージのテストと使用方法のサンプルなどに関するガイドラインを以下に示します。 このドキュメントの大部分は、[高品質な DSC リソース モジュール](https://github.com/PowerShell/DscResources/blob/master/HighQualityModuleGuidelines.md)を公開するためのガイドラインを基にしています。

PowerShell ギャラリーへのパッケージの公開の仕組みについては、[パッケージの作成と公開](../how-to/publishing-packages/publishing-a-package.md)に関するページをご覧ください。

このガイドラインについてのフィードバックをお寄せください。 フィードバックをお送りいただく場合は、Microsoft の [GitHub ドキュメント リポジトリ](https://github.com/powershell/powershell-docs/issues)でイシューを作成してください。

## <a name="best-practices-for-publishing-packages"></a>パッケージの公開に関するベスト プラクティス

以下に、PowerShell ギャラリーのアイテムのユーザーが重要と考えるベスト プラクティスを、名目上の優先度順に並べて示します。 パッケージは、以下のガイドラインに準拠していると、ユーザーにダウンロードされ使用される可能性がかなり高くなります。

- PSScriptAnalyzer を使用する
- ドキュメントとサンプルを含める
- フィードバックに素早く対応する
- スクリプトではなくモジュールを提供する
- プロジェクト サイトへのリンクを提供する
- 互換性のある PSEdition とプラットフォームでパッケージにタグを付ける
- モジュールにテストを含める
- ライセンス条項を含めるか、ライセンス条項へのリンクを提供する (またはその両方)
- コードに署名する
- バージョン管理に関する [SemVer](https://semver.org/) ガイドラインに従う
- 一般的な PowerShell ギャラリー タグに関する記事で説明されている一般的なタグを使用する
- ローカル リポジトリを使用して公開に関するテストを行う
- PowerShellGet を使用して公開する

これらのベスト プラクティスのそれぞれについて、以下のセクションで簡単に説明します。

## <a name="use-psscriptanalyzer"></a>PSScriptAnalyzer を使用する

[PSScriptAnalyzer](https://www.powershellgallery.com/packages/PSScriptAnalyzer) は、PowerShell コードに対応した無料の静的コード分析ツールです。 **PSScriptAnalyzer** では PowerShell のコードで頻繁に見られる問題を特定し、多くの場合、推奨される問題の解決方法を示します。 このツールは簡単に使用でき、問題はエラー (重大であり対処が必須)、警告 (確認が必要であり対処した方がよい)、情報 (ベスト プラクティスの観点からチェックに値する) に分類されます。 PowerShell ギャラリーに公開されたすべてのパッケージは、**PSScriptAnalyzer** によりスキャンされます。エラーがある場合には所有者に報告されるので、対処を行う必要があります。

`-Recurse` と `-Severity` として警告を指定して、`Invoke-ScriptAnalyzer` を実行することをお勧めします。

結果を確認して以下を行います。

- すべてのエラーが修正され、ドキュメントに記載されます。
- すべての警告が確認され、必要に応じて対処されます。

PowerShell ギャラリーからパッケージをダウンロードするユーザーには、**PSScriptAnalyzer** を実行してすべてのエラーと警告を確認することが強く推奨されています。 **PSScriptAnalyzer** によってエラーが報告された場合、おそらくユーザーはパッケージの所有者に問い合わせます。 パッケージにエラーのフラグが付くコードをやむを得ずそのままにする場合は、その旨をドキュメントに追加して、同じ質問を何度も受けるのを回避します。

## <a name="include-documentation-and-examples"></a>ドキュメントとサンプルを含める

ユーザーに共有コードを確実に活用してもらう一番の方法は、ドキュメントとサンプルを用意することです。

ドキュメントは、PowerShell ギャラリーに公開するパッケージに含めておくと非常に役立ちます。
パッケージにドキュメントが含まれていない場合、パッケージの中身と使用方法を理解するには代わりにコードを読まなければならないため、ほとんどのユーザーに敬遠されてしまいます。 PowerShell のパッケージと合わせてドキュメントを提供する方法に関する次のような記事が用意されています。

- [コマンドレット ヘルプの書き方](https://go.microsoft.com/fwlink/?LinkID=123415)に関するページには、ヘルプの提供に関するガイドラインがあります。
- コマンドレット ヘルプの作成。コマンドレット ヘルプは、すべての PowerShell スクリプト、関数、コマンドレットを理解するために最適な手段です。
  コマンドレット ヘルプの作成方法については、「[How to Write Cmdlet Help](https://go.microsoft.com/fwlink/?LinkID=123415)」 (コマンドレット ヘルプの書き方) をご覧ください。
  スクリプト内にヘルプを追加する方法については、「[About Comment Based Help (コメント ベースのヘルプの概要)](/powershell/module/microsoft.powershell.core/about/about_comment_based_help)」をご覧ください。
- また、多くのモジュールには、テキスト形式のドキュメント (MarkDown ファイルなど) が含まれています。 MarkDown 形式が頻繁に使用されている GitHub にプロジェクト サイトがある場合、この方法は特に役立ちます。 [GitHub 用の Markdown](https://help.github.com/categories/writing-on-github/) を使用することをお勧めします。

サンプルでは、パッケージの用途をユーザーに説明します。 多くの開発者は、使用方法を確認する場合、ドキュメントの前にサンプルを見ると述べています。 基本的な使い方に加えて実際の使用例のシミュレーションを示し、コードに適切なコメントを付けると最適なサンプルになります。 PowerShell ギャラリーに公開するモジュールのサンプルは、モジュールのルートの下の Examples フォルダーに配置することをお勧めします。

サンプルの推奨パターンは、[PSDscResource モジュール](https://www.powershellgallery.com/packages/PSDscResources) の `Examples\RegistryResource` フォルダーにあります。 各ファイルの冒頭には、4 つのサンプルの使用例とともに、サンプルの内容を示す簡単な説明が記載されています。

## <a name="manage-dependencies"></a>依存関係の管理

お客様のモジュールが依存しているモジュールを、モジュール マニフェストで指定することが重要です。 これにより、エンド ユーザーは、お客様のモジュールが依存している適切なバージョンのモジュールのインストールについて心配する必要がなくなります。 依存モジュールを指定するには、モジュール マニフェスト内の必須モジュール フィールドを使う必要があります。 これによって、一覧にあるすべてのモジュールが、お客様のモジュールをインポートする前に (既に読み込まれている場合を除いて) グローバル環境に読み込まれます。 たとえば、別のモジュールによって一部のモジュールが既に読み込まれている場合があります。 また、**ModuleVersion** フィールドではなく **RequiredVersion** フィールドを使って、特定のバージョンを読み込むように指定することもできます。 **ModuleVersion** を使う場合は、最小バージョンの指定で、使用可能な最新バージョンが読み込まれます。 特定のバージョンを指定する **RequiredVersion** フィールドを使わない場合は、必須モジュールのバージョン更新を監視することが重要です。 モジュールのユーザー エクスペリエンスに影響する可能性のある破壊的変更には、特に注意する必要があります。

```powershell
Example: RequiredModules = @(@{ModuleName="myDependentModule"; ModuleVersion="2.0"; Guid="cfc45206-1e49-459d-a8ad-5b571ef94857"})

Example: RequiredModules = @(@{ModuleName="myDependentModule"; RequiredVersion="1.5"; Guid="cfc45206-1e49-459d-a8ad-5b571ef94857"})
```

## <a name="respond-to-feedback"></a>フィードバックに対応する

パッケージ所有者としてフィードバックに適切に対応すると、コミュニティでの評価が高くなります。 建設的なフィードバックをしてくれるユーザーは、パッケージに高い関心を持っており改善の手助けをしてくれるので、そのようなユーザーには対応することが重要です。

PowerShell ギャラリーでフィードバックを行う方法は 1 つあります。

- 所有者への連絡:この方法では、ユーザーはパッケージの所有者にメールを送信できます。 パッケージの所有者は、PowerShell ギャラリー パッケージで使用されているメール アドレスをチェックし、寄せられた問題に対応することが重要です。 この方法の 1 つのデメリットは、問い合わせをしたユーザーと所有者しか会話を見られないため、所有者は同じ質問に何度も答えなければならない可能性があることです。

フィードバックに建設的に対応すると、コミュニティで高い評価を得られます。 この報告の機会を利用して、詳細情報を求めましょう。 必要に応じて解決方法を提示するか、更新プログラムで問題を解決できるか確認します。

これらの連絡経路のいずれかで不適切な振る舞いが見られた場合には、PowerShell ギャラリーの機能を使用して、ギャラリーの管理者に連絡してください。

## <a name="modules-versus-scripts"></a>モジュールとスクリプト

他のユーザーとのスクリプトの共有は便利であり、生じる可能性のある問題の解決方法の例を提示できます。 問題は、PowerShell ギャラリーではスクリプトは単一のファイルとなり、個別のドキュメントやサンプル、テストを提供できないことです。

PowerShell モジュールはフォルダー構造となっており、複数のフォルダーやファイルをパッケージ内に含めることができます。 こうしたモジュールの構造であれば、ベスト プラクティスとして示したコマンドレット ヘルプ、ドキュメント、サンプル、テストなどのパッケージも含めることができます。 最大のデメリットは、モジュール内のスクリプトを公開し関数として使用する必要があることです。 モジュールの作成方法については、「[Writing a Windows PowerShell Module (Windows PowerShell モジュールの作成)](/powershell/scripting/developer/module/writing-a-windows-powershell-module)」をご覧ください。

特に DSC 構成などで、スクリプトを提供すると、ユーザー エクスペリエンスを改善できる場合があります。 DSC 構成のベスト プラクティスとして、ドキュメント、サンプル、テストが含まれる付属モジュールと合わせて、構成をスクリプトとして公開することをお勧めします。 このスクリプトでは、`RequiredModules = @(Name of the Module)` 形式で付随モジュールが一覧表示されます。 この方法はどのようなスクリプトでも使用できます。

スタンドアロン スクリプトの場合、その他のベスト プラクティスに従うことで別のユーザーに大きな価値を提供できます。 スクリプトを PowerShell ギャラリーに公開する場合、コメント ベースのドキュメントとプロジェクト サイトへのリンクを提供することが強く推奨されます。

## <a name="provide-a-link-to-a-project-site"></a>プロジェクト サイトへのリンクを提供する

プロジェクト サイトは、パブリッシャーとパブリッシャーの PowerShell ギャラリー パッケージのユーザーが直接やりとりできる場所です。 プロジェクト サイトではパッケージの情報をより簡単に得られるため、このサイトを備えたパッケージの方がユーザーに選ばれやすくなります。 PowerShell ギャラリーのパッケージの多くは GitHub で開発されていますが、専用の Web サイトを持つ組織が提供しているものもあります。 こうしたサイトのそれぞれがプロジェクト サイトとなります。

リンクを追加するには、次のようにマニフェストの PSData セクションに ProjectURI を記載します。

```
  # A URL to the main website for this project.
  ProjectUri = 'https://github.com/powershell/powershell'
```

ProjectURI が記載されている場合、PowerShell ギャラリーではパッケージ ページの左側にプロジェクト サイトへのリンクが表示されます。

## <a name="tag-your-package-with-the-compatible-pseditions-and-platforms"></a>互換性のある PSEdition とプラットフォームでパッケージにタグを付ける

次のタグを使用し、ユーザーにその環境でうまく機能するパッケージを示します。

- PSEdition_Desktop:Windows PowerShell との間で互換性のあるパッケージ
- PSEdition_Core:PowerShell Core との間で互換性のあるパッケージ
- Windows: Windows オペレーティング システムとの間で互換性のあるパッケージ
- Linux: Linux オペレーティング システムとの間で互換性のあるパッケージ
- MacOS:Mac オペレーティング システムとの間で互換性のあるパッケージ

互換性のあるプラットフォームでパッケージをタグ付けすると、それが検索結果の左側のウィンドウ上のギャラリー検索フィルターに含まれます。 GitHub 上でパッケージをホストする場合は、パッケージにタグ付けするときに、[PowerShell ギャラリー互換性シールド](https://img.shields.io/powershellgallery/p/:packageName.svg)
![互換性シールドの例](media/publishing-guidelines/CosmosDB.svg)を利用することもできます。

## <a name="include-tests"></a>テストを含める

オープンソースのコードにテストを含めると、検証内容を保証できるとともに、コードの仕組みに関する情報を提供できるため、ユーザーにとって非常に役立ちます。 また、ユーザーが自分の環境に合わせてコードを変更した場合でも、元の機能が損なわれないことが保証されます。

テストは、PowerShell 専用に設計された Pester テスト フレームワークを使用して作成することを強くお勧めします。 Pester は [GitHub](https://github.com/Pester/Pester) と [PowerShell ギャラリー](https://www.powershellgallery.com/packages/Pester/)で入手できます。また、Windows 10、Windows Server 2016、WMF 5.0、WMF 5.1 に同梱されています。

[GitHub の Pester のプロジェクト サイト](https://github.com/Pester/Pester)では、概要を始めベスト プラクティスに至るまで、Pester でのテストの作成方法が詳しく説明されています。

テスト範囲の目標値は[高品質のリソース モジュール](https://github.com/PowerShell/DscResources/blob/master/HighQualityModuleGuidelines.md)に関するドキュメントに記載されており、単体テストでのコード カバレッジの推奨値は 70% です。

## <a name="include-andor-link-to-license-terms"></a>ライセンス条項を含めるか、ライセンス条項へのリンクを提供する (またはその両方)

PowerShell ギャラリーに公開するすべてのパッケージには、ライセンス条項を指定するか、[使用条件](https://www.powershellgallery.com/policies/Terms)に関するページの「**Exhibit A**」 (別紙 A) に記載されているライセンスで拘束する必要があります。別のライセンスを指定する最適な方法は、**PSData** の **LicenseURI** を利用してライセンスへのリンクを提供することです。 詳細については、[パッケージ マニフェストとギャラリー UI](package-manifest-affecting-ui.md) に関するページを参照してください。

```powershell
PrivateData = @{
    PSData = @{

        # Tags applied to this module. These help with module discovery in online galleries.
        Tags = @('.net','acl','active-directory')

        # A URL to the license for this module.
        LicenseUri = 'http://www.apache.org/licenses/LICENSE-2.0'
```

## <a name="sign-your-code"></a>コードに署名する

コードに署名することで、パッケージの公開者について、またユーザーが取得したコードのコピーは公開者が公開したものに間違いないということを、ユーザーに対して最も確実に保証できます。 コードへの一般的な署名方法の詳細については、「[Introduction to Code Signing (コード署名の概要)](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))」をご覧ください。
PowerShell では、コード署名について 2 つの主な検証方法がサポートされています。

- スクリプト ファイルへの署名
- モジュールへのカタログ署名

PowerShell ファイルに署名すると、実行するコードが信頼できる作成元によって作成されており、変更されていないことを確実に保証できます。 PowerShell スクリプト ファイルへの署名方法の詳細については、[署名](/powershell/module/microsoft.powershell.core/about/about_signing)に関するページを参照してください。 つまり、署名はスクリプトのロード時に PowerShell により検証されるすべての `.PS1` ファイルに追加することができます。 [実行ポリシー](/powershell/module/microsoft.powershell.core/about/about_execution_policies) コマンドレットを使用して、署名済みのスクリプトのみが使用されるように PowerShell を制限できます。

モジュールへのカタログ署名は、バージョン 5.1 で PowerShell に追加された機能です。 モジュールへの署名方法については、[カタログ コマンドレット](/powershell/scripting/wmf/whats-new/new-updated-cmdlets#catalog-cmdlets)に関するページを参照してください。
カタログ署名は、モジュール内の全ファイルのハッシュ値を含むカタログ ファイルを作成してそのファイルに署名することで行います。

**PowerShellGet** の `Publish-Module`、`Install-Module`、および `Update-Module` の各コマンドレットにより、署名が有効であることが確認され、各パッケージのハッシュ値がカタログに記載されているものと一致するか確認されます。 `Save-Module` では、署名は検証されません。 システムに旧バージョンのモジュールがインストールされている場合、`Install-Module` がインストール済みのものと一致することが、新バージョンの署名機関によって確認されます。 パッケージがカタログによって署名されていない場合、`Install-Module` と `Update-Module` は、`.PSD1` ファイルの署名を使用します。 カタログ署名は、スクリプト ファイルへの署名と併用できますが、それの代わりにはなりません。 PowerShell では、モジュールのロード時にカタログ署名の検証は行いません。

## <a name="follow-semver-guidelines-for-versioning"></a>バージョン管理に関する SemVer ガイドラインに従う

[SemVer](https://semver.org/) は、変更内容を簡単に判別できるようにする、バージョンの構造化と変更の方法を定めた一般的な規則です。 マニフェスト データには、ご利用のパッケージのバージョンを含める必要があります。

- バージョンは `0.1.1` や `4.11.192` のように、3 つの数字ブロックをピリオドで区切って構造化する必要があります。
- `0` から始まるバージョンはパッケージがまだ運用環境に対応していないことを示します。数字が 1 つのみ使用される場合は、最初の数字は `0` で開始する必要があります。
- 先頭の数字の変更 (`1.9.9999` から `2.0.0`) は、バージョン間で重大な変更が行われたことを示します。
- 2 番目の数字の変更 (`1.1` から `1.2`) は、モジュールへの新しいコマンドレットの追加など、機能レベルでの変更を示します。
- 3 番目の数字の変更は、パラメーターの新規追加、サンプルの更新、テストの新規追加など、小規模な変更を示します。
- バージョンを列挙すると、PowerShell ではバージョンは文字列として並べ替えられるため、`1.01.0` は `1.001.0` より大きいものとして扱われます。

PowerShell は SemVer の公開前に作成されたため、SemVer の大半の要素をサポートしてはいますが、以下のようにサポートされていない要素もあります。

- バージョン番号のプレリリース文字列はサポートされていません。 プレリリース文字列は、発行元がバージョン `1.0.0` の提供後に新しいメジャー バージョンのプレビュー リリースを提供したい場合に役立ちます。 これは、PowerShell ギャラリーと **PowerShellGet** コマンドレットの今後のリリースでサポートされます。
- PowerShell と PowerShell ギャラリーでは、数字ブロックが 1 つ、2 つ、4 つのバージョン文字列を使用できます。 初期のモジュールの多くはこのガイドラインに従っておらず、また Microsoft からリリースされている製品には、ビルド情報は 4 番目の数字ブロックとして含められています (例: `5.1.14393.1066`)。 バージョン管理の観点では、こうした違いは無視できます。

## <a name="test-using-a-local-repository"></a>ローカル リポジトリを使用してテストする

PowerShell ギャラリーは、公開プロセスのテストの対象としては設計されていません。 PowerShell ギャラリーへのエンドツーエンドの公開プロセスをテストする最良の方法は、独自のローカル リポジトリを設定して使用することです。 これは、次のいくつかの方法で行うことができます。

- GitHub で [PS プライベート ギャラリー プロジェクト](https://github.com/PowerShell/PSPrivateGallery)を使用して、PowerShell ギャラリーのローカル インスタンスを設定します。 このプレビュー プロジェクトでは、制御可能でありテスト対象である PowerShell ギャラリーのインスタンスを容易に設定することができます。
- [Nuget 内部リポジトリ](https://devblogs.microsoft.com/powershell/setting-up-an-internal-powershellget-repository/)を設定します。
  これには追加の設定作業が必要です。ただし、利点として、API キーの使用を検証したり、公開時にターゲット内に依存関係が存在するかどうかを検証したりするなど、追加の要件をいくつか検証することができます。
- ファイル共有をテスト **リポジトリ** として設定します。 この設定は簡単ですが、ファイル共有であるため、上記の検証は行われません。 この場合の潜在的な利点の 1 つは、ファイル共有で (必須の) API キーが確認されないため、PowerShell ギャラリーに公開するのと同じキーを使用できるということです。

これらのいずれのソリューションでも、`Register-PSRepository` を使用して、新しい **リポジトリ** を定義します。これは、`Publish-Module` の `-Repository` パラメーターで使用します。

テスト公開に関する追加のポイント: PowerShell ギャラリーに公開したパッケージは、オペレーション チームの支援がなければ削除できません。オペレーション チームでは、公開するパッケージに何も依存していないことが確認されます。 そのため、弊社では PowerShell ギャラリーをテスト対象としてはサポートしておらず、それを行った発行元には問い合わせを行います。

## <a name="use-powershellget-to-publish"></a>PowerShellGet を使用して公開する

発行元は、PowerShell ギャラリーを使用するときは `Publish-Module` と `Publish-Script` コマンドレットを使用することを強くお勧めします。 **PowerShellGet** は、PowerShell ギャラリーからインストールをしたり、PowerShell ギャラリーに公開したりする場合の重要な詳細情報を覚えておかなくても済むように作成されました。 時折、発行元は、**PowerShellGet** をスキップして、`Publish-Module` の代わりに **NuGet** クライアントを使用したり、**PackageManagement** コマンドレットを使用したりすることがあります。 簡単に見落としてしまう詳細な設定がいくつもあり、その結果さまざまなサポート要求が発生します。

`Publish-Module` または `Publish-Script` を使用できない理由がある場合には、弊社にお知らせください。
**PowerShellGet** の GitHub リポジトリにイシューを登録し、**NuGet** または **PackageManagement** を選択することになった理由を詳細にご記入ください。

## <a name="recommended-workflow"></a>推奨されるワークフロー

PowerShell ギャラリーにパッケージを公開する上で最も効果的と考えられる方法を次に示します。

- 初期の開発はオープンソースのプロジェクト サイトで行います。 PowerShell チームでは GitHub を使用しています。
- レビュー担当者と [PowerShell Script Analyzer](https://aka.ms/psscriptanalyzer) からのフィードバックを活用してコードを安定化させます。
- 他のユーザーがアイテムの使用方法を知ることができるようにドキュメントを含めます。
- ローカル リポジトリを使用して公開動作をテストします。
- PowerShell ギャラリーには安定版またはアルファ リリースを公開し、必ずドキュメントとプロジェクト サイトへのリンクを記載します。
- プロジェクト サイトでフィードバックを集めてコードを反復開発し、安定した更新版を PowerShell ギャラリーに公開します。
- プロジェクトとモジュールにサンプルと Pester テストを追加します。
- パッケージのコード署名を行うかどうかを決定します。
- プロジェクトが運用環境で使用できるようになったと思われる場合は、`1.0.0` バージョンを PowerShell ギャラリーに公開します。
- 引き続きフィードバックを収集し、ユーザーの意見に基づいてコードを反復開発します。
