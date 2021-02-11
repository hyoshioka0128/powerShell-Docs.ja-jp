---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Select-String
ms.openlocfilehash: 339afab9c817b54a79e2f1b33b7021ecb4fe6a6e
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600967"
---
# Select-String

## 概要
文字列とファイル内のテキストを検索します。

## SYNTAX

### File (既定値)

```
Select-String [-Culture <String>] [-Pattern] <String[]> [-Path] <String[]> [-SimpleMatch]
 [-CaseSensitive] [-Quiet] [-List] [-NoEmphasis] [-Include <String[]>] [-Exclude <String[]>]
 [-NotMatch] [-AllMatches] [-Encoding <Encoding>] [-Context <Int32[]>] [<CommonParameters>]
```

### ObjectRaw

```
Select-String [-Culture <String>] -InputObject <PSObject> [-Pattern] <String[]> -Raw [-SimpleMatch]
 [-CaseSensitive] [-List] [-NoEmphasis] [-Include <String[]>] [-Exclude <String[]>] [-NotMatch]
 [-AllMatches] [-Encoding <Encoding>] [-Context <Int32[]>] [<CommonParameters>]
```

### Object

```
Select-String [-Culture <String>] -InputObject <PSObject> [-Pattern] <String[]> [-SimpleMatch]
 [-CaseSensitive] [-Quiet] [-List] [-NoEmphasis] [-Include <String[]>] [-Exclude <String[]>]
 [-NotMatch] [-AllMatches] [-Encoding <Encoding>] [-Context <Int32[]>] [<CommonParameters>]
```

### FileRaw

```
Select-String [-Culture <String>] [-Pattern] <String[]> [-Path] <String[]> -Raw [-SimpleMatch]
 [-CaseSensitive] [-List] [-NoEmphasis] [-Include <String[]>] [-Exclude <String[]>] [-NotMatch]
 [-AllMatches] [-Encoding <Encoding>] [-Context <Int32[]>] [<CommonParameters>]
```

### LiteralFileRaw

```
Select-String [-Culture <String>] [-Pattern] <String[]> -LiteralPath <String[]> -Raw [-SimpleMatch]
 [-CaseSensitive] [-List] [-NoEmphasis] [-Include <String[]>] [-Exclude <String[]>] [-NotMatch]
 [-AllMatches] [-Encoding <Encoding>] [-Context <Int32[]>] [<CommonParameters>]
```

### LiteralFile

```
Select-String [-Culture <String>] [-Pattern] <String[]> -LiteralPath <String[]> [-SimpleMatch]
 [-CaseSensitive] [-Quiet] [-List] [-NoEmphasis] [-Include <String[]>] [-Exclude <String[]>]
 [-NotMatch] [-AllMatches] [-Encoding <Encoding>] [-Context <Int32[]>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Select-String` 入力文字列とファイル内のテキストパターンとテキストパターンを検索します。 `Select-String`UNIX または Windows の **findstr.exe** と同様に **、を使用** することもできます。

`Select-String` は、テキストの行に基づいています。 既定では、は各行 `Select-String` で最初の一致を検索し、一致するたびに、ファイル名、行番号、および一致を含む行のすべてのテキストを表示します。 `Select-String`1 行に複数の一致項目を検索したり、一致の前後のテキストを表示したり、一致が見つかったかどうかを示すブール値 (True または False) を表示したりすることができます。

`Select-String` 正規表現の照合を使用しますが、指定したテキストの入力を検索する一致も実行できます。

`Select-String` 各入力ファイルで、すべてのテキストの一致を表示するか、最初の一致の後に停止します。
`Select-String` を使用すると、指定したパターンに一致しないすべてのテキストを表示できます。

`Select-String`Unicode テキストのファイルを検索する場合など、で特定の文字エンコーディングを使用するように指定することもできます。 `Select-String` バイト順マーク (BOM) を使用して、ファイルのエンコード形式を検出します。 ファイルに BOM がない場合は、エンコードが UTF8 であると想定されます。

## 例

### 例 1: 大文字と小文字を区別する一致を検索する

この例では、コマンドレットにパイプラインで送信されたテキストの大文字と小文字が区別され `Select-String` ます。

```powershell
'Hello', 'HELLO' | Select-String -Pattern 'HELLO' -CaseSensitive -SimpleMatch
```

