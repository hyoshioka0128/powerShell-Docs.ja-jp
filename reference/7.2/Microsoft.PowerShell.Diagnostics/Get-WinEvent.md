---
external help file: Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Diagnostics
ms.date: 11/20/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-WinEvent
ms.openlocfilehash: 742ef9b19294b93be9a2b7dce58bad913d8fd27b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602643"
---
# Get-WinEvent

## 概要
ローカルおよびリモート コンピューター上のイベント ログとイベント トレース ログ ファイルからイベントを取得します。

## SYNTAX

### GetLogSet (既定値)

```
Get-WinEvent [[-LogName] <String[]>] [-MaxEvents <Int64>] [-ComputerName <String>]
 [-Credential <PSCredential>] [-FilterXPath <String>] [-Force] [-Oldest] [<CommonParameters>]
```

### ListLogSet

```
Get-WinEvent [-ListLog] <String[]> [-ComputerName <String>] [-Credential <PSCredential>] [-Force]
 [<CommonParameters>]
```

### ListProviderSet

```
Get-WinEvent [-ListProvider] <String[]> [-ComputerName <String>] [-Credential <PSCredential>]
 [<CommonParameters>]
```

### GetProviderSet

```
Get-WinEvent [-ProviderName] <String[]> [-MaxEvents <Int64>] [-ComputerName <String>]
 [-Credential <PSCredential>] [-FilterXPath <String>] [-Force] [-Oldest] [<CommonParameters>]
```

### 個

```
Get-WinEvent [-Path] <String[]> [-MaxEvents <Int64>] [-Credential <PSCredential>]
 [-FilterXPath <String>] [-Oldest] [<CommonParameters>]
```

### HashQuerySet

```
Get-WinEvent [-MaxEvents <Int64>] [-ComputerName <String>] [-Credential <PSCredential>]
 [-FilterHashtable] <Hashtable[]> [-Force] [-Oldest] [<CommonParameters>]
```

### XmlQuerySet

```
Get-WinEvent [-MaxEvents <Int64>] [-ComputerName <String>] [-Credential <PSCredential>]
 [-FilterXml] <XmlDocument> [-Oldest] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Get-WinEvent` **システム** ログや **アプリケーション** ログなどの従来のログを含むイベントログからイベントを取得します。 コマンドレットは、Windows Vista で導入された Windows イベントログのテクノロジによって生成されるイベントログからデータを取得します。 また、 **Windows イベントトレーシング (ETW)** によって生成されるログファイル内のイベントです。 既定では、は、 `Get-WinEvent` 最新から古い順にイベント情報を返します。

`Get-WinEvent` イベントログとイベントログプロバイダーの一覧を表示します。 コマンドを中断するには、 <kbd>ctrl</kbd> + <kbd>C</kbd>キーを押します。 選択したログまたは選択したイベント プロバイダーによって生成されたログからイベントを取得できます。 さらに、1 つのコマンドで複数のソースからのイベントを組み合わせることができます。
`Get-WinEvent` では、XPath クエリ、構造化 XML クエリ、およびハッシュテーブルクエリを使用してイベントをフィルター処理できます。

PowerShell を管理者として実行していない場合は、ログに関する情報を取得できないというエラーメッセージが表示されることがあります。

## 例

### 例 1: ローカルコンピューターからすべてのログを取得する

このコマンドは、ローカルコンピューター上のすべてのイベントログを取得します。 ログは、取得した順に一覧表示され `Get-WinEvent` ます。 まず、従来のログを取得し、次に新しい Windows イベントログを取得します。
ログの **RecordCount** を null (空白またはゼロ) にすることができます。

```powershell
Get-WinEvent -ListLog *
```

