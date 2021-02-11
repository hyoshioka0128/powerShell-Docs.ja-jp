---
description: PowerShell バックグラウンドジョブが、現在のセッションと対話せずにバックグラウンドでコマンドまたは式を実行する方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_jobs?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Jobs
ms.openlocfilehash: 51409a124363d21f540459d49eb7e08a7c70d39a
ms.sourcegitcommit: aac365f7813756e16b59322832a904e703e0465b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2020
ms.locfileid: "94524860"
---
# <a name="about-jobs"></a>ジョブについて

## <a name="short-description"></a>簡単な説明
PowerShell バックグラウンドジョブが、現在のセッションと対話せずにバックグラウンドでコマンドまたは式を実行する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell は、ジョブを使用してコマンドとスクリプトを同時に実行します。 同時実行をサポートするために PowerShell によって提供されるジョブの種類は3つあります。

- `RemoteJob` -コマンドとスクリプトは、リモートセッションで実行されます。 詳細については、「 [about_Remote_Jobs](about_Remote_Jobs.md)」を参照してください。
- `BackgroundJob` -コマンドとスクリプトは、ローカルコンピューター上で個別のプロセスで実行されます。
- `PSTaskJob` または `ThreadJob` -コマンドとスクリプトは、ローカルコンピューター上の同じプロセス内の別のスレッドで実行されます。 詳細については、「 [about_Thread_Jobs](/powershell/module/ThreadJob/about_Thread_Jobs)」を参照してください。

別のコンピューターまたは別のプロセスでスクリプトをリモートで実行すると、優れた分離が実現します。 リモートジョブで発生したエラーは、他の実行中のジョブ、またはジョブを開始した親セッションには影響しません。 ただし、リモート処理層では、オブジェクトのシリアル化などのオーバーヘッドが発生します。 すべてのオブジェクトは、親セッションとリモート (ジョブ) セッションの間で渡されるときにシリアル化および逆シリアル化されます。 大きな複雑なデータオブジェクトのシリアル化は、大量のコンピューティングリソースとメモリリソースを消費し、ネットワーク経由で大量のデータを転送することがあります。

スレッドベースのジョブは、異なるスレッドで同じプロセスで実行されるため、リモートジョブやバックグラウンドジョブほど堅牢ではありません。 あるジョブで、プロセスをクラッシュさせる重大なエラーが発生している場合は、プロセス内の他のすべてのジョブが終了します。

ただし、スレッドベースのジョブではオーバーヘッドが少なくなります。 これらは、リモート処理レイヤーまたはシリアル化を使用しません。 結果オブジェクトは、現在のセッションのライブオブジェクトへの参照として返されます。 このオーバーヘッドがないと、スレッドベースのジョブはより高速に実行され、他の種類のジョブよりも使用されるリソースが少なくなります。

> [!IMPORTANT]
> ジョブを作成した親セッションもジョブの状態を監視し、パイプラインデータを収集します。 ジョブが完了状態になると、親プロセスによってジョブの子プロセスが終了します。 親セッションが終了すると、実行中のすべての子ジョブが子プロセスと共に終了します。

この状況に対処するには、次の2つの方法があります。

1. `Invoke-Command`切断されたセッションで実行されるジョブを作成するには、を使用します。 詳細については、「 [about_Remote_Jobs](about_Remote_Jobs.md)」を参照してください。
1. `Start-Process`ジョブではなく新しいプロセスを作成するには、を使用します。 詳細については、「 [Start-Process](xref:Microsoft.PowerShell.Management.Start-Process)」を参照してください。

## <a name="the-job-cmdlets"></a>ジョブのコマンドレット

|コマンドレット          |説明                                            |
|----------------|-------------------------------------------------------|
|`Start-Job`     |ローカルコンピューターでバックグラウンドジョブを開始します。           |
|`Get-Job`       |で開始されたバックグラウンドジョブを取得します。      |
|                |現在のセッション。                                       |
|`Receive-Job`   |バックグラウンドジョブの結果を取得します。                   |
|`Stop-Job`      |バックグラウンドジョブを停止します。                                |
|`Wait-Job`      |1つまたはすべてのジョブが完了するまで、コマンドプロンプトを表示しません。|
|                |完了.                                              |
|`Remove-Job`    |バックグラウンドジョブを削除します。                              |
|`Invoke-Command`|**AsJob** パラメーターは、に対してバックグラウンドジョブを作成します。  |
|                |リモートコンピューター。 を使用して、 `Invoke-Command`   |
|                |を含め、任意のジョブコマンドをリモートで実行 `Start-Job` します。       |

## <a name="how-to-start-a-job-on-the-local-computer"></a>ローカルコンピューターでジョブを開始する方法

ローカルコンピューターでバックグラウンドジョブを開始するには、コマンドレットを使用し `Start-Job` ます。

コマンドを記述するには、 `Start-Job` ジョブを実行するコマンドを中かっこ ( `{}` ) で囲みます。 **ScriptBlock** パラメーターを使用して、コマンドを指定します。

