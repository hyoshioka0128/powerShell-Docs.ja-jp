---
description: 関数が返すオブジェクトの種類を報告する属性について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Functions_OutputTypeAttribute
ms.openlocfilehash: 82f1615c4a2fe2a24f104d8e5637103dea6e5a0a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599929"
---
# <a name="about-functions-outputtypeattribute"></a>関数の概要 OutputTypeAttribute

## <a name="short-description"></a>概要
関数が返すオブジェクトの種類を報告する属性について説明します。

## <a name="long-description"></a>詳細説明

OutputType 属性は、関数が返すオブジェクトの .NET 型を一覧表示します。 省略可能な ParameterSetName パラメーターを使用して、各パラメーターセットに対して異なる出力の種類を一覧表示できます。

OutputType 属性は、単純関数と高度な関数でサポートされています。 これは、ファイルレットバインド属性に依存しません。

OutputType 属性は、コマンドレットが返す **、system.servicemodel オブジェクトの** outputtype プロパティの値を提供し `Get-Command` ます。

OutputType 属性値は、ドキュメントのメモにすぎません。 関数のコードから派生したり、実際の関数の出力と比較したりすることはできません。 そのため、値が不正確になる可能性があります。

## <a name="syntax"></a>SYNTAX

関数の OutputType 属性の構文は次のとおりです。

```
[OutputType([<TypeLiteral>], ParameterSetName="<Name>")]
[OutputType("<TypeNameString>", ParameterSetName="<Name>")]
```

**ParameterSetName** パラメーターは省略可能です。

OutputType 属性では、複数の型を一覧表示できます。

```
[OutputType([<Type1>],[<Type2>],[<Type3>])]
```

**ParameterSetName** パラメーターを使用して、異なるパラメーターセットが異なる型を返すことを示すことができます。

```
[OutputType([<Type1>], ParameterSetName=("<Set1>","<Set2>"))]
[OutputType([<Type2>], ParameterSetName="<Set3>")]
```

OutputType 属性のステートメントを、ステートメントの前にある属性リストに配置し `Param` ます。

次の例は、単純な関数における OutputType 属性の配置を示しています。

```powershell
function SimpleFunction2
{
  [OutputType([<Type>])]
  Param ($Parameter1)

  <function body>
}
```

次の例は、高度な関数における OutputType 属性の配置を示しています。

```powershell
function AdvancedFunction1
{
  [OutputType([<Type>])]
  Param (
    [parameter(Mandatory=$true)]
    [String[]]
    $Parameter1
  )

  <function body>
}

function AdvancedFunction2
{
  [CmdletBinding(SupportsShouldProcess=<Boolean>)]
  [OutputType([<Type>])]
  Param (
    [parameter(Mandatory=$true)]
    [String[]]
    $Parameter1
  )

  <function body>
}
```

## <a name="examples"></a>例

### <a name="example-1-create-a-function-that-has-the-outputtype-of-string"></a>例 1: 文字列の OutputType を持つ関数を作成する

```powershell
function Send-Greeting
{
  [OutputType([String])]
  Param ($Name)

  "Hello, $Name"
}
```

結果の出力の種類のプロパティを表示するには、コマンドレットを使用し `Get-Command` ます。

```powershell
(Get-Command Send-Greeting).OutputType
```

```Output
Name                                               Type
----                                               ----
System.String                                      System.String
```

### <a name="example-2-use-the-output-attribute-to-indicate-dynamic-output-types"></a>例 2: 出力属性を使用して動的な出力の種類を指定する

次の高度な関数では、OutputType 属性を使用して、関数が関数コマンドで使用されるパラメーターセットに応じて異なる型を返すことを示しています。

```powershell
function Get-User
{
  [CmdletBinding(DefaultParameterSetName="ID")]

  [OutputType("System.Int32", ParameterSetName="ID")]
  [OutputType([String], ParameterSetName="Name")]

  Param (
    [parameter(Mandatory=$true, ParameterSetName="ID")]
    [Int[]]
    $UserID,

    [parameter(Mandatory=$true, ParameterSetName="Name")]
    [String[]]
    $UserName
  )

  <function body>
}
```

### <a name="example-3-shows-when-an-actual-output-differs-from-the-outputtype"></a>例 3: 実際の出力が OutputType と異なるタイミングを示します。

次の例では、正確でない場合でも、[出力の種類] プロパティの値に OutputType 属性の値が表示されることを示します。

関数は、 `Get-Time` 任意の DateTime オブジェクトの短い形式の時刻を含む文字列を返します。 ただし、OutputType 属性は、 **system.string オブジェクトが** 返されることを報告します。

```powershell
function Get-Time
{
  [OutputType([DateTime])]
  Param (
    [parameter(Mandatory=$true)]
    [Datetime]$DateTime
  )

  $DateTime.ToShortTimeString()
}
```

メソッドは、 `GetType()` 関数が文字列を返すことを確認します。

```powershell
(Get-Time -DateTime (Get-Date)).GetType().FullName
```

```Output
System.String
```

ただし、outputtype 属性から値を取得する OutputType プロパティは、関数が **DateTime** オブジェクトを返すことを報告します。

```powershell
(Get-Command Get-Time).OutputType
```

```Output
Name                                      Type
----                                      ----
System.DateTime                           System.DateTime
```

### <a name="example-4-a-function--that-shouldnt-have-output"></a>例 4: 出力を持たない関数

次の例は、およびアクションを実行する必要がありますが、何も返さないカスタム関数を示しています。

```powershell
function Invoke-Notepad
{
  [OutputType([System.Void])]
  Param ()
  & notepad.exe | Out-Null
}
```

## <a name="notes"></a>注

**Functioninfo** オブジェクトの OutputType プロパティの値は、 **PSTypeName** オブジェクトの配列であり、それぞれに Name プロパティと Type プロパティがあります。

各出力の種類の名前のみを取得するには、次の形式のコマンドを使用します。

```powershell
(Get-Command Get-Time).OutputType | ForEach {$_.Name}
```

OutputType プロパティの値には null を指定できます。 出力が、 **WMI** オブジェクトやオブジェクトの書式付きビューなどの .net 型ではない場合は、null 値を使用します。

## <a name="see-also"></a>関連項目

[about_Functions](about_Functions.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

