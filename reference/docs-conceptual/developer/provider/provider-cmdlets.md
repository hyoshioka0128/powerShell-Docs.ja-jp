---
ms.date: 09/12/2016
ms.topic: reference
title: プロバイダー コマンドレット
description: プロバイダー コマンドレット
ms.openlocfilehash: 522dacfe4d7190c12ea0de148fe83bf6b5fed10f
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92662895"
---
# <a name="provider-cmdlets"></a>プロバイダー コマンドレット

データストアを管理するためにユーザーが実行できるコマンドレットは、プロバイダーコマンドレットと呼ばれます。 これらのコマンドレットをサポートするには、基本プロバイダーのクラスおよびインターフェイスで定義されているいくつかのメソッドを上書きする必要があります。

## <a name="provider-cmdlets"></a>プロバイダーコマンドレット

ユーザーが実行できるプロバイダーコマンドレットを次に示します。

### <a name="psdrive-cmdlets"></a>PSDrive コマンドレット

- `Get-PSDrive`: このコマンドレットは、現在のセッションの Windows PowerShell ドライブを返します。 このコマンドレットをサポートするために、メソッドを上書きする必要はありません。

- `New-PSDrive`: このコマンドレットは、データストアにアクセスするための Windows PowerShell ドライブを作成することをユーザーに許可します。 このコマンドレットをサポートするには、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDrive) と Drivecmdletprovider の各メソッドを上書きします。このコマンドレットをサポートするには、 [Newdrivedynamicparameters](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDriveDynamicParameters) メソッドを上書きします。

- `Remove-PSDrive`: このコマンドレットを使用すると、データストアにアクセスする Windows PowerShell ドライブをユーザーが削除できます。 このコマンドレットをサポートするには、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.RemoveDrive) メソッドを上書きします。

### <a name="item-cmdlets"></a>項目のコマンドレット

- `Clear-Item`: このコマンドレットを使用すると、ユーザーはデータストア内の項目の値を削除できます。 このコマンドレットをサポートするには、次のように、 [system](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItem) .................... [..](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItemDynamicParameters)

- `Copy-Item`: このコマンドレットを使用すると、ユーザーは1つの場所から別の場所に項目をコピーできます。 このコマンドレットをサポートするには、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem) メソッドと [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItemDynamicParameters) メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Get-Item`: このコマンドレットは、ユーザーがデータストアからデータを取得できるようにします。 このコマンドレットをサポートするには、 [Getitemdynamicparameters](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItemDynamicParameters)メソッドと....................[メソッドを上書き](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem)します。

- `Get-ChildItem`: このコマンドレットは、ユーザーが親項目の子項目を取得できるようにします。 このコマンドレットをサポートするには、次のメソッドを上書きします。

  - [Containercmdletprovider. Getchilditems * を行います。](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)

  - [Containercmdletprovider. Getchilditemsdynamicparameters * を行います。](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItemsDynamicParameters)

  - [Containercmdletprovider. Getchildnames * のようになります。](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames)

  - [Containercmdletprovider のようにします。 Getchildnamesdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNamesDynamicParameters)

- `Invoke-Item`: このコマンドレットは、ユーザーがアイテムによって指定された既定のアクションを実行できるようにします。 このコマンドレットをサポートするには、 [Invokedefaultaction](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction) と [Invokedefaultaction](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction) の各メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Move-Item`: このコマンドレットを使用すると、ユーザーは1つの場所から別の場所に項目を移動できます。 このコマンドレットをサポートするには、次のように[して、](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItemDynamicParameters) [system.](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) ......................................。

- `New-ItemProperty`: このコマンドレットを使用すると、ユーザーはデータストアに新しい項目を作成できます。

- `Remove-Item`: このコマンドレットを使用すると、ユーザーはデータストアから項目を削除できます。 このコマンドレットをサポートするには、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem) メソッドと [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItemDynamicParameters) メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Rename-Item`: このコマンドレットを使用すると、ユーザーはデータストア内の項目の名前を変更できます。 このコマンドレットをサポートするには、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem) および [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItemDynamicParameters) の各メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Set-Item`: このコマンドレットは、ユーザーがデータストア内の項目の値を更新できるようにします。 このコマンドレットをサポートするには、 [Setitem](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) と [Setitemdynamicparameters](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItemDynamicParameters) の各メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

### <a name="item-content-cmdlets"></a>項目のコンテンツのコマンドレット

- `Add-Content`: このコマンドレットを使用すると、ユーザーはコンテンツを項目に追加できます。

- `Clear-Content`: このコマンドレットを使用すると、ユーザーは項目を削除せずに項目の内容を削除できます。 このコマンドレットをサポートするには、Icontentcmdletprovider メソッドと[Icontentcmdletprovider](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.ClearContentDynamicParameters)メソッドを上書きします。このコマンドレットの[内容](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.ClearContent)は次のようになります。

