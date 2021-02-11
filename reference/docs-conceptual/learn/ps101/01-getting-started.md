---
title: PowerShell の概要
description: 新規ユーザー向けの PowerShell の場所と起動方法。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: 8b9fee222347970df4e35f9ba0841232952a292d
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99605160"
---
# <a name="chapter-1---getting-started-with-powershell"></a>第 1 章 - PowerShell の概要

カンファレンスやユーザー グループ ミーティングのプレゼンターがエントリレベルのプレゼンテーションを開始したときに既に PowerShell を実行していることに気付かされることはよくあります。 このドキュメントでは、そういったセッションでこれまで PowerShell を使用したことのない参加者から寄せられた質問に回答することから始めます。

具体的には、この章では、PowerShell を見つけて起動すること、および PowerShell の新しいユーザーが経験する最初の問題のいくつかを解決することに焦点を当てています。 お使いの Windows 10 ラボ環境コンピューターで、この章に示されている例の手順を必ず実行してください。

## <a name="what-do-i-need-to-get-started-with-powershell"></a>PowerShell を使い始めるには何が必要ですか?

Windows オペレーティング システムのすべての最新バージョンには、PowerShell がインストールされています。 5\.1 より前のバージョンを実行している場合は、最新バージョンをインストールする必要があります。

- Windows PowerShell 5.1 にアップグレードするには、「[既存の Windows PowerShell をアップグレードする][]」を参照してください
- PowerShell の最新バージョンをインストールするには、[PowerShell のインストール][]に関するページを参照してください

## <a name="where-do-i-find-powershell"></a>PowerShell はどこにありますか?

Windows 10 で PowerShell を見つける最も簡単な方法は、図 1-1 に示すように、検索バーに「**PowerShell**」と入力することです。

![図 1-1 - [スタート] メニューでの PowerShell の検索](media/figure1-1.png)

図 1-1 には PowerShell の 4 つの異なるショートカットが示されていることに注意してください。 このドキュメントでデモに使用しているコンピューターでは、64 ビット バージョンの Windows 10 が実行されています。そのため、64 ビット バージョンの PowerShell コンソールと PowerShell ISE (統合スクリプト環境) に加え、その 32 ビット バージョンがそれぞれあります (ショートカットの (x86) サフィックスで示されます)。 32 ビット バージョンの Windows 10 を実行している場合は、2 つのショートカットだけが表示されます。 これらの項目には (x86) サフィックスはありませんが、32 ビット バージョンです。 64 ビットのオペレーティング システムを使用している場合は、32 ビット バージョンを実行する特別な理由がない限り、64 ビット バージョンの PowerShell を実行することをお勧めします。

他のバージョンの Windows で PowerShell を起動する方法については、「[Windows PowerShell の開始][]」を参照してください。

## <a name="how-do-i-launch-powershell"></a>PowerShell を起動するにはどうすればよいですか?

私がサポートする運用エンタープライズ環境では、3 つの異なる Active Directory ユーザー アカウントを使用しています。 このドキュメントで使用しているラボ環境には、これらのアカウントが複製されています。 ドメイン管理者でもローカル管理者でもない、ドメイン ユーザーとして Windows 10 コンピューターにログインします。

私は、図 1-1 に示すように、[Windows PowerShell] ショートカットをクリックして PowerShell コンソールを起動しました。

![図 1-4 - PowerShell ウィンドウのタイトル バー](media/figure1-4.png)

図 1-4 に示すように、PowerShell コンソールのタイトル バーに "Windows PowerShell" と表示されていることに注意してください。 一部のコマンドは正常に実行されますが、PowerShell をユーザー アクセス制御 (UAC) に参加させることはできません。 つまり、管理者の承認を必要とするタスクの昇格を要求することはできません。
次のエラー メッセージが生成されます。

```powershell
Get-Service -Name W32Time | Stop-Service
```

```Output
Stop-Service : Service 'Windows Time (W32Time)' cannot be stopped due to the following
error: Cannot open W32Time service on computer '.'.
At line:1 char:29
+ Get-Service -Name W32Time | Stop-Service
+
    + CategoryInfo          : CloseError: (System.ServiceProcess.ServiceController:ServiceController)
     [Stop-Service], ServiceCommandException
    + FullyQualifiedErrorId : CouldNotStopService,Microsoft.PowerShell.Commands.StopServiceCommand
```

