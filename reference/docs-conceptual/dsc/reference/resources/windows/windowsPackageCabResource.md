---
ms.date: 07/16/2020
ms.topic: reference
title: DSC WindowsPackageCab リソース
description: DSC WindowsPackageCab リソース
ms.openlocfilehash: 3ac10eb2a7da502b8cac23ab8bfee869a4e26fd3
ms.sourcegitcommit: 196c7f8cd24560cac70c88acc89909f17a86aea9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2020
ms.locfileid: "93143026"
---
# <a name="dsc-windowspackagecab-resource"></a>DSC WindowsPackageCab リソース

> 適用先:Windows PowerShell 5.1

Windows PowerShell Desired State Configuration (DSC) の **WindowsPackageCab** リソースは、ターゲット ノードで Windows cabinet (.cab) パッケージをインストールまたはアンインストールするメカニズムを備えています。

ターゲット ノードには DISM PowerShell モジュールをインストールする必要があります。 詳細については、「[Use DISM in Windows PowerShell (Windows PowerShell の DISM の使用)](/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)」を参照してください。

[!INCLUDE [Updated DSC Resources](../../../../../includes/dsc-resources.md)]

## <a name="syntax"></a>構文

```Syntax
{
    Name = [string]
    SourcePath = [string]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    Ensure = [string] { Absent | Present }
    [ PsDscRunAsCredential = [PSCredential] ]
}
```

## <a name="properties"></a>Properties

|プロパティ |説明 |
|---|---|
|名前 |特定の状態を保証するパッケージの名前を示します。 |
|SourcePath |パッケージが存在するパスを示します。 |
|LogPath |プロバイダーがパッケージをインストールまたはアンインストールするためのログ ファイルを保存する場所の完全パスを示します。 |

## <a name="common-properties"></a>共通プロパティ

|プロパティ |説明 |
|---|---|
|DependsOn |このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が ResourceName で、そのタイプが ResourceType である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。 |
|Ensure |パッケージがインストールされるかどうかを示します。 パッケージが確実にインストールされないようにする (またはパッケージがインストールされている場合はアンインストールされるようにする) には、このプロパティを **Absent** に設定します。 パッケージが確実にインストールされるようにするには、これを **Present** に設定します。 **WindowsPackageCab** リソースでは、 **Ensure** は必須のプロパティです。 |
|PsDscRunAsCredential |リソース全体を実行するための資格情報を設定します。 |

## <a name="example"></a>例

次の例の構成は入力パラメーターを必要とし、`$Name` パラメーターで指定された .cab ファイルが確実にインストールされます。

```powershell
Configuration Sample_WindowsPackageCab
{
    param
    (
        [Parameter (Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $Name,

        [Parameter (Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $SourcePath,

        [Parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $LogPath
    )

    Import-DscResource -ModuleName 'PSDscResources'

    WindowsPackageCab WindowsPackageCab1
    {
        Name = $Name
        Ensure = 'Present'
        SourcePath = $SourcePath
        LogPath = $LogPath
    }
}
```
