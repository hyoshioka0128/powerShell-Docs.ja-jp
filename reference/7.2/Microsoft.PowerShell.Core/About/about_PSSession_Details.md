---
description: PowerShell セッションとそれらがリモートコマンドで果たす役割の詳細について説明します。
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pssession_details?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PSSession_Details
ms.openlocfilehash: 79340e5d0ae1fe047f1f37bdfdbb1bebe920d851
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602765"
---
# <a name="about-pssession-details"></a>PSSession の詳細について

## <a name="short-description"></a>簡単な説明
PowerShell セッションとそれらがリモートコマンドで果たす役割の詳細について説明します。

## <a name="long-description"></a>長い説明

セッションとは、PowerShell が実行される環境です。 PowerShell を起動するたびに、セッションが作成されます。 コンピューターまたは別のコンピューターに "PowerShell セッション" または "PSSessions" という追加のセッションを作成できます。

PowerShell によって作成されるセッションとは異なり、作成する pssession を制御および管理します。

PSSessions は、リモートコンピューティングにおいて重要な役割を果たします。 リモートコンピューターに接続されている PSSession を作成すると、PowerShell は PSSession をサポートするために、リモートコンピューターへの永続的な接続を確立します。 PSSession を使用すると、データを共有する一連のコマンド、関数、およびスクリプトを実行できます。

このトピックでは、PowerShell でのセッションと pssession の詳細について説明します。 セッションで実行できるタスクに関する基本的な情報については、「 [about_PSSessions](about_PSSessions.md)」を参照してください。

## <a name="about-sessions"></a>セッションについて

技術的には、セッションとは、PowerShell が実行される実行環境のことです。 各セッションには、PowerShell を実行するための、システム管理エンジンとホストプログラムのインスタンスが含まれています。 ホストは、使い慣れた PowerShell コンソール、または Cmd.exe などのコマンドを実行する別のプログラム、または PowerShell をホストするために構築されたプログラム (Windows PowerShell Integrated Scripting Environment (ISE) など) にすることができます。 Windows の観点から見ると、セッションはターゲットコンピューター上の Windows プロセスです。

各セッションは個別に構成されます。 これには、独自のプロパティ、独自の実行ポリシー、および独自のプロファイルが含まれます。 セッションの作成時に存在する環境は、コンピューター上の環境を変更しても、その有効期間にわたって保持されます。 すべてのセッションは、スクリプトで作成したセッションでも、グローバルスコープで作成されます。

セッションでは、一度に1つのコマンド (またはコマンドパイプライン) のみを実行できます。 2番目のコマンドは同期的に (一度に1つずつ) 実行され、最初のコマンドが完了するまで最大4分間待機します。 非同期的に (同時に) 実行する2番目のコマンドは失敗します。

## <a name="about-pssessions"></a>PSSessions について

PowerShell を起動するたびにセッションが作成されます。 また、PowerShell は、個々のコマンドを実行するための一時的なセッションを作成します。
ただし、制御および管理するセッション ("PowerShell セッション" または "PSSessions" と呼ばれます) を作成することもできます。

PSSessions は、リモートコマンドにとって重要です。 またはコマンドレットの **ComputerName** パラメーターを使用する `Invoke-Command` `Enter-PSSession` と、PowerShell はコマンドを実行するための一時的なセッションを確立し、コマンドまたは対話型セッションが完了するとすぐにセッションを終了します。

ただし、コマンドレットを使用して PSSession を作成した場合、PowerShell は、 `New-PSSession` 複数のコマンドや対話型セッションを実行できる、リモートコンピューター上の永続的なセッションを確立します。 作成した pssession は開いたままで、削除するか、作成されたセッションを閉じるまで使用できます。

リモートコンピューター上に PSSession を作成すると、システムによってリモートコンピューター上に PowerShell プロセスが作成され、ローカルコンピューターからリモートコンピューター上のプロセスへの接続が確立されます。 ローカルコンピューター上に PSSession を作成すると、新しいプロセスと接続の両方がローカルコンピューター上に作成されます。

## <a name="when-do-i-need-a-pssession"></a>PSSession はどのような場合に必要ですか。

`Invoke-Command`およびコマンドレットには、 `Enter-PSSession` **ComputerName** パラメーターと **Session** パラメーターの両方があります。 リモートコマンドを実行するには、いずれかを使用します。

1つまたは複数のコンピューターで1つのコマンドまたは一連の関連のないコマンドを実行するには、 **ComputerName** パラメーターを使用します。

データを共有するコマンドを実行するには、リモートコンピューターへの永続的な接続が必要です。 その場合は、PSSession を作成し、 **セッション** パラメーターを使用して pssession でコマンドを実行します。

