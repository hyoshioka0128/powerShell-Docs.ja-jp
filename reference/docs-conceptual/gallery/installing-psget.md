---
ms.date: 09/19/2019
title: PowerShellGet のインストール
description: この記事では、さまざまなバージョンの PowerShell で PowerShellGet モジュールをインストールする方法について説明します。
ms.openlocfilehash: 06ec331446849784bb8464912fbce0e5a940823f
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92662150"
---
# <a name="installing-powershellget"></a>PowerShellGet のインストール

## <a name="powershellget-is-an-in-box-module-in-the-following-releases"></a>PowerShellGet は、以下のリリースではインボックス モジュールである

- [Windows 10](https://www.microsoft.com/windows) 以降
- [Windows Server 2016](/windows-server/windows-server) 以降
- [Windows Management Framework (WMF) 5.0](https://www.microsoft.com/download/details.aspx?id=50395) 以降
- [PowerShell 6](https://github.com/PowerShell/PowerShell/releases)

## <a name="get-the-latest-version-from-powershell-gallery"></a>PowerShell ギャラリーからの最新バージョンの取得

**PowerShellGet** を更新する前には、最新の **NuGet** プロバイダーをインストールする必要があります。 管理者特権の PowerShell セッションから次のコマンドを実行します。

```powershell
Install-PackageProvider -Name NuGet -Force
```

### <a name="for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget"></a>PowerShell 5.0 (またはそれ以降) のシステムには、最新の PowerShellGet をインストール可能

Windows 10、Windows Server 2016、WMF 5.0 または 5.1 がインストールされたシステム、PowerShell 6 を使用するシステムに PowerShellGet をインストールするには、管理者特権の PowerShell セッションから次のコマンドを実行します。

```powershell
Install-Module -Name PowerShellGet -Force
```

`Update-Module` を使用して新しいバージョンを取得します。

```powershell
Update-Module -Name PowerShellGet
Exit
```

### <a name="for-computers-running-powershell-30-or-powershell-40"></a>PowerShell 3.0 または PowerShell 4.0 を実行しているコンピューターの場合

この指示は、 **PackageManagement Preview** をインストールしているか、いかなるバージョンの **PowerShellGet** もインストールしていないコンピューターに適用されます。

`Save-Module` コマンドレットは、両方の指示セットで使用されます。 `Save-Module` では、モジュールと、登録リポジトリからの依存関係があれば、それがダウンロードされ、保存されます。 モジュールの最新版はローカル コンピューターの指定のパスに保存されますが、インストールはされません。 このモジュールを PowerShell 3.0 または 4.0 にインストールするには、モジュールが保存されたフォルダーを `$env:ProgramFiles\WindowsPowerShell\Modules` にコピーします。

詳細については、「[Save-Module](/powershell/module/PowershellGet/Save-Module)」を参照してください。

> [!NOTE]
> PowerShell 3.0 と PowerShell 4.0 では、1 つしかモジュールのバージョンをサポートしていません。 モジュールは PowerShell 5.0 以降、`<modulename>\<version>` にインストールされます。 これにより、複数のバージョンを共存させインストールできるようになります。 次の手順に示しているように、`Save-Module` を使用してモジュールをダウンロードしたら、`<modulename>\<version>` から、コピー先のコンピューターの `<modulename>` フォルダーにファイルをコピーする必要があります。

#### <a name="preparatory-step-on-computers-running-powershell-30"></a>PowerShell 3.0 を実行しているコンピューターでの準備手順

以下のセクションの手順では、ディレクトリ `$env:ProgramFiles\WindowsPowerShell\Modules` にモジュールをインストールします。
PowerShell 3.0 では、このディレクトリは既定で `$env:PSModulePath` にリストされないため、モジュールを自動読み込みするには、このディレクトリを追加する必要があります。

管理者特権の PowerShell セッションを開き、次のコマンドを実行します (今後のセッションで反映されます)。

```powershell
[Environment]::SetEnvironmentVariable(
  'PSModulePath',
  ((([Environment]::GetEnvironmentVariable('PSModulePath', 'Machine') -split ';') + "$env:ProgramFiles\WindowsPowerShell\Modules") -join ';'),
  'Machine'
)
```

#### <a name="computers-with-the-packagemanagement-preview-installed"></a>PackageManagement Preview がインストールされているコンピューター

> [!NOTE]
> PackageManagement Preview は、PowerShellGet を PowerShell バージョン 3 および 4 で使用できるようにするためのダウンロード可能なコンポーネントでしたが、現在は利用できなくなっています。
> これが特定のコンピューターにインストールされているかどうかをテストするには、`Get-Module -ListAvailable PowerShellGet` を実行します。

1. PowerShell セッションで `Save-Module` を使用して、現在のバージョンの **PowerShellGet** をダウンロードします。 2 つのフォルダーがダウンロードされます。 **PowerShellGet** と **PackageManagement** です。 各フォルダーには、バージョン番号付きのサブフォルダーが含まれています。

   ```powershell
   Save-Module -Name PowerShellGet -Path C:\LocalFolder -Repository PSGallery
   ```

1. **PowerShellGet** モジュールと **PackageManagement** モジュールが他のプロセスで確実に読み込まれていないようにします。

1. 管理者特権で PowerShell コンソールを再度開き、次のコマンドを実行します。

   ```powershell
   'PowerShellGet', 'PackageManagement' | % {
     $targetDir = "$env:ProgramFiles\WindowsPowerShell\Modules\$_"
     Remove-Item $targetDir\* -Recurse -Force
     Copy-Item C:\LocalFolder\$_\*\* $targetDir\ -Recurse -Force
   }
   ```

#### <a name="computers-without-powershellget"></a>PowerShellGet のないコンピューター

いかなるバージョンの **PowerShellGet** もインストールされていないコンピューターの場合 (`Get-Module -ListAvailable PowerShellGet` を使用してテストする)、モジュールをインストールするには、 **PowerShellGet** がインストールされているコンピューターが必要になります。

1. **PowerShellGet** がインストールされているコンピューターから、`Save-Module` を使用して **PowerShellGet** の最新版をダウンロードします。 2 つのフォルダーがダウンロードされます。 **PowerShellGet** と **PackageManagement** です。 各フォルダーには、バージョン番号付きのサブフォルダーが含まれています。

   ```powershell
   Save-Module -Name PowerShellGet -Path C:\LocalFolder -Repository PSGallery
   ```

1. **PowerShellGet** および **PackageManagement** フォルダー内の各 `<version>` サブフォルダーを、 **PowerShellGet** がインストールされていないコンピューターのフォルダー `$env:ProgramFiles\WindowsPowerShell\Modules\PowerShellGet\` と `$env:ProgramFiles\WindowsPowerShell\Modules\PackageManagement\` にそれぞれコピーします。これには、管理者特権でのセッションが必要です。

1. たとえば、他方のコンピューター (たとえば、`ws1`) のダウンロード フォルダーに、ターゲット コンピューターから UNC パス (たとえば、`\\ws1\C$\LocalFolder`) を使用してアクセスできる場合は、管理者特権で PowerShell コンソールを開き、次のコマンドを実行します。

   ```powershell
   'PowerShellGet', 'PackageManagement' | % {
     $targetDir = "$env:ProgramFiles\WindowsPowerShell\Modules\$_"
     $null = New-Item -Type Directory -Force $targetDir
     $fromComputer = 'ws1'  # Specify the name of the other computer here.
     Copy-Item \\$fromComputer\C$\LocalFolder\$_\*\* $targetDir -Recurse -Force
     if (-not (Get-ChildItem $targetDir)) { Throw "Copying failed." }
   }
   ```