テキスト文字列 **hello** と **hello** は、パイプラインの下にコマンドレットに送信され `Select-String` ます。
`Select-String`**Pattern** パラメーターを使用して **HELLO** を指定します。 **CaseSensitive** パラメーターは、大文字と小文字のパターンのみが一致する必要があることを指定します。 **SimpleMatch** は省略可能なパラメーターで、パターン内の文字列が正規表現として解釈されないことを指定します。
`Select-String` PowerShell コンソールに " **HELLO** " と表示されます。

### 例 2: テキストファイル内の一致項目を検索する

このコマンドは、現在のディレクトリ内のファイル名拡張子を持つすべてのファイルを検索 `.txt` します。 出力には、指定した文字列を含むファイル内の行が表示されます。

```powershell
Get-Alias | Out-File -FilePath .\Alias.txt
Get-Command | Out-File -FilePath .\Command.txt
Select-String -Path .\*.txt -Pattern 'Get-'
```

```Output
Alias.txt:8:Alias            cat -> Get-Content
Alias.txt:28:Alias           dir -> Get-ChildItem
Alias.txt:43:Alias           gal -> Get-Alias
Command.txt:966:Cmdlet       Get-Acl
Command.txt:967:Cmdlet       Get-Alias
```

この例で `Get-Alias` は、とを `Get-Command` コマンドレットと共に使用して、現在の `Out-File` ディレクトリに2つのテキストファイルを作成し、 **Alias.txt** と **Command.txt** します。

`Select-String`**Path** パラメーターをアスタリスク () ワイルドカードと共に使用して、 `*` 現在のディレクトリ内のすべてのファイルをファイル名拡張子で検索し `.txt` ます。 **Pattern** パラメーターは、 **Get** と一致するテキストを指定します。 `Select-String` PowerShell コンソールに出力を表示します。 **Pattern** パラメーターの一致が含まれているコンテンツの各行の前にあるファイル名と行番号。

### 例 3: パターン一致を検索する

この例では、複数のファイルを検索して、指定したパターンに一致するものを検索します。 このパターンでは、正規表現の量指定子を使用します。 詳細については、「 [about_Regular_Expressions](../Microsoft.PowerShell.Core/About/About_Regular_Expressions.md)」を参照してください。

```powershell
Select-String -Path "$PSHOME\en-US\*.txt" -Pattern '\?'
```

```Output
C:\Program Files\PowerShell\6\en-US\default.help.txt:27:    beginning at https://go.microsoft.com/fwlink/?LinkID=108518.
C:\Program Files\PowerShell\6\en-US\default.help.txt:50:    or go to: https://go.microsoft.com/fwlink/?LinkID=210614
```

