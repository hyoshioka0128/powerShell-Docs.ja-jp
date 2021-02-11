---
description: 、、およびブロックを使用して、終了エラーを処理する方法について説明し `Try` `Catch` `Finally` ます。
Locale: en-US
ms.date: 04/01/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_try_catch_finally?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Try_Catch_Finally
ms.openlocfilehash: c93fa69e3badd7777950a850dfe81b79f9197f68
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602000"
---
# <a name="about-try-catch-finally"></a>Try Catch Finally について

## <a name="short-description"></a>概要
、、およびブロックを使用して、終了エラーを処理する方法について説明し `Try` `Catch` `Finally` ます。

## <a name="long-description"></a>詳細説明

`Try`、 `Catch` 、およびブロックを使用して、 `Finally` スクリプトの終了エラーに応答したり、終了エラーを処理したりします。 ステートメントを使用して、 `Trap` スクリプトの終了エラーを処理することもできます。 詳細については、「 [about_Trap](about_Trap.md)」を参照してください。

終了エラーにより、ステートメントの実行が停止します。 PowerShell が何らかの方法で終了エラーを処理しない場合、PowerShell は、現在のパイプラインを使用した関数またはスクリプトの実行も停止します。 C などの他の言語では、 \# 終了エラーは例外と呼ばれます。

ブロックを使用して、 `Try` PowerShell でエラーを監視するスクリプトのセクションを定義します。 ブロック内でエラーが発生すると、 `Try` エラーが最初に自動変数に保存され `$Error` ます。 次に、PowerShell は、 `Catch` エラーを処理するブロックを検索します。 ステートメントに `Try` 一致するブロックがない場合 `Catch` 、PowerShell は引き続き、 `Catch` 親スコープ内の適切なブロックまたはステートメントを検索し `Trap` ます。 `Catch`ブロックが完了するか、適切な `Catch` ブロックまたは `Trap` ステートメントが見つからない場合は、 `Finally` ブロックが実行されます。 エラーを処理できない場合は、エラーストリームにエラーが書き込まれます。

ブロックには `Catch` 、エラーを追跡するコマンドや、想定されるスクリプトのフローを回復するためのコマンドを含めることができます。 ブロックは、 `Catch` キャッチするエラーの種類を指定できます。 ステートメントには、 `Try` `Catch` さまざまな種類のエラーに対応する複数のブロックを含めることができます。

ブロックは、スクリプトで不要になっ `Finally` たリソースを解放するために使用できます。

`Try`、、およびは、 `Catch` `Finally` `Try` `Catch` `Finally` C プログラミング言語で使用される、、およびの各キーワードに似て \# います。

### <a name="syntax"></a>SYNTAX

ステートメントには、 `Try` `Try` ブロック、0個以上 `Catch` のブロック、および0個または1個のブロックが含まれてい `Finally` ます。 `Try`ステートメントには、少なくとも1つの `Catch` ブロックまたは1つのブロックが必要 `Finally` です。

ブロックの構文を次に示し `Try` ます。

```powershell
try {<statement list>}
```

`Try`キーワードの後には、中かっこで囲まれたステートメントリストが続きます。 ステートメントリスト内のステートメントの実行中に終了エラーが発生した場合、スクリプトは `Try` ブロックから適切なブロックにエラーオブジェクトを渡し `Catch` ます。

ブロックの構文を次に示し `Catch` ます。

```powershell
catch [[<error type>][',' <error type>]*] {<statement list>}
```

エラーの種類は角かっこで囲まれています。 最も外側の角かっこは、要素が省略可能であることを示します。

`Catch`キーワードの後に、オプションのエラーの種類の指定リストとステートメントリストが続きます。 ブロックで終了エラーが発生した場合 `Try` 、PowerShell は適切なブロックを検索し `Catch` ます。 見つかった場合は、ブロック内のステートメント `Catch` が実行されます。

`Catch`ブロックでは、1つまたは複数のエラーの種類を指定できます。 エラーの種類は、Microsoft .NET Framework の例外、または .NET Framework の例外から派生した例外です。 ブロックは、 `Catch` 指定された .NET Framework exception クラス、または指定されたクラスから派生した任意のクラスのエラーを処理します。

ブロックで `Catch` エラーの種類が指定されている場合、その `Catch` ブロックはエラーの種類を処理します。 `Catch`ブロックでエラーの種類が指定されていない場合、ブロック `Catch` で発生したエラーがブロックによって処理さ `Try` れます。 ステートメントには `Try` `Catch` 、指定されたさまざまなエラーの種類に対して複数のブロックを含めることができます。

ブロックの構文を次に示し `Finally` ます。

```powershell
finally {<statement list>}
```

キーワードの後には、ステートメントがエラーなしで実行された `Finally` 場合 `Try` や、ステートメントでエラーがキャッチされた場合でも、スクリプトが実行されるたびに実行されるステートメントリストが続き `Catch` ます。

<kbd>CTRL</kbd>C キーを押すと + <kbd></kbd> 、パイプラインが停止することに注意してください。 パイプラインに送信されたオブジェクトは出力として表示されません。 したがって、表示するステートメント ("Finally ブロックが実行されている" など) を含めると、ブロックが実行され<kbd></kbd>た + 場合でも、CTRL<kbd>C</kbd>キーを押しても表示されません `Finally` 。

