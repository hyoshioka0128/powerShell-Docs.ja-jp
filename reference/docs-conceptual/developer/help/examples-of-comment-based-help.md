---
ms.date: 01/11/2021
ms.topic: reference
title: コメント ベースのヘルプの例
description: コメント ベースのヘルプの例
ms.openlocfilehash: 237e65c59cc3b35f48b6d667c8fb297994b03638
ms.sourcegitcommit: 4879b9cdfa3f03b04a07b84442dc1ca9ae0f6b46
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98105164"
---
# <a name="examples-of-comment-based-help"></a>コメント ベースのヘルプの例

このトピックには、スクリプトや関数にコメントベースのヘルプを使用する方法を示す例が含まれています。

## <a name="example-1-comment-based-help-for-a-function"></a>例 1: 関数のヘルプを Comment-Based

 次のサンプル関数には、コメントベースのヘルプが含まれています。

```powershell
function Add-Extension
{
    param ([string]$Name,[string]$Extension = "txt")
    $name = $name + "." + $extension
    $name

    <#
        .SYNOPSIS
        Adds a file name extension to a supplied name.

        .DESCRIPTION
        Adds a file name extension to a supplied name.
        Takes any strings for the file name or extension.

        .PARAMETER Name
        Specifies the file name.

        .PARAMETER Extension
        Specifies the extension. "Txt" is the default.

        .INPUTS
        None. You cannot pipe objects to Add-Extension.

        .OUTPUTS
        System.String. Add-Extension returns a string with the extension or file name.

        .EXAMPLE
        PS> extension -name "File"
        File.txt

        .EXAMPLE
        PS> extension -name "File" -extension "doc"
        File.doc

        .EXAMPLE
        PS> extension "File" "doc"
        File.doc

        .LINK
        Online version: http://www.fabrikam.com/extension.html

        .LINK
        Set-Item
    #>
}
```

次の出力は、 `Get-Help` 関数のヘルプを表示するコマンドの結果を示して `Add-Extension` います。

```powershell
PS> Get-Help Add-Extension -full
```

```Output
        NAME
            Add-Extension

        SYNOPSIS
            Adds a file name extension to a supplied name.

        SYNTAX
            Add-Extension [[-Name] <String>] [[-Extension] <String>] [<CommonParameters>]

        DESCRIPTION
            Adds a file name extension to a supplied name. Takes any strings for the file name or extension.

        PARAMETERS
           -Name
               Specifies the file name.

               Required?                    false
               Position?                    0
               Default value
               Accept pipeline input?       false
               Accept wildcard characters?

           -Extension
               Specifies the extension. "Txt" is the default.

               Required?                    false
               Position?                    1
               Default value
               Accept pipeline input?       false
               Accept wildcard characters?

            <CommonParameters>
               This cmdlet supports the common parameters: -Verbose, -Debug,
               -ErrorAction, -ErrorVariable, -WarningAction, -WarningVariable,
               -OutBuffer and -OutVariable. For more information, type
               "get-help about_commonparameters".

        INPUTS
            None. You cannot pipe objects to Add-Extension.

        OUTPUTS
            System.String. Add-Extension returns a string with the extension or file name.

            -------------------------- EXAMPLE 1 --------------------------

            PS> extension -name "File"
            File.txt

            -------------------------- EXAMPLE 2 --------------------------

            PS> extension -name "File" -extension "doc"
            File.doc

            -------------------------- EXAMPLE 3 --------------------------

            PS> extension "File" "doc"
            File.doc

        RELATED LINKS
            Online version: http://www.fabrikam.com/extension.html
            Set-Item
```

## <a name="example-2-comment-based-help-for-a-script"></a>例 2: スクリプトのヘルプを Comment-Based する

次のサンプル関数には、コメントベースのヘルプが含まれています。

終了とステートメントの間に空白行があることに注意して **#>** `Param` ください。 ステートメントが含まれていないスクリプトでは、 `Param` ヘルプトピックの最後のコメントと最初の関数宣言の間に、少なくとも2つの空白行が必要です。 これらの空白行がない場合、はスクリプトでは `Get-Help` なく関数にヘルプトピックを関連付けます。

```powershell
<#
  .SYNOPSIS
  Performs monthly data updates.

  .DESCRIPTION
  The Update-Month.ps1 script updates the registry with new data generated
  during the past month and generates a report.

  .PARAMETER InputPath
  Specifies the path to the CSV-based input file.

  .PARAMETER OutputPath
  Specifies the name and path for the CSV-based output file. By default,
  MonthlyUpdates.ps1 generates a name from the date and time it runs, and
  saves the output in the local directory.

  .INPUTS
  None. You cannot pipe objects to Update-Month.ps1.

  .OUTPUTS
  None. Update-Month.ps1 does not generate any output.

  .EXAMPLE
  PS> .\Update-Month.ps1

  .EXAMPLE
  PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv

  .EXAMPLE
  PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv -outputPath C:\Reports\2009\January.csv
#>

param ([string]$InputPath, [string]$OutPutPath)

function Get-Data { }
```

