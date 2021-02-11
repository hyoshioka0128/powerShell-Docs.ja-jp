---
title: PowerShell リモート処理
description: PowerShell でリモート コンピューターにコマンドを実行する方法は多数あります。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: 0e467a41282b261c56a89578dbc841725f59a6e0
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99600779"
---
# <a name="chapter-8---powershell-remoting"></a>第 8 章 - PowerShell リモート処理

PowerShell では、さまざまな方法でリモート コンピューターにコマンドを実行できます。 前の章では、CIM コマンドレットを使用して WMI に対してリモートでクエリを実行する方法を説明しました。 PowerShell には、**ComputerName** パラメーターが組み込まれたコマンドレットも複数含まれています。

次の例に示すように、`Get-Command` と共に **ParameterName** パラメーターを使用すると、どのコマンドに **ComputerName** パラメーターがあるかを確認できます。

```powershell
Get-Command -ParameterName ComputerName
```

```Output
CommandType Name                        Version Source
----------- ----                        ------- ------
Cmdlet      Connect-PSSession           7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Connect-WSMan               7.0.0.0 Microsoft.WSMan.Management
Cmdlet      Disconnect-WSMan            7.0.0.0 Microsoft.WSMan.Management
Cmdlet      Enter-PSSession             7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Get-CimAssociatedInstance   7.0.0.0 CimCmdlets
Cmdlet      Get-CimClass                7.0.0.0 CimCmdlets
Cmdlet      Get-CimInstance             7.0.0.0 CimCmdlets
Cmdlet      Get-CimSession              7.0.0.0 CimCmdlets
Cmdlet      Get-Counter                 7.0.0.0 Microsoft.PowerShell.Diagnostics
Cmdlet      Get-HotFix                  7.0.0.0 Microsoft.PowerShell.Management
Cmdlet      Get-PSSession               7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Get-WinEvent                7.0.0.0 Microsoft.PowerShell.Diagnostics
Cmdlet      Get-WSManInstance           7.0.0.0 Microsoft.WSMan.Management
Cmdlet      Invoke-CimMethod            7.0.0.0 CimCmdlets
Cmdlet      Invoke-Command              7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Invoke-WSManAction          7.0.0.0 Microsoft.WSMan.Management
Cmdlet      New-CimInstance             7.0.0.0 CimCmdlets
Cmdlet      New-CimSession              7.0.0.0 CimCmdlets
Cmdlet      New-PSSession               7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      New-WSManInstance           7.0.0.0 Microsoft.WSMan.Management
Cmdlet      Receive-Job                 7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Receive-PSSession           7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Register-CimIndicationEvent 7.0.0.0 CimCmdlets
Cmdlet      Remove-CimInstance          7.0.0.0 CimCmdlets
Cmdlet      Remove-CimSession           7.0.0.0 CimCmdlets
Cmdlet      Remove-PSSession            7.0.1.0 Microsoft.PowerShell.Core
Cmdlet      Remove-WSManInstance        7.0.0.0 Microsoft.WSMan.Management
Cmdlet      Rename-Computer             7.0.0.0 Microsoft.PowerShell.Management
Cmdlet      Restart-Computer            7.0.0.0 Microsoft.PowerShell.Management
Cmdlet      Send-MailMessage            7.0.0.0 Microsoft.PowerShell.Utility
Cmdlet      Set-CimInstance             7.0.0.0 CimCmdlets
Cmdlet      Set-WSManInstance           7.0.0.0 Microsoft.WSMan.Management
Cmdlet      Stop-Computer               7.0.0.0 Microsoft.PowerShell.Management
Cmdlet      Test-Connection             7.0.0.0 Microsoft.PowerShell.Management
Cmdlet      Test-WSMan                  7.0.0.0 Microsoft.WSMan.Management
```

`Get-Process`、`Get-Hotfix` などのコマンドには、**ComputerName** パラメーターがあります。 これは、リモート コンピューターへのコマンドの実行に関して、Microsoft が長期的に目指す方向性ではありません。 **ComputerName** パラメーターを持つコマンドが見つかったとしても、おそらく代替の資格情報を指定する必要が出てきます。**Credential** パラメーターが追加されることはないでしょう。 また、管理者特権のアカウントから PowerShell を実行しようとすると、リモート コンピューターとの間のファイアウォールによって、その要求がブロックされる可能性があります。

