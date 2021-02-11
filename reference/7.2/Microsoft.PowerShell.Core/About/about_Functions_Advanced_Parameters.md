---
description: 高度な関数にパラメーターを追加する方法について説明します。
Locale: en-US
ms.date: 10/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Functions_Advanced_Parameters
ms.openlocfilehash: da21f6fb7d19fa2ffcd9cd6c5eea217792937ae4
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599288"
---
# <a name="about-functions-advanced-parameters"></a>関数の詳細パラメーターの概要

## <a name="short-description"></a>簡単な説明

高度な関数にパラメーターを追加する方法について説明します。

## <a name="long-description"></a>長い説明

記述する高度な関数にパラメーターを追加し、パラメーターの属性と引数を使用して、ユーザーがパラメーターを使用して送信するパラメーターの値を制限することができます。

関数に追加するパラメーターは、PowerShell がすべてのコマンドレットと高度な関数に自動的に追加する共通パラメーターに加えて、ユーザーが使用できます。 PowerShell の共通パラメーターの詳細については、「 [about_CommonParameters](about_CommonParameters.md)」を参照してください。

PowerShell 3.0 以降では、スプラッティングをと共に使用して、コマンドのパラメーターを表すことができ `@Args` ます。 スプラッティングは、単純関数と高度な関数で有効です。 詳細については、「 [about_Functions](about_Functions.md) 」および「 [about_Splatting](about_Splatting.md)」を参照してください。

## <a name="type-conversion-of-parameter-values"></a>パラメーター値の型変換

異なる型を想定するパラメーターの引数として文字列を指定すると、PowerShell は文字列をパラメーターのターゲット型に暗黙的に変換します。
高度な関数は、カルチャに依存しないパラメーター値の解析を実行します。

これに対し、カルチャに依存した変換は、コンパイル済みのコマンドレットのパラメーターバインド中に実行されます。

この例では、パラメーターを受け取るコマンドレットとスクリプト関数を作成し `[datetime]` ます。 現在のカルチャはドイツ語の設定を使用するように変更されています。
ドイツ語形式の日付がパラメーターに渡されます。

```powershell
# Create a cmdlet that accepts a [datetime] argument.
Add-Type @'
  using System;
  using System.Management.Automation;
  [Cmdlet("Get", "Date_Cmdlet")]
  public class GetFooCmdlet : Cmdlet {

    [Parameter(Position=0)]
    public DateTime Date { get; set; }

    protected override void ProcessRecord() {
      WriteObject(Date);
    }
  }
'@ -PassThru | % Assembly | Import-Module

[cultureinfo]::CurrentCulture = 'de-DE'
$dateStr = '19-06-2018'

Get-Date_Cmdlet $dateStr
```

```Output
Dienstag, 19. Juni 2018 00:00:00
```

上記のように、コマンドレットでは、カルチャに依存した解析を使用して文字列を変換します。

```powershell
# Define an equivalent function.
function Get-Date_Func {
  param(
    [DateTime] $Date
  )
  process {
    $Date
  }
}

[cultureinfo]::CurrentCulture = 'de-DE'

# This German-format date string doesn't work with the invariant culture.
# E.g., [datetime] '19-06-2018' breaks.
$dateStr = '19-06-2018'

Get-Date_Func $dateStr
```

高度な関数では、カルチャに依存しない解析が使用されるため、次のエラーが発生します。

```Output
Get-Date_Func: Cannot process argument transformation on parameter 'Date'.
Cannot convert value "19-06-2018" to type "System.DateTime". Error: "String
'19-06-2018' was not recognized as a valid DateTime."
```

## <a name="static-parameters"></a>静的パラメーター

静的パラメーターは、関数で常に使用できるパラメーターです。
PowerShell コマンドレットとスクリプトのほとんどのパラメーターは静的パラメーターです。

次の例は、次の特性を持つ **ComputerName** パラメーターの宣言を示しています。

- 必須です (必須)。
- パイプラインからの入力を受け取ります。
- 文字列の配列を入力として受け取ります。

```powershell
Param(
    [Parameter(Mandatory=$true,
    ValueFromPipeline=$true)]
    [String[]]
    $ComputerName
)
```

## <a name="attributes-of-parameters"></a>パラメーターの属性

ここでは、関数のパラメーターに追加できる属性について説明します。

すべての属性は省略できます。 ただし、この属性を省略 **した場合** 、高度な関数として認識されるようにするには、関数に **パラメーター** 属性を含める必要があります。

各パラメーター宣言には、1つまたは複数の属性を追加できます。 パラメーター宣言に追加できる属性の数に制限はありません。

