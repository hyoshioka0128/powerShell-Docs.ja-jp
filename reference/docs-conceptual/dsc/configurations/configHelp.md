---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC 構成のヘルプの作成
description: DSC 構成では、コメント ベースのヘルプを使用できます。 ユーザーは、`-?` パラメーターを指定して Configuration を呼び出すか、Get-Help コマンドレットを使用することで、ヘルプにアクセスできます。
ms.openlocfilehash: 01c4f43253f4d4d8421ea3e0dfca797776acd426
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92658643"
---
# <a name="writing-help-for-dsc-configurations"></a>DSC 構成のヘルプの作成

> 適用先:Windows PowerShell 5.0

DSC 構成では、コメント ベースのヘルプを使用できます。 ユーザーは、`-?` を指定して **Configuration** を呼び出すか、 [Get-Help](/powershell/module/Microsoft.PowerShell.Core/Get-Help) コマンドレットを使用することで、ヘルプにアクセスできます。 コメント ベースのヘルプは `Configuration` キーワードのすぐ上に配置します。 パラメーターのヘルプは、コメント ブロックのインラインか、パラメーター宣言のすぐ上、または次の例のようにその両方に配置できます。

PowerShell のコメント ベースのヘルプの詳細については、「[about_Comment_Based_Help](/powershell/module/microsoft.powershell.core/about/about_comment_based_help)」を参照してください。

> [!NOTE]
> VS Code や ISE などの PowerShell 開発環境には、コメント ブロック テンプレートを自動的に挿入できるスニペットもあります。

次の例では、構成とそのコメント ベースのヘルプを含むスクリプトを示します。
この例では、パラメーターを含む構成を示します。 Configuration でのパラメーターの使用について詳しくは、「[構成にパラメーターを追加する](add-parameters-to-a-configuration.md)」をご覧ください。

```powershell
<#
.SYNOPSIS
A brief description of the function or script. This keyword can be used only once for each configuration.


.DESCRIPTION
A detailed description of the function or script. This keyword can be used only once for each
configuration.

.PARAMETER ComputerName
The description of a parameter. Add a .PARAMETER keyword for each parameter in the function or
script syntax.

Type the parameter name on the same line as the .PARAMETER keyword. Type the parameter description
on the lines following the .PARAMETER keyword. Windows PowerShell interprets all text between the
.PARAMETER line and the next keyword or the end of the comment block as part of the parameter
description. The description can include paragraph breaks.

The Parameter keywords can appear in any order in the comment block, but the function or script
syntax determines the order in which the parameters (and their descriptions) appear in help topic.
To change the order, change the syntax.

.EXAMPLE
HelpSample -ComputerName localhost

A sample command that uses the function or script, optionally followed by sample output and a
description. Repeat this keyword for each example. PowerShell automatically prefaces the first line
with a PowerShell prompt. Additional lines are treated as output and description. The example can
contain spaces, newlines and PowerShell code.

If you have multiple examples, there is no need to number them. PowerShell will number the examples in help text.

.EXAMPLE
HelpSample -FilePath "C:\output.txt"

This example will be labeled "EXAMPLE 2" when help is displayed to the user.
#>
configuration HelpSample1
{
    param
    (
        [string]$ComputerName = 'localhost',
        # Provide a PARAMETER section for each parameter that your script or function accepts.
        [string]$FilePath = 'C:\Destination.txt'
    )

    Node $ComputerName
    {
        File HelloWorld
        {
            Contents="Hello World"
            DestinationPath = $FilePath
        }
    }
}
```

## <a name="viewing-configuration-help"></a>構成のヘルプの表示

構成のヘルプを表示するには、`Get-Help` コマンドレットに関数名を付けて使うか、関数名の後に「`-?`」と入力します。 次に示すのは、前の Configuration を `Get-Help` に渡した場合の出力です。

```powershell
Get-Help HelpSample1 -Detailed
```

```Output
NAME
    HelpSample1

SYNOPSIS
    A brief description of the function or script. This keyword can be used only once for each configuration.


SYNTAX
    HelpSample1 [[-InstanceName] <String>] [[-DependsOn] <String[]>] [[-PsDscRunAsCredential] <PSCredential>]
      [[-OutputPath] <String>] [[-ConfigurationData] <Hashtable>] [[-ComputerName] <String>] [[-FilePath] <String>]
      [<CommonParameters>]


DESCRIPTION
    A detailed description of the function or script. This keyword can be used only once for each configuration.


PARAMETERS
    -InstanceName <String>

    -DependsOn <String[]>

    -PsDscRunAsCredential <PSCredential>

    -OutputPath <String>

    -ConfigurationData <Hashtable>

    -ComputerName <String>
        The description of a parameter. Add a .PARAMETER keyword for each parameter in the function
        or script syntax.

        Type the parameter name on the same line as the .PARAMETER keyword. Type the parameter
        description on the lines following the .PARAMETER keyword. Windows PowerShell interprets all
        text between the .PARAMETER line and the next keyword or the end of the comment block as
        part of the parameter description. The description can include paragraph breaks.

        The Parameter keywords can appear in any order in the comment block, but the function or
        script syntax determines the order in which the parameters (and their descriptions) appear
        in help topic. To change the order, change the syntax.

    -FilePath <String>
        Provide a PARAMETER section for each parameter that your script or function accepts.

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------

    PS C:\>HelpSample -ComputerName localhost

    A sample command that uses the function or script, optionally followed by sample output and a
    description. Repeat this keyword for each example. PowerShell automatically prefaces the first
    line with a PowerShell prompt. Additional lines are treated as output and description. The
    example can contain spaces, newlines and PowerShell code.

    If you have multiple examples, there is no need to number them. PowerShell will number the
    examples in help text.


    -------------------------- EXAMPLE 2 --------------------------

    PS C:\>HelpSample -FilePath "C:\output.txt"

    This example will be labeled "EXAMPLE 2" when help is displayed to the user.


REMARKS
    To see the examples, type: "get-help HelpSample1 -examples".
    For more information, type: "get-help HelpSample1 -detailed".
    For technical information, type: "get-help HelpSample1 -full".
```

> [!NOTE]
> 構文のフィールドとパラメーターの属性は、PowerShell によって自動的に生成されます。

## <a name="see-also"></a>参照

- [DSC 構成](configurations.md)
- [構成の作成、コンパイル、適用](write-compile-apply-configuration.md)
- [構成にパラメーターを追加する](add-parameters-to-a-configuration.md)
