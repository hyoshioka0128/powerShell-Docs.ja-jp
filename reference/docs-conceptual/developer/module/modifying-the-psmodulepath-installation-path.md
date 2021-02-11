---
ms.date: 09/13/2016
ms.topic: reference
title: PSModulePath インストール パスを変更する
description: PSModulePath インストール パスを変更する
ms.openlocfilehash: b802492bf9b49e8165e296817e3f80b9ae8265a6
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92661954"
---
# <a name="modifying-the-psmodulepath-installation-path"></a>PSModulePath インストール パスを変更する

環境変数には、 `PSModulePath` ディスクにインストールされているモジュールの場所へのパスが格納されます。 PowerShell では、モジュールへの完全パスをユーザーが指定しない場合、この変数を使用してモジュールを検索します。 この変数のパスは、表示されている順序で検索されます。

PowerShell を起動すると、 `PSModulePath` はシステム環境変数として作成されます。既定値は、windows の場合 `$HOME\Documents\PowerShell\Modules; $PSHOME\Modules` は、 `$HOME/.local/share/powershell/Modules: usr/local/share/powershell/Modules` Linux または Mac の場合は、 `$HOME\Documents\WindowsPowerShell\Modules; $PSHOME\Modules` windows powershell の場合はです。

> [!NOTE]
> ユーザー固有の **CurrentUser** の場所は、 `WindowsPowerShell\Modules` ユーザープロファイル内の **ドキュメント** の場所にあるフォルダーです。 その場所の特定のパスは、Windows のバージョンと、フォルダーリダイレクトを使用しているかどうかによって異なります。 既定では、Windows 10 の場合、その場所は `$HOME\Documents\WindowsPowerShell\Modules` です。

## <a name="to-view-the-psmodulepath-variable"></a>PSModulePath 変数を表示するには

変数に指定されているパスを表示するには、 `PSModulePath` 次のコマンドを入力します。

`$env:PSModulePath`

## <a name="to-add-locations-to-the-psmodulepath-variable"></a>PSModulePath 変数に場所を追加するには

この変数にパスを追加するには、次のいずれかの方法を使用します。

- 現在のセッションでのみ使用できる一時的な値を追加するには、コマンドラインで次のコマンドを実行します。

  `$env:PSModulePath = $env:PSModulePath + "$([System.IO.Path]::PathSeparator)$MyModulePath"`

- セッションが開かれるたびに使用できる永続的な値を追加するには、上記のコマンドを PowerShell プロファイルファイル () に追加し `$PROFILE` >

  プロファイルの詳細については、「[about_Profiles](/powershell/module/microsoft.powershell.core/about/about_profiles)」を参照してください。

- 永続変数をレジストリに追加するには、 `PSModulePath` [ **システムのプロパティ** ] ダイアログボックスの [環境変数エディター] を使用して、という名前の新しいユーザー環境変数を作成します。

- スクリプトを使用して永続変数を追加する[には、](/dotnet/api/system.environment) [SetEnvironmentVariable](/dotnet/api/system.environment.setenvironmentvariable)クラスの .net メソッドを使用します。 たとえば、次のスクリプトは、 `C:\Program Files\Fabrikam\Module` コンピューターの環境変数の値にパスを追加し `PSModulePath` ます。 ユーザー環境変数にパスを追加するには、 `PSModulePath` ターゲットを "user" に設定します。

  ```powershell
  $CurrentValue = [Environment]::GetEnvironmentVariable("PSModulePath", "Machine")
  [Environment]::SetEnvironmentVariable("PSModulePath", $CurrentValue + [System.IO.Path]::PathSeparator + "C:\Program Files\Fabrikam\Modules", "Machine")

  ```

## <a name="to-remove-locations-from-the-psmodulepath"></a>PSModulePath から場所を削除するには

同様のメソッドを使用して、変数からパスを削除できます。たとえば、 `$env:PSModulePath = $env:PSModulePath -replace "$([System.IO.Path]::PathSeparator)c:\\ModulePath"` は、現在のセッションから **C:\ モジュールパス** パスを削除します。

## <a name="see-also"></a>参照

[Windows PowerShell モジュールを記述する](./writing-a-windows-powershell-module.md)

[about_Modules](/powershell/module/microsoft.powershell.core/about/about_modules)
