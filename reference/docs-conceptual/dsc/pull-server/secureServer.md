---
ms.date: 06/12/2017
description: このドキュメントには、DSC プル サーバーを展開するエンジニアを支援するためのベスト プラクティスが記載されています。
keywords: DSC, PowerShell, 構成, セットアップ
title: プル サーバーのベスト プラクティス
ms.openlocfilehash: 6c754e6d035cc714a86da86ec916ba2c7f833268
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94389387"
---
# <a name="pull-server-best-practices"></a>プル サーバーのベスト プラクティス

適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

> [!IMPORTANT]
> プル サーバー (Windows Feature *DSC-Service*) は、Windows Server のサポート対象のコンポーネントですが、新機能がオファーされる予定はありません。 管理対象のクライアントは、(Windows Server のプル サーバー以降の機能が含まれる) [Azure Automation DSC](/azure/automation/automation-dsc-getting-started) または、[こちら](pullserver.md#community-solutions-for-pull-service)に列挙されているコミュニティ ソリューションのいずれかに切り替えを開始することをお勧めします。

概要: このドキュメントは、ソリューションを準備するエンジニアを支援するプロセスと拡張機能を示すためのものです。 ベスト プラクティスに関する推奨事項は、将来を見据えた安定したものにするために、お客様によって確認され、製品チームによって検証されたベスト プラクティスの詳細を示す必要があります。

- 作成者:Michael Greene
- 校閲者: Ben Gelens、Ravikanth Chaganti、Aleksandar Nikolic
- 公開日:2015 年 4 月

## <a name="abstract"></a>概要

このドキュメントは、Windows PowerShell Desired State Configuration プル サーバーの実装を計画しているすべてのユーザー向けの公式なガイダンスを提供するためのものです。 プル サーバーは単純なサービスで、数分で展開できる必要があります。 このドキュメントは展開で使用できる技術的な操作方法に関するガイダンスを提供するものですが、ベスト プラクティスと、展開の前に考慮すべきことを参照する場合にも役立ちます。 読者は DSC と、DSC 展開に含まれるコンポーネントの説明で使用される用語の基礎をよく理解している必要があります。 詳細については、「[Windows PowerShell Desired State Configuration の概要](/powershell/scripting/dsc/overview/overview)」を参照してください。 DSC はクラウドと共に進化することを想定しているため、プル サーバーを含む基礎技術にも進化と、新機能の導入が求められます。 このドキュメントには、以前のリリースへの参照と、先進的な設計を促進する将来を見据えたソリューションへの参照を提供する付録にバージョン テーブルが含まれています。

このドキュメントの主な 2 つのセクションは次のとおりです。

- 構成計画
- インストール ガイド

### <a name="versions-of-the-windows-management-framework"></a>Windows Management Framework のバージョン

このドキュメントの情報は、Windows Management Framework 5.0 に適用するためのものです。 プル サーバーの展開と操作で WMF 5.0 は必要ありませんが、このドキュメントではバージョン 5.0 に焦点を合わせています。

### <a name="windows-powershell-desired-state-configuration"></a>Windows PowerShell の必要な状態の構成

Desired State Configuration (DSC) は、Common Information Model (CIM) を記述するためにマネージド オブジェクト フォーマット (MOF) という業界構文を使用して、構成データの展開と管理を可能にする管理プラットフォームです。 オープン ソース プロジェクトの Open Management Infrastructure (OMI) は、Linux とネットワーク ハードウェア オペレーティング システムを含むプラットフォーム全体でこれらの標準をさらに開発するために存在します。 詳細については、[MOF 仕様にリンクされている DMTF ページ](https://www.dmtf.org/standards/cim)、および [OMI のドキュメントとソース](https://collaboration.opengroup.org/omi/documents.php)を参照してください。

Windows PowerShell には、宣言構成を作成および管理するために使用できる Desired State Configuration の一連の言語拡張が用意されています。

### <a name="pull-server-role"></a>プル サーバーの役割

プル サーバーでは、ターゲット ノードにアクセス可能な構成を格納する中央サービスが提供されます。

プル サーバーの役割は、Web サーバー インスタンスまたは SMB ファイル共有のいずれかとして展開することができます。 Web サーバー機能には OData インターフェイスが含まれており、構成の適用時に成功または失敗の確認を報告するターゲット ノードの機能もオプションで含めることができます。 この機能は、多数のターゲット ノードが存在する環境で役立ちます。 プル サーバーをポイントするようにターゲット ノード (クライアントとも呼ばれる) を構成した後で、最新の構成データと必要なすべてのスクリプトがダウンロードされ、適用されます。 これは、1 回限りの展開として、またはプル サーバーを大規模な変更を管理するための重要な資産とする再発ジョブとして行われる場合があります。 詳細については、「[Windows PowerShell Desired State Configuration プル サーバー](pullserver.md)」および「[DSC Web プル サーバーのセットアップ](pullserver.md)」を参照してください。

## <a name="configuration-planning"></a>構成計画

エンタープライズ ソフトウェアの展開の場合、事前に収集できる情報があります。この情報は、適切なアーキテクチャの計画と、展開を完了するために必要な手順を準備に役立ちます。 次のセクションでは、準備方法と、事前に必要となる可能性がある組織的な接続に関する情報を提供します。

### <a name="software-requirements"></a>ソフトウェア要件

プル サーバーの展開には、Windows Server の DSC サービス機能が必要です。 この機能は、Windows Server 2012 で導入されたもので、Windows 管理フレームワーク (WMF) の継続的なリリースを通じて更新されています。

### <a name="software-downloads"></a>ソフトウェアのダウンロード

Windows Update からの最新コンテンツのインストールに加え、DSC プル サーバーの展開のベスト プラクティスと見なされる、最新バージョンの Windows Management Framework と、プル サーバーのプロビジョニングを自動化するための DSC モジュールの 2 つをダウンロードする必要があります。

### <a name="wmf"></a>WMF

Windows Server 2012 R2 には、DSC サービスという機能が含まれています。 DSC サービス機能では、OData エンドポイントをサポートするバイナリを含む、プル サーバー機能が提供されます。 WMF は Windows Server に含まれており、Windows Server のリリースに応じて迅速に更新されます。
[新しいバージョンの WMF 5.0](https://www.microsoft.com/download/details.aspx?id=54616) には、DSC サービス機能の更新プログラムが含まれる場合があります。 そのため、WMF の最新リリースをダウンロードし、リリース ノートを確認し、リリースに DSC サービス機能の更新プログラムが含まれるかどうか判断することをお勧めします。 さらに、更新プログラムまたはシナリオの設計が安定状態であるか実験状態であるかを示すリリース ノートのセクションを確認する必要もあります。 アジャイル リリース サイクルを可能にするために、個々の機能を安定したものとして宣言することができます。これにより、WMF がプレビューでリリースされている間でも、機能は運用環境で使用する準備ができていることが示されます。 WMF リリースで既に更新されている他の機能は、次のとおりです (詳細については、WMF リリース ノートを参照)。

- Windows PowerShell、Windows PowerShell Integrated Scripting Environment (ISE)
- Windows PowerShell Web サービス (Management OData IIS 拡張機能)
- Windows PowerShell Desired State Configuration (DSC)
- Windows Remote Management (WinRM)、Windows Management Instrumentation (WMI)

### <a name="dsc-resource"></a>DSC リソース

プル サーバーの展開は、DSC 構成スクリプトを使用してサービスをプロビジョニングすることで簡略化できます。 このドキュメントには、運用準備ができているサーバー ノードの展開で使用できる構成スクリプトが含まれています。 構成スクリプトを使用するには、Windows Server に含まれていない DSC モジュールが必要になります。 必要なモジュール名は **xPSDesiredStateConfiguration** で、DSC リソースの **xDscWebService** が含まれます。 xPSDesiredStateConfiguration モジュールは[こちら](https://github.com/dsccommunity/xPSDesiredStateConfiguration)からダウンロードできます。

**PowerShellGet** モジュールから `Install-Module` コマンドレットを使用します。

```powershell
Install-Module xPSDesiredStateConfiguration
```

**PowerShellGet** モジュールの場合、次の場所にモジュールがダウンロードされます。

`C:\Program Files\Windows PowerShell\Modules`

計画タスク

- Windows Server 2012 R2 のインストール ファイルにアクセスできますか?
- 展開環境からインターネットにアクセスし、オンライン ギャラリーから WMF とモジュールをダウンロードできますか?
- オペレーティング システムをインストールした後、最新のセキュリティ更新プログラムはどのようにインストールできますか?
- 使用環境からインターネットにアクセスし、更新プログラムを取得できますか? あるいは、環境内にローカル Windows Server Update Services (WSUS) サーバーはありますか?
- オフライン導入により更新プログラムが既に含まれている Windows Server インストール ファイルにアクセスできますか?

### <a name="hardware-requirements"></a>ハードウェア要件

物理サーバーと仮想サーバーの両方でプル サーバーの展開がサポートされます。 プル サーバーのサイズ要件は、Windows Server 2012 R2 の要件と一致します。

- CPU: 1.4 GHz 64 ビット プロセッサ
- メモリ: 512 MB
- ディスク領域: 32 GB
- ネットワーク: Gigabit Ethernet Adapter

計画タスク

- 物理ハードウェアまたは仮想化プラットフォームに展開する予定ですか?
- ターゲット環境のために新しいサーバーを要求するプロセスは何ですか?
- サーバーが使用可能になるまでの平均所要時間はどれくらいですか?
- どのようなサイズのサーバーを要求しますか?

### <a name="accounts"></a>アカウント

プル サーバー インスタンスを展開する場合、サービス アカウントの要件はありません。 ただし、ローカル ユーザー アカウントのコンテキストで Web サイトを実行できるシナリオがあります。 たとえば、Web サイト コンテンツの記憶域共有にアクセスする必要があり、記憶域共有をホストしているデバイスまたは Windows Server がドメインに参加していない場合などです。

### <a name="dns-records"></a>DNS レコード

プル サーバー環境で動作するようにクライアントを構成する際に使用するサーバー名が必要になります。
テスト環境では、通常、サーバーのホスト名が使用されます。DNS 名前解決が使用できない場合には、サーバーの IP アドレスを使用できます。 運用環境、または運用環境の展開を表すためのラボ環境では、DNS CNAME レコードを作成することをお勧めします。

DNS CNAME では、ホスト (A) レコードを参照する別名を作成できます。 追加の名前レコードの目的は、後で変更が必要になった場合に備えて柔軟性を高めることです。 CNAME はクライアント構成の分離に役立ちます。したがって、プル サーバーの置換や追加などの、サーバー環境の変更に合わせて、クライアント構成を変更する必要はありません。

DNS レコードの名前を選択する場合は、ソリューションのアーキテクチャに注意してください。 負荷分散を使用する場合、HTTPS 経由でトラフィックを保護するために使用される証明書で DNS レコードと同じ名前を共有する必要があります。

|       シナリオ        |                                                                                         推奨事項
|:--------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
|テスト環境       | 可能であれば、計画された運用環境を再現します。 サーバーのホスト名は簡単な構成に適しています。 DNS が利用できない場合は、ホスト名の代わりに IP アドレスを使用することができます。
|単一ノードの展開 | サーバーのホスト名をポイントする DNS CNAME レコードを作成します。

詳細については、[Windows Server での DNS ラウンド ロビンの構成](/previous-versions/windows/it-pro/windows-server-2003/cc787484(v=ws.10))に関するページを参照してください。

計画タスク

- DNS レコードの作成や変更に関する問い合わせはどこにすればよいですか?
- DNS レコードの要求の平均所要時間はどれくらいですか?
- サーバーの静的なホスト名 (A) レコードを要求する必要がありますか?
- CNAME レコードとして何を要求しますか?
- 必要な場合に、どのような種類の負荷分散ソリューションを利用しますか? (詳細については、「負荷分散」というタイトルのセクションを参照してください)。

### <a name="public-key-infrastructure"></a>公開キー基盤

今日では、ほとんどの組織で、転送時にネットワーク トラフィック (特に、サーバーの構成方法などの機密データを含むトラフィック) を検証および/または暗号化する必要があります。
クリア テキストでのクライアント要求を容易にする HTTP を使用してプル サーバーを展開できますが、HTTPS を使用してトラフィックをセキュリティで保護することをお勧めします。 DSC リソースの **xPSDesiredStateConfiguration** で一連のパラメーターを指定して、HTTPS を使用するようにサービスを構成することができます。

プル サーバーの HTTPS トラフィックをセキュリティで保護する証明書の要件は、その他の HTTPS Web サイトのセキュリティ保護と違いはありません。 Windows Server 証明書サービスの **Web サーバー** テンプレートには必要な機能が揃っています。

計画タスク

- 証明書要求が自動化されていない場合、証明書を要求する際に誰に連絡する必要がありますか?
- 要求の平均所要時間はどれくらいですか?
- 証明書ファイルはどのように転送されますか?
- 証明書の秘密キーはどのように転送されますか?
- 既定の有効期限はどれくらいですか?
- 証明書名として使用できる、プル サーバー環境用の DNS 名はどのように決めましたか?

### <a name="choosing-an-architecture"></a>アーキテクチャの選択

プル サーバーは、IIS でホストされている Web サービス、または SMB ファイル共有を使用して展開できます。 ほとんどの場合、提供される Web サービス オプションを使用することで、柔軟性が増します。 HTTPS トラフィックではネットワーク境界をスキャンすることは珍しいことではありませんが、ネットワーク間の SMB トラフィックは多くの場合、フィルター処理されるか、ブロックされます。 Web サービスでは、一元的に確認できるようにクライアントがサーバーに状態をレポートするメカニズムを提供する Conformance Server または Web Reporting Manager (このドキュメントの今後のバージョンに両方のトピックが含まれる予定) を含めるオプションも提供されます。 SMB では、ポリシーで Web サーバーを利用しないように指示されている環境や、Web サーバー役割は好ましくないとする他の環境要件に対するオプションを提供します。 いずれの場合も、必ず、トラフィックの署名と暗号化に関する要件を評価してください。 HTTPS、SMB 署名、および IPSEC ポリシーはすべて、考慮に値するオプションです。

#### <a name="load-balancing"></a>負荷分散

Web サービスと対話するクライアントは、単一の応答で返される情報を要求します。 順次要求は必要ありません。したがって、負荷分散プラットフォームで任意の時点で単一サーバーでのセッションが維持されるようにする必要はありません。

計画タスク

- 複数サーバーでの負荷分散トラフィックではどのようなソリューションが使用されますか?
- ロード バランサー機器を使用する場合、デバイスへの新しい構成の追加要求は誰が受けますか?
- 新しい負荷分散 Web サービスの構成要求に要する平均時間はどれくらいですか?
- 要求にはどのような情報が必要ですか?
- 追加 IP を要求する必要はありますか? あるいは、負荷分散の担当チームが対応するのですか?
- 必要な DNS レコードはありますか? また、負荷分散ソリューションの構成担当チームでその DNS レコードが必要になりますか?
- 負荷分散ソリューションでは、PKI をデバイスで処理する必要がありますか? あるいは、セッション要件がない限り、HTTPS トラフィックの負荷分散は可能ですか?

### <a name="staging-configurations-and-modules-on-the-pull-server"></a>プル サーバーのステージング構成およびモジュール

構成計画の一環として、プル サーバーでホストされる DSC モジュールと構成について考慮する必要があります。 構成計画のために、コンテンツを準備し、プル サーバーに展開する方法について、基本的に理解することが重要です。

今後、このセクションは追加され、DSC プル サーバー用の運用ガイドに含まれる予定です。 このガイドでは、時間の経過と共に自動的にモジュールと構成を管理する日常的なプロセスについて説明します。

#### <a name="dsc-modules"></a>DSC モジュール

構成を要求するクライアントには、必須の DSC モジュールが必要になります。 プル サーバーの機能は、クライアントへの DSC モジュールのオンデマンド配布を自動化するものです。 初めてプル サーバーを展開する場合 (おそらく、ラボまたは概念実証として)、PowerShell ギャラリーや DSC モジュールの PowerShell.org GitHub リポジトリなどのパブリック リポジトリから使用可能な DSC モジュールに依存する可能性があります。

PowerShell ギャラリーなどの信頼できるオンライン リソースの場合でも、パブリック リポジトリからダウンロードされるすべてのモジュールを、PowerShell の使用経験があり、運用環境で使用する前にモジュールを使用する環境に関する知識がある担当者が確認する必要があります。 このタスクの実行時に、ドキュメントやスクリプト例などの削除可能な追加ペイロードをモジュールで確認することをお勧めします。 これにより、サーバーからクライアントへのネットワークを介したモジュールのダウンロード時に、最初の要求でのクライアントごとのネットワーク帯域幅を減らせます。

各モジュールは、モジュール ペイロードを含む、ModuleName_Version.zip という名前の特定の形式の ZIP ファイルにパッケージ化する必要があります。 ファイルをサーバーにコピーしたら、チェックサム ファイルを作成する必要があります。
クライアントがサーバーに接続されると、DSC モジュールのコンテンツが公開後に変更されていないことを確認するためにチェックサムが使用されます。

```powershell
New-DscChecksum -ConfigurationPath .\ -OutPath .\
```

計画タスク

- テストまたはラボ環境を計画する場合に、検証を行うために重要なシナリオはどれですか?
- 必要なものすべてをカバーするリソースを含む一般公開されているモジュールはありますか?あるいは、独自のリソースを作成する必要はありますか?
- 使用環境からインターネットにアクセスして、公開モジュールを取得できますか?
- DSC モジュールの確認担当者は誰ですか?
- 運用環境を計画する場合に、DSC モジュールを格納するためのローカル リポジトリとして何を使用しますか?
- 中央の管理チームはアプリケーション チームからの DSC モジュールを受け入れますか? プロセスはどのようになりますか?
- ソース リポジトリから、サーバーに対する運用準備済み DSC モジュールのチェックサムのパッケージ化、コピー、および作成を自動化しますか?
- チームで自動プラットフォームの管理も担当しますか?

#### <a name="dsc-configurations"></a>DSC の構成

プル サーバーの目的は、クライアント ノードに DSC 構成を配布するための一元的なメカニズムを提供することです。 構成は、MOF ドキュメントとしてサーバーに格納されます。 各ドキュメントは、一意の **GUID** で名前が付けられます。 クライアントは、プル サーバーに接続するように構成されている場合、要求する必要がある構成用の **GUID** も付与されます。 **GUID** で構成を参照するこのシステムは、グローバルな一意性を保証します。また、ノードごとに細かく構成を適用できたり、構成が同じである多くのサーバーにまたがる役割で構成を適用できるため、柔軟性があります。

#### <a name="guids"></a>GUID

構成 **GUID** の計画についても、プル サーバー展開を検討する際に考慮する必要があります。 **GUID** の処理方法に関する特定の要件はありませんが、プロセスは環境ごとに一意である可能性があります。 プロセスは、単純なものから複雑なものまでさまざまです。たとえば、セントラルに格納されている CSV ファイル、単純な SQL テーブル、CMDB、別のツールまたはソフトウェア ソリューションとの統合が必要な複雑なソリューションなどです。 2 つの一般的な方法を以下に示します。

- **サーバーごとに GUID を割り当てる** — 確実にすべてのサーバー構成が個別に制御されるようにします。 これにより、正確に更新が行われ、少数のサーバーを含む環境で適切に機能します。
- **サーバーの役割ごとに GUID を割り当てる** — Web サーバーなど、同じ機能を実行するすべてのサーバーに同じ GUID を使用し、必要な構成データが参照されるようにします。 多くのサーバーで同じ GUID を共有している場合、それらすべてのサーバーは、構成の変更と同時に更新されることに注意してください。

  GUID は、使用環境でのサーバーの展開と構成方法に関する情報を得るために悪意のあるユーザーによって利用される可能性があるため、機密データとして扱う必要があります。 詳細については、「[Securely allocating GUIDs in PowerShell Desired State Configuration Pull Mode](https://devblogs.microsoft.com/powershell/securely-allocating-guids-in-powershell-desired-state-configuration-pull-mode/)」 (PowerShell Desired State Configuration プル モードでの安全な GUID の割り当て) を参照してください。

計画タスク

- 構成の準備ができた場合に、プル サーバー フォルダーに構成をコピーする担当者は誰ですか?
- アプリケーション チームによって構成が作成されている場合、引き継ぎプロセスはどのようになっていますか?
- 構成を作成時に格納するリポジトリはチーム全体で利用しますか?
- 構成をサーバーにコピーし、準備ができたときにチェックサムを作成するプロセスを自動化しますか?
- GUID をサーバーまたは役割にどのようにマップしますか? また、どこに格納しますか?
- クライアント コンピューターの構成プロセスとして何を使用しますか? また、構成 GUID の作成および格納プロセスとどのように統合しますか?

## <a name="installation-guide"></a>インストール ガイド

*このドキュメントには、安定したスクリプトが示されています。スクリプトは、運用環境で実行する前に、必ず慎重に確認してください。*

### <a name="prerequisites"></a>前提条件

サーバー上の PowerShell のバージョンを確認するには、次のコマンドを使用します。

```powershell
$PSVersionTable.PSVersion
```

可能であれば、Windows Management Framework を最新バージョンにアップグレードしてください。 次に、以下のコマンドを使用して、`xPsDesiredStateConfiguration` モジュールをダウンロードします。

```powershell
Install-Module xPSDesiredStateConfiguration
```

モジュールをダウンロードする前に、コマンドによって承認が求められます。

### <a name="installation-and-configuration-scripts"></a>インストールおよび構成スクリプト

DSC プル サーバーを展開する最適な方法は、DSC 構成スクリプトを使用することです。 このドキュメントでは、DSC Web サービスのみを構成する基本設定と、DSC Web サービスを含む Windows Server エンド ツー エンドを構成する詳細設定の両方を含むスクリプトを示します。

注:現時点では、`xPSDesiredStateConfiguration` DSC モジュールでは、サーバーのロケールとして EN-US を設定する必要があります。

### <a name="basic-configuration-for-windows-server-2012"></a>Windows Server 2012 の基本構成

```powershell
# This is a very basic Configuration to deploy a pull server instance in a lab environment on Windows Server 2012.

Configuration PullServer {
Import-DscResource -ModuleName xPSDesiredStateConfiguration

        # Load the Windows Server DSC Service feature
        WindowsFeature DSCServiceFeature
        {
          Ensure = 'Present'
          Name = 'DSC-Service'
        }

        # Use the DSC Resource to simplify deployment of the web service
        xDSCWebService PSDSCPullServer
        {
          Ensure = 'Present'
          EndpointName = 'PSDSCPullServer'
          Port = 8080
          PhysicalPath = "$env:SYSTEMDRIVE\inetpub\wwwroot\PSDSCPullServer"
          CertificateThumbPrint = 'AllowUnencryptedTraffic'
          ModulePath = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules"
          ConfigurationPath = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration"
          State = 'Started'
          DependsOn = '[WindowsFeature]DSCServiceFeature'
        }
}
PullServer -OutputPath 'C:\PullServerConfig\'
Start-DscConfiguration -Wait -Force -Verbose -Path 'C:\PullServerConfig\'
```

### <a name="advanced-configuration-for-windows-server-2012-r2"></a>Windows Server 2012 R2 の詳細構成

```powershell
# This is an advanced Configuration example for Pull Server production deployments
# on Windows Server 2012 R2. Many of the features demonstrated are optional and
# provided to demonstrate how to adapt the Configuration for multiple scenarios
# Select the needed resources based on the requirements for each environment.
# Optional scenarios include:
#      * Reduce footprint to Server Core
#      * Rename server and join domain
#      * Switch from SSL to TLS for HTTPS
#      * Automatically load certificate from Certificate Authority
#      * Locate Modules and Configuration data on remote SMB share
#      * Manage state of default websites in IIS

param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [System.String] $ServerName,
        [System.String] $DomainName,
        [System.String] $CARootName,
        [System.String] $CAServerFQDN,
        [System.String] $CertSubject,
        [System.String] $SMBShare,
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PsCredential] $Credential
    )

Configuration PullServer {
    Import-DscResource -ModuleName xPSDesiredStateConfiguration, xWebAdministration, xCertificate, xComputerManagement
    Node localhost
    {

        # Configure the server to automatically corret configuration drift including reboots if needed.
        LocalConfigurationManager
        {
            ConfigurationMode = 'ApplyAndAutoCorrect'
            RebootNodeifNeeded = $node.RebootNodeifNeeded
            CertificateId = $node.Thumbprint
        }

        # Remove all GUI interfaces so the server has minimum running footprint.
        WindowsFeature ServerCore
        {
            Ensure = 'Absent'
            Name = 'User-Interfaces-Infra'
        }

        # Set the server name and if needed, join a domain. If not joining a domain, remove the DomainName parameter.
        xComputer DomainJoin
        {
            Name = $Node.ServerName
            DomainName = $Node.DomainName
            Credential = $Node.Credential
        }

        # The next series of settings disable SSL and enable TLS, for environments where that is required by policy.
        Registry TLS1_2ServerEnabled
        {
            Ensure = 'Present'
            Key = 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server'
            ValueName = 'Enabled'
            ValueData = 1
            ValueType = 'Dword'
        }

        Registry TLS1_2ServerDisabledByDefault
        {
            Ensure = 'Present'
            Key = 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server'
            ValueName = 'DisabledByDefault'
            ValueData = 0
            ValueType = 'Dword'
        }

        Registry TLS1_2ClientEnabled
        {
            Ensure = 'Present'
            Key = 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client'
            ValueName = 'Enabled'
            ValueData = 1
            ValueType = 'Dword'
        }

        Registry TLS1_2ClientDisabledByDefault
        {
            Ensure = 'Present'
            Key = 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client'
            ValueName = 'DisabledByDefault'
            ValueData = 0
            ValueType = 'Dword'
        }

        Registry SSL2ServerDisabled
        {
            Ensure = 'Present'
            Key = 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server'
            ValueName = 'Enabled'
            ValueData = 0
            ValueType = 'Dword'
        }

        # Install the Windows Server DSC Service feature
        WindowsFeature DSCServiceFeature
        {
            Ensure = 'Present'
            Name = 'DSC-Service'
        }

        # If using a certificate from a local Active Directory Enterprise Root Certificate Authority,
        # complete a request and install the certificate
        xCertReq SSLCert
        {
            CARootName = $Node.CARootName
            CAServerFQDN = $Node.CAServerFQDN
            Subject = $Node.CertSubject
            AutoRenew = $Node.AutoRenew
            Credential = $Node.Credential
        }

        # Use the DSC resource to simplify deployment of the web service.  You might also consider
        # modifying the default port, possibly leveraging port 443 in environments where that is
        # enforced as a standard.
        xDSCWebService PSDSCPullServer
        {
            Ensure = 'Present'
            EndpointName = 'PSDSCPullServer'
            Port = 8080
            PhysicalPath = "$env:SYSTEMDRIVE\inetpub\wwwroot\PSDSCPullServer"
            CertificateThumbPrint = 'CertificateSubject'
            CertificateSubject = $Node.CertSubject
            ModulePath = "$($Node.SMBShare)\DscService\Modules"
            ConfigurationPath = "$($Node.SMBShare)\DscService\Configuration"
            State = 'Started'
            DependsOn = '[WindowsFeature]DSCServiceFeature'
        }

        # Validate web config file contains current DB settings
        xWebConfigKeyValue CorrectDBProvider
        {
            ConfigSection = 'AppSettings'
            Key = 'dbprovider'
            Value = 'System.Data.OleDb'
            WebsitePath = 'IIS:\sites\PSDSCPullServer'
            DependsOn = '[xDSCWebService]PSDSCPullServer'
        }
        xWebConfigKeyValue CorrectDBConnectionStr
        {
            ConfigSection = 'AppSettings'
            Key = 'dbconnectionstr'
            Value = 'Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\Program Files\WindowsPowerShell\DscService\Devices.mdb;'
            WebsitePath = 'IIS:\sites\PSDSCPullServer'
            DependsOn = '[xDSCWebService]PSDSCPullServer'
        }

        # Stop the default website
        xWebsite StopDefaultSite
        {
            Ensure = 'Present'
            Name = 'Default Web Site'
            State = 'Stopped'
            PhysicalPath = 'C:\inetpub\wwwroot'
            DependsOn = '[WindowsFeature]DSCServiceFeature'
        }
    }
}

$configData = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            ServerName = $ServerName
            DomainName = $DomainName
            CARootName = $CARootName
            CAServerFQDN = $CAServerFQDN
            CertSubject = $CertSubject
            AutoRenew = $true
            SMBShare = $SMBShare
            Credential = $Credential
            RebootNodeifNeeded = $true
            CertificateFile = 'c:\PullServerConfig\Cert.cer'
            Thumbprint = 'B9A39921918B466EB1ADF2509E00F5DECB2EFDA9'
            }
        )
    }

PullServer -ConfigurationData $configData -OutputPath 'C:\PullServerConfig\'
Set-DscLocalConfigurationManager -ComputerName localhost -Path 'C:\PullServerConfig\'
Start-DscConfiguration -Wait -Force -Verbose -Path 'C:\PullServerConfig\'

# .\Script.ps1 -ServerName web1 -domainname 'test.pha' -carootname 'test-dc01-ca' -caserverfqdn 'dc01.test.pha' -certsubject 'CN=service.test.pha' -smbshare '\\sofs1.test.pha\share'
```

### <a name="verify-pull-server-functionality"></a>プル サーバーの機能を確認する

```powershell
# This function is meant to simplify a check against a DSC pull server. If you do not use the
# default service URL, you will need to adjust accordingly.
function Verify-DSCPullServer ($fqdn) {
    ([xml](Invoke-WebRequest "https://$($fqdn):8080/psdscpullserver.svc" | % Content)).service.workspace.collection.href
}

Verify-DSCPullServer 'INSERT SERVER FQDN'
```

```output
Expected Result:
Action
Module
StatusReport
Node
```

### <a name="configure-clients"></a>クライアントを構成する

```powershell
Configuration PullClient {
    param(
    $ID,
    $Server
    )
        LocalConfigurationManager
                {
                    ConfigurationID = $ID;
                    RefreshMode = 'PULL';
                    DownloadManagerName = 'WebDownloadManager';
                    RebootNodeIfNeeded = $true;
                    RefreshFrequencyMins = 30;
                    ConfigurationModeFrequencyMins = 15;
                    ConfigurationMode = 'ApplyAndAutoCorrect';
                    DownloadManagerCustomData = @{ServerUrl = "http://"+$Server+":8080/PSDSCPullServer.svc"; AllowUnsecureConnection = $true}
                }
}

PullClient -ID 'INSERTGUID' -Server 'INSERTSERVER' -Output 'C:\DSCConfig\'
Set-DscLocalConfigurationManager -ComputerName 'Localhost' -Path 'C:\DSCConfig\' -Verbose
```

## <a name="additional-references-snippets-and-examples"></a>その他の参照情報、スニペット、および例

この例では、テストのためにクライアント接続 (WMF5 が必要) を手動で開始する方法を示します。

```powershell
Update-DscConfiguration –Wait -Verbose
```

CNAME レコードを DNS ゾーンに追加する場合は、[Add-DnsServerResourceRecordName](/powershell/module/dnsserver/add-dnsserverresourcerecordcname) コマンドレットを使用します。

[チェックサムを作成し、SMB プル サーバーに DSC MOF を公開する](https://gallery.technet.microsoft.com/scriptcenter/PowerShell-Function-to-3bc4b7f0) PowerShell 関数では必要なチェックサムが自動的に生成されてから、MOF 構成とチェックサムの両方のファイルが SMB プル サーバーにコピーされます。

## <a name="appendix---understanding-odata-service-data-file-types"></a>付録 - ODATA サービス データ ファイルの種類の理解

データ ファイルは、OData Web サービスを含む、プル サーバーの展開時に情報を作成するために格納されます。 ファイルの種類は、以下の説明のとおり、オペレーティング システムによって異なります。

- **Windows Server 2012** - ファイルの種類は常に `.mdb` になる
- **Windows Server 2012 R2** - 構成で `.mdb` が指定されていない限り、ファイルの種類は既定で `.edb` になる

プル サーバーをインストールするための[スクリプトの詳細な例](https://github.com/mgreenegit/Whitepapers/blob/Dev/PullServerCPIG.md#installation-and-configuration-scripts)では、ファイルの種類によって発生する可能性のあるエラーを防ぐために web.config ファイル設定を自動的に制御する方法の例も示されます。