### <a name="parameter-attribute"></a>パラメーター属性

**パラメーター** 属性は、関数パラメーターの属性を宣言するために使用されます。

**Parameter** 属性は省略可能です。関数のどのパラメーターにも属性が必要ない場合は、省略できます。 しかし、単純な関数ではなく、高度な関数として認識されるようにするには、関数には、"属性 **" 属性また** は " **Parameter** " 属性またはその両方を指定する必要があります。

Parameter **属性に** は、パラメーターが必須か省略可能かなど、パラメーターの特性を定義する引数があります。

**パラメーター** 属性、引数、および引数値を宣言するには、次の構文を使用します。 引数とその値を囲むかっこは、余分なスペースを含まない **パラメーター** の後に指定する必要があります。

```powershell
Param(
    [Parameter(Argument=value)]
    $ParameterName
)
```

かっこ内で引数を区切るには、コンマを使用します。 **パラメーター** 属性の2つの引数を宣言するには、次の構文を使用します。

```powershell
Param(
    [Parameter(Argument1=value1,
    Argument2=value2)]
)
```

**Parameter 属性から** 省略した場合、 **parameter** 属性のブール型引数の既定値は **False** になります。 引数の値をに設定し `$true` ます。または、引数を名前で一覧表示するだけです。 たとえば、次の **パラメーター** 属性は同等です。

```powershell
Param(
    [Parameter(Mandatory=$true)]
)

# Boolean arguments can be defined using this shorthand syntax

Param(
    [Parameter(Mandatory)]
)
```

引数を指定せずに **パラメーター** 属性を使用する場合、 **この属性を** 使用する代わりに、属性名の後に続くかっこが必要になります。

```powershell
Param(
    [Parameter()]
    $ParameterName
)
```

#### <a name="mandatory-argument"></a>必須の引数

引数は、 `Mandatory` パラメーターが必須であることを示します。 この引数が指定されていない場合、パラメーターは省略可能です。

