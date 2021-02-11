---
ms.date: 09/13/2016
ms.topic: reference
title: バックグラウンド ジョブ
description: バックグラウンド ジョブ
ms.openlocfilehash: 5478789a2ee1f2eabc71a46673e3a707643cdba8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648625"
---
# <a name="background-jobs"></a>バックグラウンド ジョブ

コマンドレットは、内部で、または Windows PowerShell の *バックグラウンドジョブ* としてアクションを実行できます。 コマンドレットをバックグラウンドジョブとして実行すると、その作業は、コマンドレットが使用しているパイプラインスレッドとは別に、独自のスレッドで非同期に実行されます。 ユーザーの観点からは、コマンドレットをバックグラウンドジョブとして実行すると、ジョブの完了に時間がかかり、ジョブの実行中にユーザーが中断せずに続行できる場合でも、コマンドプロンプトが直ちに返されます。

## <a name="background-jobs-child-jobs-and-the-job-repository"></a>バックグラウンドジョブ、子ジョブ、およびジョブリポジトリ

バックグラウンドジョブをサポートするコマンドレットによって返されるジョブオブジェクトは、ジョブを定義します。 ( [Start ジョブ](/powershell/module/Microsoft.PowerShell.Core/Start-Job) コマンドレットは、ジョブオブジェクトも返します)。ジョブの名前、ジョブを指定するために使用される識別子、状態情報、および子ジョブがこの定義に含まれます。 このジョブでは、作業は実行されません。 子ジョブによって実際の作業が実行されるため、各バックグラウンドジョブには少なくとも1つの子ジョブがあります。 作業がバックグラウンドジョブとして実行されるようにコマンドレットを実行する場合、このコマンドレットはジョブと子ジョブを、 *ジョブリポジトリ* と呼ばれる共通リポジトリに追加する必要があります。

コマンドラインでのバックグラウンドジョブの処理方法の詳細については、次を参照してください。

- [about_Jobs](/powershell/module/microsoft.powershell.core/about/about_jobs)

- [about_Job_Details](/powershell/module/microsoft.powershell.core/about/about_job_details)

- [about_Remote_Jobs](/powershell/module/microsoft.powershell.core/about/about_remote_jobs)

## <a name="writing-a-cmdlet-that-runs-as-a-background-job"></a>バックグラウンドジョブとして実行されるコマンドレットの作成

バックグラウンドジョブとして実行できるコマンドレットを作成するには、次のタスクを完了する必要があります。

- `asJob`コマンドレットをバックグラウンドジョブとして実行するかどうかをユーザーが決定できるように、スイッチパラメーターを定義します。

- System.object [クラスから派生した](/dotnet/api/System.Management.Automation.Job) オブジェクトを作成します。 このオブジェクトには、カスタムジョブオブジェクト、または Windows PowerShell によって提供されるジョブオブジェクト ( [Pseventjob](/dotnet/api/System.Management.Automation.PSEventJob) オブジェクトなど) を指定できます。

- レコード処理メソッドで、 `if` コマンドレットをバックグラウンドジョブとして実行する必要があるかどうかを検出するステートメントを追加します。

- カスタムジョブオブジェクトの場合は、job クラスを実装します。

- コマンドレットがバックグラウンドジョブとして実行されているかどうかに応じて、適切なオブジェクトを返します。

コード例については、「 [ジョブをサポートする方法](./how-to-support-jobs.md)」を参照してください。

## <a name="background-job-related-apis"></a>バックグラウンド Job-Related Api

Windows PowerShell では、バックグラウンドジョブを管理するために次の Api が提供されています。

System.string はカスタムジョブオブジェクト[を派生さ](/dotnet/api/System.Management.Automation.Job)せるものです。 これは抽象クラスです。

現在アクティブなバックグラウンドジョブに関する情報[を管理および](/dotnet/api/System.Management.Automation.JobRepository)提供します。

[システムの管理. Jobstate](/dotnet/api/System.Management.Automation.JobState) は、バックグラウンドジョブの状態を定義します。 状態には、開始、実行中、停止が含まれます。

[システムの管理. Jobstateinfo](/dotnet/api/System.Management.Automation.JobStateInfo) は、バックグラウンドジョブの状態に関する情報を提供します。最後の状態変更がエラーによって発生した場合は、ジョブが現在の状態になった理由を示します。

" [System. Automation. Jobstateeventargs](/dotnet/api/System.Management.Automation.JobStateEventArgs) " は、バックグラウンドジョブが状態を変更したときに発生するイベントの引数を提供します。

## <a name="windows-powershell-job-cmdlets"></a>Windows PowerShell ジョブのコマンドレット

次のコマンドレットは、Windows PowerShell によってバックグラウンドジョブを管理するために用意されています。

[Get-Job](/powershell/module/Microsoft.PowerShell.Core/Get-Job)

現在のセッションで実行されている Windows PowerShell のバックグラウンド ジョブを取得します。

[Receive-Job](/powershell/module/Microsoft.PowerShell.Core/Receive-Job)

現在のセッションの Windows PowerShell バックグラウンド ジョブの結果を取得します。

[Remove-Job](/powershell/module/Microsoft.PowerShell.Core/Remove-Job)

Windows PowerShell バックグラウンド ジョブを削除します。

[Start-Job](/powershell/module/Microsoft.PowerShell.Core/Start-Job)

Windows PowerShell バックグラウンド ジョブを開始します。

[Stop-Job](/powershell/module/Microsoft.PowerShell.Core/Stop-Job)

Windows PowerShell バックグラウンド ジョブを停止します。

[Wait-Job](/powershell/module/Microsoft.PowerShell.Core/Wait-Job)

セッションで実行されている 1 つまたはすべての Windows PowerShell のバックグラウンド ジョブが完了するまでは、コマンド プロンプトが表示されないようにします。

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
