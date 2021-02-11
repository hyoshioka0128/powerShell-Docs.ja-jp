---
external help file: Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Diagnostics
ms.date: 11/06/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.diagnostics/get-counter?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Counter
ms.openlocfilehash: 4aed8db557b2d623aba4cd7218524af9957674c9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599294"
---
# Get-Counter

## 概要
ローカル コンピューターおよびリモート コンピューターからパフォーマンス カウンターのデータを取得します。

## SYNTAX

### GetCounterSet (既定値)

```
Get-Counter [[-Counter] <String[]>] [-SampleInterval <Int32>] [-MaxSamples <Int64>] [-Continuous]
 [-ComputerName <String[]>] [<CommonParameters>]
```

### ListSetSet

```
Get-Counter [-ListSet] <String[]> [-ComputerName <String[]>] [<CommonParameters>]
```

## Description

`Get-Counter`コマンドレットは、Windows ファミリのオペレーティングシステムのパフォーマンス監視インストルメンテーションからパフォーマンスカウンターデータを直接取得します。 `Get-Counter` ローカルコンピューターまたはリモートコンピューターからパフォーマンスデータを取得します。

パラメーターを使用して、 `Get-Counter` 1 つまたは複数のコンピューターを指定したり、パフォーマンスカウンターセットとそれらに含まれるインスタンスを一覧表示したり、サンプル間隔を設定したり、サンプルの最大数を指定したりすることができます。 パラメーターを指定しない場合は、 `Get-Counter` 一連のシステムカウンターのパフォーマンスカウンターデータを取得します。

多くのカウンターセットは、アクセス制御リスト (ACL) によって保護されています。 すべてのカウンターセットを表示するには、[ **管理者として実行** ] オプションを使用して PowerShell を開きます。

このコマンドレットは、PowerShell 7 で再導入されました。

## 例

### 例 1: カウンターセットの一覧を取得する

この例では、ローカルコンピューターのカウンターセットの一覧を取得します。

```powershell
Get-Counter -ListSet *
```

```Output
CounterSetName     : Processor
MachineName        : .
CounterSetType     : MultiInstance
Description        : The Processor performance object consists of counters that measure aspects ...
                     computer that performs arithmetic and logical computations, initiates ...
                     computer can have multiple processors.  The processor object represents ...
Paths              : {\Processor(*)\% Processor Time, \Processor(*)\% User Time, ...
PathsWithInstances : {\Processor(0)\% Processor Time, \Processor(1)\% Processor Time, ...
Counter            : {\Processor(*)\% Processor Time, \Processor(*)\% User Time, ...
```

`Get-Counter`**Listset** パラメーターをアスタリスク () と共に使用して `*` 、カウンターセットの一覧を取得します。
MachineName 列のドット ( `.` ) は、ローカルコンピューターを表します。

### 例 2: SampleInterval と MaxSamples を指定する

この例では、ローカルコンピューター上のすべてのプロセッサのカウンターデータを取得します。 データは、3つのサンプルが存在するまで2秒間隔で収集されます。

```powershell
Get-Counter -Counter "\Processor(_Total)\% Processor Time" -SampleInterval 2 -MaxSamples 3
```

```Output
Timestamp                 CounterSamples
---------                 --------------
6/18/2019 14:39:56        \\Computer01\processor(_total)\% processor time :
                          20.7144271584086

6/18/2019 14:39:58        \\Computer01\processor(_total)\% processor time :
                          10.4391790575511

6/18/2019 14:40:01        \\Computer01\processor(_total)\% processor time :
                          37.5968799396998
```

`Get-Counter`**counter パラメーターを** 使用して、カウンターのパスを指定し `\Processor(_Total)\% Processor Time` ます。 **Sampleinterval** パラメーターは、カウンターをチェックする2秒間隔を設定します。 **Maxsamples** によって、カウンターをチェックする最大回数が3であることが決定されます。

### 例 3: カウンターの継続的なサンプルを取得する

この例では、1秒ごとにカウンターの連続サンプルを取得します。 コマンドを停止するには、 <kbd>ctrl</kbd> + <kbd>C</kbd>キーを押します。 サンプル間に長い間隔を指定するには、 **sampleinterval** パラメーターを使用します。

```powershell
Get-Counter -Counter "\Processor(_Total)\% Processor Time" -Continuous
```

