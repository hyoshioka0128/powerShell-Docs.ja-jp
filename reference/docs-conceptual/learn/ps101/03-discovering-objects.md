---
title: オブジェクト、プロパティ、およびメソッドの検出
description: オブジェクト、プロパティ、およびメソッドを理解して使用するうえで開発者である必要はありません。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: f226221da7dd3b663f54cf23439dd7f945ed3a2a
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99600086"
---
# <a name="chapter-3---discovering-objects-properties-and-methods"></a>第 3 章 - オブジェクト、プロパティ、およびメソッドの検出

私が初めて出会ったコンピューターは Commodore 64 でしたが、自分の初めての現代的なコンピューターは、1 MB のメモリ、40 MB のハード ドライブ、および 1 基の 5-1/4 インチ フロッピー ディスク ドライブと CGA モニターを備え、Microsoft DOS 3.3 で動作する 286 12 MHz IBM 互換機でした。

多くの IT 担当者は、私と同様にコマンド ラインに慣れていますが、オブジェクト、プロパティ、およびメソッドの話題になると思考停止状態に陥り、"私は開発者ではない" と口にします。 でも聞いてください。 PowerShell をうまく使ううえで開発者である必要はありません。 用語にとらわれないでください。 最初からすべてを理解するのは難しいかもしれませんが、少し実践的な経験を積めばいずれピンとくる瞬間がやってきます。 "なるほど。 あの本に書かれていたのはそういうことか。"

お使いのコンピューターで例を試して、実際に体験してみてください。

## <a name="requirements"></a>必要条件

この章で紹介する例の中には、Active Directory PowerShell モジュールを必要とするものがいくつかあります。
このモジュールは、Windows 用リモート サーバー管理ツール (RSAT) の一部です。 Windows の 1809 (またはそれ以上) のビルドでは、RSAT ツールは Windows の機能としてインストールされます。

- RSAT ツールのインストールについては、「[Windows 管理モジュール][]」を参照してください。
- Windows の以前のバージョンについては、[Windows 用 RSAT][] に関するページを参照してください。

## <a name="get-member"></a>Get-Member

`Get-Member` は、コマンドで使用できるオブジェクト、プロパティ、およびメソッドを調べるのに役立ちます。
オブジェクトベースの出力を生成するすべてのコマンドを、`Get-Member` にパイプ処理することができます。 プロパティは、項目に関する特性です。 運転免許証には目の色と呼ばれるプロパティがあり、そのプロパティの最も一般的な値は青と茶色です。 メソッドは、項目に対して実行できるアクションです。 運転免許証の例をそのまま使用した場合、米国の車両管理局では運転免許証を取り消すことができることから、"取り消し" がメソッドの 1 つになります。

### <a name="properties"></a>Properties

次の例では、コンピューターで実行されている Windows タイム サービスに関する情報を取得します。

```powershell
Get-Service -Name w32time
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  w32time            Windows Time
```

前の結果のセットに示されているように、**Status**、**Name**、および **DisplayName** はプロパティの例です。 **Status** プロパティの値は `Running`、**Name** プロパティの値は `w32time`、**DisplayName** の値は `Windows Time` です。

次に、同じコマンドを `Get-Member` にパイプ処理します。

```powershell
Get-Service -Name w32time | Get-Member
```