```Output
LogMode   MaximumSizeInBytes RecordCount LogName
-------   ------------------ ----------- -------
Circular            15532032       14500 Application
Circular             1052672         117 Azure Information Protection
Circular             1052672        3015 CxAudioSvcLog
Circular            20971520             ForwardedEvents
Circular            20971520           0 HardwareEvents
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **Listlog** パラメーターでは、アスタリスク ( `*` ) のワイルドカードを使用して、各ログに関する情報を表示します。

### 例 2: クラシックセットアップログを取得する

このコマンドは、クラシック **セットアップ** ログを表す **EventLogConfiguration** オブジェクトを取得します。 オブジェクトには、ファイルサイズ、プロバイダー、ファイルパス、ログが有効かどうかなど、ログに関する情報が含まれています。

```powershell
Get-WinEvent -ListLog Setup | Format-List -Property *
```

```Output
FileSize                       : 69632
IsLogFull                      : False
LastAccessTime                 : 3/13/2019 09:41:46
LastWriteTime                  : 3/13/2019 09:41:46
OldestRecordNumber             : 1
RecordCount                    : 23
LogName                        : Setup
LogType                        : Operational
LogIsolation                   : Application
IsEnabled                      : True
IsClassicLog                   : False
SecurityDescriptor             : O:BAG:SYD: ...
LogFilePath                    : %SystemRoot%\System32\Winevt\Logs\Setup.evtx
MaximumSizeInBytes             : 1052672
LogMode                        : Circular
OwningProviderName             : Microsoft-Windows-Eventlog
ProviderNames                  : {Microsoft-Windows-WUSA, Microsoft-Windows-ActionQueue...
ProviderLevel                  :
ProviderKeywords               :
ProviderBufferSize             : 64
ProviderMinimumNumberOfBuffers : 0
ProviderMaximumNumberOfBuffers : 64
ProviderLatency                : 1000
ProviderControlGuid            :
```

コマンドレットでは、 `Get-WinEvent` **listlog** パラメーターを使用して **セットアップ** ログを指定します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Format-List` ます。 `Format-List`**プロパティ** パラメーターをアスタリスク () ワイルドカードと共に使用して、 `*` 各プロパティを表示します。

### 例 3: サーバーからイベントログを取得する

このコマンドは、イベントを含むローカルコンピューター上のイベントログのみを取得します。 ログの **RecordCount** を null または0にすることができます。 この例では、変数を使用し `$_` ます。 詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/about/about_automatic_variables.md)」を参照してください。

```powershell
Get-WinEvent -ListLog * -ComputerName localhost | Where-Object { $_.RecordCount }
```

```Output
LogMode   MaximumSizeInBytes RecordCount LogName
-------   ------------------ ----------- -------
Circular            15532032       14546 Application
Circular             1052672         117 Azure Information Protection
Circular             1052672        2990 CxAudioSvcLog
Circular             1052672           9 MSFTVPN Setup
Circular             1052672         282 OAlerts
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **Listlog** パラメーターでは、アスタリスク ( `*` ) のワイルドカードを使用して、各ログに関する情報を表示します。 **ComputerName** パラメーターは、ローカルコンピューター ( **localhost**) からログを取得するように指定します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Where-Object` ます。 `Where-Object` は `$_.RecordCount` 、データを含むログのみを返すためにを使用します。 `$_` は、パイプライン内の現在のオブジェクトを表す変数です。 **RecordCount** は、null 以外の値を持つオブジェクトのプロパティです。

### 例 4: 複数のサーバーからイベントログを取得する

この例では、Server01、Server02、および Server03 の3台のコンピューター上の **アプリケーション** イベントログを表すオブジェクトを取得します。 **ComputerName** パラメーターは値を1つだけ受け入れるため、 **ForEach** キーワードが使用されます。 詳細については、「 [about_Foreach](../Microsoft.PowerShell.Core/about/about_Foreach.md)」を参照してください。

```powershell
$S = 'Server01', 'Server02', 'Server03'
ForEach ($Server in $S) {
  Get-WinEvent -ListLog Application -ComputerName $Server |
    Select-Object LogMode, MaximumSizeInBytes, RecordCount, LogName,
      @{name='ComputerName'; expression={$Server}} |
    Format-Table -AutoSize
}
```

```Output
 LogMode MaximumSizeInBytes RecordCount LogName     ComputerName
 ------- ------------------ ----------- -------     ------------
Circular           15532032       14577 Application Server01
Circular           15532032        9689 Application Server02
Circular           15532032        5309 Application Server03
```

変数には、 `$S` 3 つのサーバー ( **Server01**、 **Server02**、 **Server03**) という名前が格納されます。 **ForEach** ステートメントでは、ループを使用して各サーバーを処理し `($Server in $S)` ます。 中かっこ () 内のスクリプトブロックは、 `{ }` コマンドを実行し `Get-WinEvent` ます。 **Listlog** パラメーターは、**アプリケーション** ログを指定します。 **ComputerName** パラメーターは、変数を使用して `$Server` 各サーバーからログ情報を取得します。

オブジェクトは、パイプラインを介してコマンドレットに送信され `Select-Object` ます。 `Select-Object`**Logmode**、 **maximumsizeinbytes**、 **RecordCount**、 **LogName** の各プロパティを取得し、計算された式を使用して、その変数を使用して **ComputerName** を表示し `$Server` ます。 オブジェクトは、パイプラインを介してコマンドレットに送信され、 `Format-Table` PowerShell コンソールに出力を表示します。 **AutoSize** パラメーターは、画面に合うように出力を書式設定します。

