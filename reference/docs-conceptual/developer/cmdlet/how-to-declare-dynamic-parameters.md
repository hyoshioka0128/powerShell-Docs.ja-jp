---
ms.date: 09/13/2016
ms.topic: reference
title: 動的パラメーターを宣言する方法
description: 動的パラメーターを宣言する方法
ms.openlocfilehash: 0f5a8f249b414663aa9702a908ea5c8ca24755ff
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667081"
---
# <a name="how-to-declare-dynamic-parameters"></a>動的パラメーターを宣言する方法

この例では、実行時にコマンドレットに追加される動的パラメーターを定義する方法を示します。 この例では、 `Department` ユーザーがスイッチパラメーターを指定するたびに、パラメーターがコマンドレットに追加され `Employee` ます。 動的パラメーターの詳細については、「 [コマンドレット動的パラメーター](./cmdlet-dynamic-parameters.md)」を参照してください。

## <a name="to-define-dynamic-parameters"></a>動的パラメーターを定義するには

1. コマンドレットのクラス宣言で、 [Idynamicparameters](/dotnet/api/System.Management.Automation.IDynamicParameters) インターフェイスを以下のように追加します。

   ```csharp
   public class SendGreetingCommand : Cmdlet, IDynamicParameters
   ```

2. [Idynamicparameters *](/dotnet/api/System.Management.Automation.IDynamicParameters.GetDynamicParameters)メソッドを呼び出します。このメソッドは、動的パラメーターが定義されているオブジェクトを返します。 この例では、パラメーターを指定したときにメソッドが呼び出され `Employee` ます。

   ```csharp
   public object GetDynamicParameters()
   {
       if (employee)
       {
         context= new SendGreetingCommandDynamicParameters();
         return context;
       }
       return null;
   }
   private SendGreetingCommandDynamicParameters context;
   ```

3. 追加する動的パラメーターを定義するクラスを宣言します。 静的コマンドレットパラメーターを宣言するために使用した属性を使用して、動的パラメーターを宣言できます。

   ```csharp
   public class SendGreetingCommandDynamicParameters
   {
     [Parameter]
     [ValidateSet ("Marketing", "Sales", "Development")]
     public string Department
     {
       get { return department; }
       set { department = value; }
     }
     private string department;
   }
   ```

## <a name="example"></a>例

この例では、 `Department` ユーザーがパラメーターを指定するたびに、パラメーターが追加され `Employee` ます。 `Department`パラメーターは省略可能なパラメーターで、ValidateSet 属性は許可される引数を指定するために使用されます。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Management.Automation;     // PowerShell assembly.

namespace SendGreeting
{
  // Declare the cmdlet class that supports the
  // IDynamicParameters interface.
  [Cmdlet(VerbsCommunications.Send, "Greeting")]
  public class SendGreetingCommand : Cmdlet, IDynamicParameters
  {
    // Declare the parameters for the cmdlet.
    [Parameter(Mandatory = true)]
    public string Name
    {
      get { return name; }
      set { name = value; }
    }
    private string name;

    [Parameter]
    [Alias ("FTE")]
    public SwitchParameter Employee
    {
      get { return employee; }
      set { employee = value; }
    }
    private Boolean employee;

    // Implement GetDynamicParameters to
    // retrieve the dynamic parameter.
    public object GetDynamicParameters()
    {
      if (employee)
      {
        context= new SendGreetingCommandDynamicParameters();
        return context;
      }
      return null;
   }
   private SendGreetingCommandDynamicParameters context;

    // Overide the ProcessRecord method to process the
    // supplied user name and write out a greeting to
    // the user by calling the WriteObject method.
    protected override void ProcessRecord()
    {
      WriteObject("Hello " + name + "! ");
      if (employee)
      {
        WriteObject("Department: " + context.Department);
      }
    }
  }

  // Define the dynamic parameters to be added
  public class SendGreetingCommandDynamicParameters
  {
    [Parameter]
    [ValidateSet ("Marketing", "Sales", "Development")]
    public string Department
    {
      get { return department; }
      set { department = value; }
    }
    private string department;
  }
}
```

## <a name="see-also"></a>参照

[System. Automation. Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)

[Idynamicparameters * のようになります。](/dotnet/api/System.Management.Automation.IDynamicParameters.GetDynamicParameters)

[コマンドレットの動的パラメーター](./cmdlet-dynamic-parameters.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
