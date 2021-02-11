---
title: Windows PowerShell 5.1 から PowerShell 7 への移行
description: Windows プラットフォームで PowerShell 5.1 から PowerShell 7 に更新します。
ms.date: 03/25/2020
ms.openlocfilehash: cb14a4f159b6dc33f31386da4264c0ebb640aef8
ms.sourcegitcommit: 077488408c820c860131382324bdd576d0edf52a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/26/2020
ms.locfileid: "83809208"
---
# <a name="migrating-from-windows-powershell-51-to-powershell-7"></a>Windows PowerShell 5.1 から PowerShell 7 への移行

クラウド、オンプレミス、およびハイブリッド環境向けに設計された PowerShell 7 には、強化された機能と[新機能](../whats-new/What-s-New-in-PowerShell-70.md)が組み込まれています。

- Windows PowerShell とのサイド バイ サイドのインストールおよび実行
- 既存の Windows PowerShell モジュールとの強化された互換性
- 三項演算子や `ForEach-Object -Parallel` などの新しい言語機能
- パフォーマンスの向上
- SSH ベースのリモート処理
- クロス プラットフォームの相互運用性
- Docker コンテナーのサポート

PowerShell 7 は Windows PowerShell とサイド バイ サイドで動作するため、簡単にテストを行い、展開前にエディション間で比較することができます。 簡単に、すばやく、安全に移行できます。

PowerShell 7 は、次の Windows オペレーティング システムでサポートされています。

- Windows 8.1 および 10
- Windows Server 2012、2012 R2、2016、2019

PowerShell 7 は、macOS と、いくつかの Linux ディストリビューションでも実行できます。 サポートされているオペレーティング システムの一覧と、サポート ライフサイクルの詳細については、「[PowerShell のサポート ライフサイクル](/powershell/scripting/powershell-support-lifecycle)」をご覧ください。

## <a name="installing-powershell-7"></a>PowerShell 7 のインストール

柔軟性を確保し、IT、DevOps エンジニア、および開発者のニーズに対応するために、PowerShell 7 のインストールにはいくつかのオプションが用意されています。 ほとんどの場合、インストール オプションは次の方法に絞ることができます。

