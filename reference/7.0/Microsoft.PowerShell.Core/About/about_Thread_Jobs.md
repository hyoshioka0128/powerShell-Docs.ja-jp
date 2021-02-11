---
description: PowerShell スレッドベースのジョブに関する情報を提供します。 スレッドジョブは、現在のセッションプロセス内の別のスレッドでコマンドまたは式を実行するバックグラウンドジョブの一種です。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/11/2020
online version: 1.0.0
schema: 2.0.0
title: about_Thread_Jobs
ms.openlocfilehash: ba6251a195d3efdebd427b3f705386336b069211
ms.sourcegitcommit: aac365f7813756e16b59322832a904e703e0465b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2020
ms.locfileid: "94524639"
---
# <a name="about-thread-jobs"></a>スレッドジョブについて

## <a name="short-description"></a>簡単な説明

PowerShell スレッドベースのジョブに関する情報を提供します。 スレッドジョブは、現在のセッションプロセス内の別のスレッドでコマンドまたは式を実行するバックグラウンドジョブの一種です。

## <a name="long-description"></a>長い説明

PowerShell は、ジョブを使用してコマンドとスクリプトを同時に実行します。 同時実行をサポートするために PowerShell によって提供されるジョブの種類は3つあります。

- `RemoteJob` -コマンドとスクリプトはリモートセッションで実行されます。 詳細については、「 [about_Remote_Jobs](about_Remote_Jobs.md)」を参照してください。
- `BackgroundJob` -コマンドとスクリプトは、ローカルコンピューター上で個別のプロセスで実行されます。 詳細については、「[about_Jobs](about_Jobs.md)」を参照してください。
- `PSTaskJob` または `ThreadJob` -コマンドとスクリプトは、ローカルコンピューター上の同じプロセス内の別のスレッドで実行されます。

スレッドベースのジョブは、異なるスレッドで同じプロセスで実行されるため、リモートジョブやバックグラウンドジョブほど堅牢ではありません。 あるジョブで、プロセスをクラッシュさせる重大なエラーが発生している場合は、プロセス内の他のすべてのジョブが終了します。

ただし、スレッドベースのジョブではオーバーヘッドが少なくなります。 これらは、リモート処理レイヤーまたはシリアル化を使用しません。 結果オブジェクトは、現在のセッションのライブオブジェクトへの参照として返されます。 このオーバーヘッドがないと、スレッドベースのジョブはより高速に実行され、他の種類のジョブよりも使用されるリソースが少なくなります。

> [!IMPORTANT]
> ジョブを作成した親セッションもジョブの状態を監視し、パイプラインデータを収集します。 ジョブが完了状態になると、親プロセスによってジョブの子プロセスが終了します。 親セッションが終了すると、実行中のすべての子ジョブが子プロセスと共に終了します。

この状況に対処するには、次の2つの方法があります。

1. `Invoke-Command`切断されたセッションで実行されるジョブを作成するには、を使用します。 詳細については、「 [about_Remote_Jobs](about_Remote_Jobs.md)」を参照してください。
1. `Start-Process`ジョブではなく新しいプロセスを作成するには、を使用します。 詳細については、「 [Start-Process](xref:Microsoft.PowerShell.Management.Start-Process)」を参照してください。

## <a name="how-to-start-and-manage-thread-based-jobs"></a>スレッドベースのジョブを開始および管理する方法

スレッドベースのジョブを開始するには、次の2つの方法があります。

- `Start-ThreadJob` - **Threadjob** モジュールから
- `ForEach-Object -Parallel -AsJob` -並列機能は PowerShell 7.0 で追加されました。

[About_Jobs](about_Jobs.md)に記載されているのと同じ **ジョブ** コマンドレットを使用して、スレッドベースのジョブを管理します。

### <a name="using-start-threadjob"></a>`Start-ThreadJob` の使用

**Threadjob** モジュールは、最初に PowerShell 6 に付属しています。 また、Windows PowerShell 5.1 の PowerShell ギャラリーからインストールすることもできます。

ローカルコンピューターでスレッドジョブを開始するに `Start-ThreadJob` は、コマンドレットを、中かっこ () で囲まれたコマンドまたはスクリプトと共に使用し `{ }` ます。

次の例では、ローカルコンピューターでコマンドを実行するスレッドジョブを開始し `Get-Process` ます。

```powershell
Start-ThreadJob -ScriptBlock { Get-Process }
```