次の例では、 **ComputerName** パラメーターを宣言しています。 引数を使用して `Mandatory` 、パラメーターを必須にします。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [String[]]
    $ComputerName
)
```

#### <a name="position-argument"></a>Position 引数

引数は、パラメーター `Position` がコマンドで使用されるときにパラメーター名が必要かどうかを決定します。 パラメーターの宣言に引数が含まれている場合 `Position` 、パラメーター名を省略することができ、名前のないパラメーター値は、コマンド内の名前のないパラメーター値のリストの位置 (順序) によって PowerShell によって識別されます。

引数が `Position` 指定されていない場合、パラメーター名またはパラメーター名の別名または省略形は、パラメーターがコマンドで使用されるたびに、パラメーター値の前に配置する必要があります。

既定では、すべての関数のパラメーターは位置指定です。 PowerShell では、パラメーターが関数内で宣言されている順序で、パラメーターに位置番号が割り当てられます。 この機能を無効にするには、[ `PositionalBinding` 属性] の引数の値をに設定 `$False` します。 引数は、 `Position` 属性の引数の値よりも優先され `PositionalBinding` ます。  詳細については、「about_Functions_CmdletBindingAttribute」を参照してください `PositionalBinding` 。 [](about_Functions_CmdletBindingAttribute.md)

引数の値 `Position` は整数として指定されます。 Position 値 **0** はコマンド内の最初の位置を表し、position 値 **1** はコマンドの2番目の位置を表します。

関数に位置指定パラメーターがない場合、PowerShell では、パラメーターが宣言されている順序に基づいて各パラメーターに位置が割り当てられます。
ただし、ベストプラクティスとして、この割り当てに依存しないでください。 パラメーターを位置指定にする場合は、引数を使用し `Position` ます。

次の例では、 **ComputerName** パラメーターを宣言しています。 この例では、 `Position` 値が **0** の引数を使用しています。 結果として、コマンドからを省略した場合、 `-ComputerName` その値は、コマンドの最初のパラメーター値または無名のパラメーター値である必要があります。

```powershell
Param(
    [Parameter(Position=0)]
    [String[]]
    $ComputerName
)
```

#### <a name="parametersetname-argument"></a>ParameterSetName 引数

引数は、 `ParameterSetName` パラメーターが属するパラメーターセットを指定します。 パラメーターセットが指定されていない場合、パラメーターは、関数によって定義されたすべてのパラメーターセットに属します。 したがって、一意にするには、各パラメーターセットに、他のパラメーターセットのメンバーではないパラメーターが少なくとも1つ必要です。

> [!NOTE]
> コマンドレットまたは関数の場合、32パラメーターセットの制限があります。

次の例では、パラメーターセット内の **ComputerName** パラメーター `Computer` 、パラメーターセットの **UserName** パラメーター、 `User` および両方のパラメーターセットの **概要** パラメーターを宣言しています。

```powershell
Param(
    [Parameter(Mandatory=$true,
    ParameterSetName="Computer")]
    [String[]]
    $ComputerName,

    [Parameter(Mandatory=$true,
    ParameterSetName="User")]
    [String[]]
    $UserName,

    [Parameter(Mandatory=$false)]
    [Switch]
    $Summary
)
```

各引数に指定できる値は1つだけで `ParameterSetName` 、 `ParameterSetName` 各 **パラメーター** 属性には1つの引数のみを指定できます。 パラメーターが複数のパラメーターセットに含まれていることを示すには、 **パラメーター属性を追加し** ます。

次の例では、およびパラメーターセットに **Summary** パラメーターを明示的に追加し `Computer` `User` ます。 パラメーターセットでは **Summary** パラメーターを省略 `Computer` でき、パラメーターセットでは必須です `User` 。

```powershell
Param(
    [Parameter(Mandatory=$true,
    ParameterSetName="Computer")]
    [String[]]
    $ComputerName,

    [Parameter(Mandatory=$true,
    ParameterSetName="User")]
    [String[]]
    $UserName,

    [Parameter(Mandatory=$false, ParameterSetName="Computer")]
    [Parameter(Mandatory=$true, ParameterSetName="User")]
    [Switch]
    $Summary
)
```

パラメーターセットの詳細については、「 [パラメーターセットについ](about_parameter_sets.md)て」を参照してください。

#### <a name="valuefrompipeline-argument"></a>ValueFromPipeline 引数

引数は、 `ValueFromPipeline` パラメーターがパイプラインオブジェクトからの入力を受け入れることを示します。 オブジェクトのプロパティだけでなく、関数がオブジェクト全体を受け入れる場合は、この引数を指定します。

次の例では、必須の **ComputerName** パラメーターを宣言し、パイプラインから関数に渡されたオブジェクトを受け入れます。

```powershell
Param(
    [Parameter(Mandatory=$true,
    ValueFromPipeline=$true)]
    [String[]]
    $ComputerName
)
```

#### <a name="valuefrompipelinebypropertyname-argument"></a>ValueFromPipelineByPropertyName 引数

引数は、 `ValueFromPipelineByPropertyName` パラメーターがパイプラインオブジェクトのプロパティからの入力を受け入れることを示します。 オブジェクトプロパティには、パラメーターと同じ名前またはエイリアスを指定する必要があります。

たとえば、関数に **computername** パラメーターがあり、パイプされたオブジェクトに **computername** プロパティがある場合、 **computername** プロパティの値は関数の **computername** パラメーターに割り当てられます。

次の例では、必須の **computername** パラメーターを宣言し、パイプラインを介して関数に渡されるオブジェクトの **computername** プロパティからの入力を受け入れます。

```powershell
Param(
    [Parameter(Mandatory=$true,
    ValueFromPipelineByPropertyName=$true)]
    [String[]]
    $ComputerName
)
```

> [!NOTE]
> パイプラインの入力 () または () を受け入れる型指定されたパラメーター `by Value` `by PropertyName` では、パラメーターで _遅延バインディング_ スクリプトブロックを使用できます。
>
> _遅延バインド_ スクリプトブロックは、 **parameterbinding** 中に自動的に実行されます。 結果は、パラメーターにバインドされます。 遅延バインディングは、型または型として定義されたパラメーターに対しては機能しません `ScriptBlock` `System.Object` 。 スクリプトブロックは呼び出され _ず_ に渡されます。
>
> _遅延バインドの_ スクリプトブロックについては、「 [about_Script_Blocks](about_Script_Blocks.md)」を参照してください。

#### <a name="valuefromremainingarguments-argument"></a>ValueFromRemainingArguments 引数

引数は、 `ValueFromRemainingArguments` パラメーターが、関数の他のパラメーターに割り当てられていないコマンド内のすべてのパラメーターの値を受け入れることを示します。

次の例では、必須の **値** パラメーターと、その関数に送信される残りのすべてのパラメーター値を受け入れる **残り** のパラメーターを宣言しています。

```powershell
function Test-Remainder
{
     param(
         [string]
         [Parameter(Mandatory = $true, Position=0)]
         $Value,
         [string[]]
         [Parameter(Position=1, ValueFromRemainingArguments)]
         $Remaining)
     "Found $($Remaining.Count) elements"
     for ($i = 0; $i -lt $Remaining.Count; $i++)
     {
        "${i}: $($Remaining[$i])"
     }
}
Test-Remainder first one,two
```

```Output
Found 2 elements
0: one
1: two
```

> [!NOTE]
> PowerShell 6.2 より前では、 **ValueFromRemainingArguments** コレクションはインデックス **0** の1つのエンティティとして結合されていました。

#### <a name="helpmessage-argument"></a>HelpMessage 引数

引数は、 `HelpMessage` パラメーターまたはその値の簡単な説明を含む文字列を指定します。 PowerShell では、コマンドに必須のパラメーター値が指定されていない場合に表示されるプロンプトに、このメッセージが表示されます。 この引数は、省略可能なパラメーターには影響しません。

次の例では、必須の **ComputerName** パラメーターと、必要なパラメーター値を説明するヘルプメッセージを宣言しています。

関数に他の [コメントベースのヘルプ](./about_comment_based_help.md) 構文 (など) がない場合は、 `.SYNOPSIS` このメッセージも出力に表示さ `Get-Help` れます。

```powershell
Param(
    [Parameter(Mandatory=$true,
    HelpMessage="Enter one or more computer names separated by commas.")]
    [String[]]
    $ComputerName
)
```

### <a name="alias-attribute"></a>Alias 属性

**Alias** 属性は、パラメーターの代替名を確立します。
パラメーターに割り当てることができるエイリアスの数に制限はありません。

次の例は、 **CN** および **MachineName** エイリアスを必須の **ComputerName** パラメーターに追加するパラメーター宣言を示しています。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [Alias("CN","MachineName")]
    [String[]]
    $ComputerName
)
```

