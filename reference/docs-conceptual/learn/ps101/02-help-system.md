---
title: ヘルプ システム
description: ヘルプ システムを使いこなすことは、PowerShell を使って成功するための鍵です。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: cfb12f57b7bb6c514f4e19a93dfe9c77245bd977
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99604927"
---
# <a name="chapter-2---the-help-system"></a>第 2 章 - ヘルプ システム

IT プロフェッショナルの 2 つのグループを対象に、PowerShell のスキル レベルを判断するための筆記テストを実施しました。このテストは、コンピューターは使用せずに行われ、 対象グループの一方は PowerShell 初心者、もう一方はエキスパートで構成されていました。
このテスト結果では、2 つのグループのスキル レベルに大きな違いはないように見えました。 その後、両方のグループに 2 回目のテストを実施しました。前回のテストと内容は似ていますが、 今回はテスト中に、インターネットにアクセスできない PowerShell が搭載されたコンピューターを使うことができました。 2 回目のテストの結果では、2 つのグループのスキル レベルの差は歴然としていました。 エキスパートは必ずしも答えを知っているわけではありません。しかし、答えを導き出す手段がわかっているのです。

2 つのグループの最初のテストと 2 番目のテストの結果の違いは何だったのでしょう。

2 つのテストの結果に違いが出た原因は、エキスパートが、PowerShell で何千ものコマンドを使用する方法を丸暗記していないからです。 エキスパートは、PowerShell のヘルプ システムを使用する方法について、よくわかっています。
だから、必要なときに必要なコマンドを見つけ、コマンドが見つかったら、そのコマンドの使用方法を見つけることができるのです。

PowerShell の発明者 Jeffrey Snover が同じようなことを話しているのを、私は幾度となく聞いています。

ヘルプ システムを使いこなすことは、PowerShell を使って成功するための鍵です。

## <a name="discoverability"></a>Discoverability (探索可能性)

PowerShell のコンパイルされたコマンドはコマンドレットと呼ばれています。 コマンドレットは、"command-let" と発音します (CMD let ではありません)。 コマンドレット名は、検出しやすいように、単数形の "動詞-名詞" コマンドの形式になっています。 たとえば、実行中のプロセスを確認するためのコマンドレットは `Get-Process`、サービスの一覧とその状態を取得するためのコマンドレットは `Get-Service` です。 本書で後述しますが、PowerShell には、エイリアス、関数など、他の種類のコマンドもあります。 PowerShell コマンドという用語は、それがコマンドレット、関数、エイリアスかどうかに関係なく、PowerShell のすべての種類のコマンドを意味する一般的な用語です。

## <a name="the-three-core-cmdlets-in-powershell"></a>PowerShell の 3 つのコア コマンドレット

- `Get-Command`
- `Get-Help`
- `Get-Member` (第 3 章で説明)

よく聞かれるのが、PowerShell で使用するコマンドを判断する方法です。 コマンドの決定には、`Get-Command` と `Get-Help` の両方を使用できます。

## <a name="get-help"></a>Get-Help

`Get-Help` は多目的コマンドです。 `Get-Help` は、コマンドを見つけた後に、そのコマンドの使用方法を確認するうえで役立ちます。 `Get-Help` を使ってコマンドを特定することもできますが、`Get-Command` とは異なり、このコマンドと比べるとより間接的です。

`Get-Help` を使用してコマンドを検索すると、まず入力内容に基づいて、コマンド名のワイルドカード一致が検索されます。 一致するものが見つからないと、ヘルプ トピックが検索され、そこで一致するものが見つからないと、エラーが返されます。 一般的な考えに反して、`Get-Help` を使って、ヘルプ トピックのないコマンドを見つけることもできます。

PowerShell のヘルプ システムでまず知っておく必要があるのが、`Get-Help` コマンドレットを使用する方法です。 次のコマンドを使用すると、`Get-Help` のヘルプ トピックが表示されます。

```powershell
Get-Help -Name Get-Help
```

```Output
Do you want to run Update-Help?
The Update-Help cmdlet downloads the most current Help files for Windows PowerShell
modules, and installs them on your computer. For more information about the Update-Help
cmdlet, see http://go.microsoft.com/fwlink/?LinkId=210614.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
```