コマンドは、 `Start-ThreadJob` `ThreadJob` 実行中のジョブを表すオブジェクトを返します。 Job オブジェクトには、現在実行中の状態を含む、ジョブに関する有用な情報が含まれています。 結果が生成されると、ジョブの結果が収集されます。

### <a name="using-foreach-object--parallel--asjob"></a>`ForEach-Object -Parallel -AsJob` の使用

PowerShell 7.0 によって、コマンドレットに新しいパラメーターが設定されました `ForEach-Object` 。 新しいパラメーターを使用すると、スクリプトブロックを PowerShell ジョブとして並列スレッドで実行できます。

パイプを使用してデータをにパイプすることができ `ForEach-Object -Parallel` ます。 データは、並列で実行されるスクリプトブロックに渡されます。 パラメーターは、 `-AsJob` 並列スレッドごとにジョブオブジェクトを作成します。

次のコマンドは、コマンドにパイプを使用して、各入力値の子ジョブを含むジョブを開始します。 各子ジョブは、 `Write-Output` パイプを使用した入力値を引数としてコマンドを実行します。

```powershell
1..5 | ForEach-Object -Parallel { Write-Output $_ } -AsJob
```

このコマンドは、パイプ処理された `ForEach-Object -Parallel` `PSTaskJob` 各入力値の子ジョブを含むオブジェクトを返します。 Job オブジェクトには、実行状態の子ジョブに関する有用な情報が含まれています。 結果が生成されるときに子ジョブの結果を収集します。

## <a name="how-to-wait-for-a-job-to-complete-and-retrieve-job-results"></a>ジョブの完了を待機してジョブの結果を取得する方法

やなどの PowerShell ジョブコマンドレットを使用して、 `Wait-Job` `Receive-Job` ジョブが完了するのを待ってから、ジョブによって生成されたすべての結果を返すことができます。

次のコマンドは、コマンドを実行するスレッドジョブを開始し、 `Get-Process` コマンドが完了するまで待機します。最後に、コマンドによって生成されたすべてのデータ結果を返します。

```powershell
Start-ThreadJob -ScriptBlock { Get-Process } | Wait-Job | Receive-Job
```

次のコマンドは、パイプ処理された `Write-Output` 各入力に対してコマンドを実行し、すべての子ジョブが完了するまで待機して、最後に子ジョブによって生成されるすべてのデータ結果を返すジョブを開始します。

```powershell
1..5 | ForEach-Object -Parallel { Write-Output $_ } -AsJob | Wait-Job | Receive-Job
```

`Receive-Job`コマンドレットは、子ジョブの結果を返します。

```powershell
1
3
2
4
5
```

各子ジョブは並行して実行されるため、生成される結果の順序は保証されません。

## <a name="thread-job-performance"></a>スレッドジョブのパフォーマンス

スレッドジョブは、他の種類のジョブよりも高速で軽量です。 ただし、ジョブが行っている作業と比べると、大きなオーバーヘッドが発生する可能性があります。

PowerShell は、セッションでコマンドとスクリプトを実行します。 セッションで一度に実行できるコマンドまたはスクリプトは1つだけです。 そのため、複数のジョブを実行する場合、各ジョブは個別のセッションで実行されます。 各セッションは、オーバーヘッドに寄与します。

スレッドジョブは、実行する作業がジョブの実行に使用されるセッションのオーバーヘッドよりも大きい場合に最適なパフォーマンスを提供します。 この条件を満たすには、2つのケースがあります。

- 作業は多くのコンピューティング処理を必要とします。複数のスレッドジョブでスクリプトを実行すると、複数のプロセッサコアを利用し、より高速に完了できます。

- 作業は、i/o またはリモート呼び出しの結果を待機する時間を費やすスクリプトで構成されています。 並列実行は通常、連続して実行した場合よりも短時間で完了します。

```powershell
(Measure-Command {
    1..1000 | ForEach { Start-ThreadJob { Write-Output "Hello $using:_" } } | Receive-Job -Wait
}).TotalMilliseconds
36860.8226

(Measure-Command {
    1..1000 | ForEach-Object { "Hello: $_" }
}).TotalMilliseconds
7.1975
```

上の最初の例は、単純な文字列の書き込みを行うために1000のスレッドジョブを作成する foreach ループを示しています。 ジョブのオーバーヘッドが原因で、完了までに36秒以上かかります。