```Output
   TypeName: System.ServiceProcess.ServiceController

Name                      MemberType    Definition
----                      ----------    ----------
Name                      AliasProperty Name = ServiceName
RequiredServices          AliasProperty RequiredServices = ServicesDependedOn
Disposed                  Event         System.EventHandler Disposed(System.Object, Sy...
Close                     Method        void Close()
Continue                  Method        void Continue()
CreateObjRef              Method        System.Runtime.Remoting.ObjRef CreateObjRef(ty...
Dispose                   Method        void Dispose(), void IDisposable.Dispose()
Equals                    Method        bool Equals(System.Object obj)
ExecuteCommand            Method        void ExecuteCommand(int command)
GetHashCode               Method        int GetHashCode()
GetLifetimeService        Method        System.Object GetLifetimeService()
GetType                   Method        type GetType()
InitializeLifetimeService Method        System.Object InitializeLifetimeService()
Pause                     Method        void Pause()
Refresh                   Method        void Refresh()
Start                     Method        void Start(), void Start(string[] args)
Stop                      Method        void Stop()
WaitForStatus             Method        void WaitForStatus(System.ServiceProcess.Servi...
CanPauseAndContinue       Property      bool CanPauseAndContinue {get;}
CanShutdown               Property      bool CanShutdown {get;}
CanStop                   Property      bool CanStop {get;}
Container                 Property      System.ComponentModel.IContainer Container {get;}
DependentServices         Property      System.ServiceProcess.ServiceController[] Depe...
DisplayName               Property      string DisplayName {get;set;}
MachineName               Property      string MachineName {get;set;}
ServiceHandle             Property      System.Runtime.InteropServices.SafeHandle Serv...
ServiceName               Property      string ServiceName {get;set;}
ServicesDependedOn        Property      System.ServiceProcess.ServiceController[] Serv...
ServiceType               Property      System.ServiceProcess.ServiceType ServiceType ...
Site                      Property      System.ComponentModel.ISite Site {get;set;}
StartType                 Property      System.ServiceProcess.ServiceStartMode StartTy...
Status                    Property      System.ServiceProcess.ServiceControllerStatus ...
ToString                  ScriptMethod  System.Object ToString();
```

前の例における結果の最初の行には、非常に重要な情報が 1 つ含まれています。 **TypeName** は、返されたオブジェクトの種類を示します。 この例では、**System.ServiceProcess.ServiceController** オブジェクトが返されました。 これは、最後のピリオドの直後にある **TypeName** の部分に省略されることがよくあります (この例では **ServiceController**)。

コマンドによって生成されるオブジェクトの種類がわかれば、この情報を使用して、その種類のオブジェクトを入力として受け取るコマンドを見つけることができます。

```powershell
Get-Command -ParameterType ServiceController
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-Service                                        3.1.0.0    Microsof...
Cmdlet          Restart-Service                                    3.1.0.0    Microsof...
Cmdlet          Resume-Service                                     3.1.0.0    Microsof...
Cmdlet          Set-Service                                        3.1.0.0    Microsof...
Cmdlet          Start-Service                                      3.1.0.0    Microsof...
Cmdlet          Stop-Service                                       3.1.0.0    Microsof...
Cmdlet          Suspend-Service                                    3.1.0.0    Microsof...
```

これらのすべてのコマンドには、パイプライン、パラメーター入力、またはその両方で **ServiceController** というオブジェクトの種類を受け取るパラメーターがあります。

既定で表示されるよりも多くのプロパティがあることに注意してください。 これらの追加のプロパティは既定では表示されませんが、コマンドを `Select-Object` コマンドレットにパイプ処理し、**Property** パラメーターを使用することにより、パイプラインから選択できます。 次の例では、`Get-Service` の結果を `Select-Object` にパイプ処理し、**Property** パラメーターの値として `*` ワイルドカード文字を指定することにより、すべてのプロパティを選択します。

```powershell
Get-Service -Name w32time | Select-Object -Property *
```

```Output
Name                : w32time
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : True
CanStop             : True
DisplayName         : Windows Time
DependentServices   : {}
MachineName         : .
ServiceName         : w32time
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :
```

特定のプロパティは、**Property** パラメーターの値のコンマ区切りリストを使用して選択することもできます。

```powershell
Get-Service -Name w32time | Select-Object -Property Status, Name, DisplayName, ServiceType
```

```Output
 Status Name    DisplayName        ServiceType
 ------ ----    -----------        -----------
Running w32time Windows Time Win32ShareProcess
```

既定では、4 つのプロパティがテーブルで返され、5 つ目以降のプロパティはリストで返されます。 一部のコマンドでは、カスタム書式設定を使用して、テーブルに既定で表示されるプロパティの数を上書きできます。
これらの既定値を手動で上書きするために使用できる `Format-*` コマンドレットがいくつかあります。 最も一般的なものは `Format-Table` と `Format-List` で、どちらも次の章で扱います。

