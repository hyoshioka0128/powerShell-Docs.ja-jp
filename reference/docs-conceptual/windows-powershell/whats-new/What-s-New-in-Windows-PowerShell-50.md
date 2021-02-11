---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: Windows PowerShell 5.0 の新機能
description: これらのリリース ノートでは、Windows PowerShell 5.x の新機能と変更点について説明します。
ms.openlocfilehash: 82d3e10fa417085911242241dcfc54e931df8eb4
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663358"
---
# <a name="whats-new-in-windows-powershell-50"></a>Windows PowerShell 5.0 の新機能

Windows PowerShell 5.0 には、その用途を拡大し、使いやすさを向上させる重要な機能や、Windows ベースの環境をより簡単かつ包括的に制御および管理できるようにする重要な新しい機能が含まれています。

Windows PowerShell 5.0 には下位互換性があります。 Windows PowerShell 4.0、Windows PowerShell 3.0、Windows PowerShell 2.0 用に設計されたコマンドレット、プロバイダー、モジュール、スナップイン、スクリプト、関数、およびプロファイルは、全般的に変更なしで Windows PowerShell 5.0 でも動作します。

## <a name="installing-windows-powershell"></a>Windows PowerShell のインストール

Windows PowerShell 5.0 は、Windows Server 2016 Technical Preview および Windows に既定でインストールされています
10.