この章で説明されている PowerShell リモート処理コマンドを使用するには、リモート コンピューターで PowerShell リモート処理を有効にする必要があります。 `Enable-PSRemoting` コマンドレットを使用すると、PowerShell リモート処理を有効にできます。

```powershell
Enable-PSRemoting
```

```Output
WinRM has been updated to receive requests.
WinRM service type changed successfully.
WinRM service started.

WinRM has been updated for remote management.
WinRM firewall exception enabled.
```

## <a name="one-to-one-remoting"></a>一対一のリモート処理

リモート セッションを対話型にするには、一対一のリモート処理が必要です。
この種類のリモート処理は、`Enter-PSSession` コマンドレットによって実行できます。

前の章で、ドメイン管理者の資格情報を `$Cred` という名前の変数に格納しました。 まだドメイン管理者の資格情報を `$Cred` 変数に格納していない場合は、この作業を行ってください。

これにより資格情報を一度に入力できるため、現在の PowerShell セッションがアクティブであれば、コマンドごとにその資格情報を使用できます。

```powershell
$Cred = Get-Credential
```

dc01 という名前のドメイン コントローラーとの一対一の PowerShell リモート処理セッションを作成します。

```powershell
Enter-PSSession -ComputerName dc01 -Credential $Cred
```

```Output
[dc01]: PS C:\Users\Administrator\Documents>
```

この例では PowerShell プロンプトの前に `[dc01]` があることに注意してください。 これは、dc01 という名前のリモート コンピューターと対話型 PowerShell セッション中であることを意味します。 実行するすべてのコマンドが、ご自身のローカル コンピューターではなく、dc01 上で実行されます。 また、リモート コンピューター上の PowerShell コマンドにしかアクセスできないことにも注意してください。ローカル コンピューター上の PowerShell コマンドにはアクセスできません。 つまり、ご自身のコンピューターに追加モジュールをインストールしてある場合、そのモジュールには、リモート コンピューターではアクセスできません。

一対一の対話型 PowerShell リモート処理セッションを使用してリモート コンピューターに接続している場合は、事実上、リモート コンピューターの前にいるのと同じです。 オブジェクトは、本書を通して使用してきたような通常のオブジェクトです。

```powershell
[dc01]:  Get-Process | Get-Member
```

