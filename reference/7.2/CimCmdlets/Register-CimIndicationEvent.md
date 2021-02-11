---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 02/20/2019
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/register-cimindicationevent?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-CimIndicationEvent
ms.openlocfilehash: b9fcd7b7be4dab6f4ccd0c2c1ccab1d27a332d57
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599702"
---
# Register-CimIndicationEvent

## 概要
フィルター式またはクエリ式を使用した表示をサブスクライブします。

## SYNTAX

### ClassNameComputerSet (既定値)

```
Register-CimIndicationEvent [-Namespace <String>] [-ClassName] <String>
 [-OperationTimeoutSec <UInt32>] [-ComputerName <String>] [[-SourceIdentifier] <String>]
 [[-Action] <ScriptBlock>] [-MessageData <PSObject>] [-SupportEvent] [-Forward]
 [-MaxTriggerCount <Int32>] [<CommonParameters>]
```

### ClassNameSessionSet

```
Register-CimIndicationEvent [-Namespace <String>] [-ClassName] <String>
 [-OperationTimeoutSec <UInt32>] -CimSession <CimSession> [[-SourceIdentifier] <String>]
 [[-Action] <ScriptBlock>] [-MessageData <PSObject>] [-SupportEvent] [-Forward]
 [-MaxTriggerCount <Int32>] [<CommonParameters>]
```

### Query式の Sessionset

```
Register-CimIndicationEvent [-Namespace <String>] [-Query] <String> [-QueryDialect <String>]
 [-OperationTimeoutSec <UInt32>] -CimSession <CimSession> [[-SourceIdentifier] <String>]
 [[-Action] <ScriptBlock>] [-MessageData <PSObject>] [-SupportEvent] [-Forward]
 [-MaxTriggerCount <Int32>] [<CommonParameters>]
```

### Query式 Computerset

```
Register-CimIndicationEvent [-Namespace <String>] [-Query] <String> [-QueryDialect <String>]
 [-OperationTimeoutSec <UInt32>] [-ComputerName <String>] [[-SourceIdentifier] <String>]
 [[-Action] <ScriptBlock>] [-MessageData <PSObject>] [-SupportEvent] [-Forward]
 [-MaxTriggerCount <Int32>] [<CommonParameters>]
```

## Description

`Register-CimIndicationEvent`コマンドレットは、クラス名またはクエリ式を指定して、表示をサブスクライブします。 **SourceIdentifier** パラメーターを使用して、サブスクリプションに名前を付けます。

このコマンドレットは、 **Eventsubscription** オブジェクトを返します。 このオブジェクトを使用して、サブスクリプションを取り消すことができます。

## 例

### 例 1: クラスによって生成されたイベントを登録する

この例では、 **Win32_ProcessStartTrace** という名前のクラスによって生成されたイベントをサブスクライブします。 プロセスが開始されるたびに、このクラスによってイベントが発生します。

```powershell
Register-CimIndicationEvent -ClassName 'Win32_ProcessStartTrace' -SourceIdentifier "ProcessStarted"
Get-Event -SourceIdentifier "ProcessStarted"
```

`Get-Event`コマンドレットでは、 **processstarted** サブスクリプションを使用してイベントを取得します。 詳細については、「 [Get イベント](../Microsoft.PowerShell.Utility/Get-Event.md)」を参照してください。

> [!NOTE]
> この例では、PowerShell を管理者として実行する必要があります。

### 例 2: クエリを使用してイベントを登録する

この例では、クエリを使用して、 **Win32_LocalTime** という名前のクラスのインスタンスが変更されるたびに生成されるイベントをサブスクライブします。

```powershell
$query = "SELECT * FROM CIM_InstModification WHERE TargetInstance ISA 'Win32_LocalTime'"
Register-CimIndicationEvent -Query $query -SourceIdentifier "Timer"
```

### 例 3: イベントが到着したときにスクリプトを実行する

この例では、イベントに応答してアクションを使用する方法を示します。 変数は、 `$action`  `$event` CIM から受信したイベントにアクセスするために変数を使用する Action のスクリプトブロックを保持します。

```powershell
$action = {
  $name = $event.SourceEventArgs.NewEvent.ProcessName
  $id = $event.SourceEventArgs.NewEvent.ProcessId
  Write-Host -Object "New Process Started : Name = $name
 ID = $id"
}
Register-CimIndicationEvent -ClassName 'Win32_ProcessStartTrace' -SourceIdentifier "ProcessStarted" -Action $action
```

詳細については、「 [Win32_ProcessStartTrace](/previous-versions/windows/desktop/krnlprov/win32-processstarttrace)」を参照してください。

### 例 4: リモートコンピューターにイベントを登録する

この例では、 **Server01** という名前のリモートコンピューター上のイベントをサブスクライブします。 CIM サーバーから受信したイベントは、現在の PowerShell セッションのイベントキューに格納され、ローカルのを実行して `Get-Event` イベントを取得します。

```powershell
Register-CimIndicationEvent -ClassName 'Win32_ProcessStartTrace' -SourceIdentifier "ProcessStarted" -ComputerName Server01
Get-Event -SourceIdentifier "ProcessStarted"
```

## PARAMETERS

### -Action

イベントを処理するコマンドを指定します。 このパラメーターで指定したコマンドは、イベントをイベントキューに送信する代わりに、イベントが発生したときに実行されます。 コマンドを中かっこ () で囲み、 `{}` スクリプトブロックを作成します。

