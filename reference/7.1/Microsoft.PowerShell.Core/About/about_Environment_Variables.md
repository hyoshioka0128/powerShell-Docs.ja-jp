---
description: PowerShell で Windows 環境変数にアクセスする方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 09/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Environment_Variables
ms.openlocfilehash: 4b5894822f4436f127ed4789fd8008a0e7f2f2df
ms.sourcegitcommit: f5986121386c81acddcf324eb0526d7d092bcc8f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/20/2021
ms.locfileid: "98584649"
---
# <a name="about-environment-variables"></a>環境変数について

## <a name="short-description"></a>概要
PowerShell で Windows 環境変数にアクセスする方法について説明します。

## <a name="long-description"></a>詳細説明

環境変数には、オペレーティングシステム環境に関する情報が格納されます。 この情報には、オペレーティングシステムのパス、オペレーティングシステムによって使用されるプロセッサの数、一時フォルダーの場所などの詳細が含まれます。

環境変数には、オペレーティングシステムおよびその他のプログラムによって使用されるデータが格納されます。 たとえば、環境変数には、 `WINDIR` Windows インストールディレクトリの場所が含まれます。 プログラムは、この変数の値を照会して、Windows オペレーティングシステムファイルが配置されている場所を判断できます。

PowerShell は、サポートされている任意のオペレーティングシステムプラットフォームで環境変数にアクセスして管理できます。 PowerShell 環境プロバイダーは、環境変数を簡単に表示および変更できるようにすることで、このプロセスを簡略化します。

環境変数は、PowerShell の他の種類の変数とは異なり、子プロセス (ローカルのバックグラウンドジョブやモジュールメンバーが実行されるセッションなど) によって継承されます。 これにより、環境変数が、親プロセスと子プロセスの両方に必要な値を格納するために適しています。

## <a name="using-and-changing-environment-variables"></a>環境変数の使用と変更

Windows では、環境変数は次の3つのスコープで定義できます。

- コンピューター (またはシステム) のスコープ
- ユーザー スコープ
- プロセススコープ

_プロセス_ スコープには、現在のプロセスまたは PowerShell セッションで使用できる環境変数が含まれています。 この変数の一覧は親プロセスから継承され、 _コンピューター_ と _ユーザー_ スコープの変数から構築されます。 Unix ベースのプラットフォームには、 _プロセス_ スコープしかありません。

環境変数の値を表示および変更するには、環境プロバイダーで変数構文を使用してコマンドレットを使用する必要があります。 環境変数の値を表示するには、次の構文を使用します。

```
$Env:<variable-name>
```

たとえば、環境変数の値を表示するには、 `WINDIR` PowerShell コマンドプロンプトで次のコマンドを入力します。

```powershell
$Env:windir
```

この構文では、ドル記号 ( `$` ) は変数を示し、ドライブ名 ( `Env:` ) は環境変数を示し、その後に変数名 () を指定し `windir` ます。

PowerShell で環境変数を変更すると、変更は現在のセッションのみに影響します。 この動作は、 `Set` Windows コマンドシェルのコマンドと `Setenv` UNIX ベースの環境でのコマンドの動作に似ています。 コンピューターまたはユーザースコープの値を変更するには、 **system.object クラスの** メソッドを使用する必要があります。

コンピュータースコープの変数に変更を加えるには、にもアクセス許可が必要です。 十分な権限を持たない値を変更しようとすると、コマンドは失敗し、PowerShell にはエラーが表示されます。

コマンドレットを使用せずに変数の値を変更するには、次の構文を使用します。

```powershell
$Env:<variable-name> = "<new-value>"
```

たとえば、環境変数の値にを追加するには、 `;c:\temp` 次の構文を使用し `Path` ます。

```powershell
$Env:Path += ";c:\temp"
```

Linux または macOS では、コマンド内のコロン () は、 `:` 新しいパスをリスト内でその前のパスと分離します。

```powershell
$Env:PATH += ":/usr/local/temp"
```

また、、、などの項目のコマンドレットを使用して、 `Set-Item` `Remove-Item` `Copy-Item` 環境変数の値を変更することもできます。 たとえば、コマンドレットを使用して `Set-Item` 環境変数の値にを追加するには、 `;c:\temp` `Path` 次の構文を使用します。

```powershell
Set-Item -Path Env:Path -Value ($Env:Path + ";C:\Temp")
```

このコマンドでは、値は、1つの単位として解釈されるように、かっこで囲まれています。

## <a name="environment-variables-that-store-preferences"></a>基本設定を格納する環境変数

PowerShell 機能では、環境変数を使用してユーザー設定を保存できます。
これらの変数は、ユーザー設定変数と同じように動作しますが、それらが作成されるセッションの子セッションによって継承されます。 ユーザー設定変数の詳細については、「 [about_preference_variables](about_Preference_Variables.md)」を参照してください。