```Output
   TypeName: System.Diagnostics.Process

Name                       MemberType     Definition
----                       ----------     ----------
Handles                    AliasProperty  Handles = Handlecount
Name                       AliasProperty  Name = ProcessName
NPM                        AliasProperty  NPM = NonpagedSystemMemorySize64
PM                         AliasProperty  PM = PagedMemorySize64
SI                         AliasProperty  SI = SessionId
VM                         AliasProperty  VM = VirtualMemorySize64
WS                         AliasProperty  WS = WorkingSet64
Disposed                   Event          System.EventHandler Disposed(System.Object, ...
ErrorDataReceived          Event          System.Diagnostics.DataReceivedEventHandler ...
Exited                     Event          System.EventHandler Exited(System.Object, Sy...
OutputDataReceived         Event          System.Diagnostics.DataReceivedEventHandler ...
BeginErrorReadLine         Method         void BeginErrorReadLine()
BeginOutputReadLine        Method         void BeginOutputReadLine()
CancelErrorRead            Method         void CancelErrorRead()
CancelOutputRead           Method         void CancelOutputRead()
Close                      Method         void Close()
CloseMainWindow            Method         bool CloseMainWindow()
CreateObjRef               Method         System.Runtime.Remoting.ObjRef CreateObjRef(...
Dispose                    Method         void Dispose(), void IDisposable.Dispose()
Equals                     Method         bool Equals(System.Object obj)
GetHashCode                Method         int GetHashCode()
GetLifetimeService         Method         System.Object GetLifetimeService()
GetType                    Method         type GetType()
InitializeLifetimeService  Method         System.Object InitializeLifetimeService()
Kill                       Method         void Kill()
Refresh                    Method         void Refresh()
Start                      Method         bool Start()
ToString                   Method         string ToString()
WaitForExit                Method         bool WaitForExit(int milliseconds), void Wai...
WaitForInputIdle           Method         bool WaitForInputIdle(int milliseconds), boo...
__NounName                 NoteProperty   string __NounName=Process
BasePriority               Property       int BasePriority {get;}
Container                  Property       System.ComponentModel.IContainer Container {...
EnableRaisingEvents        Property       bool EnableRaisingEvents {get;set;}
ExitCode                   Property       int ExitCode {get;}
ExitTime                   Property       datetime ExitTime {get;}
Handle                     Property       System.IntPtr Handle {get;}
HandleCount                Property       int HandleCount {get;}
HasExited                  Property       bool HasExited {get;}
Id                         Property       int Id {get;}
MachineName                Property       string MachineName {get;}
MainModule                 Property       System.Diagnostics.ProcessModule MainModule ...
MainWindowHandle           Property       System.IntPtr MainWindowHandle {get;}
MainWindowTitle            Property       string MainWindowTitle {get;}
MaxWorkingSet              Property       System.IntPtr MaxWorkingSet {get;set;}
MinWorkingSet              Property       System.IntPtr MinWorkingSet {get;set;}
Modules                    Property       System.Diagnostics.ProcessModuleCollection M...
NonpagedSystemMemorySize   Property       int NonpagedSystemMemorySize {get;}
NonpagedSystemMemorySize64 Property       long NonpagedSystemMemorySize64 {get;}
PagedMemorySize            Property       int PagedMemorySize {get;}
PagedMemorySize64          Property       long PagedMemorySize64 {get;}
PagedSystemMemorySize      Property       int PagedSystemMemorySize {get;}
PagedSystemMemorySize64    Property       long PagedSystemMemorySize64 {get;}
PeakPagedMemorySize        Property       int PeakPagedMemorySize {get;}
PeakPagedMemorySize64      Property       long PeakPagedMemorySize64 {get;}
PeakVirtualMemorySize      Property       int PeakVirtualMemorySize {get;}
PeakVirtualMemorySize64    Property       long PeakVirtualMemorySize64 {get;}
PeakWorkingSet             Property       int PeakWorkingSet {get;}
PeakWorkingSet64           Property       long PeakWorkingSet64 {get;}
PriorityBoostEnabled       Property       bool PriorityBoostEnabled {get;set;}
PriorityClass              Property       System.Diagnostics.ProcessPriorityClass Prio...
PrivateMemorySize          Property       int PrivateMemorySize {get;}
PrivateMemorySize64        Property       long PrivateMemorySize64 {get;}
PrivilegedProcessorTime    Property       timespan PrivilegedProcessorTime {get;}
ProcessName                Property       string ProcessName {get;}
ProcessorAffinity          Property       System.IntPtr ProcessorAffinity {get;set;}
Responding                 Property       bool Responding {get;}
SafeHandle                 Property       Microsoft.Win32.SafeHandles.SafeProcessHandl...
SessionId                  Property       int SessionId {get;}
Site                       Property       System.ComponentModel.ISite Site {get;set;}
StandardError              Property       System.IO.StreamReader StandardError {get;}
StandardInput              Property       System.IO.StreamWriter StandardInput {get;}
StandardOutput             Property       System.IO.StreamReader StandardOutput {get;}
StartInfo                  Property       System.Diagnostics.ProcessStartInfo StartInf...
StartTime                  Property       datetime StartTime {get;}
SynchronizingObject        Property       System.ComponentModel.ISynchronizeInvoke Syn...
Threads                    Property       System.Diagnostics.ProcessThreadCollection T...
TotalProcessorTime         Property       timespan TotalProcessorTime {get;}
UserProcessorTime          Property       timespan UserProcessorTime {get;}
VirtualMemorySize          Property       int VirtualMemorySize {get;}
VirtualMemorySize64        Property       long VirtualMemorySize64 {get;}
WorkingSet                 Property       int WorkingSet {get;}
WorkingSet64               Property       long WorkingSet64 {get;}
PSConfiguration            PropertySet    PSConfiguration {Name, Id, PriorityClass, Fi...
PSResources                PropertySet    PSResources {Name, Id, Handlecount, WorkingS...
Company                    ScriptProperty System.Object Company {get=$this.Mainmodule....
CPU                        ScriptProperty System.Object CPU {get=$this.TotalProcessorT...
Description                ScriptProperty System.Object Description {get=$this.Mainmod...
FileVersion                ScriptProperty System.Object FileVersion {get=$this.Mainmod...
Path                       ScriptProperty System.Object Path {get=$this.Mainmodule.Fil...
Product                    ScriptProperty System.Object Product {get=$this.Mainmodule....
ProductVersion             ScriptProperty System.Object ProductVersion {get=$this.Main...
[dc01]:
```

