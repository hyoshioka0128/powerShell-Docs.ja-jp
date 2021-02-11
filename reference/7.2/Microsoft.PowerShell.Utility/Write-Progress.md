---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-progress?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Progress
ms.openlocfilehash: 2e2bd5474198745d72ad2b87c3c305695a198f22
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601000"
---
# Write-Progress

## 概要
PowerShell コマンドウィンドウ内の進行状況バーを表示します。

## SYNTAX

```
Write-Progress [-Activity] <String> [[-Status] <String>] [[-Id] <Int32>] [-PercentComplete <Int32>]
 [-SecondsRemaining <Int32>] [-CurrentOperation <String>] [-ParentId <Int32>] [-Completed] [-SourceId <Int32>]
 [<CommonParameters>]
```

## Description

コマンド `Write-Progress` レットは、実行中のコマンドまたはスクリプトの状態を示す進行状況バーを PowerShell コマンドウィンドウに表示します。 バーに示されるインジケーターと、進行状況バーの上部および下部に表示されるテキストを選択できます。

## 例

### 例 1: For ループの進行状況を表示する

```powershell
for ($i = 1; $i -le 100; $i++ )
{
    Write-Progress -Activity "Search in Progress" -Status "$i% Complete:" -PercentComplete $i;
}
```

このコマンドは、1 から 100 をカウントする For ループの進行状況を表示します。

コマンドレットには、 `Write-Progress` ステータスバーの見出し `Activity` 、ステータス行、および `$i` タスクの相対的な完全性を示す変数 (for ループのカウンター) が含まれています。

### 例 2: 入れ子になった For ループの進行状況を表示する

```powershell
for($I = 1; $I -lt 101; $I++ )
{
    Write-Progress -Activity Updating -Status 'Progress->' -PercentComplete $I -CurrentOperation OuterLoop
    for($j = 1; $j -lt 101; $j++ )
    {
        Write-Progress -Id 1 -Activity Updating -Status 'Progress' -PercentComplete $j -CurrentOperation InnerLoop
    }
}
```

```Output
Updating
Progress ->
 [ooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo]
OuterLoop
Updating
Progress
 [oooooooooooooooooo                                                   ]
InnerLoop
```

この例は、2 つの入れ子になった For ループの進行状況をそれぞれ対応する進行状況バーに表示します。

`Write-Progress`2 番目の進行状況バーのコマンドには、最初の進行状況バーと区別する **Id** パラメーターが含まれています。

**Id** パラメーターを指定しない場合、進行状況バーは、もう一方の下に表示されるのではなく、互いに重ねて表示されます。

### 例 3: 文字列の検索中に進行状況を表示する

```powershell
# Use Get-EventLog to get the events in the System log and store them in the $Events variable.
$Events = Get-EventLog -LogName system
# Pipe the events to the ForEach-Object cmdlet.
$Events | ForEach-Object -Begin {
    # In the Begin block, use Clear-Host to clear the screen.
    Clear-Host
    # Set the $i counter variable to zero.
    $i = 0
    # Set the $out variable to a empty string.
    $out = ""
} -Process {
    # In the Process script block search the message property of each incoming object for "bios".
    if($_.message -like "*bios*")
    {
        # Append the matching message to the out variable.
        $out=$out + $_.Message
    }
    # Increment the $i counter variable which is used to create the progress bar.
    $i = $i+1
    # Use Write-Progress to output a progress bar.
    # The Activity and Status parameters create the first and second lines of the progress bar heading, respectively.
    Write-Progress -Activity "Searching Events" -Status "Progress:" -PercentComplete ($i/$Events.count*100)
} -End {
    # Display the matching messages using the out variable.
    $out
}
```

このコマンドは、System イベント ログ内で "bios" という文字列を検索するコマンドの進行状況を表示します。

**達成** 率パラメーター値は、取得されたイベントの合計数によって処理されたイベントの数を割って、その `$I` `$Events.count` 結果を100で乗算することによって計算されます。

### 例 4: 入れ子になったプロセスの各レベルの進行状況を表示する

```powershell
foreach ( $i in 1..10 ) {
  Write-Progress -Id 0 "Step $i"
  foreach ( $j in 1..10 ) {
    Write-Progress -Id 1 -ParentId 0 "Step $i - Substep $j"
    foreach ( $k in 1..10 ) {
      Write-Progress -Id 2  -ParentId 1 "Step $i - Substep $j - iteration $k"; start-sleep -m 150
    }
  }
}
```

```Output
Step 1
     Processing
    Step 1 - Substep 2
         Processing
        Step 1 - Substep 2 - Iteration 3
             Processing
```

この例では、 **ParentId** パラメーターを使用して、各ステップの進行中に親/子のリレーションシップを表示するようにインデントされた出力を設定できます。

## PARAMETERS

### -アクティビティ

ステータス バーの上の見出しのテキストの 1 行目を指定します。
このテキストは、進行状況が表示されているアクティビティについて説明します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -完了

進行状況バーを表示するかどうかを示します。
このパラメーターを省略した場合は、 `Write-Progress` 進行状況の情報が表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CurrentOperation

進行状況バーの下のテキスト行を指定します。
このテキストでは、現在実行されている操作について説明します。

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

### -Id

それぞれの進行状況バーを区別するための ID を指定します。 1 つのコマンドで複数の進行状況バーを作成する場合は、このパラメーターを使用します。 進行状況バーに異なる ID を付けなかった場合、進行状況バーは、順に表示される代わりに重なり合って表示されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ParentId

現在のアクティビティの親アクティビティを指定します。
現在のアクティビティが親アクティビティを持たない場合は、値 -1 を使用します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -達成率

完了したアクティビティの割合を指定します。
完了した割合が不明な場合や該当しない場合は、値 -1 を使用します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecondsRemaining

アクティビティが完了するまでの推定残り秒数を指定します。
残り秒数が不明な場合や該当しない場合は、値 -1 を使用します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceId

レコードのソースを指定します。 これは **Id** の代わりに使用できますが、 **ParentId** などの他のパラメーターと共に使用することはできません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -状態

ステータス バーの上の見出しのテキストの 2 行目を指定します。
このテキストは、アクティビティの現在の状態を記述します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### なし

`Write-Progress` は出力を生成しません。

## 注

進行状況バーが表示されない場合は、変数の値を確認し `$ProgressPreference` ます。 値が SilentlyContinue に設定されている場合、進行状況バーは表示されません。 PowerShell の設定の詳細については、「 [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

コマンドレットのパラメーターは、system.servicemodel **レコード** クラスのプロパティに対応しています。 詳細については、「 [進捗記録クラス](/dotnet/api/system.management.automation.progressrecord)」を参照してください。

## 関連リンク

[Write-Debug](Write-Debug.md)

[Write-Error](Write-Error.md)

[Write-Host](Write-Host.md)

[Write-Output](Write-Output.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)