PowerShell バージョン 3 以降、PowerShell ヘルプはオペレーティング システムに付属していません。 コマンドに対して初めて `Get-Help` を実行すると、このメッセージが表示されます。 `Get-Help` コマンドレットではなく `help` 関数または `man` エイリアスを使用すると、このプロンプトは表示されません。

<kbd>Y</kbd> 押して [はい] を選択すると、`Update-Help` コマンドレットが実行されます。既定では、このコマンドレットにはインターネット アクセスが必要です。 `Y` は、大文字と小文字のどちらでも指定できます。

ヘルプがダウンロードされ、更新が完了すると、指定したコマンドのヘルプ トピックが返されます。

```powershell
Get-Help -Name Get-Help
```

ここでコンピューターでこの例を実行し、出力を確認して、情報がどのようにグループ化されているかをメモしておきます。

- NAME
- 概要
- SYNTAX
- Description
- 関連リンク
- REMARKS

ご覧のように、ヘルプ トピックには膨大な情報を含めることができますが、これでもヘルプ トピック全体ではありません。

PowerShell に限ったことではりませんが、パラメーターは、コマンドに入力を提供する手段です。 `Get-Help` には、ヘルプ トピック全体またはヘルプ トピックのサブセットを取得するためのパラメーターが多数用意されています。

前の結果セットに示されているヘルプ トピックの構文セクションには、`Get-Help` のパラメーターの一覧が表示されています。 一見すると、同じパラメーターが 6 つの異なる時刻に表示されているようですが、 構文セクションのそれぞれのブロックが、パラメーター セットです。 つまり、`Get-Help` コマンドレットには 6 つの異なるパラメーター セットがあります。 さらに詳しく見ると、各パラメーター セットで、少なくとも 1 つのパラメーターが違っていることがわかります。

パラメーター セットを同時に使用することはできません。 1 つのパラメーター セットにのみ存在する一意のパラメーターが使用されている場合、使用できるのは、そのパラメーター セットに含まれるパラメーターだけです。 たとえば、**Full** パラメーターと **Detailed** パラメーターは、それぞれ異なるパラメーター セットに含まれているため、両方を同時に指定することはできません。

次のパラメーターはそれぞれ、異なるパラメーター セットに含まれています。

- [完全]
- 詳細
- 例
- オンライン
- パラメーター
- ShowWindow

構文セクションに示されている、角かっこや山かっこなどのわかりにくい構文すべてに意味がありますが、これについては、本書の付録 A で説明します。 重要ではありますが、PowerShell を初めて使うユーザーや、毎日使っているわけではないユーザーにとって、わかりにくい構文を把握するのが難しいことは少なくありません。

わかりにくい構文の詳細については、[付録 A:][] を参照してください。

初心者を対象とした、より簡単な方法で同じ情報を確認する方法があります (簡単な言語を除く)。

`Get-Help` の **Full** パラメーター を指定すると、ヘルプ トピック全体が返されます。

```powershell
Get-Help -Name Get-Help -Full
```

ここでコンピューターでこの例を実行し、出力を確認して、情報がどのようにグループ化されているかをメモしておきます。

- NAME
- 概要
- SYNTAX
- Description
- PARAMETERS
- 入力
- 出力
- 注
- 例
- 関連リンク

**Full** パラメーターを使用すると、追加セクションがいくつか返されます。そのうちの 1 つがパラメーター セクションで、このセクションには、わかりにくい構文セクションよりも多くの情報が示されています。

**Full** パラメーターはスイッチ パラメーターです。 値を必要としないパラメーターが、スイッチ パラメーターと呼ばれます。 スイッチ パラメーターが指定されている場合、その値は true であり、そうでない場合、値は false です。

この章の PowerShell コンソールでの作業では、`Get-Help` のヘルプ トピック全体を表示するための前のコマンドはあっという間に過ぎてしまい、読む時間がありませんでした。 もっと良い方法があります。

`Help` は、`more` という名前の関数に対して `Get-Help` をパイプ処理する関数で、これは Windows の `more.com` 実行可能ファイルのラッパーです。 PowerShell コンソールでは、`help` は 1 ページずつヘルプを提供します。 ISE では、`Get-Help` と同じように動作します。 操作性に優れ、入力も少なくて済むため、`Get-Help` コマンドレットではなく `help` 関数を使用することをお勧めします。