、、、などのリモートコンピューターからデータを取得する他の多くのコマンドレットには、 `Get-Process` `Get-Service` `Get-EventLog` `Get-WmiObject` **ComputerName** パラメーターしかありません。 リモートでデータを収集するために、PowerShell リモート処理以外のテクノロジを使用します。 これらのコマンドレットには **Session** パラメーターはありませんが、コマンドレットを使用して、 `Invoke-Command` PSSession でこれらのコマンドを実行できます。

## <a name="how-do-i-create-a-pssession"></a>PSSession を作成するにはどうすればよいですか。

PSSession を作成するには、 `New-PSSession` コマンドレットを使用します。 を使用し `New-PSSession` て、ローカルコンピューターまたはリモートコンピューター上に PSSession を作成できます。

## <a name="can-i-create-a-pssession-on-any-computer"></a>任意のコンピューターで PSSession を作成できますか。

リモートコンピューターに接続されている PSSession を作成するには、PowerShell でリモート処理用にコンピューターを構成する必要があります。 現在のユーザーは、リモートコンピューターの Administrators グループのメンバーであるか、または現在のユーザーが Administrators グループのメンバーの資格情報を提供できる必要があります。 詳細については、「[about_Remote_Requirements](about_Remote_Requirements.md)」を参照してください。

## <a name="can-i-see-my-pssessions-in-other-sessions"></a>他のセッションでは、PSSessions を表示できますか。

Windows PowerShell 3.0 以降では、コマンドレットの **ComputerName** パラメーターは、 `Get-PSSession` 指定されたリモートコンピューターで作成した pssessions を取得します。

アクティブな PSSessions は、リモートコンピューター (接続の "サーバー側") で保持され、任意のコンピューター上の任意のセッションから取得できます。

たとえば、Server01 コンピューターから Server02 コンピューターに PSSession を作成し、Server03 コンピューターに切り替える場合は、次のようなコマンドを使用してセッションを取得できます。

```powershell
Get-PSSession -ComputerName Server02
```

セッションから切断した場合でも、セッションは、削除するか、タイムアウトになるまで、リモートコンピューター上で保持されます。

Windows PowerShell 2.0 では、現在のセッションで作成した pssession のみを取得できます。 他のセッションで作成した pssession を取得することはできません。

詳細については、「 [Get PSSession](xref:Microsoft.PowerShell.Core.Get-PSSession)」を参照してください。

## <a name="can-i-see-the-pssessions-that-others-have-created-on-my-computer"></a>他のユーザーがマイコンピューターで作成した PSSessions を確認できますか。

PSSession を作成したユーザーの資格情報、または PSSession が使用するセッション構成に RunAs 資格情報が含まれている場合にのみ、他のユーザーが作成した PSSession のみを取得して管理できます。 そうしないと、作成した pssession だけを取得、接続、使用、管理できます。

## <a name="can-i-connect-to-a-pssession-from-a-different-computer"></a>別のコンピューターから PSSession に接続することはできますか。

Windows PowerShell 3.0 以降では、PSSessions は、それらが作成されたセッションから独立しています。 アクティブな PSSessions は、接続のリモートまたは "サーバー側" でコンピューター上に保持されます。

コマンドレットを使用して、 `Disconnect-PSSession` PSSession から切断することができます。 PSSession はローカルセッションから切断されますが、リモートコンピューター上で保持されます。
コマンドは、切断された PSSession で引き続き実行されます。 PSSession を中断せずに、PowerShell を閉じて、元のコンピューターをシャットダウンすることができます。

その後、数時間後に、コマンドレットを使用して PSSession を取得し、コマンドレットを使用して `Get-PSSession` `Connect-PSSession` 別のコンピューター上の新しいセッションから pssession に接続できます。

詳細については、「 [about_Remote_Disconnected_Sessions](about_Remote_Disconnected_Sessions.md)」を参照してください。

## <a name="what-happens-to-my-pssession-if-my-computer-stops"></a>マイコンピューターが停止した場合、PSSession はどうなりますか。

切断された pssession は、そのセッションが作成されたセッションから独立しています。 PSSession を切断してから、元のコンピューターを閉じると、PSSession がリモートコンピューター上で保持されます。

さらに、PowerShell は、コンピューターの再起動、一時的な停電、ネットワークの中断など、意図せずに切断されたアクティブな PSSessions を回復しようとします。 PowerShell は、開始された状態への PSSession の保持または復旧を試みます。発信元のセッションがまだ使用可能な場合、または切断されている場合は切断状態になります。

"アクティブ" PSSession は、コマンドを実行しているものです。 PSSession が接続されている (切断されていない) 場合、接続されたセッションが終了したときに PSSession でコマンドが実行されていると、PowerShell はリモートコンピューター上の PSSession を維持しようとします。 ただし、PSSession で実行されているコマンドがない場合は、接続されているセッションが閉じると、PowerShell によって PSSession が閉じられます。

