---
description: PowerShell モジュールをインストール、インポート、および使用する方法について説明します。
Locale: en-US
ms.date: 12/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_modules?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Modules
ms.openlocfilehash: aebebc3f41a091151fbbecd9925a4ebc063e678e
ms.sourcegitcommit: 7b376314e7640c39a53aac9f0db8bb935514a960
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2020
ms.locfileid: "96564604"
---
# <a name="about-modules"></a>モジュールについて

## <a name="short-description"></a>簡単な説明
PowerShell モジュールをインストール、インポート、および使用する方法について説明します。

## <a name="long-description"></a>長い説明

モジュールは、コマンドレット、プロバイダー、関数、ワークフロー、変数、エイリアスなどの PowerShell メンバーを含むパッケージです。

コマンドを記述するとき、モジュールを利用してコマンドを整理したり、他者と共有したりできます。 モジュールを受け取るユーザーは、モジュール内のコマンドを PowerShell セッションに追加し、組み込みコマンドと同じように使用できます。

このトピックでは、PowerShell モジュールの使用方法について説明します。 PowerShell モジュールの作成方法の詳細については、「 [Powershell モジュールの記述](/powershell/scripting/developer/module/writing-a-windows-powershell-module)」を参照してください。

## <a name="what-is-a-module"></a>モジュールとは

モジュールは、コマンドレット、プロバイダー、関数、ワークフロー、変数、エイリアスなどの PowerShell メンバーを含むパッケージです。 このパッケージのメンバーは、PowerShell スクリプト、コンパイルされた DLL、またはその両方の組み合わせで実装できます。 これらのファイルは通常、1つのディレクトリにまとめられます。 詳細については、SDK ドキュメントの「 [Windows PowerShell モジュールについ](/powershell/scripting/developer/module/understanding-a-windows-powershell-module) て」を参照してください。

## <a name="module-auto-loading"></a>モジュールの自動読み込み

PowerShell 3.0 以降では、インストールされているモジュールで任意のコマンドを初めて実行したときに、PowerShell は自動的にモジュールをインポートします。 今後は設定やプロファイル構成なしでモジュールのコマンドを使用できます。そのため、コンピューターにモジュールをインストールした後はモジュールを管理する必要がありません。

また、モジュールのコマンドが見つけやすくなりました。 `Get-Command`コマンドレットは、インストールされているすべてのモジュールのすべてのコマンドを取得するようになりました。セッションにまだ含まれていない場合でも同様です。 最初にモジュールをインポートしなくても、コマンドを見つけて使用することができます。

次の各例では、を含む CimCmdlets モジュールが `Get-CimInstance` セッションにインポートされます。

- コマンドを実行します。

  ```powershell
  Get-CimInstance Win32_OperatingSystem
  ```

- コマンドを取得する

  ```powershell
  Get-Command Get-CimInstance
  ```

- コマンドのヘルプを表示する

  ```powershell
  Get-Help Get-CimInstance
  ```

`Get-Command` ワイルドカード文字 () を含むコマンド `*` は、検出用であり、使用しないと見なされ、モジュールをインポートしません。

PSModulePath 環境変数によって指定された場所に格納されているモジュールのみが自動的にインポートされます。 他の場所のモジュールは、コマンドレットを実行してインポートする必要があり `Import-Module` ます。

また、PowerShell プロバイダーを使用するコマンドでは、モジュールは自動的にインポートされません。 たとえば、コマンドレットなど、WSMan: ドライブを必要とするコマンドを使用する場合は、コマンドレットを実行して、 `Get-PSSessionConfiguration` `Import-Module` ドライブを含む **Microsoft の wsman. 管理** モジュールをインポートする必要があり `WSMan:` ます。

コマンドを実行し `Import-Module` てモジュールをインポートし、変数を使用して `$PSModuleAutoloadingPreference` モジュールの自動インポートを有効化、無効化、および構成することもできます。 詳細については、「 [about_Preference_Variables](about_Preference_Variables.md)」を参照してください。