ただし、入力が少ないことが必ずしも良いとは限りません。 コマンドをスクリプトとして保存したり、他のユーザーと共有したりする場合は、必ず完全なコマンドレットとパラメーター名を使用してください。 完全な名前は自己文書化されるため、理解しやすくなります。 ご自身のコマンドを次に読んで理解する必要があるユーザーのことを考えてください。 それは自分自身かもしれません。 同僚そして未来の自分が、ありがたいと思うでしょう。

Windows 10 ラボ環境のコンピューターの PowerShell コンソールで、次のコマンドを実行してみます。

```powershell
Get-Help -Name Get-Help -Full
help -Name Get-Help -Full
help Get-Help -Full
```

Windows 10 ラボ環境のコンピューターで実行したとき、前に示したコマンドの出力と違いがあることに気が付きましたか。

最後の 2 つのオプションが 1 ページずつ結果を返すこと以外、違いはありません。
`Help` 関数を使用している場合、<kbd>スペース</kbd> キーを使用すると、コンテンツの次のページが表示されます。また、<kbd>Ctrl </kbd> + <kbd> C </kbd> キーを押すと、PowerShell コンソールで実行されているコマンドがキャンセルされます。

最初の例では `Get-Help` コマンドレットが使用され、2 番目の例では `Help` 関数が使用されます。3 番目の例では、`Help` 関数が使用され、**Name** パラメーターが省略されています。 **Name** は位置指定パラメーターであり、この例では位置指定に使用されています。 つまり、値自体が正しい位置で指定されていれば、パラメーター名を指定しなくても値を指定できます。 値を指定する位置はどのようにわかったのでしょうか。 次の例に示すヘルプを読めばわかります。

```powershell
help Get-Help -Parameter Name
```

```Output
-Name <String>
    Gets help about the specified command or concept. Enter the name of a cmdlet, function,
    provider, script, or workflow, such as Get-Member, a conceptual article name, such as
    about_Objects, or an alias, such as ls. Wildcard characters are permitted in cmdlet and
    provider names, but you can't use wildcard characters to find the names of function help and
    script help articles.

    To get help for a script that isn't located in a path that's listed in the $env:Path
    environment variable, type the script's path and file name.

    If you enter the exact name of a help article, Get-Help displays the article contents.

    If you enter a word or word pattern that appears in several help article titles, Get-Help
    displays a list of the matching titles.

    If you enter a word that doesn't match any help article titles, Get-Help displays a list of
    articles that include that word in their contents.

    The names of conceptual articles, such as about_Objects, must be entered in English, even in
    non-English versions of PowerShell.

    Required?                    false
    Position?                    0
    Default value                None
    Accept pipeline input?       True (ByPropertyName)
    Accept wildcard characters?  true
```

この例では、**Parameter** パラメーターが Help 関数と共に使用され、**Name** パラメーターのヘルプ トピックの情報だけが返されていることに注意してください。 これは、100 ページにも見えることがあるヘルプ トピックを手作業で詳しく調べようとするよりも、はるかに簡潔です。

これらの結果から、**Name** パラメーターが位置指定パラメーターで、位置指定に使用する場合は、位置 0 (最初の位置) で指定する必要があることがわかります。 パラメーター名が指定されている場合、パラメーターを指定する順序は重要ではありません。

もう 1 つ重要なのは、**Name** パラメーターでは、その値のデータ型が、`<String>` によって示される 1 つの文字列であることが想定されることです。 複数の文字列が受け入れられると、データ型は `<String[]>` として表示されます。

場合によっては、コマンドのヘルプ トピック全体を単に表示したくないこともあります。 `Get-Help` や `Help`で指定できる **Full** 以外にもパラメーターは多数あります。 Windows 10 ラボ環境のコンピューターで、次のコマンドを実行してみます。

```powershell
Get-Help -Name Get-Command -Full
Get-Help -Name Get-Command -Detailed
Get-Help -Name Get-Command -Examples
Get-Help -Name Get-Command -Online
Get-Help -Name Get-Command -Parameter Noun
Get-Help -Name Get-Command -ShowWindow
```

