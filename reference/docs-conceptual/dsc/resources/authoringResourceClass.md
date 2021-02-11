---
ms.date: 07/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: PowerShell クラスを使用したカスタム DSC リソースの記述
description: この記事では、指定されたパス内のファイルを管理する単純なリソースを作成する方法について説明します。
ms.openlocfilehash: 72a828795c29e10ff66f164b8871b0fea7a1e0a8
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92667319"
---
# <a name="writing-a-custom-dsc-resource-with-powershell-classes"></a>PowerShell クラスを使用したカスタム DSC リソースの記述

> 適用先:Windows PowerShell 5.0

Windows PowerShell 5.0 の PowerShell クラスの導入により、クラスを作成して DSC リソースを定義できるようになりました。 クラスでは、スキーマとリソースの実装の両方を定義するため、MOF ファイルを別途作成する必要がありません。 **DSCResources** フォルダーが必要ないため、クラスベースのリソースのフォルダー構造は単純になりました。

クラスベースの DSC リソースでは、スキーマは、プロパティの型を指定する属性で変更できるクラスのプロパティとして定義されます。 リソースは、`Get()` 、`Set()` 、および `Test()` メソッド (スクリプト リソースの `Get-TargetResource`、`Set-TargetResource`、および `Test-TargetResource` 関数に相当します) によって実装されます。

この記事では、指定されたパス内のファイルを管理する **FileResource** という名前の単純なリソースを作成します。

DSC リソースの詳細については、「[カスタム Windows PowerShell Desired State Configuration のビルド](authoringResource.md)」をご覧ください。

> [!Note]
> クラスベースのリソースでは、汎用コレクションはサポートされていません。

## <a name="folder-structure-for-a-class-resource"></a>クラス リソースのフォルダー構造

PowerShell クラスを使用して DSC カスタム リソースを実装するには、次のフォルダー構造を作成します。
クラスは `MyDscResource.psm1` で定義し、モジュール マニフェストは `MyDscResource.psd1` で定義します。

```
$env:ProgramFiles\WindowsPowerShell\Modules (folder)
    |- MyDscResource (folder)
        MyDscResource.psm1
        MyDscResource.psd1
```

## <a name="create-the-class"></a>クラスの作成

PowerShell クラスを作成するには、class キーワードを使用します。 クラスを DSC リソースとして指定するには、`DscResource()` 属性を使用します。 クラスの名前は、DSC リソースの名前です。

```powershell
[DscResource()]
class FileResource {
}
```

### <a name="declare-properties"></a>プロパティの宣言

DSC リソースのスキーマは、クラスのプロパティとして定義します。 3 つのプロパティを次のように宣言します。

```powershell
[DscProperty(Key)]
[string]$Path

[DscProperty(Mandatory)]
[Ensure] $Ensure

[DscProperty(Mandatory)]
[string] $SourcePath

[DscProperty(NotConfigurable)]
[Nullable[datetime]] $CreationTime
```

属性によってプロパティが変更されることに注意してください。 属性の意味は次のとおりです。

- **DscProperty(Key)** :このプロパティは必須です。 プロパティはキーです。 キーとしてマークされたすべてのプロパティの値を組み合わせて、構成内のリソース インスタンスを一意に識別する必要があります。
- **DscProperty(Mandatory)** :このプロパティは必須です。
- **DscProperty(NotConfigurable)** :このプロパティは読み取り専用です。 この属性でマークされたプロパティは、構成で設定できませんが、`Get()` メソッド (存在する場合) によって設定されます。
- **DscProperty()** :このプロパティは構成可能ですが、必須ではありません。

`$Path` プロパティと `$SourcePath` プロパティは、両方とも文字列です。 `$CreationTime` は、[DateTime](/dotnet/api/system.datetime) プロパティです。 `$Ensure` プロパティは、次のように定義された列挙型です。

```powershell
enum Ensure
{
    Absent
    Present
}
```

### <a name="implementing-the-methods"></a>メソッドの実装

