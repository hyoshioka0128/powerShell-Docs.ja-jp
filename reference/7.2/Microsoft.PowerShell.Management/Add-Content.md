---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/add-content?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Add-Content
ms.openlocfilehash: 70ef5033c3c5d37ed00a88abfb0d1353f5d10854
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "99605440"
---
# Add-Content

## 概要
指定した項目に内容を追加します。たとえば、ファイルに語を追加します。

## SYNTAX

### パス (既定値)

```
Add-Content [-Path] <string[]> [-Value] <Object[]> [-PassThru] [-Filter <string>]
 [-Include <string[]>] [-Exclude <string[]>] [-Force] [-Credential <pscredential>] [-WhatIf]
 [-Confirm] [-NoNewline] [-Encoding <Encoding>] [-AsByteStream] [-Stream <string>]
 [<CommonParameters>]
```

### LiteralPath

```
Add-Content [-Value] <Object[]> -LiteralPath <string[]> [-PassThru] [-Filter <string>]
 [-Include <string[]>] [-Exclude <string[]>] [-Force] [-Credential <pscredential>] [-WhatIf]
 [-Confirm] [-NoNewline] [-Encoding <Encoding>] [-AsByteStream] [-Stream <string>]
 [<CommonParameters>]
```

## Description

`Add-Content`コマンドレットは、指定された項目またはファイルにコンテンツを追加します。 内容を指定するには、コマンドに内容を入力するか、内容が格納されているオブジェクトを指定します。

次の例でファイルまたはディレクトリを作成する必要がある場合は、「 [New-Item](New-Item.md)」を参照してください。

## 例

### 例 1: 例外が発生したすべてのテキストファイルに文字列を追加する

この例では、現在のディレクトリ内のテキストファイルに値を追加しますが、ファイル名に基づいてファイルを除外します。

```powershell
Add-Content -Path .\*.txt -Exclude help* -Value 'End of file'
```

**Path** パラメーターは、現在のディレクトリ内のすべてのファイルを指定し `.txt` ますが、 **Exclude** パラメーターは、指定されたパターンに一致するファイル名を無視します。 **値** パラメーターは、ファイルに書き込まれるテキスト文字列を指定します。

### 例 2: 指定したファイルの末尾に日付を追加する

この例では、現在のディレクトリのファイルに日付を追加し、その日付を PowerShell コンソールに表示します。

```powershell
Add-Content -Path .\DateTimeFile1.log, .\DateTimeFile2.log -Value (Get-Date) -PassThru
Get-Content -Path .\DateTimeFile1.log
```

```Output
Tuesday, May 14, 2019 8:24:27 AM
Tuesday, May 14, 2019 8:24:27 AM
5/14/2019 8:24:27 AM
```

コマンドレットにより、 `Add-Content` 現在のディレクトリに2つの新しいファイルが作成されます。 **Value** パラメーターには、コマンドレットの出力が含まれてい `Get-Date` ます。 **PassThru** パラメーターは、追加された内容をパイプラインに出力します。
出力を受け取るコマンドレットは他にないため、PowerShell コンソールに表示されます。
コマンドレットにより、 `Get-Content` 更新されたファイルが表示され `DateTimeFile1.log` ます。

### 例 3: 指定したファイルの内容を別のファイルに追加する

この例では、ファイルからコンテンツを取得し、その内容を変数に格納します。 変数は、コンテンツを別のファイルに追加するために使用されます。

```powershell
$From = Get-Content -Path .\CopyFromFile.txt
Add-Content -Path .\CopyToFile.txt -Value $From
Get-Content -Path .\CopyToFile.txt
```

- コマンドレットでは、 `Get-Content` の内容を取得し、その `CopyFromFile.txt` 内容を変数に格納し `$From` ます。
- `Add-Content`コマンドレットは、 `CopyToFile.txt` 変数の内容を使用してファイルを更新し `$From` ます。
- コマンドレットによって `Get-Content` CopyToFile.txt が表示されます。

