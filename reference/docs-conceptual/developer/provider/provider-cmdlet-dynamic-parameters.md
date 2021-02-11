---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット コマンドレットの動的パラメーター
description: コマンドレット コマンドレットの動的パラメーター
ms.openlocfilehash: ac05a847afb0729c34d733fa4ba8da11f46746fe
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92662978"
---
# <a name="provider-cmdlet-dynamic-parameters"></a>コマンドレット コマンドレットの動的パラメーター

プロバイダーは、ユーザーがコマンドレットのいずれかの静的パラメーターに特定の値を指定した場合に、プロバイダーコマンドレットに追加される動的パラメーターを定義できます。 たとえば、プロバイダーは、プロバイダーのコマンドレットを呼び出すときに、ユーザーが指定するパスに基づいて、さまざまな動的パラメーターを追加でき `Get-Item` `Set-Item` ます。

## <a name="dynamic-parameter-methods"></a>動的パラメーターメソッド

動的パラメーターは、 [Getitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItemDynamicParameters) および [Setitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItemDynamicParameters)s メソッドなどの動的パラメーターメソッドの1つを実装することによって定義されています。このメソッドについては、「」をご指定ください。 これらのメソッドは、スタンドアロンのコマンドレットと同様の属性で装飾されたパブリックプロパティを持つオブジェクトを返します。 次に示すのは、証明書プロバイダーから取得した [Getitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItemDynamicParameters) メソッドの実装例です。次に例を示します。

```csharp
protected override object GetItemDynamicParameters(string path)
{
    return new CertificateProviderDynamicParameters();
}
```

プロバイダーコマンドレットの静的パラメーターとは異なり、スタンドアロンのコマンドレットでパラメーターを定義するのと同じ方法で、これらのパラメーターの特性を指定できます。 証明書プロバイダーから取得した動的パラメータークラスの例を次に示します。

```csharp
internal sealed class CertificateProviderDynamicParameters
{
  /// <summary>
  /// Dynamic parameter the controls whether we only return
  /// code signing certs.
  /// </summary>
  [Parameter()]
  public SwitchParameter CodeSigningCert
  {
    get
    {
      {
        return codeSigningCert;
      }
    }

    set
    {
      {
        codeSigningCert = value;
      }
    }
  }

    private SwitchParameter codeSigningCert = new SwitchParameter();
}
```

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターの追加に使用できる静的パラメーターの一覧を次に示します。

`Clear-Content` コマンドレットを使用すると `Path` 、 [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.ClearContentDynamicParameters) メソッドを実装することによって、Clear-Clear コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。

`Clear-Item`コマンドレットを使用すると `Path` 、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。これを行う `Clear-Item` には、このメソッドを実装[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItemDynamicParameters)します。

`Clear-ItemProperty` コマンドレットを使用して、 `Path` `Clear-ItemProperty` [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearPropertyDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。

`Copy-Item` コマンドレットを使用して、 `Path` `Destination` `Recurse` `Copy-Item` [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItemDynamicParameters) メソッドを実装することで、コマンドレットの、、およびパラメーターによってトリガーされる動的パラメーターを定義できます。

Get-ChildItems コマンドレットを使用すると、 `Path` `Recurse` `Get-ChildItem` [Containercmdletprovider. Getchilditemsdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItemsDynamicParameters) メソッドと [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNamesDynamicParameters) メソッドを実装することによって、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義することができます。

`Get-Content` コマンドレットを使用すると、 `Path` `Get-Content` [Icontentcmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentReaderDynamicParameters) メソッドを実装することによって、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。

`Get-Item` コマンドレットを使用して、 `Path` `Get-Item` [Getitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItemDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。

`Get-ItemProperty` コマンドレットを使用して、 `Path` `Name` `Get-ItemProperty` [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetPropertyDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます。

`Invoke-Item`コマンドレットを使用すると `Path` 、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。これを行う `Invoke-Item` には、このメソッドを実装[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultActionDynamicParameters)します。

`Move-Item` コマンドレットを使用する `Path` と、 `Destination` コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます `Move-Item` [。これ](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItemDynamicParameters) を行うには、このメソッドを実装します。

`New-Item` コマンドレットを使用すると、 `Path` `ItemType` `Value` `New-Item` [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItemDynamicParameters) メソッドを実装することによって、コマンドレットの、、およびパラメーターによってトリガーされる動的パラメーターを定義できます。

`New-ItemProperty` コマンドレットを使用する `Path` `Name` と、 `PropertyType` `Value` `New-ItemProperty` [Idynamicpropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.NewPropertyDynamicParameters) メソッドを実装することによって、コマンドレットの、、、およびパラメーターによってトリガーされる動的パラメーターを定義できます。

`New-PSDrive`コマンドレットを使用して、 [](/dotnet/api/System.Management.Automation.PSDriveInfo) `New-PSDrive` [Drivecmdletprovider. Newdrivedynamicparameters *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDriveDynamicParameters)メソッドを実装することで、コマンドレットによって返されるSystem.Management.Automation.PSDriveinfo オブジェクトによってトリガーされる動的パラメーターを定義できます。

`Remove-Item``Path` `Recurse` `Remove-Item` [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItemDynamicParameters)メソッドを実装することによって、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます。

`Remove-ItemProperty``Path` `Name` `Remove-ItemProperty` [Idynamicpropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RemovePropertyDynamicParameters)メソッドを実装することによって、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます。

`Rename-Item` コマンドレットを使用して、 `Path` `NewName` `Rename-Item` [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItemDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます。

`Rename-ItemProperty``Path` `Name` `NewName` `Rename-ItemProperty` [Idynamicpropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider.RenamePropertyDynamicParameters)メソッドを実装することによって、コマンドレットの、、およびパラメーターによってトリガーされる動的パラメーターを定義することができます。

`Set-Content` コマンドレットを使用して、 `Path` `Set-Content` [Icontentcmdletprovider. Getcontentwriterdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider.GetContentWriterDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。

`Set-Item` コマンドレットを使用して、 `Path` `Value` `Set-Item` [Setitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItemDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます。

`Set-ItemProperty` コマンドレットを使用して、 `Path` `Value` `Set-Item` [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetPropertyDynamicParameters) メソッドを実装することで、コマンドレットのパラメーターとパラメーターによってトリガーされる動的パラメーターを定義できます。

`Test-Path`コマンドレットを使用すると `Path` 、コマンドレットのパラメーターによってトリガーされる動的パラメーターを定義できます。これを行う `Test-Path` には、このメソッドを実装[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultActionDynamicParameters)します。

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーを記述する](./writing-a-windows-powershell-provider.md)
