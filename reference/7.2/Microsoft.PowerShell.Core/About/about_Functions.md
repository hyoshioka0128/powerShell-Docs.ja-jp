---
description: PowerShell で関数を作成して使用する方法について説明します。
Locale: en-US
ms.date: 02/27/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Functions
ms.openlocfilehash: d51c4d0bbf728bb23b4a5452d5192a262b722758
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600115"
---
# <a name="about-functions"></a>Functions について

## <a name="short-description"></a>簡単な説明

PowerShell で関数を作成して使用する方法について説明します。

## <a name="long-description"></a>長い説明

関数とは、割り当てた名前を持つ PowerShell ステートメントの一覧です。 関数を実行するときは、関数名を入力します。 リスト内のステートメントは、コマンドプロンプトで入力した場合と同じように実行されます。

関数は、次のような単純なものにすることができます。

```powershell
function Get-PowerShellProcess { Get-Process PowerShell }
```

関数は、コマンドレットまたはアプリケーションプログラムと同じように複雑にすることもできます。

コマンドレットと同様に、関数はパラメーターを持つことができます。 パラメーターには、名前付き、位置指定、スイッチ、または動的パラメーターを指定できます。 関数のパラメーターは、コマンドラインまたはパイプラインから読み取ることができます。

関数は、表示、変数への代入、または他の関数やコマンドレットに渡すことができる値を返すことができます。 キーワードを使用して戻り値を指定することもでき `return` ます。 キーワードは、 `return` 関数から返される他の出力には影響しません。 ただし、 `return` キーワードはその行の関数を終了します。 詳細については、「 [about_Return](about_Return.md)」を参照してください。

関数のステートメントリストには、キーワード、、およびを使用して、さまざまな種類のステートメントリストを含めることができ `Begin` `Process` `End` ます。 これらのステートメントでは、パイプラインからの入力の処理方法が異なります。

フィルターは、キーワードを使用する特別な種類の関数です `Filter` 。

関数は、コマンドレットのように機能することもできます。 プログラミングを使用せずに、コマンドレットと同じように動作する関数を作成でき `C#` ます。 詳細については、「 [about_Functions_Advanced](about_Functions_Advanced.md)」を参照してください。

## <a name="syntax"></a>構文

関数の構文を次に示します。

```
function [<scope:>]<name> [([type]$parameter1[,[type]$parameter2])]
{
  param([type]$parameter1 [,[type]$parameter2])
  dynamicparam {<statement list>}
  begin {<statement list>}
  process {<statement list>}
  end {<statement list>}
}
```

関数には、次の項目が含まれます。

- `Function`キーワード
- スコープ (省略可能)
- 選択した名前
- 任意の数の名前付きパラメーター (省略可能)
- 中かっこで囲まれた1つ以上の PowerShell コマンド `{}`

`Dynamicparam`関数のキーワードと動的パラメーターの詳細については、「 [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)」を参照してください。

## <a name="simple-functions"></a>単純な関数

関数は、役に立つために複雑である必要はありません。 最も単純な関数の形式は次のとおりです。

```
function <function-name> {statements}
```

たとえば、次の関数は、[管理者として実行] オプションを使用して PowerShell を起動します。

```powershell
function Start-PSAdmin {Start-Process PowerShell -Verb RunAs}
```

関数を使用するには、次のように入力します。 `Start-PSAdmin`

関数にステートメントを追加するには、各ステートメントを別の行に入力するか、セミコロンを使用し `;` てステートメントを区切ります。

たとえば、次の関数は、 `.jpg` 開始日以降に変更された、現在のユーザーのディレクトリにあるすべてのファイルを検索します。

```powershell
function Get-NewPix
{
  $start = Get-Date -Month 1 -Day 1 -Year 2010
  $allpix = Get-ChildItem -Path $env:UserProfile\*.jpg -Recurse
  $allpix | Where-Object {$_.LastWriteTime -gt $Start}
}
```

便利な小さい関数のツールボックスを作成できます。 これらの関数を PowerShell プロファイルに追加します。詳細については、このトピックの「 [about_Profiles](about_Profiles.md) 」を参照してください。

