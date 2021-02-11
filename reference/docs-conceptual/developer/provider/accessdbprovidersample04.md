---
ms.date: 09/13/2016
ms.topic: reference
title: AccessDBProviderSample04
description: AccessDBProviderSample04
ms.openlocfilehash: 962d0ab673ff797a60b56ccae7a16a810cc43c58
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649048"
---
# <a name="accessdbprovidersample04"></a>AccessDBProviderSample04

このサンプルでは、、、、およびの各コマンドレットの呼び出しをサポートするために、コンテナーメソッドを上書きする方法を示し `Copy-Item` `Get-ChildItem` `New-Item` `Remove-Item` ます。 これらのメソッドは、データ ストアにコンテナーであるアイテムが含まれる場合に実装する必要があります。 コンテナーは、共通の親項目の子項目のグループです。 このサンプルのプロバイダークラスは、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider) クラスから派生しています。

## <a name="demonstrates"></a>対象

> [!IMPORTANT]
> プロバイダークラスは、ほとんどの場合、[システム](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider)から派生することがあります。

このサンプルでは、次の方法を示します。

- 属性を宣言 `CmdletProvider` しています。
- [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider)クラスから派生したプロバイダークラスを定義しています。
- [ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)メソッドを上書きして、コマンドレットの動作を変更します `Copy-Item` 。これにより、ユーザーは、ある場所から別の場所に項目をコピーできます。 (このサンプルでは、コマンドレットに動的パラメーターを追加する方法については説明しません `Copy-Item` )。
- Containercmdletprovider コマンドレット Get-ChildItems の動作を変更するには、 [Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドを上書きします。これにより、ユーザーは親項目の子項目を取得できるようになります。 (このサンプルでは、Get-ChildItems コマンドレットに動的パラメーターを追加する方法については説明しません)。
- コマンドレットのパラメーターが指定されている場合に Get-ChildItems コマンドレットの動作を変更するには、 [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames)メソッドを上書きします。 `Name`
- [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)メソッドを上書きしてコマンドレットの動作を変更し、 `New-Item` ユーザーがデータストアに項目を追加できるようにします。 (このサンプルでは、コマンドレットに動的パラメーターを追加する方法については説明しません `New-Item` )。
- [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)メソッドを上書きして、コマンドレットの動作を変更してい `Remove-Item` ます。 (このサンプルでは、コマンドレットに動的パラメーターを追加する方法については説明しません `Remove-Item` )。

## <a name="example"></a>例

このサンプルでは、項目をコピー、作成、および削除するために必要なメソッドと、親項目の子項目を取得するメソッドを上書きする方法を示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="11-1635":::

## <a name="see-also"></a>参照

[システムの................](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider)

[Containercmdletprovider (システム管理)](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider)

[システムの管理...。](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider)

[Windows PowerShell プロバイダーを設計する](./provider-types.md)
