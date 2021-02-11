---
ms.date: 09/13/2016
ms.topic: reference
title: 制約付き実行空間を作成する
description: 制約付き実行空間を作成する
ms.openlocfilehash: 53fee3cc7d8625425bc6a73196aee9eee7f17ed6
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92651172"
---
# <a name="creating-a-constrained-runspace"></a>制約付き実行空間を作成する

パフォーマンスまたはセキュリティ上の理由により、ホストアプリケーションで使用できる Windows PowerShell コマンドを制限する必要がある場合があります。 これを行うには、System.Management.Automation.Runspaces.Initialsessionstate を呼び出して、空の [System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) を作成し [ ます。*](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState.Create) メソッドを作成し、使用可能なコマンドのみを追加します。

 指定したコマンドのみを読み込む実行空間を使用すると、パフォーマンスが大幅に向上します。

 最初のセッション状態のコマンドレットを定義するには、system.servicemodel. [Sessionstatecmd・ entry](/dotnet/api/System.Management.Automation.Runspaces.SessionStateCmdletEntry) クラスのメソッドを使用します。

 コマンドをプライベートにすることもできます。 プライベートコマンドはホストアプリケーションで使用できますが、アプリケーションのユーザーは使用できません。

## <a name="adding-commands-to-an-empty-runspace"></a>空の実行空間にコマンドを追加する

 次の例は、空の InitialSessionState を作成してコマンドを追加する方法を示しています。

```csharp
namespace Microsoft.Samples.PowerShell.Runspaces
{
  using System;
  using System.Collections.ObjectModel;
  using System.Management.Automation;
  using System.Management.Automation.Runspaces;
  using Microsoft.PowerShell.Commands;
  using PowerShell = System.Management.Automation.PowerShell;

  /// <summary>
  /// This class contains the Main entry point for the application.
  /// </summary>
  internal class Runspace10b
  {
    /// <summary>
    /// This sample shows how to create an empty initial session state,
    /// how to add commands to the session state, and then how to create a
    /// runspace that has only those two commands. A PowerShell object
    /// is used to run the Get-Command cmdlet to show that only two commands
    /// are available.
    /// </summary>
    /// <param name="args">Parameter not used.</param>
    private static void Main(string[] args)
    {
      // Create an empty InitialSessionState and then add two commands.
      InitialSessionState iss = InitialSessionState.Create();

      // Add the Get-Process and Get-Command cmdlets to the session state.
      SessionStateCmdletEntry ssce1 = new SessionStateCmdletEntry(
                                                            "get-process",
                                                            typeof(GetProcessCommand),
                                                            null);
      iss.Commands.Add(ssce1);

      SessionStateCmdletEntry ssce2 = new SessionStateCmdletEntry(
                                                            "get-command",
                                                            typeof(GetCommandCommand),
                                                            null);
      iss.Commands.Add(ssce2);

      // Create a runspace.
      using (Runspace myRunSpace = RunspaceFactory.CreateRunspace(iss))
      {
        myRunSpace.Open();
        using (PowerShell powershell = PowerShell.Create())
        {
          powershell.Runspace = myRunSpace;

          // Create a pipeline with the Get-Command command.
          powershell.AddCommand("get-command");

          Collection<PSObject> results = powershell.Invoke();

          Console.WriteLine("Verb                 Noun");
          Console.WriteLine("----------------------------");

          // Display each result object.
          foreach (PSObject result in results)
          {
            Console.WriteLine(
                             "{0,-20} {1}",
                             result.Members["verb"].Value,
                             result.Members["Noun"].Value);
          }
        }

        // Close the runspace and release any resources.
        myRunSpace.Close();
      }

      System.Console.WriteLine("Hit any key to exit...");
      System.Console.ReadKey();
    }
  }
}
```

## <a name="making-commands-private"></a>コマンドをプライベートにする

 また、コマンド **をプライベートに** 設定することも [できます。](/dotnet/api/System.Management.Automation.CommandInfo.Visibility)これを行うには、SessionStateEntryVisibility プロパティを [system](/dotnet/api/System.Management.Automation.SessionStateEntryVisibility)に設定します。 ホストアプリケーションやその他のコマンドは、そのコマンドを呼び出すことができますが、アプリケーションのユーザーは呼び出せません。 次の例では、 [get-childitem](/powershell/module/Microsoft.PowerShell.Management/Get-ChildItem) コマンドはプライベートです。

```csharp
defaultSessionState = InitialSessionState.CreateDefault();
commandIndex = GetIndexOfEntry(defaultSessionState.Commands, "get-childitem");
defaultSessionState.Commands[commandIndex].Visibility = SessionStateEntryVisibility.Private;

this.runspace = RunspaceFactory.CreateRunspace(defaultSessionState);
this.runspace.Open();
```

## <a name="see-also"></a>参照

 [InitialSessionState を作成する](./creating-an-initialsessionstate.md)