- `Get-Content`: このコマンドレットは、ユーザーが項目のコンテンツを取得できるようにします。 このコマンドレットをサポートするには、 [Icontentcmdletprovider](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReader) メソッドと Icontentcmdletprovider メソッドを上書きします。このコマンドレットは、メソッドと [パラメーター](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReaderDynamicParameters) メソッドを上書きします。 [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReader)メソッドは、コンテンツの読み取りに使用されるメソッドを定義する[Icontentreader](/dotnet/api/System.Management.Automation.Provider.IContentReader)インターフェイスを返します。このインターフェイスは、コンテンツの読み取りに使用されます。

- `Set-Content`: このコマンドレットを使用すると、ユーザーは項目のコンテンツを更新できます。 このコマンドレットをサポートするには、 [Icontentcmdletprovider](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriter) メソッドと Icontentcmdletprovider メソッドを上書きします。このコマンドレットをサポートするには、 [Getcontentwriterdynamicparameters](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriterDynamicParameters) メソッドを上書きします。 [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriter)メソッドは、コンテンツの書き込みに使用されるメソッドを定義する[Icontentwriter](/dotnet/api/System.Management.Automation.Provider.IContentWriter)インターフェイスを返します。このインターフェイスは、このインターフェイスを提供します。

### <a name="item-property-cmdlets"></a>Item プロパティのコマンドレット

- `Clear-ItemProperty`: このコマンドレットは、ユーザーがプロパティの値を削除できるようにします。 このコマンドレットをサポートするには、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty) と [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearPropertyDynamicParameters) の各メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Copy-ItemProperty`: このコマンドレットを使用すると、ユーザーはプロパティとその値を1つの場所から別の場所にコピーできます。 このコマンドレットをサポートするには、 [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.CopyProperty) メソッドと [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.CopyPropertyDynamicParameters) メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Get-ItemProperty`: このコマンドレットは、項目のプロパティを取得します。 このコマンドレットをサポートするには、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty) メソッドと [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetPropertyDynamicParameters) メソッドを上書きします。このコマンドレットについては、「」をご利用ください。

- `Move-ItemProperty`: このコマンドレットを使用すると、ユーザーはプロパティとその値を1つの場所から別の場所に移動できます。 このコマンドレットをサポートするには、 [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.MoveProperty) メソッドと [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.MovePropertyDynamicParameters) メソッドを上書きします。このコマンドレットについては、「」をご利用ください。

- `New-ItemProperty`: このコマンドレットは、新しいプロパティを作成し、その値を設定することをユーザーに許可します。 このコマンドレットをサポートするには、 [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.NewProperty) メソッドと [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.NewPropertyDynamicParameters) メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Remove-ItemProperty`: このコマンドレットを使用すると、ユーザーはプロパティとその値を削除できます。 このコマンドレットをサポートするには、 [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RemoveProperty) メソッドと [Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RemovePropertyDynamicParameters) メソッドを上書きします。このコマンドレットをサポートするには、次のようにします。

- `Rename-ItemProperty`: このコマンドレットを使用すると、ユーザーはプロパティの名前を変更できます。 このコマンドレットをサポートするには、Idynamicpropertycmdletprovider メソッドと Idynamicpropertycmdletprovider メソッドを上書きします。このコマンドレットは、[プロパティ](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RenameProperty)と[](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RenamePropertyDynamicParameters)をオーバーライドします。

- `Set-ItemProperty`: このコマンドレットを使用すると、ユーザーは項目のプロパティを更新できます。 このコマンドレットをサポートするには、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty) メソッドと [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetPropertyDynamicParameters) メソッドを上書きします。このコマンドレットについては、「」をご利用ください。

### <a name="location-cmdlets"></a>Location コマンドレット

- `Get-Location`: 現在の作業場所に関する情報を取得します。 このコマンドレットをサポートするために、メソッドを上書きする必要はありません。

- `Pop-Location`: このコマンドレットは、現在の場所を最後にスタックにプッシュした場所に変更します。 このコマンドレットをサポートするために、メソッドを上書きする必要はありません。

- `Push-Location`: このコマンドレットは、現在の場所を場所の一覧 ("スタック") の先頭に追加します。 このコマンドレットをサポートするために、メソッドを上書きする必要はありません。

- `Set-Location`: このコマンドレットは、現在の作業場所を指定された場所に設定します。 このコマンドレットをサポートするために、メソッドを上書きする必要はありません。

### <a name="path-cmdlets"></a>Path コマンドレット

- `Join-Path`: このコマンドレットを使用すると、ユーザーは親と子のパスセグメントを組み合わせて、プロバイダーの内部パスを作成できます。 このコマンドレットをサポートするには、次のように [して、](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath) system.string メソッドを上書きします。

- `Convert-Path`: このコマンドレットは、Windows PowerShell パスのパスを Windows PowerShell プロバイダーのパスに変換します。

- `Split-Path`: パスの指定された部分を返します。

- `Resolve-Path`: パスのワイルドカード文字を解決し、パスの内容を表示します。

- `Test-Path`: このコマンドレットは、パスのすべての要素が存在するかどうかを確認します。 このコマンドレットをサポートするには、 [システム](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists) の....................... [...](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExistsDynamicParameters) ................

### <a name="psprovider-cmdlets"></a>PSProvider コマンドレット

- `Get-PSProvider`: このコマンドレットは、セッションで使用可能なプロバイダーに関する情報を返します。 このコマンドレットをサポートするために、メソッドを上書きする必要はありません。
