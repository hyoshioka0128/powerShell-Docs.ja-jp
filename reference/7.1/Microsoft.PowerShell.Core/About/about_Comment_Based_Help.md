---
description: 関数とスクリプトのコメントベースのヘルプトピックを記述する方法について説明します。
keywords: powershell,コマンドレット
ms.date: 06/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_comment_based_help?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Comment_Based_Help
ms.openlocfilehash: 386ed8e1c28904c484261aa91d11ce028632cd16
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391274"
---
# <a name="about-comment-based-help"></a>コメントベースのヘルプについて

## <a name="short-description"></a>簡単な説明
関数とスクリプトのコメントベースのヘルプトピックを記述する方法について説明します。

## <a name="long-description"></a>長い説明

特別なヘルプコメントキーワードを使用して、関数およびスクリプトのコメントベースのヘルプトピックを記述できます。

[Get-help](xref:Microsoft.PowerShell.Core.Get-Help)コマンドレットは、XML ファイルから生成されたコマンドレットヘルプトピックを表示するのと同じ形式で、コメントベースのヘルプを表示します。 ユーザーは、 `Get-Help` **詳細** 、 **完全** 、 **例** 、 **オンライン** など、のすべてのパラメーターを使用して、コメントベースのヘルプの内容を表示できます。

関数とスクリプトの XML ベースのヘルプファイルを作成することもできます。 `Get-Help`コマンドレットで関数またはスクリプトの XML ベースのヘルプファイルを検索できるようにするには、キーワードを使用し `.ExternalHelp` ます。 このキーワードがない場合、で `Get-Help` は、関数またはスクリプトの XML ベースのヘルプトピックを見つけることができません。

このトピックでは、関数とスクリプトのヘルプトピックを記述する方法について説明します。 関数とスクリプトのヘルプトピックを表示する方法の詳細については、「 [get-help](xref:Microsoft.PowerShell.Core.Get-Help)」を参照してください。

[Update-help](xref:Microsoft.PowerShell.Core.Update-Help)および update-help コマンドレットは、XML ファイルでのみ[機能します](xref:Microsoft.PowerShell.Core.Save-Help)。 更新可能なヘルプでは、コメントベースのヘルプトピックはサポートされていません。

## <a name="syntax-for-comment-based-help"></a>コメントベースのヘルプの構文

コメントベースのヘルプの構文は次のとおりです。

```
# .<help keyword>
# <help content>
```

または

```
<#
.<help keyword>
<help content>
#>
```

コメントベースのヘルプは、一連のコメントとして書かれています。 コメントの各行の前にコメント記号を入力する `#` か、および記号を使用し `<#` て `#>` コメントブロックを作成することができます。 コメントブロック内のすべての行は、コメントとして解釈されます。

コメントベースのヘルプトピック内のすべての行は、連続している必要があります。 コメントベースのヘルプトピックがヘルプトピックに含まれていないコメントの後に続く場合は、ヘルプ以外の最後のコメント行とコメントベースのヘルプの先頭の間に、少なくとも1つの空白行がある必要があります。

キーワードは、コメントベースのヘルプの各セクションを定義します。 各コメントベースのヘルプキーワードの前には、ドットが付き `.` ます。 キーワードは任意の順序で表示できます。 キーワード名の大文字と小文字は区別されません。

たとえば、キーワードは、 `.Description` 関数またはスクリプトの説明の前にあります。

```
<#
.Description
Get-Function displays the name and syntax of all functions in the session.
#>
```

コメントブロックには少なくとも1つのキーワードを含める必要があります。 などのキーワードの中には、 `.EXAMPLE` 同じコメントブロックに何度も出現するものがあります。 各キーワードのヘルプコンテンツは、キーワードの後の行から開始され、複数の行にまたがることができます。

## <a name="syntax-for-comment-based-help-in-functions"></a>関数のコメントベースのヘルプの構文

関数のコメントベースのヘルプは、次の3つの場所のいずれかに表示されます。

- 関数本体の先頭にあります。
- 関数本体の末尾にあります。
- キーワードの前 `function` 。 関数のヘルプとキーワードの最後の行の間に、複数の空白行を指定することはできません `function` 。

次に例を示します。

```powershell
function Get-Function
{
<#
.<help keyword>
<help content>
#>

  # function logic
}
```

or

```powershell
function Get-Function
{
   # function logic

<#
.<help keyword>
<help content>
#>
}
```

or

```powershell
<#
.<help keyword>
<help content>
#>
function Get-Function { }
```

