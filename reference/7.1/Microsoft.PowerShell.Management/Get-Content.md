---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Content
ms.openlocfilehash: b6a8d0f68717849711a794c9482e03d4ea081e1d
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "97692656"
---
# Get-Content

## 概要
指定された場所の項目の内容を取得します。

## SYNTAX

### パス (既定値)

```
Get-Content [-ReadCount <Int64>] [-TotalCount <Int64>] [-Tail <Int32>] [-Path] <String[]>
 [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-Force] [-Credential <PSCredential>]
 [-Delimiter <String>] [-Wait] [-Raw] [-Encoding <Encoding>] [-AsByteStream] [-Stream <String>]
 [<CommonParameters>]
```

### LiteralPath

```
Get-Content [-ReadCount <Int64>] [-TotalCount <Int64>] [-Tail <Int32>] -LiteralPath <String[]>
 [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-Force] [-Credential <PSCredential>]
 [-Delimiter <String>] [-Wait] [-Raw] [-Encoding <Encoding>] [-AsByteStream] [-Stream <String>]
 [<CommonParameters>]
```

## Description

`Get-Content`コマンドレットは、ファイル内のテキストや関数の内容など、パスで指定された場所にある項目の内容を取得します。 ファイルの場合、コンテンツは一度に1行読み取られ、オブジェクトのコレクションを返します。各オブジェクトは、コンテンツの行を表します。

PowerShell 3.0 以降では、 `Get-Content` 項目の先頭または末尾から指定された数の行を取得することもできます。

## 例

### 例 1: テキストファイルの内容を取得する

この例では、現在のディレクトリ内のファイルの内容を取得します。 `LineNumbers.txt`このファイルには100行の形式が含まれています。**これは行 X** で、いくつかの例で使用されています。

```powershell
1..100 | ForEach-Object { Add-Content -Path .\LineNumbers.txt -Value "This is line $_." }
Get-Content -Path .\LineNumbers.txt
```

```Output
This is Line 1
This is Line 2
...
This is line 99.
This is line 100.
```

配列値1-100 がパイプラインからコマンドレットに送信され `ForEach-Object` ます。 `ForEach-Object` スクリプトブロックをコマンドレットと共に使用して `Add-Content` 、ファイルを作成し `LineNumbers.txt` ます。 変数は、 `$_` 各オブジェクトがパイプライン内で送信されるときの配列値を表します。 コマンドレットでは、 `Get-Content` **Path** パラメーターを使用してファイルを指定 `LineNumbers.txt` し、PowerShell コンソールにコンテンツを表示します。

### 例 2: Get-Content 返す行の数を制限する

このコマンドは、ファイルの最初の5行を取得します。 **Totalcount** パラメーターは、コンテンツの最初の5行を取得するために使用されます。 この例では、 `LineNumbers.txt` 例1で作成したファイルを使用します。

```powershell
Get-Content -Path .\LineNumbers.txt -TotalCount 5
```

```Output
This is Line 1
This is Line 2
This is Line 3
This is Line 4
This is Line 5
```

### 例 3: テキストファイルから特定の内容の行を取得する

このコマンドは、ファイルから特定の数の行を取得し、その内容の最終行のみを表示します。 **Totalcount** パラメーターは、コンテンツの最初の25行を取得します。 この例では、 `LineNumbers.txt` 例1で作成したファイルを使用します。

```powershell
(Get-Content -Path .\LineNumbers.txt -TotalCount 25)[-1]
```

```Output
This is Line 25
```

コマンドは、 `Get-Content` 次の手順に進む前に、コマンドが完了するように、かっこで囲まれています。 `Get-Content`行の配列を返します。これにより、かっこの後にインデックス表記を追加して、特定の行番号を取得できます。 この場合、インデックスは、 `[-1]` 返された25行の配列内の最後のインデックスを指定します。

### 例 4: テキストファイルの最終行を取得する

このコマンドは、ファイルからコンテンツの最初の行と最後の行を取得します。 この例では、 `LineNumbers.txt` 例1で作成したファイルを使用します。

```powershell
Get-Item -Path .\LineNumbers.txt | Get-Content -Tail 1
```

