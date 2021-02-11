---
description: 高度な関数でパラメーターセットを定義して使用する方法について説明します。
title: about_Parameter_Sets
ms.date: 01/05/2021
ms.openlocfilehash: 8f3a33345a8e2fa19810c8ebd527d9a7dca7dec5
ms.sourcegitcommit: eb7ad1850550032880f5529b4e4281514cba1673
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97917809"
---
# <a name="about-parameter-sets"></a>パラメーターセットについて

## <a name="short-description"></a>概要
高度な関数でパラメーターセットを定義して使用する方法について説明します。

## <a name="long-description"></a>詳細説明

PowerShell では、パラメーターセットを使用して、さまざまなシナリオに対して異なるアクションを実行できる単一の関数を記述できます。 パラメーターセットを使用すると、さまざまなパラメーターをユーザーに公開できます。 また、ユーザーが指定したパラメーターに基づいて異なる情報を返すことができます。

## <a name="parameter-set-requirements"></a>パラメーターセットの要件

すべてのパラメーターセットには、次の要件が適用されます。

- 各パラメーターセットには、パラメーターの一意の組み合わせが必要です。 可能な場合は、一意のパラメーターの少なくとも1つが必須パラメーターである必要があります。

- 複数の位置指定パラメーターを含むパラメーターセットでは、パラメーターごとに一意の位置を定義する必要があります。 2つの位置指定パラメーターで同じ位置を指定することはできません。

- 値がのキーワードを宣言できるのは、set 内の1つのパラメーターだけ `ValueFromPipeline` `true` です。 複数のパラメーターでキーワードを定義し、値をにすることができ `ValueFromPipelineByPropertyName` `true` ます。

- パラメーターにパラメーターセットが指定されていない場合、パラメーターはすべてのパラメーターセットに属します。

> [!NOTE]
> パラメーターセットは32個に制限されています。

## <a name="default-parameter-sets"></a>既定のパラメーターセット

複数のパラメーターセットが定義されている場合は、この `DefaultParameterSetName` 属性のキーワードによって既定のパラメーターセットが指定されます。
PowerShell では、コマンドに指定された情報に基づいて、使用するパラメーターセットを特定できないときに、既定のパラメーターセットが使用されます。 参照 **バインド** 属性の詳細については、「 [about_functions_cmdletbindingattribute](about_functions_cmdletbindingattribute.md)」を参照してください。

## <a name="declaring-parameter-sets"></a>パラメーターセットの宣言

パラメーターセットを作成するには、パラメーター `ParameterSetName` セット内のすべてのパラメーターに対して、 **parameter** 属性のキーワードを指定する必要があります。 複数のパラメーターセットに属するパラメーターの場合は、パラメーターセットごとに **パラメーター** 属性を追加します。

パラメーター **属性を** 使用すると、パラメーターセットごとに異なる方法でパラメーターを定義できます。 たとえば、あるセットでは必須として、別のセットでは省略可能なパラメーターを定義できます。 ただし、各パラメーターセットには、少なくとも1つの一意のパラメーターを含める必要があります。

パラメーターセット名が割り当てられていないパラメーターは、すべてのパラメーターセットに属しています。

### <a name="example"></a>例

次の関数の例では、テキストファイル内の行数、文字、および単語数をカウントします。 パラメーターを使用すると、どの値を取得し、どのファイルを測定するかを指定できます。 次の4つのパラメーターセットが定義されています。

- パス
- PathAll
- LiteralPath
- LiteralPathAll

