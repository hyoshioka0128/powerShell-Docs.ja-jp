---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/21/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-stringdata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-StringData
ms.openlocfilehash: c95ebca6307d58668c31faf3e492617f49ddebf4
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601413"
---
# ConvertFrom-StringData

## 概要
1 つまたは複数のキーと値のペアを含む文字列をハッシュ テーブルに変換します。

## SYNTAX

### すべて

```
ConvertFrom-StringData [-StringData] <String> [[-Delimiter] <Char>] [<CommonParameters>]
```

## Description

`ConvertFrom-StringData`コマンドレットは、1つ以上のキーと値のペアを含む文字列をハッシュテーブルに変換します。 各キーと値のペアは別々の行に配置する必要があるため、ここでは多くの場合、入力形式として文字列を使用します。 既定では、 **キー** は等号 () 文字で **値** から区切る必要があり `=` ます。

`ConvertFrom-StringData`コマンドレットは、 `DATA` スクリプトまたは関数のセクションで使用できる安全なコマンドレットと見なされます。 セクションで使用する場合、 `DATA` 文字列の内容は、データセクションの規則に準拠している必要があります。 詳細については、「 [about_Data_Sections](../Microsoft.PowerShell.Core/About/about_Data_Sections.md)」を参照してください。

`ConvertFrom-StringData` では、従来の機械翻訳ツールで許可されているエスケープ文字シーケンスがサポートされています。 つまり、コマンドレットでは、Unescape メソッドを使用して、文字列データ内のバックスラッシュ () をエスケープ文字として解釈できます。これは `\` 通常、 [](/dotnet/api/system.text.regularexpressions.regex.unescape) \` スクリプト内の行の末尾を示す PowerShell バックティック character () ではありません。 ヒア文字列内では、アクサン グラーブ文字は機能しません。 また、次のように、前の円記号でエスケープすることで、結果のリテラルバックスラッシュを保持することもできます `\\` 。 ファイルのパスでよく使用されるような、エスケープされていない円記号は、結果内で無効なエスケープ シーケンスとして扱われる可能性があります。

PowerShell 7 は、 **Delimiter** パラメーターを追加します。

## 例

### 例 1: 単一引用符で囲まれた文字列をハッシュテーブルに変換する

この例では、ユーザーメッセージの単一引用符で囲まれた文字列をハッシュテーブルに変換します。 単一引用符で囲まれた文字列内では、値は変数と置き換えられず、式も評価されません。
`ConvertFrom-StringData`コマンドレットは、変数の値を `$Here` ハッシュテーブルに変換します。

```powershell
$Here = @'
Msg1 = The string parameter is required.
Msg2 = Credentials are required for this command.
Msg3 = The specified variable does not exist.
'@
ConvertFrom-StringData -StringData $Here
```

```Output
Name                           Value
----                           -----
Msg3                           The specified variable does not exist.
Msg2                           Credentials are required for this command.
Msg1                           The string parameter is required.
```

### 例 2: 別の区切り記号を使用して文字列データを変換する

この例では、区切り記号として別の文字を使用する文字列データを変換する方法を示します。 この例では、文字列データは、区切り記号としてパイプ文字 () を使用してい `|` ます。

```powershell
$StringData = @'
color|red
model|coupe
year|1965
condition|mint
'@
$carData = ConvertFrom-StringData -StringData $StringData -Delimiter '|'
$carData
```

```Output
Name                           Value
----                           -----
condition                      mint
model                          coupe
color                          red
year                           1965
```

### 例 3: コメントを含む文字列を変換する

この例では、コメントと複数のキーと値のペアを含む文字列をハッシュテーブルに変換します。

```powershell
ConvertFrom-StringData -StringData @'
Name = Disks.ps1

# Category is optional.

Category = Storage
Cost = Free
'@
```

```Output
Name                           Value
----                           -----
Cost                           Free
Category                       Storage
Name                           Disks.ps1
```

**Convertfrom-stringdata** パラメーターの値は、ここで指定した文字列を含む変数ではなく、ここで指定した文字列です。 どちらの形式も有効です。 ヒア文字列には、文字列の 1 つについてのコメントが含まれています。
`ConvertFrom-StringData` 単一行コメントは無視されますが、 `#` 文字は行の最初の空白以外の文字である必要があります。 の後の行のすべての文字 `#` が無視されます。

### 例 4: 文字列をハッシュテーブルに変換する

この例では、通常の二重引用符で囲まれた文字列 (ここではない文字列) をハッシュテーブルに変換し、変数に保存し `$A` ます。

