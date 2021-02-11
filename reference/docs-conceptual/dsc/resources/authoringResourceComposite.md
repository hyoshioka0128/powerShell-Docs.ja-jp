---
ms.date: 07/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: 複合リソース - リソースとしての DSC 構成の使用
description: この記事では、複合リソースを作成および使用する方法について説明します。
ms.openlocfilehash: c1f0e3b45c3a393c04700b5a4bc88be365794820
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92667302"
---
# <a name="composite-resources-using-a-dsc-configuration-as-a-resource"></a>複合リソース: リソースとしての DSC 構成の使用

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

実際の状況では、構成は多くのさまざまなリソースを呼び出したり、膨大な数のプロパティを設定したりするため、長く複雑になることがあります。 このような複雑さに対処するために、Windows PowerShell Desired State Configuration (DSC) 構成を他の構成のリソースとして使用できます。 これは、複合リソースと呼ばれます。 複合リソースは、パラメーターを受け取る DSC 構成です。 構成のパラメーターは、リソースのプロパティとして機能します。
構成は、拡張子が `.schema.psm1` のファイルとして保存されます。 MOF スキーマと、一般的な DSC リソースのリソース スクリプトの両方の代わりになります。 DSC リソースの詳細については、[Windows PowerShell Desired State Configuration](resources.md) に関する記事を参照してください。

## <a name="creating-the-composite-resource"></a>複合リソースの作成

この例では、仮想マシンを構成するために多くの既存のリソースを呼び出す構成を作成します。 構成ブロックで設定する値を指定する代わりに、構成ではパラメーターが受け取られ、後でそれが構成ブロックで使用されます。

```powershell
Configuration xVirtualMachine
{
    param
    (
        # Name of VMs
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String[]] $VMName,

        # Name of Switch to create
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $SwitchName,

        # Type of Switch to create
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $SwitchType,

        # Source Path for VHD
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VHDParentPath,

        # Destination path for diff VHD
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VHDPath,

        # Startup Memory for VM
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VMStartupMemory,

        # State of the VM
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VMState
    )

    # Import the module that defines custom resources
    Import-DscResource -Module xComputerManagement,xHyper-V

    # Install the Hyper-V role
    WindowsFeature HyperV
    {
        Ensure = "Present"
        Name = "Hyper-V"
    }

    # Create the virtual switch
    xVMSwitch $SwitchName
    {
        Ensure = "Present"
        Name = $SwitchName
        Type = $SwitchType
        DependsOn = "[WindowsFeature]HyperV"
    }

    # Check for Parent VHD file
    File ParentVHDFile
    {
        Ensure = "Present"
        DestinationPath = $VHDParentPath
        Type = "File"
        DependsOn = "[WindowsFeature]HyperV"
    }

    # Check the destination VHD folder
    File VHDFolder
    {
        Ensure = "Present"
        DestinationPath = $VHDPath
        Type = "Directory"
        DependsOn = "[File]ParentVHDFile"
    }

    # Create VM specific diff VHD
    foreach ($Name in $VMName)
    {
        xVHD "VHD$Name"
        {
            Ensure = "Present"
            Name = $Name
            Path = $VHDPath
            ParentPath = $VHDParentPath
            DependsOn = @("[WindowsFeature]HyperV",
                          "[File]VHDFolder")
        }
    }

    # Create VM using the above VHD
    foreach($Name in $VMName)
    {
        xVMHyperV "VMachine$Name"
        {
            Ensure = "Present"
            Name = $Name
            VhDPath = (Join-Path -Path $VHDPath -ChildPath $Name)
            SwitchName = $SwitchName
            StartupMemory = $VMStartupMemory
            State = $VMState
            MACAddress = $MACAddress
            WaitForIP = $true
            DependsOn = @("[WindowsFeature]HyperV",
                          "[xVHD]VHD$Name")
        }
    }
}
```

> [!NOTE]
> 現在、DSC では、複合リソースまたは入れ子になった構成を複合リソース内に配置することはサポートされていません。

