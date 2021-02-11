---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット コマンドレットのパラメーター
description: コマンドレット コマンドレットのパラメーター
ms.openlocfilehash: 6fd340f22202d984b7111c34806e3461a356c670
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92662778"
---
# <a name="provider-cmdlet-parameters"></a>コマンドレット コマンドレットのパラメーター

プロバイダーコマンドレットには、コマンドレットをサポートするすべてのプロバイダーが使用できる一連の静的パラメーターと、ユーザーがプロバイダーコマンドレットの特定の静的パラメーターに特定の値を指定したときに追加される動的パラメーターのセットが付属しています。

## <a name="provider-cmdlet-static-parameters"></a>プロバイダーコマンドレットの静的パラメーター

静的パラメーターは、Windows PowerShell によって定義されます。 これらのパラメーターの大規模なセットは、すべてのプロバイダー間で一貫性を提供し、より簡単な開発環境を提供するために、Windows PowerShell によって実装されます。 これらのパラメーターの例としては、 `literalPath` `exclude` コマンドレットの、、およびパラメーターがあり `include` `Get-Item` ます。 これらのパラメーターの小さなセットを上書きして、プロバイダーに固有のアクションを提供できます。 これらのパラメーターの例として、 `Path` `Value` コマンドレットのパラメーターとパラメーターがあり `Set-Item` ます。 プロバイダーコマンドレットで上書きできるパラメーターの一覧を次に示します。

`Clear-Content` コマンドレットを使用し `Path` `Clear-Content` て、 [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.ClearContent) メソッドを実装することによって、コマンドレットのパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Clear-Item`コマンドレットを使用して、プロバイダーが、 `Path` コマンドレットのパラメーターに渡された値をどのように使用するかを定義できます。これを行う `Clear-Item` には、このメソッドを実装[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItem)します。

`Clear-ItemProperty` コマンドレットを使用し `Path` `Name` て、 `Clear-ItemProperty` [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty) メソッドを実装することによって、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Copy-Item` コマンドレットを使用し `Path` `Destination` て、 `Recurse` `Copy-Item` [ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem) メソッドを実装することによって、コマンドレットの、、およびパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

Get-ChildItems コマンドレットを使用すると、Containercmdletprovider * メソッドと Getchilditems * `Path` メソッドを `Recurse` 実装する `Get-ChildItem` ことによって、 [](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)コマンドレット[](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames)のパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義することができます。

`Get-Content` コマンドレットを使用し `Path` `Get-Content` て、 [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReader) メソッドを実装することによって、コマンドレットのパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Get-Item`コマンドレットを使用して、プロバイダーが `Path` コマンドレットのパラメーターに渡す値をどのように使用するかを定義できます。これを行う `Get-Item` には、このメソッドを[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem)実装します。

`Get-ItemProperty` コマンドレットを使用し `Path` `Name` て、 `Get-ItemProperty` [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty) メソッドを実装することによって、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Invoke-Item` コマンドレットを使用して、プロバイダーが `Path` `Invoke-Item` [Invokedefaultaction *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction) メソッドを実装することによって、コマンドレットのパラメーターに渡された値をどのように使用するかを定義できます。

`Move-Item` コマンドレットを使用して、プロバイダーが、 `Path` `Destination` コマンドレットのパラメーターとパラメーターに渡された値をどのように使用するかを定義できます `Move-Item` [。これ](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) を行うには、このメソッドを実装します。

`New-Item` コマンドレットを使用し `Path` `ItemType` て、 `Value` `New-Item` [Containercmdletprovider. Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem) メソッドを実装することで、コマンドレットの、、およびパラメーターに渡された値をプロバイダーがどのように使用するかを定義できます。

`New-ItemProperty`コマンドレットを使用して、プロバイダーが、、、、 `Path` `Name` `PropertyType` および `Value` の各パラメーター `New-ItemProperty` [](/dotnet/api/Microsoft.PowerShell.Commands.RegistryProvider.NewProperty)に渡された値を使用する方法を定義することができます。

`Remove-Item``Path` `Recurse` `Remove-Item` [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)メソッドを実装することで、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Remove-ItemProperty``Path` `Name` `Remove-ItemProperty` [Idynamicpropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RemoveProperty)メソッドを実装することで、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Rename-Item` コマンドレットを使用し `Path` `NewName` て、 `Rename-Item` [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem) メソッドを実装することによって、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Rename-ItemProperty``Path` `NewName` `Name` `Rename-ItemProperty` [Idynamicpropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RenameProperty)メソッドを実装することで、コマンドレットの、、およびパラメーターに渡された値をプロバイダーがどのように使用するかを定義できます。

`Set-Content` コマンドレットを使用し `Path` `Set-Content` て、 [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriter) メソッドを実装することによって、コマンドレットのパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Set-Item` コマンドレットを使用し `Path` `Value` て、 `Set-Item` [Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) メソッドを実装することによって、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Set-ItemProperty` コマンドレットを使用し `Path` `Value` て、 `Set-Item` [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty) メソッドを実装することによって、コマンドレットのパラメーターとパラメーターに渡される値をプロバイダーがどのように使用するかを定義できます。

`Test-Path` コマンドレットを使用して、プロバイダーが `Path` `Test-Path` [Invokedefaultaction *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction) メソッドを実装することによって、コマンドレットのパラメーターに渡された値をどのように使用するかを定義できます。

また、これらのパラメーターの特性を指定することはできません。これらのパラメーターが省略可能か必須かなど、これらのパラメーターにエイリアスを指定したり、検証属性を指定したりすることはできません。 これに対して、属性などの属性を使用して、スタンドアロンのコマンドレットでパラメーターの特性を指定することができ `Parameters` ます。

## <a name="provider-cmdlet-dynamic-parameters"></a>プロバイダーコマンドレット動的パラメーター

コマンドレットプロバイダーの動的パラメーターは、スタンドアロンのコマンドレットの動的プロバイダーに似ています。 どちらの場合も、パラメーターなど、ユーザーが既定のパラメーターのいずれかに特定の値を指定すると、パラメーターがコマンドレットに追加され `path` ます。 ただし、動的パラメーターの追加をトリガーするために、すべての静的パラメーターを使用できるわけではありません。 動的パラメーターの詳細については、「 [プロバイダーコマンドレット動的パラメーター](./provider-cmdlet-dynamic-parameters.md)」を参照してください。

## <a name="see-also"></a>参照

[プロバイダーコマンドレット動的パラメーター](./provider-cmdlet-dynamic-parameters.md)

[Windows PowerShell プロバイダーを記述する](./writing-a-windows-powershell-provider.md)
