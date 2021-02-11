---
ms.date: 09/13/2016
ms.topic: reference
title: PowerShell モジュールをインストールする
description: PowerShell モジュールをインストールする
ms.openlocfilehash: 3c7a4413168934ca4de1912c9615a6ae0fc45788
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645329"
---
# <a name="installing-a-powershell-module"></a>PowerShell モジュールをインストールする

PowerShell モジュールを作成した後は、システムにモジュールをインストールして、自分や他のユーザーが使用できるようにすることができます。 一般に、その手順はモジュール ファイル (つまり、.psm1 またはバイナリ アセンブリ、モジュール マニフェスト、その他の関連ファイル) をそのコンピューター上のディレクトリにコピーすることで構成されます。 非常に小さなプロジェクトでは、Windows エクスプローラーを使用して 1 台のリモート コンピューター上にファイルをコピーして貼り付けるだけで済みます。ただし、大規模なソリューションの場合は、より高度なインストール プロセスを使用することをお勧めします。 モジュールをシステムに導入する方法に関係なく、PowerShell ではさまざまな手法を使用してモジュールを検索し、使用することができます。 そのため、インストールに関する主な問題は、PowerShell でモジュールを検索できるようにすることです。 詳細については、「[PowerShell モジュールをインポートする](./importing-a-powershell-module.md)」をご覧ください。

## <a name="rules-for-installing-modules"></a>モジュールをインストールするための規則

以下の情報はすべてのモジュールに該当します。これには、自分で使用するために作成したモジュールや、他のパーティから取得したモジュール、他のユーザーに配布するモジュールなどが含まれます。

### <a name="install-modules-in-psmodulepath"></a>PSModulePath にモジュールをインストールする

可能な場合は常に、すべてのモジュールを **PSModulePath** 環境変数に記載されているパスにインストールするか、**PSModulePath** 環境変数の値にモジュールのパスを追加します。

**PSModulePath** 環境変数 ($Env:PSModulePath) には、Windows PowerShell モジュールの場所が含まれています。 コマンドレットでは、この環境変数の値に依存してモジュールが検索されます。

既定では、**PSModulePath** 環境変数の値には次のシステムおよびユーザー モジュールのディレクトリが含まれていますが、追加したり値を編集したりできます。

- `$PSHome\Modules` (%Windir%\System32\WindowsPowerShell\v1.0\Modules)

  > [!WARNING]
  > この場所は Windows に付属するモジュール用に予約されています。 この場所にモジュールをインストールしないでください。

- `$Home\Documents\WindowsPowerShell\Modules` (%UserProfile%\Documents\WindowsPowerShell\Modules)

- `$Env:ProgramFiles\WindowsPowerShell\Modules` (%ProgramFiles%\WindowsPowerShell\Modules)

  **PSModulePath** 環境変数の値を取得するには、次のいずれかのコマンドを使用します。

  ```powershell
  $Env:PSModulePath
  [Environment]::GetEnvironmentVariable("PSModulePath")
  ```

  **PSModulePath** 環境変数の値にモジュール パスを追加するには、次のコマンド形式を使用します。 この形式では、**System.Environment** クラスの **SetEnvironmentVariable** メソッドを使用して、**PSModulePath** 環境変数に対してセッションに依存しない変更を加えます。

  ```powershell
  #Save the current value in the $p variable.
  $p = [Environment]::GetEnvironmentVariable("PSModulePath")

  #Add the new path to the $p variable. Begin with a semi-colon separator.
  $p += ";C:\Program Files (x86)\MyCompany\Modules\"

  #Add the paths in $p to the PSModulePath value.
  [Environment]::SetEnvironmentVariable("PSModulePath",$p)

  ```

  > [!IMPORTANT]
  > **PSModulePath** にパスを追加したら、その変更に関する環境メッセージを配信する必要があります。 変更内容を配信することによって、シェルなどの他のアプリケーションで変更を取得できるようになります。 変更内容を配信するには、製品のインストール コードによって、`lParam` が文字列 "Environment" に設定された **WM_SETTINGCHANGE** メッセージを送信させます。 モジュールのインストール コードによって **PSModulePath** が更新された後にメッセージを送信してください。

### <a name="use-the-correct-module-directory-name"></a>正しいモジュール ディレクトリ名を使用する

適切な形式のモジュールは、モジュール ディレクトリ内の少なくとも 1 つのファイルのベース名と同じ名前を持つディレクトリに格納されているモジュールです。 モジュールの形式が適切でない場合は、Windows PowerShell によってモジュールとして認識されません。

ファイルの "ベース名" とは、ファイル名の拡張子を除いた名前です。 適切な形式のモジュールでは、モジュール ファイルを含むディレクトリの名前が、モジュール内の少なくとも 1 つのファイルのベース名と一致している必要があります。

たとえば、サンプルの Fabrikam モジュールでは、モジュール ファイルを含むディレクトリの名前は "Fabrikam" であり、少なくとも 1 つのファイルが "Fabrikam" というベース名を持っています。 この場合、Fabrikam.psd1 と Fabrikam.dll の両方が "Fabrikam" というベース名を持っています。

