---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 05/06/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-member?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Member
ms.openlocfilehash: 27e938b0264403d67066bb489a3b8c0870a7e661
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604932"
---
# Get-Member

## 概要
オブジェクトのプロパティとメソッドを取得します。

## SYNTAX

```
Get-Member [-InputObject <PSObject>] [[-Name] <String[]>] [-MemberType <PSMemberTypes>]
 [-View <PSMemberViewTypes>] [-Static] [-Force] [<CommonParameters>]
```

## Description

`Get-Member`コマンドレットは、オブジェクトのメンバー (プロパティとメソッド) を取得します。

オブジェクトを指定するには、 **InputObject** パラメーターを使用するか、にオブジェクトをパイプ処理 `Get-Member` します。 静的メンバー (インスタンスではなく、クラスのメンバー) に関する情報を取得するには、 **static** パラメーターを使用します。 **MemberType** **パラメーターなど、** 特定の種類のメンバーだけを取得するには、を使用します。

## 例

### 例 1: プロセスオブジェクトのメンバーを取得する

このコマンドは、コマンドレットによって生成されるサービスオブジェクトのプロパティとメソッドを表示し `Get-Service` ます。

`Get-Member`コマンドの部分にはパラメーターがないため、パラメーターには既定値が使用されます。 既定では、は `Get-Member` 静的または組み込みのメンバーを取得しません。

```powershell
Get-Service | Get-Member
```

```Output
   TypeName: System.Service.ServiceController#StartupType

Name                      MemberType    Definition
----                      ----------    ----------
Name                      AliasProperty Name = ServiceName
RequiredServices          AliasProperty RequiredServices = ServicesDependedOn
Disposed                  Event         System.EventHandler Disposed(System.Object, System.EventArgs)
Close                     Method        void Close()
Continue                  Method        void Continue()
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
WaitForStatus             Method        void WaitForStatus(System.ServiceProcess.ServiceControllerSt...
BinaryPathName            Property      System.String {get;set;}
CanPauseAndContinue       Property      bool CanPauseAndContinue {get;}
CanShutdown               Property      bool CanShutdown {get;}
CanStop                   Property      bool CanStop {get;}
Container                 Property      System.ComponentModel.IContainer Container {get;}
DelayedAutoStart          Property      System.Boolean {get;set;}
DependentServices         Property      System.ServiceProcess.ServiceController[] DependentServices {get;}
Description               Property      System.String {get;set;}
DisplayName               Property      string DisplayName {get;set;}
MachineName               Property      string MachineName {get;set;}
ServiceHandle             Property      System.Runtime.InteropServices.SafeHandle ServiceHandle {get;}
ServiceName               Property      string ServiceName {get;set;}
ServicesDependedOn        Property      System.ServiceProcess.ServiceController[] ServicesDependedOn {get;}
ServiceType               Property      System.ServiceProcess.ServiceType ServiceType {get;}
Site                      Property      System.ComponentModel.ISite Site {get;set;}
StartType                 Property      System.ServiceProcess.ServiceStartMode StartType {get;}
StartupType               Property      Microsoft.PowerShell.Commands.ServiceStartupType {get;set;}
Status                    Property      System.ServiceProcess.ServiceControllerStatus Status {get;}
UserName                  Property      System.String {get;set;}
ToString                  ScriptMethod  System.Object ToString();
```

### 例 2: サービスオブジェクトのメンバーを取得する

この例では、コマンドレットによって取得されたサービスオブジェクトのすべてのメンバー (プロパティとメソッド) を取得し `Get-Service` ます。これには、 **psbase**、 **PSObject**、 **get_** および **set_** メソッドなどの組み込みメンバーが含まれます。

```powershell
Get-Service | Get-Member -Force
(Get-Service Schedule).PSBase
```

この `Get-Member` コマンドは、 **Force** パラメーターを使用して、オブジェクトの組み込みメンバーとコンパイラによって生成されたメンバーを表示に追加します。 これらのプロパティとメソッドは、オブジェクトのアダプター適用対象のメソッドを使用する場合と同じ方法で使用できます。 2 番目のコマンドは、Schedule サービスの PSBase プロパティの値を表示する方法を示しています。

### 例 3: サービスオブジェクトの拡張メンバーを取得する