```powershell
$A = ConvertFrom-StringData -StringData "Top = Red `n Bottom = Blue"
$A
```

```Output
Name             Value
----             -----
Bottom           Blue
Top              Red
```

各キーと値のペアが別々の行に存在する必要があるという条件を満たすために、文字列は PowerShell の改行文字 ( \` n) を使用してペアを区切ります。

### 例 5: スクリプトの DATA セクションで ConvertFrom-StringData を使用する

この例は、 `ConvertFrom-StringData` スクリプトの DATA セクションで使用されるコマンドを示しています。
DATA セクションの下のステートメントによって、テキストがユーザーに表示されます。

```powershell
$TextMsgs = DATA {
ConvertFrom-StringData @'
Text001 = The $Notebook variable contains the name of the user's system notebook.
Text002 = The $MyNotebook variable contains the name of the user's private notebook.
'@
}
$TextMsgs
```

```Output
Name             Value
----             -----
Text001          The $Notebook variable contains the name of the user's system notebook.
Text002          The $MyNotebook variable contains the name of the user's private notebook.
```

テキストには変数名が含まれています。変数が展開されずにリテラルとして解釈されるようにするには、これを単一引用符の文字列で囲む必要があります。 **データ** セクションでは変数は使用できません。

### 例 6: パイプライン演算子を使用して文字列を渡す

この例は、パイプライン演算子 () を使用して、に文字列を送信できることを示して `|` `ConvertFrom-StringData` います。 変数の値 `$Here` がにパイプされ、 `ConvertFrom-StringData` 変数の結果になり `$Hash` ます。

```powershell
$Here = @'
Msg1 = The string parameter is required.
Msg2 = Credentials are required for this command.
Msg3 = The specified variable does not exist.
'@
$Hash = $Here | ConvertFrom-StringData
$Hash
```

```Output
Name     Value
----     -----
Msg3     The specified variable does not exist.
Msg2     Credentials are required for this command.
Msg1     The string parameter is required.
```

### 例 7: エスケープ文字を使用して新しい行と文字を追加する

この例では、エスケープ文字を使用して、ソースデータに新しい行を作成し、文字を返す方法を示します。 エスケープシーケンス `\n` は、結果のハッシュテーブル内の名前または項目に関連付けられたテキストのブロック内に新しい行を作成するために使用されます。

```powershell
ConvertFrom-StringData @"
Vincentio = Heaven doth with us as we with torches do,\nNot light them for themselves; for if our virtues\nDid not go forth of us, 'twere all alike\nAs if we had them not.
Angelo = Let there be some more test made of my metal,\nBefore so noble and so great a figure\nBe stamp'd upon it.
"@ | Format-List
```

```Output
Name  : Angelo
Value : Let there be some more test made of my metal,
        Before so noble and so great a figure
        Be stamp'd upon it.

Name  : Vincentio
Value : Heaven doth with us as we with torches do,
        Not light them for themselves; for if our virtues
        Did not go forth of us, 'twere all alike
        As if we had them not.
```

### 例 8: ファイルパスを正しく表示するには、円記号のエスケープ文字を使用します。

この例では、文字列データ内の円記号のエスケープ文字を使用して、結果のハッシュテーブルでファイルパスが正しくレンダリングされるようにする方法を示し `ConvertFrom-StringData` ます。 円記号を二重にすることにより、リテラルの円記号がハッシュ テーブルの出力で正しく表示されます。

```powershell
ConvertFrom-StringData "Message=Look in c:\\Windows\\System32"
```

```Output
Name                           Value
----                           -----
Message                        Look in c:\Windows\System32
```

## PARAMETERS

### -Convertfrom-stringdata

変換する文字列を指定します。 このパラメーターを使用するか、パイプを使用してに文字列を渡し `ConvertFrom-StringData` ます。 パラメーター名は省略可能です。

このパラメーターの値は、1つまたは複数のキーと値のペアを含む文字列である必要があります。 各キーと値のペアは別々の行に記述する必要があります。または、各ペアを改行文字 (n) で区切る必要があり \` ます。

文字列にコメントを含めることはできますが、コメントをキーと値のペアと同じ行に記述することはできません。 `ConvertFrom-StringData` 単一行コメントを無視します。 この `#` 文字は、行の空白以外の最初の文字である必要があります。 の後の行のすべての文字 `#` が無視されます。 コメントは、ハッシュ テーブルに含まれません。

ここでは、1つ以上の行で構成される文字列です。 ここで指定した文字列内の引用符は、文字列データの一部として文字どおり解釈されます。 詳細については、「 [about_Quoting_Rules](../Microsoft.PowerShell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -区切り記号

変換される文字列の **値** データから **キー** を区切るために使用される文字。
既定の区切り記号は等号 ( `=` ) です。 このパラメーターは、PowerShell 7 で追加されました。

```yaml
Type: System.Char
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: '='
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用して、キーと値のペアを含む文字列をにパイプすることができ `ConvertFrom-StringData` ます。

## 出力

### System.Collections.Hashtable

このコマンドレットは、キーと値のペアから作成されたハッシュテーブルを返します。

## 注

ヒア文字列は、1 つまたは複数の行から構成される文字列で、引用符はリテラルとして解釈されます。

このコマンドレットは、複数の音声言語でユーザーメッセージを表示するスクリプトで役に立ちます。 辞書形式のハッシュ テーブルを使用すると、リソース ファイルのようにコードからテキスト文字列を分離したり、翻訳ツールで使用するためにテキスト文字列を書式設定したりできます。

## 関連リンク