### <a name="catching-errors"></a>キャッチ (エラーを)

次のサンプルスクリプトは、ブロックを含むブロックを示してい `Try` `Catch` ます。

```powershell
try { NonsenseString }
catch { "An error occurred." }
```

キーワードは、 `Catch` ブロックまたは別のブロックの直後にある必要があり `Try` `Catch` ます。

PowerShell では、コマンドレットまたはその他の項目として "NonsenseString" は認識されません。
このスクリプトを実行すると、次の結果が返されます。

```powershell
An error occurred.
```

スクリプトが "NonsenseString" を検出すると、終了エラーが発生します。 ブロックは、 `Catch` ブロック内のステートメントリストを実行してエラーを処理します。

### <a name="using-multiple-catch-statements"></a>複数の CATCH ステートメントの使用

ステートメントには `Try` 、任意の数のブロックを含めることができ `Catch` ます。 たとえば、次のスクリプトにはを `Try` ダウンロードするブロックがあり、 `MyDoc.doc` 2 つのブロックが含まれてい `Catch` ます。

```powershell
try {
   $wc = new-object System.Net.WebClient
   $wc.DownloadFile("http://www.contoso.com/MyDoc.doc","c:\temp\MyDoc.doc")
}
catch [System.Net.WebException],[System.IO.IOException] {
    "Unable to download MyDoc.doc from http://www.contoso.com."
}
catch {
    "An error occurred that could not be resolved."
}

```

最初の `Catch` ブロックは、 **WebException** 型と **IOException** 型のエラーを処理します。 2番目の `Catch` ブロックでは、エラーの種類が指定されていません。 2つ目のブロックは、 `Catch` 発生した他のすべての終了エラーを処理します。

PowerShell は、継承によってエラーの種類と一致します。 ブロックは、 `Catch` 指定された .NET Framework exception クラス、または指定されたクラスから派生した任意のクラスのエラーを処理します。 次の例には、 `Catch` "コマンドが見つかりません" というエラーをキャッチするブロックが含まれています。

```powershell
catch [System.Management.Automation.CommandNotFoundException]
{"Inherited Exception" }
```

指定したエラーの種類 **CommandNotFoundException** は、 **System.Systemexception** 型から継承されます。 次の例では、コマンドが見つからないというエラーもキャッチします。

```powershell
catch [System.SystemException] {"Base Exception" }
```

この `Catch` ブロックは、"コマンドが見つかりません" エラーと、 **systemexception** 型から継承する他のエラーを処理します。

エラークラスとその派生クラスの1つを指定する場合は、 `Catch` 派生クラスのブロックを汎用クラスのブロックの前に配置し `Catch` ます。

### <a name="using-traps-in-a-try-catch"></a>Try Catch でのトラップの使用

ブロック内で定義されたを持つブロックで終了エラーが発生した場合 `Try` `Trap` `Try` 、一致するブロックがある場合でも、 `Catch` `Trap` ステートメントは制御を受け取ります。

が `Trap` より大きいブロックに存在 `Try` し、現在のスコープ内に一致するブロックがない場合、 `Catch` `Trap` 親スコープに一致するブロックがある場合でも、は制御を受け取ります `Catch` 。

### <a name="accessing-exception-information"></a>例外情報へのアクセス

ブロック内で `Catch` は、現在のエラーに `$_` はとも呼ばれるを使用してアクセスできます `$PSItem` 。 オブジェクトの型は **Errorrecord** です。

```powershell
try { NonsenseString }
catch {
  Write-Host "An error occurred:"
  Write-Host $_
}
```

このスクリプトを実行すると、次の結果が返されます。

```Output
An Error occurred:
The term 'NonsenseString' is not recognized as the name of a cmdlet, function,
script file, or operable program. Check the spelling of the name, or if a path
was included, verify that the path is correct and try again.
```

**ScriptStackTrace**、 **Exception**、 **errordetails** など、アクセスできるプロパティが追加されています。  たとえば、スクリプトを次のように変更したとします。

```powershell
try { NonsenseString }
catch {
  Write-Host "An error occurred:"
  Write-Host $_.ScriptStackTrace
}
```

結果は次のようになります。

```
An Error occurred:
at <ScriptBlock>, <No file>: line 2
```

### <a name="freeing-resources-by-using-finally"></a>FINALLY を使用したリソースの解放

スクリプトで使用されるリソースを解放するには、 `Finally` ブロックとブロックの後にブロックを追加し `Try` `Catch` ます。 ブロック `Finally` ステートメントは、 `Try` ブロックで終了エラーが発生したかどうかに関係なく実行されます。 PowerShell は、 `Finally` スクリプトが終了する前、または現在のブロックがスコープ外になる前に、ブロックを実行します。

`Finally` <kbd>CTRL</kbd>C を使用してスクリプトを停止した場合でも、ブロックは実行さ + <kbd></kbd>れます。 ブロックは、 `Finally` Exit キーワードがブロック内からスクリプトを停止した場合にも実行さ `Catch` れます。

### <a name="see-also"></a>関連項目

[about_Break](about_Break.md)

[about_Continue](about_Continue.md)

[about_Scopes](about_Scopes.md)

[about_Throw](about_Throw.md)

[about_Trap](about_Trap.md)

