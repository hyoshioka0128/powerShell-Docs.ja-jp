---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-Command
ms.openlocfilehash: d8f0a8a56792a39371a307166788f047fec99a9a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599528"
---
# Invoke-Command

## 概要
ローカル コンピューターおよびリモート コンピューター上でコマンドを実行します。

## SYNTAX

### InProcess (既定)

```
Invoke-Command [-ScriptBlock] <ScriptBlock> [-NoNewScope] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] [<CommonParameters>]
```

### FilePathRunspace 空間

```
Invoke-Command [[-Session] <PSSession[]>] [-ThrottleLimit <Int32>] [-AsJob] [-HideComputerName]
 [-JobName <String>] [-FilePath] <String> [-RemoteDebug] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] [<CommonParameters>]
```

### セッション

```
Invoke-Command [[-Session] <PSSession[]>] [-ThrottleLimit <Int32>] [-AsJob] [-HideComputerName]
 [-JobName <String>] [-ScriptBlock] <ScriptBlock> [-RemoteDebug] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] [<CommonParameters>]
```

### FilePathComputerName

```
Invoke-Command [[-ComputerName] <String[]>] [-Credential <PSCredential>] [-Port <Int32>] [-UseSSL]
 [-ConfigurationName <String>] [-ApplicationName <String>] [-ThrottleLimit <Int32>] [-AsJob]
 [-InDisconnectedSession] [-SessionName <String[]>] [-HideComputerName] [-JobName <String>]
 [-FilePath] <String> [-SessionOption <PSSessionOption>] [-Authentication <AuthenticationMechanism>]
 [-EnableNetworkAccess] [-RemoteDebug] [-InputObject <PSObject>] [-ArgumentList <Object[]>]
 [<CommonParameters>]
```

### [ComputerName]

```
Invoke-Command [[-ComputerName] <String[]>] [-Credential <PSCredential>] [-Port <Int32>] [-UseSSL]
 [-ConfigurationName <String>] [-ApplicationName <String>] [-ThrottleLimit <Int32>] [-AsJob]
 [-InDisconnectedSession] [-SessionName <String[]>] [-HideComputerName] [-JobName <String>]
 [-ScriptBlock] <ScriptBlock> [-SessionOption <PSSessionOption>]
 [-Authentication <AuthenticationMechanism>] [-EnableNetworkAccess] [-RemoteDebug]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] [-CertificateThumbprint <String>]
 [<CommonParameters>]
```

### Uri

```
Invoke-Command [-Credential <PSCredential>] [-ConfigurationName <String>] [-ThrottleLimit <Int32>]
 [[-ConnectionUri] <Uri[]>] [-AsJob] [-InDisconnectedSession] [-HideComputerName]
 [-JobName <String>] [-ScriptBlock] <ScriptBlock> [-AllowRedirection]
 [-SessionOption <PSSessionOption>] [-Authentication <AuthenticationMechanism>]
 [-EnableNetworkAccess] [-RemoteDebug] [-InputObject <PSObject>] [-ArgumentList <Object[]>]
 [-CertificateThumbprint <String>] [<CommonParameters>]
```

### FilePathUri

```
Invoke-Command [-Credential <PSCredential>] [-ConfigurationName <String>] [-ThrottleLimit <Int32>]
 [[-ConnectionUri] <Uri[]>] [-AsJob] [-InDisconnectedSession] [-HideComputerName]
 [-JobName <String>] [-FilePath] <String> [-AllowRedirection] [-SessionOption <PSSessionOption>]
 [-Authentication <AuthenticationMechanism>] [-EnableNetworkAccess] [-RemoteDebug]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] [<CommonParameters>]
```

### VMId

```
Invoke-Command -Credential <PSCredential> [-ConfigurationName <String>] [-ThrottleLimit <Int32>]
 [-AsJob] [-HideComputerName] [-ScriptBlock] <ScriptBlock> [-RemoteDebug] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] [-VMId] <Guid[]> [<CommonParameters>]
```

### VMName

```
Invoke-Command -Credential <PSCredential> [-ConfigurationName <String>] [-ThrottleLimit <Int32>]
 [-AsJob] [-HideComputerName] [-ScriptBlock] <ScriptBlock> [-RemoteDebug] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] -VMName <String[]> [<CommonParameters>]
```

### FilePathVMId

```
Invoke-Command -Credential <PSCredential> [-ConfigurationName <String>] [-ThrottleLimit <Int32>]
 [-AsJob] [-HideComputerName] [-FilePath] <String> [-RemoteDebug] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] [-VMId] <Guid[]> [<CommonParameters>]
```

### FilePathVMName

```
Invoke-Command -Credential <PSCredential> [-ConfigurationName <String>] [-ThrottleLimit <Int32>]
 [-AsJob] [-HideComputerName] [-FilePath] <String> [-RemoteDebug] [-InputObject <PSObject>]
 [-ArgumentList <Object[]>] -VMName <String[]> [<CommonParameters>]
```

### SSHHost

```
Invoke-Command [-Port <Int32>] [-AsJob] [-HideComputerName] [-JobName <String>]
 [-ScriptBlock] <ScriptBlock> -HostName <String[]> [-UserName <String>] [-KeyFilePath <String>]
 [-SSHTransport] [-RemoteDebug] [-InputObject <PSObject>] [-ArgumentList <Object[]>]
 [-Subsystem <String>] [<CommonParameters>]
```

### ContainerId

```
Invoke-Command [-ConfigurationName <String>] [-ThrottleLimit <Int32>] [-AsJob] [-HideComputerName]
 [-JobName <String>] [-ScriptBlock] <ScriptBlock> [-RunAsAdministrator] [-RemoteDebug]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] -ContainerId <String[]> [<CommonParameters>]
```

### FilePathContainerId

```
Invoke-Command [-ConfigurationName <String>] [-ThrottleLimit <Int32>] [-AsJob] [-HideComputerName]
 [-JobName <String>] [-FilePath] <String> [-RunAsAdministrator] [-RemoteDebug]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] -ContainerId <String[]> [<CommonParameters>]
```

### SSHHostHashParam

```
Invoke-Command [-AsJob] [-HideComputerName] [-JobName <String>] [-ScriptBlock] <ScriptBlock>
 -SSHConnection <Hashtable[]> [-RemoteDebug] [-InputObject <PSObject>] [-ArgumentList <Object[]>]
 [<CommonParameters>]
```

### FilePathSSHHost

```
Invoke-Command [-AsJob] [-HideComputerName] -FilePath <String> -HostName <String[]>
 [-UserName <String>] [-KeyFilePath <String>] [-SSHTransport] [-RemoteDebug]
 [-InputObject <PSObject>] [-ArgumentList <Object[]>] [<CommonParameters>]
```

### FilePathSSHHostHash

```
Invoke-Command [-AsJob] [-HideComputerName] -FilePath <String> -SSHConnection <Hashtable[]>
 [-RemoteDebug] [-InputObject <PSObject>] [-ArgumentList <Object[]>] [<CommonParameters>]
```

## Description

コマンド `Invoke-Command` レットは、ローカルコンピューターまたはリモートコンピューターでコマンドを実行し、エラーを含め、コマンドからのすべての出力を返します。 1つの `Invoke-Command` コマンドを使用すると、複数のコンピューターでコマンドを実行できます。

リモート コンピューターで 1 つのコマンドを実行するには、**ComputerName** パラメーターを使用します。 データを共有する一連の関連コマンドを実行するには、コマンドレットを使用して、 `New-PSSession` リモートコンピューター上に **pssession** (永続的な接続) を作成し、の **Session** パラメーターを使用して、 `Invoke-Command` **pssession** でコマンドを実行します。 切断されたセッションのコマンドを実行するには、**InDisconnectedSession** パラメーターを使用します。 コマンドをバックグラウンド ジョブとして実行するには、**AsJob** パラメーターを使用します。

また、 `Invoke-Command` コマンドとしてスクリプトブロックにローカルコンピューターでを使用することもできます。 PowerShell は、現在のスコープの子スコープでスクリプトブロックを直ちに実行します。

