---
description: PowerShell の and 演算子を使用したパイプラインのチェーンについて説明し `&&` `||` ます。
Locale: en-US
ms.date: 09/30/2019
schema: 2.0.0
title: about_Pipeline_Chain_Operators
ms.openlocfilehash: ece84ec061126401e53bf58112cd79a07a816e8d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601231"
---
# <a name="about-pipeline-chain-operators"></a>パイプラインチェーン演算子について

## <a name="short-description"></a>簡単な説明

PowerShell の and 演算子を使用したパイプラインのチェーンについて説明し `&&` `||` ます。

## <a name="long-description"></a>長い説明

Powershell 7 以降では、PowerShell は `&&` and 演算子を実装して、 `||` 条件に応じてパイプラインをチェーンします。 これらの演算子は、PowerShell で *パイプラインチェーン演算子* として知られています。また、bash、zsh および sh のような POSIX シェルの [and または list](https://pubs.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html#tag_02_09_03) と同様に、Windows コマンドシェル (cmd.exe) の [条件付き処理シンボル](/previous-versions/windows/it-pro/windows-xp/bb490954(v=technet.10)#using-multiple-commands-and-conditional-processing-symbols) にも似ています。

`&&` 演算子では、左側のパイプラインが成功した場合に、右側のパイプラインが実行されます。 反対に、`||` 演算子では、左側のパイプラインが失敗した場合に、右側のパイプラインが実行されます。

これらの演算子では `$?` 変数と `$LASTEXITCODE` の変数を使用して、パイプラインが失敗したかどうかが判断されます。 これにより、コマンドレットや関数だけでなく、ネイティブ コマンドでも使用できます。 次に例を示します。

```powershell
# Create an SSH key pair - if successful copy the public key to clipboard
ssh-keygen -t rsa -b 2048 && Get-Content -Raw ~\.ssh\id_rsa.pub | clip
```

### <a name="examples"></a>例

#### <a name="two-successful-commands"></a>成功した2つのコマンド

```powershell
Write-Output 'First' && Write-Output 'Second'
```

```Output
First
Second
```

#### <a name="first-command-fails-causing-second-not-to-be-executed"></a>最初のコマンドが失敗し、2番目のコマンドが実行されない

```powershell
Write-Error 'Bad' && Write-Output 'Second'
```

```Output
Write-Error: Bad
```

#### <a name="first-command-succeeds-so-the-second-command-is-not-executed"></a>最初のコマンドは成功します。そのため、2番目のコマンドは実行されません。

```powershell
Write-Output 'First' || Write-Output 'Second'
```

```Output
First
```

#### <a name="first-command-fails-so-the-second-command-is-executed"></a>最初のコマンドは失敗し、2番目のコマンドが実行されます。

```powershell
Write-Error 'Bad' || Write-Output 'Second'
```

```Output
Write-Error: Bad
Second
```

パイプラインの成功は、変数の値によって定義され `$?` ます。 PowerShell は、実行状態に基づいてパイプラインを実行した後に自動的に設定されます。
つまり、パイプラインチェーン演算子の等価性は次のようになります。

```powershell
Test-Command '1' && Test-Command '2'
```

と同じように動作します。

```powershell
Test-Command '1'; if ($?) { Test-Command '2' }
```

および

```powershell
Test-Command '1' || Test-Command '2'
```

と同じように動作します。

```powershell
Test-Command '1'; if (-not $?) { Test-Command '2' }
```

### <a name="assignment-from-pipeline-chains"></a>パイプラインチェーンからの割り当て

パイプラインチェーンから変数を割り当てると、チェーン内のすべてのパイプラインが連結されます。

```powershell
$result = Write-Output '1' && Write-Output '2'
$result
```

```Output
1
2
```

パイプラインチェーンからの割り当て中にスクリプト終了エラーが発生した場合、割り当ては成功しません。

```powershell
try
{
    $result = Write-Output 'Value' && $(throw 'Bad')
}
catch
{
    # Do nothing, just squash the error
}

"Result: $result"
```

```Output
Result:
```

### <a name="operator-syntax-and-precedence"></a>演算子の構文と優先順位

他の演算子とは異なり、 `&&` `||` はやなどの式ではなく、パイプラインを操作し `+` `-and` ます。

`&&` とは `||` 、パイプ () またはリダイレクト () よりも優先順位が低くなり `|` `>` ますが、ジョブ演算子 ( `&` )、割り当て ( `=` )、またはセミコロン () より優先順位が高くなり `;` ます。 つまり、パイプラインチェーン内のパイプラインは個別にリダイレクトできます。また、パイプラインチェーン全体を backgrounded、変数に代入、またはステートメントとして分離することができます。

パイプラインチェーン内で優先順位の低い構文を使用するには、かっこを使用することを検討してください `(...)` 。
同様に、パイプラインチェーン内にステートメントを埋め込むには、部分式を `$(...)` 使用できます。
これは、ネイティブコマンドと制御フローを組み合わせる場合に便利です。

```powershell
foreach ($file in 'file1','file2','file3')
{
    # When find succeeds, the loop breaks
    find $file && Write-Output "Found $file" && $(break)
}
```

```Output
find: file1: No such file or directory
file2
Found file2
```

PowerShell 7 の時点では、これらの構文の動作は変更されており、 `$?` かっこまたは部分式の中でコマンドが成功または失敗したときにが想定どおりに設定されるようになっています。

PowerShell の他のほとんどの演算子と同じように、 `&&` と `||` は *左から結合* されています。つまり、左からグループ化します。 次に例を示します。

```powershell
Get-ChildItem -Path ./file.txt || Write-Error "file.txt does not exist" && Get-Content -Raw ./file.txt
```

次のようにグループ化:

```
[Get-ChildItem -Path ./file.txt || Write-Error "file.txt does not exist"] && Get-Content -Raw ./file.txt
```

次と同じです。

```powershell
Get-ChildItem -Path ./file.txt

if (-not $?) { Write-Error "file.txt does not exist" }

if ($?) { Get-Content -Raw ./file.txt }
```

### <a name="error-interaction"></a>エラーの操作

パイプラインチェーン演算子は、エラーを吸収しません。 パイプラインチェーン内のステートメントがスクリプト終了エラーをスローすると、パイプラインチェーンは終了します。

次に例を示します。

```powershell
$(throw 'Bad') || Write-Output '2'
```

```Output
Exception: Bad
```

エラーがキャッチされても、パイプラインチェーンはまだ終了しています。

```powershell
try
{
    $(throw 'Bad') || Write-Output '2'
}
catch
{
    Write-Output "Caught: $_"
}
Write-Output 'Done'
```

```Output
Caught: Bad
Done
```

エラーが終了しない場合、またはパイプラインを終了した場合は、次の値を考慮してパイプラインチェーンが続行され `$?` ます。

```powershell
function Test-NonTerminatingError
{
    [CmdletBinding()]
    param()

    $exception = [System.Exception]::new('BAD')
    $errorId = 'BAD'
    $errorCategory = 'NotSpecified'

    $errorRecord = [System.Management.Automation.ErrorRecord]::new($exception, $errorId, $errorCategory, $null)

    $PSCmdlet.WriteError($errorRecord)
}

Test-NonTerminatingError || Write-Output 'Second'
```

```Output
Test-NonTerminatingError: BAD
Second
```

### <a name="chaining-pipelines-rather-than-commands"></a>コマンドではなくパイプラインのチェーン

パイプラインチェーン演算子は、その名前を使用して、コマンドだけではなく、パイプラインをチェーンすることができます。 これは他のシェルの動作と一致しますが、次のようにして *成功* を防ぐことができます。

```powershell
function Test-NotTwo
{
    [CmdletBinding()]
    param(
      [Parameter(ValueFromPipeline)]
      $Input
    )

    process
    {
        if ($Input -ne 2)
        {
            return $Input
        }

        $exception = [System.Exception]::new('Input is 2')
        $errorId = 'InputTwo'
        $errorCategory = 'InvalidData'

        $errorRecord = [System.Management.Automation.ErrorRecord]::new($exception, $errorId, $errorCategory, $null)

        $PSCmdlet.WriteError($errorRecord)
    }
}

1,2,3 | Test-NotTwo && Write-Output 'All done!'
```

```Output
1
Test-NotTwo : Input is 2
3
```

は `Write-Output 'All done!'` 実行されません `Test-NotTwo` 。これは、が終了しないエラーを生成した後に失敗したと見なされるためです。

## <a name="see-also"></a>関連項目

- [about_Operators](about_Operators.md)
- [about_Automatic_Variables](about_Automatic_Variables.md)
- [about_pipelines](about_pipelines.md)

