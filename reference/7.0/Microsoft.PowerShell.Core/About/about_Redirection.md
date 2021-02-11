---
description: PowerShell からテキストファイルに出力をリダイレクトする方法について説明します。
Locale: en-US
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_redirection?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Redirection
ms.openlocfilehash: bc72f479650d67ed17b5fafef56565ccbebfea13
ms.sourcegitcommit: b9826dcf402db8a2b6d3eab37edb82c6af113343
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040867"
---
# <a name="about-redirection"></a>リダイレクトについて

## <a name="short-description"></a>簡単な説明
PowerShell からテキストファイルに出力をリダイレクトする方法について説明します。

## <a name="long-description"></a>長い説明

既定では、powershell は出力を PowerShell ホストに送信します。 通常、これはコンソールアプリケーションです。 ただし、出力をテキストファイルに送信したり、エラー出力を通常の出力ストリームにリダイレクトしたりすることはできます。

出力をリダイレクトするには、次の方法を使用できます。

- コマンドの `Out-File` 出力をテキストファイルに送信するコマンドレットを使用します。
  通常、 `Out-File` パラメーター (、、、など) を使用する必要がある場合は、コマンドレットを使用し `Encoding` `Force` `Width` `NoClobber` ます。

- `Tee-Object`コマンド出力をテキストファイルに送信し、それをパイプラインに送信するコマンドレットを使用します。

- PowerShell リダイレクト演算子を使用します。 リダイレクト演算子をファイルターゲットと共に使用することは、 `Out-File` 追加のパラメーターを使用せずにパイプを使用する場合と機能的に同等です。

ストリームの詳細については、「 [about_Output_Streams](about_Output_Streams.md)」を参照してください。

### <a name="redirectable-output-streams"></a>リダイレクトの出力ストリーム

PowerShell では、次の出力ストリームのリダイレクトがサポートされています。

| 一連# |      説明       | で導入  |    Write コマンドレット     |
| -------- | ---------------------- | -------------- | ------------------- |
| 1        | **成功** 一連     | PowerShell 2.0 | `Write-Output`      |
| 2        | **エラー** 一連       | PowerShell 2.0 | `Write-Error`       |
| 3        | **警告** 一連     | PowerShell 3.0 | `Write-Warning`     |
| 4        | **詳細** 一連     | PowerShell 3.0 | `Write-Verbose`     |
| 5        | **デバッグ** 一連       | PowerShell 3.0 | `Write-Debug`       |
| 6        | **情報** 一連 | PowerShell 5.0 | `Write-Information` |
| *        | すべてのストリーム            | PowerShell 3.0 |                     |

> [!NOTE]
> PowerShell には **進行状況** ストリームもありますが、リダイレクトはサポートされていません。

### <a name="powershell-redirection-operators"></a>PowerShell リダイレクト演算子

PowerShell リダイレクト演算子は次のようになります。ここで、は `n` ストリーム番号を表します。 ストリームが指定されていない場合、 **成功** のストリーム ( `1` ) が既定値になります。

| 演算子 |                         説明                         | 構文 |
| -------- | ----------------------------------------------------------- | ------ |
| `>`      | 指定されたストリームをファイルに送信します。                            | `n>`   |
| `>>`     | 指定されたストリームをファイルに **追加** します。                      | `n>>`  |
| `>&1`    | 指定されたストリームを **成功** ストリームに _リダイレクト_ します。 | `n>&1` |

> [!NOTE]
> 一部の Unix シェルとは異なり、他のストリームは **成功** ストリームにのみリダイレクトできます。

## <a name="examples"></a>使用例

### <a name="example-1-redirect-errors-and-output-to-a-file"></a>例 1: エラーと出力をファイルにリダイレクトする

この例は `dir` 、正常に実行される1つの項目に対して実行され、エラーになります。

```powershell
dir 'C:\', 'fakepath' 2>&1 > .\dir.log
```