ワイルドカード文字は、`Select-Object` でプロパティ名を指定するときに使用することができます。

```powershell
Get-Service -Name w32time | Select-Object -Property Status, DisplayName, Can*
```

```powershell
Status              : Running
DisplayName         : Windows Time
CanPauseAndContinue : False
CanShutdown         : True
CanStop             : True
```

前の例では、`Can*` が **Property** パラメーターの値の 1 つとして使用され、`Can` で始まるすべてのプロパティが返されました。 これらには、**CanPauseAndContinue**、**CanShutdown**、および **CanStop** が含まれます。

### <a name="methods"></a>メソッド

メソッドは、実行できるアクションです。 `Get-Service` のメソッドのみを表示するには、**MemberType** パラメーターを使用して `Get-Member` の結果を絞り込みます。

```powershell
Get-Service -Name w32time | Get-Member -MemberType Method
```

```Output
   TypeName: System.ServiceProcess.ServiceController

Name                      MemberType Definition
----                      ---------- ----------
Close                     Method     void Close()
Continue                  Method     void Continue()
CreateObjRef              Method     System.Runtime.Remoting.ObjRef CreateObjRef(type ...
Dispose                   Method     void Dispose(), void IDisposable.Dispose()
Equals                    Method     bool Equals(System.Object obj)
ExecuteCommand            Method     void ExecuteCommand(int command)
GetHashCode               Method     int GetHashCode()
GetLifetimeService        Method     System.Object GetLifetimeService()
GetType                   Method     type GetType()
InitializeLifetimeService Method     System.Object InitializeLifetimeService()
Pause                     Method     void Pause()
Refresh                   Method     void Refresh()
Start                     Method     void Start(), void Start(string[] args)
Stop                      Method     void Stop()
WaitForStatus             Method     void WaitForStatus(System.ServiceProcess.ServiceC...
```

ご覧のとおり、多くのメソッドがあります。 **Stop** メソッドを使用すると、Windows サービスを停止できます。

```powershell
(Get-Service -Name w32time).Stop()
```

ここで、Windows タイム サービスが実際に停止していることを確認してみましょう。

```powershell
Get-Service -Name w32time
```

```Output
Status   Name               DisplayName
------   ----               -----------
Stopped  w32time            Windows Time
```

私自身はメソッドを使用することはあまりありませんが、知っておくべきことがあります。 その項目を変更するための対応するコマンドがない `Get-*` コマンドに遭遇することがあります。
多くの場合、メソッドを使用して、それを変更するアクションを実行できます。 SqlServer PowerShell モジュールの `Get-SqlAgentJob` コマンドレットは、この良い例です。 このモジュールは、[SQL Server Management Studio (SMSS)][SMSS] の一部としてインストールされます。 対応する `Set-*` コマンドレットはありませんが、メソッドを使用して同じタスクを完了できます。

メソッドに注意するもう 1 つの理由は、`Get-*` コマンドでは破壊的な変更が実行されることはないと多くの初心者が思い込んでいることにあります。 しかし、実際には、不適切な使用によって深刻な問題が発生する可能性があります。

より良いオプションは、コマンドレットがある場合はそれを使用してアクションを実行することです。 Windows タイム サービスを開始します。ただし、今回は、サービスを開始するためにコマンドレットを使用します。

```powershell
Get-Service -Name w32time | Start-Service -PassThru
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  w32time            Windows Time
```

既定では、`Start-Service` は、`Get-Service` の開始メソッドと同じように結果を返しません。
ただし、コマンドレットを使用する利点の 1 つは、メソッドでは使用できない追加機能がコマンドレットによく用意されていることです。 前の例では、**PassThru** パラメーターが使用されています。 これにより、通常は出力を生成しないコマンドレットで出力が生成されます。

コマンドレットの出力に関する思い込みに注意してください。 思い込みにとらわれるとどうなるかは皆さんご存じのとおりです。 そこで、Windows 10 ラボ環境コンピューターで実行されている PowerShell プロセスに関する情報を取得してみます。

