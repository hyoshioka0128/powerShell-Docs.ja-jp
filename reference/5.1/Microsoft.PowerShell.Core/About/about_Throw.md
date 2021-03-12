---
description: 終了エラーを生成する Throw キーワードについて説明します。
Locale: en-US
ms.date: 12/01/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_throw?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Throw
ms.openlocfilehash: e573722154fff99363b26806064ec17c8903bfd8
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103194173"
---
# <a name="about-throw"></a>Throw の概要

## <a name="short-description"></a>概要

終了エラーを生成する Throw キーワードについて説明します。

## <a name="long-description"></a>詳細説明

Throw キーワードを使うと、終了エラーが発生します。 Throw キーワードを使用すると、コマンド、関数、またはスクリプトの処理を停止できます。

たとえば、If ステートメントのスクリプトブロックで Throw キーワードを使用して、条件に応答したり、try-catch ステートメントの Catch ブロックで Throw キーワードを使用したりすることができます。 また、パラメーター宣言で Throw キーワードを使用して、関数パラメーターを必須にすることもできます。

Throw キーワードを使用すると、ユーザーメッセージ文字列や、エラーの原因となったオブジェクトなど、任意のオブジェクトをスローできます。

## <a name="syntax"></a>SYNTAX

Throw キーワードの構文は次のとおりです。

```powershell
throw [<expression>]
```

Throw 構文の式は省略可能です。 Throw ステートメントが Catch ブロックに出現せず、式も含まれていない場合、例外が生成されます。

```powershell
C:\PS> throw

ScriptHalted
At line:1 char:6
+ throw <<<<
+ CategoryInfo          : OperationStopped: (:) [], RuntimeException
+ FullyQualifiedErrorId : ScriptHalted
```

Throw キーワードが式のない Catch ブロックで使用されている場合は、現在の RuntimeException を再度スローします。 詳細については、「about_Try_Catch_Finally」を参照してください。

## <a name="throwing-a-string"></a>文字列をスローする

Throw ステートメント内の省略可能な式は、次の例に示すように文字列にすることができます。

```powershell
C:\PS> throw "This is an error."

This is an error.
At line:1 char:6
+ throw <<<<  "This is an error."
+ CategoryInfo          : OperationStopped: (This is an error.:String) [], R
untimeException
+ FullyQualifiedErrorId : This is an error.
```

## <a name="throwing-other-objects"></a>他のオブジェクトのスロー

式は、次の例に示すように、PowerShell プロセスを表すオブジェクトをスローするオブジェクトにすることもできます。

```powershell
C:\PS> throw (get-process PowerShell)

System.Diagnostics.Process (PowerShell)
At line:1 char:6
+ throw <<<<  (get-process PowerShell)
+ CategoryInfo          : OperationStopped: (System.Diagnostics.Process (Pow
erShell):Process) [],
RuntimeException
+ FullyQualifiedErrorId : System.Diagnostics.Process (PowerShell)
```

$Error 自動変数の ErrorRecord オブジェクトの TargetObject プロパティを使用して、エラーを調べることができます。

```powershell
C:\PS> $error[0].targetobject

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
319      26    61016      70864   568     3.28   5548 PowerShell
```

ErrorRecord オブジェクトまたは Microsoft .NET Framework 例外をスローすることもできます。 Throw キーワードを使用して FormatException オブジェクトをスローする例を次に示します。

```powershell
C:\PS> $formatError = new-object system.formatexception

C:\PS> throw $formatError

One of the identified items was in an invalid format.
At line:1 char:6
+ throw <<<<  $formatError
+ CategoryInfo          : OperationStopped: (:) [], FormatException
+ FullyQualifiedErrorId : One of the identified items was in an invalid
format.
```

## <a name="resulting-error"></a>結果のエラー

Throw キーワードを使用すると、ErrorRecord オブジェクトを生成できます。 ErrorRecord オブジェクトの Exception プロパティには、RuntimeException オブジェクトが含まれています。 ErrorRecord オブジェクトと RuntimeException オブジェクトの残りの部分は、Throw キーワードによってスローされるオブジェクトによって異なります。

RunTimeException オブジェクトは ErrorRecord オブジェクトにラップされ、ErrorRecord オブジェクトは $Error 自動変数に自動的に保存されます。

## <a name="using-throw-to-create-a-mandatory-parameter"></a>THROW を使用して必須パラメーターを作成する

Throw キーワードを使用すると、関数パラメーターを必須にすることができます。

これは、Parameter キーワードの必須パラメーターを使用する代わりに使用します。 必須パラメーターを使用すると、ユーザーに対して必要なパラメーター値の入力を求めるメッセージが表示されます。 Throw キーワードを使用すると、コマンドが停止し、エラーレコードが表示されます。

たとえば、parameter 部分式の Throw キーワードを使用すると、Path パラメーターが関数の必須パラメーターになります。

この場合、Throw キーワードはメッセージ文字列をスローしますが、Path パラメーターが指定されていない場合、終了エラーを生成する Throw キーワードが存在することになります。 Throw の後の式は省略可能です。

```powershell
function Get-XMLFiles
{
  param ($path = $(throw "The Path parameter is required."))
  dir -path $path\*.xml -recurse |
    sort lastwritetime |
      ft lastwritetime, attributes, name  -auto
}
```

## <a name="see-also"></a>関連項目

[about_Break](about_Break.md)

[about_Continue](about_Continue.md)

[about_Scopes](about_Scopes.md)

[about_Trap](about_Trap.md)

[about_Try_Catch_Finally](about_Try_Catch_Finally.md)
