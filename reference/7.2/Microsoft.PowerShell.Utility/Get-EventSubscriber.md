---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-eventsubscriber?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-EventSubscriber
ms.openlocfilehash: b8f55770770fa9077f654d382818386cc480c638
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599532"
---
# Get-EventSubscriber

## 概要
現在のセッションのイベント サブスクライバーを取得します。

## SYNTAX

### BySource (既定値)

```
Get-EventSubscriber [[-SourceIdentifier] <String>] [-Force] [<CommonParameters>]
```

### ById

```
Get-EventSubscriber [-SubscriptionId] <Int32> [-Force] [<CommonParameters>]
```

## Description

**Get EventSubscriber** コマンドレットは、現在のセッションのイベントサブスクライバーを取得します。

Register event コマンドレットを使用してイベントをサブスクライブすると、イベントサブスクライバーが PowerShell セッションに追加され、サブスクライブしたイベントが発生するたびにイベントキューに追加されます。
イベント サブスクリプションを取り消すには、Unregister-Event コマンドレットを使用してイベント サブスクライバーを削除します。

## 例

### 例 1: タイマーイベントのイベントサブスクライバーを取得する

```
PS C:\> $Timer = New-Object Timers.Timer
PS C:\> $Timer | Get-Member -Type Event
PS C:\> Register-ObjectEvent -InputObject $Timer -EventName Elapsed -SourceIdentifier Timer.Elapsed
PS C:\> Get-EventSubscriber
PS C:\> $Timer = New-Object Timers.Timer
PS C:\> $Timer | Get-Member -Type Event
TypeName: System.Timers.Timer
Name     MemberType Definition
----     ---------- ----------
Disposed Event      System.EventHandler Disposed(System.Object, System.EventArgs)
Elapsed  Event      System.Timers.ElapsedEventHandler Elapsed(System.Object, System.Timers.ElapsedEventArgs) PS C:\> Register-ObjectEvent -InputObject $Timer -EventName Elapsed -SourceIdentifier Timer.Elapsed
PS C:\> Get-EventSubscriber
SubscriptionId   : 4
SourceObject     : System.Timers.Timer
EventName        : Elapsed
SourceIdentifier : Timer.Elapsed
Action           :
HandlerDelegate  :
SupportEvent     : False
ForwardEvent     : False
```

この例では、 **Get eventsubscriber** コマンドを使用して、timer イベントのイベントサブスクライバーを取得します。

最初のコマンドは、New-Object コマンドレットを使用して、タイマー オブジェクトのインスタンスを作成します。
新しいタイマーオブジェクトを $Timer 変数に保存します。

2 番目のコマンドは、Get-Member コマンドレットを使用して、タイマー オブジェクトに使用できるイベントを表示します。
このコマンドでは、 **Get Member** コマンドレットの Type パラメーターを使用して、イベントの値を指定します。

3 番目のコマンドは、Register-ObjectEvent コマンドレットを使用して、タイマー オブジェクトの Elapsed イベントを登録します。

4番目のコマンドは、 **Get EventSubscriber** コマンドレットを使用して、Elapsed イベントのイベントサブスクライバーを取得します。

### 例 2: イベントサブスクライバーの Action プロパティで PSEventJob の動的モジュールを使用する

```
PS C:\> $Timer = New-Object Timers.Timer
PS C:\> $Timer.Interval = 500
PS C:\> Register-ObjectEvent -InputObject $Timer -EventName Elapsed -SourceIdentifier Timer.Random -Action { $Random = Get-Random -Min 0 -Max 100 }

Id  Name           State      HasMoreData  Location  Command
--  ----           -----      -----------  --------  -------
3   Timer.Random   NotStarted False                  $Random = Get-Random ...

PS C:\> $Timer.Enabled = $True
PS C:\> $Subscriber = Get-EventSubscriber -SourceIdentifier Timer.Random
PS C:\> ($Subscriber.action).gettype().fullname
System.Management.Automation.PSEventJob
PS C:\> $Subscriber.action | Format-List -Property *

State         : Running
Module        : __DynamicModule_6b5cbe82-d634-41d1-ae5e-ad7fe8d57fe0
StatusMessage :
HasMoreData   : True
Location      :
Command       : $random = Get-Random -Min 0 -Max 100
JobStateInfo  : Running
Finished      : System.Threading.ManualResetEvent
InstanceId    : 88944290-133d-4b44-8752-f901bd8012e2
Id            : 1
Name          : Timer.Random
ChildJobs     : {}
...

PS C:\> & $Subscriber.action.module {$Random}
96
PS C:\> & $Subscriber.action.module {$Random}
23
```