```Output
Timestamp                 CounterSamples
---------                 --------------
6/19/2019 15:35:03        \\Computer01\processor(_total)\% processor time :
                          43.8522842937022

6/19/2019 15:35:04        \\Computer01\processor(_total)\% processor time :
                          29.7896844697383

6/19/2019 15:35:05        \\Computer01\processor(_total)\% processor time :
                          29.4962645638135

6/19/2019 15:35:06        \\Computer01\processor(_total)\% processor time :
                          25.5901500127408
```

`Get-Counter`**counter パラメーターを** 使用してカウンタを指定し `\Processor\% Processor Time` ます。
**Continuous** パラメーターは、コマンドが <kbd>CTRL</kbd>C で停止されるまで、1秒ごとにサンプルを取得することを指定し + <kbd></kbd>ます。

### 例 4: カウンターセットのアルファベット順の一覧

この例では、パイプラインを使用してカウンターの一覧を取得し、リストをアルファベット順に並べ替えます。

```powershell
Get-Counter -ListSet * |
  Sort-Object -Property CounterSetName |
    Format-Table CounterSetName, CounterSetType -AutoSize
```

```Output
CounterSetName                        CounterSetType
--------------                        --------------
.NET CLR Data                         SingleInstance
.NET Data Provider for SqlServer      SingleInstance
AppV Client Streamed Data Percentage  SingleInstance
Authorization Manager Applications    SingleInstance
BitLocker                             MultiInstance
Bluetooth Device                      SingleInstance
Cache                                 SingleInstance
Client Side Caching                   SingleInstance
```

`Get-Counter`**listset** パラメーターをアスタリスク () と共に使用して、 `*` カウンターセットの完全な一覧を取得します。 **CounterSet** オブジェクトは、パイプラインを介して送信されます。 `Sort-Object`**Property** パラメーターを使用して、オブジェクトが **CounterSetName** によって並べ替えられるように指定します。 オブジェクトは、パイプラインの下位に送信され `Format-Table` ます。 **AutoSize** パラメーターを指定すると、列の幅が調整され、切り捨てが最小限に抑えられます。

MachineName 列のドット ( `.` ) は、ローカルコンピューターを表します。

### 例 5: バックグラウンドジョブを実行してカウンターデータを取得する

この例では、は、 `Start-Job` `Get-Counter` ローカルコンピューター上でバックグラウンドジョブとしてコマンドを実行します。
ジョブからのパフォーマンスカウンターの出力を表示するには、コマンドレットを使用し `Receive-Job` ます。

```powershell
Start-Job -ScriptBlock {Get-Counter -Counter "\LogicalDisk(_Total)\% Free Space" -MaxSamples 1000}
```

```Output
Id     Name  PSJobTypeName   State    HasMoreData  Location   Command
--     ----  -------------   -----    -----------  --------   -------
1      Job1  BackgroundJob   Running  True         localhost  Get-Counter -Counter
```

`Start-Job`**ScriptBlock** パラメーターを使用してコマンドを実行 `Get-Counter` します。 `Get-Counter`**counter パラメーターを** 使用して、カウンターのパスを指定し `\LogicalDisk(_Total)\% Free Space` ます。 **Maxsamples** パラメーターでは、カウンターの1000サンプルを取得するように指定します。

### 例 6: 複数のコンピューターからカウンターデータを取得する

この例では、変数を使用して、2台のコンピューターからパフォーマンスカウンターデータを取得します。

```powershell
$DiskReads = "\LogicalDisk(C:)\Disk Reads/sec"
$DiskReads | Get-Counter -ComputerName Server01, Server02 -MaxSamples 10
```

```Output
Timestamp                 CounterSamples
---------                 --------------
6/21/2019 10:51:04        \\Server01\logicaldisk(c:)\disk reads/sec :
                          0

                          \\Server02\logicaldisk(c:)\disk reads/sec :
                          0.983050344269146
```

変数には `$DiskReads` 、カウンターパスが格納され `\LogicalDisk(C:)\Disk Reads/sec` ます。 変数は、パイプラインを介し `$DiskReads` てに送信され `Get-Counter` ます。 **Counter** は最初の位置パラメーターであり、に格納されているパスを受け入れ `$DiskReads` ます。 **ComputerName** は2台のコンピューターを指定し、 **maxsamples** は各コンピューターから10個のサンプルを取得するように指定します。