**Action** で指定されたスクリプトブロックには、、、、、および自動変数を含めることができます。これにより、 `$Event` `$EventSubscriber` `$Sender` `$SourceEventArgs` `$SourceArgs` イベントに関する情報が **action** スクリプトブロックに提供されます。 詳細については、「 [自動変数につい](../microsoft.powershell.core/about/about_automatic_variables.md)て」を参照してください。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CimSession

指定された CIM セッションを使用してコマンドを実行します。 CIM セッションを含む変数、またはやコマンドレットなどの CIM セッションを作成または取得するコマンドを入力し `New-CimSession` `Get-CimSession` ます。 詳細については、「 [about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)」を参照してください。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession
Parameter Sets: ClassNameSessionSet, QueryExpressionSessionSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ClassName

サブスクライブするクラスを指定します。 タブ補完を使用してクラスの一覧を参照することができます。これは、クラス名の一覧を提供するために、PowerShell がローカル WMI サーバーからクラスの一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, ClassNameSessionSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

CIM 操作を実行するコンピューターの名前を指定します。 完全修飾ドメイン名 (FQDN)、NetBIOS 名、または IP アドレスを指定できます。

このパラメーターを指定すると、コマンドレットは WsMan プロトコルを使用して、指定されたコンピューターへの一時的なセッションを作成します。 このパラメーターを指定しない場合、コマンドレットはコンポーネントオブジェクトモデル (COM) を使用してローカルシステムで操作を実行します。

同じコンピューターで複数の操作を実行する場合は、パフォーマンスを向上させるために CIM セッションを使用して接続します。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, QueryExpressionComputerSet
Aliases: CN, ServerName

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -転送

サブスクリプションのイベントがローカルコンピューター上のセッションに転送されることを示します。 このパラメーターは、リモート コンピューターまたはリモート セッションのイベントに登録する場合に使用します。

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

### -MaxTriggerCount

パラメーターを使用して、指定された時間にトリガーされた後、サブスクライバーを自動登録解除する必要があることを示します。 値が0以下の場合は、登録を解除せずにイベントをトリガーできる回数に制限はありません。

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

### -MessageData

このイベントサブスクリプションに関連付ける追加データを指定します。 このパラメーターの値は、このサブスクリプションに関連付けられているすべてのイベントの **Messagedata** プロパティに表示されます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Namespace

CIM 操作の名前空間を指定します。 既定の名前空間は **root/cimv2** です。 タブ補完を使用して名前空間の一覧を参照することができます。これは、名前空間の一覧を提供するために、PowerShell がローカル WMI サーバーから名前空間の一覧を取得するためです。

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

### -OperationTimeoutSec

コマンドレットがコンピューターからの応答を待機する時間を指定します。 既定では、このパラメーターの値は0です。これは、コマンドレットがサーバーの既定のタイムアウト値を使用することを意味します。

**Operationtimeoutsec** パラメーターが、堅牢な接続再試行タイムアウトの3分未満の値に設定されている場合、 **operationtimeoutsec** パラメーターの値を超えるネットワークエラーは復旧できません。これは、クライアントが再接続する前に、サーバー上の操作がタイムアウトになるためです。

```yaml
Type: System.UInt32
Parameter Sets: (All)
Aliases: OT

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Query

CIM サーバーで実行するクエリを指定します。 指定した値に二重引用符 `"` 、単一引用符、または円記号が含まれている場合は `'` `\` 、円記号を付けることによってこれらの文字をエスケープする必要があります。 指定された値が WQL LIKE 演算子を使用する場合は、次の文字を角かっこで囲むことによってエスケープする必要があります `[]` : パーセント `%` 、アンダースコア `_` 、または左角かっこ `[` 。

```yaml
Type: System.String
Parameter Sets: QueryExpressionSessionSet, QueryExpressionComputerSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -QueryDialect

**クエリ** パラメーターに使用するクエリ言語を指定します。 このパラメーターに指定できる値は、 **WQL** または **cql** です。 既定値は **WQL** です。

```yaml
Type: System.String
Parameter Sets: QueryExpressionSessionSet, QueryExpressionComputerSet
Aliases:

Required: False
Position: Named
Default value: WQL
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceIdentifier

サブスクリプションの名前を指定します。 指定する名前は、現在のセッション内で一意である必要があります。 既定値は、PowerShell によって割り当てられる GUID です。 この値は、サブスクライバーオブジェクトの **SourceIdentifier** プロパティの値と、このサブスクリプションに関連付けられているすべてのイベントオブジェクトの値に表示されます。

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

### -SupportEvent

イベントサブスクリプションが非表示であることを示します。 このパラメーターは、現在のサブスクリプションがさらに複雑なイベント登録メカニズムの一部であり、単独で検出されない場合に使用します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは、入力オブジェクトを受け入れません。

## 出力

### System.Object

このコマンドレットは、 **Eventsubscription** オブジェクトを出力します。

## 注

## 関連リンク

[Get-Event](../microsoft.powershell.utility/get-event.md)

[Remove-Event](../microsoft.powershell.utility/remove-event.md)

[Unregister-Event](../microsoft.powershell.utility/unregister-event.md)

[Write-Host](../microsoft.powershell.utility/write-host.md)

[Get-CimSession](Get-CimSession.md)

[New-CimSession](New-CimSession.md)

