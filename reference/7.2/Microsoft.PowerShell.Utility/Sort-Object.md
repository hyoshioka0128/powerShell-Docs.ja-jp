---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/sort-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Sort-Object
ms.openlocfilehash: fead3b3d109a79186bc82f3c9f212f11f7b0bc57
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605182"
---
# Sort-Object

## 概要
プロパティ値別にオブジェクトを並べ替えます。

## SYNTAX

### 既定値 (既定値)

```
Sort-Object [-Stable] [-Descending] [-Unique] [-InputObject <PSObject>] [[-Property] <Object[]>]
 [-Culture <String>] [-CaseSensitive] [<CommonParameters>]
```

### 上

```
Sort-Object [-Descending] [-Unique] -Top <Int32> [-InputObject <PSObject>] [[-Property] <Object[]>]
 [-Culture <String>] [-CaseSensitive] [<CommonParameters>]
```

### 下

```
Sort-Object [-Descending] [-Unique] -Bottom <Int32> [-InputObject <PSObject>] [[-Property] <Object[]>]
 [-Culture <String>] [-CaseSensitive] [<CommonParameters>]
```

## Description

`Sort-Object`コマンドレットは、オブジェクトのプロパティ値に基づいて、オブジェクトを昇順または降順に並べ替えます。 並べ替えプロパティがコマンドに含まれていない場合、PowerShell では、最初の入力オブジェクトの既定の並べ替えプロパティを使用します。 入力オブジェクトの型に既定の並べ替えプロパティがない場合、PowerShell はオブジェクト自体を比較しようとします。 詳細については、「 [メモ](#notes) 」を参照してください。

オブジェクトは、1つのプロパティまたは複数のプロパティで並べ替えることができます。 複数のプロパティでは、ハッシュテーブルを使用して、昇順、降順、または並べ替え順序の組み合わせを並べ替えることができます。 プロパティは、大文字と小文字を区別するか、大文字と小文字を区別して並べ替えられます。 Output からの重複を排除するには、 **Unique** パラメーターを使用します。

## 例

### 例 1: 現在のディレクトリを名前で並べ替える

この例では、ディレクトリ内のファイルとサブディレクトリを並べ替えます。

```
PS> Get-ChildItem -Path C:\Test | Sort-Object

    Directory: C:\Test

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/13/2019     08:55             26 anotherfile.txt
-a----        2/13/2019     13:26             20 Bfile.txt
-a----        2/12/2019     15:40         118014 Command.txt
-a----         2/1/2019     08:43            183 CreateTestFile.ps1
d-----        2/25/2019     18:25                Files
d-----        2/25/2019     18:24                Logs
-ar---        2/12/2019     14:31             27 ReadOnlyFile.txt
-a----        2/12/2019     16:24             23 Zsystemlog.log
```

この `Get-ChildItem` コマンドレットは、 **Path** パラメーター **C:\Test** によって指定されたディレクトリからファイルとサブディレクトリを取得します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。
`Sort-Object` はプロパティを指定しないので、既定の並べ替えプロパティの **Name** によって出力が並べ替えられます。

### 例 2: ファイルの長さで現在のディレクトリを並べ替える

このコマンドは、現在のディレクトリにあるファイルを長さの昇順で表示します。

```
PS> Get-ChildItem -Path C:\Test -File | Sort-Object -Property Length

    Directory: C:\Test

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/13/2019     13:26             20 Bfile.txt
-a----        2/12/2019     16:24             23 Zsystemlog.log
-a----        2/13/2019     08:55             26 anotherfile.txt
-ar---        2/12/2019     14:31             27 ReadOnlyFile.txt
-a----         2/1/2019     08:43            183 CreateTestFile.ps1
-a----        2/12/2019     15:40         118014 Command.txt
```

`Get-ChildItem`コマンドレットは、 **Path** パラメーターで指定されたディレクトリからファイルを取得します。
**File** パラメーターは、が `Get-ChildItem` ファイルオブジェクトのみを取得することを指定します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object`**length** パラメーターを使用して、ファイルを長さで昇順に並べ替えます。

### 例 3: メモリ使用量によってプロセスを並べ替える

この例では、ワーキングセット (WS) のサイズに基づいてメモリ使用量が最も高いプロセスを表示します。

```
PS> Get-Process | Sort-Object -Property WS | Select-Object -Last 5

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
    136   193.92     217.11     889.16   87492   8 OUTLOOK
    112   347.73     297.02      95.19  106908   8 Teams
    206   266.54     323.71      37.17   60620   8 MicrosoftEdgeCP
     35   552.19     549.94     131.66    6552   8 Code
      0     1.43     595.12       0.00    2780   0 Memory Compression
```

この `Get-Process` コマンドレットは、コンピューター上で実行されているプロセスの一覧を取得します。 プロセスオブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object`**Property** パラメーターを使用して、オブジェクトを **WS** で並べ替えます。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Select-Object` ます。
`Select-Object`**最後** のパラメーターを使用して最後の5つのオブジェクトを指定します。これは、 **WS** の最大使用量を持つオブジェクトです。

PowerShell 6 では、の `Sort-Object` 代わりにパラメーター **Bottom** を指定し `Select-Object` ます。 たとえば、「 `Get-Process | Sort-Object -Property WS -Bottom 5` 」のように入力します。

### 例 4: Id で履歴情報オブジェクトを並べ替える

このコマンドは、 **Id** プロパティを使用して、PowerShell セッションの **履歴情報** オブジェクトを並べ替えます。 各 PowerShell セッションには、独自のコマンド履歴があります。

```
PS> Get-History | Sort-Object -Property Id -Descending

  Id CommandLine
  -- -----------
  10 Get-Command Sort-Object -Syntax
   9 $PSVersionTable
   8 Get-Command Sort-Object -Syntax
   7 Get-Command Sort-Object -ShowCommandInfo
   6 Get-ChildItem -Path C:\Test | Sort-Object -Property Length
   5 Get-Help Clear-History -online
   4 Get-Help Clear-History -full
   3 Get-ChildItem | Get-Member
   2 Get-Command Sort-Object -Syntax
   1 Set-Location C:\Test\
```

`Get-History`コマンドレットは、現在の PowerShell セッションから履歴オブジェクトを取得します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object`**Property** パラメーターを使用して、オブジェクトを **Id** で並べ替えます。**降順** のパラメーターは、コマンド履歴を最新から古いものに並べ替えます。

### 例 5: ハッシュテーブルを使用してプロパティを昇順および降順で並べ替える

この例では、2つのプロパティを使用して、オブジェクト、 **状態** 、および **DisplayName** を並べ替えます。 **ステータス** は降順に並べ替えられ、 **DisplayName** は昇順に並べ替えられます。

ハッシュテーブルは、 **プロパティ** パラメーターの値を指定するために使用されます。 ハッシュテーブルでは、式を使用してプロパティ名と並べ替え順序を指定します。 ハッシュ テーブルの詳細については、「[about_Hash_Tables (ハッシュ テーブルについて)](../Microsoft.PowerShell.Core/About/about_Hash_Tables.md)」をご覧ください。

ハッシュテーブルで使用される **Status** プロパティは列挙型プロパティです。
詳細については、「 [ServiceControllerStatus](/dotnet/api/system.serviceprocess.servicecontrollerstatus)」を参照してください。

```
PS C:\> Get-Service | Sort-Object -Property @{Expression = "Status"; Descending = $True}, @{Expression = "DisplayName"; Descending = $False}

Status   Name               DisplayName
------   ----               -----------
Running  Appinfo            Application Information
Running  BthAvctpSvc        AVCTP service
Running  BrokerInfrastru... Background Tasks Infrastructure Ser...
Running  BDESVC             BitLocker Drive Encryption Service
Running  CoreMessagingRe... CoreMessaging
Running  VaultSvc           Credential Manager
Running  DsSvc              Data Sharing Service
Running  Dhcp               DHCP Client
...
Stopped  ALG                Application Layer Gateway Service
Stopped  AppMgmt            Application Management
Stopped  BITS               Background Intelligent Transfer Ser...
Stopped  wbengine           Block Level Backup Engine Service
Stopped  BluetoothUserSe... Bluetooth User Support Service_14fb...
Stopped  COMSysApp          COM+ System Application
Stopped  smstsmgr           ConfigMgr Task Sequence Agent
Stopped  DeviceInstall      Device Install Service
Stopped  MSDTC              Distributed Transaction Coordinator
```

`Get-Service`コマンドレットは、コンピューター上のサービスの一覧を取得します。 サービスオブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object`**プロパティパラメーターと** ハッシュテーブルを使用して、プロパティ名と並べ替え順序を指定します。 **Property** パラメーターは2つのプロパティで並べ替えられます。**状態** は降順で表示され、 **DisplayName** は昇順です。

**Status** は列挙されたプロパティです。 **Stopped** の値は **1** で、 **Running** の値は **4** です。 **降順** のパラメーターは、プロセスを `$True` **停止** する前に **実行中** のプロセスが表示されるようにに設定されています。 **DisplayName** は、 **降順** のパラメーターをに設定して `$False` 、表示名をアルファベット順に並べ替えます。

### 例 6: 期間によってテキストファイルを並べ替える

このコマンドは、テキストファイルを、書き込み **時間** と **lastwritetime** の間の時間の降順で並べ替えます。

```
PS> Get-ChildItem -Path C:\Test\*.txt | Sort-Object -Property @{Expression = {$_.CreationTime - $_.LastWriteTime}; Descending = $False} | Format-Table CreationTime, LastWriteTime, FullName

CreationTime          LastWriteTime        FullName
------------          -------------        --------
11/21/2018 12:39:01   2/26/2019 08:59:36   C:\Test\test2.txt
12/4/2018 08:29:41    2/26/2019 08:57:05   C:\Test\powershell_list.txt
2/20/2019 08:15:59    2/26/2019 12:09:43   C:\Test\CreateTestFile.txt
2/20/2019 08:15:59    2/26/2019 12:07:41   C:\Test\Command.txt
2/20/2019 08:15:59    2/26/2019 08:57:52   C:\Test\ReadOnlyFile.txt
11/29/2018 15:16:50   12/4/2018 16:16:24   C:\Test\LogData.txt
2/25/2019 18:25:11    2/26/2019 12:08:47   C:\Test\Zsystemlog.txt
2/25/2019 18:25:11    2/26/2019 08:55:33   C:\Test\Bfile.txt
2/26/2019 08:46:59    2/26/2019 12:12:19   C:\Test\LogFile3.txt
```

この `Get-ChildItem` コマンドレットでは、 **Path** パラメーターを使用して、ディレクトリ **C:\Test** とすべてのファイルを指定し `*.txt` ます。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。
`Sort-Object`**プロパティ** パラメーターをハッシュテーブルと共に使用 **して、** 指定された時間と **lastwritetime** の間の各ファイルの時間間隔を決定します。 **降順** のパラメーターをに設定すると、 `$False` 最長の時間間隔の順序で並べ替えられます。

### 例 7: テキストファイル内の名前を並べ替える

この例では、テキストファイルからリストを並べ替える方法を示します。 元のファイルは、並べ替えられていないリストとして表示されます。 `Sort-Object` 内容を並べ替えてから、重複を削除する **一意** のパラメーターを使用して内容を並べ替えます。

```
PS> Get-Content -Path C:\Test\ServerNames.txt
localhost
server01
server25
LOCALHOST
Server19
server3
localhost

PS> Get-Content -Path C:\Test\ServerNames.txt | Sort-Object
localhost
LOCALHOST
localhost
server01
Server19
server25
server3

PS> Get-Content -Path C:\Test\ServerNames.txt | Sort-Object -Unique
localhost
server01
Server19
server25
server3
```

コマンドレットでは、 `Get-Content` **Path** パラメーターを使用して、ディレクトリとファイル名を指定します。 ファイル **ServerNames.txt** に、並べ替えられていないコンピューター名の一覧が含まれています。

コマンドレットでは、 `Get-Content` **Path** パラメーターを使用して、ディレクトリとファイル名を指定します。 ファイル **ServerNames.txt** に、並べ替えられていないコンピューター名の一覧が含まれています。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object` リストを既定の順序 (昇順) で並べ替えます。

コマンドレットでは、 `Get-Content` **Path** パラメーターを使用して、ディレクトリとファイル名を指定します。 ファイル **ServerNames.txt** に、並べ替えられていないコンピューター名の一覧が含まれています。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object`**一意** のパラメーターを使用して、重複するコンピューター名を削除します。 リストは、既定の順序 (昇順) で並べ替えられます。

### 例 8: 文字列を整数として並べ替える

この例では、文字列オブジェクトを含むテキストファイルを整数として並べ替える方法を示します。 各コマンドをパイプラインでに送信し、 `Get-Member` オブジェクトが整数ではなく文字列であることを確認できます。 これらの例では、 `ProductId.txt` 並べ替えられていない製品番号の一覧がファイルに含まれています。

最初の例では、は `Get-Content` ファイルの内容を取得し、パイプを使用して `Sort-Object` コマンドレットに渡します。 `Sort-Object` 文字列オブジェクトを昇順に並べ替えます。

```
PS> Get-Content -Path C:\Test\ProductId.txt | Sort-Object
0
1
12345
1500
2
2800
3500
4100
500
6200
77
88
99999

PS> Get-Content -Path C:\Test\ProductId.txt | Sort-Object {[int]$_}
0
1
2
77
88
500
1500
2800
3500
4100
6200
12345
99999
```

2番目の例では、はファイルの内容を取得し、パイプを使用して `Get-Content` `Sort-Object` コマンドレットに渡します。 `Sort-Object` スクリプトブロックを使用して、文字列を整数に変換します。 このサンプルコードでは、は、 `[int]` 文字列を整数に変換し、 `$_` パイプラインによって取得される各文字列を表します。 整数オブジェクトは、パイプラインからコマンドレットに送信され `Sort-Object` ます。
`Sort-Object` 整数オブジェクトを数値順に並べ替えます。

### 例 9: 安定した並べ替えを使用する

**Top**、 **Bottom**、または **Stable** パラメーターを使用すると、並べ替えられたオブジェクトは、 `Sort-Object` 並べ替え条件が等しい場合に、によって受信された順に配信されます。 この例では、1 ~ 20 の数値を値 "モジュロ 3" で並べ替えています。 剰余値の範囲は 0 ~ 2 です。

```powershell
PS> 1..20 |Sort-Object {$_ % 3}
18
3
15
6
12
9
1
16
13
10
7
4
19
11
8
14
5
17
2
20

PS> 1..20 |Sort-Object {$_ % 3} -Stable
3
6
9
12
15
18
1
4
7
10
13
16
19
2
5
8
11
14
17
20
```

最初の並べ替えからの出力は、剰余値によって正しくグループ化されますが、個々の項目は剰余の範囲内で並べ替えられません。 2番目の並べ替えでは、 **stable オプションを使用して** 安定した並べ替えを返します。

## PARAMETERS

### -Bottom

並べ替えられたオブジェクト配列の末尾から取得するオブジェクトの数を指定します。 これにより、安定した並べ替えが実行されます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: Bottom
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CaseSensitive

並べ替えで大文字と小文字が区別されることを示します。 既定では、並べ替えでは大文字と小文字は区別されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Case-insensitive
Accept pipeline input: False
Accept wildcard characters: False
```

### -Culture

並べ替えに使用するカルチャ構成を指定します。 `Get-Culture`システムのカルチャ構成を表示するには、を使用します。

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

### -降順

が `Sort-Object` オブジェクトを降順に並べ替えることを示します。 既定値は昇順です。

並べ替え順序が異なる複数のプロパティを並べ替えるには、ハッシュテーブルを使用します。 たとえば、ハッシュテーブルを使用すると、1つのプロパティを昇順に並べ替え、別のプロパティを降順に並べ替えることができます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Ascending
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

オブジェクトを並べ替えるには、パイプラインをに送信 `Sort-Object` します。 **InputObject** パラメーターを使用して項目のコレクションを送信すると、は `Sort-Object` コレクションを表す1つのオブジェクトを受け取ります。 1つのオブジェクトを並べ替えることができないため、は `Sort-Object` コレクション全体を変更せずに返します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -プロパティ

がオブジェクトの並べ替えに使用するプロパティ名を指定し `Sort-Object` ます。 ワイルドカードを使用できます。
オブジェクトは、プロパティ値に基づいて並べ替えられます。 プロパティを指定しない場合、 `Sort-Object` は、オブジェクトの種類またはオブジェクト自体の既定のプロパティに基づいて並べ替えを行います。

複数のプロパティを昇順、降順、または並べ替え順序の組み合わせで並べ替えることができます。 複数のプロパティを指定すると、オブジェクトは最初のプロパティで並べ替えられます。 1つ目のプロパティに対して複数のオブジェクトの値が同じ場合、それらのオブジェクトは2番目のプロパティで並べ替えられます。 指定されたプロパティがなくなるか、またはオブジェクトのグループがなくなるまで、このプロセスが続行されます。

**プロパティ** パラメーターの値には、計算されるプロパティを指定できます。 集計プロパティを作成するには、ハッシュ テーブルを使用します。

ハッシュテーブルの有効なキーは次のとおりです。

- `expression` - `<string>` または `<script block>`
- `ascending` もしくは `descending` - `<boolean>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: Default properties
Accept pipeline input: False
Accept wildcard characters: True
```

### -Top

並べ替えられたオブジェクト配列の先頭から取得するオブジェクトの数を指定します。 これにより、安定した並べ替えが実行されます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: Top
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -一意

が `Sort-Object` 重複を排除し、コレクションの一意のメンバーのみを返すことを示します。 並べ替えられた出力には、一意の値の最初のインスタンスが含まれます。

**一意** では大文字と小文字が区別されません。 文字の大文字と小文字のみが異なる文字列は、同じものと見なされます。
たとえば、文字や文字などです。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: All
Accept pipeline input: False
Accept wildcard characters: False
```

### -Stable

並べ替えられたオブジェクトは、並べ替え条件が等しい場合に受け取った順序で配信されます。

このパラメーターは、PowerShell v 6.2.0 で追加されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Default
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

### システム管理. PSObject

パイプを使用して、並べ替えの対象となるオブジェクトをパイプすることができ `Sort-Object` ます。

## 出力

### システム管理. PSObject

`Sort-Object` 並べ替えられたオブジェクトを返します。

## 注

`Sort-Object`コマンドレットは、コマンドで指定されたプロパティまたはオブジェクトの種類の既定の並べ替えプロパティに基づいてオブジェクトを並べ替えます。 既定の並べ替えプロパティは、 `PropertySet` ファイル内のという名前のを使用して定義され `DefaultKeyPropertySet` `types.ps1xml` ます。 詳細については、「 [about_Types.ps1xml](/powershell/module/Microsoft.PowerShell.Core/About/about_Types.ps1xml)」を参照してください。

オブジェクトに指定されたプロパティのいずれかがない場合、そのオブジェクトのプロパティ値は、によって Null として解釈され、 `Sort-Object` 並べ替え順序の最後に配置されます。 

並べ替えプロパティが使用できない場合、PowerShell はオブジェクト自体を比較しようとします。
`Sort-Object` は、各プロパティに対して **Compare** メソッドを使用します。 プロパティが **IComparable** を実装していない場合、コマンドレットはプロパティの値を文字列に変換し、 **system.string に対して** **Compare** メソッドを使用します。 詳細については、「 [PSObject (Object) メソッド](/dotnet/api/system.management.automation.psobject.compareto)」を参照してください。

**Status** などの列挙型プロパティを基準に並べ替えると、は `Sort-Object` 列挙値で並べ替えられます。 Windows サービスの場合、 **Stopped** の値は **1** で、 **Running** の値は **4** です。
**Stopped** は、列挙値によっ **て実行さ** れる前に並べ替えられます。 詳細については、「 [ServiceControllerStatus](/dotnet/api/system.serviceprocess.servicecontrollerstatus)」を参照してください。

安定した並べ替えを実行すると、並べ替えアルゴリズムのパフォーマンスが低下します。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[about_Hash_Tables](../Microsoft.PowerShell.Core/About/about_Hash_Tables.md)

[about_Types.ps1xml](../Microsoft.PowerShell.Core/About/about_Types.ps1xml.md)

[Compare-Object](Compare-Object.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Group-Object](Group-Object.md)

[Measure-Object](Measure-Object.md)

[New-Object](New-Object.md)

[Select-Object](Select-Object.md)

[Tee-Object](Tee-Object.md)
