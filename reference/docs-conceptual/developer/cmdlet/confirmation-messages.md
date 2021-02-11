---
ms.date: 09/13/2016
ms.topic: reference
title: 確認メッセージ
description: 確認メッセージ
ms.openlocfilehash: 76302b2f8d8ca6dcdfe1b3c36f71aad23a53f7cf
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355189"
---
# <a name="confirmation-messages"></a>確認メッセージ

次に示すのは、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) の種類によって異なる確認メッセージが表示されます。このメソッドは、呼び出さ [れるメソッドの](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) 種類によって異なります。

> [!IMPORTANT]
> 確認を要求する方法を示すサンプルコードについては、「確認 [を要求する方法](./how-to-request-confirmations.md)」を参照してください。

## <a name="specifying-the-resource"></a>リソースの指定

変更しようとしているリソースを指定するには、このコマンドレットを呼び出してください [。Displayproperty = Fullname](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) メソッド。 この場合、メソッドのパラメーターを使用してリソースを指定する `target` と、操作は Windows PowerShell によって追加されます。 次のメッセージでは、"MyResource" というテキストが使用されるリソースであり、操作は呼び出しを行うコマンドの名前です。

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "Test-RequestConfirmationTemplate1" on Target "MyResource".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

次の例に示すように、ユーザーが確認要求に対して **[はい]** または **[はい] を** 選択した場合は、" [System](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) ..............................................

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "Test-RequestConfirmationTemplate1" on Target "MyResource".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
```

## <a name="specifying-the-operation-and-resource"></a>操作とリソースの指定

変更しようとしているリソースと、コマンドが実行される操作を指定するには、そのリソースを使用します。この場合、 [% 2a を呼び出します。Displayproperty = Fullname](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) メソッド。 この場合、パラメーターを使用してリソースを指定し、パラメーターを使用して `target` 操作を `target` 行います。 次のメッセージでは、"MyResource" というテキストが動作しているリソースであり、"Myresource" は実行される操作です。

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "MyAction" on Target "MyResource".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

ユーザーが前のメッセージに対して **[はい]** または **[はい] を** 選択した場合は、" [System](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) ................................................

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "MyAction" on Target "MyResource".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