この例で `Types.ps1xml` は、ファイルまたはコマンドレットを使用して拡張されたサービスオブジェクトのメソッドとプロパティを取得し `Add-Member` ます。

```powershell
Get-Service | Get-Member -View Extended
```

```Output
   TypeName: System.Service.ServiceController#StartupType

Name             MemberType    Definition
----             ----------    ----------
Name             AliasProperty Name = ServiceName
RequiredServices AliasProperty RequiredServices = ServicesDependedOn
ToString         ScriptMethod  System.Object ToString();
```

この `Get-Member` コマンドは、 **View** パラメーターを使用して、サービスオブジェクトの拡張されたメンバーのみを取得します。 この場合、拡張メンバーは、 **ServiceName** プロパティの alias プロパティである **Name** プロパティです。

### 例 4: イベントログオブジェクトのスクリプトプロパティを取得する

この例では、イベントビューアーのシステムログにあるイベントログオブジェクトのスクリプトプロパティを取得します。

```powershell
Get-WinEvent -LogName System -MaxEvents 1 | Get-Member -MemberType NoteProperty
```

```Output
   TypeName: System.Diagnostics.Eventing.Reader.EventLogRecord

Name    MemberType   Definition
----    ----------   ----------
Message NoteProperty string Message=The machine-default permission settings do not grant Local ...
```

**MemberType** パラメーターは、 `NoteProperty` **MemberType** プロパティの値がであるオブジェクトのみを取得します。

このコマンドは、 **EventLogRecord** オブジェクトの **Message** プロパティを返します。

### 例 5: 指定したプロパティを使用してオブジェクトを取得する

この例では、コマンドレットの一覧から出力に **MachineName** プロパティを持つオブジェクトを取得します。

変数には、 `$list` 評価対象のコマンドレットの一覧が含まれています。 **Foreach** ステートメントは各コマンドを呼び出し、結果をに送信し `Get-Member` ます。 **Name** パラメーターを指定すると、の結果は `Get-Member` **MachineName** という名前のメンバーに制限されます。

```powershell
$list = "Get-Process", "Get-Service", "Get-Culture", "Get-PSDrive", "Get-ExecutionPolicy"
foreach ($cmdlet in $list) {& $cmdlet | Get-Member -Name MachineName}
```

```Output
   TypeName: System.Diagnostics.Process

Name        MemberType Definition
----        ---------- ----------
MachineName Property   string MachineName {get;}

   TypeName: System.Service.ServiceController#StartupType

Name        MemberType Definition
----        ---------- ----------
MachineName Property   string MachineName {get;set;}
```

結果には、プロセスオブジェクトとサービスオブジェクトのみが **MachineName** プロパティを持つことが示されています。

### 例 6: 配列のメンバーを取得する

この例では、オブジェクトの配列のメンバーを検索する方法を示します。 パイプを使用してオブジェクトの配列をに渡した場合 `Get-Member` 、コマンドレットは、配列内の一意のオブジェクト型ごとにメンバーリストを返します。
**InputObject** パラメーターを使用して配列を渡すと、配列は単一のオブジェクトとして扱われます。

```powershell
$array = @(1,'hello')
$array | Get-Member
```

```Output
   TypeName: System.Int32

Name        MemberType Definition
----        ---------- ----------
CompareTo   Method     int CompareTo(System.Object value), int CompareTo(int value), int ICompar...
Equals      Method     bool Equals(System.Object obj), bool Equals(int obj), bool IEquatable[int...
GetHashCode Method     int GetHashCode()
GetType     Method     type GetType()
GetTypeCode Method     System.TypeCode GetTypeCode(), System.TypeCode IConvertible.GetTypeCode()
ToBoolean   Method     bool IConvertible.ToBoolean(System.IFormatProvider provider)
ToByte      Method     byte IConvertible.ToByte(System.IFormatProvider provider)
...

   TypeName: System.String

Name                 MemberType            Definition
----                 ----------            ----------
Clone                Method                System.Object Clone(), System.Object ICloneable.Clone()
CompareTo            Method                int CompareTo(System.Object value), int CompareTo(str...
Contains             Method                bool Contains(string value), bool Contains(string val...
CopyTo               Method                void CopyTo(int sourceIndex, char[] destination, int ...
EndsWith             Method                bool EndsWith(string value), bool EndsWith(string val...
EnumerateRunes       Method                System.Text.StringRuneEnumerator EnumerateRunes()
Equals               Method                bool Equals(System.Object obj), bool Equals(string va...
GetEnumerator        Method                System.CharEnumerator GetEnumerator(), System.Collect...
GetHashCode          Method                int GetHashCode(), int GetHashCode(System.StringCompa...
...
```