### 例 7: 複数のランダムなコンピューターからカウンターのインスタンス値を取得する

この例では、企業内のリモートコンピューター50のパフォーマンスカウンターの値を取得します。 **ComputerName** パラメーターは、変数に格納されているランダムなコンピューター名を使用します。 変数内のコンピューター名を更新するには、変数を再作成します。

**ComputerName** パラメーターのサーバー名の代わりに、テキストファイルを使用することもできます。 次に例を示します。

`-ComputerName (Get-Random (Get-Content -Path C:\Servers.txt) -Count 50)`

カウンターパスには、 `*` 各リモートコンピューターのプロセッサのデータを取得するために、インスタンス名にアスタリスク () が含まれています。

```powershell
$Servers = Get-Random (Get-Content -Path C:\Servers.txt) -Count 50
$Counter = "\Processor(*)\% Processor Time"
Get-Counter -Counter $Counter -ComputerName $Servers
```

```Output
Timestamp                 CounterSamples
---------                 --------------
6/20/2019 12:20:35        \\Server01\processor(0)\% processor time :
                          6.52610319637854

                          \\Server01\processor(1)\% processor time :
                          3.41030663625782

                          \\Server01\processor(2)\% processor time :
                          9.64189975649925

                          \\Server01\processor(3)\% processor time :
                          1.85240835619747

                          \\Server01\processor(_total)\% processor time :
                          5.35768447160776
```

`Get-Random`コマンドレットでは、を使用し `Get-Content` て、ファイルから50のランダムなコンピューター名を選択し `Servers.txt` ます。 リモートコンピューター名は変数に格納され `$Servers` ます。 `\Processor(*)\% Processor Time`カウンターのパスは変数に格納され `$Counter` ます。 `Get-Counter`**Counter** パラメーターを使用して、変数内のカウンターを指定し `$Counter` ます。 **ComputerName** パラメーターは、変数内のコンピューター名を指定し `$Servers` ます。

### 例 8: Path プロパティを使用して、書式設定されたパス名を取得する

この例では、カウンターセットの **path** プロパティを使用して、パフォーマンスカウンターの書式設定されたパス名を検索します。

パイプラインをコマンドレットと共に使用して、 `Where-Object` パス名のサブセットを検索します。 カウンターのパスの完全な一覧を検索するには、パイプライン ( `|` ) とコマンドを削除し `Where-Object` ます。

`$_`は、パイプライン内の現在のオブジェクトの自動変数です。
詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

```powershell
(Get-Counter -ListSet Memory).Paths | Where-Object { $_ -like "*Cache*" }
```

```Output
\Memory\Cache Faults/sec
\Memory\Cache Bytes
\Memory\Cache Bytes Peak
\Memory\System Cache Resident Bytes
\Memory\Standby Cache Reserve Bytes
\Memory\Standby Cache Normal Priority Bytes
\Memory\Standby Cache Core Bytes
\Memory\Long-Term Average Standby Cache Lifetime (s)
```

`Get-Counter`**listset** パラメーターを使用して、**メモリ** カウンターセットを指定します。 コマンドはかっこで囲まれているため、 **Paths** プロパティは各パスを文字列として返します。 オブジェクトは、パイプラインの下位に送信され `Where-Object` ます。 `Where-Object` 変数を使用 `$_` して各オブジェクトを処理し、演算子を使用して `-like` 文字列の一致を検索し `*Cache*` ます。 アスタリスク ( `*` ) は、任意の文字に対するワイルドカードです。

### 例 9: PathsWithInstances プロパティを使用して、書式設定されたパス名を取得する

この例では、 **PhysicalDisk** パフォーマンスカウンターのインスタンスを含む、書式設定されたパス名を取得します。

```powershell
(Get-Counter -ListSet PhysicalDisk).PathsWithInstances
```

```Output
\PhysicalDisk(0 C:)\Current Disk Queue Length
\PhysicalDisk(_Total)\Current Disk Queue Length
\PhysicalDisk(0 C:)\% Disk Time
\PhysicalDisk(_Total)\% Disk Time
\PhysicalDisk(0 C:)\Avg. Disk Queue Length
\PhysicalDisk(_Total)\Avg. Disk Queue Length
\PhysicalDisk(0 C:)\% Disk Read Time
\PhysicalDisk(_Total)\% Disk Read Time
```

