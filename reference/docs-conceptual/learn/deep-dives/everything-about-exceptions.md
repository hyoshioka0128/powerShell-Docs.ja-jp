---
title: 例外について知りたかったことのすべて
description: エラー処理は、コードを記述するときにはなくてはならないものです。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: cd17ae6b5ded052c93923b648155a4dda8956b34
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "90012563"
---
# <a name="everything-you-wanted-to-know-about-exceptions"></a>例外について知りたかったことのすべて

エラー処理は、コードを記述するときにはなくてはならないものです。 多くの場合、予期される動作になっているかどうか、条件を確認し、検証することができます。 予期しないことが発生した場合、例外処理を利用します。 他のユーザーのコードにより生成された例外を簡単に処理でき、また他のユーザーが処理するための独自の例外を生成することもできます。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

## <a name="basic-terminology"></a>基本的な用語

この記事に進む前に、いくつかの基本的な用語について説明する必要があります。

### <a name="exception"></a>例外

例外とは、通常のエラー処理で問題に対処できない場合に作成されるイベントのようなものです。
数値を 0 で除算することやメモリ不足などが、例外が作成されることになる例です。 場合によっては、使用しているコードの作成者が、特定の問題の発生時に例外を作成することがあります。

### <a name="throw-and-catch"></a>スローとキャッチ

例外が発生することを、例外がスローされると言います。 スローされた例外を処理するには、それをキャッチする必要があります。 例外がスローされ、キャッチされない場合、スクリプトの実行が停止されます。

### <a name="the-call-stack"></a>コール スタック

コール スタックは、互いに呼び出された関数の一覧です。 関数が呼び出されると、それがスタックまたはリストの先頭に追加されます。 関数が終了するか、関数から戻ると、それはスタックから削除されます。

例外がスローされると、例外ハンドラーがそれをキャッチするためにコール スタックがチェックされます。

### <a name="terminating-and-non-terminating-errors"></a>終了エラーと終了しないエラー

例外は通常、終了エラーです。 スローされた例外は、キャッチされるか、または現在の実行を終了します。 既定では、終了しないエラーは `Write-Error` によって生成され、例外をスローすることなく出力ストリームにエラーを追加します。

これを指摘している理由は、`Write-Error` とその他の終了しないエラーでは `catch` がトリガーされないためです。

### <a name="swallowing-an-exception"></a>例外を飲み込む

これは、エラーを抑止するためだけにキャッチすることです。 これは、問題のトラブルシューティングを非常に困難にする可能性があるため、慎重に利用してください。

## <a name="basic-command-syntax"></a>基本的なコマンド構文

ここでは、PowerShell で使用される基本的な例外処理構文の概要を示します。

### <a name="throw"></a>Throw

独自の例外イベントを作成するには、`throw` キーワードを使用して例外をスローします。

```powershell
function Start-Something
{
    throw "Bad thing happened"
}
```

これにより、終了エラーであるランタイム例外が作成されます。 これは、呼び出し元の関数の `catch` によって処理されるか、またはこのようなメッセージを使用してスクリプトを終了します。

```powershell
PS> Start-Something

Bad thing happened
At line:1 char:1
+ throw "Bad thing happened"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : OperationStopped: (Bad thing happened:String) [], RuntimeException
    + FullyQualifiedErrorId : Bad thing happened
```

#### <a name="write-error--erroraction-stop"></a>Write-Error -ErrorAction Stop

既定では `Write-Error` が終了エラーをスローしないことを説明しました。 `-ErrorAction Stop` を指定した場合、`Write-Error` によって、`catch` で処理できる終了エラーが生成されます。

```powershell
Write-Error -Message "Houston, we have a problem." -ErrorAction Stop
```

`-ErrorAction Stop` をこのように使用する方法について助言してくれた Lee Dailey 氏に感謝します。

#### <a name="cmdlet--erroraction-stop"></a>コマンドレット -ErrorAction Stop

高度な関数またはコマンドレットで `-ErrorAction Stop` を指定すると、すべての `Write-Error` ステートメントが、実行を停止するか `catch` によって処理できる終了エラーになります。

