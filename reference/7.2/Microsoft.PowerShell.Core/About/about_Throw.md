---
description: 終了エラーを生成する Throw キーワードについて説明します。
Locale: en-US
ms.date: 12/01/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_throw?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Throw
ms.openlocfilehash: 2984e0a9e5f470594dd1e5987b69b92f91ce4913
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603248"
---
# <a name="about-throw"></a>Throw の概要

## <a name="short-description"></a>簡単な説明
終了エラーを生成する Throw キーワードについて説明します。

## <a name="long-description"></a>長い説明

Throw キーワードを使うと、終了エラーが発生します。 Throw キーワードを使用すると、コマンド、関数、またはスクリプトの処理を停止できます。

たとえば、If ステートメントのスクリプトブロックで Throw キーワードを使用して、条件に応答したり、try-catch ステートメントの Catch ブロックで Throw キーワードを使用したりすることができます。 また、パラメーター宣言で Throw キーワードを使用して、関数パラメーターを必須にすることもできます。

Throw キーワードを使用すると、ユーザーメッセージ文字列や、エラーの原因となったオブジェクトなど、任意のオブジェクトをスローできます。

## <a name="syntax"></a>構文

Throw キーワードの構文は次のとおりです。

```powershell
throw [<expression>]
```

Throw 構文の式は省略可能です。 Throw ステートメントが Catch ブロックに出現せず、式も含まれていない場合、例外が生成されます。

```powershell
C:\PS> throw

Exception: ScriptHalted
```

Throw キーワードが式のない Catch ブロックで使用されている場合は、現在の RuntimeException を再度スローします。 詳細については、「about_Try_Catch_Finally」を参照してください。

## <a name="throwing-a-string"></a>文字列をスローする

Throw ステートメント内の省略可能な式は、次の例に示すように文字列にすることができます。

```powershell
C:\PS> throw "This is an error."

Exception: This is an error.
```

## <a name="throwing-other-objects"></a>他のオブジェクトのスロー

式は、次の例に示すように、PowerShell プロセスを表すオブジェクトをスローするオブジェクトにすることもできます。

```powershell
C:\PS> throw (get-process Pwsh)

Exception: System.Diagnostics.Process (pwsh) System.Diagnostics.Process (pwsh) System.Diagnostics.Process (pwsh)
```

$Error 自動変数の ErrorRecord オブジェクトの TargetObject プロパティを使用して、エラーを調べることができます。

```powershell
C:\PS> $error[0].targetobject

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
    125   174.44     229.57      23.61    1548   2 pwsh
     63    44.07      81.95       1.75    1732   2 pwsh
     63    43.32      77.65       1.48    9092   2 pwsh
```

ErrorRecord オブジェクトまたは .NET 例外をスローすることもできます。 Throw キーワードを使用して FormatException オブジェクトをスローする例を次に示します。

```powershell
C:\PS> $formatError = new-object system.formatexception

C:\PS> throw $formatError

OperationStopped: One of the identified items was in an invalid format.
```

## <a name="the-resulting-error"></a>生成されたエラー

Throw キーワードを使用すると、ErrorRecord オブジェクトを生成できます。 ErrorRecord オブジェクトの Exception プロパティには、RuntimeException オブジェクトが含まれています。 ErrorRecord オブジェクトと RuntimeException オブジェクトの残りの部分は、Throw キーワードによってスローされるオブジェクトによって異なります。

RunTimeException オブジェクトは ErrorRecord オブジェクトにラップされ、ErrorRecord オブジェクトは $Error 自動変数に自動的に保存されます。

## <a name="using-throw-to-create-a-mandatory-parameter"></a>Throw を使用して必須パラメーターを作成する

以前のバージョンの PowerShell とは異なり、パラメーターの検証に Throw キーワードを使用しないでください。 正しい方法については [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md) を参照してください。

## <a name="see-also"></a>関連項目

- [about_Break](about_Break.md)
- [about_Continue](about_Continue.md)
- [about_Scopes](about_Scopes.md)
- [about_Trap](about_Trap.md)
- [about_Try_Catch_Finally](about_Try_Catch_Finally.md)
