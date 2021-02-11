---
description: PSModulePath 環境変数には、モジュールとリソースを検索するために検索されるフォルダーの場所の一覧が含まれています。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_PSModulePath?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PSModulePath
ms.openlocfilehash: 6e0307996bf619bc887b076a1f8c0faa619964fe
ms.sourcegitcommit: aac365f7813756e16b59322832a904e703e0465b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2020
ms.locfileid: "94524475"
---
# <a name="about-psmodulepath"></a>PSModulePath について

環境変数には、 `$env:PSModulePath` モジュールとリソースを検索するために検索されるフォルダーの場所の一覧が含まれています。 PowerShell は、各フォルダーでモジュール ( `.psd1` または) ファイルを再帰的に検索 `.psm1` します。

既定では、に割り当てられて `$env:PSModulePath` いる有効な場所は次のとおりです。

- システム全体の場所: これらのフォルダーには、PowerShell に付属しているモジュールが含まれています。 これらのモジュールは、フォルダーに格納されてい `$PSHOME\Modules` ます。 これは、Windows 管理モジュールがインストールされている場所でもあります。

- ユーザーがインストールしたモジュール: これらはユーザーによってインストールされたモジュールです。
  `Install-Module` には、現在のユーザーまたはすべてのユーザーに対してモジュールがインストールされているかどうかを指定できる **スコープ** パラメーターがあります。 詳細については、「 [Install-Module](xref:PowerShellGet.Install-Module)」を参照してください。

  - Windows では、ユーザー固有の **CurrentUser** スコープの場所は `$HOME\Documents\PowerShell\Modules` フォルダーです。 **AllUsers** スコープの場所は `$env:ProgramFiles\PowerShell\Modules` です。
  - Windows 以外のシステムでは、ユーザー固有の **CurrentUser** スコープの場所は `$HOME/.local/share/powershell/Modules` フォルダーです。 **AllUsers** スコープの場所は `/usr/local/share/powershell/Modules` です。

また、Program Files ディレクトリなど、他のディレクトリにモジュールをインストールするセットアッププログラムでは、の値にその場所を追加でき `$env:PSModulePath` ます。

> [!NOTE]
> Windows では、ユーザー固有の場所は、 `PowerShell\Modules` ユーザープロファイルの **Documents** フォルダーにあるフォルダーです。 その場所の特定のパスは、Windows のバージョンと、フォルダーリダイレクトを使用しているかどうかによって異なります。 Microsoft OneDrive では、 **ドキュメント** フォルダーの場所を変更することもできます。 次のコマンドを使用して、 **ドキュメント** フォルダーの場所を確認でき `[Environment]::GetFolderPath('MyDocuments')` ます。

## <a name="modifying-psmodulepath"></a>PSModulePath の変更

現在のセッションの既定のモジュールディレクトリを変更するには、次のコマンド形式を使用して環境変数の値を変更し `PSModulePath` ます。

たとえば、 `C:\Program Files\Fabrikam\Modules` PSModulePath 環境変数の値にディレクトリを追加するには、次のように入力します。

```powershell
$Env:PSModulePath = $Env:PSModulePath+";C:\Program Files\Fabrikam\Modules"
```

コマンドのセミコロン () は、 `;` 新しいパスをリスト内でその前にあるパスと分離します。 Windows 以外のプラットフォームでは、コロン () によって `:` 環境変数内のパスの場所が分離されます。

すべてのセッションでの値を変更するに `PSModulePath` は、前のコマンドを PowerShell プロファイルに追加するか、 **環境** クラスの **SetEnvironmentVariable** メソッドを使用します。

次のコマンドは、 **GetEnvironmentVariable** メソッドを使用してコンピューターの設定を取得し、SetEnvironmentVariable メソッドを使用してその `PSModulePath` パスを値に追加し **SetEnvironmentVariable** `C:\Program Files\Fabrikam\Modules` ます。

```powershell
$path = [Environment]::GetEnvironmentVariable('PSModulePath', 'Machine')
$newpath = $path + ';C:\Program Files\Fabrikam\Modules'
[Environment]::SetEnvironmentVariable('PSModulePath', $newpath, 'Machine')
```

ユーザー設定のパスを追加するには、ターゲットの値を **user** に変更します。

```powershell
$path = [Environment]::GetEnvironmentVariable('PSModulePath', 'User')
$newpath = $path + ';C:\Program Files\Fabrikam\Modules'
[Environment]::SetEnvironmentVariable('PSModulePath', $newpath, 'User')
```

System.object クラスのメソッドの詳細については、「 [環境メソッド](/dotnet/api/system.environment)」を参照して **ください。**

## <a name="powershell-psmodulepath-construction"></a>PowerShell PSModulePath の構築

の値は、 `$env:PSModulePath` PowerShell が開始されるたびに構築されます。
値は PowerShell のバージョンと起動方法によって異なります。

### <a name="windows-powershell-startup"></a>Windows PowerShell の起動

Windows PowerShell は、起動時にを構築するために次のロジックを使用し `PSModulePath` ます。

