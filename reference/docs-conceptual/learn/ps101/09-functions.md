---
title: 関数
description: PowerShell 関数を使用すると、スクリプトで再利用できるツールを作成することができます。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: e4734b556a78f67c54152dad93eada536dd1c928
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99601189"
---
# <a name="chapter-9---functions"></a>第 9 章 - 関数

PowerShell のワンライナーまたはスクリプトを記述していると、それを、さまざまなシナリオに合わせて変更しなければならないことがよく出てきます。このようなワンライナーやスクリプトが、再利用可能な関数への有力な変換候補である可能性は十分にあります。

関数ではツールがより重視されているため、私はできる限り関数を記述したいと考えています。 関数をスクリプト モジュールに置き、そのモジュールを `$env:PSModulePath` に置いて、関数を呼び出すことができます。その際、その関数が保存されている場所を物理的に特定する必要はありません。 PowerShellGet モジュールを使用すると、NuGet リポジトリでこれらのモジュールを簡単に共有できます。 PowerShellGet は、PowerShell バージョン5.0 以降に付属しています。 PowerShell バージョン3.0 以降については、個別のダウンロードとして入手できます。

物事を複雑にしすぎないようにしてください。 シンプルを心がけ、最も簡単にタスクを実行できる方法を使用します。 再利用するコードで、エイリアスと位置指定パラメーターを使用するのは避け、 読みやすさを考えてコードを書式設定してください。 値はハードコーディングせず、パラメーターと変数を使用します。 悪い影響が何もなくても、不要なコードは記述しないでください。 不必要に複雑になってしまいます。 PowerShell コードを記述するときは、細部にまで注意を払うことが大いに役立ちます。

## <a name="naming"></a>名前を付ける

PowerShell で関数に名前を付けるときは、承認されている動詞と単数形の名詞を含む[パスカル ケース][]名を使用します。 また、名詞の前にプレフィックスを付けることをお勧めします。 (例: `<ApprovedVerb>-<Prefix><SingularNoun>`)。

PowerShell には承認されている動詞の一覧があり、`Get-Verb` を実行して取得できます。

```powershell
Get-Verb | Sort-Object -Property Verb
```

```Output
Verb        Group
----        -----
Add         Common
Approve     Lifecycle
Assert      Lifecycle
Backup      Data
Block       Security
Checkpoint  Data
Clear       Common
Close       Common
Compare     Data
Complete    Lifecycle
Compress    Data
Confirm     Lifecycle
Connect     Communications
Convert     Data
ConvertFrom Data
ConvertTo   Data
Copy        Common
Debug       Diagnostic
Deny        Lifecycle
Disable     Lifecycle
Disconnect  Communications
Dismount    Data
Edit        Data
Enable      Lifecycle
Enter       Common
Exit        Common
Expand      Data
Export      Data
Find        Common
Format      Common
Get         Common
Grant       Security
Group       Data
Hide        Common
Import      Data
Initialize  Data
Install     Lifecycle
Invoke      Lifecycle
Join        Common
Limit       Data
Lock        Common
Measure     Diagnostic
Merge       Data
Mount       Data
Move        Common
New         Common
Open        Common
Optimize    Common
Out         Data
Ping        Diagnostic
Pop         Common
Protect     Security
Publish     Data
Push        Common
Read        Communications
Receive     Communications
Redo        Common
Register    Lifecycle
Remove      Common
Rename      Common
Repair      Diagnostic
Request     Lifecycle
Reset       Common
Resize      Common
Resolve     Diagnostic
Restart     Lifecycle
Restore     Data
Resume      Lifecycle
Revoke      Security
Save        Data
Search      Common
Select      Common
Send        Communications
Set         Common
Show        Common
Skip        Common
Split       Common
Start       Lifecycle
Step        Common
Stop        Lifecycle
Submit      Lifecycle
Suspend     Lifecycle
Switch      Common
Sync        Data
Test        Diagnostic
Trace       Diagnostic
Unblock     Security
Undo        Common
Uninstall   Lifecycle
Unlock      Common
Unprotect   Security
Unpublish   Data
Unregister  Lifecycle
Update      Data
Use         Other
Wait        Lifecycle
Watch       Common
Write       Communications
```

この例では、**Verb** 列に基づいて結果を並べ替えました。 **Group** 列には、これらの動詞がどのように使用されるかが示されています。 関数がモジュールに追加されている場合、PowerShell で承認されている動詞を選択することが重要です。 未承認の動詞を選択すると、読み込み時にモジュールによって警告メッセージが生成されます。 この警告メッセージにより、プロらしくない関数になってしまいます。 また、未承認の動詞があると、関数の検出可能性も制限されます。

