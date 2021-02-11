---
description: Microsoft .NET 型を操作する演算子について説明します。
Locale: en-US
ms.date: 10/15/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_type_operators?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Type_Operators
ms.openlocfilehash: 66687a2282cfd321146888daad92fef40ef6cb0a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599729"
---
# <a name="about-type-operators"></a>型演算子の概要

## <a name="short-description"></a>概要
Microsoft .NET 型を操作する演算子について説明します。

## <a name="long-description"></a>詳細説明

ブール型の演算子 ( `-is` および `-isNot` ) は、オブジェクトが指定された .net 型のインスタンスであるかどうかを示します。 演算子は、 `-is` 型がと一致する場合は **TRUE** 、それ以外の場合は **FALSE** の値を返します。 `-isNot`型がと一致する場合、演算子は値 **FALSE** を返します。それ以外の場合は **TRUE** の値を返します。

演算子は、 `-as` 入力オブジェクトから指定された .net 型への変換を試みます。 成功した場合は、変換されたオブジェクトを返します。 失敗した場合は、を返し `$null` ます。 エラーは返されません。

次の表に、PowerShell の型演算子を示します。

|演算子|説明                |例                          |
|--------|---------------------------|---------------------------------|
|`-is`   |入力時に TRUE を返します|`(get-date) -is [DateTime]`      |
|        |はのインスタンスです。      |`True`                           |
|        |指定された .NET 型。       |                                 |
|`-isNot`|入力時に TRUE を返します|`(get-date) -isNot [DateTime]`   |
|        |のインスタンスではありません。     |`False`                          |
|        |specified.NET 型。        |                                 |
|`-as`   |入力をに変換します。  |`"5/7/07" -as [DateTime]`        |
|        |指定された .NET 型。       |`Monday, May 7, 2007 12:00:00 AM`|

型演算子の構文は次のとおりです。

```powershell
<input> <operator> [.NET type]
```

また、次の構文を使用することもできます。

```powershell
<input> <operator> ".NET type"
```

.NET 型は、角かっこで囲んだ型名として、 `[DateTime]` または system.string 用やなどの文字列として書き込むことができます `"DateTime"` **。** 型が system 名前空間のルートにない場合は、オブジェクトの種類の完全な名前を指定します。 "System." を省略できます。 たとえば、system.string **を指定する** には、 `[System.Diagnostics.Process]` 、 `[Diagnostics.Process]` 、またはを入力します `"Diagnostics.Process"` 。

型演算子は、常に入力オブジェクト全体を操作します。 つまり、入力オブジェクトがコレクションである場合は、コレクションの _要素_ の型ではなく、テストされる _コレクション_ 型です。

### <a name="-isisnot-operators"></a>-is/isNot 演算子

**ブール** 型の演算子 ( `-is` および `-isNot` ) は、入力がオブジェクトのコレクションであっても、常に **ブール** 値を返します。

`<input>`がと同じか、または .Net 型から _派生_ した型である場合、 `-is` 演算子はを返し `$True` ます。

たとえば、 **DirectoryInfo** 型は、 **filesysteminfo** 型から派生します。 したがって、両方の例で **True** が返されます。

```powershell
PS> (Get-Item /) -is [System.IO.DirectoryInfo]
True
PS> (Get-Item /) -is [System.IO.FileSystemInfo]
True
```

`-is`が `<input>` 比較でインターフェイスを実装している場合、演算子はインターフェイスを照合することもできます。 この例では、入力は配列です。 配列は、system.string インターフェイスを実装 **します。**

```powershell
PS> 1, 2 -is [System.Collections.IList]
True
```

### <a name="-as-operator"></a>-as 演算子

演算子は、 `-as` 入力オブジェクトから指定された .net 型への変換を試みます。 成功した場合は、変換されたオブジェクトを返します。 失敗した場合は、を返し `$null` ます。 エラーは返されません。