```Output
This is Line 100
```

この例では、 `Get-Item` コマンドレットを使用して、ファイルをパラメーターにパイプ処理できることを示して `Get-Content` います。 **Tail** パラメーターは、ファイルの最後の行を取得します。 このメソッドは、すべての行を取得し、インデックス表記を使用するよりも高速です `[-1]` 。

### 例 5: 代替データストリームの内容を取得する

この例では、 **ストリーム** パラメーターを使用して、Windows NTFS ボリュームに格納されているファイルの代替データストリームの内容を取得する方法について説明します。 この例では、 `Set-Content` コマンドレットを使用して、という名前のファイルにサンプルコンテンツを作成し `Stream.txt` ます。

```powershell
Set-Content -Path .\Stream.txt -Value 'This is the content of the Stream.txt file'
# Specify a wildcard to the Stream parameter to display all streams of the recently created file.
Get-Item -Path .\Stream.txt -Stream *
```

```Output
PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Test\Stream.txt::$DATA
PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Test
PSChildName   : Stream.txt::$DATA
PSDrive       : C
PSProvider    : Microsoft.PowerShell.Core\FileSystem
PSIsContainer : False
FileName      : C:\Test\Stream.txt
Stream        : :$DATA
Length        : 44
```

```powershell
# Retrieve the content of the primary, or $DATA stream.
Get-Content -Path .\Stream.txt -Stream $DATA
```

```Output
This is the content of the Stream.txt file
```

```powershell
# Use the Stream parameter of Add-Content to create a new Stream containing sample content.
Add-Content -Path .\Stream.txt -Stream NewStream -Value 'Added a stream named NewStream to Stream.txt'
# Use Get-Item to verify the stream was created.
Get-Item -Path .\Stream.txt -Stream *
```

```Output
PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Test\Stream.txt::$DATA
PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Test
PSChildName   : Stream.txt::$DATA
PSDrive       : C
PSProvider    : Microsoft.PowerShell.Core\FileSystem
PSIsContainer : False
FileName      : C:\Test\Stream.txt
Stream        : :$DATA
Length        : 44

PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Test\Stream.txt:NewStream
PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Test
PSChildName   : Stream.txt:NewStream
PSDrive       : C
PSProvider    : Microsoft.PowerShell.Core\FileSystem
PSIsContainer : False
FileName      : C:\Test\Stream.txt
Stream        : NewStream
Length        : 46
```

```powershell
# Retrieve the content of your newly created Stream.
Get-Content -Path .\Stream.txt -Stream NewStream
```

```Output
Added a stream named NewStream to Stream.txt
```

