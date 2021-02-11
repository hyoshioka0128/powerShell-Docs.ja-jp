---
title: ShouldProcess について知りたかったことのすべて
description: ShouldProcess は、見過ごされることがよくある重要な機能です。 WhatIf と Confirm パラメーターを使用すると、関数に簡単に追加できます。
ms.date: 05/23/2020
ms.custom: contributor-KevinMarquette
ms.openlocfilehash: 4f11ad84f5c89423fe56cfe438ed3cb1587ce59e
ms.sourcegitcommit: be1df0bf757d734975a9aa021727608a396059ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2020
ms.locfileid: "96616048"
---
# <a name="everything-you-wanted-to-know-about-shouldprocess"></a>ShouldProcess について知りたかったことのすべて

PowerShell 関数には、ユーザーとの対話方法を大幅に向上させる機能がいくつか用意されています。
見過ごされることがよくある重要な機能の 1 つが、`-WhatIf` と `-Confirm` のサポートであり、簡単に関数に追加できます。 この記事では、この機能を実装する方法について詳しく説明します。

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@KevinMarquette][] 氏のブログ記事です。 このコンテンツを共有してくださった Kevin 氏に、PowerShell チームより感謝を申し上げます。 [PowerShellExplained.com][] のブログをご確認ください。

これは、セーフティ ネットを必要とするユーザーに対してそれを提供するために、関数で有効にできる単純な機能です。 危険な可能性があることがわかっているコマンドを初めて実行するときほど怖いことはありません。 これを `-WhatIf` で実行するオプションを使用すると、大きな違いが生まれます。

## <a name="commonparameters"></a>共通パラメーター

これらの[共通パラメーター][]の実装について説明する前に、その使用方法を簡単に見てみましょう。

## <a name="using--whatif"></a>-WhatIf を使用する

コマンドで `-WhatIf` パラメーターがサポートされている場合、コマンドがどのように実行されるかを、実際の変更を行わずに確認できます。 特に破壊的な操作を行う前には、この方法でコマンドの影響をテストすることをお勧めします。

```powershell
PS C:\temp> Remove-Item -Path .\myfile1.txt -WhatIf
What if: Performing the operation "Remove File" on target "C:\Temp\myfile1.txt".
```

コマンドによって `ShouldProcess` が正しく実装されている場合は、実行することになるすべての変更が表示されます。 ワイルドカードを使用して複数のファイルを削除する例を次に示します。

```powershell
PS C:\temp> Remove-Item -Path * -WhatIf
What if: Performing the operation "Remove File" on target "C:\Temp\myfile1.txt".
What if: Performing the operation "Remove File" on target "C:\Temp\myfile2.txt".
What if: Performing the operation "Remove File" on target "C:\Temp\importantfile.txt".
```

## <a name="using--confirm"></a>-Confirm を使用する

`-WhatIf` をサポートするコマンドは、`-Confirm` もサポートします。 これにより、実行前にアクションを確認できるようになります。

```powershell
PS C:\temp> Remove-Item .\myfile1.txt -Confirm

Confirm
Are you sure you want to perform this action?
Performing the operation "Remove File" on target "C:\Temp\myfile1.txt".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

この場合、続行、変更のスキップ、またはスクリプトの停止を行う複数のオプションがあります。 ヘルプ プロンプトでは、これらの各オプションが次のように説明されます。

```Output
Y - Continue with only the next step of the operation.
A - Continue with all the steps of the operation.
N - Skip this operation and proceed with the next operation.
L - Skip this operation and all subsequent operations.
S - Pause the current pipeline and return to the command prompt. Type "exit" to resume the pipeline.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

### <a name="localization"></a>ローカリゼーション

このプロンプトは PowerShell にローカライズされているので、お使いのオペレーティング システムの言語に基づいて言語が変化します。 これも、PowerShell によって管理されるものの 1 つです。

### <a name="switch-parameters"></a>スイッチ パラメーター

スイッチ パラメーターに値を渡す方法を簡単に見てみましょう。 これを呼び出す主な理由は、呼び出す関数にパラメーター値を渡すことが多いためです。

最初の方法は、すべてのパラメーターに使用できる特定のパラメーター構文ですが、ほとんどの場合、スイッチ パラメーターに使用されています。 パラメーターに値を付加するには、コロンを指定します。

