---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-information?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Information
ms.openlocfilehash: f15902c1c6c298dd02db3edf061d56e1446ecb1f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600397"
---
# Write-Information

## 概要

PowerShell がコマンドの情報ストリームデータを処理する方法を指定します。

## SYNTAX

```
Write-Information [-MessageData] <Object> [[-Tags] <String[]>] [<CommonParameters>]
```

## Description

`Write-Information`コマンドレットは、PowerShell がコマンドの情報ストリームデータを処理する方法を指定します。

Windows PowerShell 5.0 では、新しい構造化された情報ストリームが導入されています。 このストリームを使用して、スクリプトとその呼び出し元またはホストアプリケーションとの間で構造化データを転送できます。
`Write-Information` ストリームに情報メッセージを追加したり、PowerShell がコマンドの情報ストリームデータを処理する方法を指定したりできます。 情報ストリームは `PowerShell.Streams` 、、ジョブ、およびスケジュールされたタスクにも使用できます。

> [!NOTE]
> 情報ストリームは、メッセージを "[Stream Name]:" にプレフィックスとする標準規約に従っていません。 これは簡潔さと視覚的な cleanliness を目的としています。

ユーザー設定変数の値によって、 `$InformationPreference` に対して指定したメッセージ `Write-Information` が、スクリプトの操作の予想される位置に表示されるかどうかが決まります。 この変数の既定値はであるため、 `SilentlyContinue` 既定では情報メッセージは表示されません。 の値を変更しない場合は `$InformationPreference` 、コマンドに共通パラメーターを追加することで、その値をオーバーライドでき `InformationAction` ます。 詳細については、「 [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md) 」および「 [about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

> [!NOTE]
> Windows PowerShell 5.0 以降では、を `Write-Host` 使用して、 `Write-Information` `Write-Host` 情報ストリームに出力を出力することができます。 これにより、後方互換性を維持しながら、を使用して書き込まれたデータを **キャプチャ** または **抑制** でき `Write-Host` ます。 詳細については[、「書き込み-ホスト](Write-Host.md)」を参照してください。

`Write-Information` は、PowerShell 5.x でサポートされているワークフローアクティビティでもあります。

## 例

### 例 1: Get 結果の情報を書き込む

この例では、コマンドを実行した後、" `Get-WindowsFeature` p" で始まる名前値を持つすべての機能を検索するために、"取得した機能!" という情報メッセージが表示されます。
`$InformationPreference`変数はまだ既定値に設定されているので、パラメーターを追加して `SilentlyContinue` `InformationAction` `$InformationPreference` 値をオーバーライドし、メッセージを表示します。 `InformationAction`値は Continue です。つまり、メッセージが表示されますが、まだ完了していない場合は、スクリプトまたはコマンドが続行されます。

```powershell
Get-WindowsFeature -Name p*
Write-Information -MessageData "Got your features!" -InformationAction Continue
```

```Output
Display Name                                            Name                       Install State
------------                                            ----                       -------------
[ ] Print and Document Services                         Print-Services                 Available
    [ ] Print Server                                    Print-Server                   Available
    [ ] Distributed Scan Server                         Print-Scan-Server              Available
    [ ] Internet Printing                               Print-Internet                 Available
    [ ] LPD Service                                     Print-LPD-Service              Available
[ ] Peer Name Resolution Protocol                       PNRP                           Available
[X] Windows PowerShell                                  PowerShellRoot                 Installed
    [X] Windows PowerShell 5.0                          PowerShell                     Installed
    [ ] Windows PowerShell 2.0 Engine                   PowerShell-V2                    Removed
    [X] Windows PowerShell ISE                          PowerShell-ISE                 Installed
Got your features!
```

### 例 2: 情報を書き込んでタグを付ける

この例では、を使用し `Write-Information` て、現在のコマンドの実行が完了した後に別のコマンドを実行する必要があることをユーザーに通知します。 この例では、情報メッセージにタグ命令を追加します。 このコマンドを実行した後、情報ストリームでタグ付きのメッセージを検索すると、ここで指定したメッセージが結果の中に表示されます。

```powershell
$message = "To filter your results for PowerShell, pipe your results to the Where-Object cmdlet."
Get-WindowsFeature -Name p*
Write-Information -MessageData $message -Tags "Instructions" -InformationAction Continue
```

```Output
Display Name                                            Name                       Install State
------------                                            ----                       -------------
[ ] Print and Document Services                         Print-Services                 Available
    [ ] Print Server                                    Print-Server                   Available
    [ ] Distributed Scan Server                         Print-Scan-Server              Available
    [ ] Internet Printing                               Print-Internet                 Available
    [ ] LPD Service                                     Print-LPD-Service              Available
[ ] Peer Name Resolution Protocol                       PNRP                           Available
[X] Windows PowerShell                                  PowerShellRoot                 Installed
    [X] Windows PowerShell 5.0                          PowerShell                     Installed
    [ ] Windows PowerShell 2.0 Engine                   PowerShell-V2                    Removed
    [X] Windows PowerShell ISE                          PowerShell-ISE                 Installed
To filter your results for PowerShell, pipe your results to the Where-Object cmdlet.
```

### 例 3: ファイルに情報を書き込む

この例では、コードを使用して関数の情報ストリームをにリダイレクトし `Info.txt` `6>` ます。 ファイルを開くと、 `Info.txt` "ここに入力してください" というテキストが表示されます。

```powershell
function Test-Info
{
    Get-Process P*
    Write-Information "Here you go"
}
Test-Info 6> Info.txt
```

### 例 4: オブジェクトを渡して情報を書き込む

この例では、を使用し `Write-Information` て、複数のパイプラインを通過するオブジェクト出力から上位10個の CPU 使用率プロセスを書き込むことができ `Get-Process` ます。

```powershell
Get-Process | Sort-Object CPU -Descending |
    Select-Object Id, ProcessName, CPU -First 10 |
        Write-Information -InformationAction Continue
```

```Output
@{Id=12692; ProcessName=chrome; CPU=39431.296875}
@{Id=21292; ProcessName=OUTLOOK; CPU=23991.875}
@{Id=10548; ProcessName=CefSharp.BrowserSubprocess; CPU=20546.203125}
@{Id=312848; ProcessName=Taskmgr; CPU=13173.1875}
@{Id=10848; ProcessName=SnapClient; CPU=7014.265625}
@{Id=9760; ProcessName=Receiver; CPU=6792.359375}
@{Id=12040; ProcessName=Teams; CPU=5605.578125}
@{Id=498388; ProcessName=chrome; CPU=3062.453125}
@{Id=6900; ProcessName=chrome; CPU=2546.9375}
@{Id=9044; ProcessName=explorer; CPU=2358.765625}
```

## PARAMETERS

### -MessageData

スクリプトまたはコマンドを実行するときにユーザーに表示する情報メッセージを指定します。 最適な結果を得るには、情報メッセージを引用符で囲みます。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases: Msg, Message

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -タグ

で情報ストリームに追加したメッセージの並べ替えとフィルター処理に使用できる単純な文字列を指定し `Write-Information` ます。 このパラメーターは、の **Tags** パラメーターと同じように動作 `New-ModuleManifest` します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Object

`Write-Information` 情報ストリームに渡すパイプされたオブジェクトを受け入れます。

## 出力

### システムの... InformationRecord

## 注

## 関連リンク

[about_Output_Streams](../Microsoft.PowerShell.Core/About/about_Output_Streams.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)

[about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Write-Debug](Write-Debug.md)

[Write-Host](Write-Host.md)

[Write-Information](Write-Information.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)

[Write-Output](Write-Output.md)