`Get-Counter`**listset** パラメーターを使用して、 **PhysicalDisk** カウンターセットを指定します。 このコマンドは、 **PathsWithInstances** プロパティが各パスインスタンスを文字列として返すように、かっこで囲まれています。

### 例 10: カウンターセット内のカウンターごとに1つの値を取得する

この例では、ローカルコンピューターの **メモリ** カウンターセット内のパフォーマンスカウンターごとに1つの値が返されます。

```powershell
$MemCounters = (Get-Counter -ListSet Memory).Paths
Get-Counter -Counter $MemCounters
```

```Output
Timestamp                 CounterSamples
---------                 --------------
6/19/2019 12:05:00        \\Computer01\memory\page faults/sec :
                          868.772077545597

                          \\Computer01\memory\available bytes :
                          9031176192

                          \\Computer01\memory\committed bytes :
                          8242982912

                          \\Computer01\memory\commit limit :
                          19603333120
```

`Get-Counter`**listset** パラメーターを使用して、**メモリ** カウンターセットを指定します。 コマンドはかっこで囲まれているため、 **Paths** プロパティは各パスを文字列として返します。 パスは変数に格納され `$MemCounters` ます。 `Get-Counter`**counter** パラメーターを使用して、変数内のカウンターパスを指定し `$MemCounters` ます。

### 例 11: オブジェクトのプロパティ値を表示する

**PerformanceCounterSample** オブジェクトのプロパティ値は、各データサンプルを表します。 この例では、 **Countersamples** オブジェクトのプロパティを使用して、データの確認、選択、並べ替え、およびグループ化を行います。

```powershell
$Counter = "\\Server01\Process(Idle)\% Processor Time"
$Data = Get-Counter $Counter
$Data.CounterSamples | Format-List -Property *
```

```Output
Path             : \\Server01\process(idle)\% processor time
InstanceName     : idle
CookedValue      : 198.467899571389
RawValue         : 14329160321003
SecondValue      : 128606459528326201
MultipleCount    : 1
CounterType      : Timer100Ns
Timestamp        : 6/19/2019 12:20:49
Timestamp100NSec : 128606207528320000
Status           : 0
DefaultScale     : 0
TimeBase         : 10000000
```

カウンターパスは変数に格納され `$Counter` ます。 `Get-Counter` カウンター値の1つのサンプルを取得し、その結果を変数に格納し `$Data` ます。 変数は、 `$Data` **countersamples** プロパティを使用して、オブジェクトのプロパティを取得します。 オブジェクトは、パイプライン内でに送信され `Format-List` ます。 **Property** パラメーターは、アスタリスク ( `*` ) ワイルドカードを使用してすべてのプロパティを選択します。

### 例 12: パフォーマンスカウンターの配列値

この例では、変数に各パフォーマンスカウンターが格納されています。 **Countersamples** プロパティは、特定のカウンター値を表示できる配列です。

各カウンターサンプルを表示するには、を使用し `$Counter.CounterSamples` ます。

```powershell
$Counter = Get-Counter -Counter "\Processor(*)\% Processor Time"
$Counter.CounterSamples[0]
```

```Output
Path                                         InstanceName        CookedValue
----                                         ------------        -----------
\\Computer01\processor(0)\% processor time   0              1.33997091699662
```

`Get-Counter`**counter パラメーターを** 使用してカウンタを指定し `\Processor(*)\% Processor Time` ます。 値は変数に格納され `$Counter` ます。
`$Counter.CounterSamples[0]` 最初のカウンター値の配列値を表示します。

### 例 13: パフォーマンスカウンターの値を比較する

この例では、ローカルコンピューター上の各プロセッサによって使用されるプロセッサ時間を検索します。 **Countersamples** プロパティは、カウンターデータを指定した値と比較するために使用されます。

各カウンターサンプルを表示するには、を使用し `$Counter.CounterSamples` ます。

```powershell
$Counter = Get-Counter -Counter "\Processor(*)\% Processor Time"
$Counter.CounterSamples | Where-Object { $_.CookedValue -lt "20" }
```

```Output
Path                                         InstanceName        CookedValue
----                                         ------------        -----------
\\Computer01\processor(0)\% processor time   0              12.6398025240208
\\Computer01\processor(1)\% processor time   1              15.7598095767344
```