```powershell
Remove-Item -Path:* -WhatIf:$true
```

変数でも同じ操作を行うことができます。

```powershell
$DoWhatIf = $true
Remove-Item -Path * -WhatIf:$DoWhatIf
```

2 つ目の方法では、ハッシュテーブルを使用して値をスプラッティングします。

```powershell
$RemoveSplat = @{
    Path = '*'
    WhatIf = $true
}
Remove-Item @RemoveSplat
```

ハッシュテーブルまたはスプラッティングを初めて使用する場合は、「[ハッシュテーブルについて知りたかったことのすべて][]」という別の記事を参照してください。

## <a name="supportsshouldprocess"></a>SupportsShouldProcess

`-WhatIf` と `-Confirm` のサポートを有効にするための最初の手順は、関数の `CmdletBinding` で `SupportsShouldProcess` を指定することです。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(SupportsShouldProcess)]
    param()
    Remove-Item .\myfile1.txt
}
```

この方法で `SupportsShouldProcess` を指定することで、`-WhatIf` (または `-Confirm`) を使用して関数を呼び出すことができるようになります。

```powershell
PS> Test-ShouldProcess -WhatIf
What if: Performing the operation "Remove File" on target "C:\Temp\myfile1.txt".
```

`-WhatIf` という名前のパラメーターを作成しなかったことに注意してください。 `SupportsShouldProcess` を指定すると、自動的に作成されます。 `Test-ShouldProcess` で `-WhatIf` パラメーターを指定すると、その他の呼び出しのいくつかでも `-WhatIf` 処理が実行されます。

### <a name="trust-but-verify"></a>信頼するが検証する

呼び出すものすべてが `-WhatIf` の値を継承すると信頼することは危険です。 例の残りでは、これは動作しないという前提に立ち、他のコマンドを呼び出す場合には明示的に行うようにします。 同じようにすることをお勧めします。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(SupportsShouldProcess)]
    param()
    Remove-Item .\myfile1.txt -WhatIf:$WhatIfPreference
}
```

この微妙な違いについては、全体像をつかんだ後に、詳しく説明します。

## <a name="pscmdletshouldprocess"></a>$PSCmdlet.ShouldProcess

`SupportsShouldProcess` の実装を可能にするメソッドは `$PSCmdlet.ShouldProcess` です。 `$PSCmdlet.ShouldProcess(...)` を呼び出して、ロジックを処理する必要があるかどうかを確認すると、残りは PowerShell によって処理されます。 例を見てみましょう。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(SupportsShouldProcess)]
    param()

    $file = Get-ChildItem './myfile1.txt'
    if($PSCmdlet.ShouldProcess($file.Name)){
        $file.Delete()
    }
}
```

`$PSCmdlet.ShouldProcess($file.name)` を呼び出すと、`-WhatIf` (および `-Confirm` パラメーター) がチェックされ、それに応じて処理されます。 `-WhatIf` によって、`ShouldProcess` は変更の説明を出力し、`$false` を返します。

```powershell
PS> Test-ShouldProcess -WhatIf
What if: Performing the operation "Test-ShouldProcess" on target "myfile1.txt".
```

`-Confirm` を使用した呼び出しでは、スクリプトが一時停止され、ユーザーに続行するオプションを示すプロンプトが表示されます。 ユーザーが `Y` を選択した場合は `$true` が返されます。

```powershell
PS> Test-ShouldProcess -Confirm
Confirm
Are you sure you want to perform this action?
Performing the operation "Test-ShouldProcess" on target "myfile1.txt".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

`$PSCmdlet.ShouldProcess` の優れた機能は、詳細出力として 2 倍になることです。 `ShouldProcess` を実装するときは、これを頻繁に行います。

```powershell
PS> Test-ShouldProcess -Verbose
VERBOSE: Performing the operation "Test-ShouldProcess" on target "myfile1.txt".
```

### <a name="overloads"></a>オーバーロード

