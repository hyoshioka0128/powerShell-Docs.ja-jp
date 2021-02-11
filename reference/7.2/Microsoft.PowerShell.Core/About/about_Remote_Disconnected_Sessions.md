---
description: PowerShell セッション (PSSession) を切断して再接続する方法について説明します。
Locale: en-US
ms.date: 12/01/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_disconnected_sessions?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_Disconnected_Sessions
ms.openlocfilehash: 0756e110b1058915f6280e2ea59ee915bedb2c75
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603206"
---
# <a name="about-remote-disconnected-sessions"></a>切断されたリモートセッションについて

## <a name="short-description"></a>簡単な説明

PowerShell セッション (PSSession) を切断して再接続する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell 3.0 以降では、PSSession から切断し、同じコンピューターまたは別のコンピューター上の PSSession に再接続することができます。 セッション状態が維持され、PSSession 内のコマンドはセッションが切断されている間も引き続き実行されます。

切断されたセッション機能は、リモートコンピューターで PowerShell 3.0 以降のバージョンが実行されている場合にのみ使用できます。

切断されたセッション機能を使用すると、pssession で実行されているコマンドを中断することなく、PSSession が作成されたセッションを終了し、PowerShell を閉じてコンピューターをシャットダウンすることができます。 切断されたセッションは、完了までに時間がかかるコマンドを実行する場合に便利です。また、IT プロフェッショナルに必要な時間とデバイスの柔軟性を提供します。

コマンドレットを使用して起動された対話型セッションから切断することはできません `Enter-PSSession` 。

切断されたセッションを使用すると、コンピューターやネットワークの停止の結果として意図せず切断された pssession を管理できます。

実際の使用では、切断されたセッション機能を使用して、問題の解決を開始し、優先度の高い問題に対処してから、別の場所にある別のコンピューターでも、ソリューションで作業を再開することができます。

## <a name="disconnected-session-cmdlets"></a>切断されたセッションのコマンドレット

次のコマンドレットは、切断されたセッション機能をサポートしています。

- `Connect-PSSession`: 切断された PSSession に接続します。
- `Disconnect-PSSession`: PSSession を切断します。
- `Get-PSSession`: ローカルコンピューターまたはリモートコンピューター上の pssession を取得します。
- `Receive-PSSession`: 切断されたセッションで実行されたコマンドの結果を取得します。
- `Invoke-Command`: **Indisconnectedsession** パラメーターは PSSession を作成し、直ちに切断します。

## <a name="how-the-disconnected-sessions-feature-works"></a>切断されたセッション機能の動作

PowerShell 3.0 以降では、PSSessions は、そのセッションが作成されたセッションから独立しています。 PSSession が作成されたセッションが閉じられ、元のコンピューターがネットワークから切断されている場合でも、アクティブな PSSessions は接続のリモートコンピューターまたは **サーバー側** で保持されます。

PowerShell 2.0 では、元のセッションまたは作成されたセッションから切断されたときに、リモートコンピューターから PSSession が削除されます。

PSSession を切断すると、PSSession はアクティブのままになり、リモートコンピューター上で保持されます。 セッションの状態が " **実行中** " から " **切断**" に変わります。 切断された PSSession には、現在のセッションから、または同じコンピューター上の別のセッションから、または別のコンピューターから再接続できます。 セッションを管理するリモートコンピューターは、を実行していて、ネットワークに接続されている必要があります。

切断された PSSession 内のコマンドは、コマンドが完了するか出力バッファーがいっぱいになるまで、リモートコンピューター上でも継続して実行されます。 完全な出力バッファーによってコマンドが中断されないようにするには、 `Disconnect-PSSession` 、 `New-PSSessionOption` 、またはコマンドレットの OutputBufferingMode パラメーターを使用し `New-PSTransportOption` ます。

切断されたセッションは、リモートコンピューターの切断状態で保持されます。 コマンドレットを使用する `Remove-PSSession` か、pssession のアイドルタイムアウトが期限切れになるまで、pssession を削除するまで、再接続することができます。 、、   `Disconnect-PSSession` `New-PSSessionOption` またはコマンドレットの IdleTimeoutSec パラメーターまたは IdleTimeout パラメーターを使用して、PSSession のアイドルタイムアウトを調整でき `New-PSTransportOption` ます。

