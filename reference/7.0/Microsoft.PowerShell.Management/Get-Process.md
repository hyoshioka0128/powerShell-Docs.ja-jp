---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-process?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Process
ms.openlocfilehash: 1880651719d030bd015a6f431fce60527b56aae5
ms.sourcegitcommit: de63e9481cf8024883060aae61fb02c59c2de662
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2020
ms.locfileid: "93210224"
---
# Get-Process

## 概要
ローカルコンピューター上で実行されているプロセスを取得します。

## SYNTAX

### 名前 (既定値)

```
Get-Process [[-Name] <String[]>] [-Module] [-FileVersionInfo] [<CommonParameters>]
```

### NameWithUserName

```
Get-Process [[-Name] <String[]>] -IncludeUserName [<CommonParameters>]
```

### Id

```
Get-Process -Id <Int32[]> [-Module] [-FileVersionInfo] [<CommonParameters>]
```

### IdWithUserName

```
Get-Process -Id <Int32[]> -IncludeUserName [<CommonParameters>]
```

### InputObject

```
Get-Process -InputObject <Process[]> [-Module] [-FileVersionInfo] [<CommonParameters>]
```

### InputObjectWithUserName

```
Get-Process -InputObject <Process[]> -IncludeUserName [<CommonParameters>]
```

## Description

`Get-Process`コマンドレットは、ローカルコンピューター上のプロセスを取得します。

パラメーターを指定しない場合、このコマンドレットはローカルコンピューター上のすべてのプロセスを取得します。
また、プロセス名またはプロセス ID (PID) を使用して特定のプロセスを指定したり、パイプラインを介してこのコマンドレットにプロセスオブジェクトを渡したりすることもできます。

既定では、このコマンドレットは、プロセスに関する詳細情報を持つプロセスオブジェクトを返し、プロセスを開始および停止できるようにするメソッドをサポートします。
また、コマンドレットのパラメーターを使用して、 `Get-Process` プロセスで実行されるプログラムのファイルバージョン情報を取得したり、プロセスによって読み込まれたモジュールを取得したりすることもできます。

## 例

### 例 1: ローカルコンピューター上のすべてのアクティブなプロセスの一覧を取得する

```powershell
Get-Process
```