### 例 5: イベントログプロバイダーとログ名を取得する

このコマンドは、イベントログプロバイダーと、ログの書き込み先のログを取得します。

```powershell
Get-WinEvent -ListProvider *
```

```Output
Name     : .NET Runtime
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}

Name     : .NET Runtime Optimization Service
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **ListProvider** パラメーターでは、アスタリスク ( `*` ) のワイルドカードを使用して、各プロバイダーに関する情報を表示します。 出力では、 **名前** はプロバイダー、 **loglinks** はプロバイダーが書き込むログです。

### 例 6: 特定のログに書き込むすべてのイベントログプロバイダーを取得する

このコマンドは、 **アプリケーション** ログに書き込むすべてのプロバイダーを取得します。

```powershell
(Get-WinEvent -ListLog Application).ProviderNames
```

```Output
.NET Runtime
.NET Runtime Optimization Service
Application
Application Error
Application Hang
Application Management
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **Listlog** パラメーターは、**アプリケーション** を使用して、そのログのオブジェクトを取得します。 **Providernames** はオブジェクトのプロパティであり、 **アプリケーション** ログに書き込むプロバイダーを表示します。

### 例 7: 特定の文字列を含むイベントログプロバイダーの名前を取得する

このコマンドは、プロバイダー名に特定の文字列を含む名前のイベントログプロバイダーを取得します。

```powershell
Get-WinEvent -ListProvider *Policy*
```

```Output
Name     : Group Policy Applications
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}

Name     : Group Policy Client
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}

Name     : Group Policy Data Sources
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **ListProvider** パラメーターは、アスタリスク () のワイルドカードを使用して、 `*` プロバイダーの名前内の任意の場所で **ポリシー** を検索します。

### 例 8: イベントプロバイダーによって生成されるイベント Id を取得する

このコマンドは、イベントの説明と共に、 **Microsoft-Windows-GroupPolicy** イベントプロバイダーによって生成されるイベント id を一覧表示します。

```powershell
(Get-WinEvent -ListProvider Microsoft-Windows-GroupPolicy).Events | Format-Table Id, Description
```

```Output
  Id  Description
  --  -----------
1500  The Group Policy settings for the computer were processed successfully...
1501  The Group Policy settings for the user were processed successfully...
4115  Group Policy Service started.
4116  Started the Group Policy service initialization phase.
4117  Group Policy Session started.
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **ListProvider** パラメーターは、プロバイダー、 **Microsoft-Windows-grouppolicy** を指定します。 式がかっこで囲まれ、 **Events** プロパティを使用してオブジェクトを取得します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Format-Table` ます。 `Format-Table` イベントオブジェクトの **Id** と **説明** を表示します。

### 例 9: イベントオブジェクトのプロパティからログ情報を取得する

この例では、イベントオブジェクトのプロパティを使用して、ログの内容に関する情報を取得する方法を示します。
イベントオブジェクトは変数に格納され、 **イベント Id** と **レベル** によってグループ化およびカウントされます。

```powershell
$Event = Get-WinEvent -LogName 'Windows PowerShell'
$Event.Count
$Event | Group-Object -Property Id -NoElement | Sort-Object -Property Count -Descending
$Event | Group-Object -Property LevelDisplayName -NoElement
```

```Output
195

Count  Name
-----  ----
  147  600
   22  400
   21  601
    3  403
    2  103

Count  Name
-----  ----
    2  Warning
  193  Information