```powershell
Start-Something -ErrorAction Stop
```

### <a name="trycatch"></a>Try/Catch

PowerShell (およびその他の多くの言語) での例外処理のしくみでは、最初にコードのセクションを `try` し、エラーがスローされた場合にそれを `catch` できます。 以下に簡単なサンプルを示します。

```powershell
try
{
    Start-Something
}
catch
{
    Write-Output "Something threw an exception"
}

try
{
    Start-Something -ErrorAction Stop
}
catch
{
    Write-Output "Something threw an exception or used Write-Error"
}
```

`catch` スクリプトは、終了エラーが発生した場合にのみ実行されます。 `try` が正常に実行された場合は、`catch` はスキップされます。

### <a name="tryfinally"></a>Try/Finally

エラーを処理する必要はなくても、例外が発生したかどうかにかかわらず、何らかのコードを実行する必要があることがあります。 `finally` スクリプトはまさにこのためにあります。

次の例を見てみましょう。

```powershell
$command = [System.Data.SqlClient.SqlCommand]::New(queryString, connection)
$command.Connection.Open()
$command.ExecuteNonQuery()
$command.Connection.Close()
```

リソースを開いたり、リソースに接続したりするときは常に、リソースを閉じる必要があります。 `ExecuteNonQuery()` が例外をスローした場合、接続は閉じられません。 次に示すのは、同じコードの `try/finally` ブロック内です。

```powershell
$command = [System.Data.SqlClient.SqlCommand]::New(queryString, connection)
try
{
    $command.Connection.Open()
    $command.ExecuteNonQuery()
}
finally
{
    $command.Connection.Close()
}
```

この例では、エラーが発生した場合に接続が閉じられます。 エラーがない場合にも閉じられます。 `finally` スクリプトは毎回実行されます。

例外をキャッチしていないため、コール スタックに伝達されます。

### <a name="trycatchfinally"></a>Try/Catch/Finally

`catch` と `finally` を一緒に使用することはまったく問題ありません。 ほとんどの場合、どちらか一方を使用しますが、両方を使用するシナリオもあり得ます。

## <a name="psitem"></a>$PSItem

基本についての説明が終わったので、もう少し掘り下げてみましょう。

`catch` ブロック内には、例外の詳細を含む `ErrorRecord` 型の自動変数 (`$PSItem` または `$_`) があります。 主要なプロパティのいくつかについて簡単に説明します。

これらの例では、`ReadAllText` で無効なパスを使用して、この例外を生成しています。

```powershell
[System.IO.File]::ReadAllText( '\\test\no\filefound.log')
```

### <a name="psitemtostring"></a>PSItem.ToString()

これにより、ログ記録と一般出力で使用する簡潔なメッセージが得られます。 `$PSItem` が文字列内に配置されている場合、`ToString()` は自動的に呼び出されます。

```powershell
catch
{
    Write-Output "Ran into an issue: $($PSItem.ToString())"
}

catch
{
    Write-Output "Ran into an issue: $PSItem"
}
```

### <a name="psiteminvocationinfo"></a>$PSItem.InvocationInfo

このプロパティには、PowerShell によって収集される、例外がスローされた関数またはスクリプトに関する追加情報が含まれています。 これは、作成したサンプル例外の `InvocationInfo` です。

```powershell
PS> $PSItem.InvocationInfo | Format-List *

MyCommand             : Get-Resource
BoundParameters       : {}
UnboundArguments      : {}
ScriptLineNumber      : 5
OffsetInLine          : 5
ScriptName            : C:\blog\throwerror.ps1
Line                  :     Get-Resource
PositionMessage       : At C:\blog\throwerror.ps1:5 char:5
                        +     Get-Resource
                        +     ~~~~~~~~~~~~
PSScriptRoot          : C:\blog
PSCommandPath         : C:\blog\throwerror.ps1
InvocationName        : Get-Resource
```

ここで重要な詳細は、`ScriptName`、コードの `Line`、および呼び出しが開始された `ScriptLineNumber` です。

