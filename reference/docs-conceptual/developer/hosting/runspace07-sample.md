---
ms.date: 09/13/2016
ms.topic: reference
title: Runspace07 サンプル
description: Runspace07 サンプル
ms.openlocfilehash: 4356f33a1d962a0a6c5ca1ebb8c3e4c579463022
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657670"
---
# <a name="runspace07-sample"></a>Runspace07 サンプル

このサンプルでは、実行空間を作成し、その実行空間を使用して、 [複数のコマンド](/dotnet/api/system.management.automation.powershell) レットを同期的に実行する方法を示します。

## <a name="requirements"></a>要件

このサンプルには、Windows PowerShell 2.0 が必要です。

## <a name="demonstrates"></a>対象

このサンプルでは、次のことを示します。

- [Runspacefactory](/dotnet/api/System.Management.Automation.Runspaces.RunspaceFactory)クラスを使用して、[システムの管理](/dotnet/api/System.Management.Automation.Runspaces.Runspace)... 実行空間オブジェクトを作成します。

- 実行空間 [を使用](/dotnet/api/system.management.automation.powershell) する、system.string オブジェクトを作成します。

- コマンドレットを、 [System. Automation. Powershell](/dotnet/api/system.management.automation.powershell) オブジェクトのパイプラインに追加します。

- コマンドレットを同期的に実行します。

- コマンドによって返された、 [システムの管理](/dotnet/api/System.Management.Automation.PSObject) オブジェクトからプロパティを抽出しています。

## <a name="example"></a>例

このサンプルでは、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process)コマンドレットと[Measure オブジェクト](/powershell/module/microsoft.powershell.utility/measure-object)コマンドレットを実行するため[に、system.servicemodel オブジェクトに](/dotnet/api/System.Management.Automation.PSObject)よって使用される実行空間を作成します。

```csharp
namespace Microsoft.Samples.PowerShell.Runspaces
{
  using System;
  using System.Collections.ObjectModel;
  using System.Management.Automation;
  using System.Management.Automation.Runspaces;
  using PowerShell = System.Management.Automation.PowerShell;

  /// <summary>
  /// This class contains the Main entry point for this host application.
  /// </summary>
  internal class Runspace07
  {
    /// <summary>
    /// This sample shows how to create a runspace and how to run commands
    /// using a PowerShell object. It builds a pipeline that runs the
    /// get-process cmdlet, which is piped to the measure-object
    /// cmdlet to count the number of processes running on the system.
    /// </summary>
    /// <param name="args">The parameter is not used.</param>
    /// <remarks>
    /// This sample demonstrates the following:
    /// 1. Creating a runspace using the RunspaceFactory class.
    /// 2. Creating a PowerShell object that uses the runspace.
    /// 3. Adding cmdlets to the pipeline of the PowerShell object.
    /// 4. Running the cmdlets synchronously.
    /// 5. Working with PSObject objects to extract properties
    ///    from the objects returned by the cmdlets.
    /// </remarks>
    private static void Main(string[] args)
    {
      Collection<PSObject> result;     // Will hold the result
                                       // of running the cmdlets.

      // Create a runspace. We can not use the RunspaceInvoke class
      // because we need to get at the underlying runspace to
      // explicitly add the commands. Notice that no PSHost object is
      // supplied to the CreateRunspace method so the default host is
      // used. See the Host samples for more information on creating
      // your own custom host.
      using (Runspace myRunSpace = RunspaceFactory.CreateRunspace())
      {
        myRunSpace.Open();

        // Create a PowerShell object and specify the runspace.
        PowerShell powershell = PowerShell.Create();
        powershell.Runspace = myRunSpace;

        // Use the using statement so we dispose of the PowerShell object
        // when we're done.
        using (powershell)
        {
          // Add the get-process cmdlet to the PowerShell object. Notice
          // we are specify the name of the cmdlet, not a script.
          powershell.AddCommand("get-process");

          // Add the measure-object cmdlet to count the number
          // of objects being returned. Commands are always added to the end
          // of the pipeline.
          powershell.AddCommand("measure-object");

          // Run the cmdlets synchronously and save the objects returned.
          result = powershell.Invoke();
        }

        // Even after disposing of the pipeLine, we still need to set
        // the powershell variable to null so that the garbage collector
        // can clean it up.
        powershell = null;

        // Display the results of running the commands (checking that
        // everything is ok first.
        if (result == null || result.Count != 1)
        {
          throw new InvalidOperationException(
                    "pipeline.Invoke() returned the wrong number of objects");
        }

        PSMemberInfo count = result[0].Properties["Count"];
        if (count == null)
        {
          throw new InvalidOperationException(
                    "The object returned doesn't have a 'count' property");
        }

        Console.WriteLine(
                   "Runspace07: The Get-Process cmdlet returned {0} objects",
                   count.Value);

        // Close the runspace to release any resources.
        myRunSpace.Close();
      }

      System.Console.WriteLine("Hit any key to exit...");
      System.Console.ReadKey();
    }
  }
}
```

## <a name="see-also"></a>参照

[Windows PowerShell ホスト アプリケーションを記述する](./writing-a-windows-powershell-host-application.md)
