---
external help file: Microsoft.PowerShell.ThreadJob.dll-Help.xml
Locale: en-US
Module Name: ThreadJob
ms.date: 12/05/2020
online version: https://docs.microsoft.com/powershell/module/threadjob/start-threadjob?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Start-ThreadJob
ms.openlocfilehash: d08f883b232abadeacb445e5ba542b4b1fae023b
ms.sourcegitcommit: f9d855dd73b916559a22e337672dea3fbb11c634
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "99604928"
---
# Start-ThreadJob

## 概要
コマンドレットに似たバックグラウンドジョブを作成し `Start-Job` ます。

## SYNTAX

### スクリプト ブロック

```
Start-ThreadJob [-ScriptBlock] <ScriptBlock> [-Name <String>] [-InitializationScript <ScriptBlock>]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] [-ThrottleLimit <Int32>]
 [-StreamingHost <PSHost>] [<CommonParameters>]
```

### FilePath

```
Start-ThreadJob [-FilePath] <String> [-Name <String>] [-InitializationScript <ScriptBlock>]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] [-ThrottleLimit <Int32>]
 [-StreamingHost <PSHost>] [<CommonParameters>]
```

## Description

`Start-ThreadJob` コマンドレットに似たバックグラウンドジョブを作成し `Start-Job` ます。 主な違いは、作成されるジョブはローカルプロセス内の別々のスレッドで実行されるという点です。 既定では、ジョブはジョブを開始した呼び出し元の現在の作業ディレクトリを使用します。

コマンドレットでは、同時に実行されるジョブの数を制限する **ThrottleLimit** パラメーターもサポートされています。 ジョブが開始されると、キューに登録され、現在のジョブ数がスロットル制限を下回るまで待機します。

## 例

### 例 1-スレッドの制限が2のバックグラウンドジョブを作成する

```powershell
Start-ThreadJob -ScriptBlock { 1..100 | % { sleep 1; "Output $_" } } -ThrottleLimit 2
Start-ThreadJob -ScriptBlock { 1..100 | % { sleep 1; "Output $_" } }
Start-ThreadJob -ScriptBlock { 1..100 | % { sleep 1; "Output $_" } }
Get-Job
```

```Output
Id   Name   PSJobTypeName   State        HasMoreData   Location     Command
--   ----   -------------   -----        -----------   --------     -------
1    Job1   ThreadJob       Running      True          PowerShell   1..100 | % { sleep 1;...
2    Job2   ThreadJob       Running      True          PowerShell   1..100 | % { sleep 1;...
3    Job3   ThreadJob       NotStarted   False         PowerShell   1..100 | % { sleep 1;...
```

### 例 2-Start-Job と Start-ThreadJob のパフォーマンスを比較する

この例は、との違いを示して `Start-Job` `Start-ThreadJob` います。 このコマンドレットは、ジョブによって `Start-Sleep` 1 秒間実行されます。 ジョブは並行して実行されるため、合計実行時間は約1秒で、ジョブの作成に必要な時間もあります。

```powershell
# start five background jobs each running 1 second
Measure-Command {1..5 | % {Start-Job {Start-Sleep 1}} | Wait-Job} | Select-Object TotalSeconds
Measure-Command {1..5 | % {Start-ThreadJob {Start-Sleep 1}} | Wait-Job} | Select-Object TotalSeconds
```

```Output
TotalSeconds
------------
   5.7665849
   1.5735008
```

実行時間が1秒を引いた後は、 `Start-Job` 5 つのジョブを作成するのに約4.8 秒かかることがわかります。 `Start-ThreadJob` は8倍高速です。5つのジョブを作成するには約0.6 秒かかります。 結果は環境によって異なる場合がありますが、相対的な改善は同じである必要があります。

### 例 3-InputObject を使用してジョブを作成する

この例では、スクリプトブロックは変数を使用して `$input` **InputObject** パラメーターから入力を受け取ります。 これは、オブジェクトをにパイプ処理することによっても実行でき `Start-ThreadJob` ます。

```powershell
$j = Start-ThreadJob -InputObject (Get-Process pwsh) -ScriptBlock { $input | Out-String }
$j | Wait-Job | Receive-Job
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     94   145.80     159.02      18.31   18276   1 pwsh
    101   163.30     222.05      29.00   35928   1 pwsh
```

```powershell
$j = Get-Process pwsh | Start-ThreadJob -ScriptBlock { $input | Out-String }
$j | Wait-Job | Receive-Job
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     94   145.80     159.02      18.31   18276   1 pwsh
    101   163.30     222.05      29.00   35928   1 pwsh
```