```
C:\Program Files
  Fabrikam Technologies
    Fabrikam Manager
      Modules
        Fabrikam
          Fabrikam.psd1 (module manifest)
          Fabrikam.dll (module assembly)

```

### <a name="effect-of-incorrect-installation"></a>不適切なインストールの影響

モジュールの形式が適切でなく、その場所が **PSModulePath** 環境変数の値に含まれていない場合、次のような Windows PowerShell の基本的な検出機能が機能しません。

- モジュールの自動読み込み機能で、モジュールを自動的にインポートすることができません。

- [Get-Module](/powershell/module/Microsoft.PowerShell.Core/Get-Module) コマンドレットの `ListAvailable` パラメーターでモジュールを見つけることができません。

- [Import-Module](/powershell/module/Microsoft.PowerShell.Core/Import-Module) コマンドレットでモジュールを見つけることができません。 モジュールをインポートするには、ルート モジュール ファイルまたはモジュール マニフェスト ファイルへの完全なパスを指定する必要があります。

  次のような追加機能は、モジュールがセッションにインポートされない限り機能しません。 **PSModulePath** 環境変数に含まれている適切な形式のモジュールでは、モジュールがセッションにインポートされていない場合でも、これらの機能は動作します。

- [Get-Command](/powershell/module/Microsoft.PowerShell.Core/Get-Command) コマンドレットでモジュール内のコマンドを見つけることができません。

- [Update-Help](/powershell/module/Microsoft.PowerShell.Core/Update-Help) および [Save-Help](/powershell/module/Microsoft.PowerShell.Core/Save-Help) コマンドレットで、モジュールのヘルプを更新または保存できません。

- [Show-Command](/powershell/module/Microsoft.PowerShell.Utility/Show-Command) コマンドレットで、モジュール内のコマンドを検索して表示することができません。

  モジュール内のコマンドは、Windows PowerShell Integrated Scripting Environment (ISE) の [`Show-Command`] ウィンドウに表示されません。

## <a name="where-to-install-modules"></a>モジュールをインストールする場所

このセクションでは、ファイル システム内のどこに Windows PowerShell モジュールをインストールするかについて説明します。 その場所はモジュールの使用方法によって異なります。

### <a name="installing-modules-for-a-specific-user"></a>特定のユーザー用にモジュールをインストールする

独自のモジュールを作成したり、Windows PowerShell コミュニティ Web サイトなどの他のパーティからモジュールを取得したりして、そのモジュールを自分のユーザー アカウントでのみ使用できるようにしたい場合は、ユーザー固有の Modules ディレクトリにモジュールをインストールします。

`$home\Documents\WindowsPowerShell\Modules\<Module Folder>\<Module Files>`

ユーザー固有の Modules ディレクトリは、既定で **PSModulePath** 環境変数の値に追加されます。

### <a name="installing-modules-for-all-users-in-program-files"></a>すべてのユーザー用に Program Files にモジュールをインストールする

コンピューター上のすべてのユーザー アカウントでモジュールを使用できるようにするには、Program Files の場所にモジュールをインストールします。

`$Env:ProgramFiles\WindowsPowerShell\Modules\<Module Folder>\<Module Files>`

> [!NOTE]
> Windows PowerShell 4.0 以降では、Program Files の場所は既定で PSModulePath 環境変数の値に追加されています。 以前のバージョンの Windows PowerShell では、Program Files の場所 (%ProgramFiles%\WindowsPowerShell\Modules) を手動で作成し、前述のように PSModulePath 環境変数にこのパスを追加することができます。

### <a name="installing-modules-in-a-product-directory"></a>製品ディレクトリにモジュールをインストールする

他のパーティにモジュールを配布する場合は、上で説明した既定の Program Files の場所を使用するか、%ProgramFiles% ディレクトリに対して独自の会社固有または製品固有のサブディレクトリを作成します。

たとえば、架空の企業である Fabrikam Technologies では、Fabrikam Manager 製品用の Windows PowerShell モジュールを提供しています。 そのモジュールのインストーラーによって、Fabrikam Manager 製品サブディレクトリ内に Modules サブディレクトリが作成されます。

```
C:\Program Files
  Fabrikam Technologies
    Fabrikam Manager
      Modules
        Fabrikam
          Fabrikam.psd1 (module manifest)
          Fabrikam.dll (module assembly)

```

Windows PowerShell のモジュール検出機能で Fabrikam モジュールを検索できるようにするために、Fabrikam モジュールのインストーラーによってモジュールの場所が **PSModulePath** 環境変数の値に追加されます。

```powershell
$p = [Environment]::GetEnvironmentVariable("PSModulePath")
$p += ";C:\Program Files\Fabrikam Technologies\Fabrikam Manager\Modules\"
[Environment]::SetEnvironmentVariable("PSModulePath",$p)
```

### <a name="installing-modules-in-the-common-files-directory"></a>Common Files ディレクトリにモジュールをインストールする

製品の複数のコンポーネント、または製品の複数のバージョンでモジュールが使用される場合は、%ProgramFiles%\Common Files\Modules サブディレクトリのモジュール固有のサブディレクトリにモジュールをインストールします。