### <a name="supportswildcards-attribute"></a>SupportsWildcards 属性

**SupportsWildcards** 属性は、パラメーターがワイルドカード値を受け入れることを示すために使用されます。 次の例は、ワイルドカード値をサポートする必須の **パス** パラメーターのパラメーター宣言を示しています。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [SupportsWildcards()]
    [String[]]
    $Path
)
```

この属性を使用しても、ワイルドカードのサポートは自動的に有効になりません。 コマンドレットの開発者は、ワイルドカード入力を処理するコードを実装する必要があります。 サポートされているワイルドカードは、基になる API または PowerShell プロバイダーによって異なります。 詳細については、「 [about_Wildcards](about_Wildcards.md)」を参照してください。

### <a name="parameter-and-variable-validation-attributes"></a>パラメーターと変数の検証属性

検証属性は、ユーザーが高度な関数を呼び出したときに送信されるパラメーター値をテストするように PowerShell に指示します。 パラメーター値がテストに失敗した場合は、エラーが生成され、関数は呼び出されません。 パラメーターの検証は、指定された入力にのみ適用され、既定値のようなその他の値は検証されません。

また、検証属性を使用して、ユーザーが変数に指定できる値を制限することもできます。 検証属性と共に型コンバーターを使用する場合は、属性の前に型コンバーターを定義する必要があります。

```powershell
[int32][AllowNull()] $number = 7
```

### <a name="allownull-validation-attribute"></a>AllowNull 検証属性

**Allownull** 属性では、必須パラメーターの値をにすることができ `$null` ます。 次の例では、 **null** 値を持つことができる Hashtable **computerinfo** パラメーターを宣言しています。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [AllowNull()]
    [hashtable]
    $ComputerInfo
)
```

> [!NOTE]
> 型コンバーターが string に設定されている場合、文字列型は null 値を受け入れないため、 **Allownull** 属性は機能しません。 このシナリオでは、 **AllowEmptyString** 属性を使用できます。

### <a name="allowemptystring-validation-attribute"></a>AllowEmptyString validation 属性

**AllowEmptyString** 属性を使用すると、必須パラメーターの値を空の文字列 () にすることができ `""` ます。 次の例では、空の文字列値を持つことができる **ComputerName** パラメーターを宣言しています。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [AllowEmptyString()]
    [String]
    $ComputerName
)
```

### <a name="allowemptycollection-validation-attribute"></a>AllowEmptyCollection 検証属性

**Allowemptycollection** 属性を使用すると、必須パラメーターの値を空のコレクションにすることができ `@()` ます。 次の例では、空のコレクション値を持つことができる **ComputerName** パラメーターを宣言しています。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [AllowEmptyCollection()]
    [String[]]
    $ComputerName
)
```

### <a name="validatecount-validation-attribute"></a>ValidateCount 検証属性

**Validatecount** 属性では、パラメーターで許容されるパラメーター値の最小数と最大数を指定します。 PowerShell では、関数を呼び出すコマンド内のパラメーター値の数がその範囲外の場合、エラーが生成されます。