次のコマンドは、ローカルコンピューターでコマンドを実行するバックグラウンドジョブを開始し `Get-Process` ます。

```powershell
Start-Job -ScriptBlock {Get-Process}
```

バックグラウンドジョブを開始すると、ジョブが完了するまでに時間がかかる場合でも、コマンドプロンプトがすぐに返されます。 ジョブの実行中は、中断されることなく引き続きセッションで作業できます。

この `Start-Job` コマンドは、ジョブを表すオブジェクトを返します。 ジョブ オブジェクトには、ジョブに関する有用な情報が含まれていますが、ジョブの結果は含まれません。

ジョブオブジェクトを変数に保存し、他の **ジョブ** コマンドレットと共に使用してバックグラウンドジョブを管理できます。 次のコマンドは、ジョブオブジェクトを開始し、結果として生成されたジョブオブジェクトを変数に保存し `$job` ます。

```powershell
$job = Start-Job -ScriptBlock {Get-Process}
```

PowerShell 6.0 以降では、パイプラインの最後にバックグラウンド演算子 () を使用して、 `&` バックグラウンドジョブを開始できます。 詳細については、「 [background operator](about_Operators.md#background-operator-)」を参照してください。

Background 演算子を使用することは、前の例のコマンドレットを使用するのと機能的には同じです `Start-Job` 。

```powershell
$job = Get-Process &
```

## <a name="getting-job-objects"></a>ジョブオブジェクトの取得

`Get-Job`コマンドレットは、現在のセッションで開始されたバックグラウンドジョブを表すオブジェクトを返します。 パラメーターを指定しない場合は、 `Get-Job` 現在のセッションで開始されたすべてのジョブが返されます。

```powershell
Get-Job
```

ジョブオブジェクトには、ジョブが完了したかどうかを示すジョブの状態が含まれています。 完了したジョブの状態が " **完了** " または " **失敗** " になっています。 ジョブも **ブロック** されているか、実行され **ている** 可能性があります。

```Output
Id  Name  PSJobTypeName State      HasMoreData  Location   Command
--  ----  ------------- -----      -----------  --------   -------
1   Job1  BackgroundJob Complete   True         localhost  Get-Process
```

ジョブオブジェクトを変数に保存し、それを使用して後のコマンドでジョブを表すことができます。 次のコマンドは、ID が1のジョブを取得し、変数に保存し `$job` ます。

```powershell
$job = Get-Job -Id 1
```

## <a name="getting-the-results-of-a-job"></a>ジョブの結果を取得する

バックグラウンドジョブを実行しても、結果はすぐには表示されません。 バックグラウンドジョブの結果を取得するには、コマンドレットを使用し `Receive-Job` ます。

次の例では、 `Receive-Job` コマンドレットは、変数内の job オブジェクトを使用してジョブの結果を取得し `$job` ます。

```powershell
Receive-Job -Job $job
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)    Id ProcessName
-------  ------    -----      ----- -----   ------    -- -----------
    103       4    11328       9692    56           1176 audiodg
    804      14    12228      14108   100   101.74  1740 CcmExec
    668       7     2672       6168   104    32.26   488 csrss
...
```

ジョブの結果を変数に保存できます。 次のコマンドは、変数内のジョブの結果を `$job` 変数に保存し `$results` ます。

```powershell
$results = Receive-Job -Job $job
```

### <a name="getting-and-keeping-partial-job-results"></a>部分的なジョブ結果の取得と保持

コマンドレットでは、 `Receive-Job` バックグラウンドジョブの結果を取得します。 ジョブが完了すると、は `Receive-Job` すべてのジョブの結果を取得します。 ジョブがまだ実行中の場合は、 `Receive-Job` これまでに生成された結果を取得します。 `Receive-Job`コマンドをもう一度実行して、残りの結果を取得することができます。

既定では、は、 `Receive-Job` ジョブの結果が格納されているキャッシュから結果を削除します。 もう一度を実行すると `Receive-Job` 、最初の実行後に到着した新しい結果だけが取得されます。

次のコマンドは、 `Receive-Job` ジョブが完了する前に実行されたコマンドの結果を示しています。

```powershell
C:\PS> Receive-Job -Job $job

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    103       4    11328       9692    56            1176 audiodg
    804      14    12228      14108   100   101.74   1740 CcmExec

C:\PS> Receive-Job -Job $job

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    68       3     2632        664    29     0.36   1388 ccmsetup
   749      22    21468      19940   203   122.13   3644 communicator
   905       7     2980       2628    34   197.97    424 csrss
  1121      25    28408      32940   174   430.14   3048 explorer
```

返され **Keep** `Receive-Job` たジョブの結果がから削除されないようにするには、Keep パラメーターを使用します。 次のコマンドは、まだ完了していないジョブで **Keep** パラメーターを使用した場合の影響を示しています。

```powershell
C:\PS> Receive-Job -Job $job -Keep

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    103       4    11328       9692    56            1176 audiodg
    804      14    12228      14108   100   101.74   1740 CcmExec

C:\PS> Receive-Job -Job $job -Keep

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    103       4    11328       9692    56            1176 audiodg
    804      14    12228      14108   100   101.74   1740 CcmExec
     68       3     2632        664    29     0.36   1388 ccmsetup
    749      22    21468      19940   203   122.13   3644 communicator
    905       7     2980       2628    34   197.97    424 csrss
   1121      25    28408      32940   174   430.14   3048 explorer
```

### <a name="waiting-for-the-results"></a>結果を待機しています

完了までに時間がかかるコマンドを実行する場合は、ジョブオブジェクトのプロパティを使用して、ジョブがいつ完了したかを判断できます。 次のコマンドでは、オブジェクトを使用して、 `Get-Job` 現在のセッションのすべてのバックグラウンドジョブを取得します。

```powershell
Get-Job
```

結果はテーブルに表示されます。 ジョブの状態が [ **状態** 列に表示されます。

```Output
Id Name  PSJobTypeName State    HasMoreData Location  Command
-- ----  ------------- -----    ----------- --------  -------
1  Job1  BackgroundJob Complete True        localhost Get-Process
2  Job2  BackgroundJob Running  True        localhost Get-EventLog -Log ...
3  Job3  BackgroundJob Complete True        localhost dir -Path C:\* -Re...
```

この場合、 **State** プロパティは、ジョブ2がまだ実行されていることを示します。 コマンドレットを使用して `Receive-Job` ジョブの結果を取得した場合、結果は不完全になります。 `Receive-Job`コマンドレットを繰り返し使用して、すべての結果を取得することができます。 **State** プロパティを使用して、ジョブがいつ完了したかを確認します。

コマンドレットの **Wait** パラメーターを使用することもでき `Receive-Job` ます。 使用時にこのパラメーターを使用すると、コマンドレットは、ジョブが完了し、すべての結果を使用できるようになるまで、コマンドプロンプトを返しません。

また、コマンドレットを使用して、 `Wait-Job` ジョブの結果のいずれかまたはすべてを待機することもできます。 `Wait-Job` 1つ以上の特定のジョブまたはすべてのジョブを待機できます。
次のコマンドは、 `Wait-Job` コマンドレットを使用して、 **ID** を持つジョブを待機します。
10.

```powershell
Wait-Job -ID 10
```

その結果、ジョブが完了するまで PowerShell プロンプトは表示されません。

事前に定義した時間が経過するまで待機することもできます。 このコマンドでは、 **Timeout** パラメーターを使用して、待機時間を120秒に制限します。 時間が経過すると、コマンドプロンプトは返されますが、ジョブはバックグラウンドで実行され続けます。

```powershell
Wait-Job -ID 10 -Timeout 120
```

## <a name="stopping-a-job"></a>ジョブの停止

バックグラウンドジョブを停止するには、 `Stop-Job` コマンドレットを使用します。 次のコマンドは、システムイベントログ内のすべてのエントリを取得するジョブを開始します。 ジョブオブジェクトは変数に保存され `$job` ます。

```powershell
$job = Start-Job -ScriptBlock {Get-EventLog -Log System}
```

次のコマンドは、ジョブを停止します。 パイプライン演算子 () を使用して `|` 、変数内のジョブをに送信し `$job` `Stop-Job` ます。

```powershell
$job | Stop-Job
```

## <a name="deleting-a-job"></a>ジョブの削除

バックグラウンドジョブを削除するには、 `Remove-Job` コマンドレットを使用します。 次のコマンドは、変数内のジョブを削除し `$job` ます。

```powershell
Remove-Job -Job $job
```

## <a name="investigating-a-failed-job"></a>失敗したジョブの調査

ジョブは、さまざまな理由で失敗する可能性があります。 job オブジェクトには、失敗の原因に関する情報を含む **Reason** プロパティが含まれています。

次の例では、必要な資格情報を使用せずにジョブを開始します。

```powershell
$job = Start-Job -ScriptBlock {New-Item -Path HKLM:\Software\MyCompany}
Get-Job $job

Id Name  PSJobTypeName State  HasMoreData  Location  Command
-- ----  ------------- -----  -----------  --------  -------
1  Job1  BackgroundJob Failed False        localhost New-Item -Path HKLM:...
```

**Reason** プロパティを調べて、ジョブが失敗する原因となったエラーを見つけます。

```powershell
$job.ChildJobs[0].JobStateInfo.Reason
```

この場合、リモートコンピューターでコマンドを実行するための明示的な資格情報が必要であったため、ジョブが失敗しました。 **Reason** プロパティには、次のメッセージが含まれています。

> リモートサーバーに接続できませんでした。次のエラーメッセージが表示されました: "アクセスが拒否されました。"

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
- [Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)
