---
ms.date: 09/13/2016
ms.topic: reference
title: Runspace01 サンプル
description: Runspace01 サンプル
ms.openlocfilehash: f47f79dd507db258119016353dc5a72d110d9252
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657918"
---
# <a name="runspace01-sample"></a>Runspace01 サンプル

このサンプルでは、 [system.servicemodel クラスを](/dotnet/api/system.management.automation.powershell) 使用して、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) コマンドレットを同期的に実行する方法を示します。 [Get process](/powershell/module/Microsoft.PowerShell.Management/Get-Process)コマンドレットは、ローカルコンピューター上で実行されている各プロセスの[system.object オブジェクトを返します。](/dotnet/api/System.Diagnostics.Process) 次に、返されたオブジェクトから抽出され、コンソールウィンドウに表示される、 [Processname *](/dotnet/api/System.Diagnostics.Process.ProcessName) [プロパティと * プロパティ](/dotnet/api/System.Diagnostics.Process.Handlecount) の値を抽出します。

## <a name="requirements"></a>要件

 このサンプルには、Windows PowerShell 2.0 が必要です。

## <a name="demonstrates"></a>対象

- コマンドを実行するための、 [system.servicemodel オブジェクトの作成。](/dotnet/api/system.management.automation.powershell)

- コマンドを、 [システムの管理](/dotnet/api/system.management.automation.powershell) オブジェクトのパイプラインに追加します。

- コマンドを同期的に実行します。

- コマンドによって返されたオブジェクトからプロパティを抽出するには、[を使用します。](/dotnet/api/System.Management.Automation.PSObject)

## <a name="example"></a>例

 このサンプルでは、Windows PowerShell によって提供される既定の実行空間で、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) コマンドレットを同期的に実行します。

```csharp
namespace Microsoft.Samples.PowerShell.Runspaces
{
  using System;
  using System.Management.Automation;
  using PowerShell = System.Management.Automation.PowerShell;

  /// <summary>
  /// This class contains the Main entry point for this host application.
  /// </summary>
  internal class Runspace01
  {
    /// <summary>
    /// This sample uses the PowerShell class to execute
    /// the get-process cmdlet synchronously. The name and
    /// handlecount are then extracted from the PSObjects
    /// returned and displayed.
    /// </summary>
    /// <param name="args">Parameter not used.</param>
    /// <remarks>
    /// This sample demonstrates the following:
    /// 1. Creating a PowerShell object to run a command.
    /// 2. Adding a command to the pipeline of the PowerShell object.
    /// 3. Running the command synchronously.
    /// 4. Using PSObject objects to extract properties from the objects
    ///    returned by the command.
    /// </remarks>
    private static void Main(string[] args)
    {
      // Create a PowerShell object. Creating this object takes care of
      // building all of the other data structures needed to run the command.
      using (PowerShell powershell = PowerShell.Create().AddCommand("get-process"))
      {
        Console.WriteLine("Process              HandleCount");
        Console.WriteLine("--------------------------------");

        // Invoke the command synchronously and display the
        // ProcessName and HandleCount properties of the
        // objects that are returned.
        foreach (PSObject result in powershell.Invoke())
        {
          Console.WriteLine(
                      "{0,-20} {1}",
                      result.Members["ProcessName"].Value,
                      result.Members["HandleCount"].Value);
        }
      }

      System.Console.WriteLine("Hit any key to exit...");
      System.Console.ReadKey();
    }
  }
}
```

## <a name="see-also"></a>関連項目
