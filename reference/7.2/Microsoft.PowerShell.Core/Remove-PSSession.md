---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/remove-pssession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-PSSession
ms.openlocfilehash: cd0e2b62464a4c34278d42b833a669c6c28e377f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602633"
---
# Remove-PSSession

## 概要
1つ以上の PowerShell セッション (pssession) を閉じます。

## SYNTAX

### Id (既定値)

```
Remove-PSSession [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### セッション

```
Remove-PSSession [-Session] <PSSession[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ContainerId

```
Remove-PSSession -ContainerId <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### VMId

```
Remove-PSSession -VMId <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### VMName

```
Remove-PSSession -VMName <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceId

```
Remove-PSSession -InstanceId <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### 名前

```
Remove-PSSession -Name <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### [ComputerName]

```
Remove-PSSession [-ComputerName] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

削除コマンドレットは、現在のセッションの PowerShell **セッション (** **PSSession** ) を閉じます。
このメソッドは、pssession で実行されている **すべてのコマンドを停止** し、 **pssession** を終了して、 **pssession** が使用していたリソースを解放します。
**PSSession** がリモートコンピューターに接続されている場合は、このコマンドレットによって、ローカルコンピューターとリモートコンピューター間の接続も切断されます。

**PSSession** を削除するには、セッションの *名前*、 *ComputerName*、 *ID*、または *InstanceID* を入力します。

*Pssession* を変数に保存した場合、セッションオブジェクトは変数に残りますが、 *pssession* の状態は閉じられます。

## 例

### 例 1: Id を使用してセッションを削除する

```powershell
Remove-PSSession -Id 1, 2
```

このコマンドは、Id が1および2の **pssession を削除** します。

### 例 2: 現在のセッションのすべてのセッションを削除する

```powershell
Get-PSSession | Remove-PSSession
Remove-PSSession -Session (Get-PSSession)
$s = Get-PSSession
Remove-PSSession -Session $s
```

これらのコマンド **は、現在** のセッションのすべての pssession を削除します。
これらの 3 つのコマンドの形式は異なっていますが、効果は同じです。

### 例 3: 名前を使用してセッションを閉じる

```powershell
$r = Get-PSSession -ComputerName Serv*
$r | Remove-PSSession
```

これらのコマンドは **、名前** が Serv で始まるコンピューターに接続されている pssession を閉じます。

### 例 4: ポートに接続されているセッションを閉じる

```powershell
Get-PSSession | where {$_.port -eq 90} | Remove-PSSession
```

このコマンドは、ポート90に接続され **ている pssession** を閉じます。
このコマンド形式を使用すると、 *ComputerName*、 *Name*、 *InstanceID*、および *ID* 以外のプロパティで **pssessions** を識別できます。

### 例 5: インスタンス ID に基づいてセッションを閉じる

```powershell
Get-PSSession | Format-Table ComputerName, InstanceID  -AutoSize
```

```Output
ComputerName InstanceId
------------ ----------------
Server01     875d231b-2788-4f36-9f67-2e50d63bb82a
localhost    c065ffa0-02c4-406e-84a3-dacb0d677868
Server02     4699cdbc-61d5-4e0d-b916-84f82ebede1f
Server03     4e5a3245-4c63-43e4-88d0-a7798bfc2414
TX-TEST-01   fc4e9dfa-f246-452d-9fa3-1adbdd64ae85 PS C:\> Remove-PSSession -InstanceID fc4e9dfa-f246-452d-9fa3-1adbdd64ae85
```

これらのコマンドは、インスタンス ID または **Remoterunspace ID** に基づいて **PSSession** を閉じる方法を示しています。

最初のコマンドは、 **Get PSSession** コマンドレットを使用して、現在の **セッションの** PSSession を取得します。
この例では、パイプライン演算子 (|) を使用して、pssession を Format-Table コマンド **レットに送信します** 。このコマンドレットは、テーブル内の **ComputerName** プロパティと **InstanceID** プロパティを書式設定します。
*AutoSize* パラメーターは、表示する列を圧縮します。

結果の表示から、閉じる **pssession** を特定し、その **pssession** の *InstanceID* をコピーして2番目のコマンドに貼り付けることができます。

2番目のコマンドは、 **削除 pssession** コマンドレットを使用して、指定されたインスタンス ID を持つ *pssession* を削除します。

### 例 6: 現在のセッションのすべてのセッションを削除する関数を作成する

```powershell
Function EndPSS { Get-PSSession | Remove-PSSession }
```

この関数 **は、現在** のセッションのすべての pssession を削除します。
この関数を PowerShell プロファイルに追加した後、すべてのセッションを削除するには、「」と入力 `EndPSS` します。

## PARAMETERS

### -ComputerName

コンピューターの名前の配列を指定します。
このコマンドレットは、指定されたコンピューターに接続され **ている pssession** を閉じます。
ワイルドカード文字を使用できます。

1 台または複数のリモート コンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。
ローカルコンピューターを指定するには、コンピューター名、localhost、またはドット (.) を入力します。

```yaml
Type: System.String[]
Parameter Sets: ComputerName
Aliases: Cn

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -ContainerId

コンテナーの Id の配列を指定します。 このコマンドレットは、指定された各コンテナーのセッションを削除します。 `docker ps`コンテナー id の一覧を取得するには、コマンドを使用します。 詳細については、 [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) コマンドのヘルプを参照してください。

```yaml
Type: System.String[]
Parameter Sets: ContainerId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Id

セッションの Id の配列を指定します。
このコマンドレットは、指定された Id *を持つ pssession を閉じ* ます。
1つ以上の Id をコンマで区切って入力するか、範囲演算子 (..) を使用して Id の範囲を指定します。

ID は、現在のセッションの **PSSession** を一意に識別する整数です。
これは *InstanceId* よりも覚えやすく、入力も簡単ですが、現在のセッションでのみ一意です。
**PSSession** の ID を検索するには、パラメーターを指定せずに Get-PSSession コマンドレットを実行します。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InstanceId

インスタンス Id の配列を指定します。
このコマンドレットは、指定されたインスタンス Id を持つ **pssession を閉じ** ます。

インスタンス ID は、現在のセッションの **PSSession** を一意に識別する GUID です。
1台のコンピューターで複数のセッションが実行されている場合でも、インスタンス ID は一意です。

インスタンス ID は、 **PSSession** を表すオブジェクトの **InstanceID** プロパティに格納されます。
現在 **のセッション** の Pssession の **InstanceID** を検索するには、「」と入力 `Get-PSSession | Format-Table Name, ComputerName, InstanceId` します。

```yaml
Type: System.Guid[]
Parameter Sets: InstanceId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

セッションのフレンドリ名の配列を指定します。
このコマンド **レットは、** 指定されたフレンドリ名を持つ pssession を閉じます。
ワイルドカード文字を使用できます。

**Pssession** のフレンドリ名は一意ではない可能性があるため、 *name* パラメーターを使用する場合は、 **Pssession の削除** コマンドで *WhatIf* または *Confirm* パラメーターも使用することを検討してください。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -セッション

閉じる **pssession のセッション** オブジェクトを指定します。
**Pssessions** を含む変数、または PSSession **を作成** または取得するコマンドを入力します (New-PSSession や、 **PSSession** コマンドなど)。
パイプを使用して1つ以上のセッションオブジェクトを **削除** することもできます。

```yaml
Type: System.Management.Automation.Runspaces.PSSession[]
Parameter Sets: Session
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -VMId

仮想マシンの ID の配列を指定します。
このコマンドレットは、指定された各仮想マシンとの対話型セッションを開始します。
使用可能な仮想マシンを表示するには、次のコマンドを使用します。

`Get-VM | Select-Object -Property Name, ID`

```yaml
Type: System.Guid[]
Parameter Sets: VMId
Aliases: VMGuid

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -VMName

仮想マシンの名前の配列を指定します。
このコマンドレットは、指定された各仮想マシンとの対話型セッションを開始します。
使用可能な仮想マシンを表示するには、 **GET VM** コマンドレットを使用します。

```yaml
Type: System.String[]
Parameter Sets: VMName
Aliases:

Required: True
Position: Named
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. 実行空間

このコマンドレットには、セッションオブジェクトをパイプすることができます。

## 出力

### なし

このコマンドレットはオブジェクトを返しません。

## 注

* *Id* パラメーターは必須です。 現在の **セッションのすべての pssession** を削除するには、「」と入力 `Get-PSSession | Remove-PSSession` します。
* **PSSession** は、リモートコンピューターへの永続的な接続を使用します。 **PSSession** を作成して、データを共有する一連のコマンドを実行します。 詳細を表示するには「`Get-Help about_PSSessions`」を入力します。
* **Pssessions** は、現在のセッションに固有のものです。 セッションを終了すると、そのセッションで作成 **した pssession** が強制的に閉じられます。

## 関連リンク

[Connect-PSSession](Connect-PSSession.md)

[Disconnect-PSSession](Disconnect-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Exit-PSSession](Exit-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)

[Receive-PSSession](Receive-PSSession.md)

[about_PSSessions](About/about_PSSessions.md)

[about_Remote](About/about_Remote.md)

