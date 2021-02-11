---
description: PowerShell で使用される構文図について説明します。
ms.date: 06/27/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_command_syntax?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Command_Syntax
ms.openlocfilehash: 8182cc856de0813f0828400bcef7170a6e165c51
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602398"
---
# <a name="about-command-syntax"></a>コマンド構文について

## <a name="short-description"></a>概要
PowerShell で使用される構文図について説明します。

## <a name="long-description"></a>詳細説明

[Get-help](xref:Microsoft.PowerShell.Core.Get-Help)および[get Command](xref:Microsoft.PowerShell.Core.Get-Command)コマンドレットは、コマンドを正しく構築するのに役立つ構文図を表示します。 このトピックでは、構文図を解釈する方法について説明します。

## <a name="syntax-diagrams"></a>構文図

コマンド構文図の各段落は、有効な形式のコマンドを表しています。

コマンドを構築するには、左から右の構文図に従います。 省略可能なパラメーターの中から選択し、プレースホルダーの値を指定します。

PowerShell では、構文図に次の表記を使用します。

```powershell
<command-name> -<Required Parameter Name> <Required Parameter Value>
[-<Optional Parameter Name> <Optional Parameter Value>]
[-<Optional Switch Parameters>]
[-<Optional Parameter Name>] <Required Parameter Value>
```

次に、 [新しいエイリアス](xref:Microsoft.PowerShell.Utility.New-Alias) のコマンドレットの構文を示します。

```powershell
New-Alias [-Name] <string> [-Value] <string> [-Description <string>]
[-Force] [-Option {None | ReadOnly | Constant | Private | AllScope}]
[-PassThru] [-Scope <string>] [-Confirm] [-WhatIf] [<CommonParameters>]
```

構文は読みやすくするために大文字で表記されますが、PowerShell では大文字と小文字が区別されません。

構文ダイアグラムには、次の要素があります。

## <a name="command-name"></a>[コマンド名]

コマンドは、のように、常にコマンド名で始まり `New-Alias` ます。 コマンド名またはそのエイリアス (たとえば、の "gcm") を入力し `Get-Command` ます。

## <a name="parameters"></a>パラメーター

コマンドのパラメーターは、コマンドの動作を決定するオプションです。
一部のパラメーターは、"値" を受け取ります。これは、コマンドに対するユーザー入力です。

たとえば、コマンドには `Get-Help` **name** パラメーターがあり、ヘルプを表示するトピックの名前を指定できます。 トピック名は、 **name** パラメーターの値です。

PowerShell コマンドでは、パラメーター名は常にハイフンで始まります。
ハイフンは、コマンド内の項目がパラメーター名であることを PowerShell に伝えます。

たとえば、の Name パラメーターを使用するに `New-Alias` は、次のように入力します。

```powershell
-Name
```

パラメーターには、必須またはオプションを指定できます。 構文図では、省略可能な項目が角かっこで囲まれてい `[ ]` ます。

パラメーターの詳細については、「 [about_Parameters](about_Parameters.md)」を参照してください。

## <a name="parameter-values"></a>パラメーター値

パラメーター値は、パラメーターが受け取る入力です。 Windows PowerShell は Microsoft .NET Framework に基づいているため、パラメーター値は、.NET 型によって構文図で表現されます。

たとえば、の Name パラメーターは、 `Get-Help` "string" 値を受け取ります。これは、1つの単語または引用符で囲まれた複数の単語などのテキスト文字列です。

```powershell
[-Name] <string>
```

パラメーター値の .NET 型は、 `< >` 値のプレースホルダーであり、コマンドで入力するリテラルではないことを示すために山かっこで囲まれています。

パラメーターを使用するには、.NET 型のプレースホルダーを、指定された .NET 型のオブジェクトに置き換えます。

たとえば、 **Name** パラメーターを使用するには、「-Name」に続けて、次のような文字列を入力します。

```powershell
-Name MyAlias
```

## <a name="parameters-with-no-values"></a>値のないパラメーター