```powershell
Get-Process -Name PowerShell
```

```Output
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    922      48   107984     140552       2.84   9020   1 powershell

```

次に、同じコマンドを Get-Member にパイプ処理します。

```powershell
Get-Process -Name PowerShell | Get-Member
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
```

既定で表示されるよりも多くのプロパティがあることに注意してください。 表示される既定のプロパティの多くは、`Get-Member` の結果を表示するときにプロパティとして表示されません。 これは、表示される値の多く (`NPM(K)`、`PM(K)`、`WS(K)`、`CPU(s)` など) が計算プロパティであるためです。 実際のプロパティ名を調べるには、コマンドを `Get-Member` にパイプ処理する必要があります。

コマンドが出力を生成しない場合は、`Get-Member` にパイプ処理することはできません。 `Start-Service` は既定で出力を生成しないため、`Get-Member` にパイプ処理しようとするとエラーが生成されます。

```powershell
Start-Service -Name w32time | Get-Member
```

```Output
Get-Member : You must specify an object for the Get-Member cmdlet.
At line:1 char:31
+ Start-Service -Name w32time | Get-Member
+
    + CategoryInfo          : CloseError: (:) [Get-Member], InvalidOperationException
    + FullyQualifiedErrorId : NoObjectInGetMember,Microsoft.PowerShell.Commands.GetMembe
   rCommand
```

**PassThru** パラメーターを `Start-Service` コマンドレットと共に指定して出力を生成すると、エラーが発生することなく `Get-Member` にパイプ処理することができます。

```powershell
Start-Service -Name w32time -PassThru | Get-Member
```

```Output
   TypeName: System.ServiceProcess.ServiceController

Name                      MemberType    Definition
----                      ----------    ----------
Name                      AliasProperty Name = ServiceName
RequiredServices          AliasProperty RequiredServices = ServicesDependedOn
Disposed                  Event         System.EventHandler Disposed(System.Object, Sy...
Close                     Method        void Close()
Continue                  Method        void Continue()
CreateObjRef              Method        System.Runtime.Remoting.ObjRef CreateObjRef(ty...
Dispose                   Method        void Dispose(), void IDisposable.Dispose()
Equals                    Method        bool Equals(System.Object obj)
ExecuteCommand            Method        void ExecuteCommand(int command)
GetHashCode               Method        int GetHashCode()
GetLifetimeService        Method        System.Object GetLifetimeService()
GetType                   Method        type GetType()
InitializeLifetimeService Method        System.Object InitializeLifetimeService()
Pause                     Method        void Pause()
Refresh                   Method        void Refresh()
Start                     Method        void Start(), void Start(string[] args)
Stop                      Method        void Stop()
WaitForStatus             Method        void WaitForStatus(System.ServiceProcess.Servi...
CanPauseAndContinue       Property      bool CanPauseAndContinue {get;}
CanShutdown               Property      bool CanShutdown {get;}
CanStop                   Property      bool CanStop {get;}
Container                 Property      System.ComponentModel.IContainer Container {get;}
DependentServices         Property      System.ServiceProcess.ServiceController[] Depe...
DisplayName               Property      string DisplayName {get;set;}
MachineName               Property      string MachineName {get;set;}
ServiceHandle             Property      System.Runtime.InteropServices.SafeHandle Serv...
ServiceName               Property      string ServiceName {get;set;}
ServicesDependedOn        Property      System.ServiceProcess.ServiceController[] Serv...
ServiceType               Property      System.ServiceProcess.ServiceType ServiceType ...
Site                      Property      System.ComponentModel.ISite Site {get;set;}
StartType                 Property      System.ServiceProcess.ServiceStartMode StartTy...
Status                    Property      System.ServiceProcess.ServiceControllerStatus ...
ToString                  ScriptMethod  System.Object ToString();
```

`Get-Member` にパイプ処理するには、コマンドがオブジェクトベースの出力を生成する必要があります。

