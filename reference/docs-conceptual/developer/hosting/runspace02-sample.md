---
ms.date: 09/13/2016
ms.topic: reference
title: Runspace02 サンプル
description: Runspace02 サンプル
ms.openlocfilehash: 0206e1a80f3e5488fd2dd5628985756a5ca343c8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657892"
---
# <a name="runspace02-sample"></a>Runspace02 サンプル

このサンプル [では、system.servicemodel クラスを](/dotnet/api/system.management.automation.powershell) 使用して、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) および [Sort-Object](/powershell/module/Microsoft.PowerShell.Utility/Sort-Object) コマンドレットを同期的に実行する方法を示します。 [Get process](/powershell/module/Microsoft.PowerShell.Management/Get-Process)コマンドレットは、ローカルコンピューター上で実行[されて](/dotnet/api/System.Diagnostics.Process)いる各プロセスの System.Diagnostics.Process.Id オブジェクトを返し、はそれらのオブジェクトをその `Sort-Object` [*](/dotnet/api/System.Diagnostics.Process.Id)プロパティに基づいて並べ替えます。 これらのコマンドの結果 [は、system.string コントロールを](/dotnet/api/System.Windows.Forms.DataGridView) 使用して表示されます。

## <a name="requirements"></a>要件

このサンプルには、Windows PowerShell 2.0 が必要です。

## <a name="demonstrates"></a>対象

このサンプルでは、次のことを示します。

- コマンドを実行するため [の、system.servicemodel](/dotnet/api/system.management.automation.powershell) オブジェクトの作成。

- System.servicemodel オブジェクトのパイプラインにコマンドを追加してい[ます。](/dotnet/api/system.management.automation.powershell)

- コマンドを同期的に実行します。

- Windows フォームアプリケーションのコマンドの出力を表示する [には、system.string コントロールを](/dotnet/api/System.Windows.Forms.DataGridView) 使用します。

## <a name="example"></a>例

このサンプルでは、Windows PowerShell によって提供される既定の実行空間で、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) および [Sort オブジェクト](/powershell/module/Microsoft.PowerShell.Utility/Sort-Object) のコマンドレットを同期的に実行します。 出力は、 [システム](/dotnet/api/System.Windows.Forms.DataGridView) のコントロールを使用してフォームに表示されます。

```csharp
namespace Microsoft.Samples.PowerShell.Runspaces
{
  using System;
  using System.Collections;
  using System.Collections.ObjectModel;
  using System.Management.Automation;
  using System.Management.Automation.Runspaces;
  using System.Windows.Forms;
  using PowerShell = System.Management.Automation.PowerShell;

  /// <summary>
  /// This class contains the Main entry point for this host application.
  /// </summary>
  internal class Runspace02
  {
    /// <summary>
    /// This method creates the form where the output is displayed.
    /// </summary>
    private static void CreateForm()
    {
      Form form = new Form();
      DataGridView grid = new DataGridView();
      form.Controls.Add(grid);
      grid.Dock = DockStyle.Fill;

      // Create a PowerShell object. Creating this object takes care of
      // building all of the other data structures needed to run the command.
      using (PowerShell powershell = PowerShell.Create())
      {
        powershell.AddCommand("get-process").AddCommand("sort-object").AddArgument("ID");
        if (Runspace.DefaultRunspace == null)
        {
          Runspace.DefaultRunspace = powershell.Runspace;
        }

        Collection<PSObject> results = powershell.Invoke();

        // The generic collection needs to be re-wrapped in an ArrayList
        // for data-binding to work.
        ArrayList objects = new ArrayList();
        objects.AddRange(results);

        // The DataGridView will use the PSObjectTypeDescriptor type
        // to retrieve the properties.
        grid.DataSource = objects;
      }

      form.ShowDialog();
    }

    /// <summary>
    /// This sample uses a PowerShell object to run the Get-Process
    /// and Sort-Object cmdlets synchronously. Windows Forms and
    /// data binding are then used to display the results in a
    /// DataGridView control.
    /// </summary>
    /// <param name="args">The parameter is not used.</param>
    /// <remarks>
    /// This sample demonstrates the following:
    /// 1. Creating a PowerShell object.
    /// 2. Adding commands and arguments to the pipeline of
    ///    the PowerShell object.
    /// 3. Running the commands synchronously.
    /// 4. Using a DataGridView control to display the output
    ///    of the commands in a Windows Forms application.
    /// </remarks>
    private static void Main(string[] args)
    {
      Runspace02.CreateForm();
    }
  }
}
```

## <a name="see-also"></a>参照

[Windows PowerShell ホスト アプリケーションを記述する](./writing-a-windows-powershell-host-application.md)