## <a name="function-names"></a>関数名

関数には任意の名前を割り当てることができますが、他のユーザーと共有する関数は、すべての PowerShell コマンドに対して設定されている名前付け規則に従う必要があります。

関数名は、動詞と名詞のペアで構成されている必要があります。動詞は、関数が実行するアクションを識別し、名詞は、コマンドレットがアクションを実行する項目を識別します。

関数は、すべての PowerShell コマンドに対して承認された標準動詞を使用する必要があります。 これらの動詞を使用すると、コマンド名を簡単かつ一貫性のあるものにし、ユーザーが理解しやすいものにすることができます。

標準の PowerShell 動詞の詳細については、「Microsoft Docs での承認された [動詞](/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands) 」を参照してください。

## <a name="functions-with-parameters"></a>パラメーターを持つ関数

パラメーターには、名前付きパラメーター、位置指定パラメーター、スイッチパラメーター、動的パラメーターなどの関数を使用できます。 関数の動的パラメーターの詳細については、「 [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)」を参照してください。

### <a name="named-parameters"></a>名前付きパラメーター

任意の数の名前付きパラメーターを定義できます。 このトピックで後述するように、名前付きパラメーターの既定値を含めることができます。

`Param`次のサンプル構文に示すように、キーワードを使用して、中かっこ内にパラメーターを定義できます。

```
function <name> {
  param ([type]$parameter1[,[type]$parameter2])
  <statement list>
}
```

`Param`次のサンプル構文に示すように、キーワードを使用せずに、中かっこの外側にパラメーターを定義することもできます。

```powershell
function <name> [([type]$parameter1[,[type]$parameter2])] {
  <statement list>
}
```

この代替構文の例を次に示します。

```powershell
Function Add-Numbers($one, $two) {
    $one + $two
}
```

最初の方法をお勧めしますが、この2つの方法に違いはありません。

関数を実行すると、パラメーターに指定した値が、パラメーター名を含む変数に代入されます。 この変数の値は、関数で使用できます。

次の例は、という関数を示して `Get-SmallFiles` います。 この関数には `$Size` パラメーターがあります。 関数は、パラメーターの値よりも小さいすべてのファイルを表示 `$Size` し、ディレクトリを除外します。

```powershell
function Get-SmallFiles {
  Param($Size)
  Get-ChildItem $HOME | Where-Object {
    $_.Length -lt $Size -and !$_.PSIsContainer
  }
}
```

関数では、 `$Size` パラメーターに定義されている名前である変数を使用できます。

この関数を使用するには、次のコマンドを入力します。

```powershell
Get-SmallFiles -Size 50
```

パラメーター名を指定せずに名前付きパラメーターの値を入力することもできます。
たとえば、次のコマンドでは、 **Size** パラメーターに名前を付けたコマンドと同じ結果が得られます。

```powershell
Get-SmallFiles 50
```

パラメーターの既定値を定義するには、次の例に示すように、等号とパラメーター名の後に値を入力し `Get-SmallFiles` ます。

```powershell
function Get-SmallFiles ($Size = 100) {
  Get-ChildItem $HOME | Where-Object {
    $_.Length -lt $Size -and !$_.PSIsContainer
  }
}
```

`Get-SmallFiles`値を指定せずにを入力すると、関数は100をに割り当て `$size` ます。 値を指定した場合、関数はその値を使用します。

必要に応じて、パラメーターの既定値を説明する簡単なヘルプ文字列を指定できます。これには、パラメーターの説明に **psdefaultvalue** 属性を追加し、 **Psdefaultvalue** の **help** プロパティを指定します。 関数の **Size** パラメーターの既定値 (100) を説明するヘルプ文字列を指定するには `Get-SmallFiles` 、次の例に示すように **psdefaultvalue** 属性を追加します。

```powershell
function Get-SmallFiles {
  param (
      [PSDefaultValue(Help = '100')]
      $Size = 100
  )
}
```

