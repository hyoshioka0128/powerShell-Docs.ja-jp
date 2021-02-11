---
title: スクリプト モジュール
description: スクリプト モジュールを使用すると、スクリプトや関数を、再利用可能なツールに容易にパッケージ化できます。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: c557c071bc202a4216a77e7e5ae0bd73b4bc014b
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99601190"
---
# <a name="chapter-10---script-modules"></a>第 10 章 - スクリプト モジュール

PowerShell のワンライナーやスクリプトを頻繁に使用する予定がある場合は、それを再利用可能なツールに変換するという作業が、より重要になってきます。 関数をスクリプト モジュールにパッケージ化すると、プロらしい外観になり、共有しやすくなります。

## <a name="dot-sourcing-functions"></a>ドット ソース関数

前の章で言及しなかったのが、ドット ソース関数です。 スクリプトの関数がモジュールの一部ではない場合、その関数をメモリに読み込むには、それが保存されている `.PS1` ファイルをドットソースするしかありません。

次の関数は、`Get-MrPSVersion.ps1` として保存されています。

```powershell
function Get-MrPSVersion {
    $PSVersionTable
}
```

スクリプトを実行しても、何も起こりません。

```powershell
.\Get-MrPSVersion.ps1
```

関数を呼び出そうとすると、エラー メッセージが生成されます。

```powershell
Get-MrPSVersion
```

```Output
Get-MrPSVersion : The term 'Get-MrPSVersion' is not recognized as the name of a cmdlet,
function, script file, or operable program. Check the spelling of the name, or if a path
was included, verify that the path is correct and try again.
At line:1 char:1
+ Get-MrPSVersion
    + CategoryInfo          : ObjectNotFound: (Get-MrPSVersion:String) [], CommandNotFou
   ndException
    + FullyQualifiedErrorId : CommandNotFoundException

```

関数がメモリに読み込まれているかどうかを確認するには、その関数が **Function** PSDrive に存在するかどうかをチェックします。

```powershell
Get-ChildItem -Path Function:\Get-MrPSVersion
```

```Output
Get-ChildItem : Cannot find path 'Get-MrPSVersion' because it does not exist.
At line:1 char:1
+ Get-ChildItem -Path Function:\Get-MrPSVersion
    + CategoryInfo          : ObjectNotFound: (Get-MrPSVersion:String) [Get-ChildItem],
   ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand
```

関数を含むスクリプトを呼び出すときに問題が発生するのは、その関数が _Script_ スコープに読み込まれているからです。 スクリプトが完了すると、そのスコープは削除され、そのスコープと一緒に関数も削除されます。

関数は _Global_ スコープに読み込む必要があります。 これを行うには、関数を含むスクリプトをドットソースします。 相対パスを使用できます。

```powershell
. .\Get-MrPSVersion.ps1
```

絶対パスも使用できます。

```powershell
. C:\Demo\Get-MrPSVersion.ps1
```

パスの一部が変数に格納されている場合、その部分は、パスの残りの部分と結合できます。
文字列連結を使用して、変数とパスの残りの部分を結合する理由はありません。

```powershell
$Path = 'C:\'
. $Path\Get-MrPSVersion.ps1
```

ここで **Function** PSDrive を確認すると、`Get-MrPSVersion` 関数が存在しています。

```powershell
Get-ChildItem -Path Function:\Get-MrPSVersion
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-MrPSVersion
```

## <a name="script-modules"></a>スクリプト モジュール

PowerShell のスクリプト モジュールは、1 つ以上の関数を含むファイルに過ぎず、`.PS1` ファイルではなく `.PSM1` ファイルとして保存されます。

スクリプト モジュールを作成するには、どうすればよいのでしょうか。 `New-Module` のような名前のコマンドを使って作成すると思われるかもしれませんが、 それは間違っています。 PowerShell には `New-Module` という名前のコマンドがありますが、このコマンドによって作成されるのは動的モジュールで、スクリプト モジュールではありません。 必要なコマンドが見つかった、と思っても、必ずコマンドのヘルプを読んでください。

```powershell
help New-Module
```