### <a name="psitemscriptstacktrace"></a>$PSItem.ScriptStackTrace

このプロパティは、例外が生成されたコードまでの関数呼び出しの順序を示します。

```powershell
PS> $PSItem.ScriptStackTrace
at Get-Resource, C:\blog\throwerror.ps1: line 13
at Start-Something, C:\blog\throwerror.ps1: line 5
at <ScriptBlock>, C:\blog\throwerror.ps1: line 18
```

ここでは同じスクリプトで関数の呼び出しを行っているだけですが、複数のスクリプトが関係している場合は呼び出しが追跡されます。

### <a name="psitemexception"></a>$PSItem.Exception

これは、スローされた実際の例外です。

#### <a name="psitemexceptionmessage"></a>$PSItem.Exception.Message

これは、例外について説明する一般的なメッセージであり、トラブルシューティングを行う際には適切な出発点となります。 ほとんどの例外には既定のメッセージがありますが、例外がスローされたときにカスタム値に設定することもできます。

```powershell
PS> $PSItem.Exception.Message

Exception calling "ReadAllText" with "1" argument(s): "The network path was not found."
```

これは、`ErrorRecord` にメッセージが 1 つも設定されていない場合に、`$PSItem.ToString()` を呼び出すときに返されるメッセージでもあります。

#### <a name="psitemexceptioninnerexception"></a>$PSItem.Exception.InnerException

例外には内部例外を含めることができます。 これは多くの場合、呼び出しているコードが例外をキャッチし、別の例外をスローする場合に発生します。 元の例外は、新しい例外の内部に配置されます。

```powershell
PS> $PSItem.Exception.InnerExceptionMessage
The network path was not found.
```

これについては、例外の再スローについての説明でまた取り上げます。

#### <a name="psitemexceptionstacktrace"></a>$PSItem.Exception.StackTrace

これは例外の `StackTrace` です。 上で `ScriptStackTrace` を示しましたが、これはマネージド コードの呼び出しが対象です。

```Output
at System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean
 useRights, FileShare share, Int32 bufferSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs,
 String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost)
at System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share, Int32
 bufferSize, FileOptions options, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean
 checkHost)
at System.IO.StreamReader..ctor(String path, Encoding encoding, Boolean detectEncodingFromByteOrderMarks,
 Int32 bufferSize, Boolean checkHost)
at System.IO.File.InternalReadAllText(String path, Encoding encoding, Boolean checkHost)
at CallSite.Target(Closure , CallSite , Type , String )
```

このスタック トレースは、イベントがマネージド コードからスローされた場合にのみ取得されます。 .NET Framework 関数を直接呼び出しているため、この例で確認できるのはこれがすべてです。 一般的には、スタック トレースを確認するときは、コードが停止し、システム コールが開始された場所を探します。

## <a name="working-with-exceptions"></a>例外の処理

例外には、基本構文と例外プロパティ以外にもさらに説明が必要です。

### <a name="catching-typed-exceptions"></a>型指定された例外のキャッチ

キャッチする例外を選択することができます。 例外には型があり、キャッチする例外の型を指定できます。

```powershell
try
{
    Start-Something -Path $path
}
catch [System.IO.FileNotFoundException]
{
    Write-Output "Could not find $path"
}
catch [System.IO.IOException]
{
        Write-Output "IO error with the file: $path"
}
```

例外の型は、例外に一致するものが見つかるまで `catch` ブロックごとにチェックされます。
例外は他の例外から継承できることを認識しておくことが重要です。 上記の例では `FileNotFoundException` は `IOException` から継承されています。 そのため、`IOException` が最初に発生した場合は、代わりにこれが呼び出されます。 複数の一致がある場合でも、1 つの catch ブロックのみが呼び出されます。

`System.IO.PathTooLongException` が発生した場合、`IOException` は一致しますが、`InsufficientMemoryException` が発生した場合は、何もキャッチされずにスタックの上位に伝達されます。

### <a name="catch-multiple-types-at-once"></a>複数の型を一度にキャッチする

同じ `catch` ステートメントを使用して、複数の例外の型をキャッチすることができます。

