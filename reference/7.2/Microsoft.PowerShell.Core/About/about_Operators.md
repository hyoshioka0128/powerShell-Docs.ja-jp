---
description: PowerShell でサポートされている演算子について説明します。
Locale: en-US
ms.date: 10/28/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Operators
ms.openlocfilehash: c3884c7fc6c45e52ac9bccbe6c016908c242b8ef
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "99599699"
---
# <a name="about-operators"></a>演算子について

## <a name="short-description"></a>簡単な説明
PowerShell でサポートされている演算子について説明します。

## <a name="long-description"></a>長い説明

演算子は、コマンドまたは式で使用できる言語要素です。
PowerShell では、値の操作に役立ついくつかの種類の演算子がサポートされています。

### <a name="arithmetic-operators"></a>算術演算子

算術演算子 (、、、、) を使用して、 `+` `-` `*` `/` `%` コマンドまたは式の値を計算します。 これらの演算子を使用すると、値の加算、減算、乗算、除算を行ったり、除算演算の剰余 (剰余) を計算したりすることができます。

加算演算子は要素を連結します。 乗算演算子は、各要素の指定された数のコピーを返します。 算術演算子は、、、、、およびの各配列を実装するすべての .net 型で使用できます `Int` `String` `DateTime` `Hashtable` 。

ビットごとの演算子 ( `-band` 、、 `-bor` 、、 `-bxor` `-bnot` `-shl` 、 `-shr` ) は、値のビットパターンを操作します。

詳細については、「 [about_Arithmetic_Operators](about_Arithmetic_Operators.md)」を参照してください。

### <a name="assignment-operators"></a>代入演算子

代入演算子 ( `=` 、、 `+=` 、 `-=` 、 `*=` 、) を使用して `/=` `%=` 、変数に値を代入、変更、または追加します。 算術演算子を代入と組み合わせて、算術演算の結果を変数に代入することができます。

詳細については、「 [about_Assignment_Operators](about_Assignment_Operators.md)」を参照してください。

### <a name="comparison-operators"></a>比較演算子

比較演算子 ( `-eq` 、、 `-ne` 、 `-gt` 、 `-lt` `-le` 、) を使用して、 `-ge` 値とテスト条件を比較します。 たとえば、2つの文字列値を比較して、両者が等しいかどうかを判断できます。

比較演算子には、テキストのパターンを検索または置換する演算子も含まれます。 ( `-match` , `-notmatch` ,) 演算子は、 `-replace` 正規表現を使用し、( `-like` , `-notlike` ) ワイルドカードを使用し `*` ます。

コンテインメント比較演算子は、テスト値が参照セット (、、、) に表示されるかどうかを決定 `-in` `-notin` `-contains` `-notcontains` します。

型の比較演算子 ( `-is` , `-isnot` ) は、オブジェクトが特定の型であるかどうかを判断します。

詳細については、「 [about_Comparison_Operators](about_Comparison_Operators.md)」を参照してください。

### <a name="logical-operators"></a>論理演算子

論理演算子 (、、、、) を使用して、 `-and` `-or` `-xor` `-not` `!` 条件付きステートメントを1つの複雑な条件付きステートメントに接続します。 たとえば、論理演算子を使用すると、 `-and` 2 つの異なる条件を持つオブジェクトフィルターを作成できます。

詳細については、「 [about_Logical_Operators](about_logical_operators.md)」を参照してください。

### <a name="redirection-operators"></a>リダイレクト演算子

`>` `>>` `2>` `2>>` `2>&1` コマンドまたは式の出力をテキストファイルに送信するには、リダイレクト演算子 (、、、、および) を使用します。 リダイレクト演算子はコマンドレットのように機能し `Out-File` ますが (パラメーターなし)、指定されたファイルにエラー出力をリダイレクトすることもできます。 コマンドレットを使用して、 `Tee-Object` 出力をリダイレクトすることもできます。

詳細については、「」を参照してください [about_Redirection](about_Redirection.md)

### <a name="split-and-join-operators"></a>Split 演算子と Join 演算子

`-split`演算子と演算子は、サブ `-join` ストリングを分割して結合します。 演算子は、 `-split` 文字列を部分文字列に分割します。 演算子は、 `-join` 複数の文字列を連結して1つの文字列にします。

詳細については、「 [about_Split](about_Split.md) 」および「 [about_Join](about_Join.md)」を参照してください。

### <a name="type-operators"></a>型演算子