```powershell
function Measure-Lines {
    [CmdletBinding(DefaultParameterSetName = 'Path')]
    param (
        [Parameter(Mandatory = $true,
            ParameterSetName = 'Path',
            HelpMessage = 'Enter one or more filenames',
            Position = 0)]
        [Parameter(Mandatory = $true,
            ParameterSetName = 'PathAll',
            Position = 0)]
        [string[]]$Path,

        [Parameter(Mandatory = $true, ParameterSetName = 'LiteralPathAll')]
        [Parameter(Mandatory = $true,
            ParameterSetName = 'LiteralPath',
            HelpMessage = 'Enter a single filename',
            ValueFromPipeline = $true)]
        [string]$LiteralPath,

        [Parameter(ParameterSetName = 'Path')]
        [Parameter(ParameterSetName = 'LiteralPath')]
        [switch]$Lines,

        [Parameter(ParameterSetName = 'Path')]
        [Parameter(ParameterSetName = 'LiteralPath')]
        [switch]$Words,

        [Parameter(ParameterSetName = 'Path')]
        [Parameter(ParameterSetName = 'LiteralPath')]
        [switch]$Characters,

        [Parameter(Mandatory = $true, ParameterSetName = 'PathAll')]
        [Parameter(Mandatory = $true, ParameterSetName = 'LiteralPathAll')]
        [switch]$All,

        [Parameter(ParameterSetName = 'Path')]
        [Parameter(ParameterSetName = 'PathAll')]
        [switch]$Recurse
    )

    begin {
        if ($All) {
            $Lines = $Words = $Characters = $true
        }
        elseif (($Words -eq $false) -and ($Characters -eq $false)) {
            $Lines = $true
        }

        if ($Path) {
            $Files = Get-ChildItem -Path $Path -Recurse:$Recurse
        }
        else {
            $Files = Get-ChildItem -LiteralPath $LiteralPath
        }
    }
    process {
        foreach ($file in $Files) {
            $result = [ordered]@{ }
            $result.Add('File', $file.fullname)

            $content = Get-Content -LiteralPath $file.fullname

            if ($Lines) { $result.Add('Lines', $content.Length) }

            if ($Words) {
                $wc = 0
                foreach ($line in $content) { $wc += $line.split(' ').Length }
                $result.Add('Words', $wc)
            }

            if ($Characters) {
                $cc = 0
                foreach ($line in $content) { $cc += $line.Length }
                $result.Add('Characters', $cc)
            }

            New-Object -TypeName psobject -Property $result
        }
    }
}
```

各パラメーターセットには、一意のパラメーターまたはパラメーターの一意の組み合わせが必要です。 パラメーター `Path` `PathAll` セットとパラメーターセットはよく似ていますが、 **All** パラメーターはパラメーターセットに対して一意です `PathAll` 。 とパラメーターセットにも同じことが当てはまり `LiteralPath` `LiteralPathAll` ます。 `PathAll` `LiteralPathAll` パラメーターとパラメーターセットの両方に **All** パラメーターが設定されている場合でも、 **Path** パラメーターと **LiteralPath** パラメーターはそれらを区別します。

を使用 `Get-Command -Syntax` すると、各パラメーターセットの構文が表示されます。 ただし、パラメーターセットの名前は表示されません。 次の例では、各パラメーターセットで使用できるパラメーターを示します。

```powershell
(Get-Command Measure-Lines).ParameterSets |
  Select-Object -Property @{n='ParameterSetName';e={$_.name}},
    @{n='Parameters';e={$_.ToString()}}
```

```Output
ParameterSetName Parameters
---------------- ----------
Path             [-Path] <string[]> [-Lines] [-Words] [-Characters] [-Recurse] [<CommonParameters>]
PathAll          [-Path] <string[]> -All [-Recurse] [<CommonParameters>]
LiteralPath      -LiteralPath <string> [-Lines] [-Words] [-Characters] [<CommonParameters>]
LiteralPathAll   -LiteralPath <string> -All [<CommonParameters>]
```

### <a name="parameter-sets-in-action"></a>アクションのパラメーターセット

この例では、パラメーターセットを使用してい `PathAll` ます。

```powershell
Measure-Lines test* -All
```

```Output
File                       Lines Words Characters
----                       ----- ----- ----------
C:\temp\test\test.help.txt    31   562       2059
C:\temp\test\test.md          30  1527       3224
C:\temp\test\test.ps1          3     3         79
C:\temp\test\test[1].txt      31   562       2059
```
