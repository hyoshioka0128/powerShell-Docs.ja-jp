---
description: PowerShell で使用できる値を変数に格納する方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/09/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_variables?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Variables
ms.openlocfilehash: e1eee80740d1f59ab9a96122c09ba81ef99d35cc
ms.sourcegitcommit: 768816a5c05cc2d07ffd84bed95b0499f4b49f2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2020
ms.locfileid: "94483029"
---
# <a name="about-variables"></a>変数について

## <a name="short-description"></a>簡単な説明

PowerShell で使用できる値を変数に格納する方法について説明します。

## <a name="long-description"></a>長い説明

すべての種類の値を PowerShell 変数に格納できます。 たとえば、コマンドの結果を格納したり、名前、パス、設定、値などのコマンドや式で使用される要素を格納したりすることができます。

変数は、値が格納されるメモリの単位です。 PowerShell では、変数は、、、などのドル記号 () で始まるテキスト文字列によって表され `$` `$a` `$process` `$my_var` ます。

変数名の大文字と小文字は区別されず、スペースや特殊文字を含めることができます。 ただし、特殊文字やスペースを含む変数名は使用するのが困難であり、避ける必要があります。 詳細については、「 [特殊文字を含む変数名](#variable-names-that-include-special-characters)」を参照してください。

PowerShell には、さまざまな種類の変数があります。

- ユーザーが作成した変数: ユーザーが作成した変数は、ユーザーによって作成および管理されます。 既定では、powershell コマンドラインで作成した変数は、PowerShell ウィンドウが開いている間だけ存在します。 PowerShell ウィンドウが閉じられると、変数は削除されます。 変数を保存するには、PowerShell プロファイルに変数を追加します。 スクリプトには、グローバル、スクリプト、またはローカルスコープの変数を作成することもできます。

- 自動変数: 自動変数は、PowerShell の状態を格納します。 これらの変数は PowerShell によって作成され、それらの値の精度を維持するために PowerShell が必要に応じて値を変更します。 ユーザーはこれらの変数の値を変更できません。 たとえば、変数には、 `$PSHOME` PowerShell インストールディレクトリへのパスが格納されます。

  詳細、一覧、および自動変数の説明については、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

- ユーザー設定変数: ユーザー設定を PowerShell に格納します。 これらの変数は PowerShell によって作成され、既定値が設定されます。 ユーザーは、これらの変数の値を変更できます。 たとえば、変数は、 `$MaximumHistoryCount` セッション履歴内のエントリの最大数を決定します。

  詳細、一覧、およびユーザー設定変数の説明については、「 [about_Preference_Variables](about_Preference_Variables.md)」を参照してください。

## <a name="working-with-variables"></a>変数の操作

新しい変数を作成するには、代入ステートメントを使用して変数に値を割り当てます。 変数を使用する前に宣言する必要はありません。 すべての変数の既定値は `$null` です。

PowerShell セッションのすべての変数の一覧を取得するには、「」と入力 `Get-Variable` します。 変数名は、 `$` 変数を参照するために使用される前のドル () 記号なしで表示されます。

次に例を示します。

```powershell
$MyVariable = 1, 2, 3

$Path = "C:\Windows\System32"
```

変数は、コマンドの結果を格納するために役立ちます。

次に例を示します。

```powershell
$Processes = Get-Process

$Today = (Get-Date).DateTime
```

変数の値を表示するには、変数名の前にドル記号 () を入力し `$` ます。

次に例を示します。

```powershell
$MyVariable
```

```Output
1
2
3
```

```powershell
$Today
```

```Output
Tuesday, September 3, 2019 09:46:46
```

変数の値を変更するには、変数に新しい値を割り当てます。

次の例では、変数の値を表示し、 `$MyVariable` 変数の値を変更して、新しい値を表示します。

```powershell
$MyVariable = 1, 2, 3
$MyVariable
```

```Output
1
2
3
```

```powershell
$MyVariable = "The green cat."
$MyVariable
```

```Output
The green cat.
```

変数の値を削除するには、コマンドレットを使用する `Clear-Variable` か、値をに変更し `$null` ます。

```powershell
Clear-Variable -Name MyVariable
```

```powershell
$MyVariable = $null
```

変数を削除するには、 [削除変数](xref:Microsoft.PowerShell.Utility.Remove-Variable) または [削除項目](xref:Microsoft.PowerShell.Management.Remove-Item)を使用します。

```powershell
Remove-Variable -Name MyVariable
```

```powershell
Remove-Item -Path Variable:\MyVariable
```

## <a name="types-of-variables"></a>変数の種類

整数、文字列、配列、ハッシュテーブルなど、任意の型のオブジェクトを変数に格納できます。 およびは、プロセス、サービス、イベントログ、およびコンピューターを表すオブジェクトです。

PowerShell 変数は、厳密に型指定されていません。つまり、特定の種類のオブジェクトに限定されないことを意味します。 1つの変数に、異なる種類のオブジェクトのコレクション (配列) を同時に含めることもできます。

変数のデータ型は、変数の値の .NET 型によって決まります。 変数のオブジェクト型を表示するには、 [Get Member](xref:Microsoft.PowerShell.Utility.Get-Member)を使用します。

次に例を示します。

```powershell
$a = 12                         # System.Int32
$a = "Word"                     # System.String
$a = 12, "Word"                 # array of System.Int32, System.String
$a = Get-ChildItem C:\Windows   # FileInfo and DirectoryInfo types
```

型属性とキャスト表記を使用して、変数に特定のオブジェクト型またはその型に変換できるオブジェクトのみを含めることができるようにすることができます。 別の型の値を割り当てようとすると、PowerShell は値をその型に変換しようとします。 型を変換できない場合、代入ステートメントは失敗します。

キャスト表記を使用するには、(代入ステートメントの左側の) 変数名の前に、角かっこで囲まれた型名を入力します。 次の例では、整数のみを含む変数、文字列のみを含むことができる変数、 `$number` `$words` および `$dates` **DateTime** オブジェクトのみを含むことができる変数を作成します。

```powershell
[int]$number = 8
$number = "12345"  # The string is converted to an integer.
$number = "Hello"
```

```Output
Cannot convert value "Hello" to type "System.Int32". Error: "Input string was
 not in a correct format."
At line:1 char:1
+ $number = "Hello"
+ ~~~~~~~~~~~~~~~~~
+ CategoryInfo          : MetadataError: (:) [],
    ArgumentTransformationMetadataException
+ FullyQualifiedErrorId : RuntimeException
```

```powershell
[string]$words = "Hello"
$words = 2       # The integer is converted to a string.
$words += 10     # The plus (+) sign concatenates the strings.
$words
```

```Output
210
```

```powershell
[datetime] $dates = "09/12/91"  # The string is converted to a DateTime object.
$dates
```

```Output
Thursday, September 12, 1991 00:00:00
```

```powershell
$dates = 10    # The integer is converted to a DateTime object.
$dates
```

```Output
Monday, January 1, 0001 00:00:00
```

## <a name="using-variables-in-commands-and-expressions"></a>コマンドおよび式での変数の使用

コマンドまたは式で変数を使用するには、変数名の前にドル () 記号を入力し `$` ます。

変数名とドル記号が引用符で囲まれていない場合、または二重引用符 () で囲まれている場合は、 `"` 変数の値がコマンドまたは式で使用されます。

変数名とドル記号を単一引用符 () で囲むと、 `'` 変数名が式で使用されます。

PowerShell での引用符の使用の詳細については、「 [about_Quoting_Rules](about_Quoting_Rules.md)」を参照してください。

この例では、変数の値を取得し `$PROFILE` ます。これは、powershell コンソールの powershell ユーザープロファイルファイルへのパスです。

```powershell
$PROFILE
```

```Output
C:\Users\User01\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

この例では、 **notepad.exe** で PowerShell プロファイルを開くことができる2つのコマンドを示しています。 二重引用符 () マークの例では `"` 、変数の値を使用します。

```powershell
notepad $PROFILE

notepad "$PROFILE"
```

次の例では、変数を `'` リテラルテキストとして扱う一重引用符 () を使用します。

```powershell
'$PROFILE'
```

```Output
$PROFILE
```

```powershell
'Use the $PROFILE variable.'
```

```Output
Use the $PROFILE variable.
```

## <a name="variable-names-that-include-special-characters"></a>特殊文字を含む変数名

変数名はドル ( `$` ) 記号で始まり、英数字と特殊文字を含めることができます。 変数名の長さは、使用可能なメモリによってのみ制限されます。

変数名には英数字とアンダースコア () 文字のみが含まれることがベストプラクティスです `_` 。 空白や特殊文字を含む変数名は使用するのが困難であり、避ける必要があります。

英数字の変数名には、次の文字を含めることができます。

- これらのカテゴリの Unicode 文字: **Lu** 、 **Ll** 、 **Lt** 、 **Lm** 、 **Lo** 、 **Nd** 。
- アンダースコア ( `_` ) 文字。
- 疑問符 ( `?` ) 文字。

次の一覧には、Unicode カテゴリの説明が含まれています。 詳細については、「 [「system.globalization.unicodecategory](/dotnet/api/system.globalization.unicodecategory)」を参照してください。

- **Lu** -UppercaseLetter
- **Ll** -LowercaseLetter
- **Lt** -TitlecaseLetter
- **Lm** -ModifierLetter
- **Lo** -その他の文字
- **Nd** -DecimalDigitNumber

スペースや特殊文字を含む変数名を作成または表示するには、変数名を中かっこ ( `{}` ) で囲みます。
中かっこでは、変数名の文字をリテラルとして解釈します。

特殊文字の変数名には、次の文字を含めることができます。

- 任意の Unicode 文字。ただし、次の例外があります。
  - 右中かっこ ( `}` ) 文字 (U + 007D)。
  - バックティック ( `` ` `` ) 文字 (U + 0060)。 バックティックは、リテラルとして扱われるように Unicode 文字をエスケープするために使用されます。

PowerShell には `$$` 、 `$?` `$^` 英数字と特殊文字を含む、、、などの変数が予約されてい `$_` ます。 詳細については、「 [about_Automatic_Variables](about_automatic_variables.md)」を参照してください。

たとえば、次のコマンドは、という名前の変数を作成し `save-items` ます。 変数名には `{}` ハイフン () の特殊文字が含まれているため、中かっこ () が必要です `-` 。

```powershell
${save-items} = "a", "b", "c"
${save-items}
```

```Output
a
b
c
```

環境変数によって表されるディレクトリ内の子項目を取得するコマンドを次に示し `ProgramFiles(x86)` ます。

```powershell
Get-ChildItem ${env:ProgramFiles(x86)}
```

中かっこを含む変数名を参照するには、変数名を中かっこで囲み、バックティック文字を使用して中かっこをエスケープします。 たとえば、という名前の変数を作成するには、次のように `this{value}is` 入力します。

```powershell
${this`{value`}is} = "This variable name uses braces and backticks."
${this`{value`}is}
```

```Output
This variable name uses braces and backticks.
```

## <a name="variables-and-scope"></a>変数とスコープ

既定では、変数は、作成されたスコープ内でのみ使用できます。

たとえば、関数で作成した変数は、関数内でのみ使用できます。 スクリプトで作成した変数は、スクリプト内でのみ使用できます。 スクリプトをドットソースで作成すると、変数は現在のスコープに追加されます。 詳細については、「 [about_Scopes](about_Scopes.md)」を参照してください。

スコープ修飾子を使用して、変数の既定のスコープを変更できます。 次の式では、という名前の変数が作成され `Computers` ます。 変数は、スクリプトまたは関数で作成された場合でも、グローバルスコープを持ちます。

```powershell
$Global:Computers = "Server01"
```

セッションが不足しているスクリプトまたはコマンドについては、 `Using` 呼び出し元のセッションスコープから変数の値を埋め込むためにスコープ修飾子が必要です。これにより、セッションコードが不足してしまうことがあります。

詳細については、「 [about_Remote_Variables](about_Remote_Variables.md)」を参照してください。

## <a name="saving-variables"></a>保存 (変数を)

作成した変数は、作成元のセッションでのみ使用できます。 セッションを閉じたときに失われます。

開始するすべての PowerShell セッションで変数を作成するには、PowerShell プロファイルに変数を追加します。

たとえば、すべての PowerShell セッションで変数の値を変更するには、 `$VerbosePreference` 次のコマンドを powershell プロファイルに追加します。

```powershell
$VerbosePreference = "Continue"
```

このコマンドを PowerShell プロファイルに追加するには、 `$PROFILE` **notepad.exe** などのテキストエディターでファイルを開きます。 PowerShell プロファイルの詳細については、「 [about_Profiles](about_Profiles.md)」を参照してください。

## <a name="the-variable-drive"></a>Variable: ドライブ

PowerShell 変数プロバイダーは、 `Variable:` ファイルシステムドライブのように見えるドライブを作成しますが、セッション内の変数とその値が含まれています。

ドライブに変更するには `Variable:` 、次のコマンドを使用します。

```powershell
Set-Location Variable:
```

ドライブ内の項目と変数の一覧を表示するに `Variable:` `Get-Item` は、コマンドレットまたはコマンドレットを使用し `Get-ChildItem` ます。

```powershell
Get-ChildItem Variable:
```

特定の変数の値を取得するには、ファイルシステム表記を使用して、ドライブ名と変数名を指定します。 たとえば、自動変数を取得するには、 `$PSCulture` 次のコマンドを使用します。

```powershell
Get-Item Variable:\PSCulture
```

```Output
Name                           Value
----                           -----
PSCulture                      en-US
```

ドライブと PowerShell 変数プロバイダーに関する詳細情報を表示するには `Variable:` 、次のように入力します。

```powershell
Get-Help Variable
```

## <a name="variable-syntax-with-provider-paths"></a>プロバイダーパスを使用した変数構文

プロバイダーパスをドル ( `$` ) 記号でプレフィックスし、 [IContentCmdletProvider](/dotnet/api/system.management.automation.provider.icontentcmdletprovider) インターフェイスを実装する任意のプロバイダーのコンテンツにアクセスすることができます。

次の組み込みの PowerShell プロバイダーでは、この表記がサポートされています。

- [about_Environment_Provider](about_Environment_Provider.md)
- [about_Variable_Provider](about_Variable_Provider.md)
- [about_Function_Provider](about_Function_Provider.md)
- [about_Alias_Provider](about_Alias_Provider.md)

## <a name="the-variable-cmdlets"></a>変数コマンドレット

PowerShell には、変数を管理するために設計された一連のコマンドレットが含まれています。

コマンドレットの一覧を表示するには、次のように入力します。

```powershell
Get-Command -Noun Variable
```

特定のコマンドレットのヘルプを表示するには、次のように入力します。

```powershell
Get-Help <cmdlet-name>
```

| コマンドレット名       | 説明                                |
| ---------------   | ------------------------------------------ |
| `Clear-Variable`  | 変数の値を削除します。           |
| `Get-Variable`    | 現在のコンソール内の変数を取得します。 |
| `New-Variable`    | 新しい変数を作成します。                    |
| `Remove-Variable` | 変数とその値を削除します。          |
| `Set-Variable`    | 変数の値を変更します。           |

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Environment_Variables](about_Environment_Variables.md)

[about_Preference_Variables](about_Preference_Variables.md)

[about_Profiles](about_Profiles.md)

[about_Quoting_Rules](about_Quoting_Rules.md)

[about_Scopes](about_Scopes.md)

[about_Remote_Variables](about_Remote_Variables.md)