## <a name="syntax-for-comment-based-help-in-scripts"></a>スクリプトのコメントベースのヘルプの構文

スクリプトのコメントベースのヘルプは、スクリプト内の次の2つの場所のいずれかに表示されます。

- スクリプトファイルの先頭にあります。 スクリプトのヘルプは、コメントと空白行によってのみスクリプトの先頭に配置できます。

  スクリプト本体の最初の項目 (ヘルプの後) が関数宣言の場合、スクリプトヘルプの末尾と関数宣言の間には、少なくとも2つの空白行が必要です。 それ以外の場合、ヘルプは、スクリプトのヘルプではなく、関数のヘルプとして解釈されます。

- スクリプトファイルの末尾にあります。 ただし、スクリプトが署名されている場合は、スクリプトファイルの先頭にコメントベースのヘルプを配置します。 スクリプトの末尾は、signature ブロックによって占有されています。

次に例を示します。

```powershell
<#
.<help keyword>
<help content>
#>


function Get-Function { }
```

or

```powershell
function Get-Function { }

<#
.<help keyword>
<help content>
#>
```

## <a name="syntax-for-comment-based-help-in-script-modules"></a>スクリプトモジュールのコメントベースのヘルプの構文

スクリプトモジュールでは、 `.psm1` コメントベースのヘルプは、スクリプトの構文ではなく、関数の構文を使用します。 スクリプトの構文を使用して、スクリプトモジュールで定義されているすべての関数のヘルプを提供することはできません。

たとえば、キーワードを使用して、 `.ExternalHelp` スクリプトモジュール内の関数の XML ベースのヘルプファイルを識別する場合は、各関数にコメントを追加する必要があり `.ExternalHelp` ます。

```powershell
# .ExternalHelp <XML-file-name>
function <function-name>
{
  ...
}
```

## <a name="comment-based-help-keywords"></a>コメントベースのヘルプキーワード

有効なコメントベースのヘルプキーワードを次に示します。 これらの一覧は、通常、ヘルプトピックに表示される順序で、使用目的と共に表示されます。 これらのキーワードは、コメントベースのヘルプ内で任意の順序で表示でき、大文字と小文字は区別されません。

### <a name="synopsis"></a>.概要

関数またはスクリプトの簡単な説明。 このキーワードは、各トピックで1回だけ使用できます。

### <a name="description"></a>.記述

関数またはスクリプトの詳細な説明。 このキーワードは、各トピックで1回だけ使用できます。

### <a name="parameter"></a>.引き

パラメーターの説明。 `.PARAMETER`関数またはスクリプトの構文で、各パラメーターにキーワードを追加します。

キーワードと同じ行にパラメーター名を入力し `.PARAMETER` ます。 キーワードの後の行にパラメーターの説明を入力し `.PARAMETER` ます。 Windows PowerShell `.PARAMETER` は、パラメーターの説明の一部として、行と次のキーワード、またはコメントブロックの末尾の間のすべてのテキストを解釈します。
説明には、段落区切りを含めることができます。

```
.PARAMETER  <Parameter-Name>
```

パラメーターキーワードは、コメントブロック内の任意の順序で指定できますが、関数またはスクリプトの構文によって、パラメーター (およびその説明) がヘルプトピックに表示される順序が決定されます。 順序を変更するには、構文を変更します。

関数またはスクリプト構文のパラメーター変数名の直前にコメントを配置することで、パラメーターの説明を指定することもできます。 これを機能させるには、少なくとも1つのキーワードを含むコメントブロックが必要です。

構文コメントとキーワードの両方を使用する場合 `.PARAMETER` は、キーワードに関連付けられている説明 `.PARAMETER` が使用され、構文のコメントは無視されます。

```powershell
<#
.SYNOPSIS
    Short description here
#>
function Verb-Noun {
    [CmdletBinding()]
    param (
        # This is the same as .Parameter
        [string]$Computername
    )
    # Verb the Noun on the computer
}
```

### <a name="example"></a>.よう

関数またはスクリプトを使用するサンプルコマンド。必要に応じて、サンプル出力と説明が続きます。 各例について、このキーワードを繰り返します。

### <a name="inputs"></a>.INPUTACCEL

関数またはスクリプトにパイプすることができるオブジェクトの .NET 型。 入力オブジェクトの説明を含めることもできます。

### <a name="outputs"></a>.出力

コマンドレットが返すオブジェクトの .NET 型。 返されたオブジェクトの説明を含めることもできます。

### <a name="notes"></a>.注記