次の例では、Fabrikam モジュールは `%ProgramFiles%\Common Files\Modules` サブディレクトリの Fabrikam サブディレクトリにインストールされます。 各モジュールが Modules サブディレクトリ内の独自のサブディレクトリ内にあることに注意してください。

```
C:\Program Files
  Common Files
    Modules
      Fabrikam
        Fabrikam.psd1 (module manifest)
        Fabrikam.dll (module assembly)
```

次に、インストーラーによって、**PSModulePath** 環境変数の値に Common Files の Modules サブディレクトリのパスが含まれていることが保証されます。

```powershell
$m = $env:ProgramFiles + '\Common Files\Modules'
$p = [Environment]::GetEnvironmentVariable("PSModulePath")
$q = $p -split ';'
if ($q -notContains $m) {
    $q += ";$m"
}
$p = $q -join ';'
[Environment]::SetEnvironmentVariable("PSModulePath", $p)
```

## <a name="installing-multiple-versions-of-a-module"></a>複数のバージョンのモジュールをインストールする

同じモジュールの複数のバージョンをインストールするには、次の手順に従います。

1. モジュールのバージョンごとにディレクトリを作成します。 ディレクトリ名にバージョン番号を含めます。
2. モジュールのバージョンごとにモジュール マニフェストを作成します。 マニフェストの **ModuleVersion** キーの値に、モジュールのバージョン番号を入力します。 マニフェスト ファイル (.psd1) をモジュールのバージョン固有のディレクトリに保存します。
3. 次の例に示すように、モジュールのルート フォルダーのパスを **PSModulePath** 環境変数の値に追加します。

エンド ユーザーは、[Import-Module](/powershell/module/Microsoft.PowerShell.Core/Import-Module) コマンドレットの `MinimumVersion` または `RequiredVersion` パラメーターを使用して、モジュールの特定のバージョンをインポートできます。

たとえば、Fabrikam モジュールのバージョン 8.0 と 9.0 を使用できる場合は、Fabrikam モジュールのディレクトリ構造は次のようになる可能性があります。

 ```
C:\Program Files
Fabrikam Manager
  Fabrikam8
    Fabrikam
      Fabrikam.psd1 (module manifest: ModuleVersion = "8.0")
      Fabrikam.dll (module assembly)
  Fabrikam9
    Fabrikam
      Fabrikam.psd1 (module manifest: ModuleVersion = "9.0")
      Fabrikam.dll (module assembly)
```

インストーラーによって、両方のモジュール パスが **PSModulePath** 環境変数の値に追加されます。

```powershell
$p = [Environment]::GetEnvironmentVariable("PSModulePath")
$p += ";C:\Program Files\Fabrikam\Fabrikam8;C:\Program Files\Fabrikam\Fabrikam9"
[Environment]::SetEnvironmentVariable("PSModulePath",$p)
```

これらの手順が完了すると、[Get-Module](/powershell/module/Microsoft.PowerShell.Core/Get-Module) コマンドレットの **ListAvailable** パラメーターによって、両方の Fabrikam モジュールが取得されます。 特定のモジュールをインポートするには、[Import-Module](/powershell/module/Microsoft.PowerShell.Core/Import-Module) コマンドレットの `MinimumVersion` または `RequiredVersion` パラメーターを使用します。

両方のモジュールが同じセッションにインポートされ、モジュールに同じ名前のコマンドレットが含まれている場合は、最後にインポートされたコマンドレットがそのセッションで有効になります。

## <a name="handling-command-name-conflicts"></a>コマンド名の競合を処理する

モジュールによってエクスポートされたコマンドがユーザーのセッション内のコマンドと同じ名前を持つ場合、コマンド名の競合が発生する可能性があります。

セッションに同じ名前を持つコマンドが 2 つ含まれている場合、Windows PowerShell では優先されるコマンドの種類が実行されます。 セッションに同じ名前と同じ種類を持つコマンドが 2 つ含まれている場合、Windows PowerShell では、最後にセッションに追加されたコマンドが実行されます。 既定では実行されないコマンドを実行する場合、ユーザーはコマンド名をモジュール名で修飾できます。

たとえば、セッションに `Get-Date` 関数と `Get-Date` コマンドレットが含まれている場合、Windows PowerShell では既定で関数が実行されます。 コマンドレットを実行するには、次のように、コマンドの前にモジュール名を付けます。

```powershell
Microsoft.PowerShell.Utility\Get-Date
```

名前の競合を防ぐために、モジュールの作成者はモジュール マニフェストの **DefaultCommandPrefix** キーを使用して、モジュールからエクスポートされるすべてのコマンドに対する名詞プレフィックスを指定できます。

ユーザーは、`Import-Module` コマンドレットの **Prefix** パラメーターを使用して代替プレフィックスを使用できます。 **Prefix** パラメーターの値は、**DefaultCommandPrefix** キーの値よりも優先されます。

## <a name="see-also"></a>参照

[about_Command_Precedence](/powershell/module/microsoft.powershell.core/about/about_command_precedence)

[Windows PowerShell モジュールを記述する](./writing-a-windows-powershell-module.md)