```powershell
try
{
    Start-Something -Path $path -ErrorAction Stop
}
catch [System.IO.DirectoryNotFoundException],[System.IO.FileNotFoundException]
{
    Write-Output "The path or file was not found: [$path]"
}
catch [System.IO.IOException]
{
    Write-Output "IO error with the file: [$path]"
}
```

この追加を提案していただいた `/u/Sheppard_Ra` にお礼を申し上げます。

### <a name="throwing-typed-exceptions"></a>型指定された例外のスロー

PowerShell では、型指定された例外をスローできます。 通常は `throw` を文字列と共に呼び出します。

```powershell
throw "Could not find: $path"
```

その代わりに、次のような例外アクセラレータを使用します。

```powershell
throw [System.IO.FileNotFoundException] "Could not find: $path"
```

ただし、この方法の場合はメッセージを指定する必要があります。

スローする例外の新しいインスタンスを作成することもできます。 これを行う場合はメッセージは省略可能になります。これは、すべての組み込み例外に対するシステムの既定のメッセージがあるためです。

```powershell
throw [System.IO.FileNotFoundException]::new()
throw [System.IO.FileNotFoundException]::new("Could not find path: $path")
```

PowerShell 5.0 以降を使用していない場合は、以前の `New-Object` のアプローチを使用する必要があります。

```powershell
throw (New-Object -TypeName System.IO.FileNotFoundException )
throw (New-Object -TypeName System.IO.FileNotFoundException -ArgumentList "Could not find path: $path")
```

型指定された例外を使用すると、自分または他のユーザーが前のセクションで説明した型によって例外をキャッチできます。

#### <a name="write-error--exception"></a>Write-Error -Exception

これらの型指定された例外を `Write-Error` に追加しても、例外の型別にエラーを `catch` できます。 以下の例のように `Write-Error` を使用します。

```powershell
# with normal message
Write-Error -Message "Could not find path: $path" -Exception ([System.IO.FileNotFoundException]::new()) -ErrorAction Stop

# With message inside new exception
Write-Error -Exception ([System.IO.FileNotFoundException]::new("Could not find path: $path")) -ErrorAction Stop

# Pre PS 5.0
Write-Error -Exception ([System.IO.FileNotFoundException]"Could not find path: $path") -ErrorAction Stop

Write-Error -Message "Could not find path: $path" -Exception ( New-Object -TypeName System.IO.FileNotFoundException ) -ErrorAction Stop
```

そうすると、次のようにしてこれをキャッチできます。

```powershell
catch [System.IO.FileNotFoundException]
{
    Write-Log $PSItem.ToString()
}
```

#### <a name="the-big-list-of-net-exceptions"></a>.NET 例外の巨大な一覧

私は [Reddit/r/PowerShell コミュニティ][] の支援を得て、この投稿の補足となる、何百もの .NET 例外が含まれているマスター リストをまとめました。

- [.NET 例外の巨大な一覧][]

まず、このリストを検索して、自分の状況に適していると思われる例外を見つけます。 基本の `System` 名前空間の例外を使用するようにしてください。

### <a name="exceptions-are-objects"></a>例外はオブジェクトである

多数の型指定された例外を使い始めるときは、それらがオブジェクトであることを忘れないでください。 異なる例外には、異なるコンストラクターとプロパティがあります。 [FileNotFoundException][] のドキュメントで `System.IO.FileNotFoundException` を参照すると、メッセージとファイル パスを渡すことができるのが分かります。

```powershell
[System.IO.FileNotFoundException]::new("Could not find file", $path)
```

また、そのファイル パスを公開する `FileName` プロパティがあります。

```powershell
catch [System.IO.FileNotFoundException]
{
    Write-Output $PSItem.Exception.FileName
}
```

他のコンストラクターとオブジェクトのプロパティについては、[.NET ドキュメント][]を参照してください。

### <a name="re-throwing-an-exception"></a>例外を再スローする

`catch` ブロックで実行するのが同じ例外の `throw` のみである場合は、`catch` しないでください。 例外を `catch` するのは、それが発生したときに処理または何らかのアクションを実行する場合のみにしてください。

