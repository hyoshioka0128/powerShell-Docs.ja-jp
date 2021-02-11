---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/12/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/test-connection?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-Connection
ms.openlocfilehash: af8a953536bb9683ba737522ad738348c5c695e2
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599537"
---
# Test-Connection

## 概要
ICMP エコー要求パケット (ping) を1台以上のコンピューターに送信します。

## SYNTAX

### DefaultPing (既定)

```
Test-Connection [-TargetName] <string[]> [-Ping] [-IPv4] [-IPv6] [-ResolveDestination]
 [-Source <string>] [-MaxHops <int>] [-Count <int>] [-Delay <int>] [-BufferSize <int>]
 [-DontFragment] [-TimeoutSeconds <int>] [-Quiet] [<CommonParameters>]
```

### RepeatPing

```
Test-Connection [-TargetName] <string[]> -Repeat [-Ping] [-IPv4] [-IPv6] [-ResolveDestination]
 [-Source <string>] [-MaxHops <int>] [-Delay <int>] [-BufferSize <int>] [-DontFragment]
 [-TimeoutSeconds <int>] [-Quiet] [<CommonParameters>]
```

### MtuSizeDetect

```
Test-Connection [-TargetName] <string[]> -MtuSize [-IPv4] [-IPv6] [-ResolveDestination]
 [-TimeoutSeconds <int>] [-Quiet] [<CommonParameters>]
```

### TraceRoute

```
Test-Connection [-TargetName] <string[]> -Traceroute [-IPv4] [-IPv6] [-ResolveDestination]
 [-Source <string>] [-MaxHops <int>] [-TimeoutSeconds <int>] [-Quiet] [<CommonParameters>]
```

### TcpPort

```
Test-Connection [-TargetName] <string[]> -TcpPort <int> [-IPv4] [-IPv6] [-ResolveDestination]
 [-Source <string>] [-TimeoutSeconds <int>] [-Quiet] [<CommonParameters>]
```

## Description

`Test-Connection`コマンドレットは、インターネット制御メッセージプロトコル (ICMP) エコー要求パケット (ping) を1つ以上のリモートコンピューターに送信し、エコー応答応答を返します。 このコマンドレットを使用すると、特定のコンピューターに IP ネットワーク経由で接続できるかどうかを判断できます。

のパラメーターを使用し `Test-Connection` て、送信側と受信側の両方のコンピューターの指定、バックグラウンドジョブとしてのコマンドの実行、タイムアウトと ping の回数の設定、および接続と認証の構成を行うことができます。

使い慣れた **ping** コマンドとは異なり、は、 `Test-Connection` PowerShell で調査できる **Testconnectioncommand + ping status** オブジェクトを返します。 **Quiet** パラメーターは、テストされた各接続の **ブール値を** **system.string オブジェクトで返します。** 複数の接続がテストされている場合は、 **ブール** 値の配列が返されます。

## 例

### 例 1: リモートコンピューターにエコー要求を送信する

この例では、エコー要求パケットをローカルコンピューターから Server01 コンピューターに送信します。

```powershell
Test-Connection -TargetName Server01 -IPv4
```

```Output
   Destination: Server01

Ping Source           Address                   Latency BufferSize Status
                                                   (ms)        (B)
---- ------           -------                   ------- ---------- ------
   1 ADMIN1           10.59.137.44                   24         32 Success
   2 ADMIN1           10.59.137.44                   39         32 Success
   3 ADMIN1           *                               *          * TimedOut
   4 ADMIN1           10.59.137.44                   28         32 Success
```

`Test-Connection`**TargetName** パラメーターを使用して、Server01 コンピューターを指定します。 **IPv4** パラメーターでは、テスト用のプロトコルを指定します。

一連の **Testconnectioncommand + Ping status** オブジェクトが出力ストリームに送信されます。これは、ターゲットコンピューターからの ping 応答ごとに1つのオブジェクトです。