この問題の解決策は、ローカル管理者であるドメイン ユーザーとして PowerShell を実行することです。
これが、私の 2 番目のドメイン ユーザー アカウントの構成方法です。 最小限の特権の原則に従って、このアカウントはドメイン管理者にしたり、ドメイン内で昇格された特権を与えたりしないようにします。

PowerShell を閉じます。 PowerShell コンソールを再起動します。ただし、今回は、図 1-5 に示すように **[Windows PowerShell]** ショートカットを右クリックし、 **[管理者として実行]** を選択します。

![図 1-5 - コンテキスト メニュー - [管理者として実行]](media/figure1-5.png)

通常のユーザーとして Windows にログインしている場合は、資格情報の入力を求められます。 図 1-6 に示すように、ドメイン ユーザーとローカル管理者であるユーザー アカウントの資格情報を入力します。

![図 1-6](media/figure1-6.png)

PowerShell が管理者として再起動されると、タイトル バーに "管理者: Windows PowerShell" と表示されます (図 1-7)。

![図 1-7](media/figure1-7.png)

これで PowerShell がローカル管理者として管理者特権で実行されるようになったので、通常はローカル コンピューター上で昇格のプロンプトを必要とするコマンドを実行したときに UAC が問題にならなくなります。 このとき、PowerShell コンソールの管理者特権のインスタンスから実行されるすべてのコマンドが管理者特権で実行されることに注意してください。

PowerShell を見つけて管理者として起動する作業を簡単にするために、タスク バーにピン留めし、実行するたびに管理者として自動的に起動されるように設定することをお勧めします。

PowerShell をもう一度検索します。ただし今回は、図 1-8 に示すように、右クリックし、[タスク バーにピン留めする] を選択します。

![図 1-8](media/figure1-8.png)

タスク バーにピン留めされた PowerShell ショートカットを右クリックし、図 1-9 に示すように [プロパティ] を選択します。

![図 1-9 - [ユーザー アカウント制御] - [資格情報を入力]](media/figure1-9.png)

図 1-10 の番号 1 で示されている [詳細設定] をクリックし、図 1-10 の番号 2 で示されている [管理者として実行] チェック ボックスをオンにします。[OK] を 2 回クリックして変更を受け入れ、両方のダイアログ ボックスを終了します。

![図 1-10 - "管理者" と表示されているタイトル バー](media/figure1-10.png)

これで、PowerShell を見つけることや PowerShell が管理者として実行されているかどうかについて再び心配する必要がなくなります。

管理者特権で PowerShell を実行して UAC に関する問題を回避した場合、ローカル コンピューターに対して実行されるコマンドのみが影響を受けます。 リモート コンピューターをターゲットとするコマンドは影響を受けません。

## <a name="what-version-of-powershell-am-i-running"></a>実行されている PowerShell のバージョンは何ですか?

PowerShell には、状態情報を格納するための自動変数が多数あります。 これらの変数の 1 つに `$PSVersionTable` があります。これには、関連する PowerShell のバージョン情報を表示するために使用できるハッシュテーブルが含まれています。

```powershell
$PSVersionTable
```

```Output
Name                           Value
----                           -----
PSVersion                      5.1.19041.1
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.19041.1
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Windows PowerShell の新しいバージョンは、Windows Management Framework (WMF) の一部として配布されます。 WMF のバージョンによっては、.NET Framework の特定のバージョンが必要になります。 Windows PowerShell 5.1 にアップグレードするには、「[既存の Windows PowerShell をアップグレードする][]」を参照してください。

## <a name="execution-policy"></a>実行ポリシー

一般的な考えとは異なり、PowerShell の実行ポリシーはセキュリティ境界ではありません。 これは、ユーザーが無意識のうちにスクリプトを実行するのを防ぐように設計されています。 ユーザーは、その気になれば PowerShell の実行ポリシーを簡単にバイパスできます。 表 1-2 に、現在の Windows オペレーティング システムの既定の実行ポリシーを示します。

| Windows オペレーティング システムのバージョン | 既定の実行ポリシー |
| -------------------------------- | ------------------------ |
| Server 2019                      | リモート署名            |
| Server 2016                      | リモート署名            |
| Windows 10                       | 制限付き               |

実行ポリシーの設定に関係なく、すべての PowerShell コマンドは対話的に実行できます。 実行ポリシーは、スクリプト内で実行されているコマンドにのみ影響します。 `Get-ExecutionPolicy` コマンドレットは、現在の実行ポリシー設定を確認するために使用します。`Set-ExecutionPolicy` コマンドレットは、実行ポリシーを変更するために使用します。 お勧めは、**RemoteSigned** ポリシーを使用することです。これを使用すると、ダウンロードしたスクリプトを実行する際に、スクリプトが信頼できる公開元によって署名されていることが求められます。

現在の実行ポリシーを確認します。

```powershell
Get-ExecutionPolicy
```

```Output
Restricted
```

実行ポリシーが **Restricted** に設定されている場合は、PowerShell スクリプトをまったく実行できません。 これは、すべての Windows クライアント オペレーティング システムの既定の設定です。 この問題を実証するために、次のコードを `Stop-TimeService.ps1` という名前の `.ps1` ファイルとして保存します。

```powershell
Get-Service -Name W32Time | Stop-Service -PassThru
```

このコマンドは、PowerShell が管理者特権で実行されている限り、エラーなしで対話的に実行されます。 しかし、これをスクリプト ファイルとして保存し、スクリプトを実行しようとすると、エラーが発生します。

```powershell
.\Stop-TimeService.ps1
```

```Output
.\Stop-TimeService.ps1 : File C:\demo\Stop-TimeService.ps1 cannot be loaded because
running scripts is disabled on this system. For more information, see
about_Execution_Policies at http://go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ .\Stop-TimeService.ps1
+
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

