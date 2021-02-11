---
description: PowerShell 演算子を優先順位順に一覧表示します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/09/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_operator_precedence?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Operator_Precedence
ms.openlocfilehash: 27cc1be95067a38e6c210b37a3398416d0845846
ms.sourcegitcommit: 768816a5c05cc2d07ffd84bed95b0499f4b49f2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2020
ms.locfileid: "94483039"
---
# <a name="about-operator-precedence"></a>演算子の優先順位について

## <a name="short-description"></a>概要
PowerShell 演算子を優先順位順に一覧表示します。

## <a name="long-description"></a>詳細説明

PowerShell の演算子を使用すると、単純な式を作成できます。 このトピックでは、演算子を優先順位順に示します。 優先順位は、同じ式に複数の演算子が含まれている場合に、PowerShell が演算子を評価する順序です。

演算子の優先順位が同じである場合、PowerShell は式の中に表示されているとおりに左から右に評価します。 例外として、代入演算子、キャスト演算子、および否定演算子 ( `!` 、 `-not` 、) があり `-bnot` ます。これは右から左に評価されます。

かっこなどのエンクロージャを使用すると、標準の優先順位をオーバーライドし、PowerShell によって、囲まれていない部分の前に囲まれた式の部分を強制的に評価することができます。

次の一覧では、演算子は評価される順序で一覧表示されます。 同じ行または同じグループ内の演算子は、同じ優先順位を持ちます。

