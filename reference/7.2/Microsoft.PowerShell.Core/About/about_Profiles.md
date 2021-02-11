---
description: PowerShell プロファイルを作成して使用する方法について説明します。
ms.date: 11/30/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Profiles
ms.openlocfilehash: 3c739d6fd65714d5b6ef0f45990b614486d4e5ff
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601986"
---
# <a name="about-profiles"></a>プロファイルについて

## <a name="short-description"></a>簡単な説明
PowerShell プロファイルを作成して使用する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell プロファイルを作成して、環境をカスタマイズし、開始するすべての PowerShell セッションにセッション固有の要素を追加できます。

PowerShell プロファイルは、PowerShell の開始時に実行されるスクリプトです。 プロファイルをログオンスクリプトとして使用して、環境をカスタマイズできます。 コマンド、エイリアス、関数、変数、スナップイン、モジュール、および PowerShell ドライブを追加できます。 また、セッション固有のその他の要素をプロファイルに追加することもできます。これにより、すべてのセッションでそれらをインポートまたは再作成することなく使用できるようになります。

PowerShell では、ユーザーとホストプログラムに対して複数のプロファイルをサポートしています。 ただし、プロファイルは作成されません。 このトピックでは、プロファイルについて説明し、コンピューターでプロファイルを作成および管理する方法について説明します。

PowerShell コンソールの **Noprofile** パラメーター (PowerShell.exe) を使用して、プロファイルを指定せずに powershell を起動する方法について説明します。 また、PowerShell の実行ポリシーがプロファイルに与える影響についても説明します。

## <a name="the-profile-files"></a>プロファイルファイル

PowerShell では、複数のプロファイルファイルがサポートされています。 また、PowerShell ホストプログラムは、独自のホスト固有のプロファイルをサポートできます。

たとえば、PowerShell コンソールでは、次の基本的なプロファイルファイルがサポートされています。 プロファイルは、優先順位順に一覧表示されます。 最初のプロファイルの優先順位は最も高くなります。

|説明               | Path                                          |
|--------------------------|-----------------------------------------------|
|すべてのユーザー、すべてのホスト      |$PSHOME \\Profile.ps1                           |
|すべてのユーザー、現在のホスト   |$PSHOME \\Microsoft.PowerShell_profile.ps1      |
|現在のユーザー、すべてのホスト   |$Home \\ [My] ドキュメント \\ PowerShell \\Profile.ps1 |
|現在のユーザー、現在のホスト|$Home \\ [My] ドキュメント \\ PowerShell\\<br>Microsoft.PowerShell_profile.ps1 |

プロファイルパスには、次の変数が含まれます。

- `$PSHOME`変数。 PowerShell のインストールディレクトリを格納します。
- `$Home`現在のユーザーのホームディレクトリを格納する変数。

さらに、PowerShell をホストする他のプログラムは、独自のプロファイルをサポートできます。 たとえば、Visual Studio Code は次のホスト固有のプロファイルをサポートしています。

|説明               | Path                                     |
|--------------------------|------------------------------------------|
|すべてのユーザー、現在のホスト   |$PSHOME \\Microsoft.VSCode_profile.ps1|
|現在のユーザー、現在のホスト|$Home \\ [My] ドキュメント \\ PowerShell\\<br>Microsoft.VSCode_profile.ps1|

PowerShell ヘルプでは、"CurrentUser, Current Host" プロファイルは、"お使いの PowerShell プロファイル" と呼ばれることが多いプロファイルです。

## <a name="the-profile-variable"></a>$PROFILE 変数

`$PROFILE`自動変数は、現在のセッションで使用可能な PowerShell プロファイルへのパスを格納します。

プロファイルパスを表示するには、変数の値を表示し `$PROFILE` ます。 `$PROFILE`パスを表すために、コマンドで変数を使用することもできます。

変数には、 `$PROFILE` "現在のユーザー、現在のホスト" プロファイルへのパスが格納されます。 その他のプロファイルは、変数のメモプロパティに保存され `$PROFILE` ます。

たとえば、変数の `$PROFILE` 値は、Windows PowerShell コンソールで次のようになります。

|説明                |名前                              |
|---------------------------|----------------------------------|
|現在のユーザー、現在のホスト |`$PROFILE`                        |
|現在のユーザー、現在のホスト |`$PROFILE.CurrentUserCurrentHost` |
|現在のユーザー、すべてのホスト    |`$PROFILE.CurrentUserAllHosts`    |
|すべてのユーザー、現在のホスト    |`$PROFILE.AllUsersCurrentHost`    |
|すべてのユーザー、すべてのホスト       |`$PROFILE.AllUsersAllHosts`       |