`Select-String`コマンドレットでは、 **Path** と **Pattern** の2つのパラメーターを使用します。 **Path** パラメーターでは、PowerShell ディレクトリを指定する変数を使用し `$PSHOME` ます。 パスの残りの部分には、 **en-us** というサブディレクトリが含まれ、ディレクトリ内の各ファイルを指定し `*.txt` ます。 **Pattern** パラメーターは、各ファイルの疑問符 () に一致するように指定し `?` ます。 円記号 ( `\` ) はエスケープ文字として使用され `?` ます。疑問符 () は正規表現の量指定子であるため、これが必要です。 `Select-String` PowerShell コンソールに出力を表示します。 **Pattern** パラメーターの一致が含まれているコンテンツの各行の前にあるファイル名と行番号。

### 例 4: 関数で Select-String を使用する

この例では、PowerShell のヘルプファイルでパターンを検索する関数を作成します。 この例では、関数は PowerShell セッションにのみ存在します。 PowerShell セッションが閉じられると、関数は削除されます。 詳細については、「 [about_Functions](../Microsoft.PowerShell.Core/About/about_Functions.md)」を参照してください。

```
PS> Function Search-Help
>> {
>> $PSHelp = "$PSHOME\en-US\*.txt"
>> Select-String -Path $PSHelp -Pattern 'About_'
>> }
PS>

PS> Search-Help

C:\Program Files\PowerShell\6\en-US\default.help.txt:67:    The titles of conceptual topics begin with "About_".
C:\Program Files\PowerShell\6\en-US\default.help.txt:70:    Get-Help About_<topic-name>
C:\Program Files\PowerShell\6\en-US\default.help.txt:93:    Get-Help About_Modules : Displays help about PowerShell modules.
C:\Program Files\PowerShell\6\en-US\default.help.txt:97:    about_Updatable_Help
```

関数は、PowerShell コマンドラインで作成されます。 このコマンドでは、と `Function` いう名前の **検索ヘルプ** を使用します。 Enter **キーを** 押して、関数へのステートメントの追加を開始します。 プロンプトで `>>` 、次の例に示すように、各ステートメントを追加し、 **enter** キーを押します。 閉じかっこを追加すると、PowerShell プロンプトに戻ります。

関数には、2つのコマンドが含まれています。 変数には、 `$PSHelp` PowerShell ヘルプファイルへのパスが格納されます。 `$PSHOME` は、ディレクトリ内の各ファイルを指定する **en-us** サブディレクトリを持つ PowerShell インストールディレクトリです `*.txt` 。

`Select-String`関数内のコマンドは、 **Path** パラメーターと **Pattern** パラメーターを使用します。 **Path** パラメーターは、変数を使用して `$PSHelp` パスを取得します。 **Pattern** パラメーターでは、検索条件として文字列 **About_** を使用します。

関数を実行するには、「」と入力 `Search-Help` します。 関数のコマンドを実行すると、 `Select-String` PowerShell コンソールに出力が表示されます。

### 例 5: Windows イベントログで文字列を検索する

この例では、Windows イベントログ内の文字列を検索します。 変数は、 `$_` パイプライン内の現在のオブジェクトを表します。 詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

```powershell
$Events = Get-WinEvent -LogName Application -MaxEvents 50
$Events | Select-String -InputObject {$_.message} -Pattern 'Failed'
```

`Get-WinEvent`コマンドレットでは、 **LogName** パラメーターを使用してアプリケーションログを指定します。 **Maxevents** パラメーターは、ログから最新の50のイベントを取得します。 ログの内容は、という名前の変数に格納され `$Events` ます。

変数は、パイプラインを介して `$Events` コマンドレットに送信され `Select-String` ます。 `Select-String`**InputObject** パラメーターを使用します。 変数は、 `$_` 現在のオブジェクトを表し、 `message` イベントのプロパティです。 **Pattern** パラメーターは、文字列が **失敗した** ことを検出し、で一致を検索し `$_.message` ます。 `Select-String` PowerShell コンソールに出力を表示します。

### 例 6: サブディレクトリ内の文字列を検索する

この例では、特定のテキスト文字列のディレクトリとそのすべてのサブディレクトリを検索します。

```powershell
Get-ChildItem -Path C:\Windows\System32\*.txt -Recurse | Select-String -Pattern 'Microsoft' -CaseSensitive
```

`Get-ChildItem`**Path** パラメーターを使用して **C:\Windows\System32 \*** を指定します。 **再帰** パラメーターには、サブディレクトリが含まれます。 オブジェクトは、パイプラインの下位に送信され `Select-String` ます。

`Select-String`**Pattern** パラメーターを使用し、 **Microsoft** という文字列を指定します。 **CaseSensitive** パラメーターは、文字列の大文字と小文字を区別するために使用されます。 `Select-String` PowerShell コンソールに出力を表示します。

> [!NOTE]
> アクセス許可に応じて、出力に **アクセス拒否** メッセージが表示されることがあります。

### 例 7: パターンに一致しない文字列を検索する

この例では、パターンに一致しないデータ行を除外する方法を示します。

```powershell
Get-Command | Out-File -FilePath .\Command.txt
Select-String -Path .\Command.txt -Pattern 'Get', 'Set'  -NotMatch
```

`Get-Command`コマンドレットは、オブジェクトをパイプライン内でに送信して、 `Out-File` 現在のディレクトリに **Command.txt** ファイルを作成します。 `Select-String`**Path** パラメーターを使用して、 **Command.txt** ファイルを指定します。 **Pattern** パラメーターは、検索パターンとして **Get** と **Set** を指定します。 **Notmatch** パラメーターでは、 **Get** と **Set** が結果から除外されます。
`Select-String`**Get** または **Set** を含まない PowerShell コンソールに出力を表示します。

### 例 8: 一致の前後の行を検索する

この例では、一致したパターンの前後の行を取得する方法を示します。

```powershell
Get-Command | Out-File -FilePath .\Command.txt
Select-String -Path .\Command.txt -Pattern 'Get-Computer' -Context 2, 3
```

```Output
  Command.txt:986:Cmdlet          Get-CmsMessage           6.1.0.0    Microsoft.PowerShell.Security
  Command.txt:987:Cmdlet          Get-Command              6.1.2.0    Microsoft.PowerShell.Core
> Command.txt:988:Cmdlet          Get-ComputerInfo         6.1.0.0    Microsoft.PowerShell.Management
  Command.txt:990:Cmdlet          Get-Content              6.1.0.0    Microsoft.PowerShell.Management
  Command.txt:991:Cmdlet          Get-ControlPanelItem     3.1.0.0    Microsoft.PowerShell.Management
  Command.txt:992:Cmdlet          Get-Credential           6.1.0.0    Microsoft.PowerShell.Security
```

`Get-Command`コマンドレットは、オブジェクトをパイプライン内でに送信して、 `Out-File` 現在のディレクトリに **Command.txt** ファイルを作成します。 `Select-String`**Path** パラメーターを使用して、 **Command.txt** ファイルを指定します。 **Pattern** パラメーターは、検索パターンとして **取得コンピューター** を指定します。 **Context** パラメーターでは、before と after の2つの値を使用して、出力のパターン一致を山かっこ () でマークし `>` ます。 **Context** パラメーターを指定すると、最初のパターンに一致する2行と最後のパターンが一致した後の3行が出力されます。

### 例 9: すべてのパターン一致を検索する

この例では、 **Allmatches** パラメーターがテキスト行の各パターン一致を検索する方法を示しています。 既定では、では、 `Select-String` 最初に出現したパターンがテキスト行で検索されます。 この例では、コマンドレットを使用して検出されたオブジェクトプロパティを使用 `Get-Member` します。

```
PS> $A = Get-ChildItem -Path "$PSHOME\en-US\*.txt" | Select-String -Pattern 'PowerShell'

PS> $A

C:\Program Files\PowerShell\6\en-US\default.help.txt:3:    PowerShell Help System
C:\Program Files\PowerShell\6\en-US\default.help.txt:6:    Displays help about PowerShell cmdlets and concepts.
C:\Program Files\PowerShell\6\en-US\default.help.txt:9:    PowerShell Help describes PowerShell cmdlets

PS> $A.Matches

Groups   : {0}
Success  : True
Name     : 0
Captures : {0}
Index    : 4
Length   : 10
Value    : PowerShell

PS> $A.Matches.Length

8

PS> $B = Get-ChildItem -Path "$PSHOME\en-US\*.txt" | Select-String -Pattern 'PowerShell' -AllMatches

PS> $B.Matches.Length

9
```

`Get-ChildItem`コマンドレットでは、 **Path** パラメーターを使用します。 **Path** パラメーターでは、PowerShell ディレクトリを指定する変数を使用し `$PSHOME` ます。 パスの残りの部分には、 **en-us** というサブディレクトリが含まれ、ディレクトリ内の各ファイルを指定し `*.txt` ます。 `Get-ChildItem`オブジェクトは変数に格納され `$A` ます。 変数は、パイプラインを介して `$A` コマンドレットに送信され `Select-String` ます。 `Select-String`**Pattern** パラメーターを使用して、各ファイルで文字列 **PowerShell** を検索します。

PowerShell コマンドラインから、変数の `$A` 内容が表示されます。 文字列 **PowerShell** の2回の出現を含む行があります。

プロパティは、 `$A.Matches` 各行で最初に見つかったパターンの **PowerShell** を一覧表示します。

プロパティは、 `$A.Matches.Length` 各行で最初に見つかったパターンの **PowerShell** をカウントします。

変数は、 `$B` 同じ `Get-ChildItem` `Select-String` コマンドレットとコマンドレットを使用しますが、 **allmatches** パラメーターを追加します。 **Allmatches** は、各行に各パターンの **PowerShell** の出現箇所を検索します。 変数と変数に格納されて `$A` `$B` いるオブジェクトは同一です。

このプロパティは、各行 `$B.Matches.Length` について、すべてのパターンがカウントされるため、増加します。

## PARAMETERS

### -AllMatches

コマンドレットによって、テキストの各行で複数の一致が検索されることを示します。 このパラメーターを指定しない場合、では、 `Select-String` テキストの各行で最初の一致のみが検索されます。

`Select-String`は、1行のテキストで複数の一致を検出すると、その行に対して1つの **matchinfo** オブジェクトのみを出力しますが、オブジェクトの **matches** プロパティにはすべての一致が含まれます。

> [!NOTE]
> **SimpleMatch** パラメーターと組み合わせて使用する場合、このパラメーターは無視されます。 すべての一致を返し、検索するパターンに正規表現文字が含まれている場合は、 **SimpleMatch** を使用するのではなく、これらの文字をエスケープする必要があります。 正規表現のエスケープの詳細については、「 [about_Regular_Expressions](../Microsoft.PowerShell.Core/About/about_Regular_Expressions.md) 」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -CaseSensitive

コマンドレットの照合で大文字と小文字が区別されることを示します。 既定では、の一致では大文字と小文字が区別されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Context

パターンに一致する行の前後の指定された行数をキャプチャします。

このパラメーターの値として 1 つの数値を入力すると、その数値が一致の前後でキャプチャされる行数となります。 値として 2 つの数値を入力すると、最初の数値が一致の前の行数となり、2 番目の数値が一致の後の行数となります。 たとえば、`-Context 2,3` のようになります。

既定の表示では、一致する行は、 `>` 表示の最初の列に右山かっこ () (ASCII 62) で示されます。 マークされていない行は、コンテキストです。

**Context** パラメーターは、によって生成されるオブジェクトの数を変更しません `Select-String` 。
`Select-String` 一致するごとに1つの [Matchinfo](/dotnet/api/microsoft.powershell.commands.matchinfo) オブジェクトを生成します。 コンテキストは、オブジェクトの **コンテキスト** プロパティに文字列の配列として格納されます。

コマンドの出力 `Select-String` がパイプライン内で別のコマンドに送信されると、 `Select-String` 受信コマンドは一致した行のテキストだけを検索します。 一致した行は、コンテキスト行のテキストではなく、 **Matchinfo** オブジェクトの **line** プロパティの値です。 その結果、 **コンテキスト** パラメーターは、受信コマンドでは無効になり `Select-String` ます。

コンテキストに一致が含まれている場合、各一致の **Matchinfo** オブジェクトにはすべてのコンテキスト行が含まれますが、重複する行は表示に1回だけ表示されます。

```yaml
Type: System.Int32[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Culture

指定したパターンに一致するカルチャ名を指定します。 **Culture** パラメーターは、 **SimpleMatch** パラメーターと共に使用する必要があります。 既定の動作では、現在の PowerShell 実行空間 (セッション) のカルチャが使用されます。

サポートされているすべてのカルチャの一覧を取得するには、コマンドを使用し `Get-Culture -ListAvailable` ます。

さらに、このパラメーターは次の引数を受け取ります。

- CurrentCulture。既定値はです。
- 序数。非言語的なバイナリ比較です。
- インバリアント。これは、カルチャに依存しない比較です。

コマンドを使用 `Select-String -Culture Ordinal -CaseSensitive -SimpleMatch` すると、バイナリ比較を最速で行うことができます。

**Culture** パラメーターは、タブ補完を使用して、使用可能なカルチャを指定する引数の一覧をスクロールします。 使用可能なすべての引数を一覧表示するには、次のコマンドを使用します。

`(Get-Command Select-String).Parameters.Culture.Attributes.ValidValues`

.NET CultureInfo.Name プロパティの詳細については、 [CultureInfo.Name](/dotnet/api//system.globalization.cultureinfo.name)を参照してください。

**Culture** パラメーターは PowerShell 7 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Culture of the current PowerShell session
Accept pipeline input: False
Accept wildcard characters: False
```

### -Encoding

ターゲット ファイルのエンコードの種類を指定します。 既定値は `utf8NoBOM` です。

このパラメーターに指定できる値は次のとおりです。

- `ascii`: ASCII (7 ビット) 文字セットのエンコーディングを使用します。
- `bigendianunicode`: ビッグエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `bigendianutf32`: ビッグエンディアンのバイト順を使用して、32 UTF-8 形式でエンコードします。
- `oem`: MS-DOS およびコンソールプログラムの既定のエンコードを使用します。
- `unicode`: リトルエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `utf7`: UTF-7 形式でエンコードします。
- `utf8`: UTF-8 形式でエンコードします。
- `utf8BOM`: バイト順マーク (BOM) を使用して UTF-8 形式でエンコードします。
- `utf8NoBOM`: バイトオーダーマーク (BOM) を使用せずに UTF-8 形式でエンコードします。
- `utf32`:32 UTF-8 形式でエンコードします。

PowerShell 6.2 以降では、 **Encoding** パラメーターを使用して、登録されているコードページの数値 id (など) `-Encoding 1251` や、登録されているコードページの文字列名 (など) を使用することもでき `-Encoding "windows-1251"` ます。 詳細については、 [コードページ](/dotnet/api/system.text.encoding.codepage?view=netcore-2.2)の .net ドキュメントを参照してください。

> [!NOTE]
> **Utf-7** _ の使用は推奨されなくなりました。 PowerShell 7.1 では、 `utf7` _ *Encoding** パラメーターにを指定すると、警告が書き込まれます。

```yaml
Type: System.Text.Encoding
Parameter Sets: (All)
Aliases:
Accepted values: ASCII, BigEndianUnicode, BigEndianUTF32, OEM, Unicode, UTF7, UTF8, UTF8BOM, UTF8NoBOM, UTF32

Required: False
Position: Named
Default value: UTF8NoBOM
Accept pipeline input: False
Accept wildcard characters: False
```

### -除外

指定された項目を除外します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Include

指定した項目を処理対象に含めます。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -InputObject

検索するテキストを指定します。 テキストが格納されている変数を入力するか、テキストを取得するコマンドまたは式を入力します。

**InputObject** パラメーターの使用は、パイプラインの下にある文字列をに送信する場合と同じではありません `Select-String` 。

パイプを使用して複数の文字列をコマンドレットに渡した場合 `Select-String` 、各文字列で指定したテキストが検索され、検索テキストを含む各文字列が返されます。

**InputObject** パラメーターを使用して文字列のコレクションを送信すると、では `Select-String` コレクションが1つの結合された文字列として扱われます。 `Select-String` 文字列内の検索テキストが見つかった場合は、文字列を単位として返します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: Object, ObjectRaw
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -List

一致するテキストの最初のインスタンスのみが各入力ファイルから返されます。 これは、正規表現に一致する内容を含むファイルの一覧を取得するための最も効率的な方法です。

既定では、は、 `Select-String` 見つかった一致ごとに **matchinfo** オブジェクトを返します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath

検索するファイルへのパスを指定します。 **LiteralPath** パラメーターの値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。 詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: LiteralFileRaw, LiteralFile
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NoEmphasis

既定では、は、 `Select-String` **パターン** パラメーターを使用して検索したパターンに一致する文字列を強調表示します。 **Noemphasis** パラメーターを指定すると、強調表示が無効になります。

この強調では、PowerShell の背景色とテキストの色に基づいて、負の色を使用します。 たとえば、PowerShell の色が黒の背景に白のテキストが含まれているとします。 白い背景と黒いテキストが強調表示されています。

このパラメーターは、PowerShell 7 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotMatch

**Notmatch** パラメーターは、指定されたパターンに一致しないテキストを検索します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

検索するファイルのパスを指定します。 ワイルドカードを使用できます。 既定の場所は、ローカル ディレクトリです。

ディレクトリ内のファイル (、、など) を指定し `log1.txt` `*.doc` `*.*` ます。 ディレクトリのみを指定すると、コマンドは失敗します。

```yaml
Type: System.String[]
Parameter Sets: File, FileRaw
Aliases:

Required: True
Position: 1
Default value: Local directory
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Pattern

各行で検索するテキストを指定します。 Pattern 値は正規表現として扱われます。

正規表現の詳細については、「 [about_Regular_Expressions](../Microsoft.PowerShell.Core/About/about_Regular_Expressions.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -非表示

コマンドレットが **Matchinfo** オブジェクトの代わりにブール値 (True または False) を返すことを示します。 パターンが見つかった場合、値は True になります。それ以外の場合、値は False になります。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: File, Object, LiteralFile
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Raw

コマンドレットが **Matchinfo** オブジェクトではなく、一致する文字列のみを出力するようにします。 これは、Unix **grep** または Windows **findstr.exe** コマンドに最もよく似た動作になります。

このパラメーターは、PowerShell 7 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ObjectRaw, FileRaw, LiteralFileRaw
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -SimpleMatch

コマンドレットが正規表現と一致するのではなく、単純一致を使用することを示します。 単純一致では、 `Select-String` 入力で **Pattern** パラメーターのテキストを検索します。 **Pattern** パラメーターの値は、正規表現ステートメントとして解釈されません。

また、 **SimpleMatch** を使用すると、返される **Matchinfo** オブジェクトの **Matches** プロパティは空になります。

> [!NOTE]
> このパラメーターを **Allmatches** パラメーターと共に使用すると、 **allmatches** は無視されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して、 **ToString** メソッドを持つ任意のオブジェクトをにパイプすることができ `Select-String` ます。

## 出力

### Microsoft. PowerShell.. MatchInfo, System.string, System.string

既定では、出力は、見つかった一致ごとに1つずつの **Matchinfo** オブジェクトのセットです。 **Quiet** パラメーターを使用する場合、出力は、パターンが見つかったかどうかを示す **ブール** 値です。
**Raw** パラメーターを使用すると、出力はパターンに一致する **文字列** オブジェクトのセットになります。

## 注

`Select-String` は、UNIX では **grep** 、Windows では **findstr.exe** に似ています。

コマンドレットの **sls** エイリアスは、 `Select-String` PowerShell 3.0 で導入されました。

> [!NOTE]
> [PowerShell コマンドの承認](/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands)された動詞によれば、コマンドレットの公式のエイリアスプレフィックス `Select-*` は `sc` ではなくです `sl` 。 したがって、の適切なエイリアスはで `Select-String` はなくである必要があり `scs` `sls` ます。 これは、このルールの例外です。

を使用するには `Select-String` 、 **パターン** パラメーターの値として検索するテキストを入力します。 検索するテキストを指定するには、次の条件を使用します。

- 引用符で囲まれた文字列にテキストを入力し、それをにパイプし `Select-String` ます。
- 変数にテキスト文字列を格納し、 **InputObject** パラメーターの値として変数を指定します。
- テキストがファイルに格納されている場合は、 **path** パラメーターを使用して、ファイルへのパスを指定します。

既定では、は、 `Select-String` **Pattern** パラメーターの値を正規表現として解釈します。 詳細については、「 [about_Regular_Expressions](../Microsoft.PowerShell.Core/About/about_Regular_Expressions.md)」を参照してください。 **SimpleMatch** パラメーターを使用して、正規表現の一致をオーバーライドできます。 **SimpleMatch** パラメーターは、入力内の **Pattern** パラメーターの値のインスタンスを検索します。

の既定の出力 `Select-String` は **matchinfo** オブジェクトで、一致に関する詳細情報が含まれています。 **Matchinfo** オブジェクトには **Filename** や **Line** などのプロパティがあるため、オブジェクトの情報は、ファイル内のテキストを検索するときに役立ちます。 入力がファイルからのものではない場合、これらのパラメーターの値は **InputStream** になります。

**Matchinfo** オブジェクト内の情報が不要な場合は、 **Quiet** パラメーターを使用します。 **Quiet** パラメーターは、 **matchinfo** オブジェクトではなく、一致が見つかったかどうかを示すブール値 (True または False) を返します。

一致する語句の場合は、 `Select-String` システムに設定されている現在のカルチャを使用します。 現在のカルチャを検索するには、コマンドレットを使用し `Get-Culture` ます。

**Matchinfo** オブジェクトのプロパティを検索するには、次のコマンドを入力します。

`Select-String -Path test.txt -Pattern 'test' | Get-Member | Format-List -Property *`

## 関連リンク

[about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)

[about_Comparison_Operators](../Microsoft.PowerShell.Core/About/about_Comparison_Operators.md)

[about_Functions](../Microsoft.PowerShell.Core/About/about_Functions.md)

[about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)

[about_Regular_Expressions](../Microsoft.PowerShell.Core/About/about_Regular_Expressions.md)

[Get-Alias](Get-Alias.md)

[Get-ChildItem](../Microsoft.PowerShell.Management/Get-ChildItem.md)

[Get-Command](../Microsoft.PowerShell.Core/Get-Command.md)

[Get-Member](Get-Member.md)

[Get-WinEvent](../Microsoft.PowerShell.Diagnostics/Get-WinEvent.md)

[Out-File](Out-File.md)