Windows PowerShell 5.0 を Windows Server 2012 R2、Windows 8.1 Enterprise または Windows 8.1 Pro にインストールするには、[Windows Management Framework 5.0](https://aka.ms/wmf5download) をダウンロードしてインストールします。 Windows Management Framework 5.0 をインストールする前に、ダウンロードの詳細を読み、システム要件がすべて満たされていることを確認してください。

## <a name="in-this-topic"></a>このトピックの内容

- [KB 3000850 の Windows PowerShell 4.0 DSC の更新プログラム](#windows-powershell-40-updates-in-november-2014-update-rollup-kb-3000850)
- [Windows PowerShell 5.0 の新機能](#new-features-in-windows-powershell-50)
- [Windows PowerShell 4.0 の新機能](#new-features-in-windows-powershell-40)
- [Windows PowerShell 3.0 の新機能](#new-features-in-windows-powershell-30)

## <a name="windows-powershell-40-updates-in-november-2014-update-rollup-kb-3000850"></a>2014 年 11 月の更新プログラムのロールアップ (KB 3000850) での Windows PowerShell 4.0 の更新

Windows PowerShell 4.0 での Windows PowerShell Desired State Configuration (DSC) に対する多くの更新と機能強化が、[Windows RT 8.1、Windows 8.1、Windows Server 2012 R2 の 2014 年 11 月の更新プログラムのロールアップ](https://support.microsoft.com/kb/3000850/) (KB 3000850) で利用可能になりました。 Windows PowerShell で `Get-Hotfix -Id KB3000850` を実行することで、KB 3000850 がシステムにインストールされているかどうかを判断できます。

- [PSDesiredStateConfiguration](/powershell/module/PSDesiredStateConfiguration) モジュールの既存のコマンドレットの更新
  - [Get-DscResource](/powershell/module/PSDesiredStateConfiguration) が高速になりました (特に ISE で)。
  - [Start-DscConfiguration](/powershell/module/PSDesiredStateConfiguration) に新しいパラメーター、-UseExisting が追加されました。これは最後に適用した構成を再適用します。
  - [Start-DscConfiguration](/powershell/module/PSDesiredStateConfiguration) -Force が修正されました。
  - [Get-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration) がエンジン状態に関するより有用な情報を表示します。
  - [Test-DscConfiguration](/powershell/module/PSDesiredStateConfiguration) が True または False と共にコンピューター名を返すようになりました。
  - [New-DscChecksum](/powershell/module/PSDesiredStateConfiguration) が UNC パスをサポートするようになりました。

- [PSDesiredStateConfiguration](/powershell/module/PSDesiredStateConfiguration) モジュールの新しいコマンドレット
  - [Update-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Update-DscConfiguration):オンデマンド プル サーバー チェックを実行します。
  - [Stop-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Stop-DscConfiguration):既に実行されている構成を停止します。
  - [Remove-DscConfigurationDocument](/powershell/module/PSDesiredStateConfiguration/Remove-DscConfigurationDocument):さまざまな段階 (保留中、以前、または現在) の構成に関するドキュメントを削除できます。

- 言語の機能強化
  - DependsOn が複合リソースをサポートするようになりました。
  - DependsOn がリソース インスタンス名の番号をサポートするようになりました。
  - ノード式の評価が空の場合に、エラーをスローしなくなりました。
  - ノード式の評価が空の場合に発生するエラーが修正されました。
  - 構成を呼び出す構成が Windows PowerShell コンソールで機能するようになりました。

- プル モードの機能強化
  - プル モードがすべての ZIP ファイルをサポートするようになりました。
  - **AllowModuleOverwrite** が正しく動作するようになりました。

- 回復性の向上
  - 新しい **DebugMode** でリソース モジュールを再読み込みすることができます。
  - 構成エラーが発生した場合は、pending.mof ファイルは削除されません。
  - メタ構成の設定が破損した場合のローカル構成マネージャー (LCM) の回復性が向上しました。

- 診断の向上
  - LCM がタイマーを、ユーザーが指定した以外の別の設定に設定すると、警告が表示されます。
  - エラー ログ ファイルに Windows PowerShell リソースの呼び出し履歴が含まれるようになりました。

- スケーラビリティの向上
  - LocalConfigurationManager リソースに新しいプロパティ、 **ActionAfterReboot** が追加されました。
    - ContinueConfiguration (既定値):ターゲット ノードの再起動後に、構成を自動的に再開します。
    - StopConfiguration:ノードの再起動後、構成を自動的に再開しません。
  - 整合性の実行をプル操作よりも頻繁に行えるようになりました。また、その逆も可能です。
  - バージョン管理のサポート:DSC が新しいクライアント ([WMF 5.0](https://aka.ms/wmf5download) に含まれる) で生成されたドキュメントを認識できるようになりました。

- エラー防止の向上
  - 構成が適用される前に、モジュールのバージョンが適用されるようになりました。
  - **DebugPreference** が、Get-、Set-、または Test-TargetResource の各呼び出しに対して適切に設定されるようになりました。

- 資格情報の処理の向上
  - **Certificate** と **PSDscAllowPlainTextPassword** の両方が指定される場合、証明書が使用されるようになりました。
  - 資格情報は、暗号化が解除されます。Get-TargetResource でも暗号化解除されます。
  - メタ構成の資格情報は暗号化および暗号化解除されます。
  - PSCredentials は、埋め込みオブジェクトにある場合に暗号化が解除されるようになりました。

- 組み込みリソースの向上
  - パッケージ リソース
    - 正しくないパッケージはインストールされなくなりました (ローカルまたは Web ソースのどちらからも)。
    - HTTPS をサポートするようになりました。
  - [パッケージ リソース](/powershell/scripting/dsc/reference/resources/windows/packageresource)で HTTPS がサポートされるようになりました。
  - [アーカイブ リソース](/powershell/scripting/dsc/reference/resources/windows/archiveresource)が資格情報をサポートするようになりました。

## <a name="new-features-in-windows-powershell-50"></a>Windows PowerShell 5.0 の新機能

- [Windows PowerShell の新機能](#new-features-in-windows-powershell)
- [Windows PowerShell Desired State Configuration の新機能](#new-features-in-windows-powershell-desired-state-configuration)
- [Windows PowerShell ISE の新機能](#new-features-in-windows-powershell-ise)
- [Windows PowerShell Web サービスの新機能](#new-features-in-windows-powershell-web-services-management-odata-iis-extension)
- [Windows PowerShell 5.0 での主なバグ修正](#notable-bug-fixes-in-windows-powershell-50)

### <a name="new-features-in-windows-powershell"></a>Windows PowerShell の新機能

- Windows PowerShell 5.0 以降では、クラスを使用して、他のオブジェクト指向のプログラミング言語に類似した正式な構文とセマンティクスを使用して開発することができます。 **Class** 、 **Enum** などのキーワードが、新しい機能をサポートするために Windows PowerShell 言語に追加されました。
  クラスに関する作業の詳細については、「about_Classes」を参照してください。

- Windows PowerShell 5.0 では、スクリプトとその呼び出し元 (またはホスト環境) の間で構造化データを転送するために使用できる新しい構造化された情報ストリームが導入されています。 Write-Host を使用して、情報ストリームに出力を生成できるようになりました。 また、情報ストリームは PowerShell.Streams、ジョブ、スケジュールされたジョブ、およびワークフローでも機能します。 次の機能は情報ストリームをサポートしています。
  - Windows PowerShell がコマンドの情報ストリーム データを処理する方法を指定できる新しい Write-Information コマンドレット。 Write-Host は、Write-Information のラッパーです。 Write-Information はサポートされているワークフロー アクティビティでもあります。
  - 2 つの新しい共通パラメーター、InformationVariable と InformationAction を使用して、コマンドからの情報ストリームを表示する方法を決定できます。 InformationAction の有効な値は、SilentlyContinue、Stop、Continue、Inquire、Ignore、または Suspend です。既定値は SilentlyContinue です。 InformationVariable は、保存されたコマンドからの必要な Write-Host データに対する変数の名前として文字列を指定します。
  - 新しいユーザー設定変数、InformationPreference は、Windows PowerShell セッション内の情報ストリーム データの既定の基本設定を指定します。 既定値は SilentlyContinue です。
  - 2 つの新しいワークフロー共通パラメーター、PSInformation と InformationAction が追加されました。
  - Format-Table コマンドを使用すると、ストリームを通過する最初の 300 ミリ秒のデータが評価され、テーブル列が自動的に書式設定されるようになりました。

- [Microsoft Research](https://research.microsoft.com/) とのコラボレーションにより、新しいコマンドレット、ConvertFrom-String が追加されました。 ConvertFrom-String では、テキスト文字列のコンテンツから構造化されたオブジェクトを抽出して、解析することができます。 詳細については、ConvertFrom-String を参照してください。
- 新しい Convert-String コマンドレットは、-Example パラメーターで指定する例を基にテキストを自動的に書式設定します。
- 新しいモジュール、Microsoft.PowerShell.Archive には、コマンドレットが含まれています。そのコマンドレットでは、ファイルとフォルダーをアーカイブ (ZIP とも呼ばれる) ファイルに圧縮して、既存の ZIP ファイルからファイルを抽出し、ZIP ファイル内で圧縮されたファイルの新しいバージョンで ZIP ファイルを更新できます。
- 新しいモジュール、PackageManagement では、インターネット上のソフトウェア パッケージを検出してインストールすることができます。
  PackageManagement (旧 OneGet) モジュールは、Windows のパッケージ管理を単一の Windows PowerShell インターフェイスと統一するための、マネージャー、または既存のパッケージ マネージャー (パッケージ プロバイダーとも呼ばれる) のマルチプレクサーです。
- 新しいモジュール、PowerShellGet では、[PowerShell ギャラリー](https://www.powershellgallery.com/)上、または Register-PSRepository コマンドレットを実行することでセットアップできる内部モジュール リポジトリ上で、モジュールと DSC リソースを検索、インストール、発行、および更新することができます。
- 新しい言語キーワード、 **Hidden** が追加され、メンバー (プロパティまたはメソッド) が Get-Member の結果に既定で表示されないように指定できるようになりました (-Force パラメーターを追加する場合を除く)。
  非表示としてマークされているプロパティやメソッドは、IntelliSense の結果にも表示されません。ただし、メンバーが表示される必要のあるコンテキスト内で作業している場合を除きます。たとえば、クラス メソッドを使用している場合、自動変数 $This は非表示のメンバーを表示する必要があります。
- New-Item、Remove-Item、および Get-ChildItem が[シンボリック リンク](https://en.wikipedia.org/wiki/Symbolic_link)の作成と管理をサポートするために強化されました。 New-Item の **-ItemType** パラメーターでは、新しい値、 **SymbolicLink** を使用できます。 シンボリック リンクは、New-Item コマンドレットを実行することで、1 つの行で作成できるようになりました。
- また、Get-ChildItem には新しい -Depth パラメーターも追加されました。これを -Recurse パラメーターと共に使用すると再帰を制限します。 たとえば、Get-ChildItem -Recurse -Depth 2 は、現在のフォルダー、現在のフォルダー内のすべての子フォルダー、および子フォルダー内のすべてのフォルダーから結果を返します。
- Copy-Item で、1 つの Windows PowerShell セッションから別のセッションにファイルまたはフォルダーをコピーできるようになりました。つまり、リモート コンピューター (Nano Server を実行しているため他のインターフェイスを持たないコンピューターを含む) に接続されているセッションにファイルをコピーできるようになりました。 ファイルをコピーするには、新しい -FromSession と -ToSession パラメーターの値として PSSession ID を指定し、-Path と -Destination を追加して元のパスと宛先をそれぞれ指定します。 例: Copy-Item -Path c:\\myFile.txt -ToSession $s -Destination d:\\destinationFolder
- Windows PowerShell トランスクリプションが強化され、コンソール ホスト ( **powershell.exe** ) に加えて、すべてのホスト アプリケーション (Windows PowerShell ISE など) に適用されるようになりました。 トランスクリプション オプション (システム全体のトランスクリプトの有効化を含む) は、 **[PowerShell トランスクリプションを有効にする]** グループ ポリシー設定 ([管理用テンプレート]/[Windows コンポーネント]/[Windows PowerShell]) を有効にすることで設定できます。
- 新しい詳細スクリプト トレース機能では、システムで使用される Windows PowerShell スクリプトの詳細な追跡や分析を有効にすることができます。 詳細なスクリプト トレースを有効にすると、Windows PowerShell はすべてのスクリプト ブロックを Windows イベント トレーシング (ETW) イベント ログ、 **Microsoft-Windows-PowerShell/Operational** に記録します。
- Windows PowerShell 5.0 以降では、新しい Cryptographic Message Syntax コマンドレットが、[RFC5652](https://tools.ietf.org/html/rfc5652) で説明されているように暗号によってメッセージを保護するための IETF 標準書式を使用して、コンテンツの暗号化と暗号化解除をサポートします。 Get-CmsMessage、Protect-CmsMessage、および Unprotect-CmsMessage の各コマンドレットが [Microsoft.PowerShell.Security](/powershell/module/Microsoft.PowerShell.Security) モジュールに追加されました。
- [Microsoft.PowerShell.Utility](/powershell/module/Microsoft.PowerShell.Utility) モジュールの新しいコマンドレット、Get-Runspace、Debug-Runspace、Get-RunspaceDebug、Enable-RunspaceDebug、および Disable-RunspaceDebug を使用すると、実行空間でのデバッグ オプションを設定して、実行空間でのデバッグを開始、停止できます。 任意の実行空間 (つまり、Windows PowerShell コンソールまたは Windows PowerShell ISE セッションの既定の実行空間ではない実行空間) をデバッグする場合、Windows PowerShell では、スクリプトにブレークポイントを設定し、追加されたブレークポイントを使用して、実行空間スクリプトをデバッグするデバッガーをアタッチするまでスクリプトが実行されないようにすることができます。 実行空間用の Windows PowerShell スクリプト デバッガーに、任意の実行空間に対する入れ子になったデバッグのサポートが追加されました。
- 新しい Format-Hex コマンドレットが [Microsoft.PowerShell.Utility](/powershell/module/Microsoft.PowerShell.Utility) モジュールに追加されました。
  Format-Hex を使うと、テキスト データやバイナリ データを 16 進数形式で表示できます。
- Get-Clipboard コマンドレットと Set-Clipboard コマンドレットが、[Microsoft.PowerShell.Utility](/powershell/module/Microsoft.PowerShell.Utility) モジュールに追加されました。これらのコマンドレットにより、Windows PowerShell セッションとの間のコンテンツの転送が容易になります。 クリップボードのコマンドレットは、画像、オーディオ ファイル、ファイルのリスト、およびテキストをサポートします。
- 新しいコマンドレット、Clear-RecycleBin が、[Microsoft.PowerShell.Management](/powershell/module/Microsoft.PowerShell.Management) モジュールに追加されました。このコマンドレットは、外部ドライブを含む固定ドライブのゴミ箱を空にします。 既定では、コマンドレットの ConfirmImpact プロパティが ConfirmImpact.High に設定されているため、Clear-RecycleBin コマンドを確認するよう求めるメッセージが表示されます。
- 新しいコマンドレット、New-TemporaryFile では、スクリプトの一部として一時ファイルを作成できます。 既定では、新しい一時ファイルは ```C:\Users\<user name>\AppData\Local\Temp``` に作成されます。
- Out-File、Add-Content、および Set-Content の各コマンドレットに新しい -NoNewline パラメーターが追加されました。これを指定すると、出力後の改行が省略されます。
- New-Guid コマンドレットは、.NET Framework Guid クラスを利用して GUID を生成します。これはスクリプトまたは DSC リソースを作成している場合に便利です。
- ファイルのバージョン情報は、特にファイルにパッチが適用された場合に、解釈を間違えやすいデータであるため、新しいスクリプト プロパティ、FileVersionRaw と ProductVersionRaw が FileInfo オブジェクトに対して使用できます。 たとえば、次のコマンドを実行して、powershell.exe のこれらのプロパティ値を表示できます。ここで、$pid には Windows PowerShell の実行中のセッションのプロセス ID が含まれます: `Get-Process -Id $pid -FileVersionInfo | Format-List *version* -Force`
- 新しいコマンドレット、Enter-PSHostProcess と Exit-PSHostProcess を使用すると、Windows PowerShell コンソールで実行している現在のプロセスとは別のプロセスで Windows PowerShell スクリプトをデバッグできます。 Enter-PSHostProcess を実行して、特定のプロセス ID を入力するか、特定のプロセス ID にアタッチしてから、Get-Runspace を実行して、プロセス内のアクティブな実行空間を返します。 プロセス内でスクリプトのデバッグが完了したら、Exit-PSHostProcess を実行して、プロセスからデタッチします。
- 新しい Wait-Debugger コマンドレットが [Microsoft.PowerShell.Utility](/powershell/module/Microsoft.PowerShell.Utility) モジュールに追加されました。 Wait-Debugger を実行すると、スクリプトの次のステートメントを実行する前に、デバッガー内のスクリプトを停止することができます。
- Windows PowerShell ワークフロー デバッガーで、コマンドまたはタブ補完をサポートするようになったため、入れ子になったワークフロー関数をデバッグすることができます。 **Ctrl+Break** キーを押すと、実行中のスクリプト、ローカルとリモートの両方のセッション、およびワークフロー スクリプトで、デバッガーに入れるようになりました。
- Debug-Job コマンドレットが [Microsoft.PowerShell.Core](/powershell/module/Microsoft.PowerShell.Core) モジュールに追加され、Windows PowerShell ワークフローの実行中のジョブ スクリプト、バックグラウンド、およびリモート セッションで実行するジョブをデバッグできます。
- 新しい状態、AtBreakpoint が Windows PowerShell ジョブに追加されました。 AtBreakpoint 状態は、ジョブがブレークポイントの設定を含むスクリプトを実行し、スクリプトがブレークポイントにヒットした場合に適用されます。 ジョブがデバッグ ブレークポイントで停止したら、Debug-Job コマンドレットを実行して、ジョブをデバッグする必要があります。
- Windows PowerShell 5.0 では、$PSModulePath の同一フォルダーにある 1 つの Windows PowerShell モジュールの複数のバージョンに対するサポートを実装しています。 RequiredVersion プロパティが ModuleSpecification クラスに追加され、必要なバージョンのモジュールを取得できるようになりました。このプロパティは、ModuleVersion プロパティとは相互に排他的です。 RequiredVersion が、Get-Module、Import-Module、および Remove-Module コマンドレットの FullyQualifiedName パラメーターの値の一部としてサポートされるようになりました。
- Test-ModuleManifest コマンドレットを実行して、モジュール バージョンを検証できるようになりました。
- Get-Command コマンドレットの結果が、バージョン列に表示されるようになりました。新しい Version プロパティが CommandInfo クラスに追加されました。 Get-Command では、同じモジュールの複数のバージョンからのコマンドが表示されます。 Version プロパティは、CmdletInfo の派生クラスの一部 (CmdletInfo と ApplicationInfo) でもあります。
- Get-Command に、ShowCommand の情報を PSObject として返す新しいパラメーター、-ShowCommandInfo が追加されました。 これは、Windows PowerShell リモート処理を使用して、Windows PowerShell ISE で Show-Command を実行する場合に、特に便利な機能です。 Microsoft.PowerShell.Utility モジュールの既存の Get-SerializedCommand 関数は、-ShowCommandInfo に置き換えられます。ただし、Get-SerializedCommand スクリプトは、ダウンレベルのスクリプトをサポートするために引き続き使用できます。
- 新しい Get-ItemPropertyValue コマンドレットでは、ドット表記を使用せずにプロパティの値を取得できます。 たとえば、Windows PowerShell の以前のリリースでは、PowerShellEngine レジストリ キーの Application Base プロパティの値を取得するには、コマンド **(Get-ItemProperty -Path HKLM:\\SOFTWARE\\Microsoft\\PowerShell\\3\\PowerShellEngine -Name ApplicationBase).ApplicationBase** を実行します。 Windows PowerShell 5.0 以降では、 **Get-ItemPropertyValue -Path HKLM:\\SOFTWARE\\Microsoft\\PowerShell\\3\\PowerShellEngine -Name ApplicationBase** を実行できます。
- Windows PowerShell コンソールでは、Windows PowerShell ISE と同じ方法の構文の色分けを使用するようになりました。
- 新しい NetworkSwitch モジュールには、スイッチ、仮想 LAN (VLAN)、および基本的なレイヤー 2 ネットワーク スイッチ ポートの構成を、Windows Server 2012 R2 ロゴ認定を受けたネットワーク スイッチに適用できるコマンドレットが含まれます。
- FullyQualifiedName パラメーターが、Import-Module コマンドレットと Remove-Module コマンドレットに追加され、1 つのモジュールの複数のバージョンの保存をサポートします。
- Save-Help、Update-Help、Import-PSSession、Export-PSSession、および Get-Command に、ModuleSpecification 型の新しいパラメーター、FullyQualifiedModule が追加されました。 このパラメーターを追加して、完全修飾名でモジュールを指定します。
- **$PSVersionTable.PSVersion** の値が 5.0 に更新されました。
- WMF 5.0 (PowerShell 5.0) には **Pester** モジュールが含まれます。 Pester は、PowerShell の単体テスト フレームワークです。 スクリプトのテストを作成できるいくつかの使いやすいキーワードが提供されます。

### <a name="new-features-in-windows-powershell-desired-state-configuration"></a>Windows PowerShell Desired State Configuration の新機能

- Windows PowerShell 言語の機能が強化され、クラスを使用して、Windows PowerShell Desired State Configuration (DSC) のリソースを定義できるようになりました。 Import-DscResource は真の動的キーワードになりました。Windows PowerShell は、DscResource 属性を含むクラスを探して、指定されたモジュールのルート モジュールを解析します。 クラスを使用して DSC リソースを定義できるようになりました。このリソースには、MOF ファイルも、モジュール フォルダー内の DSCResource サブフォルダーも必要ありません。 Windows PowerShell モジュールのファイルには、複数の DSC リソース クラスを含めることができます。
- 新しいパラメーター、ThrottleLimit が PSDesiredStateConfiguration モジュールの次のコマンドレットに追加されました。 ThrottleLimit パラメーターを追加して、コマンドを同時に実行するターゲット コンピューターまたはデバイスの数を指定します。
  - Get-DscConfiguration
  - Get-DscConfigurationStatus
  - Get-DscLocalConfigurationManager
  - Restore-DscConfiguration
  - Test-DscConfiguration
  - Compare-DscConfiguration
  - Publish-DscConfiguration
  - Set-DscLocalConfigurationManager
  - Start-DscConfiguration
  - Update-DscConfiguration
- 一元化された DSC エラー報告では、詳細なエラー情報がログに記録されるだけでなく、後で分析できるように中央の場所に送信できます。 この中央の場所を使用して、環境内でサーバーに対して発生した DSC 構成エラーを保存することができます。 メタ構成でレポート サーバーが定義されると、すべてのエラーがレポート サーバーに送信され、データベースに保存されます。 ターゲット ノードがプル サーバーから構成を引き出すように構成されているかどうかに関係なく、この機能をセットアップすることができます。
- Windows PowerShell ISE の機能向上により、DSC リソースを簡単に作成できるようになりました。 次のことを行うことができます。
  - **構成** ブロック内や **ノード** ブロック内の空白行に **Ctrl+Space** を入力して、ブロック内の DSC リソースをすべて一覧表示する。
  - **列挙** 型のリソース プロパティにおけるオート コンプリート。
  - 構成内の他のリソース インスタンスに応じた、DSC リソースの **DependsOn** プロパティにおけるオート コンプリート。
  - リソース プロパティ値のタブ補完の強化。
- ユーザーは、 **PSDscRunAsCredential** 属性をノード ブロックに追加することで、指定した資格情報セットの下でリソースを実行できるようになりました。 例: PSDscRunAsCredential = Get-Credential Contoso\\DscUser。 この機能は、Windows インストーラーと実行可能インストーラーを実行する構成、ユーザーごとのレジストリ ハイブにアクセスする構成、または現在のユーザー コンテキストの外部の他のタスクを実行する構成を作成するのに役立ちます。
- 32 ビット (x86 ベース) のサポートが、 **Configuration** キーワードに追加されました。
- Windows PowerShell には、生成された構成関数に \[CmdletBinding()] を追加することで定義される、DSC 構成のカスタム ヘルプに対するサポートが含まれるようになりました。
- 新しい **DscLocalConfigurationManager** 属性では、メタ構成として構成ブロックを指定します。その構成ブロックを使って DSC ローカル構成マネージャーが構成されます。 この属性は、DSC ローカル構成マネージャーを構成するアイテムのみが含まれるように、構成を制限します。 処理中に、この構成によって \*.meta.mof ファイルが生成されます。その後、Set-DscLocalConfigurationManager コマンドレットを実行して、そのファイルは該当するターゲット ノードに送られます。
- Windows PowerShell 5.0 で、部分構成ができるようになりました。 ノードに対して構成ドキュメントをフラグメントとして提供できます。 構成ドキュメントの複数のフラグメントをノードが受け取るには、想定されるフラグメントを指定するように、そのノードのローカル構成マネージャーをまず設定する必要があります
- コンピューター間の同期は、Windows PowerShell 5.0 の DSC での新機能です。 組み込みの WaitFor\* リソース ( **WaitForAll** 、 **WaitForAny** 、および **WaitForSome** ) を使用すると、コンピューター間の依存関係を外部のオーケストレーションなしで構成の実行中に指定できます。 これらのリソースは、WS-Man プロトコルで CIM 接続を使用して、ノード間の同期を提供します。
  構成は、別のコンピューターの特定のリソースの状態が変更されるまで待機できます。
- 新しい委任セキュリティ機能である Just Enough Administration (JEA) は、DSC と Windows PowerShell の制限付き実行空間を活用して、意図的であるかどうかに関わらず、従業員によるデータの損失や侵害から企業をセキュリティで保護します。 xJEA DSC リソースをダウンロードできる場所など、JEA に関する詳細については、「[Just Enough Administration](/powershell/scripting/learn/remoting/jea/overview)」をご覧ください。
- PSDesiredStateConfiguration モジュールには、次の新しいコマンドレットが追加されました。
  - 新しい Get-DscConfigurationStatus コマンドレットは、ターゲット ノードから構成状態に関する高レベルの情報を取得します。 最後の状態、またはすべての構成の状態を取得できます。
  - 新しい Compare-DscConfiguration コマンドレットは、指定した構成を 1 つ以上のターゲット ノードの実際の状態と比較します。
  - 新しい Publish-DscConfiguration コマンドレットでは、構成の MOF ファイルをターゲット ノードにコピーしますが、その構成は適用しません。 この構成が適用されるのは、次の整合性パス中、または Update-DscConfiguration コマンドレットの実行時です。
  - 新しい Test-DscConfiguration コマンドレットを使用すると、結果の構成が必要な構成と一致することを確認して、構成が必要な構成と一致する場合は True を、実際の構成が必要な構成と一致しない場合は False を返すようにできます。
  - 新しい Update-DscConfiguration コマンドレットは、構成が処理されるようにします。 ローカル構成マネージャーがプル モードの場合は、コマンドレットは、構成を適用する前にプル サーバーから構成を取得します。

### <a name="new-features-in-windows-powershell-ise"></a>Windows PowerShell ISE の新機能

- Windows PowerShell ISE のローカル コピーでリモートの Windows PowerShell のスクリプトとファイルを編集できるようになりました。この編集を行うには、Enter-PSSession を実行し、編集するファイルが保存されているコンピューター上でリモート セッションを開始して、 **PSEdit \<path and file name on the remote computer\>** を実行します。 この機能を使用すると、Windows PowerShell ISE を実行できない Windows Server の Server Core インストール オプションに保存されている Windows PowerShell ファイルの編集が簡単になります。
- Start-Transcript コマンドレットが Windows PowerShell ISE でサポートされるようになりました。
- Windows PowerShell ISE でリモート スクリプトをデバッグできるようになりました。
- 新しいメニュー コマンド、 **Break All** (Ctrl+B) は、ローカルとリモートで実行中の両方のスクリプトを中断し、デバッガーに移ります。

### <a name="new-features-in-windows-powershell-web-services-management-odata-iis-extension"></a>Windows PowerShell Web サービス (Management OData IIS 拡張機能) の新機能

- Windows PowerShell 5.0 以降では、新しい [Microsoft.PowerShell.OdataUtils](/powershell/module/microsoft.powershell.odatautils) モジュールにある Export-ODataEndpointProxy コマンドレットを実行すると、指定した OData エンドポイントによって公開される機能に基づいて、Windows PowerShell コマンドレットのセットを生成できます。

### <a name="notable-bug-fixes-in-windows-powershell-50"></a>Windows PowerShell 5.0 での主なバグ修正

- Windows PowerShell 5.0 には、新しい COM 実装が含まれます。この実装により、COM オブジェクトを操作する場合のパフォーマンスが大幅に向上します。 その効果を示すビデオ デモについては、「[Com_Perf_Improvements](https://1drv.ms/1qu3UPZ)」をご覧ください。
- Windows PowerShell セッションでの最初のタブ補完に対するパフォーマンスが大幅に改善され、タブ補完に要する時間が 500 ミリ秒近く短縮されました。

## <a name="new-features-in-windows-powershell-40"></a>Windows PowerShell 4.0 の新機能

Windows PowerShell 4.0 には下位互換性があります。 Windows PowerShell 3.0 および Windows PowerShell 2.0 用に設計されたコマンドレット、プロバイダー、モジュール、スナップイン、スクリプト、関数、およびプロファイルは、変更なしで Windows PowerShell 4.0 でも動作します。

Windows PowerShell 4.0 は、既定で Windows 8.1 と Windows Server 2012 R2 にインストールされています。 Windows 7 SP1 または Windows Server 2008 R2 に Windows PowerShell 4.0 をインストールするには、[Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) をダウンロードしてインストールします。 Windows Management Framework 4.0 をインストールする前に、ダウンロードの詳細を読み、システム要件がすべて満たされていることを確認してください。

- [Windows PowerShell の新機能](#new-features-in-windows-powershell-1)
- [Windows PowerShell Integrated Scripting Environment (ISE) の新機能](#new-features-in-windows-powershell-integrated-scripting-environment-ise)
- [Windows PowerShell ワークフローの新機能](#new-features-in-windows-powershell-workflow)
- [Windows PowerShell Web サービスの新機能](#new-features-in-windows-powershell-web-services)
- [Windows PowerShell Web Access の新機能](#new-features-in-windows-powershell-web-access)
- [Windows PowerShell 4.0 での主なバグ修正](#notable-bug-fixes-in-windows-powershell-40)

Windows PowerShell 4.0 には、次に示す新機能があります。

### <a name="new-features-in-windows-powershell"></a><a name="new-features-in-windows-powershell-1" />Windows PowerShell の新機能

- **Windows PowerShell Desired State Configuration** (DSC) は、ソフトウェア サービスとそれらのサービスが実行される環境に対して構成データのデプロイと管理を実行できる Windows PowerShell 4.0 の新しい管理システムです。 DSC の詳細については、「[Windows PowerShell Desired State Configuration の概要](/powershell/scripting/dsc/getting-started/wingettingstarted)」を参照してください。
- **Save-Help** では、リモート コンピューターにインストールされているモジュールのヘルプを保存できるようになりました。 Save-Help を使用すると、インターネットに接続されたクライアント (ヘルプが必要なモジュールのすべてがインストールされているとは限らない) でヘルプのモジュールをダウンロードし、リモート共有フォルダーまたはインターネットへのアクセスがないリモート コンピューターにヘルプをコピーして保存できます。
- Windows PowerShell デバッガーが拡張され、Windows PowerShell ワークフローのデバッグと、リモート コンピューターで実行されているスクリプトのデバッグが可能になりました。 Windows PowerShell ワークフローは、Windows PowerShell コマンド ラインまたは Windows PowerShell ISE からスクリプト レベルでデバッグできるようになりました。 Windows PowerShell スクリプト (スクリプト ワークフローを含む) は、リモート セッション経由でデバッグできるようになりました。 リモート デバッグ セッションは、切断されても Windows PowerShell リモート セッション上に保存され、後で再接続されます。
- **Register-ScheduledJob** および **Set-ScheduledJob** で **RunNow** パラメーターを使用できるようになり、 **Trigger** パラメーターを使用して直後のジョブ開始日時を設定する必要がなくなりました。
- **Invoke-RestMethod** と **Invoke-WebRequest** で、Headers パラメーターを使用してすべてのヘッダーを設定できるようになりました。 このパラメーターは以前から存在しますが、使用すると例外やエラーが発生する Web コマンドレットのいくつかのパラメーターの 1 つでした。
- **Get-Module** に、新しいパラメーター **FullyQualifiedName** が追加されました ( **ModuleSpecification\[]** 型)。 Get-Module の **FullyQualifiedName** パラメーターを使用すると、モジュール名やバージョンを使用してモジュールを指定でき、オプションとして GUID でも指定できます。
- Windows Server 2012 R2 での既定の実行ポリシー設定は **RemoteSigned** です。 Windows 8.1 では、既定の設定に変更はありません。
- Windows PowerShell 4.0 以降で、動的なメソッド名を使用したメソッド呼び出しがサポートされるようになりました。
  変数にメソッド名を格納し、その変数を呼び出すことによって動的にメソッドを呼び出せます。
- **PSElapsedTimeoutSec** ワークフロー共通パラメーターで指定したタイムアウト期間が経過したとき、非同期のワークフロー ジョブは削除されなくなりました。
- 新しいパラメーター **RepeatIndefinitely** が **New-JobTrigger** および **Set-JobTrigger** コマンドレットに追加されました。 これを使用すると、スケジュールされたジョブの実行を無期限に繰り返す場合に、 **RepetitionDuration** パラメーターの **TimeSpan.MaxValue** 値に指定する必要がなくなります。
- **Passthru** パラメーターが **Enable-JobTrigger** および **Disable-JobTrigger** コマンドレットに追加されました。 Passthru パラメーターを使用すると、実行するコマンドによって作成または変更されるすべてのオブジェクトが表示されます。
- **Add-Computer** および **Remove-Computer** コマンドレットにワークグループを指定するパラメーターの名前が整合性のあるものになりました。 どちらのコマンドレットでも、パラメーター **WorkgroupName** を使用します。
- 新しい共通パラメーター **PipelineVariable** が追加されました。 PipelineVariable を使用すると、パイプされたコマンド (またはパイプされたコマンドの一部) の結果を変数として保存し、パイプラインの残りの部分に引き渡すことができます。
- メソッド構文を使用したコレクションのフィルター処理がサポートされるようになりました。 Where() または Where-Object の構文に似た簡潔なメソッド呼び出しの形式を使用して、オブジェクトのコレクションをフィルター処理できます。 以下に例を示します。(Get-Process).where({$_.Name -match 'powershell'})
- **Get-Process** コマンドレットに新しいスイッチ パラメーター **IncludeUserName** が追加されました。
- 新しいコマンドレット **Get-FileHash** が追加されました。これは、指定したファイルのファイル ハッシュをいくつかの形式の中から 1 つ返します。
- Windows PowerShell 4.0 では、モジュールが自らのマニフェストの中で **DefaultCommandPrefix** キーを使用している場合、またはユーザーが **Prefix** パラメーターを指定してモジュールをインポートした場合、モジュールの **ExportedCommands** プロパティにモジュール内のコマンドがプレフィックス付きで表示されます。 モジュールで修飾された構文 (ModuleName\\CommandName) を使用してコマンドを実行するときは、コマンド名にプレフィックスを含める必要があります。
- **$PSVersionTable.PSVersion** の値が 4.0 に更新されました。
- **Where()** 演算子の動作が変更されました。 `Collection.Where('property -match name')` は形式 `"Property -CompareOperator Value"` の文字列式を受け入れなくなりました。
  ただし、 **Where()** 演算子は、スクリプト ブロック形式の文字列式を受け入れます。これは、引き続きサポートされます。

### <a name="new-features-in-windows-powershell-integrated-scripting-environment-ise"></a>Windows PowerShell Integrated Scripting Environment (ISE) の新機能

- Windows PowerShell ISE では、Windows PowerShell ワークフローのデバッグとリモート スクリプト デバッグの両方がサポートされます。
- Windows PowerShell Desired State Configuration のプロバイダーおよび構成に IntelliSense サポートが追加されました。

### <a name="new-features-in-windows-powershell-workflow"></a>Windows PowerShell ワークフローの新機能

- System Center Orchestrator などで使用されている反復パイプラインのコンテキストで、新しい **PipelineVariable** 共通パラメーターのサポートが追加されました。反復パイプラインとは、ストリーミングを使用することによって散発的に実行するのではなく、コマンドを単純に左から右へと実行するパイプラインです。
- パラメーターのバインドが大幅に拡張され、Tab 補完のシナリオを越えて、たとえば、現在の実行空間に存在しないコマンドでも機能するようになりました。
- Windows PowerShell ワークフローにカスタム コンテナー アクティビティのサポートが追加されました。 アクティビティのパラメーターの型が **Activity** または **Activity\[]** (またはアクティビティのジェネリック コレクション) である場合に、ユーザーが引数としてスクリプト ブロックを提供すると、Windows PowerShell ワークフローはそのスクリプト ブロックを通常の Windows PowerShell によるスクリプトからワークフローへのコンパイルと同様に XAML に変換します。
- クラッシュの後、Windows PowerShell ワークフローは自動的に管理対象ノードに再接続します。
- **Foreach-Parallel** アクティビティ ステートメントのスロットルを、 **ThrottleLimit** プロパティを使用して調整できるようになりました。
- **ErrorAction** 共通パラメーターに新しい有効値 **Suspend** が追加されました。これはワークフロー専用です。
- ワークフロー エンドポイントは、アクティブなセッション、処理中のジョブ、および保留中のジョブがいずれもなくなった時点で、自動的に閉じるようになりました。 この機能は、自動クローズ条件を満たしている場合に、ワークフロー サーバーとして機能しているコンピューターでリソースの節約になります。

### <a name="new-features-in-windows-powershell-web-services"></a>Windows PowerShell Web サービスの新機能

- コマンドレットの実行中に Windows PowerShell Web サービス (PSWS、Management OData IIS 拡張機能ともいう) でエラーが発生した場合に、より詳細なエラー メッセージが呼び出し元に返されます。 さらに、エラーコードは「[Windows Azure REST API のエラー コードのガイドライン](/rest/api/storageservices/Common-REST-API-Error-Codes)」に従います。
- エンドポイントで API のバージョンを定義したり、特定の API バージョンの使用を強制したりできるようになりました。 クライアントとサーバーの間でバージョンの不一致が発生すると、クライアントとサーバーの両方にエラーが表示されます。
- ディスパッチ スキーマの管理が簡略化され、スキーマで欠落しているフィールドの値が自動的に生成されるようになりました。 ディスパッチ スキーマが存在しない場合でも、生成された値は出発点として役立ちます。
- PSWS での型の処理が改良され、既定のコンストラクターとは異なるコンストラクターを使用する型がサポートされ、Windows PowerShell での **PSTypeConverter** と同様の動作になりました。 これにより、PSWS で複合型を使用できます。
- PSWS で、クエリの実行中に、関連付けられているインスタンスを展開できるようになりました。 バイナリ コンテンツ (イメージ、オーディオ、ビデオなど) のサイズが大きくなると、転送のコストが重要になるため、エンコードせずにバイナリ データを転送するほうが適切です。 エンコードせずに転送する場合、PSWS では名前付きリソース ストリームを使用します。
  名前付きリソース ストリームは、 **Edm.Stream** 型のエンティティのプロパティです。 それぞれの名前付きリソース ストリームには、GET 操作用と UPDATE 操作用に別個の URI があります。
- OData アクションで、リソースに対して CRUD (作成、読み取り、更新、および削除) 以外のメソッドを呼び出すメカニズムが提供されるようになりました。 アクションを呼び出すには、そのアクションに対して定義された URI に HTTP POST 要求を送信します。 アクションに対するパラメーターは、POST 要求の本体で定義します。
- Windows Azure のガイドラインと一貫性を保つため、すべての URL を単純化する必要があります。 **Key As Segment** に組み込まれた変更により、1 つのキーでセグメントを表現できます。 なお、複数のキー値を使用する参照は、これまでと同様、かっこで囲んだコンマ区切り値で表記する必要があることに注意してください。
- PSWS の今回より前のリリースでは、作成、更新、または削除操作を実行する唯一の方法は、最上位レベルのリソースに対して Post、Put、または Delete を呼び出すことでした。 今回のリリースの PSWS では、Contained Resource 操作という新しい機能で同じ結果を得ることができます。これを使用すると、同じリソースにより間接的に到達し、それらのリソースが含まれているかのようにアクセスできます。

### <a name="new-features-in-windows-powershell-web-access"></a>Windows PowerShell Web Access の新機能

- Web ベースの Windows PowerShell Web Access コンソールで、既存のセッションを切断および再接続できます。 Web ベースのコンソールで **[保存]** ボタンを使用すると、削除せずにセッションを切断し、あとでセッションに再接続できます。
- サインイン ページに既定のパラメーターを表示できます。 既定のパラメーターを表示するには、サインイン ページの **[オプションの接続設定]** 領域に表示されるすべての設定に対して、 **web.config** という名前のファイルに値を構成します。 **web.config** ファイルを使用すると、2 次資格情報と代替資格情報を除くすべてのオプションの接続設定を構成できます。
- Windows Server 2012 R2 で、Windows PowerShell Web Access の承認規則をリモートで管理できます。 **Add-PswaAuthorizationRule** および **Test-PswaAuthorizationRule** コマンドレットに、管理者がリモート コンピューターから、または Windows PowerShell Web Access セッションで承認規則を管理できるようにするために Credential パラメーターが用意されました。
- 1 つのブラウザー セッションで各セッションに対して新しいブラウザー タブを使用することにより、複数の Windows PowerShell Web Access セッションに接続できるようになりました。 Web ベースの Windows PowerShell コンソールで新しいセッションに接続するとき、新しいブラウザー セッションを開く必要がなくなりました。

### <a name="notable-bug-fixes-in-windows-powershell-40"></a>Windows PowerShell 4.0 での主なバグ修正

- **Get-Counter** で、フランス語エディションの Windows で返されるカウンターにアポストロフィ文字を含められるようになりました。
- 逆シリアル化されたオブジェクトの **GetType** メソッドを表示できるようになりました。
- **#Requires** ステートメントで、必要な場合に、管理者アクセス権限をユーザーに要求するようになりました。
- **Import-Csv** コマンドレットで、ブランク行が無視されるようになりました。
- **Invoke-WebRequest** コマンドの実行中に Windows PowerShell ISE が使用するメモリが多すぎるという問題が修正されました。
- **Get-Module** で、 **Version** 列にモジュールのバージョンが表示されるようになりました。
- Remove-Item -Recurse で、想定どおりにサブフォルダーから項目が削除されるようになりました。
- **Get-Process** 出力オブジェクトに **UserName** プロパティが追加されました。
- **Invoke-RestMethod** コマンドレットがすべての使用可能な結果を返すようになりました。
- **Add-Member** が、ハッシュ テーブルがまだアクセスされていない場合でも、ハッシュ テーブルに効果を与えるようになりました。
- **Select-Object -Expand** が、プロパティの値が null または空の場合に、失敗したり、例外を生成したりしなくなりました。
- **Get-Process** が、オブジェクトから **ComputerName** プロパティを取得する他のコマンドと一緒にパイプラインで使用できるようになりました。
- **ConvertTo-Json** および **ConvertFrom-Json** が、二重引用符で囲まれた用語を受け付けるようになり、エラー メッセージがローカライズ可能になりました。
- **Get-Job** が、新しいセッションでも、スケジュールされたジョブのうちすべての完了したジョブを返すようになりました。
- Windows PowerShell 4.0 で **FileSystem** プロバイダーを使用した VHD のマウントとマウント解除に関する問題が修正されました。 Windows PowerShell で、同じセッション内でマウントされた新しいドライブを検出できるようになりました。
- **ScheduledJob** または **Workflow** モジュールのジョブ タイプを操作するとき、それらのモジュールを明示的に読み込む必要がなくなりました。
- 入れ子になったワークフローを定義するワークフローのインポート処理のパフォーマンスが改善され、高速に処理されるようになりました。

## <a name="new-features-in-windows-powershell-30"></a>Windows PowerShell 3.0 の新機能

Windows PowerShell 3.0 には、次に示す新機能があります。

- [Windows PowerShell ワークフロー](#windows-powershell-workflow)
- [Windows PowerShell Web Access](#windows-powershell-web-access)
- [Windows PowerShell ISE の新機能](#new-windows-powershell-ise-features)
- [Microsoft .NET Framework 4.0 のサポート](#support-for-microsoft-net-framework-4)
- [Windows プレインストール環境のサポート](#support-for-windows-preinstallation-environment)
- [切断されたセッション](#disconnected-sessions)
- [堅牢なセッション接続](#robust-session-connectivity)
- [更新可能なヘルプ システム](#updatable-help-system)
- [強化されたオンライン ヘルプ](#enhanced-online-help)
- [CIM の統合](#cim-integration)
- [セッション構成ファイル](#session-configuration-files)
- [スケジュールされたジョブ、およびタスク スケジューラとの統合](#scheduled-jobs-and-task-scheduler-integration)
- [Windows PowerShell 言語の機能強化](#windows-powershell-language-enhancements)
- [新しいコア コマンドレット](#new-core-cmdlets)
- [既存のコア コマンドレットとプロバイダーの機能強化](#improvements-to-existing-core-cmdlets-and-providers)
- [リモート モジュールのインポートと検出](#remote-module-import-and-discovery)
- [強化された Tab 補完機能](#enhanced-tab-completion)
- [モジュールの自動読み込み](#module-auto-loading)
- [モジュールのエクスペリエンスの改善](#module-experience-improvements)
- [コマンド検出の単純化](#simplified-command-discovery)
- [ログ記録、診断、およびグループ ポリシーのサポートの改善](#improved-logging-diagnostics-and-group-policy-support)
- [書式設定と出力の機能強化](#formatting-and-output-improvements)
- [コンソール ホストのエクスペリエンスの改善](#enhanced-console-host-experience)
- [新しい Cmdlet API および Hosting API](#new-cmdlet-and-hosting-apis)
- [パフォーマンスの向上](#performance-improvements)
- [RunAs および共有ホストのサポート](#runas-and-shared-host-support)
- [特殊文字の処理の改善](#special-character-handling-improvements)

### <a name="windows-powershell-workflow"></a>Windows PowerShell ワークフロー

Windows PowerShell ワークフローによって、Windows Workflow Foundation の機能を Windows PowerShell で利用できます。
ワークフローは、XAML または Windows PowerShell 言語で記述し、コマンドレットを実行するのとまったく同様に実行できます。 `Get-Command` コマンドレットを使用するとワークフロー コマンドを取得でき、`Get-Help` コマンドレットを使用するとワークフローのヘルプを取得できます。

ワークフローは、複数のコンピューターを管理するアクティビティのシーケンスです。これは、実行時間が長く、反復可能な、頻繁に実行できるシーケンスで、並列実行、割り込み、中断、および再起動が可能です。 ワークフローは、ネットワークの停止、Windows の再起動、電源障害などの意図的または偶発的な割り込みから再開できます。

また、ワークフローは移植可能で、XAML ファイルとしてエクスポートしたり、XAML ファイルからインポートしたりできます。 カスタム セッション構成を記述することにより、委任されたユーザーまたは下位ユーザーがワークフローまたはワークフロー内のアクティビティを実行可能にすることができます。

Windows PowerShell ワークフローの利点を次に示します。

- 実行時間の長いタスクのシーケンスを自動化します。
- **実行時間の長いタスクをリモート監視します** 。 アクティビティの状態と進行状況をいつでも表示できます。
- **複数コンピューターの管理。** 数百の管理対象ノードでタスクをワークフローとして同時に実行できます。
  Windows PowerShell ワークフローには、 **PSComputerName** などの共通管理パラメーターで構成される組み込みライブラリが含まれているため、複数のコンピューターを管理するシナリオに対応できます。
- **複雑なプロセスを 1 つのタスクで実行します。** エンド ツー エンドのシナリオ全体を実装する関連したスクリプトを 1 つのワークフローにまとめることができます。
- **永続化。** ワークフローは、作成者によって定義された特定の時点で保存 (またはチェックポイントを作成) できるため、ワークフローを最初から再開する代わりに、最後の永続化されたタスク (またはチェックポイント) からワークフローを再開できます。
- **信頼性。** エラー回復が自動化されます。 ワークフローは、計画的な、および計画外の再起動の後も残ります。 ワークフローの実行を中断した後、最後の永続化ポイントからワークフローを再開できます。
  ワークフローの作成者は、1 つまたは複数の管理対象ノードで障害が発生した時に再実行する特定のアクティビティを指定できます。
- **切断、再接続、切断されたセッションでの実行が可能。** ユーザーがワークフロー サーバーに接続したり切断したりしても、ワークフローは継続的に実行されます。 クライアント コンピューターからログオフするか、クライアント コンピューターを再始動しても、ワークフローに割り込むことなく別のコンピューターからワークフローの実行を監視できます。
- **スケジュール設定。** ワークフロー タスクは、Windows PowerShell のコマンドレットまたはスクリプトと同じようにスケジュール設定できます。
- **ワークフローと接続のスロットル調整。** ワークフローの実行とノードへの接続をスロットルで調整できるため、スケーラビリティと高可用性のシナリオに対応できます。

### <a name="windows-powershell-web-access"></a>Windows PowerShell Web Access

Windows PowerShell Web Access は、Windows PowerShell のコマンドとスクリプトをユーザーが Web ベースのコンソールで実行できる Windows Server 2012 の機能です。 Web ベースのコンソールを使用するデバイスでは、Windows PowerShell、リモート管理ソフトウェアまたはブラウザー プラグインをインストールする必要はありません。 必要なのは、適切に構成された Windows PowerShell Web Access ゲートウェイと、JavaScript をサポートし、Cookie を許可するクライアント デバイスのブラウザーのみです。

詳細については、「[ Windows PowerShell Web Access の展開](/previous-versions/powershell/scripting/components/web-access/install-and-use-windows-powershell-web-access)」を参照してください。

### <a name="new-windows-powershell-ise-features"></a>Windows PowerShell ISE の新機能

Windows PowerShell 3.0 では、Windows PowerShell Integrated Scripting Environment (ISE) に多くの新機能が追加されました。たとえば、IntelliSense、Show-Command ウィンドウ、統合されたコンソール ウィンドウ、スニペット、かっこの一致に基づくセクションの展開/折りたたみ、自動保存、最近使った項目の一覧、機能豊富なコピー、ブロックのコピー、および Windows PowerShell スクリプト ワークフローの記述を完全サポートする機能などです。 詳細については、「[about_Windows_PowerShell_ISE](/powershell/module/microsoft.powershell.core/about/about_windows_powershell_ise)」をご覧ください。

### <a name="support-for-microsoft-net-framework-4"></a>Microsoft .NET Framework 4 のサポート

Windows PowerShell は、共通言語ランタイム 4.0 を背景にして作成されました。 コマンドレット、スクリプト、およびワークフローの作成者は、新しい Microsoft .NET Framework 4 のクラスを Windows PowerShell で使用できます。たとえば、アプリケーションの互換性と配置、Managed Extensibility Framework、並列コンピューティング、ネットワーク接続、Windows Communication Foundation、および Windows Workflow Foundation があります。

### <a name="support-for-windows-preinstallation-environment"></a>Windows プレインストール環境のサポート

Windows PowerShell 3.0 は、Windows 8 用の Windows プレインストール環境 (Windows PE) 4.0 に含まれるオプションのコンポーネントです。 Windows PE は、オペレーティング システムのないコンピューターを起動して Windows のインストール用に準備する、最小限のオペレーティング システムです。 Windows PE を使用すると、ハード ドライブのパーティションの作成やフォーマット、ディスク イメージのコンピューターへのコピー、ネットワーク共有からの Windows セットアップの開始などを実行できます。
Windows PowerShell 3.0 は、展開、診断、および復旧のシナリオを管理するために Windows PE で使用できます。

### <a name="disconnected-sessions"></a>切断されたセッション

Windows PowerShell 3.0 以降では、New-PSSession コマンドレットを使用してユーザーが作成した、ユーザーが管理する永続的なセッション ("PSSessions") がリモート コンピューター上に保存されます。 それらのセッションは、作成元のセッションに依存しなくなりました。

セッションで実行されているコマンドを中断せずに、セッションから切断できるようになりました。 セッションを終了したり、コンピューターをシャットダウンしたりできます。 後で、同じまたは異なるコンピューター上の異なるセッションから、元のセッションに再接続できます。

`Get-PSSession` コマンドレットの **ComputerName** パラメーターで、コンピューターに接続されているすべてのユーザー セッション (異なるコンピューター上の異なるセッションで開始されたセッションを含む) を取得できるようになりました。 そのセッションへの接続、コマンドの結果の取得、新しいコマンドの開始、セッションの切断を実行できます。

"切断されたセッション" 機能をサポートする新しいコマンドレットが追加されました。たとえば、`Disconnect-PSSession`、`Connect-PSSession`、`Receive-PSSession` などです。また、PSSessions を管理するコマンドレットに新しいパラメーターが追加されました。たとえば、`Invoke-Command` コマンドレットの **InDisconnectedSession** パラメーターなどです。

"切断されたセッション" 機能がサポートされるのは、接続の両端、つまり、開始側 ("クライアント") と終了側 ("サーバー") にある両方のコンピューターで Windows PowerShell 3.0 が実行されている場合のみです。

### <a name="robust-session-connectivity"></a>堅牢なセッション接続

Windows PowerShell 3.0 は、クライアントとサーバー間の接続が予期せずに失われた状態を検出すると、自動的に接続を再接続し、実行を再開しようとします。 割り当てられた時間内にクライアントとサーバー間の接続を再確立できなかった場合は、ユーザーに通知され、セッションは切断されます。 再接続を試行している間、Windows PowerShell はユーザーに通知を継続的にフィードバックします。

切断されたセッションが InvokeCommand を使用して開始されたものである場合、Windows PowerShell は、容易に再接続して実行を再開できるようにするために、切断されたセッションのためのジョブを作成します。

これらの機能により、さらに信頼性が高く、再開可能なリモート処理エクスペリエンスが提供されるため、ワークフローなど、堅牢なセッションを必要とする長時間実行されるタスクを実行できます。

### <a name="updatable-help-system"></a>更新可能なヘルプ システム

モジュール内のコマンドレットに関する更新されたヘルプ ファイルをダウンロードできるようになりました。 `Update-Help` コマンドレットは、最新のヘルプ ファイルを識別し、それらのファイルをインターネットからダウンロードし、アンパックし、検証し、モジュール用の言語固有の該当するディレクトリにインストールします。

更新されたヘルプ ファイルは、`Get-Help` と入力するだけで使用できます。 Windows または Windows PowerShell を再起動する必要はありません。 $pshome ディレクトリのモジュールのヘルプを更新するには、[管理者として実行] オプションを使用して Windows PowerShell を起動します。

インターネットにアクセスできないユーザーやファイアウォールの内側にいるユーザーをサポートするには、新しいコマンドレット `Save-Help` を使用して、ファイル共有などのファイル システム ディレクトリにヘルプ ファイルをダウンロードします。 その後、ユーザーは `Update-Help` コマンドレットを使用して、更新されたヘルプ ファイルをファイル共有から入手できます。

`Update-Help` コマンドレットを使用して、サポートされるすべての UI カルチャの、すべてまたは特定のモジュールに対するヘルプ ファイルを更新できます。 Windows PowerShell プロファイルに `Update-Help` コマンドを置くこともできます。
既定では、Windows PowerShell は、モジュールに対するヘルプ ファイルを 1 日に 1 回を超えてダウンロードすることはありません。

Windows 8 および Windows Server 2012 モジュールにはヘルプ ファイルが含まれていません。 最新のヘルプ ファイルをダウンロードするには、`Update-Help` と入力します。 詳細については、`Get-Help` と入力する (パラメーターなし) か、「[about_Updatable_Help](/powershell/module/microsoft.powershell.core/about/about_Updatable_Help)」を参照してください。

コンピューターにコマンドレットのヘルプ ファイルがインストールされていない場合、`Get-Help` コマンドレットで自動生成されたヘルプが表示されるようになりました。 自動生成されたヘルプには、コマンドの構文と、`Update-Help` コマンドレットを使用してヘルプ ファイルをダウンロードする方法が記載されています。

モジュールの作成者は、自分のモジュールに対して更新可能なヘルプをサポートできます。 モジュールにヘルプ ファイルを組み込んでおき、更新可能なヘルプを使用してそれらのヘルプ ファイルを更新することができます。あるいは、ヘルプ ファイルを省略し、更新可能なヘルプを使用してヘルプ ファイルをインストールすることもできます。 更新可能なヘルプのサポートに関する詳細については、「[更新可能なヘルプのサポート](/powershell/scripting/developer/module/supporting-updatable-help)」をご覧ください。

### <a name="enhanced-online-help"></a>強化されたオンライン ヘルプ

Windows PowerShell のオンライン ヘルプはすべてのユーザーにとって価値のあるリソースですが、更新されたヘルプ ファイルをインストールしない、またはインストールできないユーザーにとっては特に重要です。

Windows PowerShell コマンドレットのオンライン ヘルプを取得するには、次のように入力します。

```
Get-Help <cmdlet-name> -Online
```

Windows PowerShell は、既定のインターネット ブラウザーでオンライン版のヘルプ トピックを開きます。

Windows PowerShell 3.0 の **Get-Help -Online** はさらに機能が強化され、コマンドレットのヘルプ ファイルがコンピューターにインストールされていない場合でも機能します。 **Get-Help -Online** 機能では、コマンドレットおよび高度な関数の HelpUri プロパティからオンライン ヘルプ トピックの URI を取得します。

```
PS C:\>(Get-Command Get-ScheduledJob).HelpUri
https://go.microsoft.com/fwlink/?LinkID=223923
```

Windows PowerShell 3.0 以降では、C# コマンドレットの作成者は、コマンドレット クラスで **HelpUri** 属性を作成することにより **HelpUri** プロパティに値を設定できます。 高度な関数の作成者は、 **CmdletBinding** 属性に **HelpUri** プロパティを定義できます。 **HelpUri** プロパティの値は、"http" または "https" で始まる必要があります。

さらに、 **HelpUri** の値は、コマンドレットの XML ベースのヘルプ ファイルにある最初の関連リンク内や、関数内のコメント ベースのヘルプにある .Link ディレクティブにも組み込めます。

オンライン ヘルプのサポートに関する詳細は、Microsoft Docs にある「[Supporting Online Help](/powershell/scripting/developer/module/supporting-online-help)」 (オンライン ヘルプのサポート) を参照してください。

### <a name="cim-integration"></a>CIM の統合

Windows PowerShell 3.0 には、Common Information Model (CIM) のサポートが組み込まれています。CIM は、システム、ネットワーク、アプリケーション、およびサービスの管理情報について共通の定義を提供するもので、異種システム間での管理情報の交換を可能にします。 Windows PowerShell 3.0 での CIM のサポートには、新規または既存の CIM クラスに基づいて Windows PowerShell コマンドレットを作成する機能、コマンドレットを定義する XML ファイルに基づいてコマンドを作成する機能のほか、CIM .NET Framework API、CIM 管理コマンドレット、および WMI 2.0 プロバイダーのサポートが含まれます。

### <a name="session-configuration-files"></a>セッション構成ファイル

Windows PowerShell 3.0 以降では、ファイルを使用してカスタム セッションの構成を設計できます。
新しいセッション構成ファイルでは、そのセッション構成を使用するセッションの環境を決定できます。たとえば、セッションに読み込むモジュール、スクリプト、およびフォーマット ファイル、ユーザーが使用できるコマンドレットおよび言語要素、ユーザーが実行できるモジュールおよびスクリプト、ユーザーが見ることのできる変数を決定します。

ユーザーが特定の 1 つのモジュールからコマンドレットを実行できるだけのセッションを設計できます。あるいは、ユーザーが完全な言語を持ち、すべてのモジュールにアクセスでき、高度なタスクを実行するスクリプトにアクセスできるセッションを設計できます。

以前のバージョンの Windows PowerShell では、このレベルの制御は、C# プログラムや複雑なスタートアップ スクリプトを作成できる人のみが使用できました。 現在は、コンピューターの Administrators グループのすべてのメンバーが、構成ファイルを使用してセッションの構成をカスタマイズできます。

セッション構成ファイルを作成するには、`New-PSSessionConfigurationFile` コマンドレットを使用します。 セッション構成ファイルをセッション構成に適用するには、`Register-PSSessionConfiguration` または `Set-PSSessionConfiguration コマンドレットを使用します。

詳細については、「[about_Session_Configuration_Files](/powershell/module/microsoft.powershell.core/about/about_session_configuration_files)」および `New-PSSessionConfigurationFile` を参照してください。

### <a name="scheduled-jobs-and-task-scheduler-integration"></a>スケジュールされたジョブ、およびタスク スケジューラとの統合

Windows PowerShell のバックグラウンド ジョブは、Windows PowerShell およびタスク スケジューラでスケジュールを設定し、管理できるようになりました。

Windows PowerShell のスケジュール設定されたジョブは、Windows PowerShell のバックグラウンド ジョブとタスク スケジューラのタスクを統合した便利なハイブリッドです。

Windows PowerShell のバックグラウンド ジョブと同様、スケジュールされたジョブはバックグラウンドで非同期に実行されます。
スケジュールされたジョブが完了したインスタンスは、`Start-Job` や `Get-Job` などのジョブ コマンドレットを使用して管理できます。

タスク スケジューラのタスクと同様、スケジュールされたジョブは 1 回または繰り返し実行したり、特定のアクションまたはイベントへの応答として実行したりできます。 スケジュールされたジョブはタスク スケジューラで表示および管理できます。必要に応じて有効または無効にする、実行する、テンプレートとして使用する、ジョブが開始される条件を設定するなどの操作が可能です。

さらに、スケジュールされたジョブには、それらのジョブを管理するためのカスタマイズされた一連のコマンドレットがあります。 それらのコマンドレットでは、スケジュールされたジョブの作成、編集、管理、無効化、再有効化を実行したり、スケジュールされたジョブのトリガーを作成したり、スケジュールされたジョブのオプションを設定したりできます。

スケジュールされたジョブの詳細については、「[about_Scheduled_Jobs](/powershell/module/psscheduledjob/about/about_scheduled_jobs)」を参照してください。

### <a name="windows-powershell-language-enhancements"></a>Windows PowerShell 言語の機能強化

Windows PowerShell 3.0 には、言語を単純化し、使いやすくし、一般的なエラーを回避するために設計された多くの機能が含まれます。 強化された機能としては、スカラー オブジェクトに対するプロパティの列挙、カウントや長さのプロパティ、新しいリダイレクト演算子、$Using スコープ修飾子、PSItem 自動変数、柔軟性の高いスクリプトの書式設定、変数の属性、簡略化された属性の引数、数値のコマンド名、Stop-Parsing 演算子、強化された配列スプラッティング、新しいビット演算子、順序付けられた辞書、PSCustomObject のキャスト、および強化されたコメント ベースのヘルプがあります。

### <a name="new-core-cmdlets"></a>新しいコア コマンドレット

新しいコマンドレットが Windows PowerShell コアのインストールに追加されました。スケジュールされたジョブ、切断されたセッション、CIM 統合、更新可能なヘルプ システムを管理するコマンドレットです。

- CimCmdlets
  - Get-CimAssociatedInstance
  - Get-CimClass
  - Get-CimInstance
  - Get-CimSession
  - Invoke-CimMethod
  - New-CimInstance
  - New-CimSession
  - New-CimSessionOption
  - Register-CimIndicationEvent
  - Remove-CimInstance
  - Remove-CimSession
  - Set-CimInstance
- Microsoft.PowerShell.Core
  - Connect-PSSession
  - Disconnect-PSSession
  - New-PSSessionConfigurationFile
  - New-PSSessionOption
  - Receive-PSSession
  - Resume-Job
  - Save-Help
  - Suspend-Job
  - Test-PSSessionConfigurationFile
  - Update-Help
- Microsoft.PowerShell.Diagnostics
  - New-WinEvent
- Microsoft.PowerShell.Management
  - Get-ControlPanelItem
  - Rename-Computer
  - Show-ControlPanelItem
- Microsoft.PowerShell.Utility
  - ConvertFrom-Json
  - ConvertTo-Json
  - Get-TypeData
  - Invoke-RestMethod
  - Invoke-WebRequest
  - Remove-TypeData
  - Show-Command
  - Unblock-File
- PSScheduledJob
  - Add-JobTrigger
  - Disable-JobTrigger
  - Disable-ScheduledJob
  - Enable-JobTrigger
  - Enable-ScheduledJob
  - Get-JobTrigger
  - Get-ScheduledJob
  - Get-ScheduledJobOption
  - New-JobTrigger
  - New-ScheduledJobOption
  - Register-ScheduledJob
  - Set-JobTrigger
  - Set-ScheduledJob
  - Set-ScheduledJobOption
  - Unregister-ScheduledJob
- PSWorkflow
  - New-PSWorkflowExecutionOption
  - New-PSWorkflowSession
- PSWorkflowUtility
  - Invoke-AsWorkflow
- ISE
  - Get-IseSnippet
  - Import-IseSnippet
  - New-IseSnippet

### <a name="improvements-to-existing-core-cmdlets-and-providers"></a>既存のコア コマンドレットとプロバイダーの機能強化

Windows PowerShell 3.0 では、既存のコマンドレットに新しい機能が追加されたもの、構文が単純化されたもの、新しいパラメーターが追加されたものがあります。次のコマンドレットです。Computer コマンドレット、CSV コマンドレット、Get-ChildItem、Get-Command、Get-Content、Get-History、Measure-Object、Security コマンドレット、Select-Object、Select-String、Split-Path、Start-Process、Tee-Object、Test-Connection、Add-Member、および WMI コマンドレット。

また、Windows PowerShell プロバイダーも大幅に機能強化され、Web ホスト用の Secure Socket Layer (SSL) 証明書を管理するための証明書プロバイダーのサポート、資格情報のサポート、およびファイル システム ドライブにおける永続化されたネットワーク ドライブと代替データ ストリームのサポートが改善されました。

### <a name="remote-module-import-and-discovery"></a>リモート モジュールのインポートと検出

Windows PowerShell 3.0 では、リモート コンピューター上でのモジュールの検出、インポート、および暗黙的なリモート処理の機能が拡張されました。 Module コマンドレットは、Windows PowerShell のリモート処理を使用してリモート コンピューター上のモジュールを取得し、モジュールをリモートまたはローカル コンピューターにインポートします。 新しい CIM セッションのサポートでは、Windows 以外のコンピューターを管理するために CIM および WMI を使用して、リモート コンピューター上で暗黙的に実行されるコマンドをローカル コンピューターにインポートできます。

詳細については、`Get-Module` および `Import-Module` コマンドレットのヘルプ トピックを参照してください。

### <a name="enhanced-tab-completion"></a>強化された Tab 補完機能

Windows PowerShell コンソールの Tab 補完機能では、コマンドレット、パラメーター、パラメーター値、列挙、.NET Framework 型、COM オブジェクト、隠しディレクトリなどの名前が補完されるようになりました。
Tab 補完機能は、新しいパーサーと抽象構文ツリーに基づいて完全に再作成され、インメモリ解析ツリーや行の途中での Tab 補完を含むより多くのシナリオをサポートします。

### <a name="module-auto-loading"></a>モジュールの自動読み込み

`Get-Command` コマンドレットにより、コンピューターにインストールされているすべてのモジュール (現在のセッションにインポートされないモジュールを含む) のすべてのコマンドレットと関数を取得できるようになりました。

必要なコマンドレットを取得したら、何かのモジュールをインポートすることなく、すぐに使用できます。
Windows PowerShell のモジュールは、モジュール内のいずれかのコマンドレットを使用した時点で自動的にインポートされるようになりました。 モジュールを検索したり、それに含まれるコマンドレットを使用するためにインポートしたりする必要がありません。

モジュールの自動インポートがトリガーされるのは、コマンド内でコマンドレットを使用した場合、コマンドレットの `Get-Command` をワイルドカードなしで実行した場合、またはコマンドレットの `Get-Help` をワイルドカードなしで実行した場合です。

モジュールの自動インポートの有効/無効の切り替えや構成を行うには、 **$PSModuleAutoLoadingPreference** ユーザー設定変数を使用します。

詳細については、「[about_Modules ](/powershell/module/microsoft.powershell.core/about/about_modules)」、「[about_Preference_Variables](/powershell/module/microsoft.powershell.core/about/about_Preference_Variables)」、および `Get-Command` と `Import-Module` コマンドレットのヘルプ トピックを参照してください。

### <a name="module-experience-improvements"></a>モジュールのエクスペリエンスの改善

Windows PowerShell 3.0 は、高度な機能のサポートをモジュールに提供する次のような新機能があります。

1. 個々のモジュールを対象にしたモジュール ログ (LogPipelineExecutionDetails) と、新しい "モジュール ログをオンにする" グループ ポリシー設定。
2. モジュール マニフェストから値を公開する拡張されたモジュール オブジェクト。
3. モジュール (入れ子になったモジュールを含む) の新しい **ExportedCommands** プロパティ。これは、すべての種類のコマンドを結合するプロパティです。
4. 利用可能な (まだインポートされていない) モジュール検出の機能強化。たとえば、同じコマンド内で **Path** および **ListAvailable** パラメーターを使用できます。
5. モジュールのコードを変更せずに名前の競合を回避する、モジュール マニフェストの新しい **DefaultCommandPrefix** キー。
6. モジュールの要件の改善。必要なモジュールをバージョンと GUID によって完全修飾して指定する機能や、必要なモジュールの自動インポートなどがあります。
7. `New-ModuleManifest` コマンドレットの操作が効率化され、実行時の対話が減りました。
8. &#35;Requires の新しい **Module** パラメーター。
9. `Import-Module` コマンドレットが改善され、 **MinimumVersion** と **RequiredVersion** の両方のパラメーターが追加されました。

### <a name="simplified-command-discovery"></a>コマンド検出の単純化

すべてのモジュールをインポートしないでも、現在のセッションで使用可能なコマンドを検出できるようになりました。 Windows PowerShell 3.0 では、`Get-Command` コマンドレットを使用すると、インストールされているすべてのモジュールからすべてのコマンドを取得できます。 また、あるコマンドを使用すると。そのコマンドをエクスポートするモジュールがセッションに自動的にインポートされます。

新しい `Show-Command` コマンドレットは、特に初心者向けに設計されています。 ウィンドウでコマンドを検索できます。 すべてのコマンドを表示するかモジュールによってフィルター処理し、ボタンをクリックしてモジュールをインポートし、テキスト ボックスやドロップダウン リストを使用して有効なコマンドを作成した後、そのコマンドをコピーしたり、このウィンドウを開いたままでコマンドを実行したりできます。

### <a name="improved-logging-diagnostics-and-group-policy-support"></a>ログ記録、診断、およびグループ ポリシーのサポートの改善

Windows PowerShell 3.0 において、コマンドとモジュールのログ記録とトレースのサポートが機能強化されました。Windows イベント トレーシング (ETW) ログのサポートが追加され、モジュールの **LogPipelineExecutionDetails** プロパティが編集可能になり、グループ ポリシー設定に "モジュール ログを有効にする" が追加されました。 ログ プロパティを表示すれば、ログの詳細からパラメーター値を取得できるようになりました。

### <a name="formatting-and-output-improvements"></a>書式設定と出力の機能強化

書式設定と出力の新たな機能向上により、すべての Windows PowerShell ユーザーの効率が改善されました。 向上した機能には、すべてのストリームの出力リダイレクト、Format.ps1xml ファイルなしで動的に型を追加できる拡張された Update-Type コマンドレット、出力の右端での折り返し、カスタム オブジェクトの既定の書式設定プロパティ、 **PSCustomObject** 型、WMI オブジェクトと異種オブジェクトの書式設定の改善、メソッドのオーバーロードを検出する機能のサポートなどがあります。

### <a name="enhanced-console-host-experience"></a>コンソール ホストのエクスペリエンスの改善

Windows PowerShell コンソール ホスト プログラムは Windows PowerShell 3.0 で新機能が追加され、既定でシングル スレッド アパートメントがサポートされます。 エクスプローラーの新しい [PowerShell で実行] オプションを使用すると、右クリックするだけで、制限なしのセッションでスクリプトを実行できます。 コンソール ホストの起動ロジックが新しくなり、Windows PowerShell を高速で起動できるようになりました。また、新しいフォントを使用すると、使い慣れたコンソールのウィンドウ エクペリエンスにカスタマイズできます。

### <a name="new-cmdlet-and-hosting-apis"></a>新しい Cmdlet API および Hosting API

新しい Cmdlet API および Hosting API には、公開 Advanced Syntax Tree (AST) API、パイプライン ページング、入れ子になったパイプライン、実行空間プールの Tab 補完、Windows RT のための各 API、Obsolete コマンドレット属性、FunctionInfo オブジェクトの Verb および Noun プロパティが含まれています。

### <a name="performance-improvements"></a>パフォーマンスの強化

Windows PowerShell での大幅なパフォーマンスの向上は、.NET Framework 4. の動的言語ランタイム (DLR) を基にして構築された新しい言語パーサーの成果です。さらに、ランタイム スクリプトのコンパイルの向上、エンジンの信頼性の向上、および `Get-ChildItem` のアルゴリズム変更によるパフォーマンスの向上 (特にネットワーク共有の検索時) なども貢献しています。

### <a name="runas-and-shared-host-support"></a>RunAs および共有ホストのサポート

Windows PowerShell 3.0 には、RunAs 機能と共有ホスト機能のサポートが組み込まれました。

*RunAs* 機能は、Windows PowerShell のワークフロー用に設計されたもので、これによりセッション構成のユーザーが共有ユーザー アカウントのアクセス許可で実行するセッションを作成できます。 管理者のアクセス許可が必要な特定のコマンドおよびスクリプトを権限の少ないユーザーが実行できるようになるため、スキルの限られたユーザーを Administrators グループに追加する必要性を削減できます。

**SharedHost** 機能を使用すると、複数のコンピューター上の複数のユーザーがワークフロー セッションに同時に接続して、ワークフローの進行状況をモニターできます。 ユーザーは、1 つのコンピューター上でワークフローを開始した後、別のコンピューター上でそのワークフロー セッションに接続することができ、その際に元のコンピューターからのセッションを切断する必要がありません。 ユーザーは、同じアクセス許可を持ち、同じセッション構成を使用している必要があります。 詳細については、「Windows PowerShell ワークフローについて」の「Windows PowerShell ワークフローの実行」を参照してください。

### <a name="special-character-handling-improvements"></a>特殊文字の処理の改善

Windows PowerShell 3.0 では、特殊文字を正しく解釈して処理する機能が改善されました。パスに含まれる特殊文字を処理する **LiteralPath** パラメーターが、 **Path** パラメーターを持つほとんどすべてのコマンドレットで有効になりました。これには新しい `Update-Help` および `Save-Help` コマンドレットも含まれます。 また、ファイル名やパスに含まれるアクサングラーブ文字 (\`) と角かっこの処理を改善する特別なロジックがパーサーに組み込まれました。

## <a name="see-also"></a>参照

- [about_Windows_PowerShell_5.0](/previous-versions/powershell/module/microsoft.powershell.core/about/about_windows_powershell_5.0)
- [Windows PowerShell](/powershell/)
