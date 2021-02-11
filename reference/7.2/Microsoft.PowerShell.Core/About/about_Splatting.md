---
description: スプラッティングを使用して PowerShell のコマンドにパラメーターを渡す方法について説明します。
Locale: en-US
ms.date: 08/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_splatting?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Splatting
ms.openlocfilehash: e028f43828afd69d645591ab20795689cb115e12
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602424"
---
# <a name="about-splatting"></a>スプラッティングについて

## <a name="short-description"></a>簡単な説明

スプラッティングを使用して PowerShell のコマンドにパラメーターを渡す方法について説明します。

## <a name="long-description"></a>長い説明

スプラッティングは、パラメーター値のコレクションを1つの単位としてコマンドに渡す方法です。 PowerShell は、コマンドパラメーターを使用して、コレクション内の各値を関連付けます。 Splatted パラメーターの値は、標準の変数と同じように、名前付きのスプラッティング変数に格納されますが、ドル記号 () ではなくアットマーク () で始まり `@` `$` ます。 At 記号は、単一の値ではなく、値のコレクションを渡すことを PowerShell に指示します。

スプラッティングを使用すると、コマンドが短く、読みやすくなります。 さまざまなコマンド呼び出しでスプラッティング値を再利用し、スプラッティングを使用して、 `$PSBoundParameters` 自動変数から他のスクリプトや関数にパラメーター値を渡すことができます。

Windows PowerShell 3.0 以降では、スプラッティングを使用してコマンドのすべてのパラメーターを表すこともできます。

## <a name="syntax"></a>構文

```
<CommandName> <optional parameters> @<HashTable> <optional parameters>
<CommandName> <optional parameters> @<Array> <optional parameters>
```

パラメーター名を必要としない、位置指定パラメーターのパラメーター値を指定するには、配列構文を使用します。 パラメーターの名前と値のペアを指定するには、ハッシュテーブルの構文を使用します。 Splatted の値は、パラメーターリスト内の任意の場所に配置できます。

スプラッティングの場合、すべてのパラメーターを渡すためにハッシュテーブルまたは配列を使用する必要はありません。 スプラッティングを使用していくつかのパラメーターを渡し、位置またはパラメーター名で他のパラメーターを渡すことができます。 また、複数のオブジェクトを1つのコマンドで記号して、各パラメーターに対して複数の値を渡すことはできません。

PowerShell 7.1 では、コマンドでパラメーターを明示的に定義することによって、splatted パラメーターをオーバーライドできます。

## <a name="splatting-with-hash-tables"></a>スプラッティングとハッシュテーブル

ハッシュテーブルを使用して、パラメーターの名前と値のペアを記号します。 この形式は、位置指定パラメーターやスイッチパラメーターなど、すべてのパラメーターの型に対して使用できます。
位置指定パラメーターは名前で割り当てる必要があります。

次の例では、 `Copy-Item` Test.txt ファイルを同じディレクトリ内の Test2.txt ファイルにコピーする2つのコマンドを比較します。

最初の例では、パラメーター名が含まれている従来の形式を使用します。

```powershell
Copy-Item -Path "test.txt" -Destination "test2.txt" -WhatIf
```

2番目の例では、ハッシュテーブルスプラッティングを使用します。 最初のコマンドは、パラメーター名とパラメーター値のペアのハッシュテーブルを作成し、変数に格納し `$HashArguments` ます。 2番目のコマンドは、 `$HashArguments` スプラッティングを使用してコマンド内の変数を使用します。 At 記号 () は、 `@HashArguments` コマンドのドル記号 () に代わるもの `$HashArguments` です。

**WhatIf** スイッチパラメーターの値を指定するには、 `$True` またはを使用 `$False` します。

```powershell
$HashArguments = @{
  Path = "test.txt"
  Destination = "test2.txt"
  WhatIf = $true
}
Copy-Item @HashArguments
```

> [!NOTE]
> 最初のコマンドでは、アットマーク ( `@` ) は splatted 値ではなくハッシュテーブルを示します。 PowerShell のハッシュテーブルの構文は次のとおりです。 `@{<name>=<value>; <name>=<value>; ...}`

## <a name="splatting-with-arrays"></a>配列を使用したスプラッティング

配列を使用して、パラメーター名を必要としない位置指定パラメーターの値を記号します。 値は、配列内の位置番号の順序で指定する必要があります。

次の例では、 `Copy-Item` Test.txt ファイルを同じディレクトリ内の Test2.txt ファイルにコピーする2つのコマンドを比較します。

最初の例では、パラメーター名が省略されている従来の形式を使用します。 パラメーター値は、コマンドの位置の順序で表示されます。

```powershell
Copy-Item "test.txt" "test2.txt" -WhatIf
```