### 例 2: 複数のコンピューターにエコー要求を送信する

この例では、ローカルコンピューターから複数のリモートコンピューターに ping を送信します。

```powershell
Test-Connection -TargetName Server01, Server02, Server12
```

### 例 3: パラメーターを使用してテストコマンドをカスタマイズする

この例では、のパラメーターを使用し `Test-Connection` て、コマンドをカスタマイズします。 ローカルコンピューターからリモートコンピューターに ping テストが送信されます。

```powershell
Test-Connection -TargetName Server01 -Count 3 -Delay 2 -MaxHops 255 -BufferSize 256
```

`Test-Connection`**TargetName** パラメーターを使用して Server01 を指定します。 **Count** パラメーターは、3秒間隔の **遅延** で Server01 コンピューターに送信される3つの ping を指定します。

これらのオプションは、ホップ数の増加またはトラフィックの多いネットワーク状態が原因で ping 応答が通常よりも長くかかることが予想される場合に使用できます。

### 例 4: バックグラウンドジョブとしてテストを実行する

この例では、 `Test-Connection` PowerShell バックグラウンドジョブとしてコマンドを実行する方法を示します。

```powershell
$job = Start-Job -ScriptBlock { Test-Connection -TargetName (Get-Content -Path "Servers.txt") }
$Results = Receive-Job $job -Wait
```

このコマンドは、コマンド `Start-Job` レットを使用して、 `Test-Connection` 企業内の多くのコンピューターに ping を実行します。
**TargetName** パラメーターの値は、 `Get-Content` ファイルからコンピューター名の一覧を読み取るコマンドです `Servers.txt` 。 コマンドはコマンドレットを使用し `Start-Job` てバックグラウンドジョブとしてコマンドを実行し、変数にジョブを保存し `$job` ます。

この `Receive-Job` コマンドは、ジョブが完了するまでに指示され、 `-Wait` 結果を取得して変数に格納し `$Results` ます。

### 例 5: 接続テストが成功した場合にのみセッションを作成する

この例では、コンピューターに送信された ping の少なくとも1つが成功した場合にのみ、Server01 コンピューターでセッションを作成します。

```powershell
if (Test-Connection -TargetName Server01 -Quiet) { New-PSSession -ComputerName Server01 }
```

`Test-Connection`コマンドレットは、 `Server01` **Quiet** パラメーターを指定してコンピューターに ping を行います。
`$True`4 つの ping のいずれかが成功した場合、結果の値はになります。 Ping が成功しなかった場合、値はに `$False` なります。

コマンドが `Test-Connection` 値を返す場合 `$True` 、コマンドはコマンドレットを使用して `New-PSSession` **PSSession** を作成します。

### 例 6: Traceroute パラメーターを使用する

PowerShell 6.0 で導入された **Traceroute** パラメーターは、ローカルコンピューターと、 **TargetName** パラメーターを使用して指定したリモート接続先との間のルートをマップします。

```powershell
Test-Connection -TargetName www.google.com -Traceroute
```