```

`Get-WinEvent`コマンドレットでは、 **LogName** パラメーターを使用して **Windows PowerShell** イベントログを指定します。 イベントオブジェクトは変数に格納され `$Event` ます。 の **Count** プロパティは、 `$Event` ログに記録されたイベントの合計数を示します。

変数は、パイプラインを介して `$Event` コマンドレットに送信され `Group-Object` ます。 `Group-Object`**property** パラメーターを使用して **id** プロパティを指定し、イベント id 値によってオブジェクトをカウントします。 **Noelement** パラメーターは、オブジェクトの出力から他のプロパティを削除します。 グループ化されたオブジェクトは、パイプラインを介してコマンドレットに送信され `Sort-Object` ます。 `Sort-Object`**Property** パラメーターを使用して、オブジェクトを **カウント** で並べ替えます。 [ **降順** ] パラメーターを指定すると、出力は、最大値から最低値までの数で表示されます。 出力の [ **カウント** ] 列には、各イベントの合計数が表示されます。 **名前** 列には、グループ化されたイベント Id 番号が含まれます。

変数は、パイプラインを介して `$Event` コマンドレットに送信され `Group-Object` ます。 `Group-Object`**property** パラメーターを使用して **leveldisplayname** プロパティを指定し、 **leveldisplayname** でオブジェクトをカウントします。 オブジェクトは、 **警告** や **情報** などのレベルによってグループ化されます。
**Noelement** パラメーターは、他のプロパティを出力から削除します。 出力の [ **カウント** ] 列には、各イベントの合計数が表示されます。 **名前** 列には、グループ化された **leveldisplayname** が含まれます。

### 例 10: 名前に文字列が指定されているエラーイベントを取得する

この例では、ログ名のコンマ区切りの文字列を使用します。 出力は、エラー、警告、ログ名などのレベルによってグループ化されます。

```powershell
Get-WinEvent -LogName *PowerShell*, Microsoft-Windows-Kernel-WHEA* |
  Group-Object -Property LevelDisplayName, LogName -NoElement |
    Format-Table -AutoSize
```

```Output
Count  Name
-----  ----
    1  Error, PowerShellCore/Operational
   26  Information, Microsoft-Windows-Kernel-WHEA/Operational
  488  Information, Microsoft-Windows-PowerShell/Operational
   77  Information, PowerShellCore/Operational
 9835  Information, Windows PowerShell
   19  Verbose, PowerShellCore/Operational
  444  Warning, Microsoft-Windows-PowerShell/Operational
  512  Warning, PowerShellCore/Operational
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **LogName** パラメーターでは、アスタリスク () のワイルドカードを含むコンマ区切りの文字列を使用して `*` 、ログ名を指定します。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Group-Object` ます。 `Group-Object`**プロパティ** パラメーターを使用して、 **Leveldisplayname** と **LogName** でオブジェクトをグループ化します。 **Noelement** パラメーターは、他のプロパティを出力から削除します。 グループ化されたオブジェクトは、パイプラインを介してコマンドレットに送信され `Format-Table` ます。 `Format-Table`**AutoSize** パラメーターを使用して、列の書式を設定します。 [ **カウント** 列には、各イベントの合計数が含まれます。 **名前** 列には、グループ化された **Leveldisplayname** と **LogName** が含まれます。

### 例 11: アーカイブされたイベントログからイベントを取得する

`Get-WinEvent` 保存されているログファイルからイベント情報を取得できます。 このサンプルでは、ローカルコンピューターに格納されているアーカイブ済みの PowerShell ログを使用します。

```powershell
Get-WinEvent -Path 'C:\Test\Windows PowerShell.evtx'
```

```Output
   ProviderName: PowerShell

TimeCreated              Id LevelDisplayName  Message
-----------              -- ----------------  -------
3/15/2019 13:54:13      403 Information       Engine state is changed from Available to Stopped...
3/15/2019 13:54:13      400 Information       Engine state is changed from None to Available...
3/15/2019 13:54:13      600 Information       Provider "Variable" is Started...
3/15/2019 13:54:13      600 Information       Provider "Function" is Started...
3/15/2019 13:54:13      600 Information       Provider "FileSystem" is Started...
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **Path** パラメーターには、ディレクトリとファイル名を指定します。

### 例 12: アーカイブされたイベントログから特定の数のイベントを取得する

これらのコマンドは、アーカイブされたイベントログから特定の数のイベントを取得します。 `Get-WinEvent` には、最大数のイベントまたは最も古いイベントを取得できるパラメーターがあります。 このサンプルでは、 **C:\Test\PowerShellCore .evtx** に格納されているアーカイブ済みの PowerShell ログを使用します。

```powershell
Get-WinEvent -Path 'C:\Test\PowerShellCore Operational.evtx' -MaxEvents 100
```

```Output
   ProviderName: PowerShellCore

TimeCreated                 Id   LevelDisplayName  Message
-----------                 --   ----------------  -------
3/15/2019 09:54:54        4104   Warning           Creating Scriptblock text (1 of 1):...
3/15/2019 09:37:13       40962   Information       PowerShell console is ready for user input
3/15/2019 07:56:24        4104   Warning           Creating Scriptblock text (1 of 1):...
...
3/7/2019 10:53:22        40961   Information       PowerShell console is starting up
3/7/2019 10:53:22         8197   Verbose           Runspace state changed to Opening
3/7/2019 10:53:22         8195   Verbose           Opening RunspacePool
```