関数またはスクリプトに関する追加情報。

### <a name="link"></a>.リンク

関連するトピックの名前。 値は、"の下の行に表示されます。LINK "キーワードの前にはコメント記号を付ける `#` か、コメントブロックに含める必要があります。

`.LINK`関連する各トピックについて、キーワードを繰り返します。

このコンテンツは、ヘルプトピックの「関連リンク」セクションに表示されます。

キーワードの内容には、 `.Link` 同じヘルプトピックのオンラインバージョンの Uniform Resource Identifier (URI) を含めることもできます。 の **online** パラメーターを使用すると、オンラインバージョンが開き `Get-Help` ます。 URI は "http" または "https" で始まる必要があります。

### <a name="component"></a>.成分

関数またはスクリプトが使用する、またはそれに関連するテクノロジまたは機能の名前。 の **Component** パラメーターで `Get-Help` は、この値を使用して、によって返される検索結果をフィルター処理し `Get-Help` ます。

### <a name="role"></a>.果たす

ヘルプトピックのユーザーロールの名前です。 の **Role** パラメーターで `Get-Help` は、この値を使用して、によって返される検索結果をフィルター処理し `Get-Help` ます。

### <a name="functionality"></a>.機

関数の使用目的を説明するキーワード。 の **機能** パラメーターは、 `Get-Help` この値を使用して、によって返される検索結果をフィルター処理し `Get-Help` ます。

### <a name="forwardhelptargetname"></a>.FORWARDHELPTARGETNAME

指定されたコマンドのヘルプトピックにリダイレクトします。 関数、スクリプト、コマンドレット、またはプロバイダーのヘルプトピックを含む、任意のヘルプトピックにユーザーをリダイレクトできます。

```powershell
# .FORWARDHELPTARGETNAME <Command-Name>
```

### <a name="forwardhelpcategory"></a>.FORWARDHELPCATEGORY

の項目のヘルプカテゴリを指定し `.ForwardHelpTargetName` ます。 有効な値は、、、、、、、、、、、、 `Alias` `Cmdlet` `HelpFile` `Function` `Provider` `General` `FAQ` `Glossary` `ScriptCommand` `ExternalScript` `Filter` または `All` です。 同じ名前のコマンドが存在する場合に競合を回避するには、このキーワードを使用します。

```powershell
# .FORWARDHELPCATEGORY <Category>
```

### <a name="remotehelprunspace"></a>.REMOTEHELPRUNSPACE 空間

ヘルプトピックを含むセッションを指定します。 **PSSession** オブジェクトを含む変数を入力します。 このキーワードは、 [エクスポート](xref:Microsoft.PowerShell.Utility.Export-PSSession) コマンドレットによって使用され、エクスポートされたコマンドのヘルプトピックを検索します。

```powershell
# .REMOTEHELPRUNSPACE <PSSession-variable>
```

### <a name="externalhelp"></a>..EXTERNALHELP

スクリプトまたは関数の XML ベースのヘルプファイルを指定します。

```powershell
# .EXTERNALHELP <XML Help File>
```

キーワードは、 `.ExternalHelp` 関数またはスクリプトが XML ファイルに記述されている場合に必要です。 このキーワードがない場合、は `Get-Help` 関数またはスクリプトの XML ベースのヘルプファイルを見つけることができません。

キーワードは、 `.ExternalHelp` 他のコメントベースのヘルプキーワードよりも優先されます。 `.ExternalHelp`が存在する場合、 `Get-Help` キーワードの値に一致するヘルプトピックが見つからない場合でも、にはコメントベースのヘルプは表示されません `.ExternalHelp` 。

関数がモジュールによってエクスポートされる場合は、キーワードの値 `.ExternalHelp` をパスを含まないファイル名に設定します。 `Get-Help` モジュールディレクトリの言語固有のサブディレクトリで、指定されたファイル名を検索します。 関数の XML ベースのヘルプファイルの名前には要件はありませんが、次の形式を使用することをお勧めします。

```
<ScriptModule.psm1>-help.xml
```

関数がモジュールに含まれていない場合は、XML ベースのヘルプファイルへのパスを指定します。 値にパスが含まれていて、パスに UI カルチャ固有のサブディレクトリが含まれている場合、は、 `Get-Help` モジュールディレクトリの場合と同様に、Windows 用に設定された言語フォールバック標準に従って、スクリプトまたは関数の名前を持つ XML ファイルを再帰的に検索します。