`-is` `-isnot` `-as` オブジェクトの .NET Framework 型を検索または変更するには、型演算子 (、、) を使用します。

詳細については、「 [about_Type_Operators](about_Type_Operators.md)」を参照してください。

### <a name="unary-operators"></a>単項演算子

単項演算子を使用して、変数またはオブジェクトのプロパティをインクリメントまたはデクリメントし、整数を正または負の数値に設定します。 たとえば、変数をからにインクリメントするには `$a` `9` `10` 、「」と入力し `$a++` ます。

### <a name="special-operators"></a>特別な演算子

特別な演算子には、他のどのオペレーターグループにも適合しない特定のユースケースがあります。 たとえば、特殊な演算子を使用すると、コマンドの実行、値のデータ型の変更、または配列からの要素の取得を行うことができます。

#### <a name="grouping-operator--"></a>グループ化演算子 `( )`

他の言語と同様に、は `(...)` 式の演算子の優先順位をオーバーライドします。 例: `(1 + 2) / 3`

ただし、PowerShell には追加の動作があります。

- `(...)`_コマンド_ からの出力を式に参加させることができます。 次に例を示します。

  ```powershell
  PS> (Get-Item *.txt).Count -gt 10
  True
  ```

- パイプラインの最初のセグメントとして使用する場合は、コマンドまたは式をかっこで囲むことで、式の結果を _列挙_ することができます。 かっこによって _コマンド_ がラップされている場合は、パイプラインを介して結果が送信される前に、 _メモリに収集_ されたすべての出力を使用して完了します。

#### <a name="subexpression-operator--"></a>部分式演算子 `$( )`

1つ以上のステートメントの結果を返します。 1つの結果に対して、はスカラーを返します。 複数の結果に対して、は配列を返します。 別の式の中で式を使用する場合に使用します。 たとえば、コマンドの結果を文字列式に埋め込むには、を使用します。

```powershell
PS> "Today is $(Get-Date)"
Today is 12/02/2019 13:15:20

PS> "Folder list: $((dir c:\ -dir).Name -join ', ')"
Folder list: Program Files, Program Files (x86), Users, Windows
```

#### <a name="array-subexpression-operator--"></a>配列の部分式演算子 `@( )`

1つ以上のステートメントの結果を配列として返します。 項目が1つしかない場合、配列にはメンバーが1つだけ含まれます。

```powershell
@(Get-CimInstance win32_logicalDisk)
```

#### <a name="hash-table-literal-syntax-"></a>ハッシュテーブルリテラルの構文 `@{}`

配列の部分式と同様に、この構文はハッシュテーブルを宣言するために使用されます。
詳細については、「 [about_Hash_Tables](about_Hash_Tables.md)」を参照してください。

#### <a name="call-operator-"></a>Call 演算子 `&`

コマンド、スクリプト、またはスクリプトブロックを実行します。 呼び出し演算子 ("呼び出し演算子" とも呼ばれます) を使用すると、変数に格納され、文字列またはスクリプトブロックによって表されるコマンドを実行できます。 呼び出し演算子は、子スコープで実行されます。 スコープの詳細については、「 [about_Scopes](about_Scopes.md)」を参照してください。

この例では、コマンドを文字列に格納し、call 演算子を使用して実行します。

```
PS> $c = "get-executionpolicy"
PS> $c
get-executionpolicy
PS> & $c
AllSigned
```

呼び出し演算子は、文字列を解析しません。 これは、call 演算子を使用するときに、文字列内のコマンドパラメーターを使用できないことを意味します。

```
PS> $c = "Get-Service -Name Spooler"
PS> $c
Get-Service -Name Spooler
PS> & $c
& : The term 'Get-Service -Name Spooler' is not recognized as the name of a
cmdlet, function, script file, or operable program. Check the spelling of
the name, or if a path was included, verify that the path is correct and
try again.
```

呼び出し演算子を使用する [と、解析](xref:Microsoft.PowerShell.Utility.Invoke-Expression) エラーを発生させるコードを実行できます。

```
PS> & "1+1"
& : The term '1+1' is not recognized as the name of a cmdlet, function, script
file, or operable program. Check the spelling of the name, or if a path was
included, verify that the path is correct and try again.
At line:1 char:2
+ & "1+1"
+  ~~~~~
    + CategoryInfo          : ObjectNotFound: (1+1:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
PS> Invoke-Expression "1+1"
2
```

