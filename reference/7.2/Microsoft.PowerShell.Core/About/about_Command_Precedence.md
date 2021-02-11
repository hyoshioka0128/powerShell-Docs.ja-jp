---
description: PowerShell が実行するコマンドを決定する方法について説明します。
ms.date: 02/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_command_precedence?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Command_Precedence
ms.openlocfilehash: 0b1ad7dc0fae9c6c0ec16929b3d538e5db4aea55
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599726"
---
# <a name="about-command-precedence"></a>コマンドの優先順位について

## <a name="short-description"></a>簡単な説明
PowerShell が実行するコマンドを決定する方法について説明します。

## <a name="long-description"></a>長い説明

コマンドの優先順位は、セッションに同じ名前の複数のコマンドが含まれている場合に、どのコマンドを実行するかを PowerShell が決定する方法を示します。 セッション内のコマンドは、同じ名前のコマンドで非表示にしたり置き換えたりすることができます。 この記事では、非表示のコマンドを実行する方法と、コマンド名の競合を回避する方法について説明します。

## <a name="command-precedence"></a>コマンドの優先順位

PowerShell セッションに同じ名前の複数のコマンドが含まれている場合、PowerShell は次の規則を使用して実行するコマンドを決定します。

コマンドのパスを指定すると、PowerShell はパスで指定された場所でコマンドを実行します。

たとえば、次のコマンドでは、"C: FindDocs.ps1" ディレクトリ内のスクリプトが実行され \\ ます。

```
C:\TechDocs\FindDocs.ps1
```

セキュリティ機能として、PowerShell では、コマンドが Path 環境変数に記載されているパスに配置されている `$env:path` か、スクリプトファイルへのパスを指定していない限り、powershell スクリプトなどの実行可能 (ネイティブ) コマンドは実行されません。