## <a name="how-to-use-a-module"></a>モジュールを使用する方法

モジュールを使用するには、次のタスクを実行します。

1. モジュールをインストールします。 (多くの場合、これは自動的に行われます。)
1. モジュールにより追加されたコマンドを見つけます。
1. モジュールにより追加されたコマンドを使用します。

このトピックでは、これらのタスクの実行方法について説明します。 モジュールの管理に関して役に立つその他の情報も含まれています。

## <a name="how-to-install-a-module"></a>モジュールをインストールする方法

ファイルが含まれているフォルダーとしてモジュールを受け取った場合は、PowerShell で使用する前に、コンピューターにインストールする必要があります。

ほとんどのモジュールは自動的にインストールされます。 PowerShell には、いくつかのプレインストールモジュール ( _コア_ モジュールと呼ばれることもあります) が付属しています。 Windows ベースのコンピューターでは、オペレーティングシステムに含まれている機能に、それらを管理するためのコマンドレットがある場合、これらのモジュールはプレインストールされています。 Windows の機能をインストールすると、たとえば、サーバーマネージャーの役割と機能の追加ウィザード、コントロールパネルの [Windows の機能の有効化または無効化] ダイアログボックス、機能の一部である PowerShell モジュールがインストールされます。 他にも多くのモジュールがモジュールをインストールするインストーラーまたは設定プログラムで提供されます。

次のコマンドを使用して、現在のユーザーの **モジュール** ディレクトリを作成します。

```powershell
New-Item -Type Directory -Path $HOME\Documents\PowerShell\Modules
```

モジュール フォルダー全体をモジュール ディレクトリにコピーします。 Windows Explorer と Cmd.exe、PowerShell を含むフォルダーをコピーするには、任意の方法を使用できます。 PowerShell では、 `Copy-Item` コマンドレットを使用します。 たとえば、MyModule フォルダーをから Modules ディレクトリにコピーするには、次のように `C:\ps-test\MyModule` 入力します。

```powershell
Copy-Item -Path C:\ps-test\MyModule -Destination `
    $HOME\Documents\PowerShell\Modules