この例では、イベントサブスクライバーの Action プロパティで **PSEventJob** オブジェクトの動的モジュールを使用する方法を示します。

最初のコマンドは、New-Object コマンドレットを使用してタイマー オブジェクトを作成します。
2 番目のコマンドは、タイマーの間隔を 500 (ミリ秒) に設定します。

3 番目のコマンドは、Register-ObjectEvent コマンドレットを使用して、タイマー オブジェクトの Elapsed イベントを登録します。
このコマンドには、イベントを処理するアクションが含まれています。
タイマーの間隔が経過するたびに、イベントが発生してアクション内のコマンドが実行されます。
この場合、Get-Random コマンドレットは 0 ~ 100 の範囲の乱数を生成し、$Random 変数に保存します。
イベントのソース識別子は Timer.Random です。

**ObjectEvent** コマンドで *action* パラメーターを使用すると、コマンドは、アクションを表す **PSEventJob** オブジェクトを返します。

4 番目のコマンドは、タイマーを有効にします。

5番目のコマンドは、 **Get EventSubscriber** コマンドレットを使用して、タイマーのイベントサブスクライバーを取得します。 Random イベント。
イベントサブスクライバーオブジェクトを $Subscriber 変数に保存します。

6番目のコマンドは、イベントサブスクライバーオブジェクトの Action プロパティに **PSEventJob** オブジェクトが含まれていることを示しています。
実際には、このオブジェクトには、 **PSEventJob** コマンド **によって** 返されたものと同じオブジェクトが含まれています。

7番目のコマンドは、Format-List コマンドレットを使用して、リストの Action プロパティに **PSEventJob** オブジェクトのすべてのプロパティを表示します。
結果によって、 **PSEventJob** オブジェクトには、アクションを実装する動的スクリプトモジュールを含む Module プロパティがあることがわかります。

残りのコマンドでは、呼び出し演算子 (&) を使用してモジュール内のコマンドを呼び出し、$Random 変数の値を表示します。
呼び出し演算子を使用すると、エクスポートされないコマンドを含め、モジュール内の任意のコマンドを呼び出すことができます。
この例では、これらのコマンドにより Elapsed イベントが発生したときに生成される乱数が示されます。

モジュールの詳細については、「 [about_Modules](../Microsoft.PowerShell.Core/About/about_Modules.md)」を参照してください。

## PARAMETERS

### -Force

このコマンドレットによって、すべてのイベントサブスクライバーが取得されることを示します。これには、Register-engineevent の *supportevent* パラメーターを使用して非表示になっているイベントのサブスクライバーも含まれます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceIdentifier

イベントサブスクライバーのみを取得する **SourceIdentifier** プロパティ値を指定します。
既定では、 **Get EventSubscriber** はセッション内のすべてのイベントサブスクライバーを取得します。
ワイルドカードは使用できません。
このパラメーターでは、大文字と小文字が区別されます。

```yaml
Type: System.String
Parameter Sets: BySource
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SubscriptionId

このコマンドレットが取得するサブスクリプション識別子を指定します。
既定では、 **Get EventSubscriber** はセッション内のすべてのイベントサブスクライバーを取得します。

```yaml
Type: System.Int32
Parameter Sets: ById
Aliases: Id

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### PSEventSubscriber (システム管理)

**Get-EventSubscriber** は、各イベントサブスクライバーを表すオブジェクトを返します。

## 注

* カスタム イベントを作成する New-Event コマンドレットは、サブスクライバーを生成しません。 このため、 **Get eventsubscriber** コマンドレットでは、これらのイベントのサブスクライバーオブジェクトは検出されません。 ただし、Register-EngineEvent コマンドレットを使用してカスタムイベントをサブスクライブすると (イベントを転送したりアクションを指定したりするために)、 **register-engineevent** が生成するサブスクライバーが **Get eventsubscriber** によって検出されます。

  イベント、イベント サブスクリプション、およびイベント キューは、現在のセッションにのみ存在します。
現在のセッションを閉じた場合、イベント キューが破棄され、イベント サブスクリプションが取り消されます。

*

## 関連リンク

[Get-Event](Get-Event.md)

[New-Event](New-Event.md)

[Register-EngineEvent](Register-EngineEvent.md)

[Register-ObjectEvent](Register-ObjectEvent.md)

[Remove-Event](Remove-Event.md)

[Unregister-Event](Unregister-Event.md)

[Wait-Event](Wait-Event.md)