例外に対してアクションを実行しながら、同時にダウンストリームでそれを処理できるように例外を再スローすることが必要になる場合があります。 検出された場所の近くでメッセージを書き込んだり、問題をログに記録したりするが、問題の処理はスタックのさらに上で行うことができます。

```powershell
catch
{
    Write-Log $PSItem.ToString()
    throw $PSItem
}
```

興味深いことに、`catch` 内から `throw` を呼び出して、現在の例外を再スローすることができます。

```powershell
catch
{
    Write-Log $PSItem.ToString()
    throw
}
```

ソース スクリプトや行番号などの元の実行情報を保持するために、例外を再スローできます。 この時点で新しい例外をスローすると、例外が開始された場所が隠されてしまいます。

#### <a name="re-throwing-a-new-exception"></a>新しい例外を再スローする

例外をキャッチしたが、別の例外をスローしたい場合は、元の例外を新しい例外の内側に入れ子にする必要があります。 これにより、スタックのこの後で `$PSItem.Exception.InnerException` としてアクセスできるようになります。

```powershell
catch
{
    throw [System.MissingFieldException]::new('Could not access field',$PSItem.Exception)
}
```

#### <a name="pscmdletthrowterminatingerror"></a>$PSCmdlet.ThrowTerminatingError()

未処理の例外に対して `throw` を使用することの問題の 1 つは、エラー メッセージが `throw` ステートメントを指し、その行が問題であることを示すことです。

```Output
Unable to find the specified file.
At line:31 char:9
+         throw [System.IO.FileNotFoundException]::new()
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : OperationStopped: (:) [], FileNotFoundException
    + FullyQualifiedErrorId : Unable to find the specified file.
```

31 行目で `throw` を呼び出したためにスクリプトが失敗したと通知するエラー メッセージは、スクリプトのユーザーに対して表示する適切なメッセージではありません。 これは役に立つものではありません。

Dexter Dhami 氏が、`ThrowTerminatingError()` を使用してこれを訂正できることを指摘しました。

```powershell
$PSCmdlet.ThrowTerminatingError(
    [System.Management.Automation.ErrorRecord]::new(
        ([System.IO.FileNotFoundException]"Could not find $Path"),
        'My.ID',
        [System.Management.Automation.ErrorCategory]::OpenError,
        $MyObject
    )
)
```

`ThrowTerminatingError()` が `Get-Resource` と呼ばれる関数の内部で呼び出されたと仮定した場合、このエラーが発生します。

```Output
Get-Resource : Could not find C:\Program Files (x86)\Reference
Assemblies\Microsoft\Framework\.NETPortable\v4.6\System.IO.xml
At line:6 char:5
+     Get-Resource -Path $Path
+     ~~~~~~~~~~~~
    + CategoryInfo          : OpenError: (:) [Get-Resource], FileNotFoundException
    + FullyQualifiedErrorId : My.ID,Get-Resource
```

問題の原因として `Get-Resource` 関数を指していることが確認できます。 これはユーザーにとって役立つ情報です。

`$PSItem` は `ErrorRecord` であるため、この方法で `ThrowTerminatingError` を使用して再スローすることもできます。

```powershell
catch
{
    $PSCmdlet.ThrowTerminatingError($PSItem)
}
```

これにより、エラーの原因がコマンドレットに変更され、コマンドレットのユーザーに対して関数の内部が非表示になります。

## <a name="try-can-create-terminating-errors"></a>Try で終了エラーが作成される場合がある

Kirk Munro 氏は、一部の例外は `try/catch` ブロック内で実行された場合に単なる終了エラーとなることを指摘しています。 これは、0 除算のランタイム例外を生成する例です。

```powershell
function Start-Something { 1/(1-1) }
```

次に、これを以下のように呼び出して、エラーが生成され、メッセージも出力されることを確認します。

```powershell
&{ Start-Something; Write-Output "We did it. Send Email" }
```

しかし、同じコードを `try/catch` 内に配置すると、別の現象が発生します。