別のユーザーが作成した pssession に接続できるのは、セッションの作成に使用された資格情報を提供できる場合、または `RunAs` セッション構成の資格情報を使用する場合のみです。

## <a name="how-to-get-pssessions"></a>PSSessions を取得する方法

PowerShell 3.0 以降では、 `Get-PSSession` コマンドレットは、ローカルコンピューターとリモートコンピューター上の PSSessions を取得します。 また、現在のセッションで作成された pssession を取得することもできます。

ローカルコンピューターまたはリモートコンピューターで PSSessions を取得するには、 **ComputerName** パラメーターまたは **connectionuri** パラメーターを使用します。 パラメーターを指定しない場合、は、 `Get-PSSession` 終了位置に関係なく、ローカルセッションで作成された PSSession を取得します。

PSSessions を取得するときは、それらが保持されているコンピューター (つまり、リモートコンピューターまたは **サーバー側** のコンピューター) でそれらを検索してください。

たとえば、Server01 コンピューターへの PSSession を作成する場合は、Server01 コンピューターからセッションを取得します。 別のコンピューターからローカルコンピューターに PSSession を作成する場合は、ローカルコンピューターからセッションを取得します。

次のコマンドシーケンスは、がどのように動作するかを示して `Get-PSSession` います。

最初のコマンドは、Server01 コンピューターへのセッションを作成します。 セッションは Server01 コンピューター上に存在します。

```powershell
New-PSSession -ComputerName Server01
```

```Output
Id Name      ComputerName  State    ConfigurationName     Availability
-- ----      ------------  -----    -----------------     ------------
 2 Session2  Server01      Opened   Microsoft.PowerShell     Available
```

セッションを取得するには、の **ComputerName** パラメーターに `Get-PSSession` 値 Server01 を指定します。

```powershell
Get-PSSession -ComputerName Server01
```

```Output
Id Name      ComputerName  State    ConfigurationName     Availability
-- ----      ------------  -----    -----------------     ------------
 2 Session2  Server01      Opened   Microsoft.PowerShell     Available
```

の **ComputerName** パラメーターの値 `Get-PSSession` が localhost の場合、は、 `Get-PSSession` で終了し、ローカルコンピューター上で保持されている pssession を取得します。 Server01 コンピューターでは、ローカルコンピューターで開始された場合でも、PSSessions は取得されません。

```powershell
Get-PSSession -ComputerName localhost
```

現在のセッションで作成されたセッションを取得するには、 `Get-PSSession` パラメーターを指定せずにコマンドレットを使用します。 この例では、は、 `Get-PSSession` 現在のセッションで作成された PSSession を取得し、Server01 コンピューターに接続します。

```powershell
Get-PSSession
```

```Output
Id Name      ComputerName  State    ConfigurationName     Availability
-- ----      ------------  -----    -----------------     ------------
 2 Session2  Server01      Opened   Microsoft.PowerShell     Available
```

## <a name="how-to-disconnect-sessions"></a>セッションを切断する方法

PSSession を切断するには、 `Disconnect-PSSession` コマンドレットを使用します。 PSSession を特定するには、 **セッション** パラメーターを使用するか、 `New-PSSession` またはコマンドレットの pssession をにパイプラインで指定し `Get-PSSession` `Disconnect-PSSession` ます。

次のコマンドは、PSSession を Server01 コンピューターに切断します。
**State** プロパティの値が **Disconnected** であり、 **Availability** が **None** であることに注意してください。

```powershell
Get-PSSession -ComputerName Server01 | Disconnect-PSSession
```

```Output
Id Name      ComputerName  State         ConfigurationName     Availability
-- ----      ------------  -----         -----------------     ------------
 2 Session2  Server01      Disconnected  Microsoft.PowerShell          None
```

切断されたセッションを作成するには、コマンドレットの **Indisconnectedsession** パラメーターを使用し `Invoke-Command` ます。 コマンドが出力を返す前に、セッションを作成し、コマンドを起動して、直ちに切断します。