私はいつも、`help <command name>` は、**Full** パラメーターまたは **Online** パラメーターと一緒に使用します。 例だけに関心がある場合は、**Examples** パラメーターを、特定のパラメーターにのみ関心がある場合は、**Parameter** パラメーターを使用します。 **ShowWindow** パラメーターを使用すると、別の検索可能なウィンドウでヘルプ トピックが開きます。複数のモニターがある場合は、それを別のモニターに配置できます。 この **ShowWindow** パラメーターには、ヘルプ トピック全体が表示されないという既知のバグがあるため、私は避けました。

別のウィンドウでヘルプを確認したい場合は、次の例に示すように、**Online** パラメーターを使用するか、**Full** パラメーターを使用して、結果を `Out-GridView` にパイプ処理することをお勧めします。

```powershell
help Get-Command -Full | Out-GridView
```

`Out-GridView` コマンドレットと、`Get-Help` コマンドレットの **ShowWindow** パラメーターの両方に、GUI (グラフィカル ユーザー インターフェイス) を備えたオペレーティング システムが必要です。 サーバー コア (GUI なし) インストール オプションを使用してインストールされた Windows Server 上で、いずれか一方を使おうとすると、エラー メッセージが生成されます。

`Get-Help` を使用してコマンドを検索するには、アスタリスク (`*`) ワイルドカード文字を **Name** パラメーターで使用します。 次の例に示すように、**Name** パラメーターの値として、コマンドに対する検索用語を指定します。

```powershell
help *process*
```

```Output
Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Enter-PSHostProcess               Cmdlet    Microsoft.PowerShell.Core Connects to and ...
Exit-PSHostProcess                Cmdlet    Microsoft.PowerShell.Core Closes an intera...
Get-PSHostProcessInfo             Cmdlet    Microsoft.PowerShell.Core
Debug-Process                     Cmdlet    Microsoft.PowerShell.M... Debugs one or mo...
Get-Process                       Cmdlet    Microsoft.PowerShell.M... Gets the process...
Start-Process                     Cmdlet    Microsoft.PowerShell.M... Starts one or mo...
Stop-Process                      Cmdlet    Microsoft.PowerShell.M... Stops one or mor...
Wait-Process                      Cmdlet    Microsoft.PowerShell.M... Waits for the pr...
Get-AppvVirtualProcess            Function  AppvClient                ...
Start-AppvVirtualProcess          Function  AppvClient                ...
```

この例では、`*` ワイルドカード文字は不要で、省略しても、同じ結果が生成されます。 `Get-Help` では、ワイルドカード文字がバックグラウンドで自動的に追加されます。

```powershell
help process
```

このコマンドでは、プロセスが終了するたびに、`*` ワイルドカード文字を指定した場合と同じ結果が生成されます。

このオプションの動作はいつも一貫しているため、私は好んで追加しますが、 これが必要なのは特定のシナリオだけで、他では不要です。 値の途中にワイルドカード文字を追加するとすぐに、指定した値に自動的に追加されることはなくなります。

```powershell
help pr*cess
```

`pr*cess` の先頭または末尾、あるいはその両方に `*` ワイルドカード文字が追加されない限り、そのコマンドによって結果が返されることはありません。

指定した値がダッシュで始まると、PowerShell ではそれがパラメーター名として解釈され、そのようなパラメーター名は `Get-Help` コマンドレットに対しては存在しないため、エラーが生成されます。

```powershell
help -process
```

検索対象が `-process` で終わるコマンドの場合は、その値の先頭に `*` のワイルドカード文字を追加するだけです。

```powershell
help *-process
```

`Get-Help` で PowerShell コマンドを検索する場合は、検索対象に限定するのではなく、少しあいまいにする必要があります。

前に `process` を検索したときは、コマンドの名前に `process` が含まれているコマンドのみが見つかり、その結果のみが返されました。 `Get-Help` を使用して `processes` を検索すると、コマンド名に一致するものが見つからないため、システム上の PowerShell のすべてのヘルプ トピックに対して検索が実行され、見つかった一致がすべて返されます。 このため返される結果の数が膨大になります。