`<input>`が .Net 型から _派生_ した型である場合、は、 `-as` _渡さ_ れた入力オブジェクトを変更せずに返します。 たとえば、 **DirectoryInfo** 型は、 **filesysteminfo** 型から派生します。 そのため、次の例では、オブジェクトの種類が変更されていません。

```powershell
PS> $fsroot = (Get-Item /) -as [System.IO.FileSystemInfo]
PS> $fsroot.GetType().FullName
System.IO.DirectoryInfo
```

#### <a name="converting-the-datetime-type-is-culture-sensitive"></a>DateTime 型の変換はカルチャに依存します

型キャストとは異なり、 `[DateTime]` 演算子を使用した型への変換は、 `-as` 現在のカルチャの規則に従って書式設定された文字列に対してのみ機能します。

```powershell
PS> [cultureinfo]::CurrentCulture = 'fr-FR'
PS> '13/5/20' -as [datetime]

mercredi 13 mai 2020 00:00:00

PS> '05/13/20' -as [datetime]
PS> [datetime]'05/13/20'

mercredi 13 mai 2020 00:00:00

PS> [datetime]'13/05/20'
InvalidArgument: Cannot convert value "13/05/20" to type "System.DateTime".
Error: "String '13/05/20' was not recognized as a valid DateTime."
```

オブジェクトの .NET 型を検索するには、コマンドレットを使用し `Get-Member` ます。 または、すべてのオブジェクトの **GetType** メソッドを、このメソッドの **FullName** プロパティと共に使用します。 たとえば、次のステートメントは、コマンドの戻り値の型を取得し `Get-Culture` ます。

```powershell
PS> (Get-Culture).GetType().FullName
System.Globalization.CultureInfo
```

## <a name="examples"></a>例

次の例では、型演算子の使用方法を示します。

```powershell
PS> 32 -is [Float]
False

PS> 32 -is "int"
True

PS> (get-date) -is [DateTime]
True

PS> "12/31/2007" -is [DateTime]
False

PS> "12/31/2007" -is [String]
True

PS> (get-process PowerShell)[0] -is [System.Diagnostics.Process]
True

PS> (get-command get-member) -is [System.Management.Automation.CmdletInfo]
True
```

次の例は、入力がオブジェクトのコレクションである場合、一致する型がコレクションの .NET 型であり、コレクション内の個々のオブジェクトの型ではないことを示しています。

この例では、 `Get-Culture` コマンドレットとコマンドレットはどちらも、system.string `Get-UICulture` オブジェクトを返しますが、これらのオブジェクトのコレクションは system.object 配列です。 

```powershell
PS> (get-culture) -is [System.Globalization.CultureInfo]
True

PS> (get-uiculture) -is [System.Globalization.CultureInfo]
True

PS> (get-culture), (get-uiculture) -is [System.Globalization.CultureInfo]
False

PS> (get-culture), (get-uiculture) -is [Array]
True

PS> (get-culture), (get-uiculture) | foreach {
  $_ -is [System.Globalization.CultureInfo])
}
True
True

PS> (get-culture), (get-uiculture) -is [Object]
True
```

次の例は、演算子の使用方法を示して `-as` います。

```powershell
PS> "12/31/07" -is [DateTime]
False

PS> "12/31/07" -as [DateTime]
Monday, December 31, 2007 12:00:00 AM

PS> $date = "12/31/07" -as [DateTime]

C:\PS>$a -is [DateTime]
True

PS> 1031 -as [System.Globalization.CultureInfo]

LCID      Name      DisplayName
----      ----      -----------
1031      de-DE     German (Germany)
```

次の例は、演算子が `-as` 入力オブジェクトを .net 型に変換できない場合にを返すことを示して `$null` います。

```powershell
PS> 1031 -as [System.Diagnostics.Process]
PS>
```

## <a name="see-also"></a>関連項目

[about_Operators](about_Operators.md)