`$PSCmdlet.ShouldProcess` には、メッセージングをカスタマイズするためのさまざまなパラメーターを持ついくつかのオーバーロードがあります。 既に前の例で、最初のものを紹介しました。 詳しく見てみましょう。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(SupportsShouldProcess)]
    param()

    if($PSCmdlet.ShouldProcess('TARGET')){
        # ...
    }
}
```

これにより、関数名とターゲット (パラメーターの値) の両方を含む出力が生成されます。

```powershell
What if: Performing the operation "Test-ShouldProcess" on target "TARGET".
```

2 番目のパラメーターを操作として指定すると、メッセージ内で関数名の代わりに操作値が使用されます。

```powershell
## $PSCmdlet.ShouldProcess('TARGET','OPERATION')
What if: Performing the operation "OPERATION" on target "TARGET".
```

次のオプションでは、メッセージを完全にカスタマイズするために 3 つのパラメーターを指定します。 3 つのパラメーターを使用する場合、最初のパラメーターはメッセージ全体を示します。 2 番目の 2 つのパラメーターは、`-Confirm` メッセージ出力でも引き続き使用されます。

```powershell
## $PSCmdlet.ShouldProcess('MESSAGE','TARGET','OPERATION')
What if: MESSAGE
```

### <a name="quick-parameter-reference"></a>クイック パラメーター リファレンス

使用する必要があるパラメーターを確認するだけにこの記事を参照してくる場合は、さまざまな `-WhatIf` シナリオでパラメーターがどのようにメッセージを変更するかを示すクイック リファレンスをご覧ください。

```powershell
## $PSCmdlet.ShouldProcess('TARGET')
What if: Performing the operation "FUNCTION_NAME" on target "TARGET".

## $PSCmdlet.ShouldProcess('TARGET','OPERATION')
What if: Performing the operation "OPERATION" on target "TARGET".

## $PSCmdlet.ShouldProcess('MESSAGE','TARGET','OPERATION')
What if: MESSAGE
```

2 つのパラメーターを持つものが使用される傾向があります。

### <a name="shouldprocessreason"></a>ShouldProcessReason

他よりも高度な 4 番目のオーバーロードがあります。 これにより、`ShouldProcess` が実行された理由を取得できます。 これは単に網羅するために追加しています。代わりに `$WhatIfPreference` が `$true` かどうかを確認できるためです。

```powershell
$reason = ''
if($PSCmdlet.ShouldProcess('MESSAGE','TARGET','OPERATION',[ref]$reason)){
    Write-Output "Some Action"
}
$reason
```

`[ref]` を持つ参照変数として、`$reason` 変数を 4 番目のパラメーターに渡す必要があります。 `ShouldProcess` によって、値 `None` または `WhatIf` を使用して `$reason` が設定されます。 これは特に便利なわけではなく、使用する理由もありません。

### <a name="where-to-place-it"></a>どこに配置するか

`ShouldProcess` を使用するのは、スクリプトの安全性を高めるためです。 そのため、スクリプトで変更を行うときに使用します。 `$PSCmdlet.ShouldProcess` の呼び出しをできるだけ変更の近くに配置するのが良いでしょう。

```powershell
## general logic and variable work
if ($PSCmdlet.ShouldProcess('TARGET','OPERATION')){
    # Change goes here
}
```

項目のコレクションを処理する場合は、項目ごとに呼び出します。 そのため、この呼び出しは foreach ループ内に配置されます。

```powershell
foreach ($node in $collection){
    # general logic and variable work
    if ($PSCmdlet.ShouldProcess($node,'OPERATION')){
        # Change goes here
    }
}
```

`ShouldProcess` を変更のなるべく近くに配置する理由は、`-WhatIf` が指定されたときに、可能な限り多くのコードを実行したいためです。 可能であればセットアップと検証を実行して、ユーザーがこれらのエラーを確認できるようにします。

また、プロジェクトを検証する Pester テストでこれを使用することもお勧めです。 Pester でモックを作成するのが難しいロジックがある場合、多くの場合、それを `ShouldProcess` にラップして、テストで `-WhatIf` を使用して呼び出すことができます。 少なくともコードの一部をテストすることをお勧めします。

### <a name="whatifpreference"></a>$WhatIfPreference

最初のユーザー設定変数は `$WhatIfPreference` です。 これは既定では `$false` です。 `$true` に設定した場合は、`-WhatIf` を指定したかのように関数が実行されます。 セッションでこれを設定した場合、すべてのコマンドで `-WhatIf` が実行されます。

`-WhatIf` を指定して関数を呼び出すと、関数のスコープ内で `$WhatIfPreference` の値が `$true` に設定されます。

## <a name="confirmimpact"></a>ConfirmImpact

この例のほとんどは `-WhatIf` を対象としていますが、これまでのすべての例は `-Confirm` でも機能し、ユーザーにメッセージを表示します。 関数の `ConfirmImpact` を High に設定すると、`-Confirm` で呼び出されたかのようにユーザーにプロンプトが表示されます。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(
        SupportsShouldProcess,
        ConfirmImpact = 'High'
    )]
    param()

    if ($PSCmdlet.ShouldProcess('TARGET')){
        Write-Output "Some Action"
    }
}
```