演算子列には、演算子が一覧表示されます。 [参照列には、演算子が記述されている PowerShell ヘルプトピックが一覧表示されます。 トピックを表示するには、「」と入力 `get-help <topic-name>` します。

|         OPERATOR         |           リファレンス            |
| ------------------------ | ------------------------------ |
| `$() @() () @{}`         | [about_Operators][]            |
| `.` (メンバーアクセス)      | [about_Operators][]            |
| `::` 雑音            | [about_Operators][]            |
| `[0]` (インデックス演算子)   | [about_Operators][]            |
| `[int]` (cast 演算子) | [about_Operators][]            |
| `-split` 前置         | [about_Split][]                |
| `-join` 前置          | [about_Join][]                 |
| `,` (コンマ演算子)     | [about_Operators][]            |
| `++ --`                  | [about_Assignment_Operators][] |
| `! -not`                 | [about_Logical_Operators][]    |
| `..` (範囲演算子)    | [about_Operators][]            |
| `-f` (format 演算子)   | [about_Operators][]            |
| `-` (単項/負)     | [about_Arithmetic_Operators][] |
| `* / %`                  | [about_Arithmetic_Operators][] |
| `+ -`                    | [about_Arithmetic_Operators][] |

次の演算子グループは、優先順位が同じです。 大文字小文字を区別し、大文字小文字を区別しないバリアントは、同じ優先順位を持ちます。

|         OPERATOR          |           リファレンス            |
| ------------------------- | ------------------------------ |
| `-split` バイナリ         | [about_Split][]                |
| `-join` バイナリ          | [about_Join][]                 |
| `-is -isnot -as`          | [about_Type_Operators][]       |
| `-eq -ne -gt -gt -lt -le` | [about_Comparison_Operators][] |
| `-like -notlike`          | [about_Comparison_Operators][] |
| `-match -notmatch`        | [about_Comparison_Operators][] |
| `-in -notIn`              | [about_Comparison_Operators][] |
| `-contains -notContains`  | [about_Comparison_Operators][] |
| `-replace`                | [about_Comparison_Operators][] |

リストは、次の演算子を優先順位順に使用してここで再開します。

|                OPERATOR                 |           リファレンス            |
| --------------------------------------- | ------------------------------ |
| `-band -bnot -bor -bxor -shr -shl`      | [about_Arithmetic_Operators][] |
| `-and -or -xor`                         | [about_Logical_Operators][]    |

次の項目は、真の演算子ではありません。 これらは、式の構文ではなく、PowerShell のコマンド構文に含まれています。 割り当ては常に最後に行われる操作です。

|                SYNTAX                   |           リファレンス            |
| --------------------------------------- | ------------------------------ |
| `.` (ドット-ソース)                        | [about_Operators][]            |
| `&` 発信                              | [about_Operators][]            |
| <code>&#124;</code> (パイプライン演算子) | [about_Operators][]            |
| `> >> 2> 2>> 2>&1`                      | [about_Redirection][]          |
| `= += -= *= /= %=`                      | [about_Assignment_Operators][] |

## <a name="examples"></a>例

次の2つのコマンドは、算術演算子と、かっこを使用して、式の囲まれた部分を最初に評価するように PowerShell に強制する効果を示しています。

```powershell
PS> 2 + 3 * 4
14

PS> (2 + 3) * 4
20
```

次の例では、ローカルディレクトリから読み取り専用のテキストファイルを取得し、変数に保存し `$read_only` ます。

```powershell
$read_only = Get-ChildItem *.txt | Where-Object {$_.isReadOnly}
```

これは、次の例と同じです。

```powershell
$read_only = ( Get-ChildItem *.txt | Where-Object {$_.isReadOnly} )
```

パイプライン演算子 () は `|` 代入演算子 () より優先順位が高いため `=` 、コマンドレットによって `Get-ChildItem` 取得されるファイルは、 `Where-Object` 変数に割り当てられる前に、フィルター処理のためにコマンドレットに送信され `$read_only` ます。

次の例では、index 演算子がキャスト演算子よりも優先されることを示しています。

この式は、3つの文字列の配列を作成します。 次に、値が0の index 演算子を使用して、配列内の最初のオブジェクト (最初の文字列) を選択します。 最後に、選択したオブジェクトを文字列としてキャストします。 この場合、キャストは無効です。

```powershell
PS> [string]@('Windows','PowerShell','2.0')[0]
Windows
```

この式では、かっこを使用して、インデックス選択の前にキャスト操作を強制的に実行します。 その結果、配列全体が (単一の) 文字列としてキャストされます。 次に、index 演算子は、文字列配列内の最初の項目 (最初の文字) を選択します。

```powershell
PS> ([string]@('Windows','PowerShell','2.0'))[0]
W
```

次の例では、 `-gt` (大なり) 演算子が (論理 and) 演算子よりも優先順位が高いため、 `-and` 式の結果は FALSE になります。

```powershell
PS> 2 -gt 4 -and 1
False
```

これは、次の式と同じです。

```powershell
PS> (2 -gt 4) -and 1
False
```

-And 演算子の優先順位が高い場合、答えは TRUE になります。

```powershell
PS> 2 -gt (4 -and 1)
True
```

ただし、この例では、演算子の優先順位を管理するための重要な原則を示しています。 式の解釈が難しい場合は、かっこを使用して、既定の演算子の優先順位を強制的に適用する場合でも、評価順序を強制します。 かっこを使用すると、スクリプトを読んで管理している人に対して意図が明確になります。

## <a name="see-also"></a>関連項目

[about_Operators][]

[about_Assignment_Operators][]

[about_Comparison_Operators][]

[about_Arithmetic_Operators][]

[about_Join][]

[about_Redirection][]

[about_Scopes][]

[about_Split][]

[about_Type_Operators][]

<!-- reference links -->
[about_Arithmetic_Operators]: about_Arithmetic_Operators.md
[about_Assignment_Operators]: about_Assignment_Operators.md
[about_Comparison_Operators]: about_Comparison_Operators.md
[about_Join]: about_Join.md
[about_Logical_Operators]: about_logical_operators.md
[about_Operators]: about_Operators.md
[about_Redirection]: about_Redirection.md
[about_Scopes]: about_Scopes.md
[about_Split]: about_Split.md
[about_Type_Operators]: about_Type_Operators.md
