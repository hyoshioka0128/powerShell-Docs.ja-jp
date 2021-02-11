---
ms.date: 09/13/2016
ms.topic: reference
title: AccessDBProviderSample05
description: AccessDBProviderSample05
ms.openlocfilehash: ad273720ded0b200530f4f81482d01a8fbb82aa3
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92656920"
---
# <a name="accessdbprovidersample05"></a>AccessDBProviderSample05

このサンプルでは、およびコマンドレットの呼び出しをサポートするためにコンテナーメソッドを上書きする方法を示し `Move-Item` `Join-Path` ます。 これらのメソッドは、ユーザーがコンテナー内で項目を移動する必要があり、データ ストアに入れ子状態のコンテナーが含まれる場合に実装する必要があります。 このサンプルのプロバイダークラス [は、system.servicemodel クラスの](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) クラスから派生しています。

## <a name="demonstrates"></a>対象

> [!IMPORTANT]
> プロバイダークラスは、ほとんどの場合、次のいずれかのクラスから派生し、他のプロバイダーインターフェイスを実装する可能性があります。
>
> - ...........[プロバイダー](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider)クラス。 「 [AccessDBProviderSample03](./accessdbprovidersample03.md)」を参照してください。
> - [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider) クラスを提供します。 「 [AccessDBProviderSample04](./accessdbprovidersample04.md)」を参照してください。
> - ...... [.。](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider)
>
> プロバイダーの機能に基づいて派生するプロバイダークラスを選択する方法の詳細については、「 [Windows PowerShell プロバイダーの設計](./provider-types.md)」を参照してください。

このサンプルでは、次の方法を示します。

- 属性を宣言 `CmdletProvider` しています。

- System.servicemodel クラスから [派生した](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) プロバイダークラスを定義していますが、

- このコマンド[](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem)レットの動作を変更し `Move-Item` 、ユーザーがある場所から別の場所に項目を移動できるようにするには、このメソッドを上書きします。 (このサンプルでは、コマンドレットに動的パラメーターを追加する方法については説明しません `Move-Item` )。

- コマンドレットの動作を変更するには、system.servicemodel[パス *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath)メソッドを上書きしています。 `Join-Path`

- [Isitemcontainer *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.IsItemContainer)メソッドを上書きしています。このメソッドは、

- System.object を上書きしています。 [Getchildname *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetChildName) メソッドです。

- [Getparentpath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetParentPath)メソッドを上書きしています。このメソッドは、

- [Normalizerelativepath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.NormalizeRelativePath)メソッドを上書きしています。このメソッドは、

## <a name="example"></a>例

このサンプルでは、Microsoft Access データベースで項目を移動するために必要なメソッドを上書きする方法を示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample05/AccessDBProviderSample05.cs" range="11-1960":::

## <a name="see-also"></a>参照

[システムの................](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider)

[Containercmdletprovider (システム管理)](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider)

[システムの管理...。](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider)

[Windows PowerShell プロバイダーを設計する](./provider-types.md)
