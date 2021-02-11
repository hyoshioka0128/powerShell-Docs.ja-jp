---
ms.date: 06/12/2017
title: NuGet のブートストラップ
description: この記事では、PowerShell ギャラリーの操作をサポートするために必要な NuGet コンポーネントをインストールする方法について説明します。
ms.openlocfilehash: eddbe15e7a765ad8b6df2b317a090269f06661d5
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661111"
---
# <a name="bootstrap-the-nuget-provider-and-nugetexe"></a>NuGet プロバイダーと NuGet.exe をブートストラップする

NuGet.exe は最新の NuGet プロバイダーには含まれていません。 モジュールまたはスクリプトのいずれかの公開操作には、バイナリ実行可能ファイルの **NuGet.exe** が PowerShellGet で必要です。 それ以外のすべての操作 ( **検索** 、 **インストール** 、 **保存** 、 **アンインストール** など) では、NuGet プロバイダーのみを必要とします。
PowerShellGet には、NuGet プロバイダーと NuGet.exe の結合ブートストラップ、または NuGet プロバイダーのみのブートストラップのいずれかを処理するロジックが含まれます。 どちらの場合も、プロンプト メッセージは 1 つしか表示されません。 マシンがインターネットに接続されていない場合、ユーザーまたは管理者は、切断されたマシンに NuGet プロバイダーまたは NuGet.exe ファイルの信頼されたインスタンスをコピーする必要があります。

> [!NOTE]
> バージョン 6 以降では、NuGet プロバイダーは PowerShell のインストールに含まれています。

## <a name="resolving-error-when-the-nuget-provider-has-not-been-installed-on-a-machine-that-is-internet-connected"></a>インターネット接続されているマシンに NuGet プロバイダーがインストールされていない場合のエラーを解決する

```powershell
Find-Module -Repository PSGallery -Verbose -Name Contoso
```

```Output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based
repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\user1\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet
provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you
want PowerShellGet to install and import the NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): n
Find-Module : NuGet provider is required to interact with NuGet-based repositories. Please ensure
that '2.8.5.201' or newer version of NuGet provider is installed.
At line:1 char:1
+ Find-Module -Repository PSGallery -Verbose -Name Contoso
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Find-Module], InvalidOperationException
   + FullyQualifiedErrorId : CouldNotInstallNuGetProvider,Find-Module
```

```powershell
Find-Module -Repository PSGallery -Verbose -Name Contoso
```

```Output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based
repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\user1\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet
provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you
want PowerShellGet to install and import the NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.

Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Contoso                             Module     PSGallery        Contoso module
```

## <a name="resolving-error-when-the-nuget-provider-is-available-and-nugetexe-is-not-available-during-the-publish-operation-on-a-machine-that-is-internet-connected"></a>インターネットに接続されているマシンで公開操作を行うときに NuGet プロバイダーは使用できるが NuGet.exe は使用できない場合のエラーを解決する

```powershell
Publish-Module -Name Contoso -Repository PSGallery -Verbose
```

```Output
NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGet.exe must
be available under one of the paths specified in PATH environment variable value. Do you want
PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): N
Publish-Module : NuGet.exe is required to interact with NuGet-based repositories. Please ensure
that NuGet.exe is available under one of the paths specified in PATH environment variable value.
At line:1 char:1
+ Publish-Module -Name Contoso -Repository PSGallery -Verbose
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Publish-Module], InvalidOperationException
    + FullyQualifiedErrorId : CouldNotInstallNuGetExe,Publish-Module
```

```powershell
Publish-Module -Name Contoso -Repository PSGallery -Verbose
```

```Output
NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGet.exe must
be available under one of the paths specified in PATH environment variable value. Do you want
PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://www.powershellgallery.com/api/v2/'.
Please allow few minutes for 'Contoso' to show up in the search results.
```

## <a name="resolving-error-when-both-nuget-provider-and-nugetexe-are-not-available-during-the-publish-operation-on-a-machine-that-is-internet-connected"></a>インターネットに接続されているマシンで公開操作を行うときに NuGet プロバイダーと NuGet.exe のどちらも使用できない場合のエラーを解決する

```powershell
Publish-Module -Name Contoso -Repository PSGallery -Verbose
```

```Output
NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with
the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.exe and NuGet provider
now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): N
Publish-Module : PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer
to interact with the NuGet-based repositories. Please ensure that '2.8.5.201' or newer version of
NuGet provider is installed and NuGet.exe is available under one of the paths specified in PATH
environment variable value.
At line:1 char:1
+ Publish-Module -Name Contoso -Repository PSGallery -Verbose
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Publish-Module], InvalidOperationException
    + FullyQualifiedErrorId : CouldNotInstallNuGetBinaries,Publish-Module
```