を使用して `Invoke-Command` リモートコンピューターでコマンドを実行する前に、 [about_Remote](./About/about_Remote.md)を参照してください。

PowerShell 6.0 以降では、Secure Shell (SSH) を使用して、リモートコンピューターに対する接続を確立し、コマンドを呼び出すことができます。 SSH がローカルコンピューターにインストールされ、リモートコンピューターが PowerShell SSH エンドポイントで構成されている必要があります。 SSH ベースの PowerShell リモートセッションの利点は、複数のプラットフォーム (Windows、Linux、macOS) で動作することです。 SSH ベースのセッションの場合は、 **HostName** パラメーターまたは **SSHConnection** パラメーターを使用して、リモートコンピューターと関連する接続情報を指定します。 PowerShell SSH リモート処理の設定方法の詳細については、「 [Ssh 経由の Powershell リモート](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)処理」を参照してください。

一部のコードサンプルでは、スプラッティングを使用して行の長さを減らしています。 詳細については、「 [about_Splatting](./About/about_Splatting.md)」を参照してください。

## 例

### 例 1: サーバーでスクリプトを実行する

この例では、 `Test.ps1` Server01 コンピューターでスクリプトを実行します。

```powershell
Invoke-Command -FilePath c:\scripts\test.ps1 -ComputerName Server01
```

**FilePath** パラメーターには、ローカルコンピューター上にあるスクリプトが指定されています。 このスクリプトは、リモート コンピューターで実行され、結果はローカル コンピューターに返されます。

### 例 2: リモートサーバーでコマンドを実行する

この例では `Get-Culture` 、Server01 リモートコンピューターでコマンドを実行します。

```powershell
Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock { Get-Culture }
```

**ComputerName** パラメーターは、リモートコンピューターの名前を指定します。 **Credential** パラメーターは、コマンドを実行する権限を持つユーザーである domain01\user01」のセキュリティコンテキストでコマンドを実行するために使用されます。 **ScriptBlock** パラメーターは、リモートコンピューターで実行するコマンドを指定します。

応答として、PowerShell は User01 アカウントのパスワードと認証方法を要求します。
次に、Server01 コンピューターでコマンドを実行して結果を返します。

### 例 3: 永続的な接続でコマンドを実行する

この例では、 `Get-Culture` Server02 という名前のリモートコンピューター上で、永続的な接続を使用してセッションで同じコマンドを実行します。

```powershell
$s = New-PSSession -ComputerName Server02 -Credential Domain01\User01
Invoke-Command -Session $s -ScriptBlock {Get-Culture}
```

`New-PSSession`コマンドレットは、Server02 リモートコンピューター上にセッションを作成し、変数に保存し `$s` ます。 通常、セッションを作成するには、リモートコンピューターで一連のコマンドを実行する必要があります。

`Invoke-Command`コマンドレットでは、 `Get-Culture` Server02 でコマンドを実行します。 **Session** パラメーターは、変数に保存されているセッションを指定し `$s` ます。

応答として、PowerShell は Server02 コンピューター上のセッションでコマンドを実行します。

### 例 4: セッションを使用してデータを共有する一連のコマンドを実行する

この例では、の **ComputerName** パラメーターと **Session** パラメーターを使用した場合の効果を比較し `Invoke-Command` ます。 セッションを使用して、同じデータを共有する一連のコマンドを実行する方法を示しています。

```powershell
Invoke-Command -ComputerName Server02 -ScriptBlock {$p = Get-Process PowerShell}
Invoke-Command -ComputerName Server02 -ScriptBlock {$p.VirtualMemorySize}
$s = New-PSSession -ComputerName Server02
Invoke-Command -Session $s -ScriptBlock {$p = Get-Process PowerShell}
Invoke-Command -Session $s -ScriptBlock {$p.VirtualMemorySize}
```

```Output
17930240
```

最初の2つのコマンドは、の **ComputerName** パラメーターを使用して `Invoke-Command` 、Server02 リモートコンピューターでコマンドを実行します。 最初のコマンドは、 `Get-Process` コマンドレットを使用して、リモートコンピューター上の PowerShell プロセスを取得し、変数に保存し `$p` ます。 2 番目のコマンドは、PowerShell プロセスの **VirtualMemorySize** プロパティの値を取得します。

**ComputerName** パラメーターを使用すると、PowerShell によってコマンドを実行する新しいセッションが作成されます。
コマンドが完了すると、セッションは閉じられます。 `$p`変数は1つの接続で作成されましたが、2番目のコマンド用に作成された接続には存在しません。

この問題は、リモートコンピューターで永続的なセッションを作成し、両方のコマンドを同じセッションで実行することによって解決されます。

この `New-PSSession` コマンドレットは、Server02 コンピューター上に永続的なセッションを作成し、そのセッションを変数に保存し `$s` ます。 次の行では、 `Invoke-Command` **session** パラメーターを使用して、同じセッションで両方のコマンドを実行します。 両方のコマンドが同じセッションで実行されるため、 `$p` 値はアクティブなままになります。

### 例 5: ローカル変数に格納されているコマンドを入力する

この例では、スクリプトブロックとしてローカル変数に格納されているコマンドを作成する方法を示します。
スクリプトブロックがローカル変数に保存されている場合は、 **ScriptBlock** パラメーターの値として変数を指定できます。

```powershell
$command = { Get-WinEvent -LogName PowerShellCore/Operational |
  Where-Object {$_.Message -like "*certificate*"} }
Invoke-Command -ComputerName S1, S2 -ScriptBlock $command
```

変数には、 `$command` `Get-WinEvent` スクリプトブロックとして書式設定されたコマンドが格納されます。 は、 `Invoke-Command` `$command` S1 および S2 リモートコンピューター上のに格納されているコマンドを実行します。

### 例 6: 複数のコンピューターで1つのコマンドを実行する

この例 `Invoke-Command` では、を使用して、複数のコンピューターで1つのコマンドを実行する方法を示します。

```powershell
$parameters = @{
  ComputerName = "Server01", "Server02", "TST-0143", "localhost"
  ConfigurationName = 'MySession.PowerShell'
  ScriptBlock = { Get-WinEvent -LogName PowerShellCore/Operational }
}
Invoke-Command @parameters
```

**ComputerName** パラメーターは、コンマで区切られたコンピューター名のリストを指定します。 コンピューターの一覧には、ローカルコンピューターを表す localhost 値が含まれています。 **ConfigurationName** パラメーターは、代替のセッション構成を指定します。 **ScriptBlock** パラメーターは、 `Get-WinEvent` 各コンピューターから Powershellcore/Operational イベントログを取得するために実行されます。

### 例 7: 複数のコンピューターでホストプログラムのバージョンを取得する

この例では、200リモートコンピューター上で実行されている PowerShell ホストプログラムのバージョンを取得します。

```powershell
$version = Invoke-Command -ComputerName (Get-Content Machines.txt) -ScriptBlock {(Get-Host).Version}
```

実行されるコマンドは1つだけなので、各コンピューターへの永続的な接続を作成する必要はありません。 ただし、**ComputerName** パラメーターを使用してコンピューターを指定します。 コンピューターを指定するには、コマンドレットを使用して、 `Get-Content` コンピューター名のファイルである Machine.txt ファイルの内容を取得します。

`Invoke-Command`コマンドレットは、 `Get-Host` リモートコンピューターでコマンドを実行します。 ドット表記を使用して、PowerShell ホストの **Version** プロパティを取得します。

これらのコマンドは一度に1つずつ実行します。 コマンドが完了すると、すべてのコンピューターからのコマンドの出力が変数に保存され `$version` ます。 出力には、データの発生元であるコンピューターの名前も含まれます。

### 例 8: 複数のリモートコンピューターでバックグラウンドジョブを実行する