```powershell
Get-Help processes
```

```Output
Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Disconnect-PSSession              Cmdlet    Microsoft.PowerShell.Core Disconnects from...
Enter-PSHostProcess               Cmdlet    Microsoft.PowerShell.Core Connects to and ...
ForEach-Object                    Cmdlet    Microsoft.PowerShell.Core Performs an oper...
Get-PSSessionConfiguration        Cmdlet    Microsoft.PowerShell.Core Gets the registe...
New-PSTransportOption             Cmdlet    Microsoft.PowerShell.Core Creates an objec...
Out-Host                          Cmdlet    Microsoft.PowerShell.Core Sends output to ...
Where-Object                      Cmdlet    Microsoft.PowerShell.Core Selects objects ...
Clear-Variable                    Cmdlet    Microsoft.PowerShell.U... Deletes the valu...
Compare-Object                    Cmdlet    Microsoft.PowerShell.U... Compares two set...
Convert-String                    Cmdlet    Microsoft.PowerShell.U... Formats a string...
ConvertFrom-Csv                   Cmdlet    Microsoft.PowerShell.U... Converts object ...
ConvertTo-Html                    Cmdlet    Microsoft.PowerShell.U... Converts Microso...
ConvertTo-Xml                     Cmdlet    Microsoft.PowerShell.U... Creates an XML-b...
Debug-Runspace                    Cmdlet    Microsoft.PowerShell.U... Starts an intera...
Export-Csv                        Cmdlet    Microsoft.PowerShell.U... Converts objects...
Export-FormatData                 Cmdlet    Microsoft.PowerShell.U... Saves formatting...
Format-List                       Cmdlet    Microsoft.PowerShell.U... Formats the outp...
Format-Table                      Cmdlet    Microsoft.PowerShell.U... Formats the outp...
Get-Random                        Cmdlet    Microsoft.PowerShell.U... Gets a random nu...
Get-Unique                        Cmdlet    Microsoft.PowerShell.U... Returns unique i...
Group-Object                      Cmdlet    Microsoft.PowerShell.U... Groups objects t...
Import-Clixml                     Cmdlet    Microsoft.PowerShell.U... Imports a CLIXML...
Import-Csv                        Cmdlet    Microsoft.PowerShell.U... Creates table-li...
Measure-Object                    Cmdlet    Microsoft.PowerShell.U... Calculates the n...
Out-File                          Cmdlet    Microsoft.PowerShell.U... Sends output to ...
Out-GridView                      Cmdlet    Microsoft.PowerShell.U... Sends output to ...
Select-Object                     Cmdlet    Microsoft.PowerShell.U... Selects objects ...
Set-Variable                      Cmdlet    Microsoft.PowerShell.U... Sets the value o...
Sort-Object                       Cmdlet    Microsoft.PowerShell.U... Sorts objects by...
Tee-Object                        Cmdlet    Microsoft.PowerShell.U... Saves command ou...
Trace-Command                     Cmdlet    Microsoft.PowerShell.U... Configures and s...
Write-Output                      Cmdlet    Microsoft.PowerShell.U... Sends the specif...
Debug-Process                     Cmdlet    Microsoft.PowerShell.M... Debugs one or mo...
Get-Process                       Cmdlet    Microsoft.PowerShell.M... Gets the process...
Get-WmiObject                     Cmdlet    Microsoft.PowerShell.M... Gets instances o...
Start-Process                     Cmdlet    Microsoft.PowerShell.M... Starts one or mo...
Stop-Process                      Cmdlet    Microsoft.PowerShell.M... Stops one or mor...
Wait-Process                      Cmdlet    Microsoft.PowerShell.M... Waits for the pr...
Get-Counter                       Cmdlet    Microsoft.PowerShell.D... Gets performance...
Invoke-WSManAction                Cmdlet    Microsoft.WSMan.Manage... Invokes an actio...
Remove-WSManInstance              Cmdlet    Microsoft.WSMan.Manage... Deletes a manage...
Get-WSManInstance                 Cmdlet    Microsoft.WSMan.Manage... Displays managem...
New-WSManInstance                 Cmdlet    Microsoft.WSMan.Manage... Creates a new in...
Set-WSManInstance                 Cmdlet    Microsoft.WSMan.Manage... Modifies the man...
about_Arithmetic_Operators        HelpFile                            Describes the op...
about_Arrays                      HelpFile                            Describes arrays...
about_Debuggers                   HelpFile                            Describes the Wi...
about_Execution_Policies          HelpFile                            Describes the Wi...
about_ForEach-Parallel            HelpFile                            Describes the Fo...
about_Foreach                     HelpFile                            Describes a lang...
about_Functions                   HelpFile                            Describes how to...
about_Language_Keywords           HelpFile                            Describes the ke...
about_Methods                     HelpFile                            Describes how to...
about_Objects                     HelpFile                            Provides essenti...
about_Parallel                    HelpFile                            Describes the Pa...
about_Pipelines                   HelpFile                            Combining comman...
about_Preference_Variables        HelpFile                            Variables that c...
about_Remote                      HelpFile                            Describes how to...
about_Remote_Output               HelpFile                            Describes how to...
about_Sequence                    HelpFile                            Describes the Se...
about_Session_Configuration_Files HelpFile                            Describes sessio...
about_Variables                   HelpFile                            Describes how va...
about_Windows_PowerShell_5.0      HelpFile                            Describes new fe...
about_WQL                         HelpFile                            Describes WMI Qu...
about_WS-Management_Cmdlets       HelpFile                            Provides an over...
about_ForEach-Parallel            HelpFile                            Describes the Fo...
about_Parallel                    HelpFile                            Describes the Pa...
about_Sequence                    HelpFile                            Describes the Se...
```