この `Test-ShouldProcess` の呼び出しでは、`High` の影響によって `-Confirm` アクションが実行されます。

```powershell
PS> Test-ShouldProcess

Confirm
Are you sure you want to perform this action?
Performing the operation "Test-ShouldProcess" on target "TARGET".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y
Some Action
```

明らかな問題は、ユーザーにプロンプトを表示することなく他のスクリプトで使用するのが困難になることです。 この場合は、`-Confirm` に `$false` を渡して、プロンプトを非表示にすることができます。

```powershell
PS> Test-ShouldProcess -Confirm:$false
Some Action
```

`-Force` サポートを追加する方法については、後のセクションで説明します。

### <a name="confirmpreference"></a>$ConfirmPreference

`$ConfirmPreference` は、`ConfirmImpact` によりユーザーにいつ実行の確認を求めるかを制御する自動変数です。 `$ConfirmPreference` と `ConfirmImpact` の両方で使用可能な値を次に示します。

- `High`
- `Medium`
- `Low`
- `None`

これらの値を使用すると、関数ごとに異なるレベルの影響を指定できます。 `$ConfirmPreference` を `ConfirmImpact` よりも高い値に設定した場合、実行の確認を求めるプロンプトが表示されません。

既定では、`$ConfirmPreference` は `High` に、`ConfirmImpact` は `Medium` に設定されます。 関数でユーザーに自動的にプロンプトが表示されるようにするには、`ConfirmImpact` を `High` に設定します。 それ以外の場合、破壊的な場合は `Medium` に設定し、コマンドが常に運用環境で安全に実行されている場合は `Low` に設定します。 `none` に設定すると、`-Confirm` が指定されている場合でもプロンプトは表示されません (ただし、`-WhatIf` は引き続きサポートされます)。

`-Confirm` を指定して関数を呼び出すと、関数のスコープ内で `$ConfirmPreference` の値が `Low` に設定されます。

### <a name="suppressing-nested-confirm-prompts"></a>入れ子になった確認プロンプトの抑制

`$ConfirmPreference` は、呼び出す関数によって取得できます。 これにより、確認プロンプトを追加したときに、呼び出した関数によってもユーザーにプロンプトが表示される場合があります。

よくある手法としては、プロンプトを既に処理しているときに呼び出すコマンドに `-Confirm:$false` を指定します。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(SupportsShouldProcess)]
    param()

    $file = Get-ChildItem './myfile1.txt'
    if($PSCmdlet.ShouldProcess($file.Name)){
        Remove-Item -Path $file.FullName -Confirm:$false
    }
}
```

ここで、前に説明した警告に戻ります。`-WhatIf` が関数に渡されない場合と、`-Confirm` が関数に渡される場合には、微妙な違いがあります。 これについては後で説明します。

## <a name="pscmdletshouldcontinue"></a>$PSCmdlet.ShouldContinue

`ShouldProcess` よりも多くの制御が必要な場合は、`ShouldContinue` を使用してプロンプトを直接トリガーできます。 `ShouldContinue` では、実行されるたびにプロンプトが表示されるので、`$ConfirmPreference`、`ConfirmImpact`、`-Confirm`、`$WhatIfPreference`、および `-WhatIf` は無視されます。

一見すると、`ShouldProcess` と `ShouldContinue` は混同しがちです。 パラメーター名は `CmdletBinding` で `SupportsShouldProcess` であるため、`ShouldProcess` を使用するように記憶する傾向があります。
ほとんどすべてのシナリオでは `ShouldProcess` を使用する必要があります。 そのため、最初にその方法を説明しました。

`ShouldContinue` の動作について見てみましょう。

```powershell
function Test-ShouldContinue {
    [CmdletBinding()]
    param()

    if($PSCmdlet.ShouldContinue('TARGET','OPERATION')){
        Write-Output "Some Action"
    }
}
```

これは、より少ないオプションの簡単なプロンプトを提供します。

```powershell
Test-ShouldContinue