現在のディレクトリにあるスクリプトを実行するには、完全なパスを指定するか、 `.\` 現在のディレクトリを表すドットを入力します。

たとえば、現在のディレクトリで FindDocs.ps1 ファイルを実行するには、次のように入力します。

```
.\FindDocs.ps1
```

### <a name="using-wildcards-in-execution"></a>実行でのワイルドカードの使用

コマンドの実行時には、ワイルドカードを使用できます。 ワイルドカード文字の使用は *グロビング* とも呼ばれます。

PowerShell は、リテラルが一致する前に、ワイルドカードと一致するファイルを実行します。

たとえば、次のファイルを含むディレクトリを考えてみます。

```
Get-ChildItem C:\temp\test


    Directory: C:\temp\test


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        5/20/2019   2:29 PM             28 a.ps1
-a----        5/20/2019   2:29 PM             28 [a1].ps1
```

両方のスクリプトファイルの内容は同じ `$MyInvocation.MyCommand.Path` です。
このコマンドは、呼び出されるスクリプトの名前を表示します。

を実行すると `[a1].ps1` 、 `a.ps1` ファイルがリテラル一致であってもファイルは実行され `[a1].ps1` ます。

```powershell
C:\temp\test\[a1].ps1
```

```Output
C:\temp\test\a.ps1
```

次に、ファイルを削除 `a.ps1` し、もう一度実行してみましょう。

```powershell
Remove-Item C:\temp\test\a.ps1
C:\temp\test\[a1].ps1
```

```Output
C:\temp\test\[a1].ps1
```

`[a1].ps1`リテラル一致がそのワイルドカードパターンと一致する唯一のファイルであるため、この時点で実行される出力から確認できます。

PowerShell でのワイルドカードの使用方法の詳細については、「 [about_Wildcards](about_Wildcards.md)」を参照してください。

> [!NOTE]
> 検索を相対パスに制限するには、スクリプト名の前にパスを付ける必要があり `.\` ます。 これにより、その相対パス内のファイルに対するコマンドの検索が制限されます。 このプレフィックスがないと、他の PowerShell 構文が競合する可能性があり、ファイルが確実に検出されることはほとんどありません。

パスを指定しない場合、PowerShell は、現在のセッションに読み込まれたすべての項目に対してコマンドを実行するときに、次の優先順位を使用します。

  1. エイリアス
  2. Function
  3. コマンドレット
  4. 外部実行可能ファイル (プログラムと PowerShell 以外のスクリプト)

したがって、「help」と入力すると、PowerShell はまずという名前のエイリアス、次にという名前の関数、最後にという名前のコマンドレットを検索し `help` `Help` `Help` ます。 最初に検出された項目が実行され `help` ます。

たとえば、という名前のコマンドレットと関数がセッションに含まれている場合、 `Get-Map` `Get-Map` PowerShell によって関数が実行されます。

> [!NOTE]
> これは、読み込まれたコマンドにのみ適用されます。 `build` `build` `Invoke-Build` 現在のセッションに読み込まれていないモジュール内にという名前の関数の実行可能ファイルとエイリアスがある場合は、PowerShell によって実行可能ファイルが実行され `build` ます。 この場合、モジュールが外部の実行可能ファイルを検出しても、モジュールは自動的に読み込まれません。 これは、指定された名前のエイリアス、関数、またはコマンドレットが呼び出され、そのモジュールの自動読み込みをトリガーする外部実行可能ファイルが見つからなかった場合にのみ発生します。

セッションに同じ種類の同じ名前の項目が含まれている場合、PowerShell は新しい項目を実行します。

たとえば、モジュールから別のコマンドレットをインポートする場合、 `Get-Date` 「」と入力すると、PowerShell によって、インポートされた `Get-Date` バージョンがネイティブのもので実行されます。

## <a name="hidden-and-replaced-items"></a>非表示の項目と置換された項目

これらのルールの結果として、同じ名前のアイテムでアイテムを置換または非表示にすることができます。

項目の名前をモジュールまたはスナップインの名前に修飾することによって、元の項目にアクセスできる場合は、項目が "非表示" または "影付き" になります。

たとえば、セッションのコマンドレットと同じ名前の関数をインポートした場合、コマンドレットはスナップインまたはモジュールからインポートされたため、非表示になります (ただし、置き換えられません)。

元の項目にアクセスできなくなった場合、項目は "置換" または "上書き" されます。

たとえば、セッションの変数と同じ名前の変数をインポートすると、元の変数は置き換えられ、アクセスできなくなります。
モジュール名を使用して変数を修飾することはできません。

また、コマンドラインで関数を入力し、同じ名前の関数をインポートすると、元の関数が置き換えられ、アクセスできなくなります。

### <a name="finding-hidden-commands"></a>非表示のコマンドの検索

[Get コマンド](xref:Microsoft.PowerShell.Core.Get-Command)レットの **all** パラメーターは、指定された名前を持つすべてのコマンドを取得します (非表示にしたり置き換えたりする場合でも)。 PowerShell 3.0 以降では、既定では、 `Get-Command` コマンド名を入力したときに実行されるコマンドのみが取得されます。

次の例では、セッションに `Get-Date` 関数と [Get Date](xref:Microsoft.PowerShell.Utility.Get-Date) コマンドレットが含まれています。

次のコマンドは、を入力したときに実行されるコマンドを取得し `Get-Date` `Get-Date` ます。

```powershell
Get-Command Get-Date
```

```output
CommandType     Name                      ModuleName
-----------     ----                      ----------
Function        Get-Date
```

次のコマンドでは、 **all** パラメーターを使用してすべてのコマンドを取得し `Get-Date` ます。

```powershell
Get-Command Get-Date -All
```

```output
CommandType     Name                      ModuleName
-----------     ----                      ----------
Function        Get-Date
Cmdlet          Get-Date                  Microsoft.PowerShell.Utility
```

### <a name="running-hidden-commands"></a>非表示のコマンドの実行

コマンドを、同じ名前を持つ他のコマンドと区別する項目プロパティを指定することで、特定のコマンドを実行できます。 このメソッドを使用して任意のコマンドを実行できますが、特に非表示のコマンドを実行する場合に便利です。

#### <a name="qualified-names"></a>修飾名

コマンドレットのモジュール修飾名を使用すると、同じ名前の項目によって非表示になっているコマンドを実行できます。 たとえば、コマンドレットを実行するには、 `Get-Date` モジュール名に " **Microsoft. PowerShell. ユーティリティ**" を付けます。

この推奨される方法は、配布するスクリプトを記述するときに使用します。 スクリプトが実行されるセッションに存在する可能性があるコマンドを予測することはできません。

```powershell
New-Alias -Name "Get-Date" -Value "Get-ChildItem"
Microsoft.PowerShell.Utility\Get-Date
```

```output
Tuesday, September 4, 2018 8:17:25 AM
```

`New-Map`モジュールによって追加されたコマンドを実行するには `MapFunctions` 、モジュール修飾名を使用します。

```powershell
MapFunctions\New-Map
```

コマンドがインポートされたモジュールを見つけるには、コマンドの **ModuleName** プロパティを使用します。

```
(Get-Command <command-name>).ModuleName
```

たとえば、コマンドレットのソースを検索するには、次のように `Get-Date` 入力します。

```powershell
(Get-Command Get-Date).ModuleName
```

```output
Microsoft.PowerShell.Utility
```

> [!NOTE]
> 変数または別名を修飾することはできません。

#### <a name="call-operator"></a>Call 演算子

また、演算子を使用して、 `Call` `&` [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem) (エイリアスは "dir") `Get-Command` または [get-Module](xref:Microsoft.PowerShell.Core.Get-Module)の呼び出しと組み合わせて、非表示のコマンドを実行することもできます。

呼び出し演算子は、子スコープ内で文字列とスクリプトブロックを実行します。 詳細については、「 [about_Operators](about_Operators.md)」を参照してください。

たとえば、という名前のエイリアスによって非表示になっているという名前の関数がある場合は、 `Map` `Map` 次のコマンドを使用して関数を実行します。

```powershell
&(Get-Command -Name Map -CommandType Function)
```

または

```powershell
&(dir Function:\map)
```

また、非表示のコマンドを変数に保存して、実行しやすくすることもできます。

たとえば、次のコマンドは、 `Map` 関数を変数に保存 `$myMap` し、演算子を使用し `Call` て実行します。

```powershell
$myMap = (Get-Command -Name map -CommandType function)
&($myMap)
```

### <a name="replaced-items"></a>置換された項目

"置き換えられた" 項目とは、アクセスできなくなった項目のことです。 モジュールまたはスナップインから同じ名前の項目をインポートすることによって、項目を置き換えることができます。

たとえば、 `Get-Map` セッションに関数を入力し、という名前の関数をインポートすると、 `Get-Map` 元の関数が置き換えられます。 現在のセッションでは取得できません。

変数とエイリアスは、呼び出し演算子または修飾名を使用して実行することはできないため、非表示にすることはできません。 モジュールまたはスナップインから変数とエイリアスをインポートすると、セッションの変数は同じ名前で置き換えられます。

### <a name="avoiding-name-conflicts"></a>名前の競合の回避

コマンド名の競合を管理する最良の方法は、それらを回避することです。 コマンドに名前を指定する場合は、一意の名前を使用します。 たとえば、コマンドの名詞に頭文字または会社名の頭字語を追加します。

また、PowerShell モジュールまたは別のセッションからセッションにコマンドをインポートする場合 `Prefix` は、 [インポートモジュール](xref:Microsoft.PowerShell.Core.Import-Module) のパラメーターを使用するか、

コマンドの名前の名詞にプレフィックスを追加するには[、PSSession](xref:Microsoft.PowerShell.Utility.Import-PSSession)コマンドレットをインポートします。

たとえば、次のコマンドは、モジュールをインポートする `Get-Date` `Set-Date` ときに PowerShell に付属するコマンドレットとコマンドレットとの競合を回避し `DateFunctions` ます。

```powershell
Import-Module -Name DateFunctions -Prefix ZZ
```

詳細については、以下を参照してください `Import-Module` `Import-PSSession` 。

## <a name="see-also"></a>関連項目

- [about_Path_Syntax](about_Path_Syntax.md)
- [about_Aliases](about_Aliases.md)
- [about_Functions](about_Functions.md)
- [Alias-Provider](../../Microsoft.PowerShell.Core/About/about_Alias_Provider.md)
- [Function-Provider](../../Microsoft.PowerShell.Core/About/about_Function_Provider.md)
- [Get-Command](xref:Microsoft.PowerShell.Core.Get-Command)
- [Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)
- [Import-PSSession](xref:Microsoft.PowerShell.Utility.Import-PSSession)