を使用して、 `2>&1` **エラー** ストリームを **成功** ストリームにリダイレクトし、 `>` 結果の **成功** ストリームをという名前のファイルに送信します。 `dir.log`

### <a name="example-2-send-all-success-stream-data-to-a-file"></a>例 2: すべての成功したストリームデータをファイルに送信する

この例では、すべての **成功** ストリームデータをという名前のファイルに送信 `script.log` します。

```powershell
.\script.ps1 > script.log
```

### <a name="example-3-send-success-warning-and-error-streams-to-a-file"></a>例 3: 成功、警告、およびエラーストリームをファイルに送信する

この例では、リダイレクト演算子を組み合わせて目的の結果を得る方法を示します。

```powershell
&{
   Write-Warning "hello"
   Write-Error "hello"
   Write-Output "hi"
} 3>&1 2>&1 > C:\Temp\redirection.log
```

- `3>&1`**警告** ストリームを **成功** ストリームにリダイレクトします。
- `2>&1`**エラー** ストリームを **成功** ストリームにリダイレクトします (これには、すべての **警告** ストリームデータも含まれます)。
- `>`**成功** ストリーム (**警告** ストリームと **エラー** ストリームの両方が含まれています) をという名前のファイルにリダイレクトします `C:\temp\redirection.log` 。

### <a name="example-4-redirect-all-streams-to-a-file"></a>例 4: すべてのストリームをファイルにリダイレクトする

この例では、というスクリプトから、という名前のファイルにすべてのストリーム出力を送信します。 `script.ps1``script.log`

```powershell
.\script.ps1 *> script.log
```

### <a name="example-5-suppress-all-write-host-and-information-stream-data"></a>例 5: すべての Write-Host と情報ストリームデータを非表示にする

この例では、すべての情報ストリームデータを抑制します。 **情報** ストリームのコマンドレットの詳細については、「[書き込みホスト](xref:Microsoft.PowerShell.Utility.Write-Host)と [書き込み情報](xref:Microsoft.PowerShell.Utility.Write-Information)」を参照してください。

```powershell
&{
   Write-Host "Hello"
   Write-Information "Hello" -InformationAction Continue
} 6> $null
```

### <a name="example-6-show-the-effect-of-action-preferences"></a>例 6: アクションの基本設定の効果を表示する

アクションのユーザー設定変数とパラメーターを使用すると、特定のストリームに書き込まれる内容を変更できます。 この例のスクリプトは、の値が `$ErrorActionPreference` **エラー** ストリームに書き込まれる影響を示しています。

```powershell
$ErrorActionPreference = 'Continue'
$ErrorActionPreference > log.txt
get-item /not-here 2>&1 >> log.txt

$ErrorActionPreference = 'SilentlyContinue'
$ErrorActionPreference >> log.txt
get-item /not-here 2>&1 >> log.txt

$ErrorActionPreference = 'Stop'
$ErrorActionPreference >> log.txt
Try {
    get-item /not-here 2>&1 >> log.txt
}
catch {
    "`tError caught!" >> log.txt
}
$ErrorActionPreference = 'Ignore'
$ErrorActionPreference >> log.txt
get-item /not-here 2>&1 >> log.txt

$ErrorActionPreference = 'Inquire'
$ErrorActionPreference >> log.txt
get-item /not-here 2>&1 >> log.txt

$ErrorActionPreference = 'Continue'
```

このスクリプトを実行すると、がに設定されている場合にプロンプト `$ErrorActionPreference` が表示さ `Inquire` れます。

```powershell
PS C:\temp> .\test.ps1

Confirm
Cannot find path 'C:\not-here' because it does not exist.
[Y] Yes  [A] Yes to All  [H] Halt Command  [S] Suspend  [?] Help (default is "Y"): H
Get-Item: C:\temp\test.ps1:23
Line |
  23 |  get-item /not-here 2>&1 >> log.txt
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | The running command stopped because the user selected the Stop option.
```

ログファイルを確認すると、次の内容が表示されます。

```
PS C:\temp> Get-Content .\log.txt
Continue