コマンドレットヘルプの XML ベースのヘルプファイル形式の詳細については、「 [コマンドレットのヘルプの記述方法](/powershell/scripting/developer/help/writing-comment-based-help-topics)」を参照してください。

## <a name="autogenerated-content"></a>自動生成コンテンツ

名前、構文、パラメーターリスト、パラメーター属性テーブル、共通パラメーター、および解説は、コマンドレットによって自動的に生成され `Get-Help` ます。

### <a name="name"></a>名前

関数のヘルプトピックの **名前** セクションは、関数の構文の関数名から取得されます。 スクリプトのヘルプトピックの **名前** は、スクリプトファイル名から取得されます。 名前または大文字と小文字の区別を変更するには、関数の構文またはスクリプトのファイル名を変更します。

### <a name="syntax"></a>構文

ヘルプトピックの **構文** セクションは、関数またはスクリプトの構文から生成されます。 パラメーターの .NET 型など、ヘルプトピックの構文に詳細を追加するには、構文に詳細を追加します。 パラメーターの型を指定しない場合、 **オブジェクト** の種類が既定値として挿入されます。

### <a name="parameter-list"></a>パラメーター リスト

ヘルプトピックのパラメーターリストは、関数またはスクリプトの構文、およびキーワードを使用して追加する説明から生成され `.Parameter` ます。 関数のパラメーターは、関数またはスクリプトの構文に表示されるのと同じ順序で、ヘルプトピックの **パラメーター** セクションに表示されます。 パラメーター名のスペルと大文字小文字の区別も構文から取得されます。 キーワードによって指定されたパラメーター名の影響を受けません `.Parameter` 。

### <a name="common-parameters"></a>共通パラメーター

**一般的なパラメーター** は、ヘルプトピックの構文とパラメーターリストに、何の効果もない場合でも追加されます。 共通パラメーターの詳細については、「 [about_CommonParameters](about_CommonParameters.md)」を参照してください。

### <a name="parameter-attribute-table"></a>パラメーター属性テーブル

`Get-Help` の **Full** パラメーターまたは **parameter** パラメーターを使用するときに表示されるパラメーター属性のテーブルを生成し `Get-Help` ます。 **必須** 、 **位置** 、および **既定** 値の属性の値は、関数またはスクリプトの構文から取得されます。

既定値および **Accept ワイルドカード文字** の値は、関数またはスクリプトで定義されている場合でも、parameter 属性テーブルには表示されません。 ユーザーを支援するために、この情報をパラメーターの説明に入力してください。

### <a name="remarks"></a>Remarks

ヘルプトピックの「 **解説** 」セクションは、関数またはスクリプトの名前から自動的に生成されます。 コンテンツを変更または変更することはできません。

## <a name="examples"></a>例

### <a name="comment-based-help-for-a-function"></a>関数のコメントベースのヘルプ

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

System.String. Add-Extension returns a string with the extension
or file name.

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

http://www.fabrikam.com/extension.html

.LINK

Set-Item
#>
}
```

結果は次のようになります。

```powershell
Get-Help -Name "Add-Extension" -Full
```

```Output
NAME

Add-Extension

SYNOPSIS

Adds a file name extension to a supplied name.

SYNTAX

Add-Extension [[-Name] <String>] [[-Extension] <String>]
[<CommonParameters>]

DESCRIPTION

Adds a file name extension to a supplied name. Takes any strings for the
file name or extension.

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

System.String. Add-Extension returns a string with the extension or
file name.

Example 1

PS> extension -name "File"
File.txt

Example 2

PS> extension -name "File" -extension "doc"
File.doc

Example 3

PS> extension "File" "doc"
File.doc

RELATED LINKS

http://www.fabrikam.com/extension.html
Set-Item
```

### <a name="parameter-descriptions-in-function-syntax"></a>関数の構文におけるパラメーターの説明

この例は前の例と同じですが、パラメーターの説明が関数の構文に挿入される点が異なります。 この形式は、説明が brief の場合に最も役立ちます。

```powershell
function Add-Extension
{
param
(

[string]
#Specifies the file name.
$name,

[string]
#Specifies the file name extension. "Txt" is the default.
$extension = "txt"
)

$name = $name + "." + $extension
$name

<#
.SYNOPSIS

Adds a file name extension to a supplied name.

.DESCRIPTION

Adds a file name extension to a supplied name. Takes any strings for the
file name or extension.

.INPUTS

None. You cannot pipe objects to Add-Extension.

.OUTPUTS

System.String. Add-Extension returns a string with the extension or
file name.

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

http://www.fabrikam.com/extension.html

.LINK

Set-Item
#>
}
```

### <a name="comment-based-help-for-a-script"></a>スクリプトのコメントベースのヘルプ

次のサンプルスクリプトには、コメントベースのヘルプが含まれています。 終了とステートメントの間に空白行があることに注意して `#>` `Param` ください。 ステートメントが含まれていないスクリプトでは、 `Param` ヘルプトピックの最後のコメントと最初の関数宣言の間に、少なくとも2つの空白行が必要です。 これらの空白行がない場合、では `Get-Help` ヘルプトピックがスクリプトではなく関数に関連付けられます。

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

PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv -outputPath `
C:\Reports\2009\January.csv
#>

param ([string]$InputPath, [string]$OutPutPath)

function Get-Data { }
...
```

次のコマンドは、スクリプトのヘルプを取得します。 スクリプトは環境変数に示されているディレクトリにないため `$env:Path` 、スクリプトヘルプを `Get-Help` 取得するコマンドではスクリプトパスを指定する必要があります。

```powershell
Get-Help -Path .\update-month.ps1 -Full
```

```Output
# NAME

C:\ps-test\Update-Month.ps1

# SYNOPSIS

Performs monthly data updates.

# SYNTAX

C:\ps-test\Update-Month.ps1 [-InputPath] <String> [[-OutputPath]
<String>] [<CommonParameters>]

# DESCRIPTION

The Update-Month.ps1 script updates the registry with new data
generated during the past month and generates a report.

# PARAMETERS

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

# INPUTS

None. You cannot pipe objects to Update-Month.ps1.

# OUTPUTS

None. Update-Month.ps1 does not generate any output.

Example 1

PS> .\Update-Month.ps1

Example 2

PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv

Example 3

PS> .\Update-Month.ps1 -inputpath C:\Data\January.csv -outputPath
C:\Reports\2009\January.csv

# RELATED LINKS
```

### <a name="redirecting-to-an-xml-file"></a>XML ファイルへのリダイレクト

関数とスクリプトの XML ベースのヘルプトピックを記述できます。 コメントベースのヘルプは実装が簡単ですが、更新可能なヘルプには XML ベースのヘルプが必要で、複数の言語ではヘルプトピックを提供します。

次の例は、Update-Month.ps1 スクリプトの最初の数行を示しています。
スクリプトでは、キーワードを使用して、 `.ExternalHelp` スクリプトの XML ベースのヘルプトピックへのパスを指定します。

キーワードの値は、 `.ExternalHelp` キーワードと同じ行に表示されることに注意してください。 その他の配置は無効になります。

```powershell
# .ExternalHelp C:\MyScripts\Update-Month-Help.xml

param ([string]$InputPath, [string]$OutPutPath)
function Get-Data { }
...
```

次の例は、関数内でのキーワードの有効な3つの配置を示して `.ExternalHelp` います。

```powershell
function Add-Extension
{
# .ExternalHelp C:\ps-test\Add-Extension.xml

param ([string] $name, [string]$extension = "txt")
$name = $name + "." + $extension
$name
}
```

```powershell
function Add-Extension
{
param ([string] $name, [string]$extension = "txt")
$name = $name + "." + $extension
$name

# .ExternalHelp C:\ps-test\Add-Extension.xml
}
```

```powershell
# .ExternalHelp C:\ps-test\Add-Extension.xml
function Add-Extension
{
param ([string] $name, [string]$extension = "txt")
$name = $name + "." + $extension
$name
}
```

### <a name="redirecting-to-a-different-help-topic"></a>別のヘルプトピックへのリダイレクト

次のコードは、PowerShell の組み込みヘルプ関数の先頭から抜粋したもので、一度に1画面のヘルプテキストを表示します。
コマンドレットのヘルプトピックでは `Get-Help` help 関数について説明しているので、help 関数はキーワードとキーワードを使用して `.ForwardHelpTargetName` ユーザーを `.ForwardHelpCategory` `Get-Help` コマンドレットのヘルプトピックにリダイレクトします。

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

次のコマンドは、この機能を使用します。

```powershell
Get-Help -Name help
```

```Output
NAME

Get-Help

SYNOPSIS

Displays information about PowerShell cmdlets and concepts.
...
```

## <a name="see-also"></a>関連項目

[about_Functions](about_Functions.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Scripts](about_Scripts.md)

[コマンドレットヘルプの記述方法](https://go.microsoft.com/fwlink/?LinkID=123415)
