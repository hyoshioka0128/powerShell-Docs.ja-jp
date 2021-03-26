---
ms.date: 03/22/2021
keywords: powershell,コマンドレット
title: PowerShell とは
description: この記事では、PowerShell のスクリプト環境とその機能の概要について説明します。
ms.openlocfilehash: 3e1c2cae4b8d6507057ee8136265710a8dfe74f3
ms.sourcegitcommit: 719debaed3cc32ba463b1d4cc56a491d8ecbce26
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2021
ms.locfileid: "105029691"
---
# <a name="what-is-powershell"></a>PowerShell とは

PowerShell は、コマンドライン シェル、スクリプト言語、および構成管理フレームワークで構成されるクロスプラットフォームのタスク自動化ソリューションです。 PowerShell は Windows、Linux、および macOS 上で実行されます。

## <a name="shell"></a>Shell

PowerShell は、その他の一般的なシェルの最良の機能を備えた最新のコマンド シェルです。 テキストを受け入れて返すだけの大抵のシェルとは異なり、PowerShell は .NET オブジェクトを受け入れて返します。 シェルには次の機能があります。

- 堅牢なコマンドラインの[履歴][]
- タブ補完とコマンド予測 (「[about_PSReadLine][]」を参照してください)
- コマンドとパラメーターの[別名][]のサポート
- コマンド チェーンの[パイプライン][]
- Unix `man` ページと同様のコンソール内[ヘルプ][] システム

## <a name="scripting-language"></a>[スクリプト言語]

スクリプト言語として、PowerShell はシステムの管理を自動化するためによく使用されます。 また、多くの場合、CI/CD 環境でソリューションをビルド、テスト、およびデプロイするためにも使用されます。 PowerShell は、.NET 共通言語ランタイム (CLR) 上に構築されています。 すべての入力と出力は .NET オブジェクトです。 出力から情報を抽出するためにテキスト出力を解析する必要がありません。 PowerShell スクリプト言語には、次の機能が含まれています。

- [関数][]、[クラス][]、[スクリプト][]、および[モジュール][]を通じて拡張可能
- 出力を容易にするための拡張可能な[書式設定システム][formatting]
- 動的な型を作成するための拡張可能な[型システム][types]
- [CSV][]、[JSON][]、[XML][] などの一般的なデータ形式用の組み込みサポート

## <a name="configuration-management"></a>構成管理

PowerShell の管理フレームワークである PowerShell Desired State Configuration ([DSC][]) を使用すると、コードとしての構成によりエンタープライズ インフラストラクチャを管理できます。 DSC を使用すると、次のことができます。

- 反復可能なデプロイのための宣言型の[構成][]とカスタム スクリプトを作成する
- 構成設定を適用し、構成の誤差に関するレポートを作成する
- [プッシュまたはプル][push-pull] モデルを使用して構成をデプロイする

## <a name="next-steps"></a>次のステップ

### <a name="getting-started"></a>作業の開始

PowerShell を初めて使用する場合、何から始めればよいでしょうか。 次のリソースをご覧ください。

- [PowerShell のインストール][install]
- [PowerShell 101][PS101]
- [PowerShell Bits チュートリアル][tutorials]
- [PowerShell に関する Learn モジュール][learn]

### <a name="powershell-in-action"></a>PowerShell の動作

さまざまなシナリオやプラットフォームで PowerShell がどのように使用されているかをご確認ください。

- [SSH 経由の PowerShell リモート処理][remoting]
- [Azure PowerShell を使ってみる][azure]
- [DSC を使用した CI/CD パイプラインの構築][devops]
- [Microsoft Exchange の管理][exchange]

<!-- link references -->

[履歴]: /powershell/module/microsoft.powershell.core/about/about_history
[about_PSReadLine]: /powershell/module/psreadline/about/about_psreadline
[aliases]: /powershell/module/microsoft.powershell.core/about/about_aliases
[パイプライン]: /powershell/module/microsoft.powershell.core/about/about_pipelines
[help]: /powershell/module/microsoft.powershell.core/get-help
[modules]: /powershell/module/microsoft.powershell.core/about/about_modules
[functions]: /powershell/module/microsoft.powershell.core/about/about_functions_advanced
[classes]: /powershell/module/microsoft.powershell.core/about/about_classes
[scripts]: /powershell/module/microsoft.powershell.core/about/about_scripts
[formatting]: /powershell/module/microsoft.powershell.core/about/about_format.ps1xml
[types]: /powershell/module/microsoft.powershell.core/about/about_types.ps1xml
[CSV]: /powershell/module/microsoft.powershell.utility/convertfrom-csv
[JSON]: /powershell/module/microsoft.powershell.utility/convertfrom-json
[XML]: /powershell/module/microsoft.powershell.utility/convertto-xml
[configurations]: /powershell/scripting/dsc/configurations/configurations
[DSC]: /powershell/scripting/dsc/overview/dscforengineers
[push-pull]: /powershell/scripting/dsc/pull-server/enactingconfigurations
[install]: /powershell/scripting/install/installing-powershell
[PS101]: /powershell/scripting/learn/ps101/00-introduction
[tutorials]: /powershell/scripting/learn/tutorials/00-introduction
[learn]: /learn/browse/?terms=PowerShell
[azure]: /powershell/azure/get-started-azureps
[devops]: /azure/devops/pipelines/release/dsc-cicd
[exchange]: /powershell/exchange/exchange-management-shell
[remoting]: /powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core