- [MSI パッケージ](/powershell/scripting/install/installing-powershell-core-on-windows#installing-the-msi-package)を使用して PowerShell を展開する
- [ZIP パッケージ](/powershell/scripting/install/installing-powershell-core-on-windows#installing-the-zip-package)を使用して PowerShell を展開する

> [!NOTE]
> MSI パッケージは、[System Center Configuration Manager (SCCM)](/configmgr/apps/) などの管理製品を使用して展開および更新できます。 [GitHub リリース ページ](https://github.com/PowerShell/PowerShell/releases)からパッケージをダウンロードしてください。

MSI パッケージを展開するには、管理者権限が必要です。 ZIP パッケージは、任意のユーザーが展開できます。 ZIP パッケージは、完全インストールを実行する前にテスト用の PowerShell 7 をインストールするための最も簡単な方法です。

## <a name="using-powershell-7-side-by-side-with-windows-powershell-51"></a>PowerShell 7 を Windows PowerShell 5.1 とサイド バイ サイドで使用する

PowerShell 7 は、Windows PowerShell 5.1 と共存するように設計されています。 次の機能により、ユーザーの PowerShell への投資が確実に保護され、PowerShell 7 への移行が簡単になります。

- 個別のインストール パスと実行可能ファイル名
- 個別の PSModulePath
- バージョンごとに個別のプロファイル
- 強化されたモジュールの互換性
- 新しいリモート処理エンドポイント
- グループ ポリシーのサポート
- 個別のイベント ログ

### <a name="separate-installation-path-and-executable-name"></a>個別のインストール パスと実行可能ファイル名

PowerShell 7 は新しいディレクトリにインストールされ、Windows PowerShell 5.1 とのサイド バイ サイド実行が可能になります。

バージョン別のインストール場所:

- Windows PowerShell 5.1: `$env:WINDIR\System32\WindowsPowerShell\v1.0`
- PowerShell Core 6.x: `$env:ProgramFiles\PowerShell\6`
- PowerShell 7: `$env:ProgramFiles\PowerShell\7`

新しい場所が PATH に追加され、Windows PowerShell 5.1 と PowerShell 7 の両方を実行できるようになります。 Powershell Core 6.x から PowerShell 7 に移行する場合は、PowerShell 6 が削除され、PATH が置き換えられます。

Windows PowerShell では、PowerShell の実行可能ファイルには `powershell.exe` という名前が付けられます。 バージョン 6 以降では、実行可能ファイルには `pwsh.exe` という名前が付けられます。 新しい名前を使用すると、両方のバージョンのサイド バイ サイド実行をサポートしやすくなります。

### <a name="separate-psmodulepath"></a>個別の PSModulePath

既定では、Windows PowerShell と PowerShell 7 では異なる場所にモジュールが格納されます。 PowerShell 7 では、これらの場所を組み合わせたものが `$Env:PSModulePath` 環境変数に設定されます。 モジュールを名前でインポートする場合、PowerShell では `$Env:PSModulePath` で指定された場所がチェックされます。 これにより、PowerShell 7 では Core モジュールと Desktop モジュールの両方を読み込むことができます。

|            インストール スコープ            |                Windows PowerShell 5.1                 |             PowerShell 7.0             |
| ----------------------------------- | ----------------------------------------------------- | -------------------------------------- |
| PowerShell モジュール                  | `$env:WINDIR\system32\WindowsPowerShell\v1.0\Modules` | `$PSHOME\Modules`                      |
| ユーザー インストール<br>AllUsers スコープ    | `$env:ProgramFiles\WindowsPowerShell\Modules`         | `$env:ProgramFiles\PowerShell\Modules` |
| ユーザー インストール<br>CurrentUser スコープ | `$HOME\Documents\WindowsPowerShell\Modules`           | `$HOME\Documents\PowerShell\Modules`   |

次の例では、各バージョンの `$Env:PSModulePath` の既定値を示します。

- Windows PowerShell 5.1 の場合:

  ```powershell
  $Env:PSModulePath -split (';')
  ```

  ```Output
  C:\Users\<user>\Documents\WindowsPowerShell\Modules
  C:\Program Files\WindowsPowerShell\Modules
  C:\WINDOWS\System32\WindowsPowerShell\v1.0\Modules
  ```

- PowerShell 7 の場合:

  ```powershell
  $Env:PSModulePath -split (';')
  ```

  ```Output
  C:\Users\<user>\Documents\PowerShell\Modules
  C:\Program Files\PowerShell\Modules
  C:\Program Files\PowerShell\7\Modules
  C:\Program Files\WindowsPowerShell\Modules
  C:\WINDOWS\System32\WindowsPowerShell\v1.0\Modules
  ```

PowerShell 7 では、モジュールの自動読み込みを行うために、Windows PowerShell のパスと PowerShell 7 のパスが含まれていることがわかります。

> [!NOTE]
> PSModulePath 環境変数を変更した場合、またはカスタムのモジュールかアプリケーションをインストールした場合は、追加のパスが存在する可能性があります。

詳細については、[環境変数の概要](/powershell/module/microsoft.powershell.core/about/about_environment_variables#environment-variables-that-store-preferences)に関する記事の `PSModulePath` を参照してください。

モジュールの詳細については、「[about_Modules](/powershell/module/Microsoft.PowerShell.Core/About/about_Modules)」をご覧ください。

### <a name="separate-profiles"></a>個別のプロファイル

PowerShell プロファイルは、PowerShell の起動時に実行されるスクリプトです。 このスクリプトでは、コマンド、エイリアス、関数、変数、モジュール、PowerShell ドライブを追加して、環境をカスタマイズできます。 プロファイル スクリプトを使用すると、すべてのセッションでこれらのカスタマイズを利用できるようになり、手動で再作成する必要がなくなります。

PowerShell 7 では、プロファイルの場所へのパスが変更されています。

- Windows PowerShell 5.1 では、プロファイルの場所は `$HOME\Documents\WindowsPowerShell` です。
- PowerShell 7 では、プロファイルの場所は `$HOME\Documents\PowerShell` です。

プロファイルのファイル名も変更されています。

   ```powershell
   PS> $PROFILE | Select-Object *Host* | Format-List

   AllUsersAllHosts       : C:\Program Files\PowerShell\7\profile.ps1
   AllUsersCurrentHost    : C:\Program Files\PowerShell\7\Microsoft.PowerShell_profile.ps1
   CurrentUserAllHosts    : C:\Users\<user>\Documents\PowerShell\profile.ps1
   CurrentUserCurrentHost : C:\Users\<user>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
   ```

詳細については、「[about_Profiles](/powershell/module/microsoft.powershell.core/about/about_profiles)」をご覧ください。

### <a name="powershell-7-compatibility-with-windows-powershell-51-modules"></a>PowerShell 7 の Windows PowerShell 5.1 モジュールとの互換性

Windows PowerShell 5.1 でお使いのほとんどのモジュールは、Azure PowerShell と Active Directory を含めて、既に PowerShell 7 で動作しています。 私たちは、他のチームと協力して、Microsoft Graph、Office 365 などを始めとする、さらに多くのモジュールの PowerShell 7 ネイティブ サポートを追加する作業を続けています。 サポートされているモジュールの最新の一覧については、「[PowerShell 7 モジュールの互換性](/powershell/scripting/whats-new/module-compatibility)」をご覧ください。

> [!NOTE]
> Windows では、`Import-Module` に **UseWindowsPowerShell** スイッチも追加されており、互換性のないモジュールを使用するユーザーが簡単に PowerShell 7 に切り替えられます。 この機能の詳細については、「[about_Windows_PowerShell_Compatibility](/powershell/module/Microsoft.PowerShell.Core/About/about_windows_powershell_compatibility)」をご覧ください。

### <a name="powershell-remoting"></a>PowerShell リモート処理

PowerShell リモート処理を使用すると、1 台以上のリモート コンピューター上で任意の PowerShell コマンドを実行できます。 永続的な接続の確立、対話型のセッションの開始、およびリモート コンピューターでのスクリプトの実行が可能です。

#### <a name="ws-management-remoting"></a>WS-Management リモート処理

Windows PowerShell 5.1 以前では、接続ネゴシエーションとデータ転送のために WS-Management (WSMAN) プロトコルが使用されます。 Windows リモート管理 (WinRM) では、WSMAN プロトコルが使用されます。 WinRM が有効になっている場合、PowerShell 7 では、リモート接続のために `Microsoft.PowerShell` という名前の既存の Windows PowerShell 5.1 エンドポイントが使用されます。 PowerShell 7 を更新して独自のエンドポイントを追加するには、`Enable-PSRemoting` コマンドレットを実行します。 特定のエンドポイントへの接続について詳しくは、[PowerShell Core の WS-Management リモート処理](/powershell/scripting/learn/remoting/wsman-remoting-in-powershell-core)に関する記事をご覧ください

Windows PowerShell のリモート処理を使用するには、リモート管理のためにリモート コンピューターを構成する必要があります。
手順を含む詳細については、「[about_Remote_Requirements](/powershell/module/microsoft.powershell.core/about/about_remote_requirements)」をご覧ください。

リモート処理の使用について詳しくは、「[リモートについて](/powershell/module/microsoft.powershell.core/about/about_remote)」をご覧ください。

#### <a name="ssh-based-remoting"></a>SSH ベースのリモート処理

SSH ベースのリモート処理は、**WinRM** のような Windows ネイティブ コンポーネントを使用できない他のオペレーティング システムをサポートするために、PowerShell Core 6.x に追加されました。 SSH リモート処理で、SSH サブシステムとしてターゲット コンピューター上に PowerShell ホスティング プロセスを作成します。 Windows または Linux で SSH ベースのリモート処理を設定する方法の詳細と例については、次を参照してください:「[SSH 経由の PowerShell リモート処理](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)」。

> [!NOTE]
> PowerShell ギャラリー (PSGallery) には、SSH ベースのリモート処理を自動的に構成するモジュールとコマンドレットが含まれています。 [PSGallery](https://www.powershellgallery.com/packages/Microsoft.PowerShell.RemotingTools/0.1.0) から `Microsoft.PowerShell.RemotingTools` モジュールをインストールし、`Enable-SSH` コマンドレットを実行してください。

`New-PSSession`、`Enter-PSSession`、および `Invoke-Command` コマンドレットには、SSH 接続をサポートする新しいパラメーター セットがあります。

```powershell
[-HostName <string>]  [-UserName <string>]  [-KeyFilePath <string>]
```

リモート セッションを作成するには、**HostName** パラメーターでターゲット コンピューターを指定し、**UserName** でユーザー名を指定します。 コマンドレットを対話的に実行する場合は、パスワードの入力を求められます。

```powershell
Enter-PSSession -HostName <Computer> -UserName <Username>
```

または、**HostName** パラメーターを使用する場合は、ユーザー名の情報に続けてアットマーク (`@`) を入力し、続けてコンピューター名を指定します。

```powershell
Enter-PSSession -HostName <Username>@<Computer>
```

**KeyFilePath** パラメーターを指定して、プライベート キー ファイルを使用する SSH キー認証を設定できます。
詳細については、「[OpenSSH キーの管理](/windows-server/administration/openssh/openssh_keymanagement)」をご覧ください。

### <a name="group-policy-supported"></a>グループ ポリシーのサポート

PowerShell には、エンタープライズ環境内の各サーバーに対して一貫したオプション値を定義するのに役立つ、グループ ポリシー設定が含まれています。 これらの設定には、以下が含まれます。

- コンソール セッションの構成:PowerShell を実行する構成エンドポイントを設定します。
- モジュール ログを有効にする:モジュールの LogPipelineExecutionDetails プロパティを設定します。
- PowerShell スクリプト ブロックのログ記録を有効にする:すべての PowerShell スクリプトの詳細なログ記録を有効にします。
- スクリプトの実行を有効にする:PowerShell 実行ポリシーを設定します。
- PowerShell トランスクリプションを有効にする: PowerShell コマンドの入出力をテキスト ベースのトランスクリプトにキャプチャできるようにします。
- Update-Help の既定のソース パスを設定する:更新可能なヘルプのソースを、インターネットではなく、ディレクトリに設定します。

詳細については、「[about_Group_Policy_Settings](/powershell/module/microsoft.powershell.core/about/about_group_policy_settings)」をご覧ください。

PowerShell 7 では、グループ ポリシーのテンプレートとインストール スクリプトが `$PSHOME` に含まれています。

グループ ポリシー ツールでは、管理用テンプレート ファイル (`.admx`、`.adml`) を使用して、ユーザー インターフェイスにポリシー設定を追加できます。 これにより、管理者はレジストリ ベースのポリシー設定を管理できます。 `InstallPSCorePolicyDefinitions.ps1` スクリプトを使用すると、ローカル コンピューターに PowerShell Core 管理用テンプレートがインストールされます。

```powershell
Get-ChildItem -Path $PSHOME -Filter *Core*Policy*
```

```Output
    Directory: C:\Program Files\PowerShell\7

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           2/27/2020 12:38 AM          15861 InstallPSCorePolicyDefinitions.ps1
-a---           2/27/2020 12:28 AM           9675 PowerShellCoreExecutionPolicy.adml
-a---           2/27/2020 12:28 AM           6201 PowerShellCoreExecutionPolicy.admx
```

### <a name="separate-event-logs"></a>個別のイベント ログ

Windows PowerShell と PowerShell 7 では、個別のイベント ログにイベントのログが記録されます。 PowerShell ログの一覧を取得するには、次のコマンドを使用します。

```powershell
Get-WinEvent -ListLog *PowerShell*
```

詳細については、「[about_Logging_Windows](/powershell/module/microsoft.powershell.core/about/about_logging_windows)」をご覧ください。

## <a name="improved-editing-experience-with-visual-studio-code"></a>Visual Studio Code を使用した編集エクスペリエンスの向上

[PowerShell 拡張機能](https://code.visualstudio.com/docs/languages/powershell)を使用した [Visual Studio Code (VSCode)](https://code.visualstudio.com/) は、PowerShell 7 でサポートされているスクリプト環境です。 Windows PowerShell Integrated Scripting Environment (ISE) では、Windows PowerShell のみがサポートされています。

更新された PowerShell 拡張機能には、次が含まれます。

- 新しい ISE 互換モード
- 統合コンソールでの PSReadLine (構文の強調表示、複数行の編集、バック検索を含む)
- 安定性とパフォーマンスの向上
- 新しい CodeLens 統合
- パスのオートコンプリートの向上

簡単に Visual Studio Code への移行を行うには、**コマンド パレット** で使用できる **ISE モードの有効化** 機能を使用します。 この機能を使うと、VSCode が ISE スタイルのレイアウトに切り替わります。 ISE スタイルのレイアウトでは、使い慣れたユーザー エクスペリエンスで、PowerShell のすべての新機能を使用できます。

新しい ISE レイアウトに切り替えるには、<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> キーを押して **コマンド パレット** を開き、「`PowerShell`」と入力して次を選択します: **[PowerShell:Enable ISE Mode]\(PowerShell: ISE モードの有効化\)**。

レイアウトを元のレイアウトに設定するには、**コマンド パレット** を開き、次を選択します: **[PowerShell:Disable ISE Mode (restore to defaults)]\(PowerShell: ISE モードの無効化 (既定値に戻す)\)**。

VSCode のレイアウトを ISE にカスタマイズする方法の詳細については、「[Visual Studio Code で ISE のエクスペリエンスをレプリケートする方法](/powershell/scripting/components/vscode/how-to-replicate-the-ise-experience-in-vscode)」をご覧ください

> [!NOTE]
> 新しい機能で ISE を更新する予定はありません。 最新バージョンの Windows 10 および Windows Server では、ISE はユーザーがアンインストール可能な機能になりました。 ISE を完全に削除する予定はありません。 PowerShell チームとそのパートナーは、Visual Studio Code 用の PowerShell 拡張機能におけるスクリプト作成エクスペリエンスの改善に重点を置いています。

## <a name="next-steps"></a>次の手順

効率的に移行を行うための知識が身に付いたら、すぐに [PowerShell 7 をインストール](/powershell/scripting/install/installing-powershell-core-on-windows)しましょう。