リモート コンピューターの操作が完了したら、`Exit-PSSession` コマンドレットを使用して、一対一のリモート処理セッションを終了します。

```powershell
[dc01]:  Exit-PSSession
```

## <a name="one-to-many-remoting"></a>一対多のリモート処理

リモート コンピューターでタスクを対話的に実行しなければならないこともあるでしょう。 しかし、リモート処理は、複数のリモート コンピューターで同時にタスクを実行するときに、さらに大きな力を発揮します。 `Invoke-Command` コマンドレットを使用して、1 台以上のリモート コンピューターに対してコマンドを同時に実行します。

```powershell
Invoke-Command -ComputerName dc01, sql02, web01 {Get-Service -Name W32time} -Credential $Cred
```

```Output
Status   Name        DisplayName       PSComputerName
------   ----        -----------       --------------
Running  W32time     Windows Time      web01
Start... W32time     Windows Time      dc01
Running  W32time     Windows Time      sql02
```

この例では、Windows タイム サービスの状態について、3 台のサーバーが照会されています。 `Get-Service` コマンドレットは、`Invoke-Command` のスクリプト ブロック内に置かれました。 `Get-Service` は実際にはリモート コンピューターで実行され、結果は、逆シリアル化されたオブジェクトとしてご自身のローカル コンピューターに返されます。

このコマンドを `Get-Member` にパイプ処理すると、結果が本当に逆シリアル化されたオブジェクトであることが示されます。

```powershell
Invoke-Command -ComputerName dc01, sql02, web01 {Get-Service -Name W32time} -Credential $Cred | Get-Member
```

```Output
   TypeName: Deserialized.System.ServiceProcess.ServiceController

Name                MemberType   Definition
----                ----------   ----------
GetType             Method       type GetType()
ToString            Method       string ToString(), string ToString(string format, Sys...
Name                NoteProperty string Name=W32time
PSComputerName      NoteProperty string PSComputerName=sql02
PSShowComputerName  NoteProperty bool PSShowComputerName=True
RequiredServices    NoteProperty Deserialized.System.ServiceProcess.ServiceController[...
RunspaceId          NoteProperty guid RunspaceId=570313c4-ac84-4109-bf67-c6b33236af0a
CanPauseAndContinue Property     System.Boolean {get;set;}
CanShutdown         Property     System.Boolean {get;set;}
CanStop             Property     System.Boolean {get;set;}
Container           Property      {get;set;}
DependentServices   Property     Deserialized.System.ServiceProcess.ServiceController[...
DisplayName         Property     System.String {get;set;}
MachineName         Property     System.String {get;set;}
ServiceHandle       Property     System.String {get;set;}
ServiceName         Property     System.String {get;set;}
ServicesDependedOn  Property     Deserialized.System.ServiceProcess.ServiceController[...
ServiceType         Property     System.String {get;set;}
Site                Property      {get;set;}
StartType           Property     System.String {get;set;}
Status              Property     System.String {get;set;}
```

逆シリアル化されたオブジェクトでは、メソッドのほとんどが欠落してることに注意してください。 つまり、これらのオブジェクトはライブ オブジェクトではなく、機能していません。 逆シリアル化されたオブジェクトは、リモート コンピューターでコマンドが実行された時点でのそのオブジェクトの状態のスナップショットなので、それを使ってサービスを開始または停止することはできません。

ただし、これは、`Invoke-Command` ではメソッドを使ってサービスを開始または停止できない、という意味ではなく、 メソッドは、リモート セッションで呼び出す必要がある、ということです。

これを証明するために、**Stop()** メソッドを使用して、この 3 台のリモート サーバーすべてで Windows タイム サービスを停止します。

```powershell
Invoke-Command -ComputerName dc01, sql02, web01 {(Get-Service -Name W32time).Stop()} -Credential $Cred
Invoke-Command -ComputerName dc01, sql02, web01 {Get-Service -Name W32time} -Credential $Cred
```

```Output
Status   Name        DisplayName       PSComputerName
------   ----        -----------       --------------
Running  W32time     Windows Time      web01
Start... W32time     Windows Time      dc01
Running  W32time     Windows Time      sql02
```

