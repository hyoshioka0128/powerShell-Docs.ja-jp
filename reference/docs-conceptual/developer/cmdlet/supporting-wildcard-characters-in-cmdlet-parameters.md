---
ms.date: 08/26/2019
ms.topic: reference
title: コマンドレット パラメーターでワイルドカード文字をサポートする
description: コマンドレット パラメーターでワイルドカード文字をサポートする
ms.openlocfilehash: 06693c62cd2613050bdeb9d6b12ad6e9597a9894
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646398"
---
# <a name="supporting-wildcard-characters-in-cmdlet-parameters"></a>コマンドレット パラメーターでワイルドカード文字をサポートする

多くの場合、1つのリソースに対してではなく、リソースのグループに対して実行するようにコマンドレットを設計する必要があります。 たとえば、コマンドレットでは、同じ名前または拡張子を持つデータストア内のすべてのファイルを検索することが必要になる場合があります。 リソースグループに対して実行されるコマンドレットを設計するときは、ワイルドカード文字のサポートを提供する必要があります。

> [!NOTE]
> ワイルドカード文字の使用は、 *グロビング* と呼ばれることもあります。

## <a name="windows-powershell-cmdlets-that-use-wildcards"></a>ワイルドカードを使用する Windows PowerShell コマンドレット

 多くの Windows PowerShell コマンドレットは、パラメーター値としてワイルドカード文字をサポートしています。 たとえば、パラメーターまたはパラメーターを持つほとんどすべてのコマンドレットは、 `Name` `Path` これらのパラメーターに対してワイルドカード文字をサポートしています。 (パラメーターを持つほとんどのコマンドレットには `Path` 、 `LiteralPath` ワイルドカード文字をサポートしないパラメーターもあります)。次のコマンドは、名前に Get 動詞が含まれている現在のセッションのすべてのコマンドレットを返すために、ワイルドカード文字を使用する方法を示しています。

 `Get-Command get-*`

## <a name="supported-wildcard-characters"></a>サポートされているワイルドカード文字

Windows PowerShell では、次のワイルドカード文字がサポートされています。

| ワイルドカード |                             説明                             |  例   |     ［一致する］      | ［次の値に一致しない］ |
| -------- | ------------------------------------------------------------------- | ---------- | ---------------- | -------------- |
| *        | 指定された位置を開始位置として、0個以上の文字と一致します | `a*`       | A、ag、Apple     |                |
| ?        | 指定した位置にある任意の文字と一致します                     | `?n`       | 、In、on       | 済み            |
| [ ]      | 文字の範囲に一致します。                                       | `[a-l]ook` | 書籍、クック、ルック | nook、所要時間     |
| [ ]      | 指定した文字と一致します                                    | `[bn]ook`  | book、nook       | クック、外観     |

ワイルドカード文字をサポートするコマンドレットを設計する場合は、ワイルドカード文字の組み合わせを許可します。 たとえば、次のコマンドは、コマンドレットを使用して、 `Get-ChildItem` c:\ の docs フォルダー内にあり、"a" ~ "l" の文字で始まるすべての .txt ファイルを取得します。

`Get-ChildItem c:\techdocs\[a-l]\*.txt`

前のコマンドでは、範囲のワイルドカードを使用して、 `[a-l]` ファイル名の先頭文字が "a" ~ "l" であることを指定します。また、 `*` ワイルドカード文字を、ファイル名の最初の文字と **.txt** 拡張子の間にある任意の文字のプレースホルダーとして使用します。

次の例では、文字 "d" を除くが、"a" ~ "f" の他のすべての文字を含む、範囲のワイルドカードパターンを使用します。

`Get-ChildItem c:\techdocs\[a-cef]\*.txt`

## <a name="handling-literal-characters-in-wildcard-patterns"></a>ワイルドカードパターンでのリテラル文字の処理

指定したワイルドカードパターンに、ワイルドカード文字として interpretted ないリテラル文字が含まれている場合は、バックティック文字 ( `` ` `` ) をエスケープ文字として使用します。 PowerShell API のリテラル文字を指定する場合は、1つのバックティックを使用します。 PowerShell コマンドプロンプトでリテラル文字を指定する場合は、2つのバックティックを使用します。

たとえば、次のパターンには、文字どおりに実行する必要がある2つの角かっこが含まれています。

PowerShell API で使用する場合は、次のように使用します。

- "John Smith \` [* ']"

PowerShell コマンドプロンプトから使用する場合:

- "John Smith \` \` [* \` ']"

このパターンは、"John Smith [Marketing]" または "John Smith [Development]" と一致します。 次に例を示します。

```
PS> "John Smith [Marketing]" -like "John Smith ``[*``]"
True

PS> "John Smith [Development]" -like "John Smith ``[*``]"
True
```

## <a name="cmdlet-output-and-wildcard-characters"></a>コマンドレットの出力とワイルドカード文字

コマンドレットのパラメーターでワイルドカード文字がサポートされている場合、通常、この操作では配列の出力が生成されます。
場合によっては、ユーザーが1つの項目のみを使用する可能性があるため、配列の出力をサポートすることは意味がありません。 たとえば、 `Set-Location` コマンドレットでは、ユーザーが1つの場所のみを設定するため、配列の出力はサポートされません。 このインスタンスでは、コマンドレットはワイルドカード文字を引き続きサポートしますが、1つの場所を強制的に解決します。

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)

[WildcardPattern クラス](/dotnet/api/system.management.automation.wildcardpattern)