## <a name="a-simple-function"></a>単純な関数

PowerShell の関数を宣言するには、関数キーワード、関数名、左中かっこ、右中かっこの順に指定します。 関数によって実行されるコードは、中かっこに囲まれています。

```powershell
function Get-Version {
    $PSVersionTable.PSVersion
}
```

次に示す関数は、PowerShell のバージョンを返す簡単な例です。

```powershell
Get-Version
```

```Output
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      14393  693
```

`Get-Version` などの関数の名前と、PowerShell の既定のコマンドや他のユーザーが記述するかもしれないコマンドの名前が競合する可能性は大いにあります。 関数の名詞部分にプレフィックスを付けることを勧めるのは、このためです。プレフィックスは名前の競合を防ぐうえで役立ちます。 次の例では、プレフィックス "PS" を使用します。

```powershell
function Get-PSVersion {
    $PSVersionTable.PSVersion
}
```

次の関数は、名前以外は、前の関数とまったく同じです。

```powershell
Get-PSVersion
```

```Output
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      14393  693
```

名詞の前に PS のようなプレフィックスを付けても、名前が競合する可能性は少なくありません。 そこで私は、通常、関数の名詞の前に自分のイニシャルを付けます。 基準を決めて、それに従ってください。

```powershell
function Get-MrPSVersion {
    $PSVersionTable.PSVersion
}
```

この関数は基本的には前の 2 つと変わりませんが、他の PowerShell コマンドとの名前の競合を、さらに確実に避けられる名前が使われています。

```powershell
Get-MrPSVersion
```

```Output
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      14393  693
```

メモリに読み込まれると、**Function** PSDrive で関数を確認できます。

```powershell
Get-ChildItem -Path Function:\Get-*Version
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-Version
Function        Get-PSVersion
Function        Get-MrPSVersion
```

これらの関数を現在のセッションから削除する場合は、**Function** PSDrive から削除するか、PowerShell を一度閉じてから再度開く必要があります。

```powershell
Get-ChildItem -Path Function:\Get-*Version | Remove-Item
```

関数が本当に削除されていることを確認します。

```powershell
Get-ChildItem -Path Function:\Get-*Version
```

関数がモジュールの一部として読み込まれている場合は、モジュールをアンロードすることで、関数を削除できます。

```powershell
Remove-Module -Name <ModuleName>
```

`Remove-Module` コマンドレットを実行すると、現在の PowerShell セッションのメモリからモジュールが削除されますが、システムまたはディスクからは削除されません。

## <a name="parameters"></a>パラメーター

値を静的に割り当てないでください。 パラメーターと変数を使用してください。 また、パラメーターに名前を付けるときは、できる限り既定のコマンドレットと同じ名前を、パラメーター名として使用してください。

```powershell
function Test-MrParameter {

    param (
        $ComputerName
    )

    Write-Output $ComputerName

}
```

ここでパラメーター名として **ComputerName** を使い、**Computer**、**ServerName**、または **Host** を使わなかったのは、 自分の関数を、既定のコマンドレットのように標準化したかったからです。

システム上のすべてのコマンドに対してクエリを実行する関数を作成します。この関数により、特定のパラメーター名を持つコマンドの数を取得できます。

```powershell
function Get-MrParameterCount {
    param (
        [string[]]$ParameterName
    )

    foreach ($Parameter in $ParameterName) {
        $Results = Get-Command -ParameterName $Parameter -ErrorAction SilentlyContinue

        [pscustomobject]@{
            ParameterName = $Parameter
            NumberOfCmdlets = $Results.Count
        }
    }
}
```

次に示す結果でわかるように、**ComputerName** パラメーターを持つコマンドは 39 個あります。 **Computer**、**ServerName**、**Host**、**Machine** などのパラメーターを持つコマンドレットはありません。

```powershell
Get-MrParameterCount -ParameterName ComputerName, Computer, ServerName, Host, Machine
```

```Output
ParameterName NumberOfCmdlets
------------- ---------------
ComputerName               39
Computer                    0
ServerName                  0
Host                        0
Machine                     0
```

また、パラメーター名の大文字と小文字も、既定のコマンドレットに揃えることをお勧めします。 `computername` ではなく、`ComputerName` を使用してください。 これにより、関数の外観が既定のコマンドレットと同じようになります。 既に PowerShell に慣れている方にとっては、使いやすく感じるでしょう。