```powershell
Get-Member -InputObject $array
```

```Output
   TypeName: System.Object[]

Name           MemberType            Definition
----           ----------            ----------
Add            Method                int IList.Add(System.Object value)
Address        Method                System.Object&, System.Private.CoreLib, Version=4.0.0.0, Cu...
Clear          Method                void IList.Clear()
Clone          Method                System.Object Clone(), System.Object ICloneable.Clone()
CompareTo      Method                int IStructuralComparable.CompareTo(System.Object other, Sy...
...
```

変数には、 `$array` 配列がパイプ処理されるときに見られるように、 **Int32** オブジェクトと **string** オブジェクトが含まれてい `Get-Member` ます。 `$array` **InputObject** パラメーターを使用してを渡すと、 `Get-Member` **Object []** 型のメンバーが返されます。

### 例 7: 設定できるオブジェクトのプロパティを決定する

この例は、オブジェクトのどのプロパティを変更できるかを判断する方法を示します。

```powershell
$File = Get-Item c:\test\textFile.txt
$File.PSObject.Properties | Where-Object isSettable | Select-Object -Property Name
```

```Output
Name
----
PSPath
PSParentPath
PSChildName
PSDrive
PSProvider
PSIsContainer
IsReadOnly
CreationTime
CreationTimeUtc
LastAccessTime
LastAccessTimeUtc
LastWriteTime
LastWriteTimeUtc
Attributes
```

## PARAMETERS

### -Force

組み込みメンバーと、コンパイラによって生成された **get_** および **set_** メソッドを表示に追加します。
**Force** パラメーターを使用した場合に追加されるプロパティを次に示します。

- **Psbase**: 拡張機能または適合しない .net オブジェクトの元のプロパティ。 これらは、オブジェクトクラスに対して定義されているプロパティです。
- **Psadapted**。 PowerShell 拡張型システムで定義されているプロパティとメソッド。
- **PSExtended**。 `Types.ps1xml`ファイルまたはコマンドレットを使用して追加されたプロパティとメソッド `Add-Member` 。
- **PSObject**。 ベースオブジェクトを PowerShell **PSObject** オブジェクトに変換するアダプター。
- **PSTypeNames**。 オブジェクト特定性の順に記述するオブジェクト型の一覧。 オブジェクトの書式を設定するときに、powershell は `Format.ps1xml` powershell インストールディレクトリ () 内のファイル内の型を検索 `$PSHOME` します。 検出した最初の型の書式設定定義を使用します。

既定では、は、 `Get-Member` **ベース** と **適合** を除くすべてのビューでこれらのプロパティを取得しますが、表示しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

取得するメンバーを含んでいるオブジェクトを指定します。

**InputObject** パラメーターの使用は、オブジェクトをにパイプする場合と同じではありません `Get-Member` 。 相違点は、次のとおりです。

- オブジェクトのコレクションをにパイプすると `Get-Member` 、は、 `Get-Member` 文字列の配列内の各文字列のプロパティなど、コレクション内の個々のオブジェクトのメンバーを取得します。
- **InputObject** を使用してオブジェクトのコレクションを送信すると、は、 `Get-Member` 文字列の配列の配列のプロパティなど、コレクションのメンバーを取得します。

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

### -MemberType

このコマンドレットが取得するメンバーの種類を指定します。 既定値は **[すべて]** です。

このパラメーターの有効値は、次のとおりです。

- AliasProperty
- CodeProperty
- プロパティ
- NoteProperty
- ScriptProperty
- プロパティ
- PropertySet
- メソッド
- CodeMethod
- ScriptMethod
- メソッド
- ParameterizedProperty
- セット
- イベント
- 動的
- すべて

これらの値の詳細については、「 [PSMemberTypes Enumeration](/dotnet/api/system.management.automation.psmembertypes)」を参照してください。

