---
description: PowerShell でシーケンス内の次の文字を解釈する方法を制御する特殊文字シーケンスについて説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 02/08/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_special_characters?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Special_Characters
ms.openlocfilehash: 7e0ea9298dd85627c08298917464cb005f4f37ff
ms.sourcegitcommit: 364c3fe46b2069b810107d840be59fe519ea7b4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/10/2021
ms.locfileid: "100100735"
---
# <a name="about-special-characters"></a>特殊文字について

## <a name="short-description"></a>簡単な説明

PowerShell でシーケンス内の次の文字を解釈する方法を制御する特殊文字シーケンスについて説明します。

## <a name="long-description"></a>長い説明

PowerShell では、標準文字セットの一部ではない文字を表すために使用される特殊文字シーケンスのセットをサポートしています。 シーケンスは、一般に _エスケープシーケンス_ と呼ばれます。

エスケープシーケンスは、バックティック文字で始まり (ASCII 96)、大文字と小文字が区別されます。 バックティック文字は _エスケープ文字_ と呼ばれることもあります。

エスケープシーケンスは、二重引用符で囲まれた () 文字列に含まれている場合にのみ解釈され `"` ます。

PowerShell は、次のエスケープシーケンスを認識します。

|  Sequence   |       説明       |
| ----------- | ----------------------- |
| `` `0 ``    | Null                    |
| `` `a ``    | アラート:                   |
| `` `b ``    | バックスペース               |
| `` `f ``    | フォーム フィード               |
| `` `n ``    | 改行                |
| `` `r ``    | キャリッジ リターン         |
| `` `t ``    | 水平タブ          |
| `` `v ``    | 垂直タブ            |

PowerShell には、解析を停止する場所をマークする特別なトークンもあります。 このトークンの後に続くすべての文字は、解釈されないリテラル値として使用されます。

特別な解析トークン:

| Sequence |            説明             |
| -------- | ---------------------------------- |
| `--%`    | 次のいずれかの解析を停止します。 |

## <a name="null-0"></a>Null (' 0 ')

Null ( `` `0 `` ) 文字は、PowerShell の出力に空の領域として表示されます。
この機能により、PowerShell を使用して、文字列終了インジケーターやレコード終了インジケーターなど、null 文字を使用するテキストファイルの読み取りと処理を行うことができます。 Null 特殊文字は、 `$null` **null** 値を格納する変数と同じではありません。

## <a name="alert-a"></a>アラート (' a)

Alert ( `` `a `` ) 文字は、コンピューターのスピーカーにビープ音を送ります。
この文字を使用すると、ユーザーに対して、間もなく行われるアクションについて警告することができます。 次の例では、2つのビープ音信号をローカルコンピューターのスピーカーに送信します。

```powershell
for ($i = 0; $i -le 1; $i++){"`a"}
```

## <a name="backspace-b"></a>Backspace (' b)

Backspace ( `` `b `` ) 文字はカーソルを1文字前に移動しますが、文字は削除しません。

この例では、 **バックアップ** という単語を書き込み、カーソルを2回移動します。
次に、新しい位置で、スペースを書き込み、続けて「 **out**」と入力します。

```powershell
"backup`b`b out"
```

```Output
back out
```

## <a name="form-feed-f"></a>フォームフィード (' f)

Form feed ( `` `f `` ) 文字は、現在のページを取り出して次のページで印刷を続行する印刷命令です。 フォームフィード文字は、印刷されたドキュメントにのみ影響します。 画面出力には影響しません。

## <a name="new-line-n"></a>改行 (' n)

改行文字 ( `` `n `` ) を挿入すると、文字の直後に改行が挿入されます。

この例では、改行文字を使用してコマンドで改行を作成する方法を示し `Write-Host` ます。

```powershell
"There are two line breaks to create a blank line`n`nbetween the words."
```

```Output
There are two line breaks to create a blank line

between the words.
```

## <a name="carriage-return-r"></a>キャリッジリターン (' r)

キャリッジリターン ( `` `r `` ) 文字は、出力カーソルを現在の行の先頭に移動し、書き込みを続行します。 現在の行のすべての文字が上書きされます。

この例では、復帰前のテキストが上書きされます。

```powershell
Write-Host "These characters are overwritten.`rI want this text instead "
```

文字の前のテキストは削除されず、上書きされることに注意して `` `r `` ください。

```Output
I want this text instead written.
```

## <a name="horizontal-tab-t"></a>水平タブ (\)

水平タブ ( `` `t `` ) 文字は、次のタブストップに進み、その時点での書き込みを続行します。 既定では、PowerShell コンソールのタブは、8分ごとに停止します。

この例では、各列の間に2つのタブを挿入します。

```powershell
"Column1`t`tColumn2`t`tColumn3"
```

```Output
Column1         Column2         Column3
```

## <a name="vertical-tab-v"></a>垂直タブ (' v)

垂直タブ ( `` `v `` ) 文字は、次の垂直タブに進み、その時点で残りの出力を書き込みます。 垂直タブのレンダリングは、デバイスと端末に依存します。

```powershell
Write-Host "There is a vertical tab`vbetween the words."
```

次の例では、いくつかの一般的な環境で、垂直タブのレンダリングされた出力を示します。

Windows コンソールホストアプリケーションは ( `` `v `` ) を特殊文字として解釈しますが、余分な空白は追加されません。

```Output
There is a vertical tab♂between the words.
```

[Windows ターミナル](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701)は、垂直タブ文字をキャリッジリターンとラインフィードとしてレンダリングします。 出力の残りの部分は、次の行の先頭に印刷されます。

```Output
There is a vertical tab
between the words.
```

プリンターまたは unix ベースのコンソールでは、垂直タブ文字が次の行に進み、その時点で残りの出力を書き込みます。

```Output
There is a vertical tab
                       between the words.
```

## <a name="stop-parsing-token---"></a>ストップ解析トークン (--%)

ストップ解析 () トークンは、powershell が `--%` 文字列を powershell コマンドおよび式として解釈できないようにします。 これにより、これらの文字列を他のプログラムに渡して解釈することができます。

プログラム名の後に、エラーの原因となる可能性のあるプログラム引数の前に、ストップ解析トークンを配置します。

この例では、 `Icacls` コマンドはストップ解析トークンを使用します。

```powershell
icacls X:\VMS --% /grant Dom\HVAdmin:(CI)(OI)F
```

PowerShell は、次の文字列をに送信し `Icacls` ます。

```
X:\VMS /grant Dom\HVAdmin:(CI)(OI)F
```

別の例を示します。 **Shoの gs** 関数は、渡された値を出力します。 この例では、という名前の変数を `$HOME` 関数に2回渡します。

```powershell
function showArgs {
  "`$args = " + ($args -join '|')
}

showArgs $HOME --% $HOME
```

出力では、最初のパラメーターとして変数 `$HOME` が PowerShell によって解釈されるため、変数の値が関数に渡されます。 の2番目の使用は、 `$HOME` 解析の停止トークンの後にあるため、文字列 "$HOME" は解釈せずに関数に渡されます。

```Output
$args = C:\Users\username|--%|$HOME
```

ストップ解析トークンの詳細については、「 [about_Parsing](about_Parsing.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Quoting_Rules](about_Quoting_Rules.md)