次のコマンドは、 `Get-WinEvent` Server02 リモートコンピューター上の切断されたセッションでコマンドを実行します。

```powershell
Invoke-Command -ComputerName Server02 -InDisconnectedSession -ScriptBlock {
   Get-WinEvent -LogName "*PowerShell*" }
```

```Output
Id Name      ComputerName  State         ConfigurationName     Availability
-- ----      ------------  -----         -----------------     ------------
 4 Session3  Server02      Disconnected  Microsoft.PowerShell          None
```

## <a name="how-to-connect-to-disconnected-sessions"></a>切断されたセッションに接続する方法

PSSession を作成したセッション、またはローカルコンピューターまたは他のコンピューター上の他のセッションから、使用可能な切断された PSSession に接続できます。

PSSession の作成、pssession でのコマンドの実行、PSSession からの切断、PowerShell の終了、コンピューターのシャットダウンを行うことができます。 後で、別のコンピューターを開いて、PSSession を取得し、接続して、切断中に PSSession で実行されたコマンドの結果を取得することができます。 その後、セッションで他のコマンドを実行できます。

切断された PSSession を接続するには、コマンドレットを使用し `Connect-PSSession` ます。 **ComputerName** または **connectionuri** パラメーターを使用して pssession を識別します。または、からに pssession を渡すことも `Get-PSSession` `Connect-PSSession` できます。

次のコマンドは、Server02 コンピューター上のセッションを取得します。 出力には、2つの切断されたセッションが含まれています。どちらも使用できます。

```powershell
Get-PSSession -ComputerName Server02
```

```Output
Id Name      ComputerName   State         ConfigurationName     Availability
-- ----      ------------   -----         -----------------     ------------
 2 Session2  juneb-srv8320  Disconnected  Microsoft.PowerShell          None
 4 Session3  juneb-srv8320  Disconnected  Microsoft.PowerShell          None
```

次のコマンドは、Session2 に接続します。 これで PSSession が開き、使用できるようになりました。

```powershell
Connect-PSSession -ComputerName Server02 -Name Session2
```

```Output
Id Name      ComputerName    State    ConfigurationName     Availability
-- ----      ------------    -----    -----------------     ------------
 2 Session2  juneb-srv8320   Opened   Microsoft.PowerShell     Available
```

## <a name="how-to-get-the-results"></a>結果を取得する方法

切断された PSSession で実行されたコマンドの結果を取得するには、コマンドレットを使用し `Receive-PSSession` ます。

`Receive-PSSession`コマンドレットを使用するのではなく、を使用でき `Connect-PSSession` ます。 セッションが既に再接続されている場合、は、 `Receive-PSSession` セッションが切断されたときに実行されたコマンドの結果を取得します。 PSSession がまだ切断されている場合、は `Receive-PSSession` それに接続し、切断されたときに実行されたコマンドの結果を取得します。

`Receive-PSSession` は、ジョブ (非同期) またはホストプログラムに (同期的に) 結果を返すことができます。 **ジョブ** または **ホスト** を選択するには、 **outtarget** パラメーターを使用します。 既定値は **Host** です。 ただし、受信しているコマンドが現在のセッションで **ジョブ** として開始されている場合、既定では **ジョブ** として返されます。

次のコマンドは、コマンドレットを使用して、 `Receive-PSSession` Server02 コンピューター上の PSSession に接続し、 `Get-WinEvent` Session3 セッションで実行されたコマンドの結果を取得します。 このコマンドは、 **Outtarget** パラメーターを使用して、 **ジョブ** の結果を取得します。

```powershell
Receive-PSSession -ComputerName Server02 -Name Session3 -OutTarget Job
```

```Output
Id   Name   PSJobTypeName   State         HasMoreData     Location
--   ----   -------------   -----         -----------     --------
 3   Job3   RemoteJob       Running       True            Server02
```

ジョブの結果を取得するには、 `Receive-Job` コマンドレットを使用します。

```powershell
Get-Job | Receive-Job -Keep
```

