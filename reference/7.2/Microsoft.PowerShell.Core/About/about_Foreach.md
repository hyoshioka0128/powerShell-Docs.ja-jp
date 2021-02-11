---
description: 項目のコレクション内のすべての項目を走査するために使用できる言語コマンドについて説明します。
Locale: en-US
ms.date: 02/27/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_foreach?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Foreach
ms.openlocfilehash: 2e9ca47a032834ff0c59a5c24f1f25c93d739e61
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599302"
---
# <a name="about-foreach"></a>ForEach の概要

## <a name="short-description"></a>簡単な説明
項目のコレクション内のすべての項目を走査するために使用できる言語コマンドについて説明します。

## <a name="long-description"></a>長い説明

`Foreach`ステートメント (ループとも呼ばれ `Foreach` ます) は、項目のコレクション内の一連の値をステップ実行する (反復処理する) 言語構成要素です。

走査する最も単純で一般的な種類のコレクションは配列です。
ループ内では `Foreach` 、配列内の各項目に対して1つ以上のコマンドを実行するのが一般的です。

## <a name="syntax"></a>構文

構文を次に示し `ForEach` ます。

```
foreach ($<item> in $<collection>){<statement list>}
```

かっこで囲まれたステートメントの部分は、 `ForEach` 反復処理する変数とコレクションを表します。 PowerShell `$<item>` は、ループの実行時に自動的に変数を作成し `Foreach` ます。 ループの各反復処理の前に、変数はコレクション内の値に設定されます。
ステートメントに続くブロックには、 `Foreach` `{<statement list>}` コレクション内の各項目に対して実行する一連のコマンドが含まれています。

### <a name="examples"></a>例

たとえば、次の例のループでは、 `Foreach` 配列の値が表示され `$letterArray` ます。

```powershell
$letterArray = "a","b","c","d"
foreach ($letter in $letterArray)
{
  Write-Host $letter
}
```

この例では、 `$letterArray` 配列が作成され、文字列値、、、およびで初期化され `"a"` `"b"` `"c"` `"d"` ます。 ステートメントを初めて実行すると、 `Foreach` 変数は `$letter` () の最初の項目と等しくなるように設定され `$letterArray` `"a"` ます。 次に、コマンドレットを使用して、a という `Write-Host` 文字を表示します。 次にループを実行するときは、を `$letter` に設定 `"b"` します。 ループが `Foreach` 文字 d を表示すると、PowerShell はループを終了します。

`Foreach`PowerShell コマンドプロンプトでコマンドとして実行するには、ステートメント全体が1行に記述されている必要があります。 代わりに、 `Foreach` ps1 スクリプトファイルにコマンドを配置する場合は、ステートメント全体を1行に記述する必要はありません。

`Foreach` ステートメントは、項目のコレクションを返すコマンドレットと共に使用することもできます。 次の例では、Foreach ステートメントによって、コマンドレットによって返される項目のリストがステップ実行され `Get-ChildItem` ます。

```powershell
foreach ($file in Get-ChildItem)
{
  Write-Host $file
}
```

ステートメントを使用して、返される結果を制限することで、この例を絞り込むことができ `If` ます。 次の例では、 `Foreach` ステートメントは前の例と同じループ操作を実行しますが、 `If` 結果を100キロバイト (kb) を超えるファイルに制限するステートメントを追加します。

```powershell
foreach ($file in Get-ChildItem)
{
  if ($file.length -gt 100KB)
  {
    Write-Host $file
  }
}
```

この例では、 `Foreach` ループは変数のプロパティを使用して `$file` 比較演算 () を実行し `$file.length -gt 100KB` ます。 変数には、 `$file` コマンドレットによって返されるオブジェクトのすべてのプロパティが含まれてい `Get-ChildItem` ます。 そのため、ファイル名だけを返すことができます。
次の例では、PowerShell は、ステートメントリスト内の長さと最後のアクセス時刻を返します。