```powershell
try
{
    &{ Start-Something; Write-Output "We did it. Send Email" }
}
catch
{
    Write-Output "Notify Admin to fix error and send email"
}
```

エラーが終了エラーになり、最初のメッセージが出力されないことがわかります。 何が問題かというと、このコードを関数に含めることができるが、`try/catch` が使用されると動作が異なるということです。

私自身はこの問題に遭遇したことはありませんが、注意が必要なコーナー ケースです。

### <a name="pscmdletthrowterminatingerror-inside-trycatch"></a>try/catch 内の $PSCmdlet.ThrowTerminatingError()

`$PSCmdlet.ThrowTerminatingError()` の特別な点の 1 つは、コマンドレット内では終了エラーが作成されるが、コマンドレットから出た後は、終了しないエラーに変わるということです。 これにより、エラーの処理方法を決定するのは、関数の呼び出し元の責任になります。 `-ErrorAction Stop` を使用するか `try{...}catch{...}` 内から呼び出すことによって、終了エラーに戻すことができます。

### <a name="public-function-templates"></a>パブリック関数のテンプレート

Kirk Munro 氏との会話の中で得られた最後の重要な点は、すべての高度な関数のすべての `begin`、`process`、および `end` ブロックの周りに `try{...}catch{...}` を配置することです。 これらの汎用 catch ブロックでは、`$PSCmdlet.ThrowTerminatingError($PSItem)` を使用する単一行により、関数を離れるすべての例外を処理します。

```powershell
function Start-Something
{
    [CmdletBinding()]
    param()

    process
    {
        try
        {
            ...
        }
        catch
        {
            $PSCmdlet.ThrowTerminatingError($PSItem)
        }
    }
}
```

すべてが関数の `try` ステートメント内にあるため、すべてが一貫して動作します。 これにより、生成されたエラーから内部コードを非表示にするクリーンなエラーもエンド ユーザーに提供されます。

## <a name="trap"></a>Trap

ここでは、例外の `try/catch` の側面について重点的に取り上げました。 しかし、最後に、あるレガシ機能について説明する必要があります。

`trap` は、スクリプトまたは関数内に配置され、そのスコープで発生するすべての例外をキャッチします。 例外が発生すると、`trap` 内のコードが実行され、その後、通常のコードが続行されます。 複数の例外が発生した場合、trap は何度も呼び出されます。

```powershell
trap
{
    Write-Log $PSItem.ToString()
}

throw [System.Exception]::new('first')
throw [System.Exception]::new('second')
throw [System.Exception]::new('third')
```

個人的にはこの方法を採用したことはありませんが、発生した例外をすべてログに記録し、その後実行を継続する管理者またはコントローラーのスクリプトでは、価値がある場合があります。

## <a name="closing-remarks"></a>最後に

スクリプトに適切な例外処理を追加すると、安定性が向上するだけでなく、これらの例外のトラブルシューティングも容易になります。

`throw` についてたくさんの時間を割いたのは、例外処理について話をする際の中核となる概念であるためです。 PowerShell には、`throw` を使用するすべての状況を処理できる `Write-Error` も用意されています。 そのため、これを読んだ後に `throw` を使用することが必須だとは考えないでください。

例外処理について詳細に説明してきましたが、私は `Write-Error -Stop` を使用してコードでエラーを生成する方法に切り替える予定です。 また、Kirk 氏のアドバイスに従って、すべての関数に対して `ThrowTerminatingError` をメインの例外ハンドラーにします。

<!-- link references -->
[powershellexplained.com]: https://powershellexplained.com/
[オリジナル バージョン]: https://powershellexplained.com/2017-04-10-Powershell-exceptions-everything-you-ever-wanted-to-know/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[Reddit/r/PowerShell コミュニティ]: https://www.reddit.com/r/PowerShell/comments/64866o/kevmar_all_net_46_exceptions_list_for_use_with/
[.NET 例外の巨大な一覧]: https://powershellexplained.com/2017-04-07-all-dotnet-exception-list
[FileNotFoundException]: /dotnet/api/System.IO.FileNotFoundException
[.NET ドキュメント]: /dotnet/api.