```powershell
Get-Service -Name w32time | Out-Host | Get-Member
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  w32time            Windows Time

Get-Member : You must specify an object for the Get-Member cmdlet.
At line:1 char:40
+ Get-Service -Name w32time | Out-Host | Get-Member
+
    + CategoryInfo          : CloseError: (:) [Get-Member], InvalidOperationException
    + FullyQualifiedErrorId : NoObjectInGetMember,Microsoft.PowerShell.Commands.GetMemberCommand
```

`Out-Host` は PowerShell ホストに直接書き込みますが、パイプライン用のオブジェクトベースの出力を生成しません。 そのため、`Get-Member` にパイプ処理することはできません。

## <a name="active-directory"></a>Active Directory

> [!NOTE]
> このセクションを完了するには、この章の「要件」セクションに記載されているリモート サーバー管理ツールが必要です。 また、このドキュメントの概要で説明したように、Windows 10 ラボ環境コンピューターは、ラボ環境ドメインのメンバーである必要があります。

`Get-Command` を **Module** パラメーターと共に使用して、リモート サーバー管理ツールのインストール時に ActiveDirectory PowerShell モジュールの一部として追加されたコマンドを確認します。

```powershell
Get-Command -Module ActiveDirectory
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Add-ADCentralAccessPolicyMember                    1.0.0.0    ActiveDi...
Cmdlet          Add-ADComputerServiceAccount                       1.0.0.0    ActiveDi...
Cmdlet          Add-ADDomainControllerPasswordReplicationPolicy    1.0.0.0    ActiveDi...
Cmdlet          Add-ADFineGrainedPasswordPolicySubject             1.0.0.0    ActiveDi...
Cmdlet          Add-ADGroupMember                                  1.0.0.0    ActiveDi...
Cmdlet          Add-ADPrincipalGroupMembership                     1.0.0.0    ActiveDi...
Cmdlet          Add-ADResourcePropertyListMember                   1.0.0.0    ActiveDi...
Cmdlet          Clear-ADAccountExpiration                          1.0.0.0    ActiveDi...
Cmdlet          Clear-ADClaimTransformLink                         1.0.0.0    ActiveDi...
Cmdlet          Disable-ADAccount                                  1.0.0.0    ActiveDi...
...
```

合計 147 個のコマンドが ActiveDirectory PowerShell モジュールの一部として追加されました。 これらのコマンドの一部では、既定で、使用可能なプロパティの一部のみが返されます。

このモジュールのコマンドの名前に関して何か違う点に気が付きましたか。 コマンドの名詞部分には AD プレフィックスがあります。 これは、ほとんどのモジュールのコマンドによく見られます。 プレフィックスは、名前の競合を防ぐために設計されています。

```powershell
Get-ADUser -Identity mike | Get-Member
```