`Get()` 、`Set()` 、および `Test()` メソッドは、スクリプト リソースの `Get-TargetResource`、`Set-TargetResource`、および `Test-TargetResource` 関数に似ています。

このコードには、ファイルを `$SourcePath` から `$Path` にコピーするヘルパー関数である `CopyFile()` 関数も含まれています。

```powershell
    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)

        if ($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if ($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#
        This method is equivalent of the Test-TargetResource script function.
        It should return True or False, showing whether the resource
        is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if ($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ErrorAction Ignore

        if ($item -eq $null)
        {
            $present = $false
        }
        elseif ($item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif ($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }

        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if (-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = New-Object -TypeName System.IO.FileInfo($this.Path)

        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            <#
                Use CreateDirectory instead of New-Item to avoid code
                to handle the non-terminating error
            #>
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if (Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        # DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
```

### <a name="the-complete-file"></a>完全なファイル

完全なクラス ファイルは次のとおりです。

```powershell
enum Ensure
{
    Absent
    Present
}

<#
   This resource manages the file in a specific path.
   [DscResource()] indicates the class is a DSC resource
#>

[DscResource()]
class FileResource
{
    <#
       This property is the fully qualified path to the file that is
       expected to be present or absent.

       The [DscProperty(Key)] attribute indicates the property is a
       key and its value uniquely identifies a resource instance.
       Defining this attribute also means the property is required
       and DSC will ensure a value is set before calling the resource.

       A DSC resource must define at least one key property.
    #>
    [DscProperty(Key)]
    [string]$Path

    <#
        This property indicates if the settings should be present or absent
        on the system. For present, the resource ensures the file pointed
        to by $Path exists. For absent, it ensures the file point to by
        $Path does not exist.

        The [DscProperty(Mandatory)] attribute indicates the property is
        required and DSC will guarantee it is set.

        If Mandatory is not specified or if it is defined as
        Mandatory=$false, the value is not guaranteed to be set when DSC
        calls the resource.  This is appropriate for optional properties.
    #>
    [DscProperty(Mandatory)]
    [Ensure] $Ensure

    <#
       This property defines the fully qualified path to a file that will
       be placed on the system if $Ensure = Present and $Path does not
        exist.

       NOTE: This property is required because [DscProperty(Mandatory)] is
        set.
    #>
    [DscProperty(Mandatory)]
    [string] $SourcePath

    <#
       This property reports the file's create timestamp.

       [DscProperty(NotConfigurable)] attribute indicates the property is
       not configurable in DSC configuration.  Properties marked this way
       are populated by the Get() method to report additional details
       about the resource when it is present.

    #>
    [DscProperty(NotConfigurable)]
    [Nullable[datetime]] $CreationTime

    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)
        if ($this.ensure -eq [Ensure]::Present)
        {
            if (-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if ($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#
        This method is equivalent of the Test-TargetResource script function.
        It should return True or False, showing whether the resource
        is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if ($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ea Ignore
        if ($item -eq $null)
        {
            $present = $false
        }
        elseif ($item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif ($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }

        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if (-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            <#
                Use CreateDirectory instead of New-Item to avoid code
                 to handle the non-terminating error
            #>
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if (Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        # DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
} # This module defines a class for a DSC "FileResource" provider.
```

## <a name="create-a-manifest"></a>マニフェストの作成

クラスベースのリソースを DSC エンジンで使用できるようにするには、マニフェスト ファイルに、リソースをエクスポートするようにモジュールに指示する `DscResourcesToExport` ステートメントを含める必要があります。 この例では、マニフェストは次のようになります。

```powershell
@{

# Script module or binary module file associated with this manifest.
RootModule = 'MyDscResource.psm1'

DscResourcesToExport = 'FileResource'

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '81624038-5e71-40f8-8905-b1a87afe22d7'

# Author of this module
Author = 'Microsoft Corporation'

# Company or vendor of this module
CompanyName = 'Microsoft Corporation'

# Copyright statement for this module
Copyright = '(c) 2014 Microsoft. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '5.0'

# Name of the Windows PowerShell host required by this module
# PowerShellHostName = ''
}
```

## <a name="test-the-resource"></a>リソースのテスト

