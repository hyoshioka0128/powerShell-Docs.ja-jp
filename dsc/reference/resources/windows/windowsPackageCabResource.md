---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC WindowsPackageCab リソース
ms.openlocfilehash: 035944e7ca5c7469250c48a19b79f2f2d5d38e9a
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076941"
---
# <a name="dsc-windowspackagecab-resource"></a>DSC WindowsPackageCab リソース

> 適用先:Windows PowerShell 5.1 以降

Windows PowerShell Desired State Configuration (DSC) の **WindowsPackageCab** リソースは、ターゲット ノードで Windows cabinet (.cab) パッケージをインストールまたはアンインストールするメカニズムを備えています。

ターゲット ノードには DISM PowerShell モジュールをインストールする必要があります。 詳細については、「[Use DISM in Windows PowerShell (Windows PowerShell の DISM の使用)](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/manufacture/desktop/use-dism-in-windows-powershell-s14)」を参照してください。


## <a name="syntax"></a>構文

```
{
    Name = [string]
    Ensure = [string] { Absent | Present }
    SourcePath = [string]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
}
```

## <a name="properties"></a>プロパティ

|  プロパティ  |  説明   |
|---|---|
| 名前| 特定の状態を保証するパッケージの名前を示します。|
| Ensure| パッケージがインストールされるかどうかを示します。 このプロパティを "Absent" に設定すると、パッケージはインストールされません (またはパッケージがインストールされている場合はアンインストールされます)。 パッケージがインストールされるようにするには、"Present" に設定します (既定値)。|
| パス| パッケージが存在するパスを示します。|
| LogPath| プロバイダーがパッケージをインストールまたはアンインストールするためのログ ファイルを保存する場所の完全パスを示します。|
| DependsOn | このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が **ResourceName** で、そのタイプが **ResourceType** である場合、このプロパティを使用する構文は DependsOn = "[ResourceType]ResourceName" になります。|

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