2番目の例では、配列スプラッティングを使用します。 最初のコマンドは、パラメーター値の配列を作成し、変数に格納し `$ArrayArguments` ます。 値は、配列内の位置の順序になっています。 2番目のコマンドは、 `$ArrayArguments` スプラッティングのコマンドで変数を使用します。 At 記号 () は、 `@ArrayArguments` コマンドのドル記号 () に代わるもの `$ArrayArguments` です。

```powershell
$ArrayArguments = "test.txt", "test2.txt"
Copy-Item @ArrayArguments -WhatIf
```

### <a name="using-the-argumentlist-parameter"></a>ArgumentList パラメーターの使用

いくつかのコマンドレットには、コマンドレットによって実行されるスクリプトブロックにパラメーター値を渡すために使用される **ArgumentList** パラメーターがあります。 **ArgumentList** パラメーターは、スクリプトブロックに渡される値の配列を受け取ります。 PowerShell は、実際には配列スプラッティングを使用して、値をスクリプトブロックのパラメーターにバインドします。 **ArgumentList** を使用する場合、1つのパラメーターにバインドされた単一のオブジェクトとして配列を渡す必要がある場合は、配列を別の配列の唯一の要素としてラップする必要があります。

次の例には、文字列の配列である1つのパラメーターを受け取るスクリプトブロックが含まれています。

```powershell
$array = 'Hello', 'World!'
Invoke-Command -ScriptBlock {
  param([string[]]$words) $words -join ' '
  } -ArgumentList $array
```
<!--
01234567890123456789012345678901234567890123456789012345678901234567890123456789
-->
この例では、の最初の項目だけ `$array` がスクリプトブロックに渡されます。

```Output
Hello
```

```powershell
$array = 'Hello', 'World!'
Invoke-Command -ScriptBlock {
  param([string[]]$words) $words -join ' '
} -ArgumentList (,$array)
```

この例では、配列 `$array` 全体が単一のオブジェクトとしてスクリプトブロックに渡されるように、が配列にラップされています。

```Output
Hello World!
```

## <a name="examples"></a>例

### <a name="example-1-reuse-splatted-parameters-in-different-commands"></a>例 1: 異なるコマンドで splatted パラメーターを再利用する

この例では、さまざまなコマンドで splatted 値を再利用する方法を示します。 この例のコマンドは、コマンドレットを使用して、 `Write-Host` ホストプログラムコンソールにメッセージを書き込みます。 スプラッティングを使用して、前景色と背景色を指定します。

すべてのコマンドの色を変更するには、変数の値を変更するだけ `$Colors` です。

最初のコマンドは、パラメーター名と値のハッシュテーブルを作成し、そのハッシュテーブルを変数に格納し `$Colors` ます。

```powershell
$Colors = @{ForegroundColor = "black"; BackgroundColor = "white"}
```

2番目と3番目のコマンドは、 `$Colors` コマンドでスプラッティングの変数を使用し `Write-Host` ます。 を使用するには `$Colors variable` 、ドル記号 ( `$Colors` ) をアットマーク () に置き換え `@Colors` ます。

```powershell
#Write a message with the colors in $Colors
Write-Host "This is a test." @Colors

#Write second message with same colors. The position of splatted
#hash table does not matter.
Write-Host @Colors "This is another test."
```

### <a name="example-2-forward-parameters-using-psboundparameters"></a>例 2: $PSBoundParameters を使用してパラメーターを転送する

この例では、スプラッティングと自動変数を使用して、他のコマンドにパラメーターを転送する方法を示し `$PSBoundParameters` ます。

`$PSBoundParameters`自動変数は、スクリプトまたは関数の実行時に使用されるすべてのパラメーター名と値を含むディクショナリオブジェクト (system.string) です。

次の例では、変数を使用して、 `$PSBoundParameters` スクリプトまたは関数に渡されたパラメーター値を関数から `Test2` 関数に転送し `Test1` ます。 から関数を呼び出すと、 `Test1` `Test2` スプラッティングが使用されます。

```powershell
function Test1
{
    param($a, $b, $c)

    $a
    $b
    $c
}

function Test2
{
    param($a, $b, $c)

    #Call the Test1 function with $a, $b, and $c.
    Test1 @PsBoundParameters

    #Call the Test1 function with $b and $c, but not with $a
    $LimitedParameters = $PSBoundParameters
    $LimitedParameters.Remove("a") | Out-Null
    Test1 @LimitedParameters
}
```

```Output
Test2 -a 1 -b 2 -c 3
1
2
3
2
3
```

### <a name="example-3-override-splatted-parameters-with-explicitly-defined-parameters"></a>例 3: 明示的に定義されたパラメーターを使用して splatted パラメーターをオーバーライドする

この例では、明示的に定義されたパラメーターを使用して splatted パラメーターをオーバーライドする方法を示します。 これは、新しいハッシュテーブルを作成したり、記号に使用しているハッシュテーブルの値を変更したりする必要がない場合に便利です。