Call 演算子を使用して、ファイル名を使用してスクリプトを実行できます。 次の例では、スペースを含むスクリプトファイル名を示しています。 スクリプトを実行しようとすると、PowerShell によって、ファイル名を含む引用符で囲まれた文字列の内容が表示されます。 Call 演算子を使用すると、ファイル名を含む文字列の内容を実行できます。

```
PS C:\Scripts> Get-ChildItem

    Directory: C:\Scripts


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/28/2018   1:36 PM             58 script name with spaces.ps1

PS C:\Scripts> ".\script name with spaces.ps1"
.\script name with spaces.ps1
PS C:\Scripts> & ".\script name with spaces.ps1"
Hello World!
```

スクリプトブロックの詳細については、「 [about_Script_Blocks](about_Script_Blocks.md)」を参照してください。

#### <a name="background-operator-"></a>Background 演算子 `&`

PowerShell ジョブで、パイプラインをバックグラウンドで実行します。 この演算子は UNIX の制御演算子アンパサンド () と同様に動作し `&` 、subshell でジョブとして非同期にコマンドを実行します。

この演算子は、機能的にはと同等です `Start-Job` 。 既定では、バックグラウンド操作は、並列タスクを開始した呼び出し元の現在の作業ディレクトリ内のジョブを開始します。 次の例は、バックグラウンドジョブオペレーターの基本的な使用方法を示しています。

```powershell
Get-Process -Name pwsh &
```

このコマンドは、の次の使用方法と機能的には同等です `Start-Job` 。

```powershell
Start-Job -ScriptBlock {Get-Process -Name pwsh}
```

と同様 `Start-Job` に、 `&` background 演算子はオブジェクトを返し `Job` ます。 このオブジェクトは、 `Receive-Job` `Remove-Job` ジョブの開始に使用した場合と同じように、およびで使用でき `Start-Job` ます。

```powershell
$job = Get-Process -Name pwsh &
Receive-Job $job -Wait
```

```Output

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
      0     0.00     221.16      25.90    6988 988 pwsh
      0     0.00     140.12      29.87   14845 845 pwsh
      0     0.00      85.51       0.91   19639 988 pwsh

```

```powershell
Remove-Job $job
```

`&`バックグラウンド演算子は、UNIX の制御演算子アンパサンド () と同様に、ステートメントのターミネータでも `&` あります。 これにより、バックグラウンド操作の後に追加のコマンドを呼び出すことができ `&` ます。 次の例は、バックグラウンド演算子の後に追加のコマンドを呼び出す方法を示して `&` います。

```powershell
$job = Get-Process -Name pwsh & Receive-Job $job -Wait
```

```Output

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
      0     0.00     221.16      25.90    6988 988 pwsh
      0     0.00     140.12      29.87   14845 845 pwsh
      0     0.00      85.51       0.91   19639 988 pwsh

```

これは、次のスクリプトと同じです。

```powershell
$job = Start-Job -ScriptBlock {Get-Process -Name pwsh}
Receive-Job $job -Wait
```

複数のコマンドをそれぞれ独自のバックグラウンドプロセスで実行し、すべてを1行で実行する場合は、 `&` コマンドごとにとの間に配置します。

```powershell
Get-Process -Name pwsh & Get-Service -Name BITS & Get-CimInstance -ClassName Win32_ComputerSystem &
```

PowerShell ジョブの詳細については、「 [about_Jobs](about_Jobs.md)」を参照してください。

#### <a name="cast-operator--"></a>Cast 演算子 `[ ]`

オブジェクトを指定した型に変換または制限します。 オブジェクトを変換できない場合は、PowerShell によってエラーが生成されます。

```powershell
[DateTime]"2/20/88" - [DateTime]"1/20/88"
[Int] (7/2)
[String] 1 + 0
[Int] '1' + 0
```

キャストは、 [キャスト表記](about_Variables.md)を使用して変数が割り当てられている場合にも実行できます。

#### <a name="comma-operator-"></a>コンマ演算子 `,`

二項演算子として、コンマは配列を作成するか、作成される配列にを追加します。 式モードでは、単項演算子として、コンマは1つのメンバーだけで配列を作成します。 メンバーの前にコンマを配置します。

```powershell
$myArray = 1,2,3
$SingleArray = ,1
Write-Output (,1)
```

`Write-Object`では引数が想定されているため、式をかっこで囲む必要があります。

#### <a name="dot-sourcing-operator-"></a>ドットソーシング演算子 `.`