`Help` を使用して `process` を検索すると 10 件の結果が返されます。また、`processes` を検索すると 68 件の結果が返されます。 見つかった結果が 1 つだけの場合は、コマンドの一覧ではなく、ヘルプ トピック自体が表示されます。

```powershell
get-help *hotfix*
```

```Output
NAME
    Get-HotFix

SYNOPSIS
    Gets the hotfixes that have been applied to the local and remote computers.


SYNTAX
    Get-HotFix [-ComputerName <String[]>] [-Credential <PSCredential>] [-Description
    <String[]>] [<CommonParameters>]

    Get-HotFix [[-Id] <String[]>] [-ComputerName <String[]>] [-Credential
    <PSCredential>] [<CommonParameters>]


DESCRIPTION
    The Get-Hotfix cmdlet gets hotfixes (also called updates) that have been installed
    on either the local computer (or on specified remote computers) by Windows Update,
    Microsoft Update, or Windows Server Update Services; the cmdlet also gets hotfixes
    or updates that have been installed manually by users.


RELATED LINKS
    Online Version: http://go.microsoft.com/fwlink/?LinkId=821586
    Win32_QuickFixEngineering http://go.microsoft.com/fwlink/?LinkID=145071
    Get-ComputerRestorePoint
    Add-Content

REMARKS
    To see the examples, type: "get-help Get-HotFix -examples".
    For more information, type: "get-help Get-HotFix -detailed".
    For technical information, type: "get-help Get-HotFix -full".
    For online help, type: "get-help Get-HotFix -online"
```

ここで、PowerShell の `Help` では、ヘルプ トピックが含まれているコマンドしか見つけることができないという説が嘘であることを証明します。

```powershell
help *more*
```

```Output
NAME
    more

SYNTAX
    more [[-paths] <string[]>]


ALIASES
    None


REMARKS
    None
```

この例の `more` にはヘルプ トピックがありませんが、PowerShell の `Help` システムはこれを見つけることができました。 このシステムは一致を 1 つだけ見つけて、コマンドにヘルプ トピックがないときに表示される基本的な構文情報を返しました。

PowerShell には、さまざまな概念 (About) ヘルプ トピックが含まれています。 次のコマンドを使用すると、システム上の **About** ヘルプ トピックの一覧を返すことができます。

```powershell
help About_*
```

結果を単一の About ヘルプ トピック 1 つに制限すると、リストではなく実際のヘルプ トピックが表示されます。

```powershell
help about_Updatable_Help
```