**Psdefaultvalue** 属性クラスの詳細については、「 [Psdefaultvalue 属性のメンバー](/dotnet/api/system.management.automation.psdefaultvalueattribute)」を参照してください。

### <a name="positional-parameters"></a>位置指定パラメーター

位置指定パラメーターは、パラメーター名のないパラメーターです。 PowerShell では、パラメーター値の順序を使用して、各パラメーター値を関数のパラメーターに関連付けます。

位置指定パラメーターを使用する場合は、関数名の後に1つ以上の値を入力します。 位置指定パラメーターの値は配列変数に代入され `$args` ます。
関数名の後の値は、配列内の最初の位置に割り当てられ `$args` `$args[0]` ます。

次の関数は、指定したファイル `Get-Extension` `.txt` 名にファイル名拡張子を追加します。

```powershell
function Get-Extension {
  $name = $args[0] + ".txt"
  $name
}
```

```powershell
Get-Extension myTextFile
```

```Output
myTextFile.txt
```

### <a name="switch-parameters"></a>スイッチパラメーター

スイッチは、値を必要としないパラメーターです。 代わりに、関数名の後にスイッチパラメーターの名前を入力します。

スイッチパラメーターを定義するには、 `[switch]` 次の例に示すように、パラメーター名の前に型を指定します。

```powershell
function Switch-Item {
  param ([switch]$on)
  if ($on) { "Switch on" }
  else { "Switch off" }
}
```

`On`関数名の後に switch パラメーターを入力すると、関数は "switch on" を表示します。 スイッチパラメーターを指定しない場合は、"Switch off" が表示されます。

```powershell
Switch-Item -on
```

```Output
Switch on
```

```powershell
Switch-Item
```

```Output
Switch off
```

次の例に示すように、関数を実行するときに、スイッチに **ブール** 値を割り当てることもできます。

```powershell
Switch-Item -on:$true
```

```Output
Switch on
```

```powershell
Switch-Item -on:$false
```

```Output
Switch off
```

## <a name="using-splatting-to-represent-command-parameters"></a>スプラッティングを使用してコマンドパラメーターを表す

スプラッティングを使用して、コマンドのパラメーターを表すことができます。 この機能は、Windows PowerShell 3.0 で導入されました。

この手法は、セッションでコマンドを呼び出す関数で使用します。 コマンドパラメーターを宣言または列挙したり、コマンドパラメーターが変更されたときに関数を変更したりする必要はありません。

次のサンプル関数は、 `Get-Command` コマンドレットを呼び出します。 このコマンドは、を使用して `@Args` のパラメーターを表し `Get-Command` ます。

```powershell
function Get-MyCommand { Get-Command @Args }
```

関数を呼び出すときに、のすべてのパラメーターを使用でき `Get-Command` `Get-MyCommand` ます。 パラメーターとパラメーター値は、を使用してコマンドに渡され `@Args` ます。

```powershell
Get-MyCommand -Name Get-ChildItem
```

```Output
CommandType     Name                ModuleName
-----------     ----                ----------
Cmdlet          Get-ChildItem       Microsoft.PowerShell.Management
```

この `@Args` 機能は `$Args` 自動パラメーターを使用します。これは、宣言されていないコマンドレットパラメーターと残りの引数の値を表します。

スプラッティングの詳細については、「 [about_Splatting](about_Splatting.md)」を参照してください。

## <a name="piping-objects-to-functions"></a>オブジェクトを関数にパイプする

すべての関数は、パイプラインからの入力を受け取ることができます。 、、およびキーワードを使用して、関数がパイプラインからの入力を処理する方法を制御でき `Begin` `Process` `End` ます。 次のサンプル構文は、3つのキーワードを示しています。

```
function <name> {
  begin {<statement list>}
  process {<statement list>}
  end {<statement list>}
}
```

`Begin`ステートメントリストは、関数の先頭で1回だけ実行されます。

> [!IMPORTANT]
> 関数で、、またはブロックが定義されている場合は、 `Begin` `Process` `End` すべてのコードがそのブロック内に存在する必要があります。 ブロックの *いずれか* が定義されている場合、ブロックの外側ではコードが認識されません。