現在のスコープでスクリプトを実行して、スクリプトによって作成されたすべての関数、エイリアス、および変数を現在のスコープに追加し、既存のスコープを上書きします。 スクリプトによって宣言されたパラメーターは変数になります。 値が指定されていないパラメーターは、値のない変数になります。 ただし、自動変数 `$args` は保持されます。

```powershell
. c:\scripts\sample.ps1 1 2 -Also:3
```

> [!NOTE]
> ドットソーシング演算子の後にスペースがあります。 現在のディレクトリを表すドット () 記号とドットを区別するには、スペースを使用し `.` ます。
>
> 次の例では、現在のディレクトリの Sample.ps1 スクリプトが現在のスコープで実行されています。
>
> ```powershell
> . .\sample.ps1
> ```

#### <a name="format-operator--f"></a>Format 演算子 `-f`

文字列オブジェクトの format メソッドを使用して文字列の書式を設定します。 演算子の左側に書式文字列を入力し、演算子の右側に書式設定するオブジェクトを入力します。

```powershell
"{0} {1,-10} {2:N}" -f 1,"hello",[math]::pi
```

```output
1 hello      3.14
```

書式設定された文字列で中かっこ () を保持する必要がある場合は、 `{}` 中かっこを2倍にしてエスケープすることができます。

```powershell
"{0} vs. {{0}}" -f 'foo'
```

```Output
foo vs. {0}
```

詳細については、「 [Format](/dotnet/api/system.string.format) メソッド」と「 [複合書式指定](/dotnet/standard/base-types/composite-formatting)」を参照してください。

#### <a name="index-operator--"></a>Index 演算子 `[ ]`

配列やハッシュテーブルなどのインデックス付きコレクションからオブジェクトを選択します。 配列インデックスは0から始まるため、最初のオブジェクトはとしてインデックスが作成され `[0]` ます。 配列の場合 (のみ)、負のインデックスを使用して最後の値を取得することもできます。 ハッシュテーブルには、キー値によってインデックスが付けられます。

```
PS> $a = 1, 2, 3
PS> $a[0]
1
PS> $a[-1]
3
```

```powershell
(Get-HotFix | Sort-Object installedOn)[-1]
```

```powershell
$h = @{key="value"; name="PowerShell"; version="2.0"}
$h["name"]
```

```output
PowerShell
```

```powershell
$x = [xml]"<doc><intro>Once upon a time...</intro></doc>"
$x["doc"]
```

```output
intro
-----
Once upon a time...
```

#### <a name="pipeline-operator-"></a>パイプライン演算子 `|`

このコマンドの前に続くコマンドの出力を ("パイプ" で) 送信します。 出力に複数のオブジェクト ("コレクション") が含まれている場合、パイプライン演算子はオブジェクトを一度に1つずつ送信します。

```powershell
Get-Process | Get-Member
Get-Service | Where-Object {$_.StartType -eq 'Automatic'}
```

#### <a name="pipeline-chain-operators--and-"></a>パイプラインチェーン演算子 `&&` と `||`

左側のパイプラインの成功に基づいて、条件付きで右側のパイプラインを実行します。

```powershell
# If Get-Process successfully finds a process called notepad,
# Stop-Process -Name notepad is called
Get-Process notepad && Stop-Process -Name notepad
```

```powershell
# If npm install fails, the node_modules directory is removed
npm install || Remove-Item -Recurse ./node_modules
```

詳細については、「 [About_Pipeline_Chain_Operators](About_Pipeline_Chain_Operators.md)」を参照してください。

#### <a name="range-operator-"></a>範囲演算子 `..`

上限と下限を指定して、整数配列内の連続する整数を表します。

```powershell
1..10
foreach ($a in 1..$max) {Write-Host $a}
```

範囲は逆順に作成することもできます。

```powershell
10..1
5..-5 | ForEach-Object {Write-Output $_}
```

PowerShell 6 以降では、範囲演算子は、 **文字** だけでなく **整数** でも使用できます。

文字の範囲を作成するには、境界文字を引用符で囲みます。

```powershell
PS> 'a'..'f'
a
b
c
d
e
f
```

```powershell
PS> 'F'..'A'
F
E
D
C
B
A
```

#### <a name="member-access-operator-"></a>メンバーアクセス演算子 `.`

オブジェクトのプロパティおよびメソッドにアクセスします。 メンバー名は式にすることができます。