変数には、 `$commonParams` 仮想マシンを作成するためのパラメーターが保存されて `East US` います。 変数は、 `$allVms` 作成する仮想マシンの一覧です。 リストをループ処理し、を使用して `$commonParams` 各仮想マシンを作成するためのパラメーターを記号します。 ただし、 `myVM2` 他の仮想マシンとは別のリージョンに作成する必要があります。 では、ハッシュテーブルを調整するのではなく `$commonParams` 、の **場所** パラメーターを明示的に定義して、の `New-AzVm` キーの値を置き換えることができ `Location` `$commonParams` ます。

```powershell
$commonParams = @{
    ResourceGroupName = "myResourceGroup"
    Location = "East US"
    VirtualNetworkName = "myVnet"
    SubnetName = "mySubnet"
    SecurityGroupName = "myNetworkSecurityGroup"
    PublicIpAddressName = "myPublicIpAddress"
}

$allVms = @('myVM1','myVM2','myVM3',)

foreach ($vm in $allVms)
{
    if ($vm -eq 'myVM2')
    {
        New-AzVm @commonParams -Name $vm -Location "West US"
    }
    else
    {
        New-AzVm @commonParams -Name $vm
    }
}
```

## <a name="splatting-command-parameters"></a>スプラッティングコマンドのパラメーター

スプラッティングを使用して、コマンドのパラメーターを表すことができます。 この手法は、プロキシ関数、つまり別のコマンドを呼び出す関数を作成するときに便利です。 この機能は、Windows PowerShell 3.0 で導入されました。

コマンドのパラメーターを記号するには、 `@Args` を使用してコマンドパラメーターを表します。 この手法は、コマンドパラメーターを列挙するよりも簡単です。呼び出されたコマンドのパラメーターが変更された場合でも、リビジョンなしで動作します。

この機能は、 `$Args` 割り当てられていないすべてのパラメーター値を含む自動変数を使用します。

たとえば、次の関数は、 `Get-Process` コマンドレットを呼び出します。 この関数では、は、 `@Args` コマンドレットのすべてのパラメーターを表し `Get-Process` ます。

```powershell
function Get-MyProcess { Get-Process @Args }
```

関数を使用すると、 `Get-MyProcess` 次のコマンドに示すように、割り当てられていないすべてのパラメーターとパラメーター値がに渡され `@Args` ます。

```powershell
Get-MyProcess -Name PowerShell
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    463      46   225484     237196   719    15.86   3228 powershell
```

```powershell
Get-MyProcess -Name PowerShell_Ise -FileVersionInfo
```

```Output
ProductVersion   FileVersion      FileName
--------------   -----------      --------
6.2.9200.16384   6.2.9200.1638... C:\Windows\system32\WindowsPowerShell\...
```

`@Args`パラメーターを明示的に宣言した関数でを使用できます。 関数内で複数回使用できますが、 `@Args` 次の例に示すように、入力したすべてのパラメーターがのすべてのインスタンスに渡されます。

```powershell
function Get-MyCommand
{
    Param ([switch]$P, [switch]$C)
    if ($P) { Get-Process @Args }
    if ($C) { Get-Command @Args }
}

Get-MyCommand -P -C -Name PowerShell
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
408      28    75568      83176   620     1.33   1692 powershell

Path               : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.e
Extension          : .exe
Definition         : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.e
Visibility         : Public
OutputType         : {System.String}
Name               : powershell.exe
CommandType        : Application
ModuleName         :
Module             :
RemotingCapability : PowerShell
Parameters         :
ParameterSets      :
HelpUri            :
FileVersionInfo    : File:             C:\Windows\System32\WindowsPowerShell
                     \v1.0\powershell.exe
                     InternalName:     POWERSHELL
                     OriginalFilename: PowerShell.EXE.MUI
                     FileVersion:      10.0.14393.0 (rs1_release.160715-1616
                     FileDescription:  Windows PowerShell
                     Product:          Microsoft Windows Operating System
                     ProductVersion:   10.0.14393.0
                     Debug:            False
                     Patched:          False
                     PreRelease:       False
                     PrivateBuild:     False
                     SpecialBuild:     False
                     Language:         English (United States)
```

## <a name="notes"></a>メモ

構成 **属性またはパラメーター属性を使用** して関数を高度な関数にすると、 `$args` 自動変数は関数で使用できなくなります。 高度な関数には、明示的なパラメーター定義が必要です。

PowerShell Desired State Configuration (DSC) は、スプラッティングを使用するように設計されていません。
スプラッティングを使用して、DSC リソースに値を渡すことはできません。 詳細については、「 [スプラッティング DSC リソース](https://gaelcolas.com/2017/11/05/pseudo-splatting-dsc-resources/)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Arrays](about_Arrays.md)

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Hash_Tables](about_Hash_Tables.md)

[about_Parameters](about_Parameters.md)