### 例 4: パイプラインを使用して、指定したファイルの内容を別のファイルに追加する

この例では、ファイルからコンテンツを取得し、コマンドレットにパイプし `Add-Content` ます。

```powershell
Get-Content -Path .\CopyFromFile.txt | Add-Content -Path .\CopyToFile.txt
Get-Content -Path .\CopyToFile.txt
```

コマンドレットでは、 `Get-Content` の内容を取得し `CopyFromFile.txt` ます。 結果は `Add-Content` 、を更新するコマンドレットにパイプ処理され `CopyToFile.txt` ます。
最後の `Get-Content` コマンドレットが表示され `CopyToFile.txt` ます。

### 例 5: 新しいファイルを作成してコンテンツをコピーする

この例では、新しいファイルを作成し、既存のファイルの内容を新しいファイルにコピーします。

```powershell
Add-Content -Path .\NewFile.txt -Value (Get-Content -Path .\CopyFromFile.txt)
Get-Content -Path .\NewFile.txt
```

- コマンドレットでは、 `Add-Content` **Path** パラメーターと **Value** パラメーターを使用して、現在のディレクトリに新しいファイルを作成します。
- `Get-Content`コマンドレットは、既存のファイルの内容を取得 `CopyFromFile.txt` し、それを **Value** パラメーターに渡します。 コマンドレットを囲むかっこを実行すると、コマンドが開始される `Get-Content` 前にコマンドが終了し `Add-Content` ます。
- コマンドレットにより、 `Get-Content` 新しいファイルの内容が表示され `NewFile.txt` ます。

### 例 6: 読み取り専用ファイルにコンテンツを追加する

このコマンドは、 **IsReadOnly** file 属性が **True** に設定されている場合でも、ファイルに値を追加します。
この例には、読み取り専用ファイルを作成する手順が含まれています。

```powershell
New-Item -Path .\IsReadOnlyTextFile.txt -ItemType File
Set-ItemProperty -Path .\IsReadOnlyTextFile.txt -Name IsReadOnly -Value $True
Get-ChildItem -Path .\IsReadOnlyTextFile.txt
Add-Content -Path .\IsReadOnlyTextFile.txt -Value 'Add value to read-only text file' -Force
Get-Content -Path .\IsReadOnlyTextFile.txt
```

```Output
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar--         1/28/2019     13:35              0 IsReadOnlyTextFile.txt
```

- この `New-Item` コマンドレットは、 **Path** パラメーターと **ItemType** パラメーターを使用して、現在のディレクトリにファイルを作成し `IsReadOnlyTextFile.txt` ます。
- コマンドレットでは、 `Set-ItemProperty` **Name** パラメーターと **Value** パラメーターを使用して、ファイルの **IsReadOnly** プロパティを True に変更します。
- この `Get-ChildItem` コマンドレットは、ファイルが空 (0) であり、読み取り専用属性 () を持っていることを示してい `r` ます。
- コマンドレットでは、 `Add-Content` **Path** パラメーターを使用してファイルを指定します。 **Value** パラメーターには、ファイルに追加するテキスト文字列が含まれています。 **Force** パラメーターは、テキストを読み取り専用ファイルに書き込みます。
- この `Get-Content` コマンドレットは、 **Path** パラメーターを使用してファイルの内容を表示します。

読み取り専用属性を削除するには、 `Set-ItemProperty` **値** パラメーターをに設定してコマンドを使用し `False` ます。

### 例 7: Add-Content でフィルターを使用する

コマンドレットのフィルターを指定でき `Add-Content` ます。 フィルターを使用して **path** パラメーターを修飾する場合は、パスの内容を示すために、末尾にアスタリスク () を含める必要があり `*` ます。

次のコマンドは、ディレクトリ内のすべてのファイルの内容を "Done" という単語に追加し `*.txt` `C:\Temp` ます。

```powershell
Add-Content -Path C:\Temp\* -Filter *.txt -Value "Done"
```

