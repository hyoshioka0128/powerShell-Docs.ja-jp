---
ms.date: 06/09/2020
ms.topic: reference
title: コメント ベースのヘルプのキーワード
description: コメント ベースのヘルプのキーワード
ms.openlocfilehash: d87dde8700813767f6c09cfce70ed06c7964ebc7
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645474"
---
# <a name="comment-based-help-keywords"></a>コメント ベースのヘルプのキーワード

このトピックでは、コメントベースのヘルプのキーワードの一覧とその説明を示します。

## <a name="keywords-in-comment-based-help"></a>Comment-Based ヘルプのキーワード

有効なコメントベースのヘルプキーワードを次に示します。 これらの一覧は、通常、ヘルプトピックに表示される順序で、使用目的と共に表示されます。 これらのキーワードは、コメントベースのヘルプ内で任意の順序で表示でき、大文字と小文字は区別されません。

キーワードは、 `.EXTERNALHELP` 他のすべてのコメントベースのヘルプキーワードよりも優先されることに注意してください。
が指定されている場合、 `.EXTERNALHELP` キーワードの値に一致するヘルプファイルが見つからない場合でも、このコマンドレット [では](/dotnet/api/Microsoft.PowerShell.Commands.gethelpcommand) 、コメントベースのヘルプは表示されません。

## <a name="synopsis"></a>.概要

関数またはスクリプトの簡単な説明。 このキーワードは、各トピックで1回だけ使用できます。

## <a name="description"></a>.記述

関数またはスクリプトの詳細な説明。 このキーワードは、各トピックで1回だけ使用できます。

## <a name="parameter-parameter-name"></a>.引き \<Parameter-Name>

パラメーターの説明。 `.PARAMETER`関数またはスクリプトの各パラメーターにキーワードを含めることができます。

キーワードは、 `.PARAMETER` コメントブロック内で任意の順序で表示できますが、パラメーターがステートメントまたは関数の宣言に表示される順序によって、 `Param` ヘルプトピックでパラメーターが表示される順序が決まります。 ヘルプトピックのパラメーターの順序を変更するに `Param` は、ステートメントまたは関数の宣言でパラメーターの順序を変更します。

パラメーターの説明を指定するには、パラメーター変数名の直前のステートメントにコメントを配置し `Param` ます。 ステートメントコメントとキーワードの両方を使用する場合 `Param` `.PARAMETER` は、キーワードに関連付けられている説明 `.PARAMETER` が使用され、 `Param` ステートメントコメントは無視されます。

## <a name="example"></a>.よう

関数またはスクリプトを使用するサンプルコマンド。必要に応じて、サンプル出力と説明が続きます。 各例について、このキーワードを繰り返します。

## <a name="inputs"></a>.INPUTACCEL

関数またはスクリプトにパイプできるオブジェクトの Microsoft .NET Framework 型。 入力オブジェクトの説明を含めることもできます。

## <a name="outputs"></a>.出力

コマンドレットが返すオブジェクトの .NET Framework 型。 返されたオブジェクトの説明を含めることもできます。

## <a name="notes"></a>.注記

関数またはスクリプトに関する追加情報。

## <a name="link"></a>.リンク

関連するトピックの名前。 関連する各トピックについて、このキーワードを繰り返します。 このコンテンツは、ヘルプトピックの「関連リンク」セクションに表示されます。

キーワードの内容には、 `.LINK` 同じヘルプトピックのオンラインバージョンの Uniform Resource Identifier (URI) を含めることもできます。 のパラメーターを使用すると、オンラインバージョンが開き `Online` `Get-Help` ます。 URI は "http" または "https" で始まる必要があります。

## <a name="component"></a>.成分

関数またはスクリプトが使用する、またはそれに関連するテクノロジまたは機能の名前。
の **Component** パラメーターで `Get-Help` は、この値を使用して、によって返される検索結果をフィルター処理し `Get-Help` ます。

