---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-content?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-Content
ms.openlocfilehash: 2561d569fa773d279e1e54561d6005e3eef7f7e3
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "97692783"
---
# Set-Content

## 概要
新しいコンテンツを書き込むか、ファイル内の既存のコンテンツを置き換えます。

## SYNTAX

### パス (既定値)

```
Set-Content [-Path] <string[]> [-Value] <Object[]> [-PassThru] [-Filter <string>]
 [-Include <string[]>] [-Exclude <string[]>] [-Force] [-Credential <pscredential>]
 [-WhatIf] [-Confirm] [-NoNewline] [-Encoding <Encoding>] [-AsByteStream] [-Stream <string>]
 [<CommonParameters>]
```

### LiteralPath

```
Set-Content [-Value] <Object[]> -LiteralPath <string[]> [-PassThru] [-Filter <string>]
 [-Include <string[]>] [-Exclude <string[]>] [-Force] [-Credential <pscredential>]
 [-WhatIf] [-Confirm] [-NoNewline] [-Encoding <Encoding>] [-AsByteStream] [-Stream <string>]
 [<CommonParameters>]
```

## Description

`Set-Content` は、新しいコンテンツを書き込むか、またはファイル内のコンテンツを置き換える文字列処理コマンドレットです。 `Set-Content` 既存のコンテンツを置き換え `Add-Content` ます。また、ファイルにコンテンツを追加するコマンドレットとは異なります。 にコンテンツを送信するには、 `Set-Content` コマンドラインで **Value** パラメーターを使用するか、パイプラインを介してコンテンツを送信します。

次の例でファイルまたはディレクトリを作成する必要がある場合は、「 [New-Item](New-Item.md)」を参照してください。

## 例

### 例 1: ディレクトリ内の複数のファイルの内容を置き換える

この例では、現在のディレクトリ内の複数のファイルの内容を置き換えます。

```powershell
Get-ChildItem -Path .\Test*.txt
```

```Output
Test1.txt
Test2.txt
Test3.txt
```

```powershell
Set-Content -Path .\Test*.txt -Value 'Hello, World'
Get-Content -Path .\Test*.txt
```

```Output
Hello, World
Hello, World
Hello, World
```

この `Get-ChildItem` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリので始まる **.txt** ファイルを一覧表示します。 `Test*` コマンドレットでは、 `Set-Content` **Path** パラメーターを使用してファイルを指定し `Test*.txt` ます。 **Value** パラメーターは、各ファイルの既存のコンテンツを置き換えるテキスト文字列 **Hello, World** を提供します。 コマンドレットでは、 `Get-Content` **Path** パラメーターを使用してファイルを指定 `Test*.txt` し、各ファイルの内容を PowerShell コンソールに表示します。

### 例 2: 新しいファイルを作成し、コンテンツを書き込む

この例では、新しいファイルを作成し、現在の日付と時刻をファイルに書き込みます。

```powershell
Set-Content -Path .\DateTime.txt -Value (Get-Date)
Get-Content -Path .\DateTime.txt
```

```Output
1/30/2019 09:55:08
```

`Set-Content`**Path** および **Value** パラメーターを使用して、現在のディレクトリに **DateTime.txt** という名前の新しいファイルを作成します。 **Value** パラメーターは、を使用して `Get-Date` 現在の日付と時刻を取得します。
`Set-Content`**DateTime** オブジェクトを文字列としてファイルに書き込みます。 この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、PowerShell コンソールに **DateTime.txt** の内容を表示します。

### 例 3: ファイル内のテキストを置換する

このコマンドは、既存のファイル内の word のすべてのインスタンスを置き換えます。

```powershell
Get-Content -Path .\Notice.txt
```

```Output
Warning
Replace Warning with a new word.
The word Warning was replaced.
```

```powershell
(Get-Content -Path .\Notice.txt) |
    ForEach-Object {$_ -Replace 'Warning', 'Caution'} |
        Set-Content -Path .\Notice.txt
Get-Content -Path .\Notice.txt
```

```Output
Caution
Replace Caution with a new word.
The word Caution was replaced.
```

この `Get-Content` コマンドレットは、 **Path** パラメーターを使用して、現在のディレクトリ内の **Notice.txt** ファイルを指定します。 `Get-Content`コマンドがかっこで囲まれ、パイプラインで送信される前にコマンドが終了します。