次のパラメーター宣言では、1 ~ 5 個のパラメーター値を受け取る **ComputerName** パラメーターを作成します。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidateCount(1,5)]
    [String[]]
    $ComputerName
)
```

### <a name="validatelength-validation-attribute"></a>ValidateLength validation 属性

**ValidateLength** 属性は、パラメーターまたは変数値の文字の最小数と最大数を指定します。 パラメーターまたは変数に指定された値の長さが範囲外の場合、PowerShell ではエラーが生成されます。

次の例では、各コンピューター名は 1 ~ 10 文字である必要があります。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidateLength(1,10)]
    [String[]]
    $ComputerName
)
```

次の例では、変数の値は、 `$number` 長さが1文字以上、最大で10文字である必要があります。

```powershell
[Int32][ValidateLength(1,10)]$number = '01'
```

> [!NOTE]
> この例では、の値 `01` が単一引用符で囲まれています。 **ValidateLength** 属性では、引用符で囲まずに数値を使用することはできません。

### <a name="validatepattern-validation-attribute"></a>ValidatePattern 検証属性

**Validatepattern** 属性は、パラメーターまたは変数値と比較される正規表現を指定します。 値が正規表現パターンに一致しない場合、PowerShell ではエラーが生成されます。

次の例では、パラメーター値に4桁の数字を含める必要があり、各桁には 0 ~ 9 の数値を指定する必要があります。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidatePattern("[0-9][0-9][0-9][0-9]")]
    [String[]]
    $ComputerName
)
```

次の例では、変数の値は4桁の数字にする `$number` 必要があり、各桁には 0 ~ 9 の数値を指定する必要があります。

```powershell
[Int32][ValidatePattern("^[0-9][0-9][0-9][0-9]$")]$number = 1111
```

### <a name="validaterange-validation-attribute"></a>ValidateRange validation 属性

**ValidateRange** 属性は、各パラメーターまたは変数値の数値範囲または **Validaterangekind** 列挙値を指定します。
値が範囲外の場合、PowerShell はエラーを生成します。

**Validaterangekind** 列挙型では、次の値が許可されます。

- **正** -0 より大きい数値。
- **負** -0 未満の数値。
- **非正** -0 以下の数値。
- **負** でない-0 以上の数値。

次の例では、 **Attempts** パラメーターの値を0から10までの範囲で指定する必要があります。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidateRange(0,10)]
    [Int]
    $Attempts
)
```

次の例では、変数の値を `$number` 0 から10までの範囲で指定する必要があります。

```powershell
[Int32][ValidateRange(0,10)]$number = 5
```

次の例では、変数の値 `$number` が0より大きい必要があります。

```powershell
[Int32][ValidateRange("Positive")]$number = 1
```

### <a name="validatescript-validation-attribute"></a>ValidateScript validation 属性

**ValidateScript** 属性は、パラメーターまたは変数値の検証に使用されるスクリプトを指定します。 PowerShell は、値をスクリプトにパイプし、スクリプトがを返すか、 `$false` またはスクリプトが例外をスローした場合にエラーを生成します。

**ValidateScript** 属性を使用すると、検証されている値が変数にマップされ `$_` ます。 変数を使用し `$_` て、スクリプト内の値を参照できます。

次の例では、 **Eventdate** パラメーターの値が現在の日付以上である必要があります。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidateScript({$_ -ge (Get-Date)})]
    [DateTime]
    $EventDate
)
```

次の例では、変数の値 `$date` が現在の日付と時刻以上である必要があります。

```powershell
[DateTime][ValidateScript({$_ -ge (Get-Date)})]$date = (Get-Date)
```

> [!NOTE]
> **ValidateScript** を使用する場合は、パラメーターに値を渡すことはできません `$null` 。 Null 値を渡すと、 **ValidateScript** は引数を検証できません。

### <a name="validateset-attribute"></a>ValidateSet 属性

**Validateset** 属性では、パラメーターまたは変数に有効な値のセットを指定し、タブ補完を有効にします。 パラメーターまたは変数値がセット内の値と一致しない場合、PowerShell ではエラーが生成されます。 次の例では、 **Detail** パラメーターの値には、Low、Average、または High のみを指定できます。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidateSet("Low", "Average", "High")]
    [String[]]
    $Detail
)
```

次の例では、変数の値は、 `$flavor` チョコレート、Strawberry、またはバニラのいずれかである必要があります。

```powershell
[ValidateSet("Chocolate", "Strawberry", "Vanilla")]
[String]$flavor = "Strawberry"
```