`Get-Counter`**counter パラメーターを** 使用してカウンタを指定し `\Processor(*)\% Processor Time` ます。 値は変数に格納され `$Counter` ます。 に格納されて `$Counter.CounterSamples` いるオブジェクトは、パイプラインを介して送信されます。 `Where-Object` スクリプトブロックを使用して、各オブジェクトの値を指定した20の値と比較します。 `$_.CookedValue`は、パイプライン内の現在のオブジェクトの変数です。 **CookedValue** が20未満のカウンターが表示されます。

### 例 14: パフォーマンスカウンターデータを並べ替える

この例では、パフォーマンスカウンターデータを並べ替える方法を示します。 この例では、サンプル中に最もプロセッサ時間を使用しているコンピューター上のプロセスを検索します。

```powershell
$Procs = Get-Counter -Counter "\Process(*)\% Processor Time"
$Procs.CounterSamples | Sort-Object -Property CookedValue -Descending |
   Format-Table -Property Path, InstanceName, CookedValue -AutoSize
```

```Output
Path                                                         InstanceName             CookedValue
----                                                         ------------             -----------
\\Computer01\process(_total)\% processor time                _total              395.464129650573
\\Computer01\process(idle)\% processor time                  idle                389.356575524695
\\Computer01\process(mssense)\% processor time               mssense             3.05377706293879
\\Computer01\process(csrss#1)\% processor time               csrss               1.52688853146939
\\Computer01\process(microsoftedgecp#10)\% processor time    microsoftedgecp     1.52688853146939
\\Computer01\process(runtimebroker#5)\% processor time       runtimebroker                      0
\\Computer01\process(settingsynchost)\% processor time       settingsynchost                    0
\\Computer01\process(microsoftedgecp#16)\% processor time    microsoftedgecp                    0
```

`Get-Counter`**counter** パラメーターを使用して、 `\Process(*)\% Processor Time` ローカルコンピューター上のすべてのプロセスのカウンターを指定します。 結果は変数に格納され `$Procs` ます。 `$Procs` **Countersamples** プロパティを持つ変数は、 **PerformanceCounterSample** オブジェクトをパイプラインの下位に送信します。 `Sort-Object`**Property** パラメーターを使用して、オブジェクトを **CookedValue** で **降順** に並べ替えます。 `Format-Table`**Property** パラメーターを使用して、出力の列を選択します。 **AutoSize** パラメーターを指定すると、列の幅が調整され、切り捨てが最小限に抑えられます。

## PARAMETERS

### -ComputerName

1つのコンピューター名、または **リモート** コンピューター名のコンマ区切りの配列を指定します。 NetBIOS 名、IP アドレス、またはコンピューターの完全修飾ドメイン名を使用します。

**ローカル** コンピューターからパフォーマンスカウンターデータを取得するには、 **ComputerName** パラメーターを除外します。
**MachineName** 列を含む **listset** などの出力の場合、ドット () は `.` ローカルコンピューターを示します。

`Get-Counter` は、PowerShell リモート処理に依存しません。 コンピューターがリモートコマンドを実行するように構成されていない場合でも、 **ComputerName** パラメーターを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Cn

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Continuous

**Continuous** が指定されている場合、は、 `Get-Counter` <kbd>CTRL</kbd>C キーを押すまでサンプルを取得し + <kbd></kbd>ます。 サンプルは、指定されたパフォーマンスカウンターごとに1秒ごとに取得されます。 **Sampleinterval** パラメーターを使用して、連続サンプルの間隔を長くします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: GetCounterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -カウンター

1つ以上のカウンターパスへのパスを指定します。 パスは、コンマで区切られた配列、変数、またはテキストファイルの値として入力します。 パイプラインの下にあるカウンターパス文字列をに送信でき `Get-Counter` ます。

カウンターパスは、次の構文を使用します。

`\\ComputerName\CounterSet(Instance)\CounterName`

`\CounterSet(Instance)\CounterName`

次に例を示します。

`\\Server01\Processor(*)\% User Time`

`\Processor(*)\% User Time`

`\\ComputerName`パフォーマンスカウンターパスでは、は省略可能です。 カウンターパスにコンピューター名が含まれていない場合、は `Get-Counter` ローカルコンピューターを使用します。

インスタンス内のアスタリスク ( `*` ) は、カウンターのすべてのインスタンスを取得するためのワイルドカード文字です。