設定を格納する環境変数には次のものがあります。

- PSExecutionPolicyPreference

  現在のセッションの実行ポリシーセットを格納します。 この環境変数は、1つのセッションの実行ポリシーを設定した場合にのみ存在します。
  これは、2つの異なる方法で行うことができます。

  - コマンドラインから **set-executionpolicy** パラメーターを使用してセッションを開始し、セッションの実行ポリシーを設定します。

  - `Set-ExecutionPolicy` コマンドレットを使用します。 "Process" という値を持つ Scope パラメーターを使用します。

    詳細については、「[about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

- PSModuleAnalysisCachePath

  PowerShell では、モジュールとそのコマンドレットに関するデータをキャッシュするために使用されるファイルを制御できます。 キャッシュは、コマンドの検索中に起動時に読み込まれ、モジュールがインポートされた後にバックグラウンドスレッドで書き込まれます。

  キャッシュの既定の場所は次のとおりです。

  - Windows PowerShell 5.1: `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell`
  - PowerShell 6.0 以降: `$env:LOCALAPPDATA\Microsoft\PowerShell`
  - Windows 以外の既定: `~/.cache/powershell`

  キャッシュの既定のファイル名は、 `ModuleAnalysisCache` です。 PowerShell の複数のインスタンスがインストールされている場合、ファイル名には16進数のサフィックスが付いているため、インストールごとに一意のファイル名が使用されます。

  > [!NOTE]
  > コマンド検出が正常に機能しない場合、Intellisense など、存在しないコマンドが表示される場合は、キャッシュファイルを削除できます。 キャッシュは、次に PowerShell を起動したときに再作成されます。

  キャッシュの既定の場所を変更するには、PowerShell を開始する前に環境変数を設定します。 この環境変数への変更は、子プロセスにのみ影響します。 値には、PowerShell がファイルの作成および書き込みアクセス許可を持つ完全なパス (ファイル名を含む) を指定する必要があります。

  ファイル キャッシュを無効にするには、たとえば次のような無効な場所をこの値に設定します。

  ```powershell
  # `NUL` here is a special device on Windows that cannot be written to,
  # on non-Windows you would use `/dev/null`
  $env:PSModuleAnalysisCachePath = 'NUL'
  ```

  これにより、パスが **NUL** デバイスに設定されます。 PowerShell はパスに書き込むことができませんが、エラーは返されません。 トレーサーを使用して報告されたエラーを確認できます。

  ```powershell
  Trace-Command -PSHost -Name Modules -Expression { Import-Module Microsoft.PowerShell.Management -Force }
  ```

- PSDisableModuleAnalysisCacheCleanup

  モジュール分析キャッシュを書き込むときに、PowerShell は、不要な大きなキャッシュを回避するために存在しないモジュールをチェックします。 このようなチェックが不要な場合もあります。その場合、この環境変数の値をに設定して、これらのチェックをオフにすることができ `1` ます。

  この環境変数の設定は、現在のプロセスですぐに有効になります。

- PSModulePath

  環境変数には、 `$env:PSModulePath` モジュールとリソースを検索するために検索されるフォルダーの場所の一覧が含まれています。

  既定では、に割り当てられて `$env:PSModulePath` いる有効な場所は次のとおりです。

  - システム全体の場所: これらのフォルダーには、PowerShell に付属しているモジュールが含まれています。 モジュールは場所に格納されてい `$PSHOME\Modules` ます。 また、これは Windows 管理モジュールがインストールされている場所です。

  - ユーザーがインストールしたモジュール: これらはユーザーによってインストールされたモジュールです。
    `Install-Module` には、現在のユーザーまたはすべてのユーザーに対してモジュールがインストールされているかどうかを指定できる **スコープ** パラメーターがあります。 詳細については、「 [Install-Module](xref:PowerShellGet.Install-Module)」を参照してください。

    - Windows では、ユーザー固有の **CurrentUser** スコープの場所は `$HOME\Documents\PowerShell\Modules` フォルダーです。 **AllUsers** スコープの場所は `$env:ProgramFiles\PowerShell\Modules` です。
    - Windows 以外のシステムでは、ユーザー固有の **CurrentUser** スコープの場所は `$HOME/.local/share/powershell/Modules` フォルダーです。 **AllUsers** スコープの場所は `/usr/local/share/powershell/Modules` です。

  また、Program Files ディレクトリなど、他のディレクトリにモジュールをインストールするセットアッププログラムでは、の値にその場所を追加でき `$env:PSModulePath` ます。

  詳細については、「 [about_PSModulePath](about_PSModulePath.md)」を参照してください。

## <a name="managing-environment-variables"></a>環境変数の管理

PowerShell には、環境変数を管理するためのさまざまな方法が用意されています。

- 環境プロバイダーのドライブ
- 項目のコマンドレット
- **.Net system.servicemodel** クラス
- Windows の場合、システムコントロールパネル

### <a name="using-the-environment-provider"></a>環境プロバイダーの使用

各環境変数は、system.string **エントリ** クラスのインスタンスによって表されます。 各 **Dictionaryentry** オブジェクトでは、環境変数の名前はディクショナリキーです。 変数の値はディクショナリ値です。

PowerShell で環境変数を表すオブジェクトのプロパティとメソッドを表示するには、コマンドレットを使用し `Get-Member` ます。 たとえば、ドライブ内のすべてのオブジェクトのメソッドとプロパティを表示するには、次のように `Env:` 入力します。

```powershell
Get-Item -Path Env:* | Get-Member
```

PowerShell 環境プロバイダーを使用すると、PowerShell ドライブ (ドライブ) の環境変数にアクセスでき `Env:` ます。 このドライブは、ファイルシステムドライブのように見えます。 ドライブにアクセスするには `Env:` 、次のように入力します。

```powershell
Set-Location Env:
```

コンテンツコマンドレットを使用して、環境変数の値を取得または設定します。

```powershell
PS Env:\> Set-Content -Path Test -Value 'Test value'
PS Env:\> Get-Content -Path Test
Test value
```

ドライブの環境変数は、 `Env:` 他の PowerShell ドライブから表示できます。ドライブに移動すると、 `Env:` 環境変数を表示したり、変更したりできます。

### <a name="using-item-cmdlets"></a>Item コマンドレットの使用

環境変数を参照するときは、 `Env:` ドライブ名とその後に変数の名前を入力します。 たとえば、環境変数の値を表示するには `COMPUTERNAME` 、次のように入力します。

```powershell
Get-ChildItem Env:Computername
```

すべての環境変数の値を表示するには、次のように入力します。

```powershell
Get-ChildItem Env:
```

環境変数には子項目がないため、との `Get-Item` 出力 `Get-ChildItem` は同じです。

既定では、PowerShell は、環境変数を取得した順序で表示します。 変数名で環境変数の一覧を並べ替えるには、パイプを使ってコマンドの出力を `Get-ChildItem` コマンドレットに渡し `Sort-Object` ます。 たとえば、任意の PowerShell ドライブから、次のように入力します。

```powershell
Get-ChildItem Env: | Sort Name
```

また、 `Env:` コマンドレットを使用してドライブにアクセスすることもでき `Set-Location` ます。

```powershell
Set-Location Env:
```

ドライブにいるときは、 `Env:` `Env:` パスからドライブ名を省略できます。 たとえば、すべての環境変数を表示するには、次のように入力します。

```powershell
PS Env:\> Get-ChildItem
```

ドライブ内から変数の値を表示するには `COMPUTERNAME` `Env:` 、次のように入力します。

```powershell
PS Env:\> Get-ChildItem ComputerName
```

### <a name="saving-changes-to-environment-variables"></a>環境変数への変更の保存

Windows の環境変数に対して永続的な変更を行うには、[システム] コントロールパネルを使用します。 [ **システムの詳細設定**] を選択します。 [ **詳細設定** ] タブで、[ **環境変数**] をクリックします。 **ユーザー** および **システム** (コンピューター) のスコープでは、既存の環境変数を追加または編集できます。 Windows はこれらの値をレジストリに書き込みます。これにより、セッションとシステムの再起動の間で永続化されます。

または、PowerShell プロファイルで環境変数を追加または変更することもできます。 この方法は、サポートされている任意のプラットフォーム上の任意のバージョンの PowerShell で使用できます。

### <a name="using-systemenvironment-methods"></a>System.object メソッドの使用

GetEnvironmentVariable **クラスに** は、変数のスコープを指定できるようにする、 メソッドと **SetEnvironmentVariable** メソッドが用意されています。

次の例では、 **GetEnvironmentVariable** メソッドを使用してコンピューターの設定を取得し、SetEnvironmentVariable メソッドを使用してその `PSModulePath` パスを値に追加して `C:\Program Files\Fabrikam\Modules` います。

```powershell
$path = [Environment]::GetEnvironmentVariable('PSModulePath', 'Machine')
$newpath = $path + ';C:\Program Files\Fabrikam\Modules'
[Environment]::SetEnvironmentVariable("PSModulePath", $newpath, 'Machine')
```

System.object クラスのメソッドの詳細については、「[環境メソッド](/dotnet/api/system.environment)」を参照して **ください。**

## <a name="see-also"></a>関連項目

- [環境 (プロバイダー)](../About/about_Environment_Provider.md)
- [about_Modules](about_Modules.md)

