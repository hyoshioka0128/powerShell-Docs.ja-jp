---
title: マルチスレッド処理中の進行状況の表示
description: Foreach-Object -Parallel を用いて複数のスレッド間で Write-Progress を使用する方法
ms.date: 08/02/2020
ms.openlocfilehash: 909fc1bbdeded8845b1955e3384fb55db7173030
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "89410844"
---
# <a name="writing-progress-across-multiple-threads-with-foreach-parallel"></a>Foreach Parallel を使用した複数のスレッド間での進行状況の書き込み

PowerShell 7.0 以降では、[Foreach-Object](/powershell/module/Microsoft.PowerShell.Core/Foreach-Object) コマンドレットで **Parallel** パラメーターを使用して、複数のスレッドで同時に作業できるようになります。 ただし、これらのスレッドの進行状況を監視することは困難な場合があります。 通常は、[Write-Progress](/powershell/module/Microsoft.PowerShell.Utility/Write-Progress) を使用してプロセスの進行状況を監視できます。
ただし、PowerShell では **Parallel** を使用するときにスレッドごとに個別の実行空間が使用されるため、進行状況をホストに報告することは、`Write-Progress` の通常の使用時ほど簡単ではありません。

## <a name="using-a-synced-hashtable-to-track-progress"></a>同期されたハッシュテーブルを使用した進行状況の追跡

複数のスレッドの進行状況を書き込む場合、PowerShell で並列プロセスを実行すると、各プロセスで独自の実行空間が使用されるため、追跡が困難になります。 この問題を回避するには、[同期されたハッシュテーブル](/dotnet/api/system.collections.hashtable.synchronized)を使用できます。 同期されたハッシュテーブルは、スレッド セーフなデータ構造であり、エラーをスローすることなく複数のスレッドによって同時に変更できます。

### <a name="set-up"></a>設定

このアプローチの欠点の 1 つは、すべてがエラーなしで確実に実行されるように、やや複雑な設定となることです。

```powershell
$dataset = @(
    @{
        Id   = 1
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 2
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 3
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 4
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 5
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
)

# Create a hashtable for process.
# Keys should be ID's of the processes
$origin = @{}
$dataset | Foreach-Object {$origin.($_.id) = @{}}

# Create synced hashtable
$sync = [System.Collections.Hashtable]::Synchronized($origin)
```

このセクションでは、3 つの異なる目的で、3 つの異なるデータ構造を作成します。

`$dataSet` 変数には、変更されるリスクを伴わずに次の手順を調整するために使用されるハッシュテーブルの配列が格納されます。 コレクションの反復処理中にオブジェクト コレクションが変更された場合、PowerShell によってエラーがスローされます。 オブジェクト コレクションは、変更対象のオブジェクトとは別のループに保持する必要があります。 各ハッシュテーブルの `Id` キーは、モック プロセスの識別子です。 `Wait` キーにより、追跡されている各モック プロセスのワークロードがシミュレートされます。

`$origin` 変数には、各キーがモック プロセス ID の 1 つである、入れ子になったハッシュテーブルが格納されます。
次に、これは `$sync` 変数に格納されている同期済みのハッシュテーブルをハイドレートするために使用されます。 `$sync` 変数は、進行状況が表示される親実行空間に進行状況を報告する役割を担います。

### <a name="running-the-processes"></a>プロセスの実行

このセクションでは、マルチスレッド プロセスを実行し、進行状況を表示するために使用される出力の一部を作成します。

```powershell
$job = $dataset | Foreach-Object -ThrottleLimit 3 -AsJob -Parallel {
    $syncCopy = $using:sync
    $process = $syncCopy.$($PSItem.Id)

    $process.Id = $PSItem.Id
    $process.Activity = "Id $($PSItem.Id) starting"
    $process.Status = "Processing"

    # Fake workload start up that takes x amount of time to complete
    start-sleep -Milliseconds ($PSItem.wait*5)

    # Process. update activity
    $process.Activity = "Id $($PSItem.id) processing"
    foreach ($percent in 1..100)
    {
        # Update process on status
        $process.Status = "Handling $percent/100"
        $process.PercentComplete = (($percent / 100) * 100)

        # Fake workload that takes x amount of time to complete
        Start-Sleep -Milliseconds $PSItem.Wait
    }

    # Mark process as completed
    $process.Completed = $true
}
```

モック プロセスは `Foreach-Object` に送信され、ジョブとして開始されます。 **ThrottleLimit** は **3** に設定されて、キュー内の複数のプロセスの実行が強調されます。 ジョブは `$job` 変数に格納され、これにより、後ですべてのプロセスが完了したときに知ることができます。