既に説明したように、クラスとマニフェスト ファイルをフォルダー構造で保存した後で、新しいリソースを使用する構成を作成できます。 DSC 構成を実行する方法については、「[構成の適用](../pull-server/enactingConfigurations.md)」をご覧ください。 次の構成では、`c:\test\test.txt` のファイルが存在するかどうかを確認し、存在しない場合は、ファイルを `c:\test.txt` からコピーします (構成を実行する前に `c:\test.txt` を作成する必要があります)。

```powershell
Configuration Test
{
    Import-DSCResource -module MyDscResource
    FileResource file
    {
        Path = "C:\test\test.txt"
        SourcePath = "c:\test.txt"
        Ensure = "Present"
    }
}
Test
Start-DscConfiguration -Wait -Force Test
```

## <a name="supporting-psdscrunascredential"></a>PsDscRunAsCredential のサポート

> [注] **PsDscRunAsCredential** は、PowerShell 5.0 以降でサポートされています。

**PsDscRunAsCredential** プロパティを [DSC 構成](../configurations/configurations.md)リソース ブロックで使用して、指定した資格情報のもとでリソースを実行する必要があることを指定できます。 詳細については、「[ユーザーの資格情報を指定して DSC を実行する](../configurations/runAsUser.md)」を参照してください。

### <a name="require-or-disallow-psdscrunascredential-for-your-resource"></a>使用するリソースに対する PsDscRunAsCredential の要求または却下

`DscResource()` 属性でオプションのパラメーター **RunAsCredential** を指定します。 このパラメーターには以下の 3 つの値のいずれかを指定します。

- `Optional` **PsDscRunAsCredential** は、このリソースを呼び出す構成ではオプションです。 これが既定値です。
- `Mandatory` **PsDscRunAsCredential** は、このリソースを呼び出す構成で使用する必要があります。
- `NotSupported`このリソースを呼び出す構成には **PsDscRunAsCredential** を使用できません。
- `Default``Optional` と同じです。

たとえば、次の属性を使用して、ご使用のカスタム リソースが **PsDscRunAsCredential** の使用をサポートしないことを指定します。

```powershell
[DscResource(RunAsCredential=NotSupported)]
class FileResource {
}
```

### <a name="declaring-multiple-class-resources-in-a-module"></a>1 つのモジュールで複数のクラス リソースを宣言する

1 つのモジュールで複数のクラスベースの DSC リソースを定義できます。 次の方法でフォルダー構造を作成できます。

1. `<ModuleName>.psm1` ファイルに最初のリソースを定義し、以降のリソースは **DSCResources** フォルダー以下に定義します。

   ```
   $env:ProgramFiles\WindowsPowerShell\Modules (folder)
        |- MyDscResource (folder)
           |- MyDscResource.psm1
              MyDscResource.psd1
        |- DSCResources
           |- SecondResource.psm1
   ```

1. すべてのリソースは、 **DSCResources** フォルダー以下で定義します。

   ```
   $env:ProgramFiles\WindowsPowerShell\Modules (folder)
        |- MyDscResource (folder)
           |- MyDscResource.psm1
              MyDscResource.psd1
        |- DSCResources
           |- FirstResource.psm1
              SecondResource.psm1
   ```

> [!NOTE]
> 上記の例で、 **DSCResource** 以下の PSM1 ファイルを PSD1 ファイルの **NestedModules** キーに追加します。

### <a name="access-the-user-context"></a>ユーザー コンテキストへのアクセス

カスタム リソース内からユーザー コンテキストにアクセスするには、自動変数 `$global:PsDscContext` を使用できます。

たとえば、次のコードは、リソースが詳細出力ストリームに実行しているユーザー コンテキストを記述します。

```powershell
if (PsDscContext.RunAsUser) {
    Write-Verbose "User: $global:PsDscContext.RunAsUser";
}
```

## <a name="see-also"></a>参照

[Build Custom Windows PowerShell Desired State Configuration Resources (カスタム Windows PowerShell Desired State Configuration のビルド)](authoringResource.md)
