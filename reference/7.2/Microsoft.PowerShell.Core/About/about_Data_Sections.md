---
description: テキスト文字列とその他の読み取り専用データをスクリプトロジックから分離するデータセクションについて説明します。
Locale: en-US
ms.date: 04/23/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_data_sections?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Data_Sections
ms.openlocfilehash: 1f2a0bae0721bc9adf5b3ba92d5be32d21306a46
ms.sourcegitcommit: 04faa7dc1122bce839295d4891bd8b2f0ecb06ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2021
ms.locfileid: "99600120"
---
# <a name="about-data-sections"></a>データセクションについて

## <a name="short-description"></a>簡単な説明
テキスト文字列とその他の読み取り専用データをスクリプトロジックから分離するデータセクションについて説明します。

## <a name="long-description"></a>長い説明

PowerShell 用に設計されたスクリプトには、データのみを含む1つまたは複数のデータセクションを含めることができます。 任意のスクリプト、関数、または高度な関数に1つ以上のデータセクションを含めることができます。 Data セクションの内容は、PowerShell スクリプト言語の指定したサブセットに制限されます。

コードロジックからデータを分離すると、ロジックとデータの両方を簡単に識別して管理できるようになります。 これにより、エラーメッセージやヘルプ文字列など、テキスト用の文字列リソースファイルを個別に作成できます。 また、コードロジックも分離されているため、セキュリティと検証のテストが容易になります。

PowerShell では、スクリプトの国際化をサポートするために Data セクションが使用されます。
データセクションを使用すると、多数のユーザーインターフェイス (UI) 言語に変換される文字列の分離、検索、および処理が容易になります。

Data セクションは PowerShell 2.0 の機能です。 Data セクションを含むスクリプトは、リビジョンのない PowerShell 1.0 では実行されません。

### <a name="syntax"></a>構文

データセクションの構文は次のとおりです。

```
DATA [<variable-name>] [-supportedCommand <cmdlet-name>] {
    <Permitted content>
}
```

Data キーワードが必要です。 大文字と小文字は区別されません。 許可されるコンテンツは、次の要素に限定されます。

- すべての PowerShell 演算子 (を除く) `-match`
- `If`、 `Else` 、および `ElseIf` ステートメント
- 次の自動変数: `$PsCulture` 、 `$PsUICulture` 、 `$True` 、 `$False` 、および `$Null`
- コメント
- パイプライン
- セミコロン () で区切られたステートメント `;`
- リテラル。次に例を示します。

  ```powershell
  a
  1
  1,2,3
  "PowerShell 2.0"
  @( "red", "green", "blue" )
  @{ a = 0x1; b = "great"; c ="script" }
  [XML] @'
  <p> Hello, World </p>
  '@
  ```

- データセクションで許可されているコマンドレット。 既定で `ConvertFrom-StringData` は、コマンドレットのみが許可されています。
- パラメーターを使用してデータセクションで許可するコマンドレット `-SupportedCommand` 。

データセクションでコマンドレットを使用する場合は、単一引用符で囲んだ文字列または二重引用符で囲まれた文字列で、 `ConvertFrom-StringData` キーと値のペアを囲むことができます。 ただし、変数と部分式を含む文字列は、単一引用符で囲まれた文字列または引用符で囲まれた文字列で囲む必要があります。これにより、変数が展開されず、部分式が実行可能ではなくなります。

### <a name="-supportedcommand"></a>-SupportedCommand

`-SupportedCommand`パラメーターを使用すると、コマンドレットまたは関数によってのみデータが生成されることを示すことができます。 これは、ユーザーが記述またはテストしたデータセクションにコマンドレットと関数を含めることができるように設計されています。

の値 `-SupportedCommand` は、1つまたは複数のコマンドレットまたは関数の名前のコンマ区切りのリストです。

たとえば、次の data セクションには、 `Format-Xml` データを XML ファイルにフォーマットするユーザー記述のコマンドレットが含まれています。

```powershell
DATA -supportedCommand Format-Xml
{
    Format-Xml -Strings string1, string2, string3
}
```

### <a name="using-a-data-section"></a>Data セクションの使用

データセクションの内容を使用するには、それを変数に割り当て、変数表記を使用してコンテンツにアクセスします。

たとえば、次の data セクションには、 `ConvertFrom-StringData` この文字列をハッシュテーブルに変換するコマンドが含まれています。 ハッシュテーブルは変数に割り当てられ `$TextMsgs` ます。

`$TextMsgs`変数は data セクションの一部ではありません。

```powershell
$TextMsgs = DATA {
    ConvertFrom-StringData -StringData @'
Text001 = Windows 7
Text002 = Windows Server 2008 R2
'@
}
```

でハッシュテーブルのキーと値にアクセスするには、 `$TextMsgs` 次のコマンドを使用します。

```powershell
$TextMsgs.Text001
$TextMsgs.Text002
```

また、変数名を Data セクションの定義に含めることもできます。 次に例を示します。

```powershell
DATA TextMsgs {
    ConvertFrom-StringData -StringData @'
Text001 = Windows 7
Text002 = Windows Server 2008 R2
'@
}

$TextMsgs
```

結果は前の例と同じです。

```Output
Name                           Value
----                           -----
Text001                        Windows 7
Text002                        Windows Server 2008 R2
```

### <a name="examples"></a>例

単純なデータ文字列。

```powershell
DATA {
    "Thank you for using my PowerShell Organize.pst script."
    "It is provided free of charge to the community."
    "I appreciate your comments and feedback."
}
```

許可される変数を含む文字列。

```powershell
DATA {
    if ($null) {
        "To get help for this cmdlet, type get-help new-dictionary."
    }
}
```

コマンドレットを使用する単一引用符で囲まれた文字列 `ConvertFrom-StringData` :

```powershell
DATA {
    ConvertFrom-StringData -stringdata @'
Text001 = Windows 7
Text002 = Windows Server 2008 R2
'@
}
```

ここでは、コマンドレットを使用する二重引用符で囲まれた文字列 `ConvertFrom-StringData` です。

```powershell
DATA  {
    ConvertFrom-StringData -stringdata @"
Msg1 = To start, press any key.
Msg2 = To exit, type "quit".
"@
}
```

データを生成するユーザー記述のコマンドレットを含むデータセクション。

```powershell
DATA -supportedCommand Format-XML {
    Format-Xml -strings string1, string2, string3
}
```

## <a name="see-also"></a>参照

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Hash_Tables](about_Hash_Tables.md)

[about_If](about_If.md)

[about_Operators](about_Operators.md)

[about_Quoting_Rules](about_Quoting_Rules.md)

[about_Script_Internationalization](about_Script_Internationalization.md)

[ConvertFrom-StringData](xref:Microsoft.PowerShell.Utility.ConvertFrom-StringData)

[Import-LocalizedData](xref:Microsoft.PowerShell.Utility.Import-LocalizedData)