### <a name="saving-the-configuration-as-a-composite-resource"></a>複合リソースとしての構成の保存

パラメーター化された構成を DSC リソースとして使用するには、他の MOF ベースのリソースに似たディレクトリ構造で保存し、`.schema.psm1` 拡張子を持つ名前を指定します。 この例では、ファイルに `xVirtualMachine.schema.psm1` という名前を付けます。 次の行を含む `xVirtualMachine.psd1` というマニフェストも作成する必要があります。

```powershell
RootModule = 'xVirtualMachine.schema.psm1'
```

> [!NOTE]
> これは、`MyDscResources` フォルダーにあるすべてのリソースのモジュール マニフェストである `MyDscResources.psd1` に追加されます。

完了すると、フォルダー構造は次のようになります。

```
$env: psmodulepath
    |- MyDscResources
        |- MyDscResources.psd1
        |- DSCResources
            |- xVirtualMachine
                |- xVirtualMachine.psd1
                |- xVirtualMachine.schema.psm1
```

これで、リソースは `Get-DscResource` コマンドレットを使用して検出できるようになり、そのプロパティは、そのコマンドレットまたは Windows PowerShell ISE の <kbd>Ctrl</kbd> + <kbd>Space</kbd> キーによるオートコンプリートを使用して検出できるようになります。

## <a name="using-the-composite-resource"></a>複合リソースの使用

次に、複合リソースを呼び出す構成を作成します。 この構成は、xVirtualMachine 複合リソースを呼び出して仮想マシンを作成してから、 **xComputer** リソースを呼び出して名前を変更します。

```powershell
configuration RenameVM
{
    Import-DscResource -Module xVirtualMachine
    Node localhost
    {
        xVirtualMachine VM
        {
            VMName = "Test"
            SwitchName = "Internal"
            SwitchType = "Internal"
            VhdParentPath = "C:\Demo\VHD\RTM.vhd"
            VHDPath = "C:\Demo\VHD"
            VMStartupMemory = 1024MB
            VMState = "Running"
        }
    }

    Node "192.168.10.1"
    {
        xComputer Name
        {
            Name = "SQL01"
            DomainName = "fourthcoffee.com"
        }
    }
}
```

このリソースを使用して複数の VM を作成することもできます。具体的には VM 名の配列を xVirtualMachine リソースに渡します。

```PowerShell
Configuration MultipleVms
{
    Import-DscResource -Module xVirtualMachine
    Node localhost
    {
        xVirtualMachine VMs
        {
            VMName = "IIS01", "SQL01", "SQL02"
            SwitchName = "Internal"
            SwitchType = "Internal"
            VhdParentPath = "C:\Demo\VHD\RTM.vhd"
            VHDPath = "C:\Demo\VHD"
            VMStartupMemory = 1024MB
            VMState = "Running"
        }
    }
}
```

## <a name="supporting-psdscrunascredential"></a>PsDscRunAsCredential のサポート

> [!NOTE]
> **PsDscRunAsCredential** は PowerShell 5.0 以降でサポートされています。

**PsDscRunAsCredential** プロパティを [DSC 構成](../configurations/configurations.md)リソース ブロックで使用して、指定した資格情報のもとでリソースを実行する必要があることを指定できます。 詳細については、「[ユーザーの資格情報を指定して DSC を実行する](../configurations/runAsUser.md)」を参照してください。

カスタム リソース内からユーザー コンテキストにアクセスするには、自動変数 `$PsDscContext` を使用できます。

たとえば、次のコードでは、リソースが実行されているユーザー コンテキストが、詳細出力ストリームに書き込まれます。

```powershell
if ($PsDscContext.RunAsUser) {
    Write-Verbose "User: $PsDscContext.RunAsUser";
}
```

## <a name="see-also"></a>参照

### <a name="concepts"></a>概念

- [MOF を使用したカスタム DSC リソースの記述](authoringResourceMOF.md)
- [Windows PowerShell Desired State Configuration の概要](../overview/overview.md)
