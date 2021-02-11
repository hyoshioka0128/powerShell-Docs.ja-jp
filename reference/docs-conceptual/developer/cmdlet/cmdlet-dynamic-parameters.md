---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットの動的パラメーター
description: コマンドレットの動的パラメーター
ms.openlocfilehash: b44dda2354e8b689e419c7bf4deefadfc4edcb07
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653432"
---
# <a name="cmdlet-dynamic-parameters"></a>コマンドレット動的パラメーター

コマンドレットでは、他のパラメーターの引数が特定の値の場合など、特殊な条件下でユーザーが使用できるパラメーターを定義できます。 これらのパラメーターは、必要なときにのみ追加されるため、実行時に追加され、動的パラメーターと呼ばれます。 たとえば、特定のスイッチパラメーターが指定されている場合にのみ、いくつかのパラメーターを追加するコマンドレットを設計できます。

> [!NOTE]
> プロバイダーと PowerShell 関数では、動的パラメーターを定義することもできます。

## <a name="dynamic-parameters-in-powershell-cmdlets"></a>PowerShell コマンドレットの動的パラメーター

PowerShell では、プロバイダーコマンドレットのいくつかで動的パラメーターが使用されます。 たとえば、 `Get-Item` `Get-ChildItem` コマンドレットとコマンドレットは、 **Path** パラメーターで **証明書** プロバイダーのパスを指定した場合に、実行時に **codesigningcert** パラメーターを追加します。 **Path** パラメーターに別のプロバイダーのパスが指定されている場合、 **Codesigningcert** パラメーターは使用できません。

次の例は、を実行したときに、実行時に **Codesigningcert** パラメーターがどのように追加されるかを示してい `Get-Item` ます。

この例では、PowerShell ランタイムによってパラメーターが追加され、コマンドレットが正常に実行されました。

```powershell
Get-Item -Path cert:\CurrentUser -CodeSigningCert
```

```Output
Location   : CurrentUser
StoreNames : {SmartCardRoot, UserDS, AuthRoot, CA...}
```

この例では、 **ファイルシステム** ドライブが指定され、エラーが返されます。 エラーメッセージは、 **Codesigningcert** パラメーターが見つからないことを示しています。

```powershell
Get-Item -Path C:\ -CodeSigningCert
```

```Output
Get-Item : A parameter cannot be found that matches parameter name 'codesigningcert'.
At line:1 char:37
+  get-item -path C:\ -codesigningcert <<<<
--------
    CategoryInfo          : InvalidArgument: (:) [Get-Item], ParameterBindingException
    FullyQualifiedErrorId : NamedParameterNotFound,Microsoft.PowerShell.Commands.GetItemCommand
```

## <a name="support-for-dynamic-parameters"></a>動的パラメーターのサポート

動的パラメーターをサポートするには、次の要素をコマンドレットコードに含める必要があります。

### <a name="interface"></a>インターフェイス

[IDynamicParameters](/dotnet/api/System.Management.Automation.IDynamicParameters)のようになります。
このインターフェイスは、動的パラメーターを取得するメソッドを提供します。

次に例を示します。

`public class SendGreetingCommand : Cmdlet, IDynamicParameters`

### <a name="method"></a>Method

[IDynamicParameters](/dotnet/api/System.Management.Automation.IDynamicParameters.GetDynamicParameters)のようにしてください。
このメソッドは、動的パラメーター定義を含むオブジェクトを取得します。

次に例を示します。

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

### <a name="class"></a>クラス

追加する動的パラメーターを定義するクラス。 このクラスには、各パラメーターの **パラメーター** 属性と、コマンドレットで必要とされるオプションの **エイリアス** および **検証** 属性が含まれている必要があります。

次に例を示します。

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

動的パラメーターをサポートするコマンドレットの完全な例については、「 [動的パラメーターを宣言する方法](./how-to-declare-dynamic-parameters.md)」を参照してください。

## <a name="see-also"></a>関連項目

[IDynamicParameters (システム管理)](/dotnet/api/System.Management.Automation.IDynamicParameters)

[IDynamicParameters (システムの管理) パラメーター](/dotnet/api/System.Management.Automation.IDynamicParameters.GetDynamicParameters)

[動的パラメーターを宣言する方法](./how-to-declare-dynamic-parameters.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