2番目の例では、 `ForEach` 同じ1000操作を実行するためにコマンドレットを実行します。
この時間は、ジョブのオーバーヘッドを発生させる `ForEach-Object` ことなく、1つのスレッドで順番に実行されます。 わずか7ミリ秒で完了します。

次の例では、10個の個別のシステムログについて最大5000のエントリが収集されます。 このスクリプトには多数のログが含まれているため、操作を並行して実行するのが理にかなっています。

```powershell
$logNames.count
10

Measure-Command {
    $logs = $logNames | ForEach-Object {
        Get-WinEvent -LogName $_ -MaxEvents 5000 2>$null
    }
}

TotalMilliseconds : 252398.4321 (4 minutes 12 seconds)
$logs.Count
50000
```

このスクリプトは、ジョブが並列で実行される半分の時間に完了します。

```powershell
Measure-Command {
    $logs = $logNames | ForEach {
        Start-ThreadJob {
            Get-WinEvent -LogName $using:_ -MaxEvents 5000 2>$null
        } -ThrottleLimit 10
    } | Wait-Job | Receive-Job
}

TotalMilliseconds : 115994.3 (1 minute 56 seconds)
$logs.Count
50000
```

## <a name="thread-jobs-and-variables"></a>スレッドジョブと変数

スレッドベースのジョブには、複数の方法で値を渡すことができます。

`Start-ThreadJob`は、コマンドレットにパイプ処理されるか、キーワードを使用してスクリプトブロックに渡されるか、ArgumentList パラメーターを介して渡される変数を受け入れることができ `$using` ます。 **ArgumentList**

```powershell
$msg = "Hello"

$msg | Start-ThreadJob { $input | Write-Output } | Wait-Job | Receive-Job

Start-ThreadJob { Write-Output $using:msg } | Wait-Job | Receive-Job

Start-ThreadJob { param ([string] $message) Write-Output $message } -ArgumentList @($msg) |
  Wait-Job | Receive-Job
```

`ForEach-Object -Parallel` 変数でパイプを受け取り、キーワードを使用してスクリプトブロックに直接渡された変数を受け入れ `$using` ます。

```powershell
$msg = "Hello"

$msg | ForEach-Object -Parallel { Write-Output $_ } -AsJob | Wait-Job | Receive-Job

1..1 | ForEach-Object -Parallel { Write-Output $using:msg } -AsJob | Wait-Job | Receive-Job
```

スレッドジョブは同じプロセスで実行されるため、ジョブに渡される変数参照型は慎重に扱う必要があります。 スレッドセーフなオブジェクトでない場合は、に割り当てられないようにし、メソッドとプロパティを呼び出さないようにする必要があります。

次の例では、スレッドセーフな .NET `ConcurrentDictionary` オブジェクトをすべての子ジョブに渡して、一意の名前付きプロセスオブジェクトを収集します。 スレッドセーフなオブジェクトであるため、ジョブがプロセス内で同時に実行されている間に安全に使用できます。

```powershell
$threadSafeDictionary = [System.Collections.Concurrent.ConcurrentDictionary[string,object]]::new()
$jobs = Get-Process | ForEach {
    Start-ThreadJob {
        $proc = $using:_
        $dict = $using:threadSafeDictionary
        $dict.TryAdd($proc.ProcessName, $proc)
    }
}
$jobs | Wait-Job | Receive-Job

$threadSafeDictionary.Count
96

$threadSafeDictionary["pwsh"]

NPM(K)  PM(M)   WS(M) CPU(s)    Id SI ProcessName
------  -----   ----- ------    -- -- -----------
  112  108.25  124.43  69.75 16272  1 pwsh
```

## <a name="see-also"></a>関連項目

- [about_Remote_Jobs](about_Remote_Jobs.md)
- [about_Thread_Jobs](about_Thread_Jobs.md)
- [about_Job_Details](about_Job_Details.md)
- [about_Remote](about_Remote.md)
- [about_PSSessions](about_PSSessions.md)
- [Start-Job](xref:Microsoft.PowerShell.Core.Start-Job)
- [Get-Job](xref:Microsoft.PowerShell.Core.Get-Job)
- [Receive-Job](xref:Microsoft.PowerShell.Core.Receive-Job)
- [Stop-Job](xref:Microsoft.PowerShell.Core.Stop-Job)
- [Wait-Job](xref:Microsoft.PowerShell.Core.Wait-Job)
- [Remove-Job](xref:Microsoft.PowerShell.Core.Remove-Job)