ステートメントの一覧は、 `Process` パイプライン内の各オブジェクトに対して1回実行されます。
`Process`ブロックが実行されている間、各パイプラインオブジェクトは、一度に1つのパイプラインオブジェクトに対して自動変数に割り当てられ `$_` ます。

関数がパイプライン内のすべてのオブジェクトを受け取ると、 `End` ステートメントの一覧が1回だけ実行されます。 、、またはキーワードが使用されていない場合は、 `Begin` `Process` `End` すべてのステートメントがステートメントリストと同様に扱われ `End` ます。

次の関数では、キーワードを使用し `Process` ます。 この関数は、パイプラインの例を表示します。

```powershell
function Get-Pipeline
{
  process {"The value is: $_"}
}
```

この関数を示すには、次の例に示すように、コンマで区切られた数値のリストを入力します。

```powershell
1,2,4 | Get-Pipeline
```

```Output
The value is: 1
The value is: 2
The value is: 4
```

パイプラインで関数を使用すると、関数にパイプされたオブジェクトが自動変数に割り当てられ `$input` ます。 関数は、 `Begin` オブジェクトがパイプラインから取得される前に、キーワードを使用してステートメントを実行します。 関数は、 `End` すべてのオブジェクトがパイプラインから受信された後、キーワードを使用してステートメントを実行します。

次の例は、 `$input` キーワードとキーワードを使用した自動変数を示して `Begin` `End` います。

```powershell
function Get-PipelineBeginEnd
{
  begin {"Begin: The input is $input"}
  end {"End:   The input is $input" }
}
```

パイプラインを使用してこの関数を実行すると、次の結果が表示されます。

```powershell
1,2,4 | Get-PipelineBeginEnd
```

```Output
Begin: The input is
End:   The input is 1 2 4
```

ステートメントの実行時に、この `Begin` 関数にはパイプラインからの入力がありません。 ステートメントは、 `End` 関数の値がの後に実行されます。

関数にキーワードがある場合 `Process` 、内の各オブジェクト `$input` はから削除され、 `$input` に割り当てられ `$_` ます。 次の例には、ステートメントの一覧があり `Process` ます。

```powershell
function Get-PipelineInput
{
  process {"Processing:  $_ " }
  end {"End:   The input is: $input" }
}
```

この例では、関数にパイプ処理された各オブジェクトが、ステートメントリストに送信され `Process` ます。 ステートメントは、 `Process` 各オブジェクトに対して一度に1つずつ実行されます。 `$input`関数がキーワードに達した場合、自動変数は空になり `End` ます。

```powershell
1,2,4 | Get-PipelineInput
```

```Output
Processing:  1
Processing:  2
Processing:  4
End:   The input is:
```

