---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット内からコマンドレットを呼び出す方法
description: コマンドレット内からコマンドレットを呼び出す方法
ms.openlocfilehash: d137ac895f66000329de76a2c16a74b02c0e82ca
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667047"
---
# <a name="how-to-invoke-a-cmdlet-from-within-a-cmdlet"></a>コマンドレット内からコマンドレットを呼び出す方法

この例では、別のコマンドレット内からコマンドレットを呼び出す方法を示します。このコマンドレットを使用すると、開発中のコマンドレットに、呼び出されたコマンドレットの機能を追加できます。 この例では、 `Get-Process` コマンドレットを呼び出して、ローカルコンピューター上で実行されているプロセスを取得します。 コマンドレットの呼び出し `Get-Process` は、次のコマンドと同じです。 このコマンドは、名前が "a" ~ "t" で始まるすべてのプロセスを取得します。

```powershell
Get-Process -name [a-t]
```

> [!IMPORTANT]
> を呼び出すことができるのは、 [system.servicemodel クラスから直接派生した](/dotnet/api/System.Management.Automation.Cmdlet) コマンドレットだけです。 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet)クラスから派生したコマンドレットを呼び出すことはできません。

## <a name="to-invoke-a-cmdlet-from-within-a-cmdlet"></a>コマンドレット内からコマンドレットを呼び出すには

1. 呼び出すコマンドレットを定義するアセンブリが参照されていること、および適切なステートメントが追加されていることを確認し `using` ます。 この例では、次の名前空間が追加されています。

    ```csharp
    using System.Diagnostics;
    using System.Management.Automation;   // Windows PowerShell assembly.
    using Microsoft.PowerShell.Commands;  // Windows PowerShell assembly.
    ```

2. コマンドレットの入力処理メソッドで、呼び出すコマンドレットの新しいインスタンスを作成します。 この例では、コマンドレットが呼び出されたときに使用される引数を含む文字列と共に、型を指定すると、 [Getprocesscommand](/dotnet/api/Microsoft.PowerShell.Commands.GetProcessCommand) という型のオブジェクトが作成されます。

    ```csharp
    GetProcessCommand gp = new GetProcessCommand();
    gp.Name = new string[] { "[a-t]*" };
    ```

3. コマンドレットを呼び出すには、system.servicemodel [*](/dotnet/api/System.Management.Automation.Cmdlet.Invoke)メソッドを呼び出します。 `Get-Process`

    ```csharp
      foreach (Process p in gp.Invoke<Process>())
      {
        Console.WriteLine(p.ToString());
      }
    }
    ```

## <a name="example"></a>例

この例では、コマンドレットは、コマンド `Get-Process` レットの System.servicemodel [処理](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) メソッド内から呼び出されます。

```csharp
using System;
using System.Diagnostics;
using System.Management.Automation;   // Windows PowerShell assembly.
using Microsoft.PowerShell.Commands;  // Windows PowerShell assembly.

namespace SendGreeting
{
  // Declare the class as a cmdlet and specify an
  // appropriate verb and noun for the cmdlet name.
  [Cmdlet(VerbsCommunications.Send, "GreetingInvoke")]
  public class SendGreetingInvokeCommand : Cmdlet
  {
    // Declare the parameters for the cmdlet.
    [Parameter(Mandatory = true)]
    public string Name
    {
      get { return name; }
      set { name = value; }
    }
    private string name;

    // Override the BeginProcessing method to invoke
    // the Get-Process cmdlet.
    protected override void BeginProcessing()
    {
      GetProcessCommand gp = new GetProcessCommand();
      gp.Name = new string[] { "[a-t]*" };
      foreach (Process p in gp.Invoke<Process>())
      {
        Console.WriteLine(p.ToString());
      }
    }

    // Override the ProcessRecord method to process
    // the supplied user name and write out a
    // greeting to the user by calling the WriteObject
    // method.
    protected override void ProcessRecord()
    {
      WriteObject("Hello " + name + "!");
    }
  }
}
```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