Second
TARGET
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
```

`ShouldContinue` の最大の問題は、常にユーザーにプロンプトが表示されるため、ユーザーが対話形式で実行する必要があることです。 他のスクリプトで使用できるツールを常に作成しておく必要があります。 その方法は、`-Force` を実装することです。 このアイデアについては後で説明します。

### <a name="yes-to-all"></a>すべてはい

これは `ShouldProcess` では自動的に処理されますが、`ShouldContinue` については多少の処理が必要になります。 ロジックを制御するために参照渡しによっていくつかの値を渡す必要がある 2 番目のメソッド オーバーロードがあります。

```powershell
function Test-ShouldContinue {
    [CmdletBinding()]
    param()

    $collection = 1..5
    $yesToAll = $false
    $noToAll = $false

    foreach($target in $collection) {

        $continue = $PSCmdlet.ShouldContinue(
                "TARGET_$target",
                'OPERATION',
                [ref]$yesToAll,
                [ref]$noToAll
            )

        if ($continue){
            Write-Output "Some Action [$target]"
        }
    }
}
```

`foreach` ループとコレクションを追加して、その動作を示しています。 読みやすくするために、`if` ステートメントから `ShouldContinue` 呼び出しを抜き出してあります。 4 つのパラメーターを使用してメソッドを呼び出すと、少し見にくくなりますが、できる限り見やすい状態にしてあります。

## <a name="implementing--force"></a>-Force の実装

`ShouldProcess` と `ShouldContinue` では、`-Force` を異なる方法で実装する必要があります。 これらの実装に対する注意点として、`ShouldProcess` は常に実行される必要がありますが、`-Force` が指定されている場合 `ShouldContinue` は実行してはなりません。

### <a name="shouldprocess--force"></a>ShouldProcess -Force

`ConfirmImpact` を `high` に設定した場合、ユーザーが最初に試みるのは、`-Force` を使用してそれを抑制することです。 まずはそれを実行してみましょう。

```powershell
Test-ShouldProcess -Force
Error: Test-ShouldProcess: A parameter cannot be found that matches parameter name 'force'.
```

`ConfirmImpact` のセクションで説明したように、実際には次のように呼び出す必要があります。

```powershell
Test-ShouldProcess -Confirm:$false
```

これを行う必要があることに気が付かない人もおり、また `-Force` により `ShouldContinue` は抑制されません。
そのため、ユーザーのために `-Force` を実装する必要があります。 次の完全な例を見てみましょう。

```powershell
function Test-ShouldProcess {
    [CmdletBinding(
        SupportsShouldProcess,
        ConfirmImpact = 'High'
    )]
    param(
        [Switch]$Force
    )

    if ($Force){
        $ConfirmPreference = 'None'
    }

    if ($PSCmdlet.ShouldProcess('TARGET')){
        Write-Output "Some Action"
    }
}
```

独自の `-Force` スイッチをパラメーターとして追加します。 `CmdletBinding` で `SupportsShouldProcess` を使用すると、`-Confirm` パラメーターが自動的に追加されます。

```powershell
[CmdletBinding(
    SupportsShouldProcess,
    ConfirmImpact = 'High'
)]
param(
    [Switch]$Force
)
```

ここでは、`-Force` ロジックに焦点を絞っています。

```powershell
if ($Force){
    $ConfirmPreference = 'None'
}
```

ユーザーが `-Force` を指定した場合は、`-Confirm` も指定されていない限り、確認プロンプトを抑制します。 これにより、ユーザーは変更を強制することができますが、引き続き変更を確認できます。 次に、ローカル スコープで `$ConfirmPreference` を設定します。 次に、`-Force` パラメーターを使用することで `$ConfirmPreference` を一時的に "なし" に設定します。確認のプロンプトが無効になります。

```powershell
if ($Force -or $PSCmdlet.ShouldProcess('TARGET')){
        Write-Output "Some Action"
    }