`param`ステートメントを使用すると、1つ以上のパラメーターを定義できます。 パラメーター定義はコンマ () で区切られ `,` ます。 詳細については、「 [about_Functions_Advanced_Parameters][]」を参照してください。

## <a name="advanced-functions"></a>高度な関数

PowerShell の関数は、非常に簡単に高度な関数に変換できます。 関数と高度な関数の違いの 1 つが、高度な関数には、多くの共通パラメーターが自動的に追加されることです。 たとえば、**Verbose**、**Debug** などのパラメーターが、共通パラメーターとして挙げられます。

最初に、前のセクションで使用した `Test-MrParameter` 関数について説明します。

```powershell
function Test-MrParameter {

    param (
        $ComputerName
    )

    Write-Output $ComputerName

}
```

ここでは `Test-MrParameter` 関数に共通パラメーターがないことに注意してください。 共通パラメーターを表示する方法はいくつかあります。 1 つは `Get-Command` を使用して構文を表示する方法です。

```powershell
Get-Command -Name Test-MrParameter -Syntax
```

```Output
Test-MrParameter [[-ComputerName] <Object>]
```

もう 1 つは、`Get-Command` でパラメーターをドリルダウンする方法です。

```powershell
(Get-Command -Name Test-MrParameter).Parameters.Keys
```

```Output
ComputerName
```

`CmdletBinding` を追加して、関数を高度な関数に変換します。

```powershell
function Test-MrCmdletBinding {

    [CmdletBinding()] #<<-- This turns a regular function into an advanced function
    param (
        $ComputerName
    )

    Write-Output $ComputerName

}
```

`CmdletBinding` を追加すると、共通パラメーターが自動的に追加されます。 `CmdletBinding` には `param` ブロックが必要ですが、`param` ブロックは空にできます。

```powershell
Get-Command -Name Test-MrCmdletBinding -Syntax
```

```Output
Test-MrCmdletBinding [[-ComputerName] <Object>] [<CommonParameters>]
```

`Get-Command` でパラメーターをドリルダウンすると、共通パラメーター名を含む実際のパラメーター名が表示されます。

```powershell
(Get-Command -Name Test-MrCmdletBinding).Parameters.Keys
```

```Output
ComputerName
Verbose
Debug
ErrorAction
WarningAction
InformationAction
ErrorVariable
WarningVariable
InformationVariable
OutVariable
OutBuffer
PipelineVariable
```

## <a name="supportsshouldprocess"></a>SupportsShouldProcess

`SupportsShouldProcess` により、**WhatIf** パラメーターと **Confirm** パラメーターが追加されます。 これらのパラメーターは、変更するコマンドにのみ必要です。

```powershell
function Test-MrSupportsShouldProcess {

    [CmdletBinding(SupportsShouldProcess)]
    param (
        $ComputerName
    )

    Write-Output $ComputerName

}
```

現在、**WhatIf** パラメーターと **Confirm** パラメーターがあります。

```powershell
Get-Command -Name Test-MrSupportsShouldProcess -Syntax
```

```Output
Test-MrSupportsShouldProcess [[-ComputerName] <Object>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

ここでも、`Get-Command` を使用して、共通パラメーター名と WhatIf および Confirm を含む実際のパラメーター名の一覧を取得できます。

```powershell
(Get-Command -Name Test-MrSupportsShouldProcess).Parameters.Keys
```

```Output
ComputerName
Verbose
Debug
ErrorAction
WarningAction
InformationAction
ErrorVariable
WarningVariable
InformationVariable
OutVariable
OutBuffer
PipelineVariable
WhatIf
Confirm
```

## <a name="parameter-validation"></a>パラメーターの検証

入力は早い段階で検証してください。 コードの処理を続行できるようにしても、有効な値が入力されていなくて実行できなければ意味がありません。

入力する変数は必ず、パラメーターに対して使用されているものにします (データ型を指定します)。

```powershell
function Test-MrParameterValidation {

    [CmdletBinding()]
    param (
        [string]$ComputerName
    )

    Write-Output $ComputerName

}
```

この例では、**ComputerName** パラメーターに対して、**String** をデータ型として指定しました。 これにより、コンピューター名を 1 つしか指定できなくなります。 コンマ区切りのリストを使用して複数のコンピューター名が指定されていると、エラーが生成されます。

```powershell
Test-MrParameterValidation -ComputerName Server01, Server02
```

```Output
Test-MrParameterValidation : Cannot process argument transformation on parameter
'ComputerName'. Cannot convert value to type System.String.
At line:1 char:42
+ Test-MrParameterValidation -ComputerName Server01, Server02
+
    + CategoryInfo          : InvalidData: (:) [Test-MrParameterValidation], ParameterBindingArg
     umentTransformationException
    + FullyQualifiedErrorId : ParameterArgumentTransformationError,Test-MrParameterValidation