`using:` ステートメントを使用して PowerShell で親スコープ変数を参照する場合、式を使用してこれを動的にすることはできません。 たとえば、`$process = $using:sync.$($PSItem.id)` のような `$process` 変数を作成しようとすると、ここで式を使用できないことを示すエラーが表示されます。 そのため、エラーが表示されることなく `$sync` 変数を参照および変更できるように、`$syncCopy` 変数を作成します。

次に、同期されたハッシュテーブル キーを参照することで、`$process` 変数を使用して現在ループ内にあるプロセスの進行状況を表すハッシュテーブルを作成します。 **Activity** および **Status** キーは、次のセクションで特定のモック プロセスの状態を表示するために `Write-Progress` のパラメーター値として使用されます。

`foreach` ループは、プロセスの動作をシミュレートする 1 つの方法であり、`$dataSet` **Wait** 属性に基づいてランダム化され、ミリ秒を使用して `Start-Sleep` が設定されます。 プロセスの進行状況を計算する方法は異なる場合があります。

### <a name="displaying-the-progress-of-multiple-processes"></a>複数のプロセスの進行状況の表示

これで、モック プロセスがジョブとして実行されるようになったので、プロセスの進行状況を PowerShell ウィンドウに書き込み始めることができます。

```powershell
while($job.State -eq 'Running')
{
    $sync.Keys | Foreach-Object {
        # If key is not defined, ignore
        if(![string]::IsNullOrEmpty($sync.$_.keys))
        {
            # Create parameter hashtable to splat
            $param = $sync.$_

            # Execute Write-Progress
            Write-Progress @param
        }
    }

    # Wait to refresh to not overload gui
    Start-Sleep -Seconds 0.1
}
```

`$job` 変数には親 **ジョブ** が含まれており、モック プロセスごとに子 **ジョブ** が存在します。 子ジョブがまだ実行されている間、親ジョブの **State** は "Running" のままになります。 これにより、すべてのプロセスが完了するまで、`while` ループを使用してすべてのプロセスの進行状況を継続的に更新できます。

while ループ内で、`$sync` 変数内の各キーをループ処理します。 これは同期されたハッシュテーブルであるため、常に更新されますが、依然としてエラーがスローされることなくアクセスすることができます。

報告されるプロセスが `IsNullOrEmpty()` メソッドを使用して実際に実行されていることが確認されます。 プロセスが開始されていない場合、ループではそれが報告されず、開始されたプロセスに達するまで次のプロセスに移動します。 プロセスが開始されている場合は、現在のキーのハッシュテーブルを使用して、パラメーターが `Write-Progress` にスプラッティングされます。

### <a name="full-example"></a>完全な例

```powershell
# Example workload
$dataset = @(
    @{
        Id   = 1
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 2
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 3
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 4
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
    @{
        Id   = 5
        Wait = 3..10 | get-random | Foreach-Object {$_*100}
    }
)

# Create a hashtable for process.
# Keys should be ID's of the processes
$origin = @{}
$dataset | Foreach-Object {$origin.($_.id) = @{}}

# Create synced hashtable
$sync = [System.Collections.Hashtable]::Synchronized($origin)

$job = $dataset | Foreach-Object -ThrottleLimit 3 -AsJob -Parallel {
    $syncCopy = $using:sync
    $process = $syncCopy.$($PSItem.Id)

    $process.Id = $PSItem.Id
    $process.Activity = "Id $($PSItem.Id) starting"
    $process.Status = "Processing"

    # Fake workload start up that takes x amount of time to complete
    start-sleep -Milliseconds ($PSItem.wait*5)

    # Process. update activity
    $process.Activity = "Id $($PSItem.id) processing"
    foreach ($percent in 1..100)
    {
        # Update process on status
        $process.Status = "Handling $percent/100"
        $process.PercentComplete = (($percent / 100) * 100)

        # Fake workload that takes x amount of time to complete
        Start-Sleep -Milliseconds $PSItem.Wait
    }

    # Mark process as completed
    $process.Completed = $true
}

while($job.State -eq 'Running')
{
    $sync.Keys | Foreach-Object {
        # If key is not defined, ignore
        if(![string]::IsNullOrEmpty($sync.$_.keys))
        {
            # Create parameter hashtable to splat
            $param = $sync.$_

            # Execute Write-Progress
            Write-Progress @param
        }
    }

    # Wait to refresh to not overload gui
    Start-Sleep -Seconds 0.1
}
```

## <a name="related-links"></a>関連リンク

- [about_Jobs](/powershell/module/Microsoft.PowerShell.Core/About/about_Jobs)
- [about_Scopes](/powershell/module/Microsoft.PowerShell.Core/About/about_Scopes)
- [about_Splatting](/powershell/module/Microsoft.PowerShell.Core/About/about_Splatting)