```powershell
Publish-Module -Name Contoso -Repository PSGallery -Verbose
```

```Output
NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with
the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.exe and NuGet provider
now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://www.powershellgallery.com/api/v2/'.
 Please allow few minutes for 'Contoso' to show up in the search results.
```

## <a name="manually-bootstrapping-the-nuget-provider-on-a-machine-that-is-not-connected-to-the-internet"></a>インターネットに接続されていないマシンで NuGet プロバイダーを手動でブートストラップする

上記のプロセスは、マシンがインターネットに接続されていて、共有の場所にあるファイルをダウンロードできることが前提となっています。 この前提条件を満たせない場合は、唯一の方法として、上記のプロセスに従ってマシンをブートストラップしてから、オフラインの信頼されたプロセスを使用して、分離されたノードにプロバイダーを手動でコピーします。 このシナリオで最も一般的なユース ケースは、分離された環境のサポートでプライベート ギャラリーが使用可能な場合です。

上記のプロセスに従ってインターネットに接続されたマシンをブートストラップすると、次の場所にプロバイダー ファイルが保存されます。

`C:\Program Files\PackageManagement\ProviderAssemblies\`

NuGet プロバイダーのフォルダーまたはファイルの構造は、次のようになります (バージョン番号は異なる場合があります)。

```
NuGet
--2.8.5.208
----Microsoft.PackageManagement.NuGetProvider.dll
```

信頼されたプロセスを使用して、これらのフォルダーとファイルをオフラインのマシンにコピーします。 オフライン マシンでプロバイダーを使用するには、それをインポートする必要があります。 オフライン マシン上で次のコマンドを実行します。

```powershell
Import-PackageProvider -Name NuGet
```

## <a name="manually-bootstrapping-nugetexe-to-support-publish-operations-on-a-machine-that-is-not-connected-to-the-internet"></a>手動で NuGet.exe をブートストラップしてインターネットに接続していないマシンでの公開操作をサポートする

NuGet.exe プロバイダーを手動でブートストラップするプロセスの他に、マシンで `Publish-Module` または `Publish-Script` コマンドレットを使用してプライベート ギャラリーにモジュールまたはスクリプトを公開する場合に、NuGet.exe のバイナリ実行可能ファイルが必要になります。

このシナリオで最も一般的なユース ケースは、分離された環境のサポートでプライベート ギャラリーが使用可能な場合です。 NuGet.exe ファイルを取得する方法は 2 つあります。

1 つは、インターネット接続されているマシンをブートストラップしてから、信頼されたプロセスを使用してオンラインのマシンにファイルをコピーする方法です。 インターネット接続されているマシンをブートストラップすると、次の 2 つのフォルダーのいずれかに NuGet.exe バイナリが配置されます。

- `Publish-Module` または `Publish-Script` コマンドレットが、昇格されたアクセス許可で (管理者として) 実行された場合:

   ```powershell
   $env:ProgramData\Microsoft\Windows\PowerShell\PowerShellGet
   ```

- 管理者特権のアクセス許可を持たないユーザーによってコマンドレットが実行された場合:

  ```powershell
  $env:userprofile\AppData\Local\Microsoft\Windows\PowerShell\PowerShellGet\
  ```

2 つ目は、次に示す NuGet.Org の Web サイトから NuGet.exe をダウンロードする方法です: [https://dist.nuget.org/index.html](https://www.nuget.org/downloads)。運用環境のマシンで使用する NugGet のバージョンを選択するときに、2.8.5.208 よりも新しく、"推奨" の表記があるバージョンであることを確認してください。 ブラウザーを使用してダウンロードした場合、ファイルのブロックを解除することを忘れないようにしてください。 この操作は `Unblock-File` コマンドレットを使用して実行できます。

どちらの場合も、NuGet.exe ファイルを `$env:path` の任意の場所にコピーできますが、通常は次の場所にコピーします。

- すべてのユーザーが `Publish-Module` と `Publish-Script` コマンドレットを使用できるように、実行可能ファイルを使用可能にする場合:

  ```powershell
  $env:ProgramData\Microsoft\Windows\PowerShell\PowerShellGet
  ```

- 特定のユーザーだけが実行可能ファイルを使用できるようにするには、対象ユーザーのプロファイル内の場所にのみコピーします。

  ```powershell
  $env:userprofile\AppData\Local\Microsoft\Windows\PowerShell\PowerShellGet\
  ```
