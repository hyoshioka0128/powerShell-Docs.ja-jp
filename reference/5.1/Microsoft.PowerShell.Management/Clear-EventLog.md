---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/clear-eventlog?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Clear-EventLog
ms.openlocfilehash: af1c808b22a812700857e756136fd570fa0acc35
ms.sourcegitcommit: 1dfd5554b70c7e8f4e3df19e29c384a9c0a4b227
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101685898"
---
# Clear-EventLog

## 概要
ローカルコンピューターまたはリモートコンピューター上の指定されたイベントログからすべてのエントリを削除します。

## SYNTAX

```
Clear-EventLog [-LogName] <String[]> [[-ComputerName] <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Clear-EventLog`コマンドレットは、ローカルコンピューターまたはリモートコンピューター上の指定されたイベントログからすべてのエントリを削除します。 を使用するには `Clear-EventLog` 、影響を受けるコンピューターの Administrators グループのメンバーである必要があります。

**Eventlog** 名詞を含むコマンドレット (eventlog コマンドレット) は、従来のイベントログでのみ機能します。 Windows Vista 以降のバージョンの windows で windows イベントログテクノロジを使用するログからイベントを取得するには、Get-WinEvent コマンドレットを使用します。

## 例

### 例 1: ローカルコンピューターから特定のイベントログの種類をクリアする

```powershell
Clear-EventLog "Windows PowerShell"
```

このコマンドは、ローカルコンピューター上の Windows PowerShell イベントログからエントリを削除します。

### 例 2: ローカルコンピューターとリモートコンピューターから特定の複数のログの種類を消去する

```powershell
Clear-EventLog -LogName ODiag, OSession -ComputerName localhost, Server02
```

このコマンドは、ローカルコンピューターおよび Server02 リモートコンピューター上の Microsoft Office Diagnostics (ODiag) および Microsoft Office Sessions (Odiag) ログに含まれるすべてのエントリをクリアします。

### 例 3: 指定されたコンピューター上のすべてのログを消去し、イベントログの一覧を表示する

```powershell
Clear-EventLog -LogName application, system -confirm
```

このコマンドでは、指定したイベント ログのエントリを削除する前に確認を求められます。

### 例 4: 指定したコンピューター上のすべてのログを消去し、イベントログの一覧を表示する

```powershell
function clear-all-event-logs ($computerName="localhost")
{
   $logs = Get-EventLog -ComputerName $computername -List | ForEach-Object {$_.Log}
   $logs | ForEach-Object {Clear-EventLog -ComputerName $computername -LogName $_ }
   Get-EventLog -ComputerName $computername -list
}

clear-all-event-logs -ComputerName Server01
```

```Output
Max(K) Retain OverflowAction        Entries Log
------ ------ --------------        ------- ---
15,168      0 OverwriteAsNeeded           0 Application
15,168      0 OverwriteAsNeeded           0 DFS Replication
512         7 OverwriteOlder              0 DxStudio
20,480      0 OverwriteAsNeeded           0 Hardware Events
512         7 OverwriteOlder              0 Internet Explorer
20,480      0 OverwriteAsNeeded           0 Key Management Service
16,384      0 OverwriteAsNeeded           0 Microsoft Office Diagnostics
16,384      0 OverwriteAsNeeded           0 Microsoft Office Sessions
30,016      0 OverwriteAsNeeded           1 Security
15,168      0 OverwriteAsNeeded           2 System
15,360      0 OverwriteAsNeeded           0 Windows PowerShell
```

この関数は指定されたコンピューター上のすべてのイベント ログをクリアし、その結果のイベント ログの一覧を表示します。

クリアした後で表示される前にシステム ログやセキュリティ ログに追加されたエントリがあることに注意してください。

## PARAMETERS

### -ComputerName

リモート コンピューターを指定します。 既定値はローカル コンピューターです。

リモート コンピューターの NetBIOS 名、インターネット プロトコル (IP) アドレス、または完全修飾ドメイン名を入力します。 ローカル コンピューターを指定するには、コンピューター名、ドット (.)、または「localhost」を入力します。

このパラメーターは、Windows PowerShell リモート処理に依存しません。  `Get-EventLog` コンピューターがリモートコマンドを実行するように構成されていない場合でも、の ComputerName パラメーターを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Cn

Required: False
Position: 1
Default value: Local computer
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -LogName

イベント ログを指定します。 1つ以上のイベントログのログ名 ( **Logdisplayname** ではなく **log** プロパティの値) をコンマで区切って入力します。 ワイルドカード文字は使用できません。 このパラメーターは必須です。

> [!IMPORTANT]
> このパラメーターは、プロパティ名によってパイプラインからの値を受け取ることを想定しています。 ただし、これが機能しないというバグがあります。 パラメーターを使用して値を直接渡す必要があります。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: LN

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

パイプを使用してオブジェクトをにパイプすることはできません `Clear-EventLog` 。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

- Windows Vista 以降のバージョンの Windows でを使用するには `Clear-EventLog` 、[管理者として実行] オプションを使用して Windows PowerShell を起動します。

## 関連リンク

[Get-EventLog](Get-EventLog.md)

[Limit-EventLog](Limit-EventLog.md)

[New-EventLog](New-EventLog.md)

[Remove-EventLog](Remove-EventLog.md)

[Show-EventLog](Show-EventLog.md)

[Write-EventLog](Write-EventLog.md)