検証は、スクリプト内でもその変数が割り当てられるたびに行われます。 たとえば、次の例では、実行時にエラーが発生します。

```powershell
Param(
    [ValidateSet("hello", "world")]
    [String]$Message
)

$Message = "bye"
```

#### <a name="dynamic-validateset-values"></a>動的 validateSet 値

**クラス** を使用して、実行時に **validateset** の値を動的に生成できます。 次の例では、変数の有効な値が、 `$Sound` 使用可能なサウンドファイルの3つのファイルシステムパスをチェックする **soundnames** という名前の **クラス** を使用して生成されています。

```powershell
Class SoundNames : System.Management.Automation.IValidateSetValuesGenerator {
    [String[]] GetValidValues() {
        $SoundPaths = '/System/Library/Sounds/',
            '/Library/Sounds','~/Library/Sounds'
        $SoundNames = ForEach ($SoundPath in $SoundPaths) {
            If (Test-Path $SoundPath) {
                (Get-ChildItem $SoundPath).BaseName
            }
        }
        return [String[]] $SoundNames
    }
}
```

クラスは、次のように `[SoundNames]` 動的な **validateset** 値として実装されます。

```powershell
Param(
    [ValidateSet([SoundNames])]
    [String]$Sound
)
```

### <a name="validatenotnull-validation-attribute"></a>ValidateNotNull 検証属性

**Validatenotnull** 属性は、パラメーター値をにすることができないことを指定し `$null` ます。 パラメーター値がの場合、PowerShell ではエラーが生成され `$null` ます。

**Validatenotnull** 属性は、パラメーターが省略可能で、型が定義されていない場合、または **オブジェクト** などの null 値を暗黙的に変換できない型コンバーターを持つ場合に使用するように設計されています。 **文字列** などの null 値を暗黙的に変換する型を指定した場合、 **validatenotnull** 属性を使用していても null 値は空の文字列に変換されます。 このシナリオでは、 **Validatenotnullorempty** を使用します。

次の例では、 **ID** パラメーターの値をにすることはできません `$null` 。

```powershell
Param(
    [Parameter(Mandatory=$false)]
    [ValidateNotNull()]
    $ID
)
```

### <a name="validatenotnullorempty-validation-attribute"></a>ValidateNotNullOrEmpty 検証属性

**Validatenotnullorempty** 属性は、パラメーター値をにすることができず、 `$null` 空の文字列 () にすることができないことを指定し `""` ます。 パラメーターが関数呼び出しで使用されていても、その値が `$null` 、空の文字列 ( `""` )、または空の配列の場合、PowerShell ではエラーが生成され `@()` ます。

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [String[]]
    $UserName
)
```

### <a name="validatedrive-validation-attribute"></a>ValidateDrive 検証属性

**Validatedrive** 属性は、パラメーター値がパスを表す必要があることを指定します。これは、許可されるドライブのみを指します。 パラメーター値が許可されている以外のドライブを参照している場合、PowerShell はエラーを生成します。 ドライブ自体を除き、パスの存在は検証されません。

相対パスを使用する場合は、現在のドライブがドライブの一覧に含まれている必要があります。

```powershell
Param(
    [ValidateDrive("C", "D", "Variable", "Function")]
    [String]$Path
)
```

### <a name="validateuserdrive-validation-attribute"></a>ValidateUserDrive 検証属性

**Validateuserdrive** 属性は、パラメーター値がドライブを参照するパスを表す必要があることを指定し `User` ます。 パスが別のドライブを参照している場合は、PowerShell によってエラーが生成されます。 検証属性は、パスのドライブ部分が存在するかどうかのみをテストします。

相対パスを使用する場合は、現在のドライブがである必要があり `User` ます。

```powershell
function Test-UserDrivePath{
    [OutputType([bool])]
    Param(
      [Parameter(Mandatory=, Position=0)][ValidateUserDrive()][String]$Path
      )
    $True
}

Test-UserDrivePath -Path C:\
```

```Output
Test-UserDrivePath: Cannot validate argument on parameter 'Path'. The path
argument drive C does not belong to the set of approved drives: User.
Supply a path argument with an approved drive.
```

```powershell
Test-UserDrivePath -Path 'User:\A_folder_that_does_not_exist'
```

```Output
Test-UserDrivePath: Cannot validate argument on parameter 'Path'. Cannot
find drive. A drive with the name 'User' does not exist.
```

ドライブは、 `User` 十分な管理 (JEA) セッション構成でのみ定義できます。 この例では、User: ドライブを作成します。

```powershell
New-PSDrive -Name 'User' -PSProvider FileSystem -Root $env:HOMEPATH
```

```Output
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
User               75.76         24.24 FileSystem    C:\Users\ExampleUser

