---
ms.date: 09/12/2016
ms.topic: reference
title: Windows PowerShell ホスト クイック スタート
description: Windows PowerShell ホスト クイック スタート
ms.openlocfilehash: 4cb7dae60342abb40bd7a989a27a692826b360e5
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657418"
---
# <a name="windows-powershell-host-quickstart"></a>Windows PowerShell ホスト クイック スタート

アプリケーションで Windows PowerShell をホストするには、 [System. powershell](/dotnet/api/System.Management.Automation.PowerShell) クラスを使用します。
このクラスには、コマンドのパイプラインを作成し、そのコマンドを実行空間で実行するメソッドが用意されています。
ホストアプリケーションを作成する最も簡単な方法は、既定の実行空間を使用することです。
既定の実行空間には、Windows PowerShell のコアコマンドがすべて含まれています。
アプリケーションで Windows PowerShell コマンドのサブセットのみを公開する場合は、カスタム実行空間を作成する必要があります。

## <a name="using-the-default-runspace"></a>既定の実行空間の使用

最初に、既定の実行空間を使用します。また、コマンド、パラメーター、ステートメント、およびスクリプトをパイプラインに追加する [には、このクラスの](/dotnet/api/System.Management.Automation.PowerShell) メソッドを使用します。

### <a name="addcommand"></a>AddCommand

パイプラインにコマンドを追加するには、. [Powershell. AddCommand](/dotnet/api/System.Management.Automation.PowerShell.AddCommand) メソッドを使用します。
たとえば、コンピューター上で実行中のプロセスの一覧を取得するとします。
このコマンドを実行する方法は次のとおりです。

1. システムの [管理. PowerShell](/dotnet/api/System.Management.Automation.PowerShell) オブジェクトを作成します。

   ```csharp
   PowerShell ps = PowerShell.Create();
   ```

2. 実行するコマンドを追加します。

   ```csharp
   ps.AddCommand("Get-Process");
   ```

3. コマンドを呼び出します。

   ```csharp
   ps.Invoke();
   ```

AddCommand メソッドを複数回呼び出す前に、 [このメソッドを](/dotnet/api/System.Management.Automation.PowerShell.Invoke) 呼び出すと、最初のコマンドの結果が2番目のコマンドにパイプ処理されます。
パイプを使用して前のコマンドの結果をコマンドに渡したくない場合は [、代わりに system.object を呼び出し](/dotnet/api/System.Management.Automation.PowerShell.AddStatement) て追加します。

### <a name="addparameter"></a>AddParameter

前の例では、パラメーターを指定せずに1つのコマンドを実行しています。
コマンドにパラメーターを追加するには、を使用します。そのためには、 [AddParameter](/dotnet/api/System.Management.Automation.PSCommand.AddParameter) メソッドを使用します。
たとえば、次のコードは、 `PowerShell` コンピューター上で実行されているという名前のすべてのプロセスの一覧を取得します。

```csharp
PowerShell.Create().AddCommand("Get-Process")
                   .AddParameter("Name", "PowerShell")
                   .Invoke();
```

AddParameter メソッドを繰り返し呼び出すことで、追加のパラメーターを追加できます。

```csharp                   
PowerShell.Create().AddCommand("Get-ChildItem")
                   .AddParameter("Path", @"c:\Windows")
                   .AddParameter("Filter", "*.exe")
                   .Invoke();
```

また、パラメーターの名前と値のディクショナリを追加するには、 [system.string メソッドを呼び出します。](/dotnet/api/System.Management.Automation.PowerShell.AddParameters)

```csharp
IDictionary parameters = new Dictionary<String, String>();
parameters.Add("Path", @"c:\Windows");
parameters.Add("Filter", "*.exe");

PowerShell.Create().AddCommand("Get-Process")
   .AddParameters(parameters)
      .Invoke()

```

### <a name="addstatement"></a>AddStatement

バッチ処理をシミュレートするには、追加のステートメントをパイプラインの末尾に追加する、 [addstatement](/dotnet/api/System.Management.Automation.PowerShell.AddStatement) メソッドを使用します。
次のコードでは、という名前の実行中のプロセスの一覧を取得 `PowerShell` し、実行中のサービスの一覧を取得します。

```csharp
PowerShell ps = PowerShell.Create();
ps.AddCommand("Get-Process").AddParameter("Name", "PowerShell");
ps.AddStatement().AddCommand("Get-Service");
ps.Invoke();
```

### <a name="addscript"></a>AddScript

既存のスクリプトを実行するには、. [PowerShell. AddScript](/dotnet/api/System.Management.Automation.PowerShell.AddScript) メソッドを呼び出します。
次の例では、パイプラインにスクリプトを追加して実行します。
この例では、という名前のフォルダーにという名前のスクリプトが既に存在することを前提としてい `MyScript.ps1` `D:\PSScripts` ます。