```Output
   Target: google.com

Hop Hostname                  Ping Latency Status           Source       TargetAddress
                                      (ms)
--- --------                  ---- ------- ------           ------       -------------
  1 172.20.0.1                   1       4 Success          Lira         172.217.9.174
  1 172.20.0.1                   2       3 Success          Lira         172.217.9.174
  1 172.20.0.1                   3       2 Success          Lira         172.217.9.174
  2 12.108.153.193               1       3 Success          Lira         172.217.9.174
  2 12.108.153.193               2       3 Success          Lira         172.217.9.174
  2 12.108.153.193               3       2 Success          Lira         172.217.9.174
  3 12.244.85.177                1      11 Success          Lira         172.217.9.174
  3 12.244.85.177                2      12 Success          Lira         172.217.9.174
  3 12.244.85.177                3      12 Success          Lira         172.217.9.174
  4 *                            1      14 DestinationNetw… Lira         172.217.9.174
  4 *                            2       * TimedOut         Lira         172.217.9.174
  4 *                            3      20 DestinationNetw… Lira         172.217.9.174
  5 *                            1       * TimedOut         Lira         172.217.9.174
  5 *                            2      15 DestinationNetw… Lira         172.217.9.174
  5 *                            3       * TimedOut         Lira         172.217.9.174
  6 *                            1      18 DestinationNetw… Lira         172.217.9.174
  6 *                            2       * TimedOut         Lira         172.217.9.174
  6 *                            3      16 DestinationNetw… Lira         172.217.9.174
  7 *                            1       * TimedOut         Lira         172.217.9.174
  7 *                            2       * TimedOut         Lira         172.217.9.174
  7 *                            3       * TimedOut         Lira         172.217.9.174
  8 *                            1       * TimedOut         Lira         172.217.9.174
  8 *                            2       * TimedOut         Lira         172.217.9.174
  8 *                            3       * TimedOut         Lira         172.217.9.174
  9 *                            1       * TimedOut         Lira         172.217.9.174
  9 *                            2       * TimedOut         Lira         172.217.9.174
  9 *                            3       * TimedOut         Lira         172.217.9.174
 10 *                            1       * TimedOut         Lira         172.217.9.174
 10 *                            2       * TimedOut         Lira         172.217.9.174
 10 *                            3       * TimedOut         Lira         172.217.9.174
 11 172.217.9.174                1      23 Success          Lira         172.217.9.174
 11 172.217.9.174                2      21 Success          Lira         172.217.9.174
 11 172.217.9.174                3      22 Success          Lira         172.217.9.174
```

`Test-Connection`コマンドは、 **Traceroute** パラメーターを使用して呼び出されます。 結果 ( `[Microsoft.PowerShell.Commands.TestConnectionCommand+TraceStatus]` オブジェクト) は、 **成功** 出力ストリームに出力されます。

## PARAMETERS

### -BufferSize

このコマンドで送信されるバッファーのサイズをバイト単位で指定します。 既定値は 32 です。

```yaml
Type: System.Int32
Parameter Sets: DefaultPing, RepeatPing
Aliases: Size, Bytes, BS

Required: False
Position: Named
Default value: 32
Accept pipeline input: False
Accept wildcard characters: False
```

### -Repeat

コマンドレットによって ping 要求が連続して送信されます。 このパラメーターは、 **Count** パラメーターと共に使用することはできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: RepeatPing
Aliases: Continuous

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -カウント

送信するエコー要求の数を指定します。 既定値は 4 ですが、

```yaml
Type: System.Int32
Parameter Sets: DefaultPing
Aliases:

Required: False
Position: Named
Default value: 4
Accept pipeline input: False
Accept wildcard characters: False
```

### -Delay

ping 間の間隔を秒単位で指定します。

```yaml
Type: System.Int32
Parameter Sets: DefaultPing, RepeatPing
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DontFragment

このパラメーターは、IP ヘッダーに don't **Fragment** フラグを設定します。 このパラメーターを **BufferSize** パラメーターと共に使用して、パスの MTU サイズをテストできます。 パス MTU の詳細については、wikipedia の [パス Mtu 検出](https://wikipedia.org/wiki/Path_MTU_Discovery) に関する記事を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DefaultPing, RepeatPing
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -IPv4

テストに IPv4 プロトコルを使用するようにコマンドレットを強制します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -IPv6

テストに IPv6 プロトコルを使用するようにコマンドレットを強制します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaxHops

ICMP 要求メッセージを送信できる最大ホップ数を設定します。 既定値は、オペレーティングシステムによって制御されます。 Windows 10 の既定値は128ホップです。

```yaml
Type: System.Int32
Parameter Sets: DefaultPing, RepeatPing, TraceRoute
Aliases: Ttl, TimeToLive, Hops