変数の値は各 `$PROFILE` ユーザーと各ホストアプリケーションで変更されるため、使用する各 PowerShell ホストアプリケーションでプロファイル変数の値を表示するようにしてください。

変数の現在の値を表示するには `$PROFILE` 、次のように入力します。

```powershell
$PROFILE | Get-Member -Type NoteProperty
```

変数は、 `$PROFILE` 多くのコマンドで使用できます。 たとえば、次のコマンドは、"現在のユーザー、現在のホスト" プロファイルをメモ帳で開きます。

```powershell
notepad $PROFILE
```

次のコマンドは、"すべてのユーザー、すべてのホスト" プロファイルがローカルコンピューター上に作成されているかどうかを確認します。

```powershell
Test-Path -Path $PROFILE.AllUsersAllHosts
```

## <a name="how-to-create-a-profile"></a>プロファイルを作成する方法

PowerShell プロファイルを作成するには、次のコマンド形式を使用します。

```powershell
if (!(Test-Path -Path <profile-name>)) {
  New-Item -ItemType File -Path <profile-name> -Force
}
```

たとえば、現在の PowerShell ホストアプリケーションで現在のユーザーのプロファイルを作成するには、次のコマンドを使用します。

```powershell
if (!(Test-Path -Path $PROFILE)) {
  New-Item -ItemType File -Path $PROFILE -Force
}
```

このコマンドでは、 `If` ステートメントを使用して既存のプロファイルを上書きすることはできません。 プレースホルダーの値を、 \<profile-path\> 作成するプロファイルファイルのパスに置き換えます。

> [!NOTE]
> Windows Vista 以降のバージョンの Windows で "すべてのユーザー" プロファイルを作成するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

## <a name="how-to-edit-a-profile"></a>プロファイルを編集する方法

任意の PowerShell プロファイルを、メモ帳などのテキストエディターで開くことができます。

現在の PowerShell ホストアプリケーションで現在のユーザーのプロファイルをメモ帳で開くには、次のように入力します。

```powershell
notepad $PROFILE
```

他のプロファイルを開くには、プロファイル名を指定します。 たとえば、すべてのホストアプリケーションのすべてのユーザーのプロファイルを開くには、次のように入力します。

```powershell
notepad $PROFILE.AllUsersAllHosts
```

変更を適用するには、プロファイルファイルを保存し、PowerShell を再起動します。

## <a name="how-to-choose-a-profile"></a>プロファイルを選択する方法

複数のホストアプリケーションを使用する場合は、すべてのホストアプリケーションで使用する項目をプロファイルに配置し `$PROFILE.CurrentUserAllHosts` ます。 ホストアプリケーションに固有の項目 (ホストアプリケーションの背景色を設定するコマンドなど) を、そのホストアプリケーションに固有のプロファイルに配置します。

多くのユーザーに対して PowerShell をカスタマイズしている管理者は、次のガイドラインに従ってください。

- プロファイルに共通項目を格納する `$PROFILE.AllUsersAllHosts`
- ホストアプリケーションに固有の項目をホストアプリケーションに固有のプロファイルに格納する `$PROFILE.AllUsersCurrentHost`
- ユーザー固有のプロファイルに特定のユーザーの項目を格納する

PowerShell プロファイルの特別な実装については、ホストアプリケーションのドキュメントを確認してください。

## <a name="how-to-use-a-profile"></a>プロファイルの使用方法

PowerShell で作成した項目の多くは、現在のセッションのみに影響を与えます。 セッションを終了すると、項目が削除されます。

セッション固有のコマンドと項目には、変数、ユーザー設定変数、エイリアス、関数、コマンド ( [set-executionpolicy](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)を除く)、およびセッションに追加する PowerShell モジュールが含まれます。

これらの項目を保存して、今後のすべてのセッションで使用できるようにするには、それらを PowerShell プロファイルに追加します。

プロファイルのもう1つの一般的な用途は、頻繁に使用される関数、エイリアス、および変数を保存することです。 プロファイルに項目を保存すると、その項目を再作成せずに、適用可能な任意のセッションで使用できるようになります。

## <a name="how-to-start-a-profile"></a>プロファイルを開始する方法

プロファイルファイルを開くと、そのファイルは空白になります。 ただし、頻繁に使用する変数、エイリアス、およびコマンドを入力することができます。

作業を開始するためのいくつかの推奨事項を次に示します。

### <a name="add-commands-that-make-it-easy-to-open-your-profile"></a>プロファイルを簡単に開くためのコマンドを追加する

