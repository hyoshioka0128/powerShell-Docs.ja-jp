---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-event?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Event
ms.openlocfilehash: df78675e6593fcc7378f151002c1861a68c081b4
ms.sourcegitcommit: de63e9481cf8024883060aae61fb02c59c2de662
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2020
ms.locfileid: "93209696"
---
# New-Event

## 概要
新しいイベントを作成します。

## SYNTAX

```
New-Event [-SourceIdentifier] <String> [[-Sender] <PSObject>] [[-EventArguments] <PSObject[]>]
 [[-MessageData] <PSObject>] [<CommonParameters>]
```

## Description

**新しいイベント** レットは、新しいカスタムイベントを作成します。

カスタム イベントを使用して、プログラム内の状態の変更やプログラムが検出した変更 (ハードウェアまたはシステムの状態、アプリケーションの状態、ディスクの状態、ネットワークの状態、バック グラウンド ジョブの完了など) についてユーザーに通知することができます。

カスタム イベントは、セッションで発生するたびにイベント キューに自動的に追加されます。それらをサブスクライブする必要はありません。
ただし、ローカルのセッションにイベントを転送するか、イベントに応答するアクションを指定する場合は、Register-EngineEvent コマンドレットを使用してカスタム イベントにサブスクライブします。

カスタム イベントをサブスクライブすると、イベント サブスクライバーがセッションに追加されます。
Unregister-Event コマンドレットを使用してイベント サブスクリプションを取り消すと、イベント サブスクライバーおよびカスタム イベントはセッションから削除されます。
カスタムイベントをサブスクライブしていない場合は、イベントを削除するために、プログラムの状態を変更するか、PowerShell セッションを閉じる必要があります。

## 例

### 例 1: イベントキューに新しいイベントを作成する

```
PS C:\> New-Event -SourceIdentifier Timer -Sender windows.timer -MessageData "Test"
```

このコマンドは、PowerShell イベントキューに新しいイベントを作成します。
このメソッドは、 **Windows の Timer** オブジェクトを使用してイベントを送信します。

### 例 2: 別のイベントに応答してイベントを発生させる

```
PS C:\> function Enable-ProcessCreationEvent
{
   $Query = New-Object System.Management.WqlEventQuery "__InstanceCreationEvent", (New-Object TimeSpan 0,0,1), "TargetInstance isa 'Win32_Process'"
   $ProcessWatcher = New-Object System.Management.ManagementEventWatcher $Query
   $Identifier = "WMI.ProcessCreated"
   Register-ObjectEvent $ProcessWatcher "EventArrived" -SupportEvent $Identifier -Action
   {
      [void] (New-Event -SourceID "PowerShell.ProcessCreated" -Sender $Args[0] -EventArguments $Args[1].SourceEventArgs.NewEvent.TargetInstance)
   }
}
```

このサンプル関数では、 **新しい** イベントレットを使用して、別のイベントに応答してイベントを発生させます。
このコマンドは、Register-ObjectEvent コマンドレットを使用して、新しいプロセスの作成時に発生する Windows Management Instrumentation (WMI) イベントをサブスクライブします。
このコマンドは、コマンドレットの *Action* パラメーターを使用して、新しいイベントを作成する **、新しいイベントレットを** 呼び出します。

**新しいイベント** が発生するイベントは PowerShell イベントキューに自動的に追加されるため、そのイベントに登録する必要はありません。

## PARAMETERS

### -EventArguments

イベントのオプションを格納しているオブジェクトを指定します。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MessageData

イベントに関連付けられている追加のデータを指定します。
このパラメーターの値は、イベント オブジェクトの **MessageData** プロパティに表示されます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -送信者

イベントを発生させるオブジェクトを指定します。
既定値は PowerShell エンジンです。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceIdentifier

新しいイベントの名前を指定します。
このパラメーターは必須であり、セッション内で一意である必要があります。

このパラメーターの値は、イベントの **SourceIdentifier** プロパティに表示されます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### PSEventArgs (システム管理)

## 注

新しいカスタム イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。 現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

## 関連リンク

[Get-Event](Get-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)
