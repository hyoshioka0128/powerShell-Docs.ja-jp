---
description: 参照型の変数を作成して使用する方法について説明します。 参照型の変数を使用すると、関数に渡される変数の値を変更することができます。
Locale: en-US
ms.date: 08/24/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_ref?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Ref
ms.openlocfilehash: 5b966816c8ac1ef91e03c78378f57fa20b5697de
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602620"
---
# <a name="about-ref"></a>Ref について

## <a name="short-description"></a>簡単な説明
参照型の変数を作成して使用する方法について説明します。 参照型の変数を使用すると、関数に渡される変数の値を変更することができます。

## <a name="long-description"></a>長い説明

変数は、参照または *値* に *よって* 関数に渡すことができます。

変数を *値で* 渡すと、データのコピーが渡されます。

次の例では、関数は、渡された変数の値を変更します。 PowerShell では、整数は値型であるため、値によって渡されます。
したがって、の値 `$var` は、関数のスコープ外でも変更されません。

```powershell
Function Test($data)
{
    $data = 3
}

$var = 10
Test -data $var
$var
```

```output
10
```

次の例では、を含む変数 `Hashtable` が関数に渡されます。 `Hashtable` はオブジェクト型であるため、既定では、 *参照によって* 関数に渡されます。

変数を *参照* 渡しで渡す場合、関数はデータを変更することができ、関数が実行された後もその変更は保持されます。

```powershell
Function Test($data)
{
    $data.Test = "New Text"
}

$var = @{}
Test -data $var
$var
```

```output
Name                           Value
----                           -----
Test                           New Text
```

関数は、関数のスコープの外部に保持される新しいキーと値のペアを追加します。

### <a name="writing-functions-to-accept-reference-parameters"></a>参照パラメーターを受け取る関数の記述

渡されたデータの型に関係なく、パラメーターを参照として取得するように関数をコーディングできます。 そのためには、パラメーターの型を、またはとして指定する必要があり `System.Management.Automation.PSReference` `[ref]` ます。

参照を使用する場合は、型のプロパティを使用してデータにアクセスする必要があり `Value` `System.Management.Automation.PSReference` ます。

```powershell
Function Test([ref]$data)
{
    $data.Value = 3
}
```

参照を必要とするパラメーターに変数を渡すには、「変数を参照としてキャストする」と入力する必要があります。

> [!NOTE]
> 角かっことかっこは両方とも必要です。

```powershell
$var = 10
Test -data ([ref]$var)
$var
```

```output
3
```

### <a name="passing-references-to-net-methods"></a>.NET メソッドへの参照の引き渡し

一部の .NET メソッドでは、参照として変数を渡すことが必要になる場合があります。 メソッドの定義で、 `in` パラメーターにキーワード、、またはが使用されている場合は、 `out` `ref` 参照が想定されます。

```powershell
[int] | Get-Member -Static -Name TryParse
```

```output
Name     MemberType Definition
----     ---------- ----------
TryParse Method     static bool TryParse(string s, [ref] int result)
```

メソッドは、 `TryParse` 文字列を整数として解析しようとします。 メソッドが成功すると、が返され、 `$true` 結果は **参照によって** 渡された変数に格納されます。

```
PS> $number = 0
PS> [int]::TryParse("15", ([ref]$number))
True
PS> $number
15
```

### <a name="references-and-scopes"></a>参照とスコープ

参照を使用すると、親スコープ内の変数の値を子スコープ内で変更できます。

```powershell
# Create a value type variable.
$i = 0
# Create a reference type variable.
$iRef = [ref]0
# Invoke a scriptblock to attempt to change both values.
&{$i++;$iRef.Value++}
# Output the results.
"`$i = $i;`$iRef = $($iRef.Value)"
```

```output
$i = 0;$iRef = 1
```

参照型の変数のみが変更されました。

## <a name="see-also"></a>関連項目

[about_Variables](about_Variables.md)

[about_Environment_Variables](about_Environment_Variables.md)

[about_Functions](about_Functions.md)

[about_Script_Blocks](about_Script_Blocks.md)

[about_Scopes](about_scopes.md)