このコマンドは、ローカルコンピューター上で実行されているすべてのアクティブなプロセスの一覧を取得します。
各列の定義については、「 [メモ](#notes) 」を参照してください。

### 例 2: 1 つ以上のプロセスについて使用可能なすべてのデータを取得する

```powershell
Get-Process winword, explorer | Format-List *
```

このコマンドは、コンピューター上の Winword プロセスと Explorer プロセスに関して利用可能なすべてのデータを取得します。
**Name** パラメーターを使用してプロセスを指定しますが、省略可能なパラメーター名は省略しています。
パイプライン演算子は `|` データを `Format-List` コマンドレットに渡します。これにより、 `*` winword.exe および Explorer プロセスオブジェクトの使用可能なすべてのプロパティが表示されます。

プロセスをプロセス ID で指定することもできます。
たとえば、`Get-Process -Id 664, 2060` です。

### 例 3: ワーキングセットが指定したサイズを超えるすべてのプロセスを取得する

```powershell
Get-Process | Where-Object {$_.WorkingSet -gt 20000000}
```

このコマンドは、ワーキング セットが 20 MB を超えているプロセスをすべて取得します。
コマンドレットを使用して、 `Get-Process`  実行中のすべてのプロセスを取得します。
パイプライン演算子は、 `|` プロセスオブジェクトを `Where-Object` コマンドレットに渡します。このコマンドレットは、 **ワーキング** セットプロパティの値が2000万バイトを超えるオブジェクトのみを選択します。

**ワーキング** セットは、プロセスオブジェクトの多くのプロパティの1つです。
すべてのプロパティを表示するには、「」と入力 `Get-Process | Get-Member` します。
既定では、既定の表示がキロバイト単位やメガバイト単位であっても、サイズに関するプロパティの値はすべてバイト単位で表されます。

### 例 4: 優先度に基づいてグループ内のコンピューターのプロセスを一覧表示する

```powershell
$A = Get-Process
$A | Get-Process | Format-Table -View priority
```

これらのコマンドは、コンピューター上のプロセスを優先度クラスに基づいてグループ化します。
最初のコマンドは、コンピューター上のすべてのプロセスを取得し、変数に格納し `$A` ます。

2番目のコマンドは、変数に格納されている **プロセス** オブジェクトをコマンド `$A` レットに渡し、 `Get-Process` `Format-Table` コマンドレットに渡します。このコマンドレットは、 **優先度** ビューを使用してプロセスをフォーマットします。

**優先度** ビュー、およびその他のビューは、PowerShell ホームディレクトリ () の types.ps1xml フォーマットファイルで定義されてい `$pshome` ます。

### 例 5: 標準 Get-Process 出力表示にプロパティを追加する

```powershell
Get-Process pwsh | Format-Table `
    @{Label = "NPM(K)"; Expression = {[int]($_.NPM / 1024)}},
    @{Label = "PM(K)"; Expression = {[int]($_.PM / 1024)}},
    @{Label = "WS(K)"; Expression = {[int]($_.WS / 1024)}},
    @{Label = "VM(M)"; Expression = {[int]($_.VM / 1MB)}},
    @{Label = "CPU(s)"; Expression = {if ($_.CPU) {$_.CPU.ToString("N")}}},
    Id, MachineName, ProcessName -AutoSize
```

```Output
NPM(K) PM(K) WS(K) VM(M) CPU(s)   Id MachineName ProcessName
------ ----- ----- ----- ------   -- ----------- -----------
     6 23500 31340   142 1.70   1980 .           pwsh
     6 23500 31348   142 2.75   4016 .           pwsh
    27 54572 54520   576 5.52   4428 .           pwsh
```

この例では、 `Format-Table` **MachineName** プロパティを標準出力表示に追加するコマンドを示し `Get-Process` ます。

### 例 6: プロセスのバージョン情報を取得する

```powershell
Get-Process pwsh -FileVersionInfo
```

```Output
ProductVersion   FileVersion      FileName
--------------   -----------      --------
6.1.2            6.1.2            C:\Program Files\PowerShell\6\pwsh.exe
```

このコマンドは、 **FileVersionInfo** パラメーターを使用して、PowerShell プロセスのメインモジュールである pwsh.exe ファイルのバージョン情報を取得します。

Windows Vista 以降のバージョンの Windows で所有していないプロセスでこのコマンドを実行するには、[管理者として実行] オプションを使用して PowerShell を開く必要があります。

### 例 7: 指定したプロセスで読み込まれたモジュールを取得する

```powershell
Get-Process SQL* -Module
```

このコマンドは、 **Module** パラメーターを使用して、プロセスによって読み込まれたモジュールを取得します。
このコマンドは、名前が SQL で始まるプロセスのモジュールを取得します。

Windows Vista 以降のバージョンの Windows で、自分が所有していないプロセスを使用してこのコマンドを実行するには、[管理者として実行] オプションを使用して PowerShell を起動する必要があります。

### 例 8: プロセスの所有者を検索する

```powershell
Get-Process pwsh -IncludeUserName
```

```Output
Handles      WS(K)   CPU(s)     Id UserName            ProcessName
-------      -----   ------     -- --------            -----------
    782     132080     2.08   2188 DOMAIN01\user01     pwsh
```

このコマンドは、プロセスの所有者を見つける方法を示します。
Windows では、 **Includeusername** パラメーターには管理者特権 (管理者として実行) が必要です。
出力には、所有者が Domain01\user01 であることが示されます。

### 例 9: 自動変数を使用して、現在のセッションをホストしているプロセスを識別する

```powershell
Get-Process pwsh
```

```Output
NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
------    -----      -----     ------      --  -- -----------
    83    96.21     105.95       4.33    1192  10 pwsh
    79    83.81     117.61       2.16   10580  10 pwsh
```

```powershell
Get-Process -Id $PID
```

```Output
NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
------    -----      -----     ------      --  -- -----------
    83    96.21      77.53       4.39    1192  10 pwsh
```

これらのコマンドは、自動変数を使用して、現在の PowerShell セッションをホストしているプロセスを識別する方法を示して `$PID` います。
このメソッドを使用すると、停止または終了する PowerShell プロセスとホスト プロセスを区別できます。

最初のコマンドは、現在のセッションのすべての PowerShell プロセスを取得します。

2番目のコマンドは、現在のセッションをホストしている PowerShell プロセスを取得します。

### 例 10: メインウィンドウタイトルを持つすべてのプロセスを取得し、テーブルに表示する

```powershell
Get-Process | Where-Object {$_.mainWindowTitle} | Format-Table Id, Name, mainWindowtitle -AutoSize
```

このコマンドは、メイン ウィンドウ タイトルを持つすべてのプロセスを取得し、プロセス ID とプロセス名の表形式で表示します。

**MainWindowTitle** プロパティは、を返す **Process** オブジェクトの多くの便利なプロパティのうちの1つにすぎ `Get-Process` ません。
すべてのプロパティを表示するには、コマンドの結果を `Get-Process` コマンドレットに渡し `Get-Member` `Get-Process | Get-Member` ます。

## PARAMETERS

### -FileVersionInfo

このコマンドレットが、プロセスで実行されるプログラムのファイルバージョン情報を取得することを示します。

Windows Vista 以降のバージョンの Windows では、[管理者として実行] オプションを使用して PowerShell を開いて、所有していないプロセスでこのパラメーターを使用する必要があります。

リモートコンピューター上のプロセスのファイルバージョン情報を取得するには、 `Invoke-Command` コマンドレットを使用します。

このパラメーターを使用することは、各プロセスオブジェクトの **FileVersionInfo** プロパティを取得することと同じです。
このパラメーターを使用すると、は `Get-Process` プロセスオブジェクトではなく、 **FileVersionInfo** オブジェクト **FileVersionInfo** を返します。
そのため、コマンドの出力を、などのプロセスオブジェクトを必要とするコマンドレットにパイプすることはできません `Stop-Process` 。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Name, Id, InputObject
Aliases: FV, FVI

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

プロセス ID (PID) を使用して、プロセスを 1 つ以上指定します。
複数の ID を指定するには、ID をコンマで区切ります。
プロセスの PID を検索するには、「」と入力 `Get-Process` します。

```yaml
Type: System.Int32[]
Parameter Sets: Id, IdWithUserName
Aliases: PID

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -IncludeUserName

**プロセス** オブジェクトのユーザー名の値がコマンドの結果と共に返されることを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameWithUserName, IdWithUserName, InputObjectWithUserName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

1 つまたは複数のプロセス オブジェクトを指定します。
オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Diagnostics.Process[]
Parameter Sets: InputObject, InputObjectWithUserName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -モジュール

このコマンドレットが、プロセスによって読み込まれたモジュールを取得することを示します。

Windows Vista 以降のバージョンの Windows では、[管理者として実行] オプションを使用して PowerShell を開いて、所有していないプロセスでこのパラメーターを使用する必要があります。

リモートコンピューター上のプロセスによって読み込まれたモジュールを取得するには、コマンドレットを使用し `Invoke-Command` ます。

このパラメーターは、各プロセスオブジェクトの **Modules** プロパティを取得することと同じです。
このパラメーターを使用すると、このコマンドレットはプロセスオブジェクトではなく **processmodule** **オブジェクトを返します。**
そのため、コマンドの出力を、などのプロセスオブジェクトを必要とするコマンドレットにパイプすることはできません `Stop-Process` 。

同じコマンドで *Module* と **FileVersionInfo** の両方のパラメーターを使用すると、このコマンドレットは、すべてのモジュールのファイルバージョンに関する情報を含む **FileVersionInfo** オブジェクトを返します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Name, Id, InputObject
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

プロセス名を使用してプロセスを 1 つ以上指定します。
複数のプロセス名をコンマで区切って指定することも、ワイルドカード文字を使用することもできます。
パラメーター名 ("Name") は省略可能です。

```yaml
Type: System.String[]
Parameter Sets: Name, NameWithUserName
Aliases: ProcessName

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.Diagnostics.Process

パイプを使用してプロセスオブジェクトをこのコマンドレットに渡します。

## 出力

### FileVersionInfo、system.string、および System. Diagnostics... ProcessModule

既定では、このコマンドレット **は、system.string オブジェクトを** 返します。
**FileVersionInfo** パラメーターを使用すると、 **FileVersionInfo** オブジェクトが返されます。
**Module** パラメーターを使用する場合、 **FileVersionInfo** パラメーターを指定しないと、 **system.string オブジェクトが** 返されます。

## 注

- このコマンドレットは、組み込みのエイリアスである ps と gps を使用して参照することもできます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。
- 64ビットバージョンの Windows を実行しているコンピューターでは、64ビットバージョンの PowerShell は64ビットプロセスモジュールだけを取得し、PowerShell の32ビットバージョンは、32ビットプロセスモジュールのみを取得します。
- PowerShell では、Windows Management Instrumentation (WMI) Win32_Process オブジェクトのプロパティとメソッドを使用できます。 詳細については、「」および「WMI SDK」を参照してください `Get-WmiObject` 。
- 既定では、次の欄を含む表としてプロセスが表示されます。 プロセスオブジェクトのすべてのプロパティの詳細については、MSDN ライブラリの「 [Process properties (プロセスプロパティ](/dotnet/api/system.diagnostics.process) )」を参照してください。
  - ハンドル: プロセスが開いたハンドルの数。
  - NPM (K): プロセスが使用している非ページメモリの量 (kb 単位)。
  - PM (K): プロセスが使用しているページング可能なメモリの量 (kb 単位)。
  - WS (K): プロセスのワーキングセットのサイズ (kb 単位)。
    ワーキング セットは、プロセスが最近参照したメモリのページで構成されます。
  - VM (M): プロセスが使用している仮想メモリの量 (メガバイト単位)。
    仮想メモリには、ディスク上のページング ファイルの記憶域が含まれます。
  - CPU (s): プロセスがすべてのプロセッサで使用したプロセッサ時間の合計 (秒単位)。
  - ID: プロセスのプロセス ID (PID)。
  - ProcessName: プロセスの名前。
    プロセスに関連する概念の説明については、ヘルプとサポート センターにある用語集と、タスク マネージャーのヘルプを参照してください。
- また `Format-Table` 、 **StartTime** や **Priority** など、で使用できるプロセスの組み込みの代替ビューを使用したり、独自のビューを設計したりすることもできます。

## 関連リンク

[Debug-Process](Debug-Process.md)

[Get-Process](Get-Process.md)

[Start-Process](Start-Process.md)

[Stop-Process](Stop-Process.md)

[Wait-Process](Wait-Process.md)