- が存在しない場合は、 `PSModulePath` **CurrentUser** 、 **AllUsers** 、および modules の各パスを結合します。 `$PSHOME`
- `PSModulePath`が存在する場合:
  - に `PSModulePath` モジュールパスが含まれている場合 `$PSHOME` :
    - モジュールパスの前に **AllUsers** モジュールパスが挿入されます `$PSHOME`
  - さも
    - `PSModulePath`ユーザーが意図的に場所を削除した後に定義されたを使用する `$PSHOME`

**CurrentUser** モジュールパスは、ユーザースコープが存在しない場合にのみプレフィックスが付けられ `$env:PSModulePath` ます。 それ以外の場合 `$env:PSModulePath` は、定義に従ってユーザースコープが使用されます。

### <a name="powershell-core-6-startup"></a>PowerShell Core 6 スタートアップ

Powershell Core 6 は `$env:PSModulePath` 、powershell から開始されたことを検出すると、の内容を使用しません。 次のように上書きされます。

- **CurrentUser** モジュールパス + **AllUsers** モジュールパス + `$PSHOME` モジュールパス + Windows PowerShell `$PSHOME` モジュールパス。

### <a name="powershell-7-startup"></a>PowerShell 7 スタートアップ

Windows では、ほとんどの環境変数で、ユーザースコープ変数が存在する場合、同じ名前のマシンスコープ変数が存在する場合でも、新しいプロセスでその値が使用されます。

PowerShell 7 で `PSModulePath` は、は、 `Path` 環境変数が Windows でどのように扱われるかと同様に扱われます。 Windows で `Path` は、は他の環境変数とは異なる方法で処理されます。 プロセスが開始されると、Windows はユーザースコープ `Path` とコンピュータースコープを組み合わせ `Path` ます。

- ユーザースコープを取得します。 `PSModulePath`
- プロセス継承環境変数との比較 `PSModulePath`
  - 同じ場合:
    - **AllUsers** `PSModulePath` 環境変数のセマンティクスに従って、最後に AllUsers を追加します。 `Path`
    - Windows パスは、 `System32` 定義されたコンピューターから取得さ `PSModulePath` れるため、明示的に追加する必要はありません。
  - 異なる場合は、ユーザーが明示的に変更したものとして扱い、 **AllUsers** を追加しません。 `PSModulePath`
- PS7 User、System、および path を `$PSHOME` その順序でプレフィックスとして使用する
  - に `powershell.config.json` ユーザースコープが含まれている場合は、 `PSModulePath` ユーザーの既定値の代わりにこれを使用します。
  - に `powershell.config.json` スコープのあるシステムが含まれている場合は、 `PSModulePath` システムの既定のではなく、これを使用します。

Unix システムでは、ユーザー環境変数とシステム環境変数が分離されていません。
`PSModulePath` が継承され、まだ定義されていない場合は、PS7 固有のパスにプレフィックスが付けられます。

### <a name="starting-windows-powershell-from-powershell-7"></a>PowerShell 7 から Windows PowerShell を開始する

この説明では、 _Windows PowerShell_ はとの両方を意味し `powershell.exe` `powershell_ise.exe` ます。

の値 `$env:PSModulePath` は、 `WinPSModulePath` 次の変更によってにコピーされます。

- ユーザーモジュールパスの PS7 を削除します。
- システムモジュールパスの PS7 を削除します。
- モジュールパスの PS7 を削除します。 `$PSHOME`

PS7 モジュールが Windows PowerShell に読み込まれないように、PS7 パスが削除されます。 この `WinPSModulePath` 値は、Windows PowerShell を起動するときに使用されます。

### <a name="starting-powershell-7-from-windows-powershell"></a>Windows PowerShell から PowerShell 7 を開始する

PowerShell 7 のスタートアップは、Windows PowerShell によって追加された継承パスを追加することでそのままの状態で続行されます。 PS7 固有のパスにはプレフィックスが付いているため、機能上の問題はありません。

### <a name="starting-powershell-6-from-powershell-7"></a>Powershell 7 から PowerShell 6 を開始する

PowerShell Core 6 は上書きさ `$env:PSModulePath` れます。 変更は行われません。

### <a name="starting-powershell-7-from-powershell-6"></a>Powershell 6 から powershell 7 を開始する

Powershell 7 のスタートアップは、PowerShell Core 6 によって追加された継承パスを追加することでそのままの状態で続行されます。 PS7 固有のパスにはプレフィックスが付いているため、機能上の問題はありません。

## <a name="module-search-behavior"></a>モジュールの検索動作

PowerShell は、 **PSModulePath** のモジュール ( `.psd1` または) ファイルの各フォルダーを再帰的に検索し `.psm1` ます。 この検索パターンを使用すると、同じモジュールの複数のバージョンを異なるフォルダーにインストールできます。 次に例を示します。

```Output
    Directory: C:\Program Files\WindowsPowerShell\Modules\PowerShellGet

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           8/14/2020  5:56 PM                1.0.0.1
d----           9/13/2019  3:53 PM                2.1.2
```

既定では、複数のバージョンが検出されると、PowerShell はモジュールの最大バージョン番号を読み込みます。 特定のバージョンを読み込むには、FullyQualifiedName パラメーターを指定してを使用し `Import-Module` ます。 **FullyQualifiedName** 詳細については、「[Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)」を参照してください。

## <a name="see-also"></a>関連項目

- [about_Modules](about_Modules.md)
- [環境メソッド](/dotnet/api/system.environment)