**Stream** パラメーターは、 [FileSystem プロバイダー](../microsoft.powershell.core/about/about_filesystem_provider.md#stream-systemstring)の動的パラメーターです。
既定では、既定で `Get-Content` は、またはストリームからのみデータが取得さ `:$DATA` れます。 **ストリーム** は、属性、セキュリティ設定、その他のデータなどの非表示データを格納するために使用できます。 子項目ではなく、ディレクトリに格納することもできます。

### 例 6: 生のコンテンツを取得する

この例のコマンドは、文字列の配列ではなく、1つの文字列としてファイルの内容を取得します。 既定では、 **生** の動的パラメーターを指定しない場合、コンテンツは改行で区切られた文字列の配列として返されます。 この例では、 `LineNumbers.txt` 例で作成したファイルを使用します。
1.

```powershell
$raw = Get-Content -Path .\LineNumbers.txt -Raw
$lines = Get-Content -Path .\LineNumbers.txt
Write-Host "Raw contains $($raw.Count) lines."
Write-Host "Lines contains $($lines.Count) lines."
```

```Output
Raw contains 1 lines.
Lines contains 100 lines.
```

### 例 7: Get-Content でフィルターを使用する

コマンドレットのフィルターを指定でき `Get-Content` ます。 フィルターを使用して **path** パラメーターを修飾する場合は、パスの内容を示すために、末尾にアスタリスク () を含める必要があり `*` ます。

次のコマンドは、ディレクトリ内のすべてのファイルの内容を取得し `*.log` `C:\Temp` ます。

```powershell
Get-Content -Path C:\Temp\* -Filter *.log
```

### 例 8: ファイルの内容をバイト配列として取得する

この例では、ファイルの内容をとして1つのオブジェクトとして取得する方法を示し `[byte[]]` ます。

```powershell
$byteArray = Get-Content -Path C:\temp\test.txt -AsByteStream -Raw
Get-Member -InputObject $bytearray
```

```Output
   TypeName: System.Byte[]

Name           MemberType            Definition
----           ----------            ----------
Count          AliasProperty         Count = Length
Add            Method                int IList.Add(System.Object value)
```

最初のコマンドは、 **AsByteStream** パラメーターを使用して、ファイルからバイトのストリームを取得します。
**Raw** パラメーターは、バイトがとして返されることを保証し `[System.Byte[]]` ます。 **Raw** パラメーターが指定されていない場合、戻り値はバイトのストリームになります。これは、PowerShell によってとして解釈され `[System.Object[]]` ます。

## PARAMETERS

### -Path

がコンテンツを取得する位置の項目へのパスを指定し `Get-Content` ます。 ワイルドカード文字を使用できます。 コンテナーのパスではなく、項目のパスを指定してください。 たとえば、ディレクトリのパスではなく、1 つ以上のファイルのパスを指定する必要があります。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ReadCount

パイプライン経由で、一度に何行の内容を送るかを指定します。 既定値は 1 です。
値が 0 (ゼロ) の場合は、すべての内容が一度に送信されます。

このパラメーターによって表示される内容が変わることはありませんが、内容を表示する時間には影響を与えます。 **ReadCount** の値を大きくすると、最初の行を返すまでの時間は長くなりますが、処理全体の時間が短くなります。 これにより、大きな項目で出ることの違いを実現できます。

```yaml
Type: System.Int64
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 1
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -TotalCount

ファイルまたはその他の項目の先頭からの行数を指定します。 既定値は -1 (すべての行) です。

**Totalcount** パラメーター名またはそのエイリアス (**最初** または **先頭**) を使用できます。

```yaml
Type: System.Int64
Parameter Sets: (All)
Aliases: First, Head

Required: False
Position: Named
Default value: -1
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -尾

ファイルまたはその他の項目の末尾からの行数を指定します。 **末尾** のパラメーター名またはそのエイリアスである **Last** を使用できます。 このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: Last

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Filter

**パス** パラメーターを修飾するフィルターを指定します。 [ファイルシステム](../Microsoft.PowerShell.Core/About/about_FileSystem_Provider.md)プロバイダーは、フィルターの使用をサポートする唯一のインストール済み PowerShell プロバイダーです。 **ファイルシステム** フィルター言語の構文については、 [about_Wildcards](../Microsoft.PowerShell.Core/About/about_Wildcards.md)を参照してください。
フィルターは他のパラメーターよりも効率的です。これは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、コマンドレットがオブジェクトを取得するときに、フィルターが適用されるためです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Include

文字列配列として、このコマンドレットによって操作に含まれる項目を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `"*.txt"` ます。 ワイルドカード文字を使用できます。 **Include** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

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

### -除外

このコマンドレットによって操作で除外される項目を文字列配列として指定します。
このパラメーターの値は、**Path** パラメーターを修飾します。

パス要素またはパターン (など) を入力し `*.txt` ます。
ワイルドカード文字を使用できます。

**Exclude** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

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

### -Force

**Force** は、読み取り専用属性をオーバーライドするか、ディレクトリを作成してファイルパスを完成させます。 **Force** パラメーターは、ファイルのアクセス許可の変更またはセキュリティ制限のオーバーライドを試行しません。

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

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。
> 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、 [Invoke コマンド](../Microsoft.PowerShell.Core/Invoke-Command.md)を使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -区切り記号

が `Get-Content` 読み取り中にファイルをオブジェクトに分割するために使用する区切り記号を指定します。 既定値は `\n` 、行末文字です。 テキストファイルを読み取るときに、は `Get-Content` 文字列オブジェクトのコレクションを返します。各オブジェクトは行末文字で終わります。 ファイルに存在しない区切り文字を入力すると、は `Get-Content` ファイル全体を単一の区切りのないオブジェクトとして返します。

このパラメーターを使用すると、区切り記号としてファイルの区切り記号を指定することにより、大きなファイルを小さなファイルに分割できます。 区切り文字は予約されています (破棄されません)。各ファイル セクションの最後の項目になります。

**Delimiter** は、 **FileSystem** プロバイダーによってコマンドレットに追加される動的パラメーターです `Get-Content` 。 このパラメーターはファイル システム ドライブでのみ機能します。

> [!NOTE]
> 現在、 **Delimiter** パラメーターの値が空の文字列の場合、 `Get-Content` は何も返しません。 これは既知の問題です。 強制的に、 `Get-Content` ファイル全体を1つの区切りのない文字列として返すようにする場合は。 ファイルに存在しない値を入力してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: End-of-line character
Accept pipeline input: False
Accept wildcard characters: False
```

### -Wait

すべての既存の行が出力された後もファイルを開いたままにします。 待機中、は 1 `Get-Content` 秒ごとにファイルをチェックし、存在する場合は新しい行を出力します。 CTRL キーを押し **ながら C** キーを押すと、**待機** を中断できます。 待機は、ファイルが削除された場合にも終了します。この場合、終了しないエラーが報告されます。

**Wait** は、FileSystem プロバイダーによってコマンドレットに追加される動的パラメーターです `Get-Content` 。 このパラメーターはファイル システム ドライブでのみ機能します。 **Wait** を **Raw** と組み合わせることはできません。

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

### -Raw

改行文字を無視し、改行を保持した状態で、1つの文字列内のファイルの内容全体を返します。 既定では、ファイル内の改行文字は、入力を文字列の配列に分割する区切り記号として使用されます。 このパラメーターは、PowerShell 3.0 で導入されました。

**Raw** は、 **FileSystem** プロバイダーによってコマンドレットに追加される動的パラメーターです `Get-Content` 。このパラメーターは、ファイルシステムドライブでのみ機能します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
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

Encoding は、 **FileSystem** プロバイダーによってコマンドレットに追加される動的パラメーターです `Get-Content` 。
このパラメーターは、ファイルシステムドライブでのみ使用できます。

バイナリファイルの読み取りと書き込みを行う場合は、 **AsByteStream** パラメーターを使用し、 **readcount** パラメーターに値0を指定します。 **Readcount** 値が0の場合は、1回の読み取り操作でファイル全体が読み取られます。 既定の **Readcount** 値は1で、各読み取り操作で1バイトを読み取り、各バイトを別のオブジェクトに変換します。これにより、 `Set-Content` **AsByteStream** パラメーターを使用しない限り、コマンドレットを使用してバイトをファイルに書き込むとエラーが発生します。

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

### -ストリーム

> [!NOTE]
> このパラメーターは Windows でのみ使用できます。

指定した代替 NTFS ファイル ストリームの内容をファイルから取得します。 ストリーム名を入力します。
ワイルドカードはサポートされていません。

**Stream** は、 **FileSystem** プロバイダーによってコマンドレットに追加される動的パラメーターです `Get-Content` 。
このパラメーターは、Windows システムのファイルシステムドライブでのみ機能します。 このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsByteStream

コンテンツをバイトストリームとして読み取ることを指定します。 **AsByteStream** パラメーターは、Windows PowerShell 6.0 で導入されました。

**Encoding** パラメーターで **AsByteStream** パラメーターを使用すると、警告が発生します。 **AsByteStream** パラメーターはエンコードを無視し、出力はバイトのストリームとして返されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Int64、System.String[]、System.Management.Automation.PSCredential

パイプを使用して、読み取り数、合計数、パス、または資格情報をにパイプすることができ `Get-Content` ます。

## 出力

### System.string、System.string

`Get-Content` 文字列またはバイトを返します。 出力の種類は、入力として指定するコンテンツの種類によって異なります。

## 注

`Get-Content`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションのプロバイダーを取得するには、コマンドレットを使用し `Get-PSProvider` ます。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Add-Content](Add-Content.md)

[Clear-Content](Clear-Content.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Get-PSProvider](Get-PSProvider.md)

[Set-Content](Set-Content.md)