詳細については、「 [about_Remote_Disconnected_Sessions](about_Remote_Disconnected_Sessions.md)」を参照してください。

## <a name="can-i-run-a-background-job-in-a-pssession"></a>バックグラウンドジョブを PSSession で実行できますか。

はい。 バックグラウンドジョブは、現在のセッションと対話することなくバックグラウンドで非同期に実行されるコマンドです。 ジョブを開始するコマンドを送信すると、このコマンドはジョブオブジェクトを返しますが、ジョブは完了するまでバックグラウンドで実行され続けます。

ローカルコンピューターでバックグラウンドジョブを開始するには、コマンドを使用し `Start-Job` ます。
バックグラウンドジョブは、( **ComputerName** パラメーターを使用して) 一時的な接続で実行するか、または ( **Session** パラメーターを使用して) PSSession で実行できます。

リモートコンピューターでバックグラウンドジョブを開始するに `Invoke-Command` は、コマンドレットを **AsJob** パラメーターと共に使用するか、コマンドレットを使用して `Invoke-Command` `Start-Job` リモートコンピューターでコマンドを実行します。 **AsJob** パラメーターを使用する場合は、 **ComputerName** パラメーターまたは **Session** パラメーターを使用できます。

を使用してコマンドを実行する場合は、 `Invoke-Command` `Start-Job` PSSession でコマンドを実行する必要があります。 **ComputerName** パラメーターを使用する場合、PowerShell はジョブオブジェクトから制御が戻ったときに接続を終了し、ジョブが中断されます。

詳細については、「[about_Jobs](about_Jobs.md)」を参照してください。

## <a name="can-i-run-an-interactive-session"></a>対話型セッションを実行できますか。

はい。 リモートコンピューターとの対話型セッションを開始するには、 `Enter-PSSession` コマンドレットを使用します。 対話型セッションでは、入力したコマンドはリモートコンピューター上で直接入力した場合と同じように、リモートコンピューター上で実行されます。

( **ComputerName** パラメーターを使用して) 一時的なセッションで、または ( **セッション** パラメーターを使用して) PSSession で対話型セッションを実行できます。
PSSession を使用する場合、PSSession は前のコマンドのデータを保持し、PSSession は、後のコマンドで使用するために対話型セッション中に生成されたすべてのデータを保持します。

対話型セッションを終了すると、PSSession は開いたままになり、使用できるようになります。

詳細については、「PSSession と[出口](xref:Microsoft.PowerShell.Core.Exit-PSSession)の[入力](xref:Microsoft.PowerShell.Core.Enter-PSSession)」を参照してください。

## <a name="must-i-delete-the-pssessions"></a>PSSessions を削除する必要がありますか。

はい。 PSSession は、メモリやその他のリソースを使用していない場合でも、それを使用する自己完結型の環境であるプロセスです。 PSSession が終了したら、それを削除します。 複数の pssession を作成する場合は、使用していない PSSessions を閉じ、現在使用されているものだけを維持します。

PSSessions を削除するには、コマンドレットを使用し `Remove-PSSession` ます。 PSSessions を削除し、使用していたすべてのリソースを解放します。

また、の **IdleTimeOut** パラメーターを使用して、 `New-PSSessionOption` 指定した間隔の後にアイドル状態の PSSession を閉じることもできます。 詳細については、「 [New-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)」を参照してください。

PSSession オブジェクトを変数に保存し、PSSession を削除するか、タイムアウトにすると、変数には PSSession オブジェクトが残りますが、PSSession はアクティブではなく、使用または修復もできません。

## <a name="are-all-sessions-and-pssessions-alike"></a>すべてのセッションと PSSessions は似ていますか。

いいえ。 開発者は、選択したプロバイダーとコマンドレットのみを含むカスタムセッションを作成できます。 あるセッションではコマンドが動作し、別のセッションでは動作しない場合は、セッションが制限されていることが原因である可能性があります。

## <a name="see-also"></a>参照

[about_Jobs](about_Jobs.md)

[about_PSSessions](about_PSSessions.md)

[about_Remote](about_Remote.md)

[about_Remote_Disconnected_Sessions](about_Remote_Disconnected_Sessions.md)

[about_Remote_Requirements](about_Remote_Requirements.md)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)

[Exit-PSSession](xref:Microsoft.PowerShell.Core.Exit-PSSession)

[Get-PSSession](xref:Microsoft.PowerShell.Core.Get-PSSession)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

[Remove-PSSession](xref:Microsoft.PowerShell.Core.Remove-PSSession)