`Get-WinEvent`コマンドレットは、コンピューターからログ情報を取得します。 **Path** パラメーターには、ディレクトリとファイル名を指定します。 **Maxevents** パラメーターを指定すると、100レコードが最新から古いものに表示されます。

### 例 13: Windows イベントトレーシング

Windows イベントトレーシング (ETW) は、イベントが発生したときにイベントをログに書き込みます。 イベントは、最も古い順に格納されます。 アーカイブされた ETW ファイルは、 `.etl` **tracelog .etl** などとして保存されます。
イベントはログに書き込まれた順序で一覧表示されるため、 *最も古い* パラメーターが必要です。

```powershell
Get-WinEvent -Path 'C:\Tracing\TraceLog.etl' -Oldest |
  Sort-Object -Property TimeCreated -Descending |
    Select-Object -First 100
```

コマンドレットは、アーカイブされ `Get-WinEvent` たファイルからログ情報を取得します。 **Path** パラメーターには、ディレクトリとファイル名を指定します。 **最も古い** パラメーターは、イベントが書き込まれた順序でイベントを出力するために使用されます。 オブジェクトはパイプラインの下に送信され `Sort-Object` 、 `Sort-Object` **TimeCreated** プロパティの値によって降順に並べ替えられます。 オブジェクトは、 `Select-Object` 100 の最新のイベントを表示するコマンドレットにパイプラインを介して送信されます。

### 例 14: イベントトレースログからイベントを取得する

この例では、イベントトレースログファイル ( `.etl` ) およびアーカイブ済みの Windows PowerShell ログファイル () からイベントを取得する方法を示し `.evtx` ます。 1 つのコマンドで複数のファイルの種類を組み合わせることができます。
ファイルには同じ種類の **.NET Framework** オブジェクト **EventLogRecord** が含まれているため、同じプロパティを使用してそれらをフィルター処理できます。 コマンドはファイルから読み取るため、 **最も古い** パラメーターを必要とし `.etl` ますが、 **最も古い** パラメーターは各ファイルに適用されます。

```powershell
Get-WinEvent -Path 'C:\Tracing\TraceLog.etl', 'C:\Test\Windows PowerShell.evtx' -Oldest |
  Where-Object { $_.Id -eq '403' }
```

コマンドレットは、アーカイブされ `Get-WinEvent` たファイルからログ情報を取得します。 **Path** パラメーターでは、コンマ区切りの一覧を使用して、各ファイルのディレクトリとファイル名を指定します。 **最も古い** パラメーターは、イベントが書き込まれた順序でイベントを出力するために使用されます。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Where-Object` ます。 `Where-Object` スクリプトブロックを使用して、および **Id** **403** のイベントを検索します。 変数は、 `$_` パイプライン内の現在のオブジェクトを表し、 **Id** はイベント id プロパティです。

### 例 15: イベントログの結果をフィルター処理する

この例は、イベントログからイベントをフィルター処理および選択するためのさまざまなメソッドを示しています。 これらのコマンドはすべて、 **Windows PowerShell** イベントログから過去24時間以内に発生したイベントを取得します。
フィルターメソッドは、コマンドレットを使用するよりも効率的です `Where-Object` 。 フィルターは、オブジェクトの取得時に適用されます。 `Where-Object` すべてのオブジェクトを取得し、すべてのオブジェクトにフィルターを適用します。

```powershell
# Using the Where-Object cmdlet:
$Yesterday = (Get-Date) - (New-TimeSpan -Day 1)
Get-WinEvent -LogName 'Windows PowerShell' | Where-Object { $_.TimeCreated -ge $Yesterday }

# Using the FilterHashtable parameter:
$Yesterday = (Get-Date) - (New-TimeSpan -Day 1)
Get-WinEvent -FilterHashtable @{ LogName='Windows PowerShell'; Level=3; StartTime=$Yesterday }