## <a name="role"></a>.果たす

ヘルプトピックのユーザーロールの名前です。 の **Role** パラメーターで `Get-Help` は、この値を使用して、によって返される検索結果をフィルター処理し `Get-Help` ます。

## <a name="functionality"></a>.機

関数の使用目的を説明するキーワード。 の **機能** パラメーターは、 `Get-Help` この値を使用して、によって返される検索結果をフィルター処理し `Get-Help` ます。

## <a name="forwardhelptargetname-command-name"></a>.FORWARDHELPTARGETNAME \<Command-Name>

指定されたコマンドのヘルプトピックにリダイレクトします。 関数、スクリプト、コマンドレット、またはプロバイダーのヘルプトピックを含む、任意のヘルプトピックにユーザーをリダイレクトできます。

## <a name="forwardhelpcategory-category"></a>.FORWARDHELPCATEGORY \<Category>

の項目のヘルプカテゴリを指定し `.FORWARDHELPTARGETNAME` ます。 同じ名前のコマンドが存在する場合に競合を回避するには、このキーワードを使用します。

有効な値は次のとおりです。

- エイリアス
- コマンドレット
- HelpFile
- 機能
- プロバイダー
- 全般
- よく寄せられる質問
- 用語集
- ScriptCommand
- ExternalScript
- フィルター
- すべて

## <a name="remotehelprunspace-pssession-variable"></a>.REMOTEHELPRUNSPACE 空間 \<PSSession-variable>

ヘルプトピックを含むセッションを指定します。 PSSession を含む変数を入力します。 このキーワードは、エクスポートされ `Export-PSSession` たコマンドのヘルプトピックを検索するために、コマンドレットによって使用されます。

## <a name="externalhelp-xml-help-file"></a>..EXTERNALHELP \<XML Help File>

スクリプトまたは関数の XML ベースのヘルプファイルのパスと名前の両方またはいずれかを指定します。

キーワードは、 `.EXTERNALHELP` XML ベースのファイル内のスクリプトまたは関数のヘルプを取得するように [、コマンド](/dotnet/api/Microsoft.PowerShell.Commands.gethelpcommand) レットに指示します。 キーワードは、 `.EXTERNALHELP` スクリプトまたは関数に XML ベースのヘルプファイルを使用する場合に必要です。 使用しない場合、 `Get-Help` 関数またはスクリプトのヘルプファイルは検索されません。

キーワードは、 `.EXTERNALHELP` 他のすべてのコメントベースのヘルプキーワードよりも優先されます。 が指定されている場合、 `.EXTERNALHELP` キーワードの値に一致するヘルプファイルが見つからない場合でも、このコマンドレット [では](/dotnet/api/Microsoft.PowerShell.Commands.gethelpcommand) 、コメントベースのヘルプは表示されません。

関数がスクリプトモジュールによってエクスポートされる場合、の値は `.EXTERNALHELP` パスを含まないファイル名にする必要があります。 `Get-Help` は、モジュールディレクトリのロケール固有のサブディレクトリでファイルを検索します。 ファイル名には要件はありませんが、ファイル名の形式としてを使用することをお勧め `<ScriptModule>.psm1-help.xml` します。

関数がモジュールに関連付けられていない場合は、キーワードの値にパスとファイル名を含め `.EXTERNALHELP` ます。 XML ファイルへの指定したパスに UI カルチャ固有のサブディレクトリが含まれている場合、は、 `Get-Help` すべての xml ベースのヘルプトピックと同様に、Windows 用に設定された言語フォールバック標準に従って、スクリプトまたは関数の名前を持つ xml ファイルを再帰的に検索します。

コマンドレットヘルプの XML ベースのヘルプファイル形式の詳細については、「 [Windows PowerShell コマンドレットヘルプの記述](./writing-help-for-windows-powershell-cmdlets.md)」を参照してください。