## PARAMETERS

### -AsByteStream

コンテンツをバイトストリームとして読み取ることを指定します。 このパラメーターは、PowerShell 6.0 で導入されました。

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
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Encoding

ターゲット ファイルのエンコードの種類を指定します。 既定値は `utf8NoBOM` です。

Encoding は、FileSystem プロバイダーによってコマンドレットに追加される動的パラメーターです `Add-Content` 。 このパラメーターはファイル システム ドライブでのみ機能します。

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

このコマンドレットによって操作で除外される項目を文字列配列として指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカード文字を使用できます。 **Exclude** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

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

### -Force

読み取り専用属性をオーバーライドし、読み取り専用ファイルに内容を追加できるようにします。 たとえば、**Force** を指定すると、読み取り専用属性がオーバーライドされるか、ファイル パスを完成させるためにディレクトリが作成されますが、ファイルのアクセス許可は変更されません。

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

### -なし Wline

このコマンドレットがコンテンツに新しい行または復帰を追加しないことを示します。

入力オブジェクトの文字列形式が連結され、出力が形成されます。 出力文字列間にスペースや改行は挿入されません。 最後の出力文字列の後に改行は追加されません。

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

### -PassThru

追加された内容を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -Path

内容を追加する項目へのパスを指定します。
ワイルドカード文字を使用できます。
コンテナーのパスではなく、項目のパスを指定してください。
たとえば、ディレクトリのパスではなく、1 つ以上のファイルのパスを指定する必要があります。
複数のパスを指定する場合は、各パスをコンマで区切ります。

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

### -ストリーム

> [!NOTE]
> このパラメーターは Windows でのみ使用できます。

コンテンツの代替データストリームを指定します。 ストリームが存在しない場合は、このコマンドレットによって作成されます。 ワイルドカード文字はサポートされていません。

**Stream** は、FileSystem プロバイダーによってに追加される動的パラメーターです `Add-Content` 。 このパラメーターはファイル システム ドライブでのみ機能します。

コマンドレットを使用して、など `Add-Content` の代替データストリームの内容を変更でき `Zone.Identifier` ます。 ただし、インターネットからダウンロードされたファイルをブロックするセキュリティチェックを省略する方法としては、この方法をお勧めしません。 ダウンロードしたファイルが安全であることを確認した場合は、コマンドレットを使用し `Unblock-File` ます。

このパラメーターは、PowerShell 3.0 で導入されました。  PowerShell 7.2 の場合、で `Add-Content` は、ファイルとディレクトリの両方で別のデータストリームをターゲットにすることができます。


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

### -Value

追加する内容を指定します。 **このデータが内部でのみ使用さ** れるなどの引用符で囲まれた文字列を入力するか、生成する **DateTime** オブジェクトなど、コンテンツを含むオブジェクトを指定し `Get-Date` ます。

パスは文字列であるため、パスを入力してファイルの内容を指定することはできません。
コマンドを使用して `Get-Content` コンテンツを取得し、それを **Value** パラメーターに渡すことができます。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.object、システムの管理、PSCredential

パイプを使用して、値、パス、または資格情報をにパイプすることができ `Set-Content` ます。

## 出力

### None または System.String

**PassThru** パラメーターを使用すると、によって、 `Add-Content` コンテンツを表す **system.string** オブジェクトが生成されます。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

- オブジェクトをにパイプすると `Add-Content` 、オブジェクトは項目に追加される前に文字列に変換されます。 オブジェクト型によって文字列の形式が決まりますが、オブジェクトの既定の表示形式と異なる場合があります。 文字列の形式を制御するには、送信コマンドレットの書式設定パラメーターを使用します。
- また、組み込みエイリアスであるを参照することもでき `Add-Content` `ac` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。
- `Add-Content`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Clear-Content](Clear-Content.md)

[Get-Content](Get-Content.md)

[Get-Item](Get-Item.md)

[New-Item](New-Item.md)

[Set-Content](Set-Content.md)