Get-Item: C:\temp\test.ps1:3
Line |
   3 |  get-item /not-here 2>&1 >> log.txt
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Cannot find path 'C:\not-here' because it does not exist.

SilentlyContinue
Stop
    Error caught!
Ignore
Inquire
```

## <a name="notes"></a>注

データを追加しないリダイレクト演算子 ( `>` および) は、 `n>` 指定されたファイルの現在の内容を警告なしで上書きします。

ただし、ファイルが読み取り専用、非表示、またはシステムファイルの場合、リダイレクトは **失敗** します。 追加リダイレクト演算子 ( `>>` および) は、 `n>>` 読み取り専用ファイルには書き込みませんが、システムファイルまたは隠しファイルにコンテンツを追加します。

読み取り専用、非表示、またはシステムファイルにコンテンツを強制的にリダイレクトするには、 `Out-File` コマンドレットをパラメーターと共に使用し `Force` ます。

ファイルに書き込む場合、リダイレクト演算子は encoding を使用し `UTF8NoBOM` ます。 ファイルのエンコードが異なる場合は、出力の形式が正しくない可能性があります。 別のエンコーディングを使用してファイルに書き込むには、 `Out-File` コマンドレットをパラメーターと共に使用し `Encoding` ます。

### <a name="potential-confusion-with-comparison-operators"></a>比較演算子との混同の可能性

演算子は、 `>` [大なり](about_Comparison_Operators.md#-gt--ge--lt-and--le) 比較演算子と混同しないでください (多くの場合、 `>` 他のプログラミング言語でとして示されています)。

比較対象のオブジェクトによっては、を使用した出力が `>` 正しく表示されることがあります (36 は42を超えないため)。

```powershell
PS> if (36 > 42) { "true" } else { "false" }
false
```

ただし、ローカルのファイルシステムをチェックすると、という名前のファイルが `42` 内容と共に書き込まれたことがわかり `36` ます。

```powershell
PS> dir

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------          1/02/20  10:10 am              3 42

PS> cat 42
36
```

逆比較 (より小さい) を使用しようとすると `<` 、システムエラーが発生します。

```powershell
PS> if (36 < 42) { "true" } else { "false" }
ParserError:
Line |
   1 |  if (36 < 42) { "true" } else { "false" }
     |         ~
     | The '<' operator is reserved for future use.
```

数値比較が必要な操作である場合は、 `-lt` を `-gt` 使用する必要があります。 詳細については、 `-gt` [about_Comparison_Operators](about_Comparison_Operators.md#-gt--ge--lt-and--le)の演算子を参照してください。

## <a name="see-also"></a>関連項目

- [Out-File](xref:Microsoft.PowerShell.Utility.Out-File)
- [Tee-Object](xref:Microsoft.PowerShell.Utility.Tee-Object)
- [Write-Debug](xref:Microsoft.PowerShell.Utility.Write-Debug)
- [Write-Error](xref:Microsoft.PowerShell.Utility.Write-Error)
- [Write-Host](xref:Microsoft.PowerShell.Utility.Write-Host)
- [Write-Information](xref:Microsoft.PowerShell.Utility.Write-Information)
- [Write-Output](xref:Microsoft.PowerShell.Utility.Write-Output)
- [Write-Progress](xref:Microsoft.PowerShell.Utility.Write-Progress)
- [Write-Verbose](xref:Microsoft.PowerShell.Utility.Write-Verbose)
- [Write-Warning](xref:Microsoft.PowerShell.Utility.Write-Warning)
- [about_Output_Streams](about_Output_Streams.md)
- [about_Operators](about_Operators.md)
- [about_Command_Syntax](about_Command_Syntax.md)
- [about_Path_Syntax](about_Path_Syntax.md)