前の章で説明したように、タスクを完了するためのコマンドレットが存在する場合は、メソッドではなく、そのコマンドレットを使用することをお勧めします。 前のシナリオでは、stop メソッドではなく、`Stop-Service` コマンドレットを使用することをお勧めします。 ここで **Stop()** メソッドを使用したのは、PowerShell リモート処理を使用しているときにメソッドを呼び出すことができないと誤解している人が多数いるので、そうではないことを証明するためです。 返されたオブジェクトは逆シリアル化されているため、そのオブジェクトではメソッドを呼び出すことはできませんが、リモート セッション自体では呼び出すことができます。

## <a name="powershell-sessions"></a>PowerShell セッション

前のセクションの最後の例では、`Invoke-Command` コマンドレットを使用して 2 つのコマンドを実行しました。
つまり、これらの 2 つのコマンドを実行するには、2 つの個別のセッションを設定して、破棄する必要がありました。

第 7 章で説明した CIM セッションと同様、リモート コンピューターとの PowerShell セッションを使用すると、リモート コンピューターに対して複数のコマンドを実行できます。コマンドごとに新しいセッションを使用するオーバーヘッドは発生しません。

この章で使用している 3 台のコンピューター DC01、SQL02、WEB01 それぞれについて、PowerShell セッションを作成します。

```powershell
$Session = New-PSSession -ComputerName dc01, sql02, web01 -Credential $Cred
```

ここで、`$Session` という名前の変数を使用して、メソッドを使って Windows タイムサービスを開始し、サービスの状態を確認します。

```powershell
Invoke-Command -Session $Session {(Get-Service -Name W32time).Start()}
Invoke-Command -Session $Session {Get-Service -Name W32time}
```

```Output
Status   Name        DisplayName       PSComputerName
------   ----        -----------       --------------
Running  W32time     Windows Time      web01
Start... W32time     Windows Time      dc01
Running  W32time     Windows Time      sql02
```

代替の資格情報を使用してセッションが作成されると、コマンドが実行されるたびに資格情報を指定する必要がなくなります。

使い終わったセッションは必ず削除してください。

```powershell
Get-PSSession | Remove-PSSession
```

## <a name="summary"></a>まとめ

この章では、PowerShell リモート処理、1 台のリモート コンピューターを使用して対話型セッションでコマンドを実行する方法、および一対多のリモート処理を使用して複数のコンピューターに対してコマンドを実行する方法について学習しました。 また、同じリモート コンピューターに対して複数のコマンドを実行するときに PowerShell セッションを使用するメリットについても学習しました。

## <a name="review"></a>確認

1. PowerShell リモート処理を有効にするには、どうすればよいですか。
1. リモート コンピューターとの対話型セッションを開始する PowerShell コマンドは何ですか。
1. 各コマンドでただコンピューター名を指定するのではなく、PowerShell リモート処理セッションを使用するメリットは何ですか。
1. 一対一のリモート処理セッションで PowerShell リモート処理セッションを使用できますか。
1. コマンドレットによって返されるオブジェクトの種類と、その同じコマンドレットを、`Invoke-Command` を使ってリモート コンピューターに対して実行することで返されるオブジェクトの種類は、どのように違いますか。

## <a name="recommended-reading"></a>推奨トピック

- [about_Remote][]
- [about_Remote_FAQ][]
- [about_Remote_Output][]
- [about_Remote_Requirements][]
- [about_Remote_Troubleshooting][]
- [about_Remote_Variables][]

<!-- link references -->
[about_Remote]: /powershell/module/microsoft.powershell.core/about/about_remote
[about_Remote_FAQ]: /powershell/module/microsoft.powershell.core/about/about_remote_faq
[about_Remote_Output]: /powershell/module/microsoft.powershell.core/about/about_remote_output
[about_Remote_Requirements]: /powershell/module/microsoft.powershell.core/about/about_remote_requirements
[about_Remote_Troubleshooting]: /powershell/module/microsoft.powershell.core/about/about_remote_troubleshooting
[about_Remote_Variables]: /powershell/module/microsoft.powershell.core/about/about_remote_variables
[Breaking changes in PowerShell 6.0]: /powershell/scripting/whats-new/breaking-changes-ps6#remove--protocol-from--computer-cmdlets-5277