```Output
NAME
    New-Module

SYNOPSIS
    Creates a new dynamic module that exists only in memory.

SYNTAX
    New-Module [-Name] <String> [-ScriptBlock] <ScriptBlock> [-ArgumentList <Object[]>]
    [-AsCustomObject] [-Cmdlet <String[]>] [-Function <String[]>] [-ReturnResult]
    [<CommonParameters>]

DESCRIPTION
    The New-Module cmdlet creates a dynamic module from a script block. The members of
    the dynamic module, such as functions and variables, are immediately available in
    the session and remain available until you close the session.

    Like static modules, by default, the cmdlets and functions in a dynamic module are
    exported and the variables and aliases are not. However, you can use the
    Export-ModuleMember cmdlet and the parameters of New-Module to override the defaults.

    You can also use the AsCustomObject parameter of New-Module to return the dynamic
    module as a custom object. The members of the modules, such as functions, are
    implemented as script methods of the custom object instead of being imported into
    the session.

    Dynamic modules exist only in memory, not on disk. Like all modules, the members of
    dynamic modules run in a private module scope that is a child of the global scope.
    Get-Module cannot get a dynamic module, but Get-Command can get the exported members.

    To make a dynamic module available to Get-Module , pipe a New-Module command to
    Import-Module, or pipe the module object that New-Module returns to Import-Module .
    This action adds the dynamic module to the Get-Module list, but it does not save the
    module to disk or make it persistent.

RELATED LINKS
    Online Version: http://go.microsoft.com/fwlink/?LinkId=821495
    Export-ModuleMember
    Get-Module
    Import-Module
    Remove-Module

REMARKS
    To see the examples, type: "get-help New-Module -examples".
    For more information, type: "get-help New-Module -detailed".
    For technical information, type: "get-help New-Module -full".
    For online help, type: "get-help New-Module -online"
```

前の章で、関数では承認されている動詞を使用する必要があり、そうしないとモジュールのインポート時に警告メッセージが生成されると言いました。 次のコードは、`New-Module` コマンドレットを使って、メモリで動的モジュールを作成しています。 このモジュールは、未承認の動詞に関する警告を示しています。

```powershell
New-Module -Name MyModule -ScriptBlock {

    function Return-MrOsVersion {
        Get-CimInstance -ClassName Win32_OperatingSystem |
        Select-Object -Property @{label='OperatingSystem';expression={$_.Caption}}
    }

    Export-ModuleMember -Function Return-MrOsVersion

} | Import-Module
```

```Output
WARNING: The names of some imported commands from the module 'MyModule' include
unapproved verbs that might make them less discoverable. To find the commands with
unapproved verbs, run the Import-Module command again with the Verbose parameter. For a
list of approved verbs, type Get-Verb.
```

繰り返しになりますが、この例では `New-Module` コマンドレットが使用されましたが、これは PowerShell でスクリプト モジュールを作成するコマンドではありません。

次の 2 つの関数を `MyScriptModule.psm1` という名前のファイルに保存します。

```powershell
function Get-MrPSVersion {
    $PSVersionTable
}

function Get-MrComputerName {
    $env:COMPUTERNAME
}
```

関数のいずれかを呼び出してみます。

```powershell
Get-MrComputerName
```