```

`-Force` と `-WhatIf` の両方が指定されている場合は、`-WhatIf` を優先する必要があります。 この方法では、`ShouldProcess` が常に実行されるため、`-WhatIf` の処理が保持されます。

`ShouldProcess` が含まれる `if` ステートメントの中に `$Force` 値のチェックを追加しないでください。 これは、この特定のシナリオの場合のアンチパターンですが、次のセクションの `ShouldContinue` でこれについて説明します。

### <a name="shouldcontinue--force"></a>ShouldContinue -Force

これは、`ShouldContinue` で `-Force` を実装するための正しい方法です。

```powershell
function Test-ShouldContinue {
    [CmdletBinding()]
    param(
        [Switch]$Force
    )

    if($Force -or $PSCmdlet.ShouldContinue('TARGET','OPERATION')){
        Write-Output "Some Action"
    }
}
```

`-or` 演算子の左側に `$Force` を配置することで、最初に評価が行われます。 このように記述すると、`if` ステートメントの実行を省くことができます。 `$force` が `$true` の場合、`ShouldContinue` は実行されません。

```powershell
PS> Test-ShouldContinue -Force
Some Action
```

このシナリオでは `-Confirm` や `-WhatIf` について考慮する必要はありません。なぜなら、これらは `ShouldContinue` でサポートされていないためです。 このため、`ShouldProcess` とは異なる方法で処理する必要があります。

## <a name="scope-issues"></a>スコープの問題

`-WhatIf` と `-Confirm` の使用は、関数内のすべて、およびそれらが呼び出すすべてに適用することが想定されています。 これを行うには、関数のローカル スコープ内で `$WhatIfPreference` を `$true` に、または `$ConfirmPreference` を`Low` に設定します。 別の関数を呼び出すと、`ShouldProcess` の呼び出しでそれらの値が使用されます。

これは、ほとんどの場合、実際に正常に動作します。 組み込みのコマンドレットまたは関数を同じスコープ内で呼び出すときは、いつでも機能します。 また、コンソールからスクリプト モジュール内のスクリプトまたは関数を呼び出す場合にも機能します。

これが動作しない特定の場合は、スクリプトまたはスクリプト モジュールが別のスクリプト モジュールの関数を呼び出す場合です。 これは大きな問題でないように感じるかもしれませんが、PSGallery から作成したりプルしたりするモジュールのほとんどは、スクリプト モジュールです。

主な問題は、スクリプト モジュールは、他のスクリプト モジュールの関数から呼び出されたときに、`$WhatIfPreference` および `$ConfirmPreference` (またその他いくつか) の値を継承しないことです。

これを一般的な規則としてうまくまとめると、バイナリ モジュールに対しては正しく機能するが、スクリプト モジュールの場合は決して信頼しないことです。 確信がない場合は、テストするか、正しく動作しないと想定してください。

これは、分離されていると正しく動作する複数のモジュールに `-WhatIf` サポートを追加し、これらが互いを呼び出すと正しく機能しないというシナリオが発生するため、非常に危険です。

この問題を解決するために、GitHub の RFC が存在します。 詳細については、[スクリプト モジュールのスコープを超えた実行設定の伝達][RFC]に関する記事を参照してください。

## <a name="in-closing"></a>最後に

`ShouldProcess` 使用する必要があるたびに、その使用方法を確認する必要あります。 `ShouldProcess` と `ShouldContinue` を区別するために長い時間がかかりました。 ほとんどの場合、使用するパラメーターを調べる必要があります。 混乱することがあってもあまり気にしないでください。 必要なときには、この記事がここにあります。 私自身も頻繁にこれを参照することになるでしょう。

この投稿を気にっていただけた場合は、以下のリンクを使用して、Twitter でご意見をお聞かせください。 コンテンツの価値を評価していただける方からのご意見は常に歓迎いたします。

<!-- link references -->
[オリジナル バージョン]: https://powershellexplained.com/2020-03-15-Powershell-shouldprocess-whatif-confirm-shouldcontinue-everything/
[powershellexplained.com]: https://powershellexplained.com/
[@KevinMarquette]: https://twitter.com/KevinMarquette
[共通パラメーター]: /powershell/module/microsoft.powershell.core/about/about_commonparameters
[ハッシュテーブルについて知りたかったことのすべて]: everything-about-hashtable.md
[RFC]: https://github.com/PowerShell/PowerShell-RFC/pull/221#issuecomment-592954839