一部のパラメーターは入力を受け付けないため、パラメーター値を持ちません。
値のないパラメーターは、スイッチのオン/オフのように機能するため、"スイッチパラメーター" と呼ばれます。 これらのコマンドを使用するか ()、コマンドからそれらを省略します (オフ)。

スイッチパラメーターを使用するには、パラメーター名の前にハイフンを入力するだけです。

たとえば、コマンドレットの **WhatIf** パラメーターを使用するに `New-Alias` は、次のように入力します。

```powershell
-WhatIf
```

## <a name="parameter-sets"></a>パラメーターセット

コマンドのパラメーターは、[パラメーターセット] に一覧表示されます。 パラメーターセットは、構文図の段落のようになります。

`New-Alias`コマンドレットには1つのパラメーターセットがありますが、多くのコマンドレットには複数のパラメーターセットがあります。 コマンドレットのパラメーターの中には、パラメーターセットに固有のものや、複数のパラメーターセットに含まれるものがあります。 各パラメーターセットは、有効なコマンドの形式を表します。 パラメーターセットには、コマンドで一緒に使用できるパラメーターだけが含まれています。 パラメーターを同じコマンド内で使用できない場合は、個別のパラメーターセットに含まれます。

たとえば、 [Get Random](xref:Microsoft.PowerShell.Utility.Get-Random) コマンドレットには、次のパラメーターセットがあります。

```powershell
Get-Random [[-Maximum] <Object>] [-Minimum <Object>] [-SetSeed <int>]
[<CommonParameters>]

Get-Random [-InputObject] <Object[]> [-Count <int>] [-SetSeed <int>]
[<CommonParameters>]
```

乱数を返す最初のパラメーターセットには、 **最小** 値と **最大値** のパラメーターがあります。 2番目のパラメーターセットは、オブジェクトのセットからランダムに選択されたオブジェクトを返し、 **InputObject** パラメーターと **Count** パラメーターを含みます。 どちらのパラメーターセットにも **Setseed** パラメーターと共通パラメーターがあります。

これらのパラメーターセットは、同じコマンドで **InputObject** および **count** パラメーターを使用できることを示していますが、同じコマンドで **Maximum** パラメーターと **count** パラメーターを使用することはできません。

パラメーターセットのパラメーターを使用して、使用するパラメーターセットを指定します。

ただし、すべてのコマンドレットには、既定のパラメーターセットもあります。 パラメーターセットに固有のパラメーターを指定しない場合は、既定のパラメーターセットが使用されます。 たとえば、パラメーターを指定せずにを使用する場合、 `Get-Random` Windows PowerShell は **number** パラメーターセットを使用していると想定し、乱数を返します。

各パラメーターセットでは、パラメーターが位置の順序で表示されます。 コマンド内のパラメーターの順序は、省略可能なパラメーター名を省略した場合にのみ重要です。 パラメーター名を省略すると、PowerShell では、位置と型によってパラメーターに値が割り当てられます。 パラメーターの位置の詳細については、「」を参照してください `about_Parameters` 。

## <a name="symbols-in-syntax-diagrams"></a>構文図のシンボル

構文ダイアグラムには、コマンド名、コマンドパラメーター、およびパラメーター値が表示されます。 また、シンボルを使用して、有効なコマンドを作成する方法を示します。

構文ダイアグラムでは、次の記号が使用されます。

- ハイフンは、 `-` パラメーター名を示します。 コマンドで、構文図に示すように、スペースを含めずに、パラメーター名の直前にハイフンを入力します。

  たとえば、の **Name** パラメーターを使用するには `New-Alias` 、次のように入力します。

  ```powershell
  -Name
  ```

- 山かっこは `<>` 、プレースホルダーテキストを示します。 コマンドで山かっこやプレースホルダーテキストを入力することはできません。 代わりに、それが記述されている項目に置き換えます。

  山かっこは、パラメーターの値の .NET 型を識別するために使用されます。 たとえば、コマンドレットの **Name** パラメーターを使用するに `New-Alias` は、を `<string>` 1 つの単語または引用符で囲まれた単語のグループである文字列に置き換えます。