```Output
Get-MrComputerName : The term 'Get-MrComputerName' is not recognized as the name of a
cmdlet, function, script file, or operable program. Check the spelling of the name, or
if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ Get-MrComputerName
    + CategoryInfo          : ObjectNotFound: (Get-MrComputerName:String) [], CommandNot
   FoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

関数が見つからないというエラー メッセージが生成されます。 ここでも、前と同じように **Function** PSDrive を確認でき、そこに関数が存在しないことがわかります。

`Import-Module` コマンドレットを使用して、ファイルをインポートします。

```powershell
Import-Module C:\MyScriptModule.psm1
```

モジュールの自動読み込み機能は、PowerShell バージョン 3 で導入されました。 モジュールの自動読み込みを利用するには、スクリプト モジュールを、`$env:PSModulePath` で指定された場所にある、`.PSM1` ファイルと同じ基本名を持つフォルダーに保存する必要があります。

```powershell
$env:PSModulePath
```

```Output
C:\Users\mike-ladm\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShell\
Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modules;C:\Program Files (x86)\Microsof
t SQL Server\130\Tools\PowerShell\Modules\
```

読みにくい結果が出力されます。 それぞれのパスがセミコロンで区切られているので、結果を分割して、パスごとに改行します。 これにより、読みやすくなります。

```powershell
$env:PSModulePath -split ';'
```

```Output
C:\Users\mike-ladm\Documents\WindowsPowerShell\Modules
C:\Program Files\WindowsPowerShell\Modules
C:\Windows\system32\WindowsPowerShell\v1.0\Modules
C:\Program Files (x86)\Microsoft SQL Server\130\Tools\PowerShell\Modules\
```

一覧に示されている最初の 3 つは、既定のパスです。 SQL Server Management Studio がインストールされている場合は、最後のパスが追加されます。 モジュールの自動読み込みを機能させるには、`MyScriptModule.psm1` ファイルを、これらのパスのいずれかにある `MyScriptModule` という名前のフォルダーに直接配置する必要があります。

慌てないでください。 私の現在のユーザー パスは、この一覧の先頭のパスではありません。 私が Windows にログインするときのユーザーは、PowerShell を実行するときに使うユーザーとは異なるため、このパスはほとんど使用しません。 つまり、そのフォルダーは、通常のドキュメント フォルダーには存在しません。

2 番目のパスは、**AllUsers** パスです。 この場所には、私のすべてのモジュールが格納されます。

3 番目のパスは `C:\Windows\System32` の下にあります。 この場所は、オペレーティング システム フォルダー内にあるため、ここにモジュールを格納する必要があるのは Microsoft だけです。

`.PSM1` ファイルが正しいパスに配置されると、そのコマンドの 1 つが呼び出されたときに、モジュールは自動的に読み込まれます。

## <a name="module-manifests"></a>モジュール マニフェスト

すべてのモジュールにモジュール マニフェストが必要です。 モジュール マニフェストには、モジュールに関するメタデータが含まれています。
モジュール マニフェスト ファイルのファイル名拡張子は `.PSD1` です。 `.PSD1` 拡張子のファイルすべてがモジュール マニフェストであるとは限りません。 この拡張子は、DSC 構成の環境部分の格納などの用途にも使用できます。 `New-ModuleManifest` を使用すると、モジュール マニフェストを作成できます。 必須の値は **Path** のみですが、 **RootModule** が指定されていないと、モジュールは機能しません。 PowerShellGet を使用してモジュールを NuGet リポジトリにアップロードする場合に備えて、**Author** と **Description** を指定することをお勧めします。そのシナリオでは、この値が必要になります。

マニフェストのないモジュールのバージョンは 0.0 です。 これにより、モジュールにマニフェストがないことは一目瞭然です。

```powershell
Get-Module -Name MyScriptModule
```

```Output
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     0.0        myscriptmodule                      {Get-MrComputerName, Get-MrP...
```

モジュール マニフェストは、すべての推奨情報を使用して作成できます。

```powershell
New-ModuleManifest -Path $env:ProgramFiles\WindowsPowerShell\Modules\MyScriptModule\MyScriptModule.psd1 -RootModule MyScriptModule -Author 'Mike F Robbins' -Description 'MyScriptModule' -CompanyName 'mikefrobbins.com'
```

最初に作成したモジュール マニフェストに、この情報のいずれかが欠落していた場合は、`Update-ModuleManifest` を使って後で追加または更新できます。 既に作成されているマニフェストについては、`New-ModuleManifest` を使って再作成しないでください。再作成すると、GUID が変更されてしまいます。

## <a name="defining-public-and-private-functions"></a>パブリック関数とプライベート関数の定義

ご自身のヘルパー関数をプライベートにして、モジュール内の他の関数からのみアクセスできるようにしたいことがあります。 モジュールのユーザーがアクセスできるようにするつもりはありません。 これを実現する方法はいくつかあります。

ベスト プラクティスに従っておらず、`.PSM1` ファイルしかない場合、`Export-ModuleMember` コマンドレットを使用する以外の方法はありません。

```powershell
function Get-MrPSVersion {
    $PSVersionTable
}

function Get-MrComputerName {
    $env:COMPUTERNAME
}

Export-ModuleMember -Function Get-MrPSVersion
```

この例では、モジュールのユーザーは、`Get-MrPSVersion` 関数にのみアクセスできます。`Get-MrComputerName` 関数は、モジュール自体にある他の関数が使用できます。

```powershell
Get-Command -Module MyScriptModule

CommandType     Name                        Version    Source
-----------     ----                        -------    ------
Function        Get-MrPSVersion             1.0        MyScript...
```

モジュール マニフェストをご自身のモジュールに追加した場合は (追加する必要がある場合は)、モジュール マニフェストの **FunctionsToExport**  セクションで、エクスポートする関数を個別に指定することをお勧めします。

```powershell
FunctionsToExport = 'Get-MrPSVersion'
```

`.PSM1` ファイルの `Export-ModuleMember` と、モジュール マニフェストの **FunctionsToExport** セクションの両方を使用する必要はありません。 どちらか一方で十分です。

## <a name="summary"></a>まとめ

この章では、PowerShell で関数をスクリプト モジュールに変換する方法を学習しました。 また、スクリプト モジュールのモジュール マニフェストの作成など、スクリプト モジュール作成のベスト プラクティスについてもいくつか学びました。

## <a name="review"></a>確認

1. PowerShell でスクリプト モジュールを作成するには、どうすればよいですか。
1. 承認されている動詞を使用することが関数にとって重要なのは、なぜですか。
1. PowerShell でモジュール マニフェストを作成するには、どうすればよいですか。
1. モジュールから特定の関数のみをエクスポートする 2 つのオプションは何ですか。
1. コマンドが呼び出されたときのモジュールの自動読み込みには何が必要ですか。

## <a name="recommended-reading"></a>推奨トピック

- [PowerShell スクリプト モジュールとモジュール マニフェストを作成する方法][]
- [about_Modules][]
- [New-ModuleManifest][]
- [Export-ModuleMember][]

<!-- link references -->
[PowerShell スクリプト モジュールとモジュール マニフェストを作成する方法]: https://mikefrobbins.com/2013/07/04/how-to-create-powershell-script-modules-and-module-manifests/
[about_Modules]: /powershell/module/microsoft.powershell.core/about/about_modules
[New-ModuleManifest]: /powershell/module/microsoft.powershell.core/new-modulemanifest
[Export-ModuleMember]: /powershell/module/microsoft.powershell.core/export-modulemember