```csharp
PowerShell ps = PowerShell.Create();
ps.AddScript("D:\PSScripts\MyScript.ps1").Invoke();
```

また、という名前のブール型パラメーターを受け取る AddScript メソッドのバージョンもあり `useLocalScope` ます。
このパラメーターがに設定されている場合、 `true` スクリプトはローカルスコープで実行されます。
次のコードでは、スクリプトがローカルスコープで実行されます。

```csharp
PowerShell ps = PowerShell.Create();
ps.AddScript(@"D:\PSScripts\MyScript.ps1", true).Invoke();
```

## <a name="creating-a-custom-runspace"></a>カスタム実行空間の作成

前の例で使用した既定の実行空間は、すべてのコア Windows PowerShell コマンドを読み込みますが、指定されたすべてのコマンドのサブセットのみを読み込むカスタム実行空間を作成できます。
これを行うと、パフォーマンスを向上させることができます (多数のコマンドを読み込むとパフォーマンスが低下します)。または、ユーザーの機能を制限して操作を実行できます。
限られた数のコマンドのみを公開する実行空間は、制約付き実行空間と呼ばれます。
制約付き実行空間を作成する [には、tialSessionState クラスと](/dotnet/api/System.Management.Automation.Runspaces.Runspace) [System.Management.Automation.Runspaces.Ini](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) クラスを使用します。

### <a name="creating-an-initialsessionstate-object"></a>InitialSessionState オブジェクトの作成

カスタム実行空間を作成するには、最初に [System.Management.Automation.Runspaces.InitialSessionState](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) オブジェクトを作成する必要があります。
次の例では、 [RunspaceFactory](/dotnet/api/System.Management.Automation.Runspaces.RunspaceFactory) を使用して、既定の InitialSessionState オブジェクトを作成した後に実行空間を作成します。

```csharp
InitialSessionState iss = InitialSessionState.CreateDefault();
Runspace rs = RunspaceFactory.CreateRunspace(iss);
rs.Open();
PowerShell ps = PowerShell.Create();
ps.Runspace = rs;
ps.AddCommand("Get-Command");
ps.Invoke();
```

### <a name="constraining-the-runspace"></a>実行空間の制約

前の例では、組み込みのコア Windows PowerShell をすべて読み込む既定の [System.Management.Automation.Runspaces.InitialSessionState](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) オブジェクトを作成しました。
また、tialSessionState メソッドをSystem.Management.Automation.Runspaces.Ini呼び出して、 [ CreateDefault2](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState.CreateDefault2) スナップインのコマンドのみを読み込む InitialSessionState オブジェクトを作成することもできます。
より制約のある実行空間を作成するには、 [System.Management.Automation.Runspaces.InitialSessionState](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState.Create) メソッドを呼び出し、InitialSessionState にコマンドを追加することによって、空の InitialSessionState オブジェクトを作成する必要があります。

指定したコマンドのみを読み込む実行空間を使用すると、パフォーマンスが大幅に向上します。

最初のセッション状態のコマンドレットを定義するには、system.servicemodel. [Sessionstatecmd・ entry](/dotnet/api/System.Management.Automation.Runspaces.SessionStateCmdletEntry) クラスのメソッドを使用します。
次の例では、空の初期セッション状態を作成し、を定義して、 `Get-Command` `Import-Module` コマンドとコマンドを初期セッション状態に追加します。
その後、その最初のセッション状態によって制約された実行空間を作成し、その実行空間でコマンドを実行します。

初期セッション状態を作成します。

```csharp
InitialSessionState iss = InitialSessionState.Create();
```

コマンドを定義し、初期セッション状態に追加します。

```csharp
SessionStateCmdletEntry getCommand = new SessionStateCmdletEntry(
        "Get-Command", typeof(Microsoft.PowerShell.Commands.GetCommandCommand), "");
SessionStateCmdletEntry importModule = new SessionStateCmdletEntry(
        "Import-Module", typeof(Microsoft.PowerShell.Commands.ImportModuleCommand), "");
iss.Commands.Add(getCommand);
iss.Commands.Add(importModule);
```

実行空間を作成して開きます。

```csharp
Runspace rs = RunspaceFactory.CreateRunspace(iss);
rs.Open();
```

コマンドを実行し、結果を表示します。

```csharp
PowerShell ps = PowerShell.Create();
ps.Runspace = rs;
ps.AddCommand("Get-Command");
Collection<CommandInfo> result = ps.Invoke<CommandInfo>();
foreach (var entry in result)
{
       Console.WriteLine(entry.Name);
}
```

実行すると、このコードの出力は次のようになります。

```powershell
Get-Command
Import-Module
```
