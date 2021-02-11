---
ms.date: 09/13/2016
ms.topic: reference
title: InitialSessionState を作成する
description: InitialSessionState を作成する
ms.openlocfilehash: d58a32c2ae8a22132f3095d093e3cb322f65c486
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649410"
---
# <a name="creating-an-initialsessionstate"></a>InitialSessionState を作成する

PowerShell コマンドは実行空間で実行されます。
アプリケーションで PowerShell をホストする [には、system.servicemodel オブジェクトを](/dotnet/api/System.Management.Automation.Runspaces.Runspace) 作成する必要があります。
各実行空間には、 [System.Management.Automation.Runspaces.InitialSessionState](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) オブジェクトが関連付けられています。
InitialSessionState は、その実行空間で使用できるコマンド、変数、モジュールなどの実行空間の特性を指定します。

## <a name="create-a-default-initialsessionstate"></a>既定の InitialSessionState を作成する

**InitialSessionState** クラスの [Createdefault](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState.CreateDefault)メソッドと [CreateDefault2](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState.CreateDefault2)メソッドを使用して、 **InitialSessionState** オブジェクトを作成できます。
**Createdefault** メソッドは、すべての組み込みコマンドが読み込まれた **InitialSessionState** を作成します。一方、CreateDefault2 メソッドは、powershell をホストするために必要なコマンド ( モジュールのコマンド) のみを読み込みます。

ホストアプリケーションで使用できるコマンドをさらに制限する場合は、制約付き実行空間を作成する必要があります。
詳細については、「 [制約付き実行空間の作成](creating-a-constrained-runspace.md)」を参照してください。

次のコードは、 **InitialSessionState** を作成して、実行空間に割り当て、その実行空間のパイプラインにコマンドを追加し、コマンドを呼び出す方法を示しています。
コマンドの追加と呼び出しの詳細については、「 [コマンドの追加と呼び出し](adding-and-invoking-commands.md)」を参照してください。

```csharp
namespace SampleHost
{
  using System;
  using System.Management.Automation;
  using System.Management.Automation.Runspaces;

  class HostP4b
  {
    static void Main(string[] args)
    {
      // Call the InitialSessionState.CreateDefault method to create
      // an empty InitialSessionState object, and then add the
      // elements that will be available when the runspace is opened.
      InitialSessionState iss = InitialSessionState.CreateDefault();
      SessionStateVariableEntry var1 = new
          SessionStateVariableEntry("test1",
                                    "MyVar1",
                                    "Initial session state MyVar1 test");
      iss.Variables.Add(var1);

      SessionStateVariableEntry var2 = new
          SessionStateVariableEntry("test2",
                                    "MyVar2",
                                    "Initial session state MyVar2 test");
      iss.Variables.Add(var2);

      // Call the RunspaceFactory.CreateRunspace(InitialSessionState)
      // method to create the runspace where the pipeline is run.
      Runspace rs = RunspaceFactory.CreateRunspace(iss);
      rs.Open();

      // Call the PowerShell.Create() method to create the PowerShell object,
      // and then specify the runspace and commands to the pipeline.
      // and create the command pipeline.
      PowerShell ps = PowerShell.Create();
      ps.Runspace = rs;
      ps.AddCommand("Get-Variable");
      ps.AddArgument("test*");

      Console.WriteLine("Variable             Value");
      Console.WriteLine("--------------------------");

      // Call the PowerShell.Invoke() method to run
      // the pipeline synchronously.
      foreach (PSObject result in ps.Invoke())
      {
        Console.WriteLine("{0,-20}{1}",
            result.Members["Name"].Value,
            result.Members["Value"].Value);
      } // End foreach.

      // Close the runspace to free resources.
      rs.Close();

    } // End Main.
  } // End SampleHost.
}
```

## <a name="see-also"></a>参照

[制約付き実行空間を作成する](creating-a-constrained-runspace.md)

[コマンドを追加し、呼び出す](adding-and-invoking-commands.md)