### 例 4-親ホストへのジョブ出力のストリーミング

**Streaminghost** パラメーターを使用すると、すべてのホスト出力を特定のホストに転送するようにジョブに指示できます。 このパラメーターを指定しない場合、出力はジョブのデータストリームコレクションに送信され、ジョブからの出力を受け取るまではホストコンソールに表示されません。

この例では、 `Start-ThreadJob` 自動変数を使用して、現在のホストをに渡し `$Host` ます。

```powershell
PS> Start-ThreadJob -ScriptBlock { Read-Host 'Say hello'; Write-Warning 'Warning output' } -StreamingHost $Host

Id   Name   PSJobTypeName   State         HasMoreData     Location      Command
--   ----   -------------   -----         -----------     --------      -------
7    Job7   ThreadJob       NotStarted    False           PowerShell    Read-Host 'Say hello'; �

PS> Say hello: Hello
WARNING: Warning output
PS> Receive-Job -Id 7
Hello
WARNING: Warning output
PS>
```

プロンプト `Read-Host` が表示され、入力を入力できることに注意してください。 次に、からのメッセージ `Write-Warning` が表示されます。 `Receive-Job`コマンドレットは、ジョブからのすべての出力を返します。

## PARAMETERS

### -ArgumentList

**FilePath** または **ScriptBlock** パラメーターで指定されたスクリプトの引数の配列、またはパラメーター値を指定します。

**ArgumentList** は、コマンドラインの最後のパラメーターである必要があります。 パラメーター名の後に続くすべての値は、引数リスト内の値として解釈されます。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

バックグラウンドジョブとして実行するスクリプトファイルを指定します。 スクリプトのパスとファイル名を入力します。 スクリプトは、ローカルコンピューター上、またはローカルコンピューターがアクセスできるフォルダー内にある必要があります。

このパラメーターを使用すると、PowerShell によって、指定したスクリプトファイルの内容がスクリプトブロックに変換され、バックグラウンドジョブとしてスクリプトブロックが実行されます。

```yaml
Type: System.String
Parameter Sets: FilePath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InitializationScript

ジョブの開始前に実行するコマンドを指定します。 コマンドを中かっこ () で囲み、 `{}` スクリプトブロックを作成します。

このパラメーターは、ジョブを実行するセッションを準備するために使用します。 たとえば、このメソッドを使用して、関数とモジュールをセッションに追加できます。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

スクリプトブロックへの入力として使用されるオブジェクトを指定します。 また、パイプラインを入力することもできます。 `$input`入力オブジェクトにアクセスするには、スクリプトブロックで自動変数を使用します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

新しいジョブのフレンドリ名を指定します。 この名前を使用して、コマンドレットなど、他のジョブコマンドレットに対してジョブを識別でき `Stop-Job` ます。

既定のフレンドリ名は "Job #" です。ここで、"#" は、ジョブごとに増加する序数です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptBlock

バックグラウンド ジョブで実行するコマンドを指定します。 コマンドを中かっこ () で囲み、 `{}` スクリプトブロックを作成します。 `$Input` **InputObject** パラメーターの値にアクセスするには、自動変数を使用します。 このパラメーターは必須です。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ScriptBlock
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -StreamingHost

このパラメーターは、 `Write-Host` 渡された **PSHost** オブジェクトに直接出力を送信できるようにするスレッドセーフな方法を提供します。 このファイルを使用しない場合、 `Write-Host` 出力はジョブ情報データストリームコレクションに送信され、ジョブの実行が完了するまではホストコンソールに表示されません。

```yaml
Type: System.Management.Automation.Host.PSHost
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ThrottleLimit

このパラメーターは、一度に実行されるジョブの数を制限します。 ジョブが開始されると、キューに入れられ、スレッドプールでスレッドを使用してジョブを実行できるようになるまで待機します。 既定の制限は5スレッドです。

スレッドプールのサイズは、PowerShell セッションに対してグローバルです。 1回の呼び出しで **ThrottleLimit** を指定すると、同じセッション内の後続の呼び出しの制限が設定されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 5
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

## 出力

### ThreadJob。 ThreadJob

## 注

## 関連リンク

[Start-Job](../Microsoft.PowerShell.Core/Start-Job.md)

[Stop-Job](../Microsoft.PowerShell.Core/Stop-Job.md)

[Receive-Job](../Microsoft.PowerShell.Core/Receive-Job.md)