次のコマンドは、スクリプトのヘルプを取得します。 スクリプトは Path 環境変数に示されているディレクトリにないため、スクリプトヘルプを `Get-Help` 取得するコマンドではスクリプトパスを指定する必要があります。

```powershell
PS> Get-Help c:\ps-test\update-month.ps1 -full
```

```Output
            NAME
                C:\ps-test\Update-Month.ps1

            SYNOPSIS
                Performs monthly data updates.

            SYNTAX
                C:\ps-test\Update-Month.ps1 [-InputPath] <String> [[-OutputPath]
                <String>] [<CommonParameters>]

            DESCRIPTION
                The Update-Month.ps1 script updates the registry with new data
                generated during the past month and generates a report.

            PARAMETERS
               -InputPath
                   Specifies the path to the CSV-based input file.

                   Required?                    true
                   Position?                    0
                   Default value
                   Accept pipeline input?       false
                   Accept wildcard characters?

               -OutputPath
                   Specifies the name and path for the CSV-based output file. By
                   default, MonthlyUpdates.ps1 generates a name from the date
                   and time it runs, and saves the output in the local directory.

                   Required?                    false
                   Position?                    1
                   Default value
                   Accept pipeline input?       false
                   Accept wildcard characters?

               <CommonParameters>
                   This cmdlet supports the common parameters: -Verbose, -Debug,
                   -ErrorAction, -ErrorVariable, -WarningAction, -WarningVariable,
                   -OutBuffer and -OutVariable. For more information, type,
                   "get-help about_commonparameters".

            INPUTS
                   None. You cannot pipe objects to Update-Month.ps1.

            OUTPUTS
                   None. Update-Month.ps1 does not generate any output.

            -------------------------- EXAMPLE 1 --------------------------

            PS> .\Update-Month.ps1

            -------------------------- EXAMPLE 2 --------------------------

            PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv

            -------------------------- EXAMPLE 3 --------------------------

            PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv -outputPath
            C:\Reports\2009\January.csv

            RELATED LINKS
```

## <a name="example-3-parameter-descriptions-in-a-param-statement"></a>例 3: Param ステートメント内のパラメーターの説明

この例で `Param` は、関数またはスクリプトのステートメントでパラメーターの説明を挿入する方法を示します。 この形式は、パラメーターの説明が brief の場合に最も役立ちます。

```powershell
function Add-Extension
{
    param
    (
        [string]
        # Specifies the file name.
        $name,

        [string]
        # Specifies the file name extension. "Txt" is the default.
        $extension = "txt"
    )
    $name = $name + "." + $extension
    $name

    <#
        .SYNOPSIS
        Adds a file name extension to a supplied name.
...
    #>
```

結果は、例1の結果と同じです。 `Get-Help` パラメーターの説明は、キーワードが付いているかのように解釈され `.Parameter` ます。

## <a name="example-4--redirecting-to-an-xml-file"></a>例 4: XML ファイルへのリダイレクト

関数とスクリプトの XML ベースのヘルプトピックを記述できます。 コメントベースのヘルプは実装が簡単ですが、ヘルプコンテンツをより細かく制御する場合や、ヘルプトピックを複数の言語に変換する場合は、XML ベースのヘルプが必要です。次の例は、スクリプトの最初の数行を示して `Update-Month.ps1` います。 スクリプトでは、キーワードを使用して、 `.ExternalHelp` スクリプトの XML ベースのヘルプトピックへのパスを指定します。

```powershell
#  .ExternalHelp C:\MyScripts\Update-Month-Help.xml

    param ([string]$InputPath, [string]$OutPutPath)

    function Get-Data { }
```

次の例は、関数でキーワードを使用する方法を示して `.ExternalHelp` います。

```powershell
function Add-Extension
{
    param ([string] $name, [string]$extension = "txt")
    $name = $name + "." + $extension
    $name

    # .ExternalHelp C:\ps-test\Add-Extension.xml
}
```

## <a name="example-5--redirecting-to-a-different-help-topic"></a>例 5: 別のヘルプトピックへのリダイレクト

次のコードは、PowerShell の組み込み関数の先頭から抜粋したもので、一度 `Help` に1画面のヘルプテキストを表示します。 Get-Help コマンドレットのヘルプトピックでは Help 関数について説明しているので、Help 関数はキーワードとキーワードを使用して、 `.ForwardHelpTargetName` `.ForwardHelpCategory` ユーザーを Get-Help コマンドレットヘルプトピックにリダイレクトします。

```powershell
function help
{

    <#
      .FORWARDHELPTARGETNAME Get-Help
      .FORWARDHELPCATEGORY Cmdlet
    #>
    [CmdletBinding(DefaultParameterSetName='AllUsersView')]
    param(
            [Parameter(Position=0, ValueFromPipelineByPropertyName=$true)]
            [System.String]
            ${Name},
    ...
```

次のコマンドでは、この機能を使用します。 ユーザーが関数のコマンドを入力すると `Get-Help` `Help` 、によってコマンド `Get-Help` レットのヘルプトピックが表示され `Get-Help` ます。

```powershell
PS> get-help help
```

```Output
            NAME
                Get-Help

            SYNOPSIS
                Displays information about Windows PowerShell cmdlets and concepts.
            ...
```