- 角かっこは `[ ]` 省略可能な項目を示します。 パラメーターとその値は省略可能です。また、必須パラメーターの名前は省略可能です。

  たとえば、の **Description** パラメーター `New-Alias` とその値は、両方とも省略可能であるため、角かっこで囲まれています。

  ```powershell
  [-Description <string>]
  ```

  また、角かっこは、Name パラメーターの値が必須であることを示してい `<string>` ますが、パラメーター名 "name" は省略可能です。

  ```powershell
  [-Name] <string>
  ```

- .NET 型に右および左の角かっこが追加された場合、 `[]` その型の1つまたは複数の値をパラメーターが受け入れることができることを示します。 コンマ区切りのリストで、値を入力します。

  たとえば、コマンドレットの **name** パラメーターには `New-Alias` 1 つの文字列しか使用できませんが、 [Get Process](xref:Microsoft.PowerShell.Management.Get-Process)の **name** パラメーターは、1つまたは複数の文字列を受け取ることができます。

  ```powershell
  New-Alias [-Name] <string>

  New-Alias -Name MyAlias
  ```

  ```powershell
  Get-Process [-Name] <string[]>

  Get-Process -Name Explorer, Winlogon, Services
  ```

- 中かっこ `{}` は、パラメーターの有効な値のセットである "列挙型" を示します。

  中かっこ内の値は、縦棒で区切られ `|` ます。 これらのバーは "排他的" または "選択" を示します。つまり、中かっこ内に一覧表示される値のセットから1つの値のみを選択できます。

  たとえば、コマンドレットの構文には、 `New-Alias` **Option** パラメーターの次の値の列挙が含まれています。

  ```powershell
  -Option {None | ReadOnly | Constant | Private | AllScope}
  ```

  中かっこと縦棒は、"ReadOnly" や "AllScope" など、 **オプション** パラメーターに対して一覧表示されているいずれかの値を選択できることを示しています。

  ```powershell
  -Option ReadOnly
  ```

## <a name="optional-items"></a>オプションの項目

省略可能な項目は角かっこで `[]` 囲みます。 たとえば、 `New-Alias` コマンドレットの構文の説明では、 **Scope** パラメーターは省略可能です。 この構文では、パラメーター名と型を囲む角かっこで示されています。

```powershell
[-Scope <string>]
```

次の例はどちらも、コマンドレットの正しい使用法です `New-Alias` 。

```powershell
New-Alias -Name utd -Value Update-TypeData
New-Alias -Name utd -Value Update-TypeData -Scope Global
```

パラメーターの値が必要な場合でも、パラメーター名を省略可能にすることができます。 この構文では、コマンドレットの例のように、パラメーターの型ではなく、パラメーター名を囲む角かっこで囲まれてい `New-Alias` ます。

```powershell
[-Name] <string> [-Value] <string>
```

次のコマンドでは、コマンドレットを正しく使用し `New-Alias` ます。 コマンドを実行すると、同じ結果が生成されます。

```powershell
New-Alias -Name utd -Value Update-TypeData
New-Alias -Name utd Update-TypeData
New-Alias utd -Value Update-TypeData
New-Alias utd Update-TypeData
```

パラメーター名が型指定されたステートメントに含まれていない場合、Windows PowerShell は引数の位置を使用して、パラメーターに値を割り当てようとします。

次の例は完全ではありません。

```powershell
New-Alias utd
```

このコマンドレットでは、 **名前** と **値** の両方のパラメーターの値が必要です。

構文の例では、.NET Framework 型に名前を付けてキャストする際にも角かっこが使用されます。 このコンテキストでは、角かっこは要素が省略可能であることを示していません。

## <a name="see-also"></a>関連項目

- [about_Parameters](about_Parameters.md)
- [Get-Command](xref:Microsoft.PowerShell.Core.Get-Command)
- [Get-Help](xref:Microsoft.PowerShell.Core.Get-Help)