```powershell
Test-UserDrivePath -Path 'User:\A_folder_that_does_not_exist'
```

```Output
True
```

### <a name="validatetrusteddata-validation-attribute"></a>ValidateTrustedData validation 属性

この属性は、PowerShell 6.1.1 で追加されました。

現時点では、属性は PowerShell 自体によって内部的に使用され、外部での使用を目的としたものではありません。

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターとは、特定の条件下でのみ使用可能なコマンドレット、関数、またはスクリプトのパラメーターです。

たとえば、いくつかのプロバイダーコマンドレットには、プロバイダードライブまたはプロバイダードライブの特定のパスでコマンドレットが使用されている場合にのみ使用可能なパラメーターがあります。 たとえば、 **Encoding** パラメーターは `Add-Content` 、 `Get-Content` `Set-Content` ファイルシステムドライブで使用されている場合にのみ、、、およびコマンドレットで使用できます。

また、関数コマンドで別のパラメーターが使用されている場合、または別のパラメーターに特定の値が含まれている場合にのみ表示されるパラメーターを作成することもできます。

動的パラメーターは役に立つ場合がありますが、ユーザーが検出するのが困難な場合があるため、必要な場合にのみ使用してください。 動的パラメーターを検索するには、ユーザーがプロバイダーのパスに存在しているか、コマンドレットの **ArgumentList** パラメーターを使用している `Get-Command` か、の **path** パラメーターを使用している必要があり `Get-Help` ます。

関数またはスクリプトの動的パラメーターを作成するには、キーワードを使用し `DynamicParam` ます。

構文は次のとおりです。

`DynamicParam {<statement-list>}`

[ステートメント] ボックスの一覧で、ステートメントを使用して、 `If` 関数でパラメーターを使用できる条件を指定します。

コマンドレットを使用して、 `New-Object` パラメーターを表す system.string オブジェクトを作成し、その名前を指定します。

コマンドを使用し `New-Object` て、パラメーターの属性を表すための system.object オブジェクトを作成できます。たとえば、**必須**、**位置**、 **valuefrompipeline** 、パラメーターセットなどです。

次の例では、 **Name** と **Path** という名前の標準パラメーターと、 **sjc-dp1** という名前の省略可能な動的パラメーターを使用したサンプル関数を示します。 **Sjc-dp1** パラメーターはパラメーターセットにあり、 `PSet1` の型を持ち `Int32` ます。
**Sjc-dp1** パラメーターは、 `Get-Sample` **Path** パラメーターの値がで始まっている場合にのみ、関数で使用でき `HKLM:` ます。これは、レジストリドライブで使用されていることを示し `HKEY_LOCAL_MACHINE` ます。

```powershell
function Get-Sample {
  [CmdletBinding()]
  Param([String]$Name, [String]$Path)

  DynamicParam
  {
    if ($Path.StartsWith("HKLM:"))
    {
      $attributes = New-Object -Type `
        System.Management.Automation.ParameterAttribute
      $attributes.ParameterSetName = "PSet1"
      $attributes.Mandatory = $false
      $attributeCollection = New-Object `
        -Type System.Collections.ObjectModel.Collection[System.Attribute]
      $attributeCollection.Add($attributes)

      $dynParam1 = New-Object -Type `
        System.Management.Automation.RuntimeDefinedParameter("DP1", [Int32],
          $attributeCollection)

      $paramDictionary = New-Object `
        -Type System.Management.Automation.RuntimeDefinedParameterDictionary
      $paramDictionary.Add("DP1", $dynParam1)
      return $paramDictionary
    }
  }
}
```

詳細については、「 [Runtimeのパラメーター](/dotnet/api/system.management.automation.runtimedefinedparameter)」を参照してください。

## <a name="switch-parameters"></a>スイッチ パラメーター

スイッチパラメーターはパラメーター値のないパラメーターです。 これらは、使用されている場合にのみ有効になり、効果は1つだけです。

たとえば、 **powershell.exe** の **noprofile** パラメーターはスイッチパラメーターです。

関数でスイッチパラメーターを作成するには、 `Switch` パラメーター定義で型を指定します。

次に例を示します。

```powershell
Param([Switch]<ParameterName>)
```

または、別のメソッドを使用することもできます。

```powershell
Param(
    [Parameter(Mandatory=$false)]
    [Switch]
    $<ParameterName>
)
```

スイッチパラメーターは簡単に使用でき、より複雑な構文を持つブール型パラメーターよりも優先されます。

