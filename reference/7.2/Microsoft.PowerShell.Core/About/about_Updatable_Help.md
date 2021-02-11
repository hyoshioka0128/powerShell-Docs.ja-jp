---
description: PowerShell の更新可能なヘルプシステムについて説明します。
Locale: en-US
ms.date: 08/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_updatable_help?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Updatable_Help
ms.openlocfilehash: d688448b5aab8ec35ab5d57b444ad9902b8e8ecd
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601424"
---
# <a name="about-updatable-help"></a>更新可能なヘルプについて

## <a name="short-description"></a>簡単な説明
PowerShell の更新可能なヘルプシステムについて説明します。

## <a name="long-description"></a>長い説明

Powershell には、PowerShell コマンドレットと概念に関する最新のヘルプトピックにアクセスするためのさまざまな方法が用意されています。

PowerShell 3.0 で導入された更新可能なヘルプシステムは、コマンドラインから読み取ることができるように、ローカルコンピューターに常に最新のヘルプトピックがあることを保証するように設計されています。 これにより、ヘルプファイルをダウンロードしてインストールしたり、新しいヘルプファイルが使用可能になるたびに更新したりすることが簡単になります。

企業内の複数のコンピューター、およびインターネットにアクセスできないコンピューターの更新されたヘルプを提供するために、更新可能なヘルプを使用して、ファイルシステムディレクトリまたはファイル共有にヘルプファイルをダウンロードし、ファイル共有からヘルプファイルをインストールすることができます。