```

モジュールはどこにインストールしても構いませんが、モジュールの既定の場所にモジュールをインストールすれば、管理しやすくなります。 既定のモジュールの場所の詳細については、 [モジュールと DSC リソースの場所、および PSModulePath](#module-and-dsc-resource-locations-and-psmodulepath) セクションを参照してください。

## <a name="how-to-find-installed-modules"></a>インストールされているモジュールを検索する方法

モジュールの既定の場所にインストールしたが、まだセッションにインポートしていないモジュールを見つけるには、次のように入力します。

```powershell
Get-Module -ListAvailable
```

セッションに既にインポートされているモジュールを見つけるには、PowerShell プロンプトで次のように入力します。

```powershell
Get-Module
```

コマンドレットの詳細につい `Get-Module` ては、「 [Get-Module](xref:Microsoft.PowerShell.Core.Get-Module)」を参照してください。

## <a name="how-to-find-the-commands-in-a-module"></a>モジュール内のコマンドを検索する方法

使用 `Get-Command` 可能なすべてのコマンドを検索するには、コマンドレットを使用します。 コマンドレットのパラメーターを使用し `Get-Command` て、モジュール、名前、名詞などのコマンドをフィルター処理できます。

モジュールのすべてのコマンドを見つけるには、次のように入力します。

```powershell
Get-Command -Module <module-name>
```

たとえば、BitsTransfer モジュール内のコマンドを検索するには、次のように入力します。

```powershell
Get-Command -Module BitsTransfer
```

コマンドレットの詳細につい `Get-Command` ては、「 [Get-Command](xref:Microsoft.PowerShell.Core.Get-Command)」を参照してください。

## <a name="how-to-get-help-for-the-commands-in-a-module"></a>モジュール内のコマンドのヘルプを取得する方法

モジュールにエクスポートするコマンドのヘルプファイルが含まれている場合は、コマンドレットによって `Get-Help` ヘルプトピックが表示されます。 `Get-Help`PowerShell で任意のコマンドのヘルプを取得するために使用するのと同じコマンド形式を使用します。

PowerShell 3.0 以降では、モジュールのヘルプファイルをダウンロードし、ヘルプファイルに更新プログラムをダウンロードして、互換性が残されていないようにすることができます。

モジュールのコマンドのヘルプを取得するには、次のように入力します。

```powershell
Get-Help <command-name>
```

モジュール内のコマンドのヘルプをオンラインで取得するには、次のように入力します。

```powershell
Get-Help <command-name> -Online
```

モジュール内のコマンドのヘルプファイルをダウンロードしてインストールするには、次のように入力します。

```powershell
Update-Help -Module <module-name>
```

詳細については、「 [get-help](xref:Microsoft.PowerShell.Core.Get-Help) and [update-help](xref:Microsoft.PowerShell.Core.Update-Help)」を参照してください。

## <a name="how-to-import-a-module"></a>モジュールをインポートする方法

場合によっては、モジュールまたはモジュール ファイルをインポートする必要があります。 インポートは、 **PSModulePath** 環境変数で指定された場所にモジュールがインストールされていない場合、 `$env:PSModulePath` またはフォルダーとして配信される一般的なモジュールではなく、.dll や hbase-runner.psm1 ファイルなどのファイルでモジュールが構成されている場合に必要です。

また、コマンドのパラメーターを使用できるように、モジュールをインポートすることもできます。これにより、インポートされた `Import-Module` すべてのコマンドの名詞名に特徴的なプレフィックスが追加されます。または、 **NoClobber** パラメーターによって、セッション内の既存のコマンドを非表示にしたり、置き換えたりするコマンドをモジュールで追加できなくなります。

モジュールをインポートするには、コマンドレットを使用し `Import-Module` ます。

場所 PSModulePath にあるモジュールを現在のセッションにインポートするには、次のコマンド形式を使用します。

```powershell
Import-Module <module-name>
```

たとえば、次のコマンドは、BitsTransfer モジュールを現在のセッションにインポートします。

```powershell
Import-Module BitsTransfer
```

モジュールの既定の場所にないモジュールをインポートするには、コマンドでモジュール フォルダーの完全修飾パスを使用します。

たとえば、ディレクトリの TestCmdlets モジュールをセッションに追加するには、次のように `C:\ps-test` 入力します。

```powershell
Import-Module C:\ps-test\TestCmdlets
```

モジュール フォルダーに含まれていないモジュール ファイルをインポートするには、コマンドでモジュール ファイルの完全修飾パスを使用します。

たとえば、ディレクトリ内の TestCmdlets.dll モジュールをセッションに追加するには、次のように `C:\ps-test` 入力します。

```powershell
Import-Module C:\ps-test\TestCmdlets.dll
```

セッションにモジュールを追加する方法の詳細については、「 [Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)」を参照してください。

## <a name="how-to-import-a-module-into-every-session"></a>モジュールをすべてのセッションにインポートする方法

コマンドは、 `Import-Module` 現在の PowerShell セッションにモジュールをインポートします。 開始するすべての PowerShell セッションにモジュールをインポートするには、 `Import-Module` powershell プロファイルにコマンドを追加します。

プロファイルの詳細については、「[about_Profiles](about_Profiles.md)」を参照してください。

## <a name="how-to-remove-a-module"></a>モジュールを削除する方法

モジュールを削除すると、モジュールが追加したコマンドがセッションから削除されます。

セッションからモジュールを削除するには、次のコマンド形式を使用します。

```powershell
Remove-Module <module-name>
```

たとえば、次のコマンドは、現在のセッションから BitsTransfer モジュールを削除します。

```powershell
Remove-Module BitsTransfer
```

モジュールを削除では、モジュールのインポートの反対の操作が行われます。 モジュールを削除してもモジュールはアンインストールされません。 詳細については、「 [Remove-Module](xref:Microsoft.PowerShell.Core.Remove-Module)」を参照してください。

## <a name="module-and-dsc-resource-locations-and-psmodulepath"></a>モジュールと DSC リソースの場所、および PSModulePath

環境変数には、 `$env:PSModulePath` モジュールとリソースを検索するために検索されるフォルダーの場所の一覧が含まれています。

既定では、に割り当てられて `$env:PSModulePath` いる有効な場所は次のとおりです。

- システム全体の場所: `$PSHOME\Modules`

  これらのフォルダーには、Windows と PowerShell に付属しているモジュールが含まれています。

  PowerShell に含まれる DSC リソースは、フォルダーに格納され `$PSHOME\Modules\PSDesiredStateConfiguration\DSCResources` ます。

- ユーザー固有のモジュール: ユーザーによってユーザーのスコープにインストールされたモジュールです。 `Install-Module` には、現在のユーザーまたはすべてのユーザーに対してモジュールがインストールされているかどうかを指定できる **スコープ** パラメーターがあります。 詳細については、「 [Install-Module](xref:PowerShellGet.Install-Module)」を参照してください。

  Windows 上のユーザー固有の **CurrentUser** の場所は、 `PowerShell\Modules` ユーザープロファイル内の **ドキュメント** の場所にあるフォルダーです。 その場所の特定のパスは、Windows のバージョンと、フォルダーリダイレクトを使用しているかどうかによって異なります。 Microsoft OneDrive では、 **ドキュメント** フォルダーの場所を変更することもできます。

  既定では、Windows 10 の場合、その場所は `$HOME\Documents\PowerShell\Modules` です。 Linux または Mac では、 **CurrentUser** の場所は `$HOME/.local/share/powershell/Modules` です。

  > [!NOTE]
  > 次のコマンドを使用して、 **ドキュメント** フォルダーの場所を確認でき `[Environment]::GetFolderPath('MyDocuments')` ます。

- **AllUsers** の場所は `$env:PROGRAMFILES\PowerShell\Modules` Windows 上にあります。 Linux または Mac では、モジュールはに格納され `/usr/local/share/powershell/Modules` ます。

> [!NOTE]
> ディレクトリ内のファイルを追加または変更するには、 `$env:Windir\System32` [ **管理者として実行** ] オプションを使用して PowerShell を起動します。

**PSModulePath** 環境変数の値を変更することで、システム上の既定のモジュールの場所を変更でき `$Env:PSModulePath` ます。 **PSModulePath** 環境変数は Path 環境変数でモデル化されており、同じ形式です。

モジュールの既定の場所を表示するには、次のように入力します。

```powershell
$Env:PSModulePath
```

モジュールの既定の場所を追加するには、次のコマンド形式を使用します。

```powershell
$Env:PSModulePath = $Env:PSModulePath + ";<path>"
```

コマンドのセミコロン () は、 `;` 新しいパスをリスト内でその前にあるパスと分離します。

たとえば、ディレクトリを追加するには、次のように `C:\ps-test\Modules` 入力します。

```powershell
$Env:PSModulePath + ";C:\ps-test\Modules"
```

Linux または MacOS で既定のモジュールの場所を追加するには、次のコマンド形式を使用します。

```powershell
$Env:PSModulePath += ":<path>"
```

たとえば、 `/usr/local/Fabrikam/Modules` **PSModulePath** 環境変数の値にディレクトリを追加するには、次のように入力します。

```powershell
$Env:PSModulePath += ":/usr/local/Fabrikam/Modules"
```

Linux または MacOS では、コマンド内のコロン () は、 `:` 新しいパスをリスト内でその前のパスと分離します。

パスを **PSModulePath** に追加すると、 `Get-Module` コマンドには `Import-Module` そのパスのモジュールが含まれます。

設定した値は現在のセッションのみに影響します。 変更を永続化するには、PowerShell プロファイルにコマンドを追加するか、コントロールパネルの [システム] を使用して、レジストリの **PSModulePath** 環境変数の値を変更します。

また、変更を永続化するために、SetEnvironmentVariable クラスのメソッドを使用して、 **PSModulePath** 環境変数にパスを追加することもでき **ます。**

**PSModulePath** 変数の詳細については、「 [about_Environment_Variables](about_Environment_Variables.md)」を参照してください。

## <a name="modules-and-name-conflicts"></a>モジュールと名前の競合

名前の競合は、セッションの複数のコマンドの名前が同じときに発生します。 モジュールのコマンドの名前がセッションのコマンドまたはアイテムの名前と同じとき、モジュールをインポートすると名前の競合が発生します。

名前が競合していると、コマンドが隠されたり、置換されたりします。

### <a name="hidden"></a>[非表示]

コマンド名を入力して実行されるコマンドと違う場合、コマンドが隠されています。ただし、別の方法でコマンドを実行できます。たとえば、出所のモジュールまたはスナップインの名前でコマンド名を修飾します。

### <a name="replaced"></a>置換後

実行できなければコマンドは置換されています。同じ名前のコマンドで上書きされたためです。 競合を引き起こしたモジュールを削除しても、セッションを再起動しなければ、置換されたコマンドは実行できません。

`Import-Module` では、現在のセッションのコマンドを非表示にしたり置換したりするコマンドを追加できます。 また、セッションのコマンドはモジュールが追加したコマンドを隠すことがあります。

名前の競合を検出するには、コマンドレットの **All** パラメーターを使用し `Get-Command` ます。 PowerShell 3.0 以降では、コマンド名を入力したときに実行される `Get-Command` コマンドのみが取得されます。 **All** パラメーターは、セッション内の特定の名前を持つすべてのコマンドを取得します。

名前の競合を回避するには、コマンドレットの **NoClobber** パラメーターまたは **Prefix** パラメーターを使用し `Import-Module` ます。 **Prefix** パラメーターは、インポートされたコマンドの名前にプレフィックスを追加して、セッション内で一意になるようにします。 **NoClobber** パラメーターでは、セッションの既存のコマンドを非表示にしたり、置き換えたりするコマンドはインポートされません。

また、の **Alias**、 **コマンドレット**、 **関数**、および **Variable** パラメーターを使用して、 `Import-Module` インポートするコマンドだけを選択したり、セッションで名前の競合を引き起こすコマンドを除外したりすることもできます。

モジュールの作成者は、モジュールマニフェストの **Defaultcommandprefix** プロパティを使用して、すべてのコマンド名に既定のプレフィックスを追加することで、名前の競合を防ぐことができます。
**Prefix** パラメーターの値は、 **defaultcommandprefix** の値よりも優先されます。

コマンドが隠れているとしても、出所のモジュールまたはスナップインの名前でコマンド名を修飾すればコマンドを実行できます。

PowerShell コマンドの優先順位規則は、セッションに同じ名前のコマンドが含まれている場合に実行されるコマンドを決定します。

たとえば、セッションに同じ名前の関数とコマンドレットが含まれている場合、PowerShell は既定で関数を実行します。 セッションに種類と名前が同じコマンドが含まれるとき、たとえば、2 つのコマンドレットの名前が同じ場合、既定では追加された日が新しいほうのコマンドが実行されます。

優先順位の規則の説明や、非表示のコマンドを実行する手順などの詳細については、「 [about_Command_Precedence](about_Command_Precedence.md)」を参照してください。

## <a name="modules-and-snap-ins"></a>モジュールとスナップイン

モジュールとスナップインからセッションにコマンドを追加できます。モジュールでは、コマンドレット、プロバイダー、関数、変数、エイリアス、PowerShell ドライブなどの項目を含む、すべての種類のコマンドを追加できます。 スナップインはコマンドレットとプロバイダーのみを追加できます。

モジュールまたはスナップインをセッションから削除する前に、次のコマンドを利用し、削除するコマンドを決定します。

セッション内のコマンドレットのソースを検索するには、次のコマンド形式を使用します。

```powershell
Get-Command <cmdlet-name> | Format-List -Property verb,noun,pssnapin,module
```

たとえば、コマンドレットのソースを検索するには、次のように `Get-Date` 入力します。

```powershell
Get-Command Get-Date | Format-List -Property verb,noun,module
```

## <a name="module-related-warnings-and-errors"></a>モジュール関連の警告とエラー

モジュールによってエクスポートされるコマンドは、PowerShell コマンドの名前付け規則に従う必要があります。 インポートしたモジュールが、名前に許可されていない動詞を持つコマンドレットまたは関数をエクスポートすると、 `Import-Module` コマンドレットは次の警告メッセージを表示します。

> 警告: インポートされたコマンド名には、検出できない可能性がある未承認の動詞が含まれています。 詳細については Verbose パラメーターを使用するか、「Get-Verb」と入力して承認されている動詞の一覧を参照してください。

このメッセージは、単なる警告です。 実際には、非準拠のコマンドを含む、すべてのモジュールがインポートされます。 モジュール ユーザーにメッセージが表示されますが、名前付けの問題はモジュール作成者が解決する必要があります。

警告メッセージを表示しないようにするには、コマンドレットの **DisableNameChecking** パラメーターを使用し `Import-Module` ます。

## <a name="built-in-modules-and-snap-ins"></a>組み込みのモジュールおよびスナップイン

Powershell 2.0 および powershell 3.0 以降の旧形式のホストプログラムでは、powershell と共にインストールされるコアコマンドは、すべての PowerShell セッションに自動的に追加されるスナップインにパッケージ化されます。

PowerShell 3.0 以降では、初期セッション状態 API を実装するホストプログラムに対して、 `InitialSessionState.CreateDefault2` 既定ではすべてのセッションに対して、powershell が追加されます。 モジュールは最初の使用で自動的に読み込まれます。

> [!NOTE]
> コマンドレットを使用して開始されたセッションを含むリモートセッション `New-PSSession` は、組み込みのコマンドがスナップインにパッケージ化されている古い形式のセッションです。

次のモジュール (またはスナップイン) は、PowerShell と共にインストールされます。

- CimCmdlets
- Microsoft.PowerShell.Archive
- Microsoft.PowerShell.Core
- Microsoft.PowerShell.Diagnostics
- Microsoft.PowerShell.Host
- Microsoft.PowerShell.Management
- Microsoft.PowerShell.Security
- Microsoft.PowerShell.Utility
- Microsoft.WSMan.Management
- PackageManagement
- PowerShellGet
- PSDesiredStateConfiguration
- PSDiagnostics
- PSReadline

## <a name="logging-module-events"></a>モジュールイベントのログ記録

PowerShell 3.0 以降では、PowerShell モジュールおよびスナップイン内のコマンドレットと関数の実行イベントを記録できます。これを行うには、モジュールとスナップインの **Logpipelineexecutiondetails** プロパティをに設定し `$True` ます。
また、グループポリシー設定を使用してモジュールのログ記録を有効にし、すべての PowerShell セッションでモジュールのログ記録を有効にすることもできます。 詳細については、ログ記録とグループポリシーに関する記事を参照してください。

## <a name="see-also"></a>参照

[about_Logging_Windows](about_Logging_Windows.md)

[about_Logging_Non-Windows](about_Logging_Non-Windows.md)

[about_Group_Policy_Settings](about_Group_Policy_Settings.md)

[about_Command_Precedence](about_Command_Precedence.md)

[about_Group_Policy_Settings](about_Group_Policy_Settings.md)

[Get-Command](xref:Microsoft.PowerShell.Core.Get-Command)

[Get-Help](xref:Microsoft.PowerShell.Core.Get-Help)

[Get-Module](xref:Microsoft.PowerShell.Core.Get-Module)

[Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)

[Remove-Module](xref:Microsoft.PowerShell.Core.Remove-Module)
