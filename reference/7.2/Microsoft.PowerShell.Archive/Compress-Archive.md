---
external help file: Microsoft.PowerShell.Archive-help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Archive
ms.date: 02/20/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/compress-archive?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Compress-Archive
ms.openlocfilehash: 58807ba0745a6b09e7956547425c489b427d4f4b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601392"
---
# Compress-Archive

## 概要
指定したファイルおよびディレクトリから圧縮されたアーカイブ (zip 形式のファイル) を作成します。

## SYNTAX

### パス (既定値)

```
Compress-Archive [-Path] <String[]> [-DestinationPath] <String> [-CompressionLevel <String>]
 [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### PathWithUpdate

```
Compress-Archive [-Path] <String[]> [-DestinationPath] <String> [-CompressionLevel <String>] -Update
 [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### PathWithForce

```
Compress-Archive [-Path] <String[]> [-DestinationPath] <String> [-CompressionLevel <String>] -Force
 [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### LiteralPathWithUpdate

```
Compress-Archive -LiteralPath <String[]> [-DestinationPath] <String> [-CompressionLevel <String>]
 -Update [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### LiteralPathWithForce

```
Compress-Archive -LiteralPath <String[]> [-DestinationPath] <String> [-CompressionLevel <String>]
 -Force [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### LiteralPath

```
Compress-Archive -LiteralPath <String[]> [-DestinationPath] <String> [-CompressionLevel <String>]
 [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Compress-Archive` 1 つ以上の指定したファイルまたはディレクトリから、圧縮されたアーカイブファイルまたは zip 形式のアーカイブファイルを作成します。 アーカイブでは、配布と保存を容易にするために、オプションで圧縮された複数のファイルを1つの zip 形式のファイルにパッケージ化します。 アーカイブファイルは、 **CompressionLevel** パラメーターで指定した圧縮アルゴリズムを使用して圧縮できます。

この `Compress-Archive` コマンドレットは、MICROSOFT .NET API [System.IO.Compression.Zipアーカイブ](/dotnet/api/system.io.compression.ziparchive) を使用してファイルを圧縮します。
基になる API の制限があるため、最大ファイルサイズは 2 GB です。

いくつかの例では、スプラッティングを使用して、コードサンプルの行の長さを減らしています。 詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md)」を参照してください。

## 例

### 例 1: ファイルを圧縮してアーカイブファイルを作成する

この例では、別のディレクトリのファイルを圧縮し、アーカイブファイルを作成します。 特定のファイル拡張子を持つすべてのファイルを取得するには、ワイルドカードを使用します。 アーカイブファイルにはディレクトリ構造がありません。これは、 **パス** がファイル名のみを指定しているためです。

```powershell
$compress = @{
  Path = "C:\Reference\Draftdoc.docx", "C:\Reference\Images\*.vsd"
  CompressionLevel = "Fastest"
  DestinationPath = "C:\Archives\Draft.Zip"
}
Compress-Archive @compress
```

**Path** パラメーターは、ワイルドカードを使用して、特定のファイル名とファイル名を受け取り `*.vsd` ます。 **パス** では、コンマ区切りのリストを使用して、異なるディレクトリからファイルを取得します。 圧縮レベルは、処理時間を短縮するのに **最も高速** です。 **DestinationPath** パラメーターは、ファイルの場所を指定し `Draft.zip` ます。 `Draft.zip`ファイルには `Draftdoc.docx` と拡張子を持つすべてのファイルが含まれ `.vsd` ます。

### 例 2: LiteralPath を使用してファイルを圧縮する

この例では、特定の名前付きファイルを圧縮し、新しいアーカイブファイルを作成します。 アーカイブファイルにはディレクトリ構造がありません。これは、 **パス** がファイル名のみを指定しているためです。

```powershell
$compress = @{
LiteralPath= "C:\Reference\Draft Doc.docx", "C:\Reference\Images\diagram2.vsd"
CompressionLevel = "Fastest"
DestinationPath = "C:\Archives\Draft.Zip"
}
Compress-Archive @compress
```

**LiteralPath** パラメーターにはワイルドカードを使用できないため、絶対パスとファイル名が使用されます。 **パス** では、コンマ区切りのリストを使用して、異なるディレクトリからファイルを取得します。 圧縮レベルは、処理時間を短縮するのに **最も高速** です。 **DestinationPath** パラメーターは、ファイルの場所を指定し `Draft.zip` ます。 ファイルには `Draft.zip` とのみが含まれ `Draftdoc.docx` `diagram2.vsd` ます。

### 例 3: ルートディレクトリを含むディレクトリを圧縮する

この例では、ディレクトリを圧縮し、ルートディレクトリとそのすべてのファイルとサブディレクトリを **含む** アーカイブファイルを作成します。 **パス** にはルートディレクトリが指定されているため、アーカイブファイルにはディレクトリ構造があります。

```powershell
Compress-Archive -Path C:\Reference -DestinationPath C:\Archives\Draft.zip
```

`Compress-Archive`**Path** パラメーターを使用して、ルートディレクトリを指定し `C:\Reference` ます。 **DestinationPath** パラメーターは、アーカイブファイルの場所を指定します。 アーカイブには、 `Draft.zip` `Reference` ルートディレクトリ、およびそのすべてのファイルとサブディレクトリが含まれます。

### 例 4: ルートディレクトリを除外するディレクトリを圧縮する

この例では、ディレクトリを圧縮し、ルートディレクトリを **除外** するアーカイブファイルを作成します。これは、 **パス** でアスタリスク () ワイルドカードが使用されているため `*` です。 アーカイブには、ルートディレクトリのファイルとサブディレクトリを含むディレクトリ構造が含まれています。

```powershell
Compress-Archive -Path C:\Reference\* -DestinationPath C:\Archives\Draft.zip
```

`Compress-Archive`**Path** パラメーターを使用して、ルートディレクトリを指定し `C:\Reference` ます。アスタリスク () のワイルドカードを使用し `*` ます。 **DestinationPath** パラメーターは、アーカイブファイルの場所を指定します。 アーカイブには `Draft.zip` 、ルートディレクトリのファイルとサブディレクトリが含まれています。 `Reference`ルートディレクトリはアーカイブから除外されます。

### 例 5: ルートディレクトリ内のファイルのみを圧縮する

この例では、ルートディレクトリ内のファイルのみを圧縮し、アーカイブファイルを作成します。 ファイルのみが圧縮されているため、アーカイブにはディレクトリ構造がありません。

```powershell
Compress-Archive -Path C:\Reference\*.* -DestinationPath C:\Archives\Draft.zip
```

`Compress-Archive`**Path** パラメーターを使用して、ルートディレクトリを指定します。これには、 `C:\Reference` アスタリスク (!)**星**() のワイルドカードを使用し `*.*` ます。 **DestinationPath** パラメーターは、アーカイブファイルの場所を指定します。 アーカイブには `Draft.zip` ルートディレクトリのファイルのみが含まれ、 `Reference` ルートディレクトリは除外されます。

### 例 6: パイプラインを使用してファイルをアーカイブする

この例では、ファイルをパイプラインに送信してアーカイブを作成します。 アーカイブファイルにはディレクトリ構造がありません。これは、 **パス** がファイル名のみを指定しているためです。

```powershell
Get-ChildItem -Path C:\Reference\Afile.txt, C:\Reference\Images\Bfile.txt |
  Compress-Archive -DestinationPath C:\Archives\PipelineFiles.zip
```

`Get-ChildItem`**Path** パラメーターを使用して、異なるディレクトリの2つのファイルを指定します。 各ファイルは、 **FileInfo** オブジェクトによって表され、パイプライン内でに送信され `Compress-Archive` ます。
指定された2つのファイルは、にアーカイブされ `PipelineFiles.zip` ます。

### 例 7: パイプラインを使用してディレクトリをアーカイブする

この例では、ディレクトリをパイプラインの下に送信してアーカイブを作成します。 ファイルは、 **DirectoryInfo** オブジェクトとして **FileInfo** オブジェクトおよびディレクトリとして送信されます。 アーカイブのディレクトリ構造にはルートディレクトリが含まれていませんが、そのファイルとサブディレクトリはアーカイブに含まれています。

```powershell
Get-ChildItem -Path C:\LogFiles | Compress-Archive -DestinationPath C:\Archives\PipelineDir.zip
```

`Get-ChildItem`**Path** パラメーターを使用して、 `C:\LogFiles` ルートディレクトリを指定します。 各 **FileInfo** および **DirectoryInfo** オブジェクトは、パイプラインを介して送信されます。

`Compress-Archive` 各オブジェクトをアーカイブに追加し `PipelineDir.zip` ます。 パイプラインオブジェクトがパラメーター位置0に渡されるため、 **Path** パラメーターが指定されていません。

### 例 8: 再帰がアーカイブに与える影響

この例では、再帰によってアーカイブ内のファイルを複製する方法を示します。 たとえば、を `Get-ChildItem` **再帰** パラメーターと共に使用するとします。 再帰処理として、各 **FileInfo** オブジェクトと **DirectoryInfo** オブジェクトがパイプラインで送信され、アーカイブに追加されます。

```powershell
Get-ChildItem -Path C:\TestLog -Recurse |
  Compress-Archive -DestinationPath C:\Archives\PipelineRecurse.zip
```

`C:\TestLog`ディレクトリにファイルが含まれていません。 このファイルには、ファイルを含むという名前のサブディレクトリが含まれてい `testsub` `testlog.txt` ます。

`Get-ChildItem`**Path** パラメーターを使用して、ルートディレクトリを指定し `C:\TestLog` ます。 **再帰** パラメーターは、ファイルとディレクトリを処理します。 と FileInfo オブジェクトの **DirectoryInfo** オブジェクトが作成され `testsub`  `testlog.txt` ます。

各オブジェクトは、パイプライン内でに送信され `Compress-Archive` ます。 **DestinationPath** は、アーカイブファイルの場所を指定します。 パイプラインオブジェクトがパラメーター位置0に渡されるため、 **Path** パラメーターが指定されていません。

次の概要では、 `PipelineRecurse.zip` 重複するファイルを含むアーカイブの内容について説明します。

- **DirectoryInfo** オブジェクトは、ディレクトリを作成 `testsub` し、 `testlog.txt` ファイルを格納します。このファイルには、元のディレクトリ構造が反映されています。
- **FileInfo** オブジェクトは、 `testlog.txt` アーカイブのルートに複製を作成します。 再帰によってファイルオブジェクトがに送信されたため、重複したファイルが作成され `Compress-Archive` ます。 パイプラインを経由して送信された各オブジェクトがアーカイブに追加されるため、この動作が想定されます。

### 例 9: 既存のアーカイブファイルを更新する

この例では、ディレクトリ内の既存のアーカイブファイルを更新し `Draft.Zip` `C:\Archives` ます。 この例では、既存のアーカイブファイルにルートディレクトリとそのファイルとサブディレクトリが含まれています。

```powershell
Compress-Archive -Path C:\Reference -Update -DestinationPath C:\Archives\Draft.Zip
```

このコマンドは、 `Draft.Zip` `C:\Reference` ディレクトリとそのサブディレクトリ内の既存のファイルの新しいバージョンで更新されます。 また、そのサブディレクトリに追加された新しいファイルは、 `C:\Reference` 更新されたアーカイブに含まれ `Draft.Zip` ます。

## PARAMETERS

### -CompressionLevel

アーカイブファイルを作成しているときに適用する圧縮の量を指定します。 より高速な圧縮では、ファイルの作成に時間がかかりますが、ファイルサイズが大きくなる可能性があります。

このパラメーターが指定されていない場合、コマンドは既定値である [ **最適**] を使用します。

このパラメーターに指定できる値は次のとおりです。

- **最速**。 処理時間を短縮するために使用できる最速の圧縮方法を使用します。 圧縮を高速化すると、ファイルサイズが大きくなる可能性があります。
- **Nocompression**。 では、ソースファイルは圧縮されません。
- **[最適]** 。 処理時間はファイルサイズに依存します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Optimal, NoCompression, Fastest

Required: False
Position: Named
Default value: Optimal
Accept pipeline input: False
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

### -DestinationPath

このパラメーターは必須で、アーカイブ出力ファイルへのパスを指定します。 **DestinationPath** には、zip 形式のファイルの名前と、zip 形式のファイルへの絶対パスまたは相対パスを含める必要があります。

**DestinationPath** のファイル名にファイル名拡張子がない場合は、コマンドレットによって `.zip` ファイル名拡張子が追加され `.zip` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PathWithForce, LiteralPathWithForce
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath

アーカイブ zip ファイルに追加するファイルのパスを指定します。 **Path** パラメーターとは異なり、 **LiteralPath** の値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、各エスケープ文字を単一引用符で囲み、すべての文字をエスケープシーケンスとして解釈しないように PowerShell に指示します。
複数のパスを指定し、出力 zip ファイル内の複数の場所にファイルを含めるには、コンマを使用してパスを区切ります。

```yaml
Type: System.String[]
Parameter Sets: LiteralPathWithUpdate, LiteralPathWithForce, LiteralPath
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

作成されたアーカイブファイルを表すファイルオブジェクトをコマンドレットが出力するようにします。

このパラメーターは、PowerShell 6.0 で導入されました。

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

アーカイブ zip ファイルに追加するファイルのパスを指定します。 複数のパスを指定し、複数の場所にファイルを含めるには、コンマを使用してパスを区切ります。

このパラメーターは、ワイルドカード文字を受け入れます。 ワイルドカード文字を使用すると、ディレクトリ内のすべてのファイルをアーカイブファイルに追加できます。

ルートディレクトリでワイルドカードを使用すると、アーカイブの内容に影響します。

- ルートディレクトリとそのすべてのファイルとサブディレクトリを **含む** アーカイブを作成するには、 **パス** にワイルドカードを使用せずにルートディレクトリを指定します。 例: `-Path C:\Reference`
- ルートディレクトリを **除外** し、すべてのファイルとサブディレクトリを zip するアーカイブを作成するには、アスタリスク ( `*` ) ワイルドカードを使用します。 例: `-Path C:\Reference\*`
- ルートディレクトリ内のファイルのみを zip するアーカイブを作成するには、 **アスタリスク** (*) のワイルドカードを使用し `*.*` ます。 ルートのサブディレクトリがアーカイブに含まれていません。 例: `-Path C:\Reference\*.*`

```yaml
Type: System.String[]
Parameter Sets: Path, PathWithUpdate, PathWithForce
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -更新

アーカイブ内の古いバージョンのファイルを、同じ名前の新しいファイルバージョンに置き換えることによって、指定されたアーカイブを更新します。 また、このパラメーターを追加して、既存のアーカイブにファイルを追加することもできます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PathWithUpdate, LiteralPathWithUpdate
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

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

### System.String

パイプを使用して、パスを含む文字列を1つ以上のファイルに送ることができます。

## 出力

### システム. IO. FileInfo

コマンドレットは、 **PassThru** パラメーターを使用する場合にのみ、 **FileInfo** オブジェクトを返します。

## 注

再帰を使用してオブジェクトを送信すると、パイプラインによってアーカイブ内のファイルが複製される可能性があります。 たとえば、を `Get-ChildItem` **再帰** パラメーターと共に使用すると、パイプラインに送信される各 **FileInfo** および **DirectoryInfo** オブジェクトがアーカイブに追加されます。

[ZIP ファイルの仕様](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)では、ASCII 以外の文字を含むファイル名をエンコードするための標準的な方法は指定されていません。 コマンドレットでは、 `Compress-Archive` utf-8 エンコードを使用します。 他の ZIP アーカイブツールでは、別のエンコード方式を使用できます。 UTF-8 エンコードを使用して格納されていないファイル名を使用してファイルを抽出すると、は `Expand-Archive` アーカイブで見つかった生の値を使用します。 これにより、アーカイブに格納されているソースファイル名とは異なるファイル名が生成される可能性があります。

## 関連リンク

[Expand-Archive](Expand-Archive.md)

[Get-ChildItem](../Microsoft.PowerShell.Management/Get-ChildItem.md)