これは、"現在のユーザー、現在のホスト" プロファイル以外のプロファイルを使用する場合に特に便利です。 たとえば、次のコマンドを追加します。

```powershell
function Pro {notepad $PROFILE.CurrentUserAllHosts}
```

### <a name="add-a-function-that-lists-the-aliases-for-any-cmdlet"></a>任意のコマンドレットのエイリアスを一覧表示する関数を追加する

```powershell
function Get-CmdletAlias ($cmdletname) {
  Get-Alias |
    Where-Object -FilterScript {$_.Definition -like "$cmdletname"} |
      Format-Table -Property Definition, Name -AutoSize
}
```

### <a name="customize-your-console"></a>コンソールをカスタマイズする

```powershell
function Color-Console {
  $Host.ui.rawui.backgroundcolor = "white"
  $Host.ui.rawui.foregroundcolor = "black"
  $hosttime = (Get-ChildItem -Path $PSHOME\PowerShell.exe).CreationTime
  $hostversion="$($Host.Version.Major)`.$($Host.Version.Minor)"
  $Host.UI.RawUI.WindowTitle = "PowerShell $hostversion ($hosttime)"
  Clear-Host
}
Color-Console
```

### <a name="add-a-customized-powershell-prompt"></a>カスタマイズした PowerShell プロンプトを追加する

```powershell
function Prompt
{
$env:COMPUTERNAME + "\" + (Get-Location) + "> "
}
```

PowerShell プロンプトの詳細については、「 [about_Prompts](about_Prompts.md)」を参照してください。

## <a name="the-noprofile-parameter"></a>NoProfile パラメーター

プロファイルなしで PowerShell を開始するには、PowerShell を起動するプログラム **PowerShell.exe** の **noprofile** パラメーターを使用します。

まず、Cmd.exe や PowerShell 自体など、PowerShell を起動できるプログラムを開きます。 Windows の [実行] ダイアログボックスを使用することもできます。

型:

```
PowerShell -NoProfile
```

PowerShell.exe のパラメーターの完全な一覧を表示するには、次のように入力します。

```
PowerShell -?
```

## <a name="profiles-and-execution-policy"></a>プロファイルと実行ポリシー

PowerShell の実行ポリシーによって、スクリプトを実行したり、構成ファイル (プロファイルを含む) を読み込むことができるかどうかが決まります。 **制限** された実行ポリシーが既定値です。 プロファイルを含め、すべてのスクリプトの実行を防止します。 "Restricted" ポリシーを使用すると、プロファイルは実行されず、その内容は適用されません。

コマンドは、 `Set-ExecutionPolicy` 実行ポリシーを設定して変更します。 値はレジストリに保存されるため、すべての PowerShell セッションで適用されるいくつかのコマンドの1つです。 コンソールを開いたときに設定する必要はありません。また、プロファイルにコマンドを保存する必要もありません `Set-ExecutionPolicy` 。

## <a name="profiles-and-remote-sessions"></a>プロファイルとリモートセッション

PowerShell プロファイルはリモートセッションで自動的に実行されないため、プロファイルによって追加されたコマンドはリモートセッションに存在しません。 また、 `$PROFILE` 自動変数は、リモートセッションでは設定されません。

セッションでプロファイルを実行するには、 [コマンド](xref:Microsoft.PowerShell.Core.Invoke-Command) レットを使用します。

たとえば、次のコマンドは、のセッションでローカルコンピューターから "現在のユーザー、現在のホスト" プロファイルを実行し `$s` ます。

```powershell
Invoke-Command -Session $s -FilePath $PROFILE
```

次のコマンドは、のセッションで、リモートコンピューターから "現在のユーザー、現在のホスト" プロファイルを実行し `$s` ます。 変数が設定されていないため、この `$PROFILE` コマンドはプロファイルへの明示的なパスを使用します。 ドットソーシング演算子を使用すると、プロファイルはリモートコンピューター上の現在のスコープで実行され、それ自体のスコープ内では実行されません。

```powershell
Invoke-Command -Session $s -ScriptBlock {
  . "$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"
}
```

このコマンドを実行すると、プロファイルによってセッションに追加されたコマンドがで使用できるように `$s` なります。

## <a name="see-also"></a>参照

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Functions](about_Functions.md)

[about_Prompts](about_Prompts.md)

[about_Execution_Policies](about_Execution_Policies.md)

[about_Signing](about_Signing.md)

[about_Remote](about_Remote.md)

[about_Scopes](about_Scopes.md)

[Set-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)