```yaml
Type: System.String[]
Parameter Sets: GetCounterSet
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -ListSet

コンピューター上のパフォーマンスカウンターセットの一覧を表示します。 `*`すべてのカウンターセットを指定するには、アスタリスク () を使用します。 カウンターセット名を1つまたはコンマで区切った文字列を入力します。 カウンターセット名は、パイプラインの下位に送信できます。

カウンターセットの書式設定されたカウンターパスを取得するには、 **Listset** パラメーターを使用します。 各カウンターセットの **paths** プロパティと **PathsWithInstances** プロパティには、文字列として書式設定された個々のカウンターパスが含まれます。

カウンターパス文字列を変数に保存することも、パイプラインを使用して別のコマンドに文字列を送信することもでき `Get-Counter` ます。

たとえば、各 **プロセッサ** カウンターパスをに送信するには、次のようにし `Get-Counter` ます。

`Get-Counter -ListSet Processor | Get-Counter`

> [!NOTE]
> PowerShell 7 では、 `Get-Counter` カウンターセットの **Description** プロパティを取得できません。 **説明** はに設定されて `$null` います。

```yaml
Type: System.String[]
Parameter Sets: ListSetSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: True
```

### -MaxSamples

指定された各パフォーマンスカウンターから取得するサンプルの数を指定します。 サンプルの定数ストリームを取得するには、 **Continuous** パラメーターを使用します。

**Maxsamples** パラメーターが指定されていない場合は、 `Get-Counter` 指定されたカウンターごとに1つのサンプルのみを取得します。

大きなデータセットを収集するには、 `Get-Counter` PowerShell バックグラウンドジョブとしてを実行します。 詳細については、「[about_Jobs](../Microsoft.PowerShell.Core/About/about_Jobs.md)」を参照してください。

```yaml
Type: System.Int64
Parameter Sets: GetCounterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SampleInterval

指定された各パフォーマンスカウンターのサンプル間の秒数を指定します。 **Sampleinterval** パラメーターが指定されていない場合、で `Get-Counter` は1秒間隔が使用されます。

```yaml
Type: System.Int32
Parameter Sets: GetCounterSet
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

### System.String[]

`Get-Counter` カウンターパスとカウンターセット名のパイプライン入力を受け入れます。

## 出力

### PerformanceCounterSampleSet では、PerformanceCounterSample が、その後には、そのようになります。この場合は、このコマンドを呼び出します。

オブジェクトのプロパティを表示するには、パイプラインの出力をに送信し `Get-Member` ます。 出力されるオブジェクトの種類は次のとおりです。

**Listset** パラメーター: **Microsoft. PowerShell. Getcounter。 CounterSet**

**カウンター** のパラメーター: **PerformanceCounterSampleSet** を実行します。

**Countersamples** プロパティには、 **PerformanceCounterSample** があります。

## 注

パラメーターが指定されていない場合は、 `Get-Counter` 指定されたパフォーマンスカウンターごとに1つのサンプルを取得します。 詳細なサンプルを取得するには、 **Maxsamples** パラメーターと **Continuous** パラメーターを使用します。

`Get-Counter` サンプル間で1秒間隔を使用します。 間隔を長くするには、 **sampleinterval** パラメーターを使用します。

**Maxsamples** と **sampleinterval** の値は、コマンド内の各コンピューター上のすべてのカウンターに適用されます。 カウンターごとに異なる値を設定するには、個別のコマンドを入力し `Get-Counter` ます。

PowerShell 7 では、 **listset** パラメーターを使用する場合、 `Get-Counter` カウンターセットの **Description** プロパティを取得できません。 **説明** はに設定されて `$null` います。

## 関連リンク

[about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)

[about_Jobs](../Microsoft.PowerShell.Core/About/about_Jobs.md)

[Format-List](../Microsoft.PowerShell.Utility/Format-List.md)

[Format-Table](../Microsoft.PowerShell.Utility/Format-Table.md)

[Get-Member](../Microsoft.PowerShell.Utility/Get-Member.md)

[Receive-Job](../Microsoft.PowerShell.Core/Receive-Job.md)

[Start-Job](../Microsoft.PowerShell.Core/Start-Job.md)

[Where-Object](..//Microsoft.PowerShell.Core/Where-Object.md)