```

現時点での定義の問題は、**ComputerName** パラメーターの値は省略できるのに、関数が正常に完了するには、その値が必要であることです。 ここで、`Mandatory` パラメーター属性が役に立ちます。

```powershell
function Test-MrParameterValidation {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory)]
        [string]$ComputerName
    )

    Write-Output $ComputerName

}
```

この例で使用している構文は、PowerShell バージョン3.0 以降に対応しています。
`[Parameter(Mandatory=$true)]` を指定すると、関数と PowerShell バージョン 2.0 以降の互換性が確保されます。 これで **ComputerName** が必須になり、指定されていないと、入力を求めるメッセージが関数によって表示されます。

```powershell
Test-MrParameterValidation
```

```Output
cmdlet Test-MrParameterValidation at command pipeline position 1
Supply values for the following parameters:
ComputerName:
```

**ComputerName** パラメーターに複数の値を指定できるようにする場合は、**String** データ型を使用しますが、文字列の配列を指定できるように、データ型には左角かっこと右角かっこを追加します。

```powershell
function Test-MrParameterValidation {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory)]
        [string[]]$ComputerName
    )

    Write-Output $ComputerName

}
```

**ComputerName** パラメーターに既定値を指定する必要が出てくることもあるでしょう (指定されていない場合)。
問題は、必須パラメーターでは既定値を使用できないことです。 この場合は、`ValidateNotNullOrEmpty` パラメーター検証属性を既定値と共に使用する必要があります。

```powershell
function Test-MrParameterValidation {

    [CmdletBinding()]
    param (
        [ValidateNotNullOrEmpty()]
        [string[]]$ComputerName = $env:COMPUTERNAME
    )

    Write-Output $ComputerName

}
```

既定値を設定するときも、静的な値は使用しないでください。 この例では、`$env:COMPUTERNAME` が既定値として使用されています。つまり、値が指定されていない場合、これはローカルコンピューター名に自動的に変換されます。

## <a name="verbose-output"></a>詳細出力

インライン コメントは、特に複雑なコードを記述している場合は便利ですが、コード自体を調べていないユーザーには表示されません。

次の例の関数には、`foreach` ループにインライン コメントがあります。 この特別なコメントについては、それほど苦労せずに見つけられるでしょう。しかし、関数に数百行のコードが含まれていたらどうかを想像してみてください。

```powershell
function Test-MrVerboseOutput {

    [CmdletBinding()]
    param (
        [ValidateNotNullOrEmpty()]
        [string[]]$ComputerName = $env:COMPUTERNAME
    )

    foreach ($Computer in $ComputerName) {
        #Attempting to perform some action on $Computer <<-- Don't use
        #inline comments like this, use write verbose instead.
        Write-Output $Computer
    }

}
```

もっと良い方法があります。インライン コメントではなく `Write-Verbose` を使用する方法です。

```powershell
function Test-MrVerboseOutput {

    [CmdletBinding()]
    param (
        [ValidateNotNullOrEmpty()]
        [string[]]$ComputerName = $env:COMPUTERNAME
    )

    foreach ($Computer in $ComputerName) {
        Write-Verbose -Message "Attempting to perform some action on $Computer"
        Write-Output $Computer
    }

}
```

**Verbose** パラメーターを指定せずに関数を呼び出すと、詳細出力は表示されません。

```powershell
Test-MrVerboseOutput -ComputerName Server01, Server02
```

**Verbose** パラメーターを指定して呼び出すと、詳細出力は表示されます。

```powershell
Test-MrVerboseOutput -ComputerName Server01, Server02 -Verbose
```

## <a name="pipeline-input"></a>パイプライン入力

関数でパイプライン入力を受け入れるには、追加コーディングがいくつか必要です。 本書で前述したように、コマンドが受け入れることができるのは、**値** (型) または **プロパティ名** によるパイプライン入力です。 関数はネイティブ コマンドと同じように記述できるため、これらの入力の種類のいずれか、または両方を受け入れることができます。

**値** によるパイプライン入力を受け入れるには、その特定のパラメーターに対して `ValueFromPipeline` パラメーター属性を指定します。 受け入れることができるのは、いずれかのデータ型からの **値** によるパイプライン入力だけであることに注意してください。 たとえば、文字列入力を受け入れるパラメーターが 2 つある場合は、一方だけが **値** によるパイプライン入力を受け入れることができます。これは、両方の文字列パラメーターに対してそれを指定すると、パイプライン入力でバインド先を認識できなくなるためです。 これも、私がこの種類のパイプライン入力を、**値** ではなく "_型_" によるパイプライン入力と呼ぶ理由です。

`foreach` ループでの項目の処理方法と同じように、パイプライン入力は 1 つずつ行われます。
少なくとも、配列を入力として受け入れる場合は、`process` ブロックがこれらの各項目を処理する必要があります。 入力として 1 つの値しか受け入れていない場合、`process` ブロックは不要ですが、それでも一貫性を確保するために指定することをお勧めします。

```powershell
function Test-MrPipelineInput {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory,
                   ValueFromPipeline)]
        [string[]]$ComputerName
    )

    PROCESS {
        Write-Output $ComputerName
    }

}
```

**プロパティ名** によるパイプライン入力の受け入れも似ていますが、これは `ValueFromPipelineByPropertyName` パラメーター属性で指定され、データ型に関係なく、任意の数のパラメーターに対して指定できる点が異なります。 重要なのは、パイプ入力中のコマンドの出力には、関数のパラメーターまたはパラメーター エイリアスの名前と一致するプロパティ名が必要であることです。

```powershell
function Test-MrPipelineInput {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory,
                   ValueFromPipelineByPropertyName)]
        [string[]]$ComputerName
    )

    PROCESS {
            Write-Output $ComputerName
    }

}
```

`BEGIN` ブロックと`END` ブロックは省略可能です。 `BEGIN` は `PROCESS` ブロックの前で指定され、パイプラインから項目が受信される前の初期処理を実行するときに使用されます。 これを理解することが重要です。 パイプ処理された値には、`BEGIN` ブロックではアクセスできません。 `END` ブロックは `PROCESS` ブロックの後ろで指定され、パイプ入力されたすべての項目が処理された後、その項目をクリーンアップするために使用されます。

## <a name="error-handling"></a>エラー処理

次の例に示す関数では、コンピューターに接続できない場合に、ハンドルされない例外が生成されます。

```powershell
function Test-MrErrorHandling {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory,
                   ValueFromPipeline,
                   ValueFromPipelineByPropertyName)]
        [string[]]$ComputerName
    )

    PROCESS {
        foreach ($Computer in $ComputerName) {
            Test-WSMan -ComputerName $Computer
        }
    }

}
```

PowerShell でエラーを処理する方法はいくつかあります。 `Try/Catch` は、最新のエラー処理方法です。

```powershell
function Test-MrErrorHandling {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory,
                   ValueFromPipeline,
                   ValueFromPipelineByPropertyName)]
        [string[]]$ComputerName
    )

    PROCESS {
        foreach ($Computer in $ComputerName) {
            try {
                Test-WSMan -ComputerName $Computer
            }
            catch {
                Write-Warning -Message "Unable to connect to Computer: $Computer"
            }
        }
    }

}
```

この例の関数では、エラー処理が使用されていますが、ハンドルされない例外も生成されます。終了するエラーがコマンドによって生成されないためです。 これを理解することも重要です。 キャッチされるのは、終了するエラーだけです。 そこで **ErrorAction** パラメーターと **Stop** を値として指定して、終了しないエラーを終了するエラーに変換します。

```powershell
function Test-MrErrorHandling {

    [CmdletBinding()]
    param (
        [Parameter(Mandatory,
                   ValueFromPipeline,
                   ValueFromPipelineByPropertyName)]
        [string[]]$ComputerName
    )

    PROCESS {
        foreach ($Computer in $ComputerName) {
            try {
                Test-WSMan -ComputerName $Computer -ErrorAction Stop
            }
            catch {
                Write-Warning -Message "Unable to connect to Computer: $Computer"
            }
        }
    }

}
```

グローバル `$ErrorActionPreference` 変数は、どうしても必要な場合を除き、変更しないでください。 PowerShell 関数内から .NET のような何かを直接使用している場合は、コマンド自体で **ErrorAction** を指定することはできません。 このシナリオでは、グローバル `$ErrorActionPreference` 変数の変更が必要になる場合がありますが、変更する場合は、コマンドの実行後すぐに元に戻してください。

## <a name="comment-based-help"></a>コメント ベースのヘルプ

関数にはコメント ベースのヘルプを追加することをお勧めします。これにより、関数を共有しているユーザーが、その使用方法を理解できるようになります。

```powershell
function Get-MrAutoStoppedService {

<#
.SYNOPSIS
    Returns a list of services that are set to start automatically, are not
    currently running, excluding the services that are set to delayed start.

.DESCRIPTION
    Get-MrAutoStoppedService is a function that returns a list of services from
    the specified remote computer(s) that are set to start automatically, are not
    currently running, and it excludes the services that are set to start automatically
    with a delayed startup.

.PARAMETER ComputerName
    The remote computer(s) to check the status of the services on.

.PARAMETER Credential
    Specifies a user account that has permission to perform this action. The default
    is the current user.

.EXAMPLE
     Get-MrAutoStoppedService -ComputerName 'Server1', 'Server2'

.EXAMPLE
     'Server1', 'Server2' | Get-MrAutoStoppedService

.EXAMPLE
     Get-MrAutoStoppedService -ComputerName 'Server1' -Credential (Get-Credential)

.INPUTS
    String

.OUTPUTS
    PSCustomObject

.NOTES
    Author:  Mike F Robbins
    Website: http://mikefrobbins.com
    Twitter: @mikefrobbins
#>

    [CmdletBinding()]
    param (

    )

    #Function Body

}
```

関数にコメント ベースのヘルプを追加すると、ユーザーが、既定の組み込みコマンドと同じようにヘルプを取得できます。

PowerShell で関数を記述するためのすべての構文に、特に初心者は圧倒されるかもしれません。 何かの構文を思い出せないときは、私はいつも別のモニターで ISE の 2 番目のコピーを開き、関数のコードを入力しながら、"Cmdlet (advanced function) - Complete" スニペットを表示します。 スニペットにアクセスするには、PowerShell ISE で <kbd>Ctrl</kbd> + <kbd>J</kbd> キーを使用します。

## <a name="summary"></a>まとめ

この章では、PowerShell での関数の記述に関する基本、たとえば、関数を高度な関数に変換する方法や、パラメーターの検証、詳細出力、パイプライン入力、エラー処理、コメントベースのヘルプなど、PowerShell 関数を記述するときに考慮する必要がある重要な要素について学習しました。

## <a name="review"></a>確認

1. PowerShell で承認されている動詞の一覧を取得するには、どうすればよいですか。
1. PowerShell 関数を高度な関数に変換するには、どうすればよいですか。
1. **WhatIf** パラメーターと **Confirm** パラメーターは、どのようなときに PowerShell 関数に追加する必要がありますか。
1. 終了しないエラーを終了するエラーに変換するには、どうすればよいですか。
1. コメント ベースのヘルプを関数に追加する必要があるのは、なぜですか。

## <a name="recommended-reading"></a>推奨トピック

- [about_Functions][]
- [about_Functions_Advanced_Parameters][]
- [about_CommonParameters][]
- [about_Functions_CmdletBindingAttribute][]
- [about_Functions_Advanced][]
- [about_Try_Catch_Finally][]
- [about_Comment_Based_Help][]
- [ビデオ: 高度な関数とスクリプト モジュールを使用した PowerShell でのツール作成][]

<!-- link references -->
[about_Functions]: /powershell/module/microsoft.powershell.core/about/about_functions
[about_Functions_Advanced_Parameters]: /powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters
[about_CommonParameters]: /powershell/module/microsoft.powershell.core/about/about_commonparameters
[about_Functions_CmdletBindingAttribute]: /powershell/module/microsoft.powershell.core/about/about_functions_cmdletbindingattribute
[about_Functions_Advanced]: /powershell/module/microsoft.powershell.core/about/about_functions_advanced
[about_Try_Catch_Finally]: /powershell/module/microsoft.powershell.core/about/about_try_catch_finally
[about_Comment_Based_Help]: /powershell/module/microsoft.powershell.core/about/about_comment_based_help
[ビデオ: 高度な関数とスクリプト モジュールを使用した PowerShell でのツール作成]: https://mikefrobbins.com/2016/05/26/video-powershell-toolmaking-with-advanced-functions-and-script-modules/
[パスカル ケース]: /dotnet/standard/design-guidelines/capitalization-conventions