```Output
ProviderName: PowerShell

TimeCreated             Id LevelDisplayName Message     PSComputerName
-----------             -- ---------------- -------     --------------
5/14/2012 7:26:04 PM   400 Information      Engine stat Server02
5/14/2012 7:26:03 PM   600 Information      Provider "W Server02
5/14/2012 7:26:03 PM   600 Information      Provider "C Server02
5/14/2012 7:26:03 PM   600 Information      Provider "V Server02
```

## <a name="state-and-availability-properties"></a>状態と可用性のプロパティ

切断された PSSession の **状態** と **可用性** のプロパティは、セッションを再接続できるかどうかを通知します。

PSSession が現在のセッションに接続されると、その状態が **開か** れ、その可用性が **利用可能** になります。 PSSession から切断すると、PSSession の状態は **Disconnected** になり、その可用性は **None** になります。

**State** プロパティの値は、現在のセッションに関連付けられています。 値が **Disconnected** の場合は、PSSession が現在のセッションに接続されていないことを示します。 ただし、PSSession がすべてのセッションから切断されているわけではありません。 別のセッションに接続されている可能性があるためです。

PSSession に接続または再接続できるかどうかを判断するには、 **Availability** プロパティを使用します。 値 **None** は、セッションに接続できることを示します。 値が **Busy** の場合は、PSSession が別のセッションに接続されているため、PSSession に接続できないことを示します。

次の例は、同じコンピューター上の2つの PowerShell セッションで実行されます。
PSSession が切断されて再接続されると、各セッションの **状態** と **可用性** のプロパティの値を変更することに注意してください。

```powershell
# Session 1
New-PSSession -ComputerName Server30 -Name Test
```

```Output
Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
1  Test   Server30        Opened        Microsoft.PowerShell     Available
```

```powershell
# Session 2
Get-PSSession -ComputerName Server30 -Name Test
```

```Output
Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
1 Test    Server30        Disconnected  Microsoft.PowerShell          Busy
```

```powershell
# Session 1
Get-PSSession -ComputerName Server30 -Name Test | Disconnect-PSSession
```

```Output
Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
1 Test    Server30        Disconnected  Microsoft.PowerShell          None
```

```powershell
# Session 2
Get-PSSession -ComputerName Server30
```

```Output
Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
1 Test    Server30        Disconnected  Microsoft.PowerShell          None
```

```powershell
# Session 2
Connect-PSSession -ComputerName Server30 -Name Test
```

```Output
Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
3 Test    Server30        Opened        Microsoft.PowerShell     Available
```

```powershell
# Session 1
Get-PSSession -ComputerName Server30
```

```Output
Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
1 Test    Server30        Disconnected  Microsoft.PowerShell          Busy
```

```Output
# Idle Timeout
```

切断されたセッションは、コマンドレットなどを使用して削除するか、タイムアウトにするまで、リモートコンピューター上で保持され `Remove-PSSession` ます。PSSession の **IdleTimeout** プロパティは、切断されたセッションが削除されるまでの期間を決定します。

PSSessions は、 **ハートビートスレッド** が応答を受信しなかったときにアイドル状態になります。
セッションを切断すると、切断されたセッションでコマンドがまだ実行されている場合でも、アイドル状態になり、 **IdleTimeout** クロックが開始されます。 PowerShell は、切断されたセッションをアクティブにし、アイドル状態と見なします。

セッションを作成または切断するときに、PSSession のアイドルタイムアウトが、必要なセッションを維持するのに十分な長さであることを確認します。ただし、リモートコンピューター上の不要なリソースを消費することはありません。

セッション構成の **IdleTimeoutMs** プロパティによって、セッション構成を使用するセッションの既定のアイドルタイムアウトが決まります。 既定値を上書きすることはできますが、使用する値はセッション構成の **MaxIdleTimeoutMs** プロパティを超えることはできません。

セッション構成の **IdleTimeoutMs** 値と **MaxIdleTimeoutMs** 値の値を調べるには、次のコマンド形式を使用します。

```powershell
Get-PSSessionConfiguration |
  Format-Table Name, IdleTimeoutMs, MaxIdleTimeoutMs
```

セッション構成の既定値を上書きし、PSSession の作成時や切断時に PSSession のアイドルタイムアウトを設定できます。

