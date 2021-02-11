---
description: リモートコンピューターでジョブを実行する方法について説明します。
Locale: en-US
ms.date: 11/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_jobs?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_Jobs
ms.openlocfilehash: 93e1d3aba1f4037cc3c5c18386488bf321fc2a64
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599903"
---
# <a name="about-remote-jobs"></a>リモートジョブについて

## <a name="short-description"></a>簡単な説明
リモートコンピューターでバックグラウンドジョブを実行する方法について説明します。

## <a name="detailed-description"></a>詳しい説明

PowerShell は、ジョブを使用してコマンドとスクリプトを同時に実行します。 同時実行をサポートするために PowerShell によって提供されるジョブの種類は3つあります。

- `RemoteJob` -コマンドとスクリプトはリモートセッションで実行されます。
- `BackgroundJob` -コマンドとスクリプトは、ローカルコンピューター上で個別のプロセスで実行されます。 詳細については、「[about_Jobs](about_Jobs.md)」を参照してください。
- `PSTaskJob` または `ThreadJob` -コマンドとスクリプトは、ローカルコンピューター上の同じプロセス内の別のスレッドで実行されます。 詳細については、「 [about_Thread_Jobs](/powershell/module/ThreadJob/about_Thread_Jobs)」を参照してください。

別のコンピューターまたは別のプロセスでスクリプトをリモートで実行すると、優れた分離が実現します。 リモートジョブで発生したエラーは、他の実行中のジョブ、またはジョブを開始した親セッションには影響しません。 ただし、リモート処理層では、オブジェクトのシリアル化などのオーバーヘッドが発生します。 すべてのオブジェクトは、親セッションとリモート (ジョブ) セッションの間で渡されるときにシリアル化および逆シリアル化されます。 大きな複雑なデータオブジェクトのシリアル化は、大量のコンピューティングリソースとメモリリソースを消費し、ネットワーク経由で大量のデータを転送することがあります。

> [!IMPORTANT]
> ジョブを作成した親セッションもジョブの状態を監視し、パイプラインデータを収集します。 ジョブが完了状態になると、親プロセスによってジョブの子プロセスが終了します。 親セッションが終了すると、実行中のすべての子ジョブが子プロセスと共に終了します。

この状況に対処するには、次の2つの方法があります。