前の結果のセットに示されているエラー "running scripts is disabled on this system" (このシステムではスクリプトの実行が無効になっています) から問題の内容が正確にわかることに注意してください。 PowerShell でエラー メッセージを生成するコマンドを実行する場合は、コマンドを再実行して正常に実行されることを期待するのではなく、必ずエラー メッセージを読んでください。

PowerShell 実行ポリシーを "リモート署名" に変更します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

```Output
Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution
policy might expose you to the security risks described in the about_Execution_Policies help topic
at http://go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "N"):y
```

実行ポリシーを変更するときは、表示される警告を必ずお読みください。 また、[about_Execution_Policies][] のヘルプ トピックを参照して、実行ポリシーを変更した場合のセキュリティへの影響を理解することをお勧めします。

実行ポリシーが **RemoteSigned** に設定されたので、`Stop-TimeService.ps1` スクリプトはエラーなしで実行されます。

```powershell
.\Stop-TimeService.ps1
```

```Output
Status   Name               DisplayName
------   ----               -----------
Stopped  W32Time            Windows Time
```

続行する前に Windows タイム サービスを開始してください。そうしないと、予期しない問題が発生する可能性があります。

```powershell
Start-Service -Name w32time
```

## <a name="summary"></a>まとめ

この章では、PowerShell を見つけて起動する方法と、PowerShell を管理者として起動するショートカットを作成する方法を学習しました。 また、既定の実行ポリシーとその変更方法についても学習しました。

## <a name="review"></a>確認

1. コンピューターで実行されている PowerShell のバージョンを確認するにはどうすればよいか。
1. 管理者特権で PowerShell を起動することが重要なのはなぜか。
1. 現在の PowerShell 実行ポリシーを確認するにはどうすればよいか。
1. Windows クライアント コンピューターの既定の PowerShell 実行ポリシーでは、どのような操作が回避されるか。
1. PowerShell 実行ポリシーを変更するにはどうすればよいか。

## <a name="recommended-reading"></a>推奨トピック

この章で説明しているトピックについてもっと詳しく知りたい方は、次の PowerShell のヘルプ トピックを読むことをお勧めします。

- [about_Automatic_Variables][]
- [about_Hash_Tables][]
- [about_Execution_Policies][]

次の章では、PowerShell のコマンドの見つけやすさについて学習します。 扱われている項目の 1 つでは、PowerShell を更新して、これらのヘルプ トピックをインターネットで表示する代わりに PowerShell 内から直接表示できるようにする方法について説明します。

<!-- link references -->
[about_Automatic_Variables]: /powershell/module/microsoft.powershell.core/about/about_automatic_variables
[about_Hash_Tables]: /powershell/module/microsoft.powershell.core/about/about_hash_tables
[about_Execution_Policies]: /powershell/module/microsoft.powershell.core/about/about_execution_policies
[既存の Windows PowerShell をアップグレードする]: /powershell/scripting/windows-powershell/install/installing-windows-powershell#upgrading-existing-windows-powershell
[PowerShell のインストール]: /powershell/scripting/install/installing-powershell
[Windows PowerShell の開始]: /powershell/scripting/windows-powershell/starting-windows-powershell