**Notice.txt** ファイルの内容は、パイプラインを介してコマンドレットに送信され `ForEach-Object` ます。
`ForEach-Object` 自動変数を使用 `$_` し、 **警告** が発生するたびに **注意** して置き換えます。 オブジェクトは、パイプラインを介してコマンドレットに送信され `Set-Content` ます。 `Set-Content`**Path** パラメーターを使用して **Notice.txt** ファイルを指定し、更新された内容をファイルに書き込みます。

最後の `Get-Content` コマンドレットは、更新されたファイルの内容を PowerShell コンソールに表示します。

### 例 4: Set-Content でフィルターを使用する

コマンドレットのフィルターを指定でき `Set-Content` ます。 フィルターを使用して **path** パラメーターを修飾する場合は、パスの内容を示すために、末尾にアスタリスク () を含める必要があり `*` ます。

次のコマンドは、 `*.txt` ディレクトリ内のすべてのファイルの内容 `C:\Temp` を空の **値** に設定します。

```powershell
Set-Content -Path C:\Temp\* -Filter *.txt -Value "Empty"
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

Encoding は、FileSystem プロバイダーによってに追加される動的パラメーターです `Set-Content` 。 このパラメーターはファイル システム ドライブでのみ機能します。

このパラメーターに指定できる値は次のとおりです。

- `ascii`: ASCII (7 ビット) 文字セットのエンコーディングを使用します。
- `bigendianunicode`: ビッグエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `oem`: MS-DOS およびコンソールプログラムの既定のエンコードを使用します。
- `unicode`: リトルエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `utf7`: UTF-7 形式でエンコードします。
- `utf8`: UTF-8 形式でエンコードします。
- `utf8BOM`: バイト順マーク (BOM) を使用して UTF-8 形式でエンコードします。
- `utf8NoBOM`: バイトオーダーマーク (BOM) を使用せずに UTF-8 形式でエンコードします。
- `utf32`:32 UTF-8 形式でエンコードします。

PowerShell 6.2 以降では、 **Encoding** パラメーターを使用して、登録されているコードページの数値 id (など) `-Encoding 1251` や、登録されているコードページの文字列名 (など) を使用することもでき `-Encoding "windows-1251"` ます。 詳細については、 [コードページ](/dotnet/api/system.text.encoding.codepage?view=netcore-2.2)の .net ドキュメントを参照してください。

```yaml
Type: System.Text.Encoding
Parameter Sets: (All)
Aliases:
Accepted values: ASCII, BigEndianUnicode, OEM, Unicode, UTF7, UTF8, UTF8BOM, UTF8NoBOM, UTF32

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

ファイルが読み取り専用の場合でも、ファイルの内容をコマンドレットに強制的に設定します。 実装はプロバイダーごとに異なります。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。
**Force** パラメーターは、セキュリティ制限をオーバーライドしません。

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

コンテンツを表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

コンテンツを受信する項目のパスを指定します。
ワイルドカード文字を使用できます。

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

**Stream** は、 **FileSystem** プロバイダーによってに追加される動的パラメーターです `Set-Content` 。 このパラメーターはファイル システム ドライブでのみ機能します。

コマンドレットを使用して、など `Set-Content` の代替データストリームの内容を作成または更新でき `Zone.Identifier` ます。 ただし、インターネットからダウンロードされたファイルをブロックするセキュリティチェックを省略する方法としては、この方法をお勧めしません。 ダウンロードしたファイルが安全であることを確認した場合は、コマンドレットを使用し `Unblock-File` ます。

このパラメーターは、PowerShell 3.0 で導入されました。

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

項目の新しい内容を指定します。

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

### System.Object

パイプを使用して、項目の新しい値を含むオブジェクトをにパイプすることができ `Set-Content` ます。

## 出力

### None または System.String

**PassThru** パラメーターを使用すると、によって、 `Set-Content` コンテンツを表す **system.string** オブジェクトが生成されます。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

- また、組み込みエイリアスであるを参照することもでき `Set-Content` `sc` ます。
  詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。
- `Set-Content` は、文字列処理用に設計されています。 文字列以外のオブジェクトをにパイプする場合は `Set-Content` 、オブジェクトを書き込む前に文字列に変換します。 オブジェクトをファイルに書き込むには、を使用 `Out-File` します。
- `Set-Content`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PsProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)

[about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Add-Content](Add-Content.md)

[Clear-Content](Clear-Content.md)

[Get-ChildItem](Get-ChildItem.md)

[Get-Content](Get-Content.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[New-Item](New-Item.md)