```powershell
$myProcess.peakWorkingSet
(Get-Process PowerShell).kill()
'OS', 'Platform' | Foreach-Object { $PSVersionTable. $_ }
```

#### <a name="static-member-operator-"></a>静的メンバー演算子 `::`

.NET Framework クラスの静的プロパティおよびメソッドを呼び出します。 オブジェクトの静的なプロパティとメソッドを検索するには、コマンドレットの Static パラメーターを使用し `Get-Member` ます。  メンバー名は式にすることができます。

```powershell
[datetime]::Now
'MinValue', 'MaxValue' | Foreach-Object { [int]:: $_ }
```

#### <a name="ternary-operator--if-true--if-false"></a>三項演算子 `? <if-true> : <if-false>`

単純な条件付きケースでは、ステートメントの代わりに三項演算子を使用でき `if-else` ます。

詳細については、「 [about_If](about_If.md)」を参照してください。

#### <a name="null-coalescing-operator-"></a>Null 合体演算子 `??`

null 合体演算子 `??` は、左側のオペランドが null でない場合に、左側のオペランドの値を返します。 それ以外の場合は、右側のオペランドを評価し、その結果を返します。 左側のオペランドが null 以外に評価された場合、`??` 演算子は右側のオペランドを評価しません。

```powershell
$x = $null
$x ?? 100
```

```Output
100
```

次の例では、右側のオペランドは評価されません。

```powershell
[string] $todaysDate = '1/10/2020'
$todaysDate ?? (Get-Date).ToShortDateString()
```

```Output
1/10/2020
```

#### <a name="null-coalescing-assignment-operator-"></a>Null 合体代入演算子 `??=`

Null 合体代入演算子は、左側のオペランドが `??=` null と評価された場合にのみ、右側のオペランドの値を左側のオペランドに代入します。 左側のオペランドが null 以外に評価された場合、`??=` 演算子は右側のオペランドを評価しません。

```powershell
$x = $null
$x ??= 100
$x
```

```Output
100
```

次の例では、右側のオペランドは評価されません。

```powershell
[string] $todaysDate = '1/10/2020'
$todaysDate ??= (Get-Date).ToShortDateString()
```

```Output
1/10/2020
```

#### <a name="null-conditional-operators--and-"></a>Null 条件演算子 `?.` と `?[]`

> [!NOTE]
> この機能は、PowerShell 7.1 の試験段階からメインストリームに移行されました。

Null 条件演算子は、 `?.` そのオペランドが null 以外に評価される場合にのみ、メンバーアクセス、、または要素アクセス、、演算をオペランドに適用します。 `?[]` それ以外の場合は、null を返します。

PowerShell では、変数名の一部として `?` が許可されるため、これらの演算子を使用するには、変数名の正式な仕様が必要です。 したがって、変数名は、`${a}` や、`?` が変数名 `${a?}` に含まれる場合のように、`{}` で囲む必要があります。

次の例では、 **PropName** の値が返されます。

```powershell
$a = @{ PropName = 100 }
${a}?.PropName
```

```Output
100
```

次の例では、メンバー名 **PropName** にアクセスしないで、null を返します。

```powershell
$a = $null
${a}?.PropName
```

同様に、要素の値が返されます。

```powershell
$a = 1..10
${a}?[0]
```

```Output
1
```

オペランドが null の場合、要素はアクセスされず、null が返されます。

```PowerShell
$a = $null
${a}?[0]
```

> [!NOTE]
> PowerShell では、変数名の一部として `?` が許可されるため、これらの演算子を使用するには、変数名の正式な仕様が必要です。 したがって、変数名は、`${a}` や、`?` が変数名 `${a?}` に含まれる場合のように、`{}` で囲む必要があります。
>
> の変数名の構文は、 `${<name>}` 部分式演算子と混同しないようにしてください `$()` 。 詳細については、「 [about_Variables](about_Variables.md#variable-names-that-include-special-characters)」の「変数名」セクションを参照してください。

## <a name="see-also"></a>関連項目

[about_Arithmetic_Operators](about_Arithmetic_Operators.md)

[about_Assignment_Operators](about_Assignment_Operators.md)

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Logical_Operators](about_logical_operators.md)

[about_Operator_Precedence](about_operator_precedence.md)

[about_Type_Operators](about_Type_Operators.md)

[about_Pipeline_Chain_Operators](about_Pipeline_Chain_Operators.md)

[about_Split](about_Split.md)

[about_Join](about_Join.md)

[about_Redirection](about_Redirection.md)