すべてのオブジェクトにすべての型のメンバーがあるわけではありません。 オブジェクトに含まれていないメンバーの種類を指定すると、PowerShell は null 値を返します。

すべての拡張メンバーなど、関連する型のメンバーを取得するには、**View** パラメーターを使用します。 **MemberType** パラメーターを **Static** または **View** パラメーターと共に使用すると、は `Get-Member` 両方のセットに属するメンバーを取得します。

```yaml
Type: System.Management.Automation.PSMemberTypes
Parameter Sets: (All)
Aliases: Type
Accepted values: AliasProperty, CodeProperty, Property, NoteProperty, ScriptProperty, Properties, PropertySet, Method, CodeMethod, ScriptMethod, Methods, ParameterizedProperty, MemberSet, Event, Dynamic, All

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

オブジェクトの 1 つまたは複数のプロパティまたはメソッドの名前を指定します。 `Get-Member` 指定されたプロパティおよびメソッドのみを取得します。

**Name** パラメーターを **MemberType**、 **View**、または **Static** パラメーターと共に使用すると、 `Get-Member` すべてのパラメーターの条件を満たすメンバーのみが取得されます。

静的メンバーを名前で取得するには、 **static** パラメーターを **name** パラメーターと共に使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Static

このコマンドレットがオブジェクトの静的プロパティおよびメソッドのみを取得することを示します。 静的プロパティおよびメソッドは、クラスの特定のインスタンスで定義されるのではなく、オブジェクトのクラスで定義されます。

**Static** パラメーターを **view** パラメーターと共に使用すると、 **view** パラメーターは無視されます。
**Static** パラメーターを **MemberType** パラメーターと共に使用すると、 `Get-Member` 両方のセットに属しているメンバーのみが取得されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ビュー

このコマンドレットが特定の型のプロパティとメソッドだけを取得することを指定します。 1 つまたは複数の値を指定します。 既定値は、[**拡張** **] になっています**。

このパラメーターの有効値は、次のとおりです。

- 底。 .NET オブジェクトの元のプロパティおよびメソッドのみを取得します (拡張子または適合なし)。
- 適用. PowerShell 拡張型システムで定義されているプロパティとメソッドだけを取得します。
- 長引い. `Types.ps1xml`ファイルまたはコマンドレットを使用して追加されたプロパティとメソッドだけを取得し `Add-Member` ます。
- すべて。 Base、Adapted、および Extended ビューのメンバーを取得します。

**View** パラメーターは、メンバーの表示だけでなく、取得するメンバーを決定します。

スクリプトプロパティなど、特定のメンバーの種類を取得するには、 **MemberType** パラメーターを使用します。 同じコマンドで **MemberType** パラメーターと **View** パラメーターを使用すると、 `Get-Member` 両方のセットに属するメンバーを取得します。 **Static** パラメーターと **view** パラメーターを同じコマンドで使用すると、 **view** パラメーターは無視されます。

```yaml
Type: System.Management.Automation.PSMemberViewTypes
Parameter Sets: (All)
Aliases:
Accepted values: Extended, Adapted, Base, All

Required: False
Position: Named
Default value: Adapted, Extended
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをにパイプすることができ `Get-Member` ます。

## 出力

### Microsoft. PowerShell. MemberDefinition

`Get-Member` が取得する各プロパティまたはメソッドのオブジェクトを返します。

## 注

コレクションオブジェクトに関する情報を取得するには、 **InputObject** パラメーターを使用するか、オブジェクトの前にコンマを付加してを渡し `Get-Member` ます。

`$This`新しいプロパティとメソッドの値を定義するスクリプトブロックでは、自動変数を使用できます。 変数は、 `$This` プロパティとメソッドを追加するオブジェクトのインスタンスを参照します。 変数の詳細については `$This` 、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

などの型リテラルと _同様に、型を表す_ オブジェクトを渡すと、 `[int]` `Get-Member` 型に関する情報が返さ `[System.RuntimeType]` れます。 ただし、 **static** パラメーターを使用すると、は、 `Get-Member` インスタンスによって表される特定の型の静的メンバーを返し `System.RuntimeType` ます。

## 関連リンク

[Add-Member](Add-Member.md)