# Using the FilterXML parameter:
$xmlQuery = @'
<QueryList>
  <Query Id="0" Path="Windows PowerShell">
    <Select Path="System">*[System[(Level=3) and
        TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select>
  </Query>
</QueryList>
'@
Get-WinEvent -FilterXML $xmlQuery

# Using the FilterXPath parameter:
$XPath = '*[System[Level=3 and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]'
Get-WinEvent -LogName 'Windows PowerShell' -FilterXPath $XPath
```

### 例 16: FilterHashtable を使用してアプリケーションログからイベントを取得する

この例では、 **Filterhashtable** パラメーターを使用して、 **アプリケーション** ログからイベントを取得します。 ハッシュテーブルでは、 **キーと値** のペアが使用されます。 **Filterhashtable** パラメーターの詳細については、「 [filterhashtable を使用した Get-WinEvent クエリの作成](/powershell/scripting/samples/Creating-Get-WinEvent-queries-with-FilterHashtable)」を参照してください。
ハッシュ テーブルの詳細については、「[about_Hash_Tables (ハッシュ テーブルについて)](../Microsoft.PowerShell.Core/about/about_hash_tables.md)」をご覧ください。

```powershell
$Date = (Get-Date).AddDays(-2)
Get-WinEvent -FilterHashtable @{ LogName='Application'; StartTime=$Date; Id='1003' }
```

この `Get-Date` コマンドレットは、 **AddDays** メソッドを使用して、現在の日付の2日前の日付を取得します。 Date オブジェクトは変数に格納され `$Date` ます。

コマンドレットでは、 `Get-WinEvent` ログ情報を取得します。 **Filterhashtable** パラメーターは、出力をフィルター処理するために使用されます。 **LogName** キーは、**アプリケーション** ログとして値を指定します。 **StartTime** キーは、変数に格納されている値を使用し `$Date` ます。 **Id** キーは、イベント id 値 **1003** を使用します。

### 例 17: FilterHashtable を使用してアプリケーションエラーを取得する

この例では、 **Filterhashtable** パラメーターを使用して、先週に発生した Internet Explorer のアプリケーションエラーを検索します。

```powershell
$StartTime = (Get-Date).AddDays(-7)
Get-WinEvent -FilterHashtable @{
  Logname='Application'
  ProviderName='Application Error'
  Data='iexplore.exe'
  StartTime=$StartTime
}
```

この `Get-Date` コマンドレットは、 **AddDays** メソッドを使用して、現在の日付の7日前の日付を取得します。 Date オブジェクトは変数に格納され `$StartTime` ます。

コマンドレットでは、 `Get-WinEvent` ログ情報を取得します。 **Filterhashtable** パラメーターは、出力をフィルター処理するために使用されます。 **LogName** キーは、**アプリケーション** ログとして値を指定します。 **ProviderName** キーは、イベントの **ソース** である、**アプリケーションエラー** という値を使用します。 **データ** キーは値を使用しiexplore.exe **StartTime** キーは変数に格納されている値を使用し `$StartTime` ます。

### 例 18: SuppressHashFilter を使用してアプリケーションエラーをフィルター処理する

上の例16と同様に、この例では、 **Filterhashtable** パラメーターを使用して、 **アプリケーション** ログからイベントを取得します。 ただし、**情報** レベルのイベントを除外するには、 **SuppressHashFilter** キーを追加します。

```powershell
$Date = (Get-Date).AddDays(-2)
$filter = @{
  LogName='Application'
  StartTime=$Date
  SuppressHashFilter=@{Level=4}
}
Get-WinEvent -FilterHashtable $filter
```

この例では、は、 `Get-WinEvent` **レベル** 4 (情報) を持つイベントを除いて、**アプリケーション** ログからすべてのイベントを取得します。

## PARAMETERS

### -ComputerName

このコマンドレットがイベントログからイベントを取得するコンピューターの名前を指定します。 コンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名 (FQDN) を入力します。 既定値はローカルコンピューター **localhost** です。 このパラメーターは、一度に 1 つのみコンピューター名を受け入れます。

リモートコンピューターからイベントログを取得するには、リモートアクセスを許可するようにイベントログサービスのファイアウォールポートを構成します。

このコマンドレットは、PowerShell リモート処理に依存しません。 コンピューターがリモート コマンドを実行するように構成されていない場合でも、**ComputerName** パラメーターを使用できます。

```yaml
Type: System.String
Parameter Sets: GetLogSet, ListLogSet, ListProviderSet, GetProviderSet, HashQuerySet, XmlQuerySet
Aliases: Cn

Required: False
Position: Named
Default value: Local computer
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力します。 または、コマンドレットによって生成されるような **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードの入力を求めるメッセージが表示されます。 パラメーター名のみを入力した場合は、ユーザー名とパスワードの両方を入力するように求められます。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilterHashtable

1つ以上のイベントログからイベントを選択するために、ハッシュテーブル形式でクエリを指定します。 このクエリには、1つまたは複数の **キーと値** のペアを持つハッシュテーブルが含まれています。

ハッシュ テーブル クエリには、次のルールがあります。

- キーと値では大文字と小文字が区別されません。
- ワイルドカード文字は、 **LogName** および **ProviderName** キーに関連付けられている値でのみ有効です。
- 各キーは、各ハッシュテーブルに1回だけ表示できます。
- **パス** の値には `.etl` 、、、およびの各ログファイルへのパスを指定し `.evt` `.evtx` ます。
- **LogName**、 **Path**、および **ProviderName** キーは、同じクエリで使用できます。
- **UserID** キーは有効なセキュリティ識別子 (SID) またはドメインアカウント名を受け取ることができます。この名前は、有効な **system.object オブジェクト** を構築するために使用できます。
- **データ** 値は、名前のないフィールドでイベントデータを受け取ります。 たとえば、従来のイベントログのイベントです。
- `<named-data>` キーは、名前付きイベントデータフィールドを表します。

`Get-WinEvent`は、**キーと値** のペアを解釈できない場合、イベント内のイベントデータに対して、大文字と小文字が区別される名前としてキーを解釈します。

有効な `Get-WinEvent` **キーと値** のペアは次のとおりです。

- **/L**=`<String[]>`
- **プロバイダー**=`<String[]>`
- **道**=`<String[]>`
- **Keywords**=`<Long[]>`
- **番号**=`<Int32[]>`
- **平準**=`<Int32[]>`
- **/St**=`<DateTime>`
- **EndTime**=`<DateTime>`
- **UserID**=`<SID>`
- **データ**=`<String[]>`
- `<named-data>`=`<String[]>`
- **SuppressHashFilter**=`<Hashtable>`

```yaml
Type: System.Collections.Hashtable[]
Parameter Sets: HashQuerySet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilterXml

このコマンドレットが1つ以上のイベントログからイベントを選択する構造化 XML クエリを指定します。

有効な XML クエリを生成するには、Windows イベントビューアーの [ **カスタムビューの作成** ] と [ **現在のログをフィルター** ] 機能を使用します。 ダイアログ ボックス内の項目を使用してクエリを作成し、その後、[XML] タブをクリックして XML 形式でクエリを表示します。 [XML] タブから **FilterXml** パラメーターの値に XML をコピーできます。 イベント ビューアーの機能の詳細については、イベント ビューアーのヘルプを参照してください。

XML クエリを使用して、いくつかの XPath ステートメントを含む複雑なクエリを作成します。 XML 形式では、クエリからのイベントを除外する非表示の **xml** 要素を使用することもできます。 イベントログクエリの XML スキーマの詳細については、「 [クエリスキーマ](/windows/win32/wes/queryschema-schema) 」および「 [イベントの選択](/previous-versions/aa385231(v=vs.85))」の「xml イベントクエリ」セクションを参照してください。

**Filterhashtable** パラメーターを使用して、**抑制** 要素を作成することもできます。

```yaml
Type: System.Xml.XmlDocument
Parameter Sets: XmlQuerySet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilterXPath

このコマンドレットが1つ以上のログからイベントを選択する XPath クエリを指定します。

XPath 言語の詳細については、「 [Xpath リファレンス](/previous-versions/dotnet/netframework-4.0/ms256115(v=vs.100)) 」および「イベントの [選択](/previous-versions/aa385231(v=vs.85))」の選択フィルターに関するセクションを参照してください。

```yaml
Type: System.String
Parameter Sets: GetLogSet, GetProviderSet, FileSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

その他のイベント ログに加え、デバッグ ログと分析ログを取得します。 **Force** パラメーターは、Name パラメーターにワイルドカード文字が含まれる場合に、デバッグ ログと分析ログを取得するために必要です。

既定では、 `Get-WinEvent` コマンドレットは、デバッグログまたは分析ログの完全な名前を指定しない限り、これらのログを除外します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: GetLogSet, ListLogSet, GetProviderSet, HashQuerySet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ListLog

イベント ログを指定します。 コンマ区切りの一覧で、イベント ログ名を入力します。 ワイルドカードを使用できます。 すべてのログを取得するには、アスタリスク ( `*` ) ワイルドカードを使用します。

```yaml
Type: System.String[]
Parameter Sets: ListLogSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ListProvider

このコマンドレットが取得するイベントログプロバイダーを指定します。 イベント ログ プロバイダーは、イベント ログにイベントを書き込むプログラムまたはサービスです。

コンマ区切りの一覧でプロバイダー名を入力します。 ワイルドカードを使用できます。 コンピューター上のすべてのイベントログのプロバイダーを取得するには、アスタリスク ( `*` ) ワイルドカードを使用します。

```yaml
Type: System.String[]
Parameter Sets: ListProviderSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -LogName

このコマンドレットがイベントを取得するイベントログを指定します。 コンマ区切りの一覧で、イベント ログ名を入力します。 ワイルドカードを使用できます。 また、ログ名をコマンドレットにパイプすることもでき `Get-WinEvent` ます。

> [!NOTE]
> PowerShell では、要求できるログの量が制限されていません。 ただし、この `Get-WinEvent` コマンドレットは、制限が256の WINDOWS API を照会します。 これにより、すべてのログを一度にフィルター処理するのが困難になる可能性があります。 この問題を回避するには、ループを使用して、次 `foreach` のように各ログを反復処理します。 `Get-WinEvent -ListLog * | ForEach-Object{ Get-WinEvent -LogName $_.Logname }`

```yaml
Type: System.String[]
Parameter Sets: GetLogSet
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -MaxEvents

返されるイベントの最大数を指定します。 100などの整数を入力します。 既定では、ログやファイル内のすべてのイベントを返します。

```yaml
Type: System.Int64
Parameter Sets: GetLogSet, GetProviderSet, FileSet, HashQuerySet, XmlQuerySet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -最も古い

このコマンドレットが、古い順にイベントを取得することを示します。 既定では、イベントは新しい順に返されます。

このパラメーターは、およびファイルからイベントを取得し、 `.etl` `.evt` デバッグログと分析ログからイベントを取得するために必要です。 これらのファイルでは、イベントは古い順に記録され、イベントは古い順でのみ返すことができます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: GetLogSet, GetProviderSet, FileSet, HashQuerySet, XmlQuerySet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

このコマンドレットがイベントを取得するイベントログファイルのパスを指定します。 コンマ区切りの一覧でログ ファイルへのパスを入力するか、ワイルドカード文字を使用してファイル パス パターンを作成します。

`Get-WinEvent` は、、 `.evt` `.evtx` 、およびファイル名拡張子を持つファイルをサポートし `.etl` ます。 同じコマンドで、異なるファイルおよびファイルの種類からのイベントを含めることができます。

```yaml
Type: System.String[]
Parameter Sets: FileSet
Aliases: PSPath

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -ProviderName

このコマンドレットがイベントを取得するイベントログプロバイダーを文字列配列として指定します。 コンマ区切りの一覧でプロバイダー名を入力するか、ワイルドカード文字を使用してプロバイダー名パターンを作成します。

イベント ログ プロバイダーは、イベント ログにイベントを書き込むプログラムまたはサービスです。 PowerShell プロバイダーではありません。

```yaml
Type: System.String[]
Parameter Sets: GetProviderSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.string、System.Xml.Xmlドキュメント、System. Collections. Hashtable

では、 **LogName** (文字列)、 **filterxml** クエリ、または **filterxml** クエリをにパイプラインでき `Get-WinEvent` ます。

## 出力

### EventLogConfiguration、EventLogRecord、、..................。

**Listlog** パラメーターを指定すると、 `Get-WinEvent` **EventLogConfiguration** オブジェクトが返されます。

**ListProvider** パラメーターを指定すると、は system.string `Get-WinEvent` **メタデータ** オブジェクトを返します。

他のすべてのパラメーターと共に、 `Get-WinEvent` **EventLogRecord** オブジェクトを返します。

## 注

`Get-WinEvent` は、 `Get-EventLog` Windows Vista 以降のバージョンの windows を実行しているコンピューターでコマンドレットを置き換えるように設計されています。 `Get-EventLog` 従来のイベントログでのみイベントを取得します。 `Get-EventLog` 関数は下位互換性のために残されています。

`Get-WinEvent`および `Get-EventLog` コマンドレットは、Windows プレインストール環境 (windows PE) ではサポートされていません。

## 関連リンク

[about_Automatic_Variables](../Microsoft.PowerShell.Core/about/about_automatic_variables.md)

[about_Foreach](../Microsoft.PowerShell.Core/about/about_Foreach.md)

[about_Hash_Tables](../Microsoft.PowerShell.Core/about/about_hash_tables.md)

[FilterHashtable を使った Get-WinEvent クエリの作成](/powershell/scripting/samples/Creating-Get-WinEvent-queries-with-FilterHashtable)

[Format-Table](../Microsoft.PowerShell.Utility/Format-Table.md)

[Group-Object](../Microsoft.PowerShell.Utility/Group-Object.md)

[Sort-Object](../Microsoft.PowerShell.Utility/Sort-Object.md)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)