**About** ヘルプ トピックを表示するには、PowerShell のヘルプ システムを更新する必要があります。 何らかの理由でコンピューター上のヘルプ システムの初期更新が失敗した場合、`Update-Help` コマンドレットが正常に実行されるまで、ファイルは使用できません。

## <a name="get-command"></a>Get-Command

`Get-Command` は、コマンドを見つけやすくすることを目的としています。 パラメーターを指定せずに `Get-Command` を実行すると、システム上のコマンドの一覧が返されます。 次の例は、`Get-Command` コマンドレットを使用して、プロセスを操作するためのコマンドを確認しています。

```powershell
Get-Command -Noun Process
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Debug-Process                                      3.1.0.0    Microsof...
Cmdlet          Get-Process                                        3.1.0.0    Microsof...
Cmdlet          Start-Process                                      3.1.0.0    Microsof...
Cmdlet          Stop-Process                                       3.1.0.0    Microsof...
Cmdlet          Wait-Process                                       3.1.0.0    Microsof...
```

この例の `Get-Command` の実行例では、**Noun** パラメーターが使用され、`Process` が **Noun** パラメーターの値として指定されていることに注意してください。 `Get-Command` コマンドレットの使用方法がわからない場合は、 `Get-Help` を使えば、`Get-Command` のヘルプ トピックを表示できます。

**Name**、**Noun**、**Verb** の各パラメーターで、ワイルドカード文字を使用できます。 次の例では、**Name** パラメーターでワイルドカード文字が使用されています。

```Output
Get-Command -Name *service*
```

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-NetFirewallServiceFilter                       2.0.0.0    NetSecurity
Function        Set-NetFirewallServiceFilter                       2.0.0.0    NetSecurity
Cmdlet          Get-Service                                        3.1.0.0    Microsof...
Cmdlet          New-Service                                        3.1.0.0    Microsof...
Cmdlet          New-WebServiceProxy                                3.1.0.0    Microsof...
Cmdlet          Restart-Service                                    3.1.0.0    Microsof...
Cmdlet          Resume-Service                                     3.1.0.0    Microsof...
Cmdlet          Set-Service                                        3.1.0.0    Microsof...
Cmdlet          Start-Service                                      3.1.0.0    Microsof...
Cmdlet          Stop-Service                                       3.1.0.0    Microsof...
Cmdlet          Suspend-Service                                    3.1.0.0    Microsof...
Application     AgentService.exe                                   10.0.14... C:\Windo...
Application     SensorDataService.exe                              10.0.14... C:\Windo...
Application     services.exe                                       10.0.14... C:\Windo...
Application     services.msc                                       0.0.0.0    C:\Windo...
Application     TieringEngineService.exe                           10.0.14... C:\Windo...
```

`Get-Command` の **Name** パラメーターでワイルドカード文字を使用することは、あまり好きではありません。ネイティブの PowerShell コマンドではない実行可能ファイルも返されるためです。

**Name** パラメーターと共にワイルドカード文字を使用する場合は、**CommandType** パラメーターを使って結果を制限することをお勧めします。

```powershell
Get-Command -Name *service* -CommandType Cmdlet, Function, Alias
```

それよりも良いのは、**Verb** パラメーターまたは **Noun** パラメーターのいずれか、または両方を使用することでしょう。動詞と名詞の両方が含まれるのは、PowerShell コマンドだけだからです。

ヘルプ トピックで問題が見つかっても大丈夫です。 PowerShell のヘルプ トピックはオープンソースなので、GitHub の [PowerShell-Docs][] リポジトリから入手できます。 誤っている情報は、自分だけでなく他のユーザー全員が修正し、皆で協力し合うことができます。 ただ GitHub での PowerShell ドキュメント リポジトリのフォーク、ヘルプ トピック更新、pull request の送信のみを行ってください。
pull request が受け入れられたら、修正されたドキュメントは全員が利用できます。

## <a name="updating-help"></a>ヘルプの更新

PowerShell ヘルプ トピックのローカル コピーは、コマンドの最初のヘルプが要求されたときに更新されました。 ヘルプ コンテンツは不定期で更新されることがあるため、ヘルプ システムは定期的に更新することをお勧めします。 `Update-Help` コマンドレットは、ヘルプ トピックの更新に使用されます。
管理者として昇格された PowerShell を実行するには、既定でインターネット アクセスが必要です。

```powershell
Update-Help
```

```Output
Update-Help : Failed to update Help for the module(s) 'BitsTransfer' with UI culture(s)
{en-US} : Unable to retrieve the HelpInfo XML file for UI culture en-US. Make sure the HelpInfoUri
property in the module manifest is valid or check your network connection and then try the command again.
At line:1 char:1
+ Update-Help
+
    + CategoryInfo          : InvalidOperation: (:) [Update-Help], Exception
    + FullyQualifiedErrorId : InvalidHelpInfoUri,Microsoft.PowerShell.Commands.UpdateHel
   pCommand