この例では、2台のリモートコンピューターでコマンドを実行します。 コマンドは `Invoke-Command` **AsJob** パラメーターを使用して、コマンドがバックグラウンドジョブとして実行されるようにします。 コマンドはリモートコンピューター上で実行されますが、ジョブはローカルコンピューター上に存在します。 結果はローカルコンピューターに送信されます。

```powershell
$s = New-PSSession -ComputerName Server01, Server02
Invoke-Command -Session $s -ScriptBlock {Get-EventLog system} -AsJob
```

```Output
Id   Name    State      HasMoreData   Location           Command
---  ----    -----      -----         -----------        ---------------
1    Job1    Running    True          Server01,Server02  Get-EventLog system
```

```powershell
$j = Get-Job
$j | Format-List -Property *
```

```Output
HasMoreData   : True
StatusMessage :
Location      : Server01,Server02
Command       : Get-EventLog system
JobStateInfo  : Running
Finished      : System.Threading.ManualResetEvent
InstanceId    : e124bb59-8cb2-498b-a0d2-2e07d4e030ca
Id            : 1
Name          : Job1
ChildJobs     : {Job2, Job3}
Output        : {}
Error         : {}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
StateChanged  :
```

```powershell
$results = $j | Receive-Job
```

コマンドレットでは、 `New-PSSession` Server01 と Server02 のリモートコンピューターにセッションを作成します。 `Invoke-Command`コマンドレットは、各セッションでバックグラウンドジョブを実行します。 このコマンドは **AsJob** パラメーターを使用して、バックグラウンド ジョブとしてコマンドを実行します。 このコマンドは、2 台のリモート コンピューターでそれぞれ実行されるジョブに対応する 2 つの子ジョブ オブジェクトを含むジョブ オブジェクトを返します。

この `Get-Job` コマンドは、ジョブオブジェクトを変数に保存し `$j` ます。 次に、 `$j` 変数をコマンドレットにパイプして、 `Format-List` ジョブオブジェクトのすべてのプロパティを一覧に表示します。 最後のコマンドは、ジョブの結果を取得します。 ジョブオブジェクトを `$j` コマンドレットにパイプ処理 `Receive-Job` し、結果を変数に格納し `$results` ます。

### 例 9: リモートコンピューターでコマンドを実行するときにローカル変数を含める

この例は、リモート コンピューターで実行されるコマンドにローカル変数の値を含める方法を示しています。 このコマンドは、 `Using` スコープ修飾子を使用して、リモートコマンドでローカル変数を識別します。 既定では、すべての変数は、リモート セッションで定義されると見なされます。 `Using`スコープ修飾子は、PowerShell 3.0 で導入されました。 スコープ修飾子の詳細については `Using` 、「 [about_Remote_Variables](./About/about_Remote_Variables.md) 」および「 [about_Scopes](./about/about_scopes.md)」を参照してください。

```powershell
$Log = "PowerShellCore/Operational"
Invoke-Command -ComputerName Server01 -ScriptBlock {Get-WinEvent -LogName $Using:Log -MaxEvents 10}
```

変数には、 `$Log` イベントログの名前 (PowerShellCore/Operational) が格納されます。 `Invoke-Command`コマンドレットは、Server01 上で実行され、 `Get-WinEvent` イベントログから最新の10個のイベントを取得します。 **LogName** パラメーターの値は、 `$Log` `Using` リモートセッションではなくローカルセッションで作成されたことを示すために、スコープ修飾子のプレフィックスが付いた変数です。

### 例 10: コンピューター名を非表示にする

この例は、の **HideComputerName** パラメーターを使用した場合の効果を示して `Invoke-Command` います。
**HideComputerName** は、このコマンドレットが返すオブジェクトを変更しません。 表示のみが変更されます。 **Format** コマンドレットを使用して、影響を受けるオブジェクトの **PsComputerName** プロパティを表示することもできます。

```powershell
Invoke-Command -ComputerName S1, S2 -ScriptBlock {Get-Process PowerShell}
```

```Output
PSComputerName    Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id   ProcessName
--------------    -------  ------    -----      ----- -----   ------     --   -----------
S1                575      15        45100      40988   200     4.68     1392 PowerShell
S2                777      14        35100      30988   150     3.68     67   PowerShell
```

```powershell
Invoke-Command -ComputerName S1, S2 -ScriptBlock {Get-Process PowerShell} -HideComputerName
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id   ProcessName
-------  ------    -----      ----- -----   ------     --   -----------
575      15        45100      40988   200     4.68     1392 PowerShell
777      14        35100      30988   150     3.68     67   PowerShell
```

最初の2つのコマンドは、を使用して `Invoke-Command` `Get-Process` PowerShell プロセス用のコマンドを実行します。 最初のコマンドの出力には、**PsComputerName** プロパティが含まれます。このプロパティは、コマンドが実行されているコンピューター名を示します。 **HideComputerName** を使用する2番目のコマンドの出力には、 **PsComputerName** 列が含まれていません。

### 例 11: スクリプトブロックで Param キーワードを使用する

`Param`キーワードと **ArgumentList** パラメーターは、変数の値をスクリプトブロック内の名前付きパラメーターに渡すために使用されます。 この例では、文字で始まり、拡張子がのファイル名が表示され `a` `.pdf` ます。