たとえば、スイッチパラメーターを使用する場合、ユーザーはコマンドでパラメーターを入力します。

`-IncludeAll`

ブール型パラメーターを使用するには、ユーザーがパラメーターとブール値を入力します。

`-IncludeAll:$true`

スイッチパラメーターを作成するときは、パラメーター名を慎重に選択してください。 パラメーター名は、パラメーターの効果をユーザーに伝えるようにしてください。
値が必要であると考えられる **フィルター** や **最大** 値など、あいまいな用語を使用しないようにしてください。

## <a name="argumentcompleter-attribute"></a>ArgumentCompleter 属性

**ArgumentCompleter** 属性を使用すると、特定のパラメーターにタブ補完の値を追加できます。 タブ補完が必要な各パラメーターに対して、 **ArgumentCompleter** 属性を定義する必要があります。 **Dynamicparameters** と同様に、使用可能な値は、ユーザーがパラメーター名の後に <kbd>tab キー</kbd>を押したときに実行時に計算されます。

**ArgumentCompleter** 属性を追加するには、値を決定するスクリプトブロックを定義する必要があります。 スクリプトブロックは、次のパラメーターを以下の順序で指定する必要があります。 パラメーターの名前は、値が位置に指定されている場合には問題になりません。

構文は次のとおりです。

```powershell
Param(
    [Parameter(Mandatory)]
    [ArgumentCompleter({
        param ( $commandName,
                $parameterName,
                $wordToComplete,
                $commandAst,
                $fakeBoundParameters )
        # Perform calculation of tab completed values here.
    })]
)
```

### <a name="argumentcompleter-script-block"></a>ArgumentCompleter スクリプトブロック

スクリプトブロックのパラメーターは、次の値に設定されます。

- `$commandName` (位置 0)-このパラメーターには、スクリプトブロックがタブ補完を提供するコマンドの名前を設定します。
- `$parameterName` (位置 1)-このパラメーターは、値にタブ補完が必要なパラメーターに設定されています。
- `$wordToComplete` (位置 2)-このパラメーターは、 <kbd>Tab キー</kbd>を押す前にユーザーが指定した値に設定されます。スクリプトブロックは、この値を使用してタブ補完の値を決定する必要があります。
- `$commandAst` (位置 3)-このパラメーターは、現在の入力行の抽象構文ツリー (AST) に設定されます。 詳細については、「 [Ast クラス](/dotnet/api/system.management.automation.language.ast)」を参照してください。
- `$fakeBoundParameters` (位置 4)-このパラメーターは、 `$PSBoundParameters` ユーザーが <kbd>Tab キー</kbd>を押した前に、コマンドレットのを含むハッシュテーブルに設定されます。詳細については、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

**ArgumentCompleter** スクリプトブロックは `ForEach-Object` 、、 `Where-Object` 、またはその他の適切なメソッドなどのパイプラインを使用して値のロールアウトを解除する必要があります。
値の配列を返すと、PowerShell は配列全体を **1 つ** のタブ補完値として扱います。

次の例では、 **値** パラメーターにタブ補完機能を追加します。 **Value** パラメーターのみを指定した場合は、**値** に対して使用可能なすべての値 (引数) が表示されます。 **Type** パラメーターが指定されている場合、 **Value** パラメーターにはその型の有効な値のみが表示されます。

さらに、演算子は、 `-like` ユーザーが次のコマンドを入力して <kbd>タブ</kbd> 補完を使用する場合に、 **Apple** のみが返されるようにします。

`Test-ArgumentCompleter -Type Fruits -Value A`

```powershell
function Test-ArgumentCompleter {
[CmdletBinding()]
 param (
        [Parameter(Mandatory=$true)]
        [ValidateSet('Fruits', 'Vegetables')]
        $Type,
        [Parameter(Mandatory=$true)]
        [ArgumentCompleter( {
            param ( $commandName,
                    $parameterName,
                    $wordToComplete,
                    $commandAst,
                    $fakeBoundParameters )

            $possibleValues = @{
                Fruits = @('Apple', 'Orange', 'Banana')
                Vegetables = @('Tomato', 'Squash', 'Corn')
            }
            if ($fakeBoundParameters.ContainsKey('Type'))
            {
                $possibleValues[$fakeBoundParameters.Type] | Where-Object {
                    $_ -like "$wordToComplete*"
                }
            }
            else
            {
                $possibleValues.Values | ForEach-Object {$_}
            }
        } )]
        $Value
      )
}
```

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Functions](about_Functions.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

[about_Functions_OutputTypeAttribute](about_Functions_OutputTypeAttribute.md)