PowerShell 4.0 では、 **Helpinfouri** プロパティは Windows PowerShell リモート処理を介して保持されます。これにより、は `Save-Help` リモートコンピューターにインストールされているモジュールで動作しますが、必ずしもローカルコンピューターにインストールされるわけではありません。 インターネットにアクセスできないコンピューター上でを実行して、 **PSModuleInfo** オブジェクトをディスクまたはリムーバブルメディア (USB ドライブなど) に保存することができます。その場合、 `Export-Clixml` インターネットにアクセスできるコンピューターに **PSModuleInfo** オブジェクトをインポートし、 `Save-Help` **PSModuleInfo** オブジェクトで実行します。 保存されているヘルプは、リムーバブルメディアを使用してリモートの切断されたコンピューターにコピーしてから、を実行してインストールすることができ `Update-Help` ます。 これらの機能強化により、 `Save-Help` ネットワークアクセスのないコンピューターにヘルプをインストールできるようになりました。 新しい機能を使用する方法の例につい `Save-Help` ては、このトピックの「 [ファイル共有からヘルプを更新する方法](#how-to-update-help-from-a-file-share) 」を参照してください。

更新可能なヘルプは、コンピューター上にヘルプファイルがない場合でも、最新のヘルプトピックへのオンラインアクセスとコマンドレットの基本的なヘルプをサポートします。

PowerShell 3.0 にはヘルプファイルが付属していません。 更新可能なヘルプ機能を使用して、PowerShell およびすべての Windows モジュールに既定で含まれているすべてのコマンドのヘルプファイルをインストールすることができます。

## <a name="updatable-help-cmdlets"></a>更新可能なヘルプコマンドレット

- `Update-Help`: インターネットまたはファイル共有から最新のヘルプファイルをダウンロードし、ローカルコンピューターにインストールします。

- `Save-Help`: インターネットから最新のヘルプファイルをダウンロードし、ファイルシステムディレクトリまたはファイル共有に保存します。 ヘルプファイルをコンピューターにインストールするには、を使用 `Update-Help` します。

- `Get-Help`: コマンドラインにヘルプトピックを表示します。 コンピューター上のヘルプファイルからヘルプを取得します。 ヘルプファイルのないコマンドレットと関数の自動生成されたヘルプを表示します。 既定のインターネットブラウザーでコマンドレット、関数、スクリプト、およびワークフローのオンラインヘルプトピックを開きます。

## <a name="auto-generated-help-help-without-help-files"></a>自動生成ヘルプ: ヘルプファイルなしのヘルプ

コマンドレット、関数、またはワークフローのヘルプファイルがコンピューター上にない場合、 `Get-Help` コマンドレットは自動生成されたヘルプを表示し、ヘルプファイルをダウンロードするか、オンラインで読み取るかを確認するメッセージを表示します。

自動生成されたヘルプには、構文とエイリアス、および更新可能なヘルプコマンドレットの使用方法とオンラインヘルプトピックへのアクセス方法を説明するコメントが含まれています。

たとえば、次のコマンドは、コマンドレットの基本的なヘルプを取得し `Get-Culture` ます。 出力には、 `Get-Help` コンピューターにヘルプファイルがない場合の表示が示されます。

```powershell
Get-Help Get-Culture
```

```output
NAME
    Get-Culture

SYNTAX
    Get-Culture [<CommonParameters>]

ALIASES
    None

REMARKS
    To get the latest Help content including descriptions and examples
    type: Update-Help.
```

## <a name="help-files-for-modules"></a>モジュールのヘルプファイル

更新可能なヘルプの最小単位は、モジュールのヘルプです。 モジュールヘルプには、モジュールのすべてのコマンドレット、関数、ワークフロー、プロバイダー、スクリプト、および概念のヘルプが含まれています。 現在のセッションにインポートされていない場合でも、コンピューターにインストールされているすべてのモジュールのヘルプを更新できます。

モジュール全体のヘルプを更新できますが、個々のコマンドレットのヘルプは更新できません。

特定のコマンドレットを含むモジュールを見つけるには、次のコマンド形式を使用します。

```powershell
(Get-Command <cmdlet-name>).ModuleName
```

たとえば、コマンドレットを含むモジュールを検索するには、次のように `Set-ExecutionPolicy` 入力します。

```powershell
(Get-Command Set-ExecutionPolicy).ModuleName
```

特定のモジュールのヘルプを更新するには、次のように入力します。

```powershell
Update-Help -Module <ModuleName>
```

たとえば、Set-ExecutionPolicy コマンドレットを含むモジュールのヘルプを更新するには、次のように入力します。

```powershell
Update-Help -Module Microsoft.PowerShell.Security
```

## <a name="permissions-for-updatable-help"></a>更新可能なヘルプのアクセス許可

ディレクトリ内のモジュールのヘルプを更新するに `$pshome/Modules` は、コンピューターの Administrators グループのメンバーである必要があります。

Administrators グループのメンバーでない場合は、これらのモジュールのヘルプを更新することはできません。しかし、インターネットにアクセスできる場合は、オンラインでヘルプを表示できます。

ディレクトリ `$home/Documents/PowerShell/Modules` またはディレクトリの他のサブディレクトリ内のモジュールのヘルプを更新 `$home` するには、特別なアクセス許可は必要ありません。

`Update-Help`およびコマンドレットには、 `Save-Help` 現在のユーザーの明示的な資格情報を提供する **UseDefaultCredentials** パラメーターがあります。 このパラメーターは、セキュリティで保護されたインターネット上の場所にアクセスするために設計されて

`Update-Help`およびコマンドレットには、 `Save-Help` リモートコンピューターでコマンドを実行し、3番目のコンピューターのファイル共有にアクセスするための **資格情報** パラメーターもあります。 **Credential** パラメーターは、の SourcePath パラメーターまたは **LiteralPath** パラメーター、 `Update-Help` およびの **DestinationPath** パラメーターまたは **LiteralPath** パラメーターを使用する場合にのみ有効です `Save-Help` 。

## <a name="how-to-install-and-update-help-files"></a>ヘルプファイルをインストールおよび更新する方法

ヘルプファイルを初めてダウンロードしてインストールする場合、またはコンピューター上のヘルプファイルを更新する場合は、コマンドレットを使用し `Update-Help` ます。

`Update-Help`コマンドレットは、次のタスクを含む、すべてのハード機能を実行します。

- 更新可能なヘルプをサポートするモジュールを指定します。
- 各モジュールが更新可能なヘルプファイルを格納するインターネット上の場所を検索します。
- コンピューター上の各モジュールのヘルプファイルを、各モジュールで使用可能な最新のヘルプファイルと比較します。
- インターネットから新しいファイルをダウンロードします。
- ヘルプファイルパッケージのラップを解除します。
- ファイルが有効なヘルプファイルであることを確認します。
- モジュールディレクトリの言語固有のサブディレクトリにヘルプファイルをインストールします。

新しいヘルプトピックにアクセスするには、 `Get-Help` コマンドレットを使用します。 PowerShell を再起動する必要はありません。

更新可能なヘルプをサポートするコンピュータ上のすべてのモジュールのヘルプをインストールまたは更新するには、次のように入力します。

```powershell
Update-Help
```

特定のモジュールのヘルプを更新するには、の **Module** パラメーターを追加し `Update-Help` ます。 モジュール名にはワイルドカード文字を使用できます。

たとえば、ServerManager モジュールのヘルプを更新するには、次のように入力します。

```powershell
Update-Help -Module ServerManager
```

パラメーターを指定しない場合、は、 `Update-Help` セッション内のすべてのモジュールと、更新可能なヘルプをサポートするすべてのインストール済みモジュールのヘルプを更新します。 これを含めるには、PSModulePath 環境変数の値に示されているディレクトリにモジュールをインストールする必要があります。 これらは、"Get-help-ListAvailable" コマンドによって返されるモジュールでもあります。

**Module** パラメーターの値が `*` (すべて) の場合、では、更新 `Update-Help` 可能なヘルプをサポートしていないモジュールを含め、インストールされているすべてのモジュールのヘルプを更新しようとします。 このコマンドは、通常、更新可能なヘルプをサポートしていないモジュールをコマンドレットが検出したときに多くのエラーを生成します。

## <a name="how-to-update-help-from-a-file-share"></a>ファイル共有からヘルプを更新する方法

インターネットに接続されていないコンピューターをサポートする、または企業内での更新を制御または合理化するには、コマンドレットを使用し `Save-Help` ます。 `Save-Help`コマンドレットは、インターネットからヘルプファイルをダウンロードし、指定したファイルシステムディレクトリに保存します。

`Save-Help` 指定したディレクトリ内のヘルプファイルを、各モジュールで使用可能な最新のヘルプファイルと比較します。 ディレクトリにヘルプファイルがない場合、または新しいヘルプファイルがモジュールに使用できる場合、 `Save-Help` コマンドレットはインターネットから新しいファイルをダウンロードします。 ただし、ヘルプファイルのラップを解除したりインストールしたりすることはありません。

ファイルシステムディレクトリに保存されたヘルプファイルからコンピューターのヘルプファイルをインストールまたは更新するには、コマンドレットの **SourcePath** パラメーターを使用し `Update-Help` ます。 `Update-Help`コマンドレットは、最新のヘルプファイルを識別し、ラップを解除して検証し、モジュールディレクトリの言語固有のサブディレクトリにインストールします。

たとえば、インストールされているすべてのモジュールのヘルプをディレクトリに保存するには、次のように `\\Server\Share` 入力します。

```powershell
Save-Help -DestinationPath \\Server\Share
```

次に、ディレクトリからヘルプを更新するには、次のように `\\Server\Share` 入力します。

```powershell
Update-Help -SourcePath \\Server\Share
```

次の例は、を使用し `Save-Help` て、ローカルコンピューターにインストールされていないモジュールのヘルプを保存する方法を示しています。 この例では、管理者がを実行して、 `Save-Help` ローカルコンピューターに DhcpServer モジュールまたは DHCP サーバーの役割をインストールせずに、インターネットに接続されたクライアントコンピューターから DhcpServer モジュールのヘルプを保存します。

オプション 1: を実行して `Invoke-Command` リモートモジュールの **PSModuleInfo** オブジェクトを取得し、それを変数に保存 `$m` します。次に、 `Save-Help` モジュール名として変数を指定して、 **PSModuleInfo** オブジェクトに対してを実行し `$m` ます。

```powershell
$m = Invoke-Command -ComputerName RemoteServer -ScriptBlock
{ Get-Module -Name DhcpServer -ListAvailable }
Save-Help -Module $m -DestinationPath C:\SavedHelp
```

オプション 2: DHCP サーバーモジュールを実行しているコンピューターを対象とする PSSession を開き、モジュールの **PSModuleInfo** オブジェクトを取得し、変数に保存して、 `$m` `Save-Help` 変数に保存されているオブジェクトでを実行し `$m` ます。

```powershell
$s = New-PSSession -ComputerName RemoteServer
$m = Get-Module -PSSession $s -Name DhcpServer -ListAvailable
Save-Help -Module $m -DestinationPath C:\SavedHelp
```

オプション 3: DHCP サーバーモジュールを実行しているコンピューターを対象とする CIM セッションを開き、モジュールの **PSModuleInfo** オブジェクトを取得し、変数に保存して、 `$m` `Save-Help` 変数に保存されているオブジェクトでを実行し `$m` ます。

```powershell
$c = New-CimSession -ComputerName RemoteServer
$m = Get-Module -CimSession $c -Name DhcpServer -ListAvailable
Save-Help -Module $m -DestinationPath C:\SavedHelp
```

次の例では、管理者がネットワークにアクセスできないコンピューターに DHCP サーバーモジュールのヘルプをインストールします。

まず、を実行し `Export-Clixml` て、 **PSModuleInfo** オブジェクトを共有フォルダーまたはリムーバブルメディアにエクスポートします。

```powershell
$m = Get-Module -Name DhcpServer -ListAvailable
Export-Clixml -Path E:\UsbFlashDrive\DhcpModule.xml -InputObject $m
```

次に、インターネットにアクセスできるコンピューターにリムーバブルメディアを転送し、を使用して **PSModuleInfo** オブジェクトをインポートし `Import-Clixml` ます。 を実行して、インポートした `Save-Help` DhcpServer Module **PSModuleInfo** オブジェクトのヘルプを保存します。

```powershell
$deserialized_m = Import-Clixml E:\UsbFlashDrive\DhcpModule.xml
Save-Help -Module $deserialized_m -DestinationPath E:\UsbFlashDrive\SavedHelp
```

最後に、リムーバブルメディアをネットワークにアクセスできないコンピューターに転送してから、を実行してヘルプをインストールし `Update-Help` ます。

```powershell
Update-Help -Module DhcpServer -SourcePath E:\UsbFlashDrive\SavedHelp
```

パラメーターを指定しない場合、は、 `Save-Help` セッション内のすべてのモジュールと、更新可能なヘルプをサポートするすべてのインストール済みモジュールのヘルプをダウンロードします。 モジュールを含めるには、 `$env:PSModulePath` ローカルコンピューターまたはヘルプを保存するリモートコンピューターで、環境変数の値に一覧表示されているディレクトリにモジュールをインストールする必要があります。 これらは、コマンドの実行によって返されるモジュールでもあり `Get-Help -ListAvailable` ます。

## <a name="how-to-update-help-files-in-different-languages"></a>さまざまな言語のヘルプファイルを更新する方法

既定では、 `Update-Help` および `Save-Help` コマンドレットは、ローカルコンピューターの Windows に設定されている UI カルチャおよび言語のヘルプをダウンロードします。 指定したモジュールのヘルプファイルがローカル UI カルチャで使用できない場合は、 `Update-Help` `Save-Help` Windows 言語フォールバック規則を使用して、サポートされている言語を検索します。

ただし、およびコマンドレットの **UICulture** パラメーターを使用して、 `Update-Help` `Save-Help` 使用可能な任意の UI カルチャでヘルプファイルをダウンロードしてインストールすることができます。

たとえば、セッションのすべてのモジュールの最新のヘルプファイルを日本語 (Ja-jp) とフランス語 (fr-fr) で保存するには、次のように入力します。

```powershell
Save-Help -Path \Server\Share -UICulture ja-jp, fr-fr
```

指定した言語でモジュールのヘルプファイルを使用できない場合、 `Update-Help` `Save-Help` コマンドレットとコマンドレットは、各モジュールのヘルプが使用可能な言語を示すエラーメッセージを返します。これにより、ニーズに最も合った代替手段を選択できます。

> [!NOTE]
> 現在、更新可能なヘルプコンテンツは英語 (en-us) でのみ公開されています。 Windows 以外のシステムでは、 **UICulture** パラメーターを使用して、コンテンツを明示的に要求する必要があり `en-US` ます。

## <a name="how-to-use-online-help"></a>オンラインヘルプの使用方法

ローカルコンピューターのヘルプファイルを更新できない場合、または更新しない場合でも、最新のヘルプファイルをオンラインで取得できます。

コマンドレットまたは関数のオンラインヘルプトピックを開くには、コマンドレットの **online** パラメーターを使用し `Get-Help` ます。

たとえば、次のコマンドは、 `Get-Job` 既定のインターネットブラウザーでコマンドレットのオンラインヘルプトピックを開きます。

```powershell
Get-Help Get-Job -Online
```

スクリプトのオンラインヘルプを表示するには、 **online** パラメーターとスクリプトへの完全なパスを使用します。

**Online** パラメーターは、About トピックでは機能しません。 Powershell 言語に関するヘルプトピックを含む、PowerShell の概要に関するトピックについては、 [powershell の概要に](about.md)関するトピックを参照してください。

## <a name="how-to-minimize-or-prevent-internet-downloads"></a>インターネットのダウンロードを最小化または禁止する方法

インターネットに接続されていないユーザーに、インターネットのダウンロードを最小限にし、更新可能なヘルプを提供するには、コマンドレットを使用し `Save-Help` ます。 インターネットからヘルプをダウンロードし、ネットワーク共有に保存します。 次に、 `Update-Help` すべてのコンピューターでコマンドを実行するグループポリシー設定またはスケジュールされたジョブを作成します。 コマンドレットの **SourcePath** パラメーターの値 `Update-Help` をネットワーク共有に設定します。

インターネットアクセス権を持つユーザーがインターネットから更新可能なヘルプをダウンロードできないようにするには、[ **update-help グループポリシーの既定のソースパスを設定** する] 設定を使用します。

このグループポリシー設定では、影響を受けるすべてのコンピューター上のすべてのコマンドに、指定したファイルシステムの場所を持つ **SourcePath** パラメーターが暗黙的に追加され `Update-Help` ます。 ユーザーは **sourcepath** パラメーターを明示的に使用して別のファイルシステムの場所を指定できますが、 **sourcepath** パラメーターを除外して、インターネットからヘルプをダウンロードすることはできません。

> [!NOTE]
> [**コンピューターの構成** と **ユーザーの構成**] の下に、[**更新プログラムの既定のソースパスを設定** してください] ポリシー設定が表示されます。 ただし、[ **コンピューターの構成** ] の下のポリシー設定のみが有効です。 [ **ユーザーの構成** ] のポリシー設定は無視されます。

詳細については、「[about_Group_Policy_Settings](about_Group_Policy_Settings.md)」をご覧ください。

## <a name="how-to-update-help-for-non-standard-modules"></a>標準以外のモジュールのヘルプを更新する方法

コマンドレットの **ListAvailable** パラメーターによって返されないモジュールのヘルプを更新または保存するに `Get-Module` は、またはコマンドを実行する前に、現在のセッションにモジュールをインポートし `Update-Help` `Save-Help` ます。 リモートコンピューターで、コマンドを実行する前に、 `Save-Help` `Invoke-Command` リモートコンピューターに接続されている現在のセッションまたはスクリプトブロックにモジュールをインポートします。

モジュールが現在のセッション内にある場合は、 `Update-Help` `Save-Help` パラメーターを指定せずにまたはコマンドレットを実行するか、 **module** パラメーターを使用してモジュール名を指定します。

およびコマンドレットの **モジュール** パラメーターには、 `Update-Help` `Save-Help` モジュール名のみを使用できます。 モジュールファイルのパスを受け取ることはできません。

この手法は、コマンドレットの **ListAvailable** パラメーターによって返されないモジュール (環境変数に一覧表示されていない場所にインストールされているモジュールや、適切な形式ではないモジュールなど) のヘルプを更新または保存するために使用し `Get-Module` `$env:PSModulePath` ます (モジュールディレクトリには、ディレクトリ名と同じ名前のファイルが少なくとも1つ含まれ

## <a name="how-to-support-updatable-help"></a>更新可能なヘルプをサポートする方法

モジュールを作成する場合は、モジュールのオンラインヘルプと更新可能なヘルプをサポートできます。 詳細については、「Microsoft Docs の [更新可能なヘルプ](/powershell/scripting/developer/help/supporting-updatable-help) と [サポートオンラインヘルプ](/powershell/scripting/developer/module/supporting-online-help) のサポート」を参照してください。

更新可能なヘルプは、PowerShell スナップインやコメントベースのヘルプには使用できません。

## <a name="remarks"></a>解説

`Update-Help`および `Save-Help` コマンドレットは、Windows Preinstallation Environment (Windows PE) ではサポートされていません。

## <a name="see-also"></a>関連項目

[Get-Help](xref:Microsoft.PowerShell.Core.Get-Help)

[Save-Help](xref:Microsoft.PowerShell.Core.Save-Help)

[Update-Help](xref:Microsoft.PowerShell.Core.Update-Help)