リモートコンピューターの Administrators グループのメンバーである場合は、セッション構成の **IdleTimeoutMs** プロパティと **MaxIdleTimeoutMs** プロパティを作成および変更できます。

## <a name="idle-timeout-values"></a>アイドルタイムアウト値

セッション構成とセッションオプションのアイドルタイムアウト値は、ミリ秒単位です。 セッションのアイドルタイムアウト値とセッション構成オプションは、秒単位で指定します。

Pssession (、) を作成したとき `New-PSSession` と、その接続を切断したとき () に、pssession のアイドルタイムアウトを設定でき `Invoke-Command` `Disconnect-PSSession` ます。 ただし、PSSession () に接続するときや、結果を取得するとき ()、 **IdleTimeout** 値を変更することはできません `Connect-PSSession` `Receive-PSSession` 。

コマンドレットとコマンドレットには、 `Connect-PSSession` `Receive-PSSession` コマンドレットによって返されるような **sessionoption** オブジェクトを受け取る **sessionoption** パラメーターがあり `New-PSSessionOption` ます。 **Sessionoption** オブジェクトの **idletimeout** 値と、ユーザー設定変数の **idletimeout** 値は、 `$PSSessionOption` またはコマンドで PSSession の **idletimeout** の値を変更しません `Connect-PSSession` `Receive-PSSession` 。

特定のアイドルタイムアウト値を使用して PSSession を作成するには、ユーザー設定変数を作成し `$PSSessionOption` ます。 **IdleTimeout** プロパティの値を目的の値 (ミリ秒単位) に設定します。

PSSessions を作成すると、変数の値は、 `$PSSessionOption` セッション構成の値よりも優先されます。

たとえば、次のコマンドでは、アイドルタイムアウトが48時間に設定されています。

```powershell
$PSSessionOption = New-PSSessionOption -IdleTimeoutMSec 172800000
```

特定のアイドルタイムアウト値を使用して PSSession を作成するには、コマンドレットの **IdleTimeoutMSec** パラメーターを使用し `New-PSSessionOption` ます。 次に、またはコマンドレットの **sessionoption** パラメーターの値で session オプションを使用し `New-PSSession` `Invoke-Command` ます。

セッションの作成時に設定される値は、ユーザー設定変数およびセッション構成で設定された値よりも優先され `$PSSessionOption` ます。

次に例を示します。

```powershell
$o = New-PSSessionOption -IdleTimeoutMSec 172800000
New-PSSession -SessionOption $o
```

切断時の PSSession のアイドルタイムアウトを変更するには、コマンドレットの **IdleTimeoutSec** パラメーターを使用し `Disconnect-PSSession` ます。

次に例を示します。

```powershell
Disconnect-PSSession -IdleTimeoutSec 172800
```

特定のアイドルタイムアウトと最大アイドルタイムアウトを使用してセッション構成を作成するには、コマンドレットの **IdleTimeoutSec** パラメーターと **MaxIdleTimeoutSec** パラメーターを使用し `New-PSTransportOption` ます。 次に、の **TransportOption** パラメーターの値で transport オプションを使用し `Register-PSSessionConfiguration` ます。

次に例を示します。

```powershell
$o = New-PSTransportOption -IdleTimeoutSec 172800 -MaxIdleTimeoutSec 259200
Register-PSSessionConfiguration -Name Test -TransportOption $o
```

セッション構成の既定のアイドルタイムアウトと最大アイドルタイムアウトを変更するには、コマンドレットの **IdleTimeoutSec** パラメーターと **MaxIdleTimeoutSec** パラメーターを使用し `New-PSTransportOption` ます。 次に、の **TransportOption** パラメーターの値で transport オプションを使用し `Set-PSSessionConfiguration` ます。

次に例を示します。

```powershell
$o = New-PSTransportOption -IdleTimeoutSec 172800 -MaxIdleTimeoutSec 259200
Set-PSSessionConfiguration -Name Test -TransportOption $o
```

## <a name="output-buffering-mode"></a>出力バッファーモード

Pssession の出力バッファーモードによって、PSSession の出力バッファーがいっぱいになったときのコマンド出力の管理方法が決まります。

