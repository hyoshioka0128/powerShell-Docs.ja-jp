---
ms.date: 09/13/2016
ms.topic: reference
title: 確認を要求する方法
description: 確認を要求する方法
ms.openlocfilehash: 3e29803407bd9fbf13e6db0d0a02239c34e9c4fa
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666996"
---
# <a name="how-to-request-confirmations"></a>確認を要求する方法

この例で[は、メソッド](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)を呼び出して、アクションを実行する前にユーザーからの確認を要求するように、メソッドを使用する方法を[示します。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)

> [!IMPORTANT]
> Windows PowerShell がこれらの要求を処理する方法の詳細については、「 [確認の要求](./requesting-confirmation-from-cmdlets.md)」を参照してください。

## <a name="to-request-confirmation"></a>確認を要求するには

1. `SupportsShouldProcess`コマンドレット属性のパラメーターがに設定されていることを確認し `true` ます。 (関数の場合は、この属性は、"パラメーター" になります。

    ```csharp
    [Cmdlet(VerbsDiagnostic.Test, "RequestConfirmationTemplate1",
            SupportsShouldProcess = true)]
    ```

2. `Force`コマンドレットにパラメーターを追加して、ユーザーが確認要求をオーバーライドできるようにします。

    ```csharp
    [Parameter()]
    public SwitchParameter Force
    {
      get { return force; }
      set { force = value; }
    }
    private bool force;
    ```

3. System.string メソッド `if` の戻り値を使用するステートメント[](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)を[](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)追加し、このメソッドが呼び出されるかどうかを確認します。このメソッドは、を呼び出します。

4. `if`[システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)の戻り値を使用する2番目のステートメントを追加し、パラメーターの値を指定して、 `Force` 操作を実行するかどうかを決定します。

## <a name="example"></a>例

次のコード例では、 [system..](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) [...](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) .................... [...](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) ......... ただし、他の入力処理メソッドからこれらのメソッドを呼び出すこともできます。

```csharp
protected override void ProcessRecord()
{
  if (ShouldProcess("ShouldProcess target"))
  {
    if (Force || ShouldContinue("", ""))
    {
      // Add code that performs the operation.
    }
  }
}
```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