Required: False
Position: Named
Default value: 128 hops in Windows 10
Accept pipeline input: False
Accept wildcard characters: False
```

### -Ping

コマンドレットによって ping テストが実行されます。 これは、コマンドレットの既定のモードです `Test-Connection` 。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DefaultPing, RepeatPing
Aliases:

Required: False
Position: Named
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

### -非表示

**Quiet** パラメーターは **ブール** 値を返します。 このパラメーターを使用すると、すべてのエラーが抑制します。

テストされた各接続は、 **ブール** 値を返します。 **TargetName** パラメーターに複数のコンピューターが指定されている場合は、**ブール** 値の配列が返されます。

特定のターゲットへの ping が成功 **した場合** は、 `$True` が返されます。

特定のターゲットへの ping が **すべて** 失敗した場合 `$False` は、が返されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResolveDestination

コマンドレットがターゲットの DNS 名を解決しようとします。 **Traceroute** パラメーターと組み合わせて使用すると、可能であれば、すべての中間ホストの DNS 名も取得されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Source

ping の送信元コンピューターの名前を指定します。 コンピューター名のコンマ区切りのリストを入力します。 既定値はローカル コンピューターです。

**注:** このパラメーターは、PowerShell バージョン6以降では機能しません。
このパラメーターを指定しても、コマンドには影響しません。

```yaml
Type: System.String
Parameter Sets: DefaultPing, RepeatPing, TraceRoute, TcpPort
Aliases:

Required: False
Position: Named
Default value: Local computer
Accept pipeline input: False
Accept wildcard characters: False
```

### -TargetName

テストするコンピューターを指定します。 コンピューター名を入力するか、IP アドレスを IPv4 または IPv6 形式で入力します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: ComputerName

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -TimeoutSeconds

テストのタイムアウト値を設定します。 タイムアウトの期限が切れる前に応答を受信しなかった場合、テストは失敗します。 既定値は 5 秒です。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 5 seconds
Accept pipeline input: False
Accept wildcard characters: False
```

### -Traceroute

コマンドレットが traceroute テストを実行します。 このパラメーターを使用すると、コマンドレットは `TestConnectionCommand+TraceStatus` オブジェクトを返します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: TraceRoute
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -MtuSize

このパラメーターは、パスの MTU サイズを検出するために使用されます。 コマンドレットは、ターゲットにパスの MTU サイズを含む、 **ping # MTUSize** オブジェクトを返します。 パス MTU の詳細については、wikipedia の [パス Mtu 検出](https://wikipedia.org/wiki/Path_MTU_Discovery) に関する記事を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: MtuSizeDetect
Aliases: MtuSizeDetect

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -TcpPort

TCP 接続テストで使用するターゲットの TCP ポート番号を指定します。 コマンドレットは、ターゲットの指定されたポートへの TCP 接続を試行します。

接続を確立できる場合は、 `$True` が返されます。

接続を確立できない場合は、 `$False` が返されます。

```yaml
Type: System.Int32
Parameter Sets: TcpPort
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットに入力をパイプすることはできません。

## 出力

### TestConnectionCommand + Ping Status、TestConnectionCommand + TraceStatus、Boolean、TestConnectionCommand + PingMtuStatus

既定では、は、 `Test-Connection` 各 ping 応答の **Testconnectioncommand + ping status** オブジェクトを返します。

**Traceroute** パラメーターを指定すると、コマンドレットは、ルートに沿って ping 応答ごとに **testconnectioncommand + tracestatus** オブジェクトを返します。

**Quiet** パラメーターまたは **TcpPort** パラメーターを指定すると、**ブール** 値が返されます。 複数の接続がテストされている場合は、 **ブール** 値の配列が返されます。

## 注

## 関連リンク

[Restart-Computer](Restart-Computer.md)

[Stop-Computer](Stop-Computer.md)