```Output
   TypeName: Microsoft.ActiveDirectory.Management.ADUser

Name              MemberType            Definition
----              ----------            ----------
Contains          Method                bool Contains(string propertyName)
Equals            Method                bool Equals(System.Object obj)
GetEnumerator     Method                System.Collections.IDictionaryEnumerator GetEn...
GetHashCode       Method                int GetHashCode()
GetType           Method                type GetType()
ToString          Method                string ToString()
Item              ParameterizedProperty Microsoft.ActiveDirectory.Management.ADPropert...
DistinguishedName Property              System.String DistinguishedName {get;set;}
Enabled           Property              System.Boolean Enabled {get;set;}
GivenName         Property              System.String GivenName {get;set;}
Name              Property              System.String Name {get;}
ObjectClass       Property              System.String ObjectClass {get;set;}
ObjectGUID        Property              System.Nullable`1[[System.Guid, mscorlib, Vers...
SamAccountName    Property              System.String SamAccountName {get;set;}
SID               Property              System.Security.Principal.SecurityIdentifier S...
Surname           Property              System.String Surname {get;set;}
UserPrincipalName Property              System.String UserPrincipalName {get;set;}
```

Active Directory に漠然と慣れているだけの場合でも、ユーザー アカウントにはこの例に示されているよりも多くのプロパティがあることに気付いていることでしょう。

`Get-ADUser` コマンドレットには **Properties** パラメーターがあります。これを使用すると、返される追加の (既定以外の) プロパティを指定できます。 `*` ワイルドカード文字を指定すると、そのすべてが返されます。

```powershell
Get-ADUser -Identity mike -Properties * | Get-Member
```

```Output
   TypeName: Microsoft.ActiveDirectory.Management.ADUser

Name                                 MemberType            Definition
----                                 ----------            ----------
Contains                             Method                bool Contains(string proper...
Equals                               Method                bool Equals(System.Object obj)
GetEnumerator                        Method                System.Collections.IDiction...
GetHashCode                          Method                int GetHashCode()
GetType                              Method                type GetType()
ToString                             Method                string ToString()
Item                                 ParameterizedProperty Microsoft.ActiveDirectory.M...
AccountExpirationDate                Property              System.DateTime AccountExpi...
accountExpires                       Property              System.Int64 accountExpires...
AccountLockoutTime                   Property              System.DateTime AccountLock...
AccountNotDelegated                  Property              System.Boolean AccountNotDe...
AllowReversiblePasswordEncryption    Property              System.Boolean AllowReversi...
AuthenticationPolicy                 Property              Microsoft.ActiveDirectory.M...
AuthenticationPolicySilo             Property              Microsoft.ActiveDirectory.M...
BadLogonCount                        Property              System.Int32 BadLogonCount ...
badPasswordTime                      Property              System.Int64 badPasswordTim...
badPwdCount                          Property              System.Int32 badPwdCount {g...
CannotChangePassword                 Property              System.Boolean CannotChange...
CanonicalName                        Property              System.String CanonicalName...
Certificates                         Property              Microsoft.ActiveDirectory.M...
City                                 Property              System.String City {get;set;}
CN                                   Property              System.String CN {get;}
codePage                             Property              System.Int32 codePage {get;...
Company                              Property              System.String Company {get;...
CompoundIdentitySupported            Property              Microsoft.ActiveDirectory.M...
Country                              Property              System.String Country {get;...
countryCode                          Property              System.Int32 countryCode {g...
Created                              Property              System.DateTime Created {get;}
createTimeStamp                      Property              System.DateTime createTimeS...
Deleted                              Property              System.Boolean Deleted {get;}
Department                           Property              System.String Department {g...
Description                          Property              System.String Description {...
DisplayName                          Property              System.String DisplayName {...
DistinguishedName                    Property              System.String Distinguished...
Division                             Property              System.String Division {get...
DoesNotRequirePreAuth                Property              System.Boolean DoesNotRequi...
dSCorePropagationData                Property              Microsoft.ActiveDirectory.M...
EmailAddress                         Property              System.String EmailAddress ...
EmployeeID                           Property              System.String EmployeeID {g...
EmployeeNumber                       Property              System.String EmployeeNumbe...
Enabled                              Property              System.Boolean Enabled {get...
Fax                                  Property              System.String Fax {get;set;}
GivenName                            Property              System.String GivenName {ge...
HomeDirectory                        Property              System.String HomeDirectory...
HomedirRequired                      Property              System.Boolean HomedirRequi...
HomeDrive                            Property              System.String HomeDrive {ge...
HomePage                             Property              System.String HomePage {get...
HomePhone                            Property              System.String HomePhone {ge...
Initials                             Property              System.String Initials {get...
instanceType                         Property              System.Int32 instanceType {...
isDeleted                            Property              System.Boolean isDeleted {g...
KerberosEncryptionType               Property              Microsoft.ActiveDirectory.M...
LastBadPasswordAttempt               Property              System.DateTime LastBadPass...
LastKnownParent                      Property              System.String LastKnownPare...
lastLogoff                           Property              System.Int64 lastLogoff {ge...
lastLogon                            Property              System.Int64 lastLogon {get...
LastLogonDate                        Property              System.DateTime LastLogonDa...
lastLogonTimestamp                   Property              System.Int64 lastLogonTimes...
LockedOut                            Property              System.Boolean LockedOut {g...
logonCount                           Property              System.Int32 logonCount {ge...
LogonWorkstations                    Property              System.String LogonWorkstat...
Manager                              Property              System.String Manager {get;...
MemberOf                             Property              Microsoft.ActiveDirectory.M...
MNSLogonAccount                      Property              System.Boolean MNSLogonAcco...
MobilePhone                          Property              System.String MobilePhone {...
Modified                             Property              System.DateTime Modified {g...
modifyTimeStamp                      Property              System.DateTime modifyTimeS...
msDS-User-Account-Control-Computed   Property              System.Int32 msDS-User-Acco...
Name                                 Property              System.String Name {get;}
nTSecurityDescriptor                 Property              System.DirectoryServices.Ac...
ObjectCategory                       Property              System.String ObjectCategor...
ObjectClass                          Property              System.String ObjectClass {...
ObjectGUID                           Property              System.Nullable`1[[System.G...
objectSid                            Property              System.Security.Principal.S...
Office                               Property              System.String Office {get;s...
OfficePhone                          Property              System.String OfficePhone {...
Organization                         Property              System.String Organization ...
OtherName                            Property              System.String OtherName {ge...
PasswordExpired                      Property              System.Boolean PasswordExpi...
PasswordLastSet                      Property              System.DateTime PasswordLas...
PasswordNeverExpires                 Property              System.Boolean PasswordNeve...
PasswordNotRequired                  Property              System.Boolean PasswordNotR...
POBox                                Property              System.String POBox {get;set;}
PostalCode                           Property              System.String PostalCode {g...
PrimaryGroup                         Property              System.String PrimaryGroup ...
primaryGroupID                       Property              System.Int32 primaryGroupID...
PrincipalsAllowedToDelegateToAccount Property              Microsoft.ActiveDirectory.M...
ProfilePath                          Property              System.String ProfilePath {...
ProtectedFromAccidentalDeletion      Property              System.Boolean ProtectedFro...
pwdAnswer                            Property              System.String pwdAnswer {ge...
pwdLastSet                           Property              System.Int64 pwdLastSet {ge...
pwdQuestion                          Property              System.String pwdQuestion {...
SamAccountName                       Property              System.String SamAccountNam...
sAMAccountType                       Property              System.Int32 sAMAccountType...
ScriptPath                           Property              System.String ScriptPath {g...
sDRightsEffective                    Property              System.Int32 sDRightsEffect...
ServicePrincipalNames                Property              Microsoft.ActiveDirectory.M...
SID                                  Property              System.Security.Principal.S...
SIDHistory                           Property              Microsoft.ActiveDirectory.M...
SmartcardLogonRequired               Property              System.Boolean SmartcardLog...
sn                                   Property              System.String sn {get;set;}
State                                Property              System.String State {get;set;}
StreetAddress                        Property              System.String StreetAddress...
Surname                              Property              System.String Surname {get;...
Title                                Property              System.String Title {get;set;}
TrustedForDelegation                 Property              System.Boolean TrustedForDe...
TrustedToAuthForDelegation           Property              System.Boolean TrustedToAut...
UseDESKeyOnly                        Property              System.Boolean UseDESKeyOnl...
userAccountControl                   Property              System.Int32 userAccountCon...
userCertificate                      Property              Microsoft.ActiveDirectory.M...
UserPrincipalName                    Property              System.String UserPrincipal...
uSNChanged                           Property              System.Int64 uSNChanged {get;}
uSNCreated                           Property              System.Int64 uSNCreated {get;}
whenChanged                          Property              System.DateTime whenChanged...
whenCreated                          Property              System.DateTime whenCreated...
```

これで、それらしく見えるようになりました。

Active Directory ユーザー アカウントのプロパティが既定で制限される理由を考えてみてください。 運用版の Active Directory 環境で、すべてのユーザー アカウントについてすべてのプロパティを返した場合を想像してください。 ドメイン コントローラー自体だけでなく、ネットワークにも発生する可能性があるパフォーマンスの低下を考えてみてください。 とにかく実際にすべてのプロパティが必要になるかどうかは疑問です。 存在するプロパティを調べる場合、単一のユーザー アカウントについてすべてのプロパティを返すことはまったく問題ありません。

プロトタイプを作成するときにコマンドを何度も実行することは珍しくありません。 大きなクエリを実行する場合は、クエリを 1 回実行し、その結果を変数に格納します。 次に、面倒なクエリを繰り返し使用する代わりに、変数の内容を操作します。

```powershell
$Users = Get-ADUser -Identity mike -Properties *
```

前のコマンドを何度も実行する代わりに、`$Users` 変数の内容を使用します。
Active Directory 内でそのユーザーに変更を加えても変数の内容は更新されないことに注意してください。

`$Users` 変数を `Get-Member` にパイプ処理すると、使用可能なプロパティを見つけることができます。

```powershell
$Users | Get-Member
```

次に、`$Users` を `Select-Object` にパイプ処理して個々のプロパティを選択します。Active Directory に対して何度もクエリを実行する必要はありません。

```powershell
$Users | Select-Object -Property Name, LastLogonDate, LastBadPasswordAttempt
```

Active Directory に対して複数回クエリを実行する場合は、**Properties** パラメーターを使用して、任意の既定以外のプロパティを指定します。

```powershell
Get-ADUser -Identity mike -Properties LastLogonDate, LastBadPasswordAttempt
```

```Output
DistinguishedName      : CN=Mike F. Robbins,OU=Sales,DC=mikefrobbins,DC=com
Enabled                : True
GivenName              : Mike
LastBadPasswordAttempt : 2/4/2017 10:46:15 AM
LastLogonDate          : 2/18/2017 12:45:14 AM
Name                   : Mike F. Robbins
ObjectClass            : user
ObjectGUID             : a82a8c58-1332-4a57-a6e2-68e0c750ea56
SamAccountName         : mike
SID                    : S-1-5-21-2989741381-570885089-3319121794-1108
Surname                : Robbins
UserPrincipalName      : miker@mikefrobbins.com
```

## <a name="summary"></a>まとめ

この章では、コマンドによって生成されるオブジェクトの種類を調べる方法、コマンドで使用できるプロパティとメソッドを調べる方法、および既定で返されるプロパティを制限するコマンドを操作する方法を学習しました。

## <a name="review"></a>確認

1. `Get-Process` コマンドレットではどのような種類のオブジェクトが生成されますか。
1. コマンドの使用可能なプロパティを調べるにはどうすればよいですか。
1. 何かを取得するためのコマンドが存在する一方で、それを設定するためのコマンドがない場合、何を確認する必要がありますか。
1. 既定で出力を生成しない特定のコマンドで出力を生成するにはどうすればよいですか。
1. 大量の出力を生成するコマンドの結果を処理する場合は、どのようなことを検討する必要がありますか。

## <a name="recommended-reading"></a>推奨トピック

- [Get-Member][]
- [オブジェクトの構造を表示する (Get-member)][]
- [about_Objects][]
- [about_Properties][]
- [about_Methods][]
- [PowerShell コマンドレットで何かを開始または停止することはできませんか。Get コマンドレットでメソッドを確認することを忘れないでください。][use-methods]

<!-- link references -->
[Windows 用 RSAT]: https://support.microsoft.com/help/2693643
[Windows 管理モジュール]: /powershell/scripting/whats-new/module-compatibility#windows-management-modules
[Get-Member]: /powershell/module/microsoft.powershell.utility/get-member
[オブジェクトの構造を表示する (Get-member)]: /powershell/scripting/samples/viewing-object-structure--get-member-
[about_Objects]: /powershell/module/microsoft.powershell.core/about/about_objects
[about_Properties]: /powershell/module/microsoft.powershell.core/about/about_properties
[about_Methods]: /powershell/module/microsoft.powershell.core/about/about_methods
[use-methods]: https://mikefrobbins.com/2016/12/15/no-powershell-cmdlet-to-start-or-stop-something-dont-forget-to-check-for-methods-on-the-get-cmdlets/
[SMSS]: /sql/ssms/download-sql-server-management-studio-ssms