キーワードの詳細については `Param` 、「 [about_Language_Keywords](./about/about_language_keywords.md#param)」を参照してください。

```powershell
$parameters = @{
    ComputerName = "Server01"
    ScriptBlock = { Param ($param1,$param2) Get-ChildItem -Name $param1 -Include $param2 }
    ArgumentList = "a*", "*.pdf"
}
Invoke-Command @parameters
```

```Output
aa.pdf
ab.pdf
ac.pdf
az.pdf
```

`Invoke-Command` 2つの変数 (と) を定義する **ScriptBlock** パラメーターを使用し `$param1` `$param2` ます。 `Get-ChildItem` 名前付きパラメーター、 **名前** **、およびを変数名と共** に使用します。 **ArgumentList** は、値を変数に渡します。

### 例 12: スクリプトブロックで $args 自動変数を使用する

`$args`自動変数と **ArgumentList** パラメーターは、配列の値をスクリプトブロック内のパラメーターの位置に渡すために使用されます。 この例では、ファイルのサーバーのディレクトリの内容を表示 `.txt` します。 `Get-ChildItem` **Path** パラメーターは position 0 で、 **Filter** パラメーターは position です。
1.

変数の詳細については `$args` 、「」を参照してください [about_Automatic_Variables](./about/about_automatic_variables.md#args)

```powershell
$parameters = @{
    ComputerName = "Server01"
    ScriptBlock = { Get-ChildItem $args[0] $args[1] }
    ArgumentList = "C:\Test", "*.txt*"
}
Invoke-Command @parameters
```

```Output
    Directory: C:\Test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           6/12/2019    15:15            128 alog.txt
-a---           7/27/2019    15:16            256 blog.txt
-a---           9/28/2019    17:10             64 zlog.txt
```

`Invoke-Command`**ScriptBlock** パラメーターを使用し、 `Get-ChildItem` との配列値を指定し `$args[0]` `$args[1]` ます。 **ArgumentList** は、 `$args` `Get-ChildItem` **パス** と **フィルター** のパラメーター位置に配列値を渡します。

### 例 13: テキストファイルに一覧表示されているすべてのコンピューターでスクリプトを実行する

この例では、 `Invoke-Command` コマンドレットを使用し `Sample.ps1` て、ファイルに一覧表示されているすべてのコンピューターでスクリプトを実行し `Servers.txt` ます。 このコマンドは、**FilePath** パラメーターを使用して、スクリプト ファイルを指定しています。 このコマンドを使用すると、リモートコンピューターでスクリプトファイルにアクセスできない場合でも、リモートコンピューターでスクリプトを実行できます。

```powershell
Invoke-Command -ComputerName (Get-Content Servers.txt) -FilePath C:\Scripts\Sample.ps1 -ArgumentList Process, Service
```

コマンドを送信すると、ファイルの内容 `Sample.ps1` がスクリプトブロックにコピーされ、スクリプトブロックが各リモートコンピューター上で実行されます。 この手順では、**ScriptBlock** パラメーターを使用してスクリプトの内容を送信した場合と同じ結果が得られます。

### 例 14: URI を使用してリモートコンピューターでコマンドを実行する

この例では、Uniform Resource Identifier (URI) で識別されるリモートコンピューターでコマンドを実行する方法を示します。 この特定の例では、 `Set-Mailbox` リモートの Exchange サーバー上でコマンドを実行します。

```powershell
$LiveCred = Get-Credential
$parameters = @{
  ConfigurationName = 'Microsoft.Exchange'
  ConnectionUri = 'https://ps.exchangelabs.com/PowerShell'
  Credential = $LiveCred
  Authentication = 'Basic'
  ScriptBlock = {Set-Mailbox Dan -DisplayName "Dan Park"}
}
Invoke-Command @parameters
```

最初の行では、コマンドレットを使用して、 `Get-Credential` Windows LIVE ID の資格情報を変数に格納し `$LiveCred` ます。 PowerShell は、Windows Live ID の資格情報を入力するようにユーザーに求めます。

`$parameters`変数は、コマンドレットに渡されるパラメーターを含むハッシュテーブルです `Invoke-Command` 。 `Invoke-Command`コマンドレットは、 `Set-Mailbox` **Microsoft Exchange** のセッション構成を使用してコマンドを実行します。 **ConnectionURI** パラメーターは、Exchange サーバーのエンドポイントの URL を指定しています。 **Credential** パラメーターは、変数に格納されている資格情報を指定し `$LiveCred` ます。 **AuthenticationMechanism** パラメーターでは、基本認証の使用を指定しています。 **ScriptBlock** パラメーターは、コマンドが含まれているスクリプト ブロックを指定しています。

### 例 15: セッションオプションを使用する

この例では、 **Sessionoption** パラメーターを作成して使用する方法を示します。

```powershell
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
Invoke-Command -ComputerName server01 -UseSSL -ScriptBlock { Get-HotFix } -SessionOption $so -Credential server01\user01
```

コマンドレットでは、 `New-PSSessionOption` セッションオプションオブジェクトを作成します。これにより、リモートエンドは、受信 HTTPS 接続の評価中に証明機関、正規名、および失効リストを検証しません。 **Sessionoption** オブジェクトは変数に保存され `$so` ます。

> [!NOTE]
> これらのチェックを無効にすると、トラブルシューティングには便利ですが、セキュリティが明らかになります。

`Invoke-Command`コマンドレットは、 `Get-HotFix` コマンドをリモートで実行します。 **Sessionoption** パラメーターには、変数が指定されてい `$so` ます。

### 例 16: リモートコマンドで URI リダイレクトを管理する

この例では、 **Allowredirection** パラメーターと **sessionoption** パラメーターを使用して、リモートコマンドで URI リダイレクトを管理する方法を示します。

```powershell
$max = New-PSSessionOption -MaximumRedirection 1
$parameters = @{
  ConnectionUri = "https://ps.exchangelabs.com/PowerShell"
  ScriptBlock = { Get-Mailbox dan }
  AllowRedirection = $true
  SessionOption = $max
}
Invoke-Command @parameters
```

`New-PSSessionOption`コマンドレットでは、変数に保存される **Pssessionoption** オブジェクトを作成し `$max` ます。 このコマンドは、**MaximumRedirection** パラメーターを使用して、**PSSessionOption** オブジェクトの **MaximumConnectionRedirectionCount** プロパティを 1 に設定しています。

`Invoke-Command`コマンドレットは、 `Get-Mailbox` リモートの Microsoft Exchange サーバー上でコマンドを実行します。 **Allowredirection** パラメーターは、接続を代替エンドポイントにリダイレクトするための明示的なアクセス許可を提供します。 **Sessionoption** パラメーターは、変数に格納されているセッションオブジェクトを使用し `$max` ます。

その結果、 **Connectionuri** によって指定されたリモートコンピューターがリダイレクトメッセージを返す場合、PowerShell は接続をリダイレクトしますが、新しい送信先が別のリダイレクトメッセージを返す場合は、リダイレクトカウントの値1を超え、 `Invoke-Command` 終了しないエラーを返します。

### 例 17: リモートセッションでネットワーク共有にアクセスする

この例では、リモートセッションからネットワーク共有にアクセスする方法を示します。 例を示すために、3台のコンピューターが使用されています。 Server01 はローカルコンピューター、Server02 はリモートコンピューター、Net03 にはネットワーク共有が含まれています。 Server01 は Server02 に接続し、Server02 はネットワーク共有にアクセスするために Net03 に2番目のホップを行います。 PowerShell リモート処理でコンピューター間のホップがサポートされる方法の詳細については、「 [Powershell リモート処理で2番目のホップを作成](/powershell/scripting/learn/remoting/ps-remoting-second-hop)する」を参照してください。

必要な Credential Security Support Provider (CredSSP) 委任は、ローカルコンピューターのクライアント設定およびリモートコンピューターのサービス設定で有効になっています。 この例のコマンドを実行するには、ローカルコンピューターとリモートコンピューターの **Administrators** グループのメンバーである必要があります。

```powershell
Enable-WSManCredSSP -Role Client -DelegateComputer Server02
$s = New-PSSession Server02
Invoke-Command -Session $s -ScriptBlock {Enable-WSManCredSSP -Role Server -Force}
$parameters = @{
  Session = $s
  ScriptBlock = { Get-Item \\Net03\Scripts\LogFiles.ps1 }
  Authentication = "CredSSP"
  Credential = "Domain01\Admin01"
}
Invoke-Command @parameters
```

`Enable-WSManCredSSP`コマンドレットは、Server01 ローカルコンピューターから Server02 リモートコンピューターへの CredSSP 委任を有効にします。 **Role** パラメーターは、ローカルコンピューターの CredSSP クライアント設定を構成する **クライアント** を指定します。

`New-PSSession` Server02 の **PSSession** オブジェクトを作成し、そのオブジェクトを変数に格納し `$s` ます。

この `Invoke-Command` コマンドレットは、変数を使用して `$s` リモートコンピューター Server02 に接続します。 **ScriptBlock** パラメーターは、 `Enable-WSManCredSSP` リモートコンピューター上で実行されます。 **Role** パラメーターは、リモートコンピューターの CredSSP サーバー設定を構成する **サーバー** を指定します。

変数には、 `$parameters` ネットワーク共有に接続するためのパラメーター値が含まれています。 `Invoke-Command`コマンドレットは、 `Get-Item` のセッションでコマンドを実行し `$s` ます。 このコマンドは、ネットワーク共有からスクリプトを取得し `\\Net03\Scripts` ます。 このコマンドでは、 **Authentication** パラメーターの値が **CredSSP** で、 **Credential** パラメーターに値 **Domain01\Admin01** が使用されています。

### 例 18: 多くのリモートコンピューターでスクリプトを起動する

この例では、100台以上のコンピューターでスクリプトを実行します。 ローカル コンピューターへの影響を最小限に抑えるために、各コンピューターに接続してスクリプトを起動した後で、各コンピューターとの接続を切断します。
スクリプトは、引き続き切断されたセッションで実行されます。

```powershell
$parameters = @{
  ComputerName = (Get-Content -Path C:\Test\Servers.txt)
  InDisconnectedSession = $true
  FilePath = "\\Scripts\Public\ConfigInventory.ps1"
  SessionOption = @{OutputBufferingMode="Drop";IdleTimeout=43200000}
}
Invoke-Command @parameters
```

このコマンドは、を使用し `Invoke-Command` てスクリプトを実行します。 **ComputerName** パラメーターの値は、 `Get-Content` テキストファイルからリモートコンピューターの名前を取得するコマンドです。 **InDisconnectedSession** パラメーターにより、コマンドを起動するとすぐにセッションが切断されます。 **FilePath** パラメーターの値は、 `Invoke-Command` 各コンピューターで実行されるスクリプトです。

**Sessionoption** の値はハッシュテーブルです。 **OutputBufferingMode** 値が **Drop** に設定され、 **IdleTimeout** 値が **4320万** ミリ秒 (12 時間) に設定されています。

切断されたセッションで実行されるコマンドとスクリプトの結果を取得するには、コマンドレットを使用し `Receive-PSSession` ます。

### 例 19: SSH を使用してリモートコンピューターでコマンドを実行する

この例では、Secure Shell (SSH) を使用してリモートコンピューターでコマンドを実行する方法を示します。 SSH がリモートコンピューターでパスワードを要求するように構成されている場合は、パスワードの入力を求めるプロンプトが表示されます。
それ以外の場合は、SSH キーベースのユーザー認証を使用する必要があります。

```powershell
Invoke-Command -HostName UserA@LinuxServer01 -ScriptBlock { Get-MailBox * }
```

### 例 20: SSH を使用してリモートコンピューターでコマンドを実行し、ユーザー認証キーを指定する

この例では、SSH を使用してリモートコンピューターでコマンドを実行し、ユーザー認証用のキーファイルを指定する方法を示します。 キー認証が失敗し、リモートコンピューターが基本パスワード認証を許可するように構成されている場合を除き、パスワードの入力を求められることはありません。

```powershell
Invoke-Command -HostName UserA@LinuxServer01 -ScriptBlock { Get-MailBox * } -KeyFilePath /UserA/UserAKey_rsa
```

### 例 21: ジョブとして SSH を使用して複数のリモートコンピューターでスクリプトファイルを実行する

この例では、SSH と **SSHConnection** パラメーターセットを使用して、複数のリモートコンピューターでスクリプトファイルを実行する方法を示します。 **SSHConnection** パラメーターは、各コンピューターの接続情報を含むハッシュテーブルの配列を受け取ります。 この例では、ターゲットのリモートコンピューターで、キーベースのユーザー認証をサポートするように SSH が構成されている必要があります。

```powershell
$sshConnections =
@{ HostName="WinServer1"; UserName="Domain\UserA"; KeyFilePath="C:\Users\UserA\id_rsa" },
@{ HostName="UserB@LinuxServer5"; KeyFilePath="/Users/UserB/id_rsa" }
$results = Invoke-Command -FilePath c:\Scripts\CollectEvents.ps1 -SSHConnection $sshConnections
```

## PARAMETERS

### -AllowRedirection

この接続を代替 Uniform Resource Identifier (URI) にリダイレクトできます。

**ConnectionURI** パラメーターを使用すると、リモートの送信先は別の URI にリダイレクトするように指示を返すことができます。 既定では、PowerShell は接続をリダイレクトしませんが、このパラメーターを使用して接続をリダイレクトすることができます。

また、**MaximumConnectionRedirectionCount** セッション オプション値を変更することで、接続をリダイレクトする回数を制限することもできます。 コマンドレットの **Maximumredirection** パラメーターを使用する `New-PSSessionOption` か、Preference 変数の **MaximumConnectionRedirectionCount** プロパティを設定し `$PSSessionOption` ます。 既定値は 5 です。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Uri, FilePathUri
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ApplicationName

接続 URI のアプリケーション名セグメントを指定します。 コマンドで **Connectionuri** パラメーターを使用しない場合は、このパラメーターを使用してアプリケーション名を指定します。

既定値は、 `$PSSessionApplicationName` ローカルコンピューター上のユーザー設定変数の値です。 このユーザー設定変数が定義されていない場合、既定値は WSMAN です。 この値はほとんどの用途に適しています。 詳細については、「 [about_Preference_Variables](./About/about_Preference_Variables.md)」を参照してください。

WinRM サービスは、アプリケーション名を使用して、接続要求を処理するリスナーを選択します。
このパラメーターの値は、リモート コンピューター上のリスナーの **URLPrefix** プロパティの値に一致している必要があります。

```yaml
Type: System.String
Parameter Sets: ComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: $PSSessionApplicationName if set on the local computer, otherwise WSMAN
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ArgumentList

コマンド内のローカル変数の値を指定します。 コマンド内のローカル変数は、リモート コンピューターでコマンドが実行される前に、これらの値に置き換えられます。 コンマ区切りのリストで、値を入力します。 値は、一覧表示されている順序で変数に関連付けられます。 **ArgumentList** のエイリアスは Args です。

**ArgumentList** パラメーターの値には、1024などの実際の値を指定することも、ローカル変数 (など) への参照を指定することもでき `$max` ます。

コマンド内でローカル変数を使用するには、次のコマンド形式を使用します。

`{param($<name1>[, $<name2>]...) <command-with-local-variables>} -ArgumentList <value>` または `<local-variable>`

**Param** キーワードは、コマンドで使用されるローカル変数を一覧表示します。 **ArgumentList** は、変数の値を一覧の順序で指定します。 **ArgumentList** の動作の詳細については、「 [about_Splatting](about/about_Splatting.md#splatting-with-arrays)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsJob

このコマンドレットがリモートコンピューター上でコマンドをバックグラウンドジョブとして実行することを示します。 このパラメーターを使用して、完了までに時間のかかるコマンドを実行します。

**AsJob** パラメーターを使用すると、コマンドによってジョブを表すオブジェクトが返され、コマンドプロンプトが表示されます。 ジョブが完了しても、引き続きセッションで作業できます。 ジョブを管理するには、 `*-Job` コマンドレットを使用します。 ジョブの結果を取得するには、`Receive-Job` コマンドレットを使用します。

**AsJob** パラメーターはコマンドレットを使用して、 `Invoke-Command` `Start-Job` コマンドレットをリモートで実行します。 ただし、 **AsJob** を使用すると、ジョブがリモートコンピューター上で実行されている場合でも、ジョブはローカルコンピューター上に作成されます。 リモートジョブの結果は、自動的にローカルコンピューターに返されます。

PowerShell バックグラウンドジョブの詳細については、「 [about_Jobs](About/about_Jobs.md) 」および「 [about_Remote_Jobs](About/about_Remote_Jobs.md)」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: FilePathRunspace, Session, ComputerName, FilePathComputerName, Uri, FilePathUri, VMId, VMName, FilePathVMId, FilePathVMName, SSHHost, ContainerId, FilePathContainerId, SSHHostHashParam, FilePathSSHHost, FilePathSSHHostHash
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -認証

ユーザーの資格情報の認証に使用するメカニズムを指定します。 CredSSP 認証は、windows Vista、Windows Server 2008、およびそれ以降のバージョンの Windows オペレーティングシステムでのみ使用できます。

このパラメーターに指定できる値は次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential

既定値は Default です。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!CAUTION]
> ユーザーの資格情報が認証対象のリモート コンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモート ネットワーク共有にアクセスする場合など、複数のリソースの認証を必要とするコマンドを対象としています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。 詳細については、「 [Credential Security Support Provider](/windows/win32/secauthn/credential-security-support-provider)」を参照してください。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: ComputerName, FilePathComputerName, Uri, FilePathUri
Aliases:
Accepted values: Basic, Default, Credssp, Digest, Kerberos, Negotiate, NegotiateWithImplicitCredential

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

切断されたセッションに接続するためのアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 ローカルユーザーアカウントにしかマップできず、ドメインアカウントでは使用できません。

証明書の拇印を取得するに `Get-Item` は、 `Get-ChildItem` PowerShell Cert: ドライブでまたはコマンドを使用します。

```yaml
Type: System.String
Parameter Sets: ComputerName, Uri
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

コマンドを実行するコンピューターを指定します。 既定値はローカル コンピューターです。

**ComputerName** パラメーターを使用すると、PowerShell は、指定されたコマンドを実行するためだけに使用され、その後閉じられる一時的な接続を作成します。 永続的な接続が必要な場合は、 **Session** パラメーターを使用します。

コンマ区切りのリストで、1 台または複数のコンピューターの NETBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。 ローカルコンピューターを指定するには、コンピューター名、localhost、またはドット () を入力し `.` ます。

**ComputerName** の値に IP アドレスを使用するには、コマンドに **Credential** パラメーターを含める必要があります。 コンピューターが HTTPS トランスポート用に構成されているか、リモートコンピューターの IP アドレスがローカルコンピューターの WinRM **TrustedHosts** 一覧に含まれている必要があります。 **TrustedHosts** の一覧にコンピューター名を追加する手順については、「[コンピューターを信頼されたホストの一覧に追加する方法](./about/about_remote_troubleshooting.md#how-to-add-a-computer-to-the-trusted-hosts-list)」を参照してください。

Windows Vista 以降のバージョンの Windows オペレーティングシステムでは、ローカルコンピューターを **ComputerName** の値に含めるには、[ **管理者として実行** ] オプションを使用して PowerShell を実行する必要があります。

```yaml
Type: System.String[]
Parameter Sets: ComputerName, FilePathComputerName
Aliases: Cn

Required: False
Position: 0
Default value: Local computer
Accept pipeline input: False
Accept wildcard characters: False
```

### -ConfigurationName

新しい **PSSession** に使用するセッション構成を指定します。

セッション構成の構成名または完全修飾リソース URI を入力します。 構成名だけを指定した場合は、次のスキーマ URI が付加され `http://schemas.microsoft.com/PowerShell` ます。

SSH と共に使用する場合、このパラメーターは、「」で定義されているターゲットで使用するサブシステムを指定し `sshd_config` ます。 SSH の既定値は `powershell` サブシステムです。

セッションのセッション構成は、リモート コンピューター上にあります。 指定したセッション構成がリモートコンピューターに存在しない場合、コマンドは失敗します。

既定値は、 `$PSSessionConfigurationName` ローカルコンピューター上のユーザー設定変数の値です。 このユーザー設定変数が設定されていない場合、既定値は [ **Microsoft. PowerShell**] です。 詳細については、「 [about_Preference_Variables](about/about_Preference_Variables.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: ComputerName, FilePathComputerName, Uri, FilePathUri, VMId, VMName, FilePathVMId, FilePathVMName, ContainerId, FilePathContainerId
Aliases:

Required: False
Position: Named
Default value: $PSSessionConfigurationName if set on the local computer, otherwise Microsoft.PowerShell
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ConnectionUri

セッションの接続エンドポイントを定義する Uniform Resource Identifier (URI) を指定します。
URI は完全修飾名にする必要があります。

この文字列の形式は次のとおりです。

`<Transport>://<ComputerName>:<Port>/<ApplicationName>`

既定値は、次のとおりです。

`http://localhost:5985/WSMAN`

接続 URI を指定しない場合は、 **UseSSL** パラメーターと **Port** パラメーターを使用して接続 uri の値を指定できます。

URI の **Transport** セグメントの有効な値は、HTTP および HTTPS です。 トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合、セッションは標準ポート80で HTTP と443が HTTPS 用に作成されます。 PowerShell リモート処理に既定のポートを使用するには、HTTP の場合はポート5985、HTTPS の場合は5986を指定します。

対象のコンピューターが接続を別の URI にリダイレクトする場合、コマンドで **Allowredirection** パラメーターを使用しない限り、PowerShell によってリダイレクトが禁止されます。

```yaml
Type: System.Uri[]
Parameter Sets: Uri, FilePathUri
Aliases: URI, CU

Required: False
Position: 0
Default value: http://localhost:5985/WSMAN
Accept pipeline input: False
Accept wildcard characters: False
```

### -ContainerId

コンテナー Id の配列を指定します。

```yaml
Type: System.String[]
Parameter Sets: ContainerId, FilePathContainerId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ComputerName, FilePathComputerName, Uri, FilePathUri, VMId, VMName, FilePathVMId, FilePathVMName
Aliases:

Required: True (VMId, VMName, FilePathVMId, FilePathVMName), False (ComputerName, FilePathComputerName, Uri, FilePathUri)
Position: Named
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -EnableNetworkAccess

このコマンドレットが、ループバックセッションに対話型セキュリティトークンを追加することを示します。 対話型トークンを使用すると、他のコンピューターからデータを取得するコマンドをループバック セッションで実行できます。 たとえば、リモート コンピューターからローカル コンピューターに XML ファイルをコピーするコマンドをセッションで実行できます。

ループバックセッションは、同じコンピューター上で発生して終了する **PSSession** です。 ループバックセッションを作成するには、 **ComputerName** パラメーターを省略するか、値をドット ( `.` )、localhost、またはローカルコンピューターの名前に設定します。

既定では、ループバックセッションはネットワークトークンを使用して作成されますが、リモートコンピューターに対して認証を行うための十分なアクセス許可がない可能性があります。

**EnableNetworkAccess** パラメーターは、ループバック セッションでのみ有効です。 リモートコンピューターでセッションを作成するときに **EnableNetworkAccess** を使用すると、コマンドは成功しますが、パラメーターは無視されます。

**認証** パラメーターの **CredSSP** 値を使用して、ループバックセッションでリモートアクセスを許可できます。これにより、セッションの資格情報が他のコンピューターに委任されます。

悪意のあるアクセスからコンピューターを保護するために、 **EnableNetworkAccess** を使用して作成された対話型トークンを持つ切断されたループバックセッションは、セッションが作成されたコンピューターからのみ再接続することができます。 CredSSP 認証を使用するセッションが切断された場合には、他のコンピューターから再接続することができます。 詳細については、「`Disconnect-PSSession`」を参照してください。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName, FilePathComputerName, Uri, FilePathUri
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

このコマンドレットが1つ以上のリモートコンピューターで実行するローカルスクリプトを指定します。 スクリプトのパスとファイル名を入力するか、パイプを介してへのスクリプトパスを指定し `Invoke-Command` ます。 スクリプトは、ローカルコンピューターまたはローカルコンピューターがアクセスできるディレクトリに存在する必要があります。 **ArgumentList** を使用して、スクリプト内のパラメーターの値を指定します。

このパラメーターを使用すると、PowerShell は指定されたスクリプトファイルの内容をスクリプトブロックに変換し、スクリプトブロックをリモートコンピューターに送信して、リモートコンピューター上で実行します。

```yaml
Type: System.String
Parameter Sets: FilePathRunspace, FilePathComputerName, FilePathUri, FilePathVMId, FilePathVMName, FilePathContainerId, FilePathSSHHost, FilePathSSHHostHash
Aliases: PSPath

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -HideComputerName

このコマンドレットは、出力表示から各オブジェクトのコンピューター名を省略することを示します。 既定では、オブジェクトを生成したコンピューター名が画面に表示されます。

このパラメーターが有効なのは、出力の表示だけです。 オブジェクトは変更されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: FilePathRunspace, Session, ComputerName, FilePathComputerName, Uri, FilePathUri, VMId, VMName, FilePathVMId, FilePathVMName, SSHHost, ContainerId, FilePathContainerId, SSHHostHashParam, FilePathSSHHost, FilePathSSHHostHash
Aliases: HCN

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -HostName

Secure Shell (SSH) ベースの接続に使用するコンピューター名の配列を指定します。 これは **ComputerName** パラメーターと似ていますが、リモートコンピューターへの接続は Windows WinRM ではなく SSH を使用して行われる点が異なります。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: SSHHost, FilePathSSHHost
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InDisconnectedSession

このコマンドレットが、切断されたセッションでコマンドまたはスクリプトを実行することを示します。

**Indisconnectedsession** パラメーターを使用すると、は `Invoke-Command` 各リモートコンピューター上に永続的なセッションを作成し、 **ScriptBlock** または **FilePath** パラメーターで指定されたコマンドを開始してから、セッションから切断します。 これらのコマンドは、切断されたセッションで引き続き実行されます。 **Indisconnectedsession** を使用すると、リモートセッションへの接続を維持せずにコマンドを実行できます。 また、セッションは、結果が返される前に切断されるため、 **Indisconnectedsession** は、セッション間で分割されるのではなく、すべてのコマンドの結果が再接続されたセッションに返されるようにします。

**セッション** パラメーターまたは **AsJob** パラメーターと共に **indisconnectedsession** を使用することはできません。

**Indisconnectedsession** を使用するコマンドは、切断されたセッションを表す **PSSession** オブジェクトを返します。 コマンドの出力は返されません。 切断されたセッションに接続するに `Connect-PSSession` は、コマンドレットまたはコマンドレットを使用し `Receive-PSSession` ます。 セッションで実行されたコマンドの結果を取得するには、コマンドレットを使用し `Receive-PSSession` ます。 切断されたセッションで出力を生成するコマンドを実行するには、 **OutputBufferingMode** session オプションの値を **Drop** に設定します。 切断されたセッションに接続する場合は、セッションのアイドルタイムアウトを設定して、セッションを削除する前に接続するのに十分な時間を確保できるようにします。

**Sessionoption** パラメーターまたはユーザー設定変数で、出力バッファーモードとアイドルタイムアウトを設定でき `$PSSessionOption` ます。 セッションオプションの詳細については、「」および about_Preference_Variables を参照してください `New-PSSessionOption` 。 [](./about/about_preference_variables.md)

切断されたセッションの機能の詳細については、「[about_Remote_Disconnected_Sessions](about/about_Remote_Disconnected_Sessions.md)」を参照してください。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName, FilePathComputerName, Uri, FilePathUri
Aliases: Disconnected

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

コマンドへの入力を指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

**InputObject** パラメーターを使用する場合は、 `$Input` **ScriptBlock** パラメーターの値で自動変数を使用して入力オブジェクトを表します。

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

### -JobName

バックグラウンド ジョブのフレンドリ名を指定します。 既定では、ジョブにはという名前が付けられ `Job<n>` ます。ここで、 `<n>` は序数です。

コマンドで **JobName** パラメーターを使用すると、コマンドはジョブとして実行され、 `Invoke-Command` コマンドに **AsJob** が含まれていない場合でも、ジョブオブジェクトが返されます。

PowerShell バックグラウンドジョブの詳細については、「 [about_Jobs](./About/about_Jobs.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: FilePathRunspace, Session, ComputerName, FilePathComputerName, Uri, FilePathUri, SSHHost, ContainerId, FilePathContainerId, SSHHostHashParam
Aliases:

Required: False
Position: Named
Default value: Job<n>
Accept pipeline input: False
Accept wildcard characters: False
```

### -KeyFilePath

リモートコンピューター上のユーザーを認証するために Secure Shell (SSH) によって使用されるキーファイルパスを指定します。

SSH では、基本パスワード認証の代わりに、プライベートキーと公開キーを使用してユーザー認証を実行できます。 リモートコンピューターがキー認証用に構成されている場合は、このパラメーターを使用して、ユーザーを識別するキーを指定できます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: SSHHost, FilePathSSHHost
Aliases: IdentityFilePath

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -None Wscope

このコマンドレットが、指定されたコマンドを現在のスコープ内で実行することを示します。 既定では、は `Invoke-Command` 独自のスコープでコマンドを実行します。

このパラメーターは、現在のセッションで実行されるコマンド、つまり、**ComputerName** および **Session** の両パラメーターを除外したコマンド内でのみ有効です。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: InProcess
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Port

このコマンドに使用するリモートコンピューターのネットワークポートを指定します。 リモート コンピューターに接続するには、リモート コンピューターで、接続に使用されるポートをリッスンすることが必要です。 既定のポートは 5985 (HTTP の場合は WinRM ポート)、5986 (HTTPS の場合は WinRM ポート) です。

別のポートを使用する場合には、そのポートでリッスンするようにリモート コンピューターの WinRM リスナーを構成します。 リスナーを構成するには、PowerShell プロンプトで次の2つのコマンドを入力します。

`Remove-Item -Path WSMan:\Localhost\listener\listener* -Recurse`

`New-Item -Path WSMan:\Localhost\listener -Transport http -Address * -Port \<port-number\>`

必要な場合を除き、 **Port** パラメーターは使用しないでください。 コマンドに設定されているポートは、コマンドが実行されるすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。

```yaml
Type: System.Int32
Parameter Sets: ComputerName, FilePathComputerName, SSHHost
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RemoteDebug

リモート PowerShell セッションで、呼び出されたコマンドをデバッグモードで実行するために使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: FilePathRunspace, Session, ComputerName, FilePathComputerName, Uri, FilePathUri, VMId, VMName, FilePathVMId, FilePathVMName, SSHHost, ContainerId, FilePathContainerId, SSHHostHashParam, FilePathSSHHost, FilePathSSHHostHash
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunAsAdministrator

このコマンドレットが管理者としてコマンドを呼び出すことを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ContainerId, FilePathContainerId
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptBlock

実行するコマンドを指定します。 コマンドを中かっこで囲み、 `{ }` スクリプトブロックを作成します。
このパラメーターは必須です。

既定では、コマンド内のどの変数もリモート コンピューター上で評価されます。 コマンドにローカル変数を含めるには、 **ArgumentList** を使用します。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: InProcess, Session, ComputerName, Uri, VMId, VMName, SSHHost, ContainerId, SSHHostHashParam
Aliases: Command

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -セッション

このコマンドレットがコマンドを実行するセッションの配列を指定します。 **Pssession** オブジェクトを含む変数、  `New-PSSession` またはコマンドやコマンドなどの pssession オブジェクトを作成または取得するコマンドを入力し `Get-PSSession` ます。

**PSSession** を作成すると、PowerShell によって、リモートコンピューターへの永続的な接続が確立されます。 **PSSession** を使用して、データを共有する一連の関連コマンドを実行します。 1つのコマンドまたは一連の関連のないコマンドを実行するには、 **ComputerName** パラメーターを使用します。 詳細については、「 [about_PSSessions](./About/about_PSSessions.md)」を参照してください。

```yaml
Type: System.Management.Automation.Runspaces.PSSession[]
Parameter Sets: FilePathRunspace, Session
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -セッション名

切断されたセッションのフレンドリ名を指定します。 この名前を使用して、コマンドなどの後続のコマンドでセッションを参照することができ `Get-PSSession` ます。 このパラメーターは、**InDisconnectedSession** パラメーターと共に使用する場合に限り有効です。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: ComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SessionOption

セッションの詳細オプションを指定します。 コマンドレットを使用して作成する **sessionoption** オブジェクト、 `New-PSSessionOption` またはキーがセッションオプションの名前で、値がセッションオプションの値であるハッシュテーブルを入力します。

オプションの既定値は、設定されている場合は、ユーザー設定変数の値によって決まり `$PSSessionOption` ます。 それ以外の場合、既定値はセッション構成で設定されたオプションによって決まります。

セッションオプションの値は、ユーザー設定変数およびセッション構成で設定されたセッションの既定値よりも優先され `$PSSessionOption` ます。 ただし、セッション構成で設定された最大値、クォータ、または制限よりも優先されるわけではありません。

既定値を含むセッションオプションの説明については、「」を参照してください `New-PSSessionOption` 。 ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。
セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

```yaml
Type: System.Management.Automation.Remoting.PSSessionOption
Parameter Sets: ComputerName, FilePathComputerName, Uri, FilePathUri
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SSHConnection

このパラメーターは、ハッシュテーブルの配列を受け取ります。各ハッシュテーブルには、Secure Shell (SSH) 接続を確立するために必要な接続パラメーターが1つ以上含まれています。 **SSHConnection** パラメーターは、各セッションが異なる接続情報を必要とする複数のセッションを作成する場合に便利です。

ハッシュテーブルには、次のメンバーがあります。

- **ComputerName** (または **ホスト名**)
- **[ポート]**
- **UserName**
- **Keyfilepath** **(または**、または、またはの場合)

必要なキーと値のペアは、 **ComputerName** (または **HostName**) だけです。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Collections.Hashtable[]
Parameter Sets: SSHHostHashParam, FilePathSSHHostHash
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SSHTransport

リモート接続が Secure Shell (SSH) を使用して確立されることを示します。

既定では、PowerShell は Windows WinRM を使用してリモートコンピューターに接続します。 このスイッチは、SSH ベースのリモート接続を確立するために **HostName** パラメーターを使用するように PowerShell に強制します。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: SSHHost, FilePathSSHHost
Aliases:
Accepted values: true

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ThrottleLimit

このコマンドを実行するために確立できる最大コンカレント接続数を指定します。
このパラメーターを省略した場合、または値 0 を入力した場合は、既定値の 32 が使用されます。

スロットル制限は現在のコマンドのみに適用され、セッションまたはコンピューターには適用されません。

```yaml
Type: System.Int32
Parameter Sets: FilePathRunspace, Session, ComputerName, FilePathComputerName, Uri, FilePathUri, VMId, VMName, FilePathVMId, FilePathVMName, ContainerId, FilePathContainerId
Aliases:

Required: False
Position: Named
Default value: 32
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserName

リモートコンピューターでコマンドを実行するために使用するアカウントのユーザー名を指定します。 ユーザー認証方法は、リモートコンピューターで Secure Shell (SSH) がどのように構成されているかによって異なります。

SSH が基本パスワード認証用に構成されている場合は、ユーザーのパスワードの入力を求められます。

SSH がキーベースのユーザー認証用に構成されている場合、 **Keyfilepath** パラメーターを使用してキーファイルのパスを指定することができ、パスワードのプロンプトは表示されません。 クライアントユーザーキーファイルが SSH の既知の場所にある場合、キーベースの認証に **Keyfilepath** パラメーターは必要ありません。ユーザー認証はユーザー名に基づいて自動的に行われます。 詳細については、キーベースのユーザー認証に関するプラットフォームの SSH ドキュメントを参照してください。

これは必須のパラメーターではありません。 **UserName** パラメーターが指定されていない場合、現在ログオンしているユーザー名が接続に使用されます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: SSHHost, FilePathSSHHost
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSSL

このコマンドレットが、リモートコンピューターへの接続を確立するために Secure Sockets Layer (SSL) プロトコルを使用することを示します。 既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で送信されるすべての PowerShell コンテンツを暗号化します。 **UseSSL** パラメーターは、HTTP ではなく HTTPS 経由でデータを送信する追加の保護です。

このパラメーターを使用しても、コマンドに使用されているポートで SSL が使用できない場合、コマンドは失敗します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -VMId

仮想マシンの Id の配列を指定します。

```yaml
Type: System.Guid[]
Parameter Sets: VMId, FilePathVMId
Aliases: VMGuid

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -VMName

仮想マシンの名前の配列を指定します。

```yaml
Type: System.String[]
Parameter Sets: VMName, FilePathVMName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -サブシステム

新しい **PSSession** に使用する SSH サブシステムを指定します。

これは sshd_config で定義されているターゲットで使用するサブシステムを指定します。
サブシステムは、定義済みのパラメーターを使用して特定のバージョンの PowerShell を起動します。
指定したサブシステムがリモートコンピューターに存在しない場合、コマンドは失敗します。

このパラメーターを使用しない場合、既定値は ' powershell ' サブシステムになります。

```yaml
Type: System.String
Parameter Sets: SSHHost
Aliases:

Required: False
Position: Named
Default value: powershell
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システムの管理. ScriptBlock

パイプを使用して、スクリプトブロック内のコマンドをにパイプすることができ `Invoke-Command` ます。 `$Input`コマンド内の入力オブジェクトを表すには、自動変数を使用します。

## 出力

### システムの管理. PSRemotingJob、System...、または呼び出されたコマンドの出力です。

**AsJob** パラメーターを使用した場合、このコマンドレットはジョブオブジェクトを返します。 **Indisconnectedsession** パラメーターを指定すると、は `Invoke-Command` **PSSession** オブジェクトを返します。 それ以外の場合は、呼び出されたコマンドの出力を返します。これは **ScriptBlock** パラメーターの値です。

## 注

Windows Vista 以降のバージョンの Windows オペレーティングシステムでは、の **ComputerName** パラメーターを使用して `Invoke-Command` ローカルコンピューターでコマンドを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を実行する必要があります。

複数のコンピューターでコマンドを実行すると、PowerShell は、一覧に表示されている順序でコンピューターに接続します。 ただし、コマンドの出力は、リモートコンピューターから受信した順序で表示されますが、異なる場合もあります。

実行するコマンドによって発生したエラー `Invoke-Command` は、コマンドの結果に含まれています。
ローカル コマンドでは "終了するエラー" となるエラーは、リモート コマンドでは "終了しないエラー" として扱われます。 この方法を使用すると、1台のコンピューターでエラーが発生しても、実行されているすべてのコンピューターでコマンドが終了しないようにすることができます。 また、この方法は、1 台のコンピューターでリモート コマンドを実行する場合にも、使用されます。

リモートコンピューターがローカルコンピューターによって信頼されているドメインにない場合、そのコンピューターはユーザーの資格情報を認証できない可能性があります。 WS-MANAGEMENT の信頼されたホストの一覧にリモートコンピューターを追加するには、プロバイダーで次のコマンドを使用し `WSMAN` ます。ここで、 `<Remote-Computer-Name>` はリモートコンピューターの名前です。

`Set-Item -Path WSMan:\Localhost\Client\TrustedHosts -Value \<Remote-Computer-Name\>`

**Indisconnectedsession** パラメーターを使用して **PSSession** を切断すると、セッション状態は **Disconnected** になり、可用性は **None** になります。 **State** プロパティの値は、現在のセッションに関連付けられています。 値が **Disconnected** の場合は、 **PSSession** が現在のセッションに接続されていないことを示します。 ただし、 **PSSession** がすべてのセッションから切断されているわけではありません。 別のセッションに接続されている可能性があるためです。 セッションに接続または再接続できるかどうかを確認するには、**Availability** プロパティを使用します。

**Availability** の値が **None** の場合は、セッションに接続できることを示します。 値が **Busy** の場合は、PSSession が別のセッションに接続されているため、 **PSSession** に接続できないことを示します。 セッションの **State** プロパティの値の詳細については、「 [RunspaceState](/dotnet/api/system.management.automation.runspaces.runspacestate)」を参照してください。 セッションの **Availability** プロパティの値の詳細については、「 [RunspaceAvailability](/dotnet/api/system.management.automation.runspaces.runspaceavailability)」を参照してください。

**HostName** および **SSHConnection** パラメーターは、PowerShell 6.0 以降に含まれていました。 これらは、Secure Shell (SSH) に基づいて PowerShell リモート処理を提供するために追加されました。 PowerShell と SSH は複数のプラットフォーム (Windows、Linux、macOS) でサポートされており、powershell リモート処理は、PowerShell と SSH がインストールおよび構成されているこれらのプラットフォーム上で動作します。 これは、WinRM に基づく以前の Windows のみのリモート処理とは別のものであり、WinRM 固有の機能の多くは適用されません。 たとえば、WinRM ベースのクォータ、セッションオプション、カスタムエンドポイントの構成、および切断/再接続機能は現在サポートされていません。 PowerShell SSH リモート処理の設定方法の詳細については、「 [Ssh 経由の Powershell リモート](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)処理」を参照してください。

## 関連リンク

[about_PSSessions](./About/about_PSSessions.md)

[about_Remote](./About/about_Remote.md)

[about_Remote_Disconnected_Sessions](./About/about_Remote_Disconnected_Sessions.md)

[about_Remote_Troubleshooting](./About/about_remote_troubleshooting.md)

[about_Remote_Variables](./About/about_Remote_Variables.md)

[about_Scopes](./About/about_scopes.md)

[Enter-PSSession](Enter-PSSession.md)

[Exit-PSSession](Exit-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Item](../Microsoft.PowerShell.Management/Invoke-Item.md)

[New-PSSession](New-PSSession.md)

[Remove-PSSession](Remove-PSSession.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