切断されたセッションでは、出力バッファーモードは、セッションが切断されている間もコマンドを実行し続けるかどうかを効果的に判断します。

有効な値は次のとおりです。

- **[ブロック]** 。 出力バッファーがいっぱいのとき、バッファーが空くまで実行が中断されます。 既定値。
- **[削除]** 。 出力バッファーがいっぱいのとき、実行は続行されます。 新しい出力が生成されると、最も古い出力が破棄されます。

**ブロック** はデータを保持しますが、コマンドを中断する可能性があります。 値が **Drop** の場合は、データが失われる可能性はありますが、コマンドを完了できます。 **Drop** 値を使用すると、コマンド出力がディスク上のファイルにリダイレクトされます。 この値は、切断されたセッションにお勧めします。

セッション構成の **OutputBufferingMode** プロパティによって、セッション構成を使用するセッションの既定の出力バッファーモードが決まります。

**OutputBufferingMode** のセッション構成の値を検索するには、次のいずれかのコマンド形式を使用できます。

```powershell
(Get-PSSessionConfiguration <ConfigurationName>).OutputBufferingMode
```

```powershell
Get-PSSessionConfiguration | Format-Table Name, OutputBufferingMode
```

セッション構成の既定値を上書きし、PSSession の作成時、切断時、再接続時に PSSession の出力バッファーモードを設定することができます。

リモートコンピューターの Administrators グループのメンバーである場合は、セッション構成の出力バッファーモードを作成および変更できます。

出力バッファリングモードが **drop** の PSSession を作成するに `$PSSessionOption` は、 **OutputBufferingMode** プロパティの値が **drop** に設定されているユーザー設定変数を作成します。

PSSessions を作成すると、変数の値は、 `$PSSessionOption` セッション構成の値よりも優先されます。

次に例を示します。

```powershell
$PSSessionOption = New-PSSessionOption -OutputBufferingMode Drop
```

出力バッファリングモード **drop** を使用して PSSession を作成するには、コマンドレットの **OutputBufferingMode** パラメーターを使用して、 `New-PSSessionOption` 値 **drop** を指定したセッションオプションを作成します。 次に、またはコマンドレットの **sessionoption** パラメーターの値で session オプションを使用し `New-PSSession` `Invoke-Command` ます。

セッションの作成時に設定される値は、ユーザー設定変数およびセッション構成で設定された値よりも優先され `$PSSessionOption` ます。

次に例を示します。

```powershell
$o = New-PSSessionOption -OutputBufferingMode Drop
New-PSSession -SessionOption $o
```

切断時に PSSession の出力バッファーモードを変更するには、コマンドレットの **OutputBufferingMode** パラメーターを使用し `Disconnect-PSSession` ます。

次に例を示します。

```powershell
Disconnect-PSSession -OutputBufferingMode Drop
```

再接続時に PSSession の出力バッファーモードを変更するには、コマンドレットの **OutputBufferingMode** パラメーターを使用し `New-PSSessionOption` て、値 **Drop** を指定したセッションオプションを作成します。 次に、またはの **sessionoption** パラメーターの値で session オプションを使用し `Connect-PSSession` `Receive-PSSession` ます。

次に例を示します。

```powershell
$o = New-PSSessionOption -OutputBufferingMode Drop
Connect-PSSession -ComputerName Server01 -Name Test -SessionOption $o
```

既定の出力バッファーモード **drop** を使用してセッション構成を作成するには、コマンドレットの **OutputBufferingMode** パラメーターを使用して、 `New-PSTransportOption` 値 **drop** を指定してトランスポートオプションオブジェクトを作成します。 次に、の **TransportOption** パラメーターの値で transport オプションを使用し `Register-PSSessionConfiguration` ます。

次に例を示します。

```powershell
$o = New-PSTransportOption -OutputBufferingMode Drop
Register-PSSessionConfiguration -Name Test -TransportOption $o
```

セッション構成の既定の出力バッファーモードを変更するには、コマンドレットの **OutputBufferingMode** パラメーターを使用し `New-PSTransportOption` て、値 **Drop** を指定したトランスポートオプションを作成します。 次に、の **sessionoption** パラメーターの値で Transport オプションを使用し `Set-PSSessionConfiguration` ます。