Update-Help : Failed to update Help for the module(s) 'NetworkControllerDiagnostics,
StorageReplica' with UI culture(s) {en-US} : Unable to retrieve the HelpInfo XML file
for UI culture en-US. Make sure the HelpInfoUri property in the module manifest is valid
or check your network connection and then try the command again.
At line:1 char:1
+ Update-Help
+
    + CategoryInfo          : ResourceUnavailable: (:) [Update-Help], Exception
    + FullyQualifiedErrorId : UnableToRetrieveHelpInfoXml,Microsoft.PowerShell.Commands.
   UpdateHelpCommand
```

いくつかのモジュールによってエラーが返されましたが、これは珍しいことではありません。 コンピューターがインターネットにアクセスできない場合は、インターネットにアクセスできる別のマシンで `Save-Help` コマンドレットを使用して、更新されたヘルプ情報をネットワーク上のファイル共有に保存してから、`Update-Help` の **SourcePath** パラメーターを使用し、ヘルプ トピックに対してこのネットワークの場所を指定できます。

コンピューターのヘルプ コンテンツを定期的に更新するために、スケジュールされたタスクを設定するか、PowerShell でプロファイル スクリプトにロジックを追加することを検討してください。 プロファイル スクリプトについては、以降の章で説明します。

## <a name="summary"></a>まとめ

この章では、`Get-Help` と `Get-Command` の両方を使ってコマンドを見つける方法と、 コマンドを見つけたら、ヘルプ システムを使ってどのようにそのコマンドの使用方法を確認するかを学習しました。 また、更新プログラムが利用可能になったときにヘルプ トピックのコンテンツを更新する方法についても学習しました。

私があなたに言いたいのは、ぜひ 1 日 1 つの PowerShell コマンドを覚えてほしい、ということです。

```powershell
Get-Command | Get-Random | Get-Help -Full
```

## <a name="review"></a>確認

1. `Get-Service` の **DisplayName** パラメーターは、位置指定のパラメーターですか。
1. `Get-Process` コマンドレットにはパラメーター セットの数がいくつありますか。
1. イベント ログを操作するための PowerShell コマンドは何ですか。
1. コンピューターで実行されている PowerShell プロセス リストを返すための PowerShell コマンドは何ですか。
1. コンピューターに格納されている PowerShell ヘルプ コンテンツを更新するには、どうすればよいですか。

## <a name="recommended-reading"></a>推奨トピック

この章で説明しているトピックについてもっと詳しく知りたい方は、次の PowerShell のヘルプ トピックを読むことをお勧めします。

- [Get-Help][]
- [Get-Command][]
- [Update-Help][]
- [Save-Help][]
- [about_Updatable_Help][]
- [about_Command_Syntax][]

次の章では、`Get-Member` コマンドレットと、オブジェクト、プロパティ、およびメソッドについて説明します。

<!-- link references -->
[Get-Help]: /powershell/module/microsoft.powershell.core/get-help
[Get-Command]: /powershell/module/microsoft.powershell.core/get-command
[Update-Help]: /powershell/module/microsoft.powershell.core/update-help
[Save-Help]: /powershell/module/microsoft.powershell.core/save-help
[about_Updatable_Help]: /powershell/module/microsoft.powershell.core/about/about_updatable_help
[about_Command_Syntax]: /powershell/module/microsoft.powershell.core/about/about_command_syntax
[PowerShell-Docs]: https://github.com/powershell/powershell
[付録 A:]: appendix-a.md