```powershell
foreach ($file in Get-ChildItem)
{
  if ($file.length -gt 100KB)
  {
    Write-Host $file
    Write-Host $file.length
    Write-Host $file.lastaccesstime
  }
}
```

この例では、ステートメントリストで1つのコマンドを実行するだけではありません。

また、ループの外側で変数を使用 `Foreach` し、ループ内で変数をインクリメントすることもできます。 次の例では、ファイルのサイズが 100 KB を超えてカウントされます。

```powershell
$i = 0
foreach ($file in Get-ChildItem) {
  if ($file.length -gt 100KB) {
    Write-Host $file "file size:" ($file.length / 1024).ToString("F0") KB
    $i = $i + 1
  }
}

if ($i -ne 0) {
  Write-Host
  Write-Host $i " file(s) over 100 KB in the current directory."
}
else {
  Write-Host "No files greater than 100 KB in the current directory."
}
```

前の例では、 `$i` 変数がループの外部に設定され、 `0` 100 KB を超えるファイルが検出されるたびに、変数がループ内でインクリメントされます。 ループが終了すると、 `If` ステートメントによっての値が評価され、 `$i` すべてのファイルの数が 100 KB を超えて表示されます。 または、100 KB を超えるファイルが見つからなかったことを示すメッセージが表示されます。

前の例では、ファイルの長さの結果を書式設定する方法も示しています。

```powershell
($file.length / 1024).ToString("F0")
```

値は、バイトではなく、結果を kb 単位で示すために1024で除算され、結果の値は、結果から10進値を削除するために固定小数点書式指定子を使用して書式設定されます。 0は、書式指定子が小数点以下を表示しないようにします。

次の例では、定義された関数は、PowerShell スクリプトとスクリプトモジュールを解析し、内に含まれる関数の場所を返します。 この例では、 `MoveNext` メソッド (ループの場合と同様に動作する `skip X` `For` ) と、 `Current` `$foreach` foreach スクリプトブロック内の変数のプロパティを使用する方法を示します。 この例の関数では、複数の行にまたがる関数定義が異常であるか、または一貫性がない場合でも、スクリプト内の関数を見つけることができます。

詳細については、「 [列挙子の使用](about_Automatic_Variables.md#using-enumerators)」を参照してください。

```powershell
function Get-FunctionPosition {
  [CmdletBinding()]
  [OutputType('FunctionPosition')]
  param(
    [Parameter(Position = 0, Mandatory,
      ValueFromPipeline, ValueFromPipelineByPropertyName)]
    [ValidateNotNullOrEmpty()]
    [Alias('PSPath')]
    [System.String[]]
    $Path
  )

  process {
    try {
      $filesToProcess = if ($_ -is [System.IO.FileSystemInfo]) {
        $_
      } else {
        $filesToProcess = Get-Item -Path $Path
      }
      $parser = [System.Management.Automation.Language.Parser]
      foreach ($item in $filesToProcess) {
        if ($item.PSIsContainer -or
            $item.Extension -notin @('.ps1', '.psm1')) {
          continue
        }
        $tokens = $errors = $null
        $ast = $parser::ParseFile($item.FullName, ([REF]$tokens),
          ([REF]$errors))
        if ($errors) {
          $msg = "File '{0}' has {1} parser errors." -f $item.FullName,
            $errors.Count
          Write-Warning $msg
        }
        :tokenLoop foreach ($token in $tokens) {
          if ($token.Kind -ne 'Function') {
            continue
          }
          $position = $token.Extent.StartLineNumber
          do {
            if (-not $foreach.MoveNext()) {
              break tokenLoop
            }
            $token = $foreach.Current
          } until ($token.Kind -in @('Generic', 'Identifier'))
          $functionPosition = [pscustomobject]@{
            Name       = $token.Text
            LineNumber = $position
            Path       = $item.FullName
          }
          Add-Member -InputObject $functionPosition `
            -TypeName FunctionPosition -PassThru
        }
      }
    }
    catch {
      throw
    }
  }
}
```

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_If](about_If.md)

[ForEach-Object](xref:Microsoft.PowerShell.Core.ForEach-Object)