次に例を示します。

```powershell
$o = New-PSTransportOption -OutputBufferingMode Drop
Set-PSSessionConfiguration -Name Test -TransportOption $o
```

## <a name="disconnecting-loopback-sessions"></a>ループバックセッションの切断

ループバックセッション (ローカルセッション) は、同じコンピューター上で開始および終了する pssession です。 他の PSSessions と同様に、アクティブなループバックセッションは接続のリモートエンド (ローカルコンピューター) 上のコンピューターで保持されるため、ループバックセッションに接続して再接続することができます。

既定では、ループバックセッションは、他のコンピューターにアクセスするためにセッションで実行されるコマンドを許可しないネットワークセキュリティトークンを使用して作成されます。 ローカルコンピューターまたはリモートコンピューター上の任意のセッションから、ネットワークセキュリティトークンを持つループバックセッションに再接続できます。

ただし、、、またはコマンドレットの **EnableNetworkAccess** パラメーターを使用すると、 `New-PSSession` `Enter-PSSession` `Invoke-Command` 対話的なセキュリティトークンを使用してループバックセッションが作成されます。 対話型トークンを使用すると、ループバックセッションで実行されるコマンドで他のコンピューターからデータを取得できます。

対話型トークンを使用してループバックセッションを切断し、同じセッションまたは同じコンピューター上の別のセッションからそれらに再接続することができます。
ただし、悪意のあるアクセスを防ぐために、対話的なトークンを使用して、作成されたコンピューターからのみループバックセッションに再接続することができます。

## <a name="waiting-for-jobs-in-disconnected-sessions"></a>切断されたセッションのジョブを待機しています

コマンド `Wait-Job` レットは、ジョブが完了するまで待機してから、コマンドプロンプトまたは次のコマンドに戻ります。 既定では、ジョブが実行されている `Wait-Job` セッションが切断されている場合、はを返します。 `Wait-Job`セッションが再接続されるまで待機するようにコマンドレットに指示するには、 **Opened** 状態で **Force** パラメーターを使用します。 詳細については、「 [待機-ジョブ](xref:Microsoft.PowerShell.Core.Wait-Job)」を参照してください。

## <a name="robust-sessions-and-unintentional-disconnection"></a>堅牢なセッションと意図しない切断

PSSession は、コンピューターの障害やネットワークの停止が原因で、意図せず切断される可能性があります。 PowerShell は PSSession の復旧を試みますが、その成功は原因の重大度と期間によって異なります。

意図せず切断された PSSession の状態は、 **壊れ** ているか **閉じ** られている可能性がありますが、 **切断** されることもあります。 **State** の値が **disconnected** の場合は、セッションが意図的に切断された場合と同じ手法を使用して PSSession を管理できます。 たとえば、コマンドレットを使用してセッションに再接続し、コマンドレットを使用して、 `Connect-PSSession` `Receive-PSSession` セッションの切断中に実行されたコマンドの結果を取得することができます。

コマンドが PSSession で実行されているときに PSSession が作成されたセッションを終了 (終了) した場合、PowerShell は、リモートコンピューター上の **Disconnected** 状態の pssession を保持します。 PSSession が作成されたセッションを終了 (終了) したが、PSSession で実行されているコマンドが存在しない場合、PowerShell は PSSession を維持しません。

## <a name="see-also"></a>関連項目

[about_Jobs](about_Jobs.md)

[about_Remote](about_Remote.md)

[about_Remote_Variables](about_Remote_Variables.md)

[about_PSSessions](about_PSSessions.md)

[about_Session_Configurations](about_Session_Configurations.md)

[Connect-PSSession](xref:Microsoft.PowerShell.Core.Connect-PSSession)

[Disconnect-PSSession](xref:Microsoft.PowerShell.Core.Disconnect-PSSession)

[Get-PSSession](xref:Microsoft.PowerShell.Core.Get-PSSession)

[Receive-PSSession](xref:Microsoft.PowerShell.Core.Receive-PSSession)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