詳細については、「[列挙子の使用](about_Automatic_Variables.md#using-enumerators)」を参照してください。

## <a name="filters"></a>フィルタ

フィルターは、パイプライン内の各オブジェクトに対して実行される関数の一種です。 フィルターは、ブロック内のすべてのステートメントを含む関数に似て `Process` います。

フィルターの構文は次のとおりです。

```
filter [<scope:>]<name> {<statement list>}
```

次のフィルターは、パイプラインからログエントリを受け取り、エントリ全体を表示するか、エントリのメッセージ部分のみを表示します。

```powershell
filter Get-ErrorLog ([switch]$message)
{
  if ($message) { Out-Host -InputObject $_.Message }
  else { $_ }
}
```

## <a name="function-scope"></a>関数のスコープ

関数は、それが作成されたスコープ内に存在します。

関数がスクリプトの一部である場合、関数はそのスクリプト内のステートメントで使用できます。 既定では、スクリプト内の関数はコマンドプロンプトでは使用できません。

関数のスコープを指定できます。 たとえば、次の例では、関数がグローバルスコープに追加されます。

```powershell
function global:Get-DependentSvs {
  Get-Service | Where-Object {$_.DependentServices}
}
```

関数がグローバルスコープ内にある場合は、スクリプト、関数、およびコマンドラインで関数を使用できます。

関数は、通常、スコープを作成します。 変数などの関数で作成されたアイテムは、関数スコープにのみ存在します。

PowerShell のスコープの詳細については、「 [about_Scopes](about_Scopes.md)」を参照してください。

## <a name="finding-and-managing-functions-using-the-function-drive"></a>関数を使用した関数の検索と管理: ドライブ

PowerShell のすべての関数とフィルターは、自動的にドライブに格納され `Function:` ます。 このドライブは、PowerShell **関数** プロバイダーによって公開されています。

ドライブを参照するとき `Function:` は、コンピューターのまたはドライブを参照する場合と同様に、 **関数** の後にコロンを入力し `C` `D` ます。

次のコマンドは、PowerShell の現在のセッションのすべての関数を表示します。

```powershell
Get-ChildItem function:
```

関数内のコマンドは、関数の definition プロパティにスクリプトブロックとして格納されます。 たとえば、PowerShell に付属する Help 関数のコマンドを表示するには、次のように入力します。

```powershell
(Get-ChildItem function:help).Definition
```

次の構文を使用することもできます。

```powershell
$function:help
```

ドライブの詳細については `Function:` 、 **関数** プロバイダーのヘルプトピックを参照してください。 「`Get-Help Function`.

## <a name="reusing-functions-in-new-sessions"></a>新しいセッションでの関数の再利用

PowerShell コマンドプロンプトで関数を入力すると、関数が現在のセッションの一部になります。 セッションが終了するまで使用できます。

すべての PowerShell セッションで関数を使用するには、関数を PowerShell プロファイルに追加します。 プロファイルの詳細については、「[about_Profiles](about_Profiles.md)」を参照してください。

また、関数を PowerShell スクリプトファイルに保存することもできます。 テキストファイルに関数を入力し、ファイル名拡張子を付けてファイルを保存し `.ps1` ます。

## <a name="writing-help-for-functions"></a>関数のヘルプの記述

`Get-Help`コマンドレットは、コマンドレット、プロバイダー、およびスクリプトに加えて、関数のヘルプを取得します。 関数のヘルプを表示するには、 `Get-Help` 関数名の後に「」と入力します。

たとえば、関数のヘルプを表示するには、次のように `Get-MyDisks` 入力します。

```powershell
Get-Help Get-MyDisks
```

関数のヘルプは、次の2つの方法のいずれかを使用して記述できます。

- 関数のヘルプの Comment-Based

  コメントに特殊なキーワードを使用して、ヘルプトピックを作成します。 関数のコメントベースのヘルプを作成するには、関数本体の先頭または末尾にコメントを配置するか、関数キーワードの前の行に記述する必要があります。 コメントベースのヘルプの詳細については、「 [about_Comment_Based_Help](about_Comment_Based_Help.md)」を参照してください。

- 関数のヘルプの XML-Based

  コマンドレットに対して通常作成される型など、XML ベースのヘルプトピックを作成します。 ヘルプトピックを複数の言語にローカライズする場合は、XML ベースのヘルプが必要です。

  関数を XML ベースのヘルプトピックに関連付けるには、 `.ExternalHelp` コメントベースのヘルプキーワードを使用します。 このキーワードがない場合、は `Get-Help` 関数のヘルプトピックを見つけることができず、関数に対してを呼び出すと、 `Get-Help` 自動生成されたヘルプだけが返されます。

  キーワードの詳細については `ExternalHelp` 、「 [about_Comment_Based_Help](about_Comment_Based_Help.md)」を参照してください。 XML ベースのヘルプの詳細については、「 [How To Write Cmdlet help](/powershell/scripting/developer/help/writing-help-for-windows-powershell-cmdlets)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Comment_Based_Help](about_Comment_Based_Help.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

[about_Functions_OutputTypeAttribute](about_Functions_OutputTypeAttribute.md)

[about_Parameters](about_Parameters.md)

[about_Profiles](about_Profiles.md)

[about_Scopes](about_Scopes.md)

[about_Script_Blocks](about_Script_Blocks.md)

[about_Function_provider](about_Function_provider.md)