1. `Invoke-Command`切断されたセッションで実行されるジョブを作成するには、を使用します。 この記事の「デタッチされた [プロセス](#how-to-run-as-a-detached-process) 」セクションを参照してください。
1. `Start-Process`ジョブではなく新しいプロセスを作成するには、を使用します。 詳細については、「 [Start-Process](xref:Microsoft.PowerShell.Management.Start-Process)」を参照してください。

## <a name="remote-jobs"></a>リモートジョブ

3種類の方法を使用して、リモートコンピューターでジョブを実行できます。

- リモートコンピューターで対話型セッションを開始します。 次に、対話型セッションでジョブを開始します。 これらの手順はローカルジョブを実行するのと同じですが、すべての操作はリモートコンピューター上で実行されます。

- 結果をローカルコンピューターに返すリモートコンピューターでジョブを実行します。 ジョブの結果を収集し、ローカルコンピューター上の中央の場所に保持する場合は、この方法を使用します。

- リモートコンピューター上で結果が保持されているリモートコンピューターでジョブを実行します。 この方法は、ジョブデータを元のコンピューターでより安全に維持する場合に使用します。

### <a name="start-a-job-in-an-interactive-session"></a>対話型セッションでジョブを開始する

リモートコンピューターで対話型セッションを開始し、対話型セッション中にジョブを開始することができます。 対話型セッションの詳細については、「about_Remote」および「」を参照してください `Enter-PSSession` 。

対話型セッションでジョブを開始する手順は、ローカルコンピューターでバックグラウンドジョブを開始する手順とほぼ同じです。 ただし、すべての操作は、ローカルコンピューターではなく、リモートコンピューター上で行われます。

1. `Enter-PSSession`コマンドレットを使用して、リモートコンピューターとの対話型セッションを開始します。 の ComputerName パラメーターを使用して、 `Enter-PSSession` 対話型セッションの一時的な接続を確立できます。 または、Session パラメーターを使用して、PowerShell セッション (PSSession) で対話型セッションを実行できます。

   次のコマンドは、Server01 コンピューター上で対話型セッションを開始します。

   ```powershell
   C:\PS> Enter-PSSession -computername Server01
   ```

   Server01 コンピューターに接続されていることを示すために、コマンドプロンプトが変更されます。

   ```
   Server01\C:>
   ```

1. セッションでリモートジョブを開始するには、コマンドレットを使用し `Start-Job` ます。 次のコマンドは、Server01 コンピューター上の Windows PowerShell イベントログに含まれるイベントを取得するリモートジョブを実行します。 `Start-Job`コマンドレットは、ジョブを表すオブジェクトを返します。

   このコマンドは、ジョブオブジェクトを変数に保存し `$job` ます。

   ```powershell
   Server01\C:> $job = Start-Job -scriptblock {
     Get-Eventlog "Windows PowerShell"
   }
   ```

   ジョブの実行中に、対話型セッションを使用して他のコマンド (他のジョブを含む) を実行できます。 ただし、ジョブが完了するまで、対話型セッションを開いたままにしておく必要があります。 セッションを終了すると、ジョブは中断され、結果は失われます。

1. ジョブが完了したかどうかを確認するには、変数の値を表示する `$job` か、コマンドレットを使用してジョブを取得し `Get-Job` ます。 次のコマンドでは、 `Get-Job` コマンドレットを使用してジョブを表示します。

   ```powershell
   Server01\C:> Get-Job $job

   SessionId  Name  State      HasMoreData  Location   Command
   ---------  ----  -----      -----------  --------   -------
   1          Job1  Complete   True         localhost  Get-Eventlog "Windows...
   ```

   この出力は、ジョブ `Get-Job` が "localhost" コンピューター上で実行されていることを示しています。これは、ジョブがで開始され、同じコンピューター (この場合は Server01) で実行されているためです。

1. ジョブの結果を取得するには、コマンドレットを使用し `Receive-Job` ます。 対話型セッションで結果を表示したり、リモートコンピューター上のファイルに結果を保存したりできます。 次のコマンドは、$job 変数内のジョブの結果を取得します。 このコマンドは、リダイレクト演算子 () を使用して、 `>` ジョブの結果を Server01 コンピューターの PsLog.txt ファイルに保存します。

   ```powershell
   Server01\C:> Receive-Job $job > c:\logs\PsLog.txt
   ```

1. 対話型セッションを終了するには、 `Exit-PSSession` コマンドレットを使用します。 コマンドプロンプトが変更され、ローカルコンピューターの元のセッションに戻ったことが示されます。

   ```powershell
   Server01\C:> Exit-PSSession
   C:\PS>
   ```

1. Server01 コンピューター上のファイルの内容をいつでも表示するに `PsLog.txt` は、別の対話型セッションを開始するか、リモートコマンドを実行します。 この種類のコマンドは、複数のコマンドを使用してファイル内のデータを調査および管理する場合に、PSSession (固定接続) で実行することをお勧めし `PsLog.txt` ます。 PSSessions の詳細については、「 [about_PSSessions](about_PSSessions.md)」を参照してください。

   次のコマンドは、コマンドレットを使用して、 `New-PSSession` Server01 コンピューターに接続されている **pssession** を作成し、コマンドレットを使用して `Invoke-Command` pssession 内のコマンドを実行し、 `Get-Content` ファイルの内容を表示します。

   ```powershell
   $s = New-PSSession -computername Server01
   Invoke-Command -session $s -scriptblock {
     Get-Content c:\logs\pslog.txt}
   ```

### <a name="start-a-remote-job-that-returns-the-results-to-the-local-computer-asjob"></a>結果をローカルコンピューター (AsJob) に返すリモートジョブを開始します。

コマンドの結果をローカルコンピューターに返すリモートコンピューターでジョブを開始するには、コマンドレットなどのコマンドレットの **AsJob** パラメーターを使用し `Invoke-Command` ます。

**AsJob** パラメーターを使用すると、ジョブがリモートコンピューター上で実行されている場合でも、ジョブオブジェクトは実際にはローカルコンピューター上に作成されます。 ジョブが完了すると、結果がローカルコンピューターに返されます。

ジョブの名詞を含むコマンドレット (Job コマンドレット) を使用して、任意のコマンドレットによって作成されたすべてのジョブを管理できます。 **AsJob** パラメーターを持つコマンドレットの多くは、PowerShell リモート処理を使用しないので、リモート処理用に構成されておらず、リモート処理の要件を満たしていないコンピューターでも使用できます。

1. 次のコマンドでは、の **AsJob** パラメーターを使用して `Invoke-Command` 、Server01 コンピューター上でジョブを開始します。 このジョブは、 `Get-Eventlog` システムログ内のイベントを取得するコマンドを実行します。 JobName パラメーターを使用して、ジョブに表示名を割り当てることができます。

   ```powershell
   Invoke-Command -computername Server01 -scriptblock {
     Get-Eventlog system} -AsJob
   ```

   コマンドの結果は、次のサンプル出力のようになります。

   ```Output
   SessionId   Name   State    HasMoreData   Location   Command
   ---------   ----   -----    -----------   --------   -------
   1           Job1   Running  True          Server01   Get-Eventlog system
   ```

   **AsJob** パラメーターが使用されている場合、はを `Invoke-Command` 返すのと同じ種類のジョブオブジェクトを返し `Start-Job` ます。 ジョブオブジェクトを変数に保存することも、コマンドを使用してジョブを取得することもでき `Get-Job` ます。

   Location プロパティの値は、Server01 コンピューターでジョブが実行されたことを示しています。

1. コマンドレットの **AsJob** パラメーターを使用して開始されたジョブを管理するには `Invoke-Command` 、job コマンドレットを使用します。 リモートジョブを表すジョブオブジェクトはローカルコンピューター上にあるため、リモートコマンドを実行してジョブを管理する必要はありません。

   ジョブが完了したかどうかを確認するには、コマンドを使用し `Get-Job` ます。 次のコマンドは、現在のセッションで開始されたすべてのジョブを取得します。

   ```powershell
   Get-Job
   ```

   リモートジョブが現在のセッションで開始されたため、ローカル `Get-Job` コマンドはジョブを取得します。 ジョブオブジェクトの State プロパティは、コマンドが正常に完了したことを示しています。

   ```Output
   SessionId   Name   State      HasMoreData   Location   Command
   ---------   ----   -----      -----------   --------   -------
   1           Job1   Completed  True          Server01   Get-Eventlog system
   ```

1. ジョブの結果を取得するには、コマンドレットを使用し `Receive-Job` ます。 ジョブの結果は、ジョブオブジェクトが存在するコンピューターに自動的に返されるため、ローカルコマンドを使用して結果を取得でき `Receive-Job` ます。

   次のコマンドでは、 `Receive-Job` コマンドレットを使用してジョブの結果を取得します。 セッション ID を使用してジョブを識別します。 このコマンドは、ジョブの結果を $results 変数に保存します。 また、結果をファイルにリダイレクトすることもできます。

   ```powershell
   $results = Receive-Job -id 1
   ```

### <a name="start-a-remote-job-that-keeps-the-results-on-the-remote-computer"></a>リモートコンピューターで結果を保持するリモートジョブを開始する

リモートコンピューターでコマンドの結果を保持するジョブをリモートコンピューター上で開始するには、コマンドレットを使用して `Invoke-Command` リモートコンピューターでコマンドを実行し `Start-Job` ます。 この方法を使用すると、複数のコンピューターでジョブを実行できます。

コマンドをリモートで実行すると、 `Start-Job` ジョブオブジェクトがリモートコンピューター上に作成され、ジョブの結果がリモートコンピューターに保持されます。
ジョブの観点から見ると、すべての操作はローカルです。 リモートコンピューター上のローカルジョブを管理するために、コマンドをリモートで実行しているだけです。

1. コマンドレットを使用して、 `Invoke-Command` `Start-Job` リモートコンピューターでコマンドを実行します。

   このコマンドには PSSession (永続的な接続) が必要です。 の ComputerName パラメーターを使用して `Invoke-Command` 一時的な接続を確立すると、 `Invoke-Command` ジョブオブジェクトが返されたときにコマンドは完了したと見なされます。 その結果、一時的な接続は閉じられ、ジョブは取り消されます。

   次のコマンドでは、コマンドレットを使用して、 `New-PSSession` Server01 コンピューターに接続されている PSSession を作成します。 このコマンドは、PSSession を変数に保存し `$s` ます。

   ```powershell
   $s = New-PSSession -computername Server01
   ```

   次のコマンドは、コマンドレットを使用して `Invoke-Command` `Start-Job` PSSession 内のコマンドを実行します。 `Start-Job`コマンドとコマンドは、 `Get-Eventlog` 中かっこで囲まれています。

   ```powershell
   Invoke-Command -session $s -scriptblock {
     Start-Job -scriptblock {Get-Eventlog system}}
   ```

   結果は次のサンプル出力のようになります。

   ```Output
   Id       Name    State      HasMoreData     Location   Command
   --       ----    -----      -----------     --------   -------
   2        Job2    Running    True            Localhost  Get-Eventlog system
   ```

   コマンドをリモートで実行すると `Start-Job` 、に `Invoke-Command` よって返されるのと同じ種類のジョブオブジェクトが返さ `Start-Job` れます。 ジョブオブジェクトを変数に保存することも、コマンドを使用してジョブを取得することもでき `Get-Job` ます。

   **Location** プロパティの値は、ジョブが Server01 コンピューター上で実行された場合でも、ローカルコンピューター上でジョブが実行されたことを示しています。これは、"LocalHost" と呼ばれています。 ジョブオブジェクトは Server01 コンピューター上に作成され、ジョブは同じコンピューター上で実行されるため、ローカルのバックグラウンドジョブと見なされます。

1. リモートジョブを管理するには、 **job** コマンドレットを使用します。 ジョブオブジェクトはリモートコンピューター上にあるため、リモートコマンドを実行して、ジョブの結果を取得、停止、待機、または取得する必要があります。

   ジョブが完了したかどうかを確認するには、コマンドを使用して、 `Invoke-Command` `Get-Job` Server01 コンピューターに接続されている PSSession でコマンドを実行します。

   ```powershell
   Invoke-Command -session $s -scriptblock {Get-Job}
   ```

   このコマンドによって返されるのは、ジョブ オブジェクトです。 ジョブオブジェクトの **State** プロパティは、コマンドが正常に完了したことを示しています。

   ```Output
   SessionId   Name  State      HasMoreData   Location   Command
   ---------   ----  -----      -----------   --------   -------
   2           Job2  Completed  True          LocalHost   Get-Eventlog system
   ```

1. ジョブの結果を取得するには、コマンドレットを使用して、 `Invoke-Command` `Receive-Job` Server01 コンピューターに接続されている PSSession でコマンドを実行します。

   次のコマンドでは、 `Receive-Job` コマンドレットを使用してジョブの結果を取得します。 セッション ID を使用してジョブを識別します。 このコマンドは、ジョブの結果を変数に保存し `$results` ます。 この例では、の Keep パラメーターを使用して、 `Receive-Job` リモートコンピューター上のジョブキャッシュに結果を保持します。

   ```powershell
   $results = Invoke-Command -session $s -scriptblock {
     Receive-Job -SessionId 2 -Keep
   }
   ```

   また、ローカルコンピューターまたはリモートコンピューター上のファイルに結果をリダイレクトすることもできます。
   次のコマンドは、リダイレクト演算子を使用して、Server01 コンピューター上のファイルに結果を保存します。

   ```powershell
   Invoke-Command -session $s -command {
     Receive-Job -SessionId 2 > c:\logs\pslog.txt
   }
   ```

## <a name="how-to-run-as-a-detached-process"></a>デタッチされたプロセスとして実行する方法

既に説明したように、親セッションが終了すると、実行中のすべての子ジョブが子プロセスと共に終了します。 ローカルコンピューターでリモート処理を使用して、現在の PowerShell セッションにアタッチされていないジョブを実行できます。

ローカルコンピューターで新しい PowerShell セッションを作成します。 `Invoke-Command`このセッションでジョブを開始するために使用する。 `Invoke-Command` リモートセッションを切断し、親セッションを終了することができます。 後で、新しい PowerShell セッションを開始し、以前に切断されたセッションに接続して、ジョブの監視を再開できます。 ただし、元の PowerShell セッションに返されたデータは、そのセッションが終了したときに失われます。 再接続時に返されるのは、切断後に生成される新しいデータオブジェクトだけです。

```powershell
# Create remote session on local machine
PS> $session = New-PSSession -cn localhost

# Start remote job
PS> $job = Invoke-Command -Session $session -ScriptBlock { 1..60 | % { sleep 1; "Output $_" } } -AsJob
PS> $job

Id     Name     PSJobTypeName   State         HasMoreData     Location      Command
--     ----     -------------   -----         -----------     --------      -------
1      Job1     RemoteJob       Running       True            localhost     1..60 | % { sleep 1; ...

# Disconnect the job session
PS> Disconnect-PSSession $session

Id Name         Transport ComputerName    ComputerType    State         ConfigurationName     Availability
-- ----         --------- ------------    ------------    -----         -----------------     ------------
1 Runspace1     WSMan     localhost       RemoteMachine   Disconnected  Microsoft.PowerShell          None

PS> $job

Id     Name     PSJobTypeName   State         HasMoreData     Location      Command
--     ----     -------------   -----         -----------     --------      -------
1      Job1     RemoteJob       Disconnected  True            localhost     1..60 | % { sleep 1;

# Reconnect the session to a new job object
PS> $jobNew = Receive-PSSession -Session $session -OutTarget Job
PS> $job | Wait-Job | Receive-Job
Output 9
Output 10
Output 11
...
```

この例では、ジョブは引き続き親 PowerShell セッションにアタッチされます。
ただし、親セッションは、が実行された元の PowerShell セッションではありません `Invoke-Command` 。

## <a name="see-also"></a>関連項目

- [about_Jobs](about_Jobs.md)
- [about_Job_Details](about_Job_Details.md)
- [about_Remote](about_Remote.md)
- [about_Remote_Variables](about_Remote_Variables.md)
- [Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)
- [Start-Job](xref:Microsoft.PowerShell.Core.Start-Job)
- [Get-Job](xref:Microsoft.PowerShell.Core.Get-Job)
- [Wait-Job](xref:Microsoft.PowerShell.Core.Wait-Job)
- [Stop-Job](xref:Microsoft.PowerShell.Core.Stop-Job)
- [Remove-Job](xref:Microsoft.PowerShell.Core.Remove-Job)
- [New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)
- [Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)
- [Exit-PSSession](xref:Microsoft.PowerShell.Core.Exit-PSSession)
