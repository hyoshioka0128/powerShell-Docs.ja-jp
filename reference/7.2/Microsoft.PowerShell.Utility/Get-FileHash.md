---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 05/16/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-FileHash
ms.openlocfilehash: 0b31409d1da56979887e606b76ddf20532b188c1
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599304"
---
# Get-FileHash

## 概要
指定されたハッシュ アルゴリズムを使用して、ファイルのハッシュ値を計算します。

## SYNTAX

### パス (既定値)

```
Get-FileHash [-Path] <String[]> [[-Algorithm] <String>] [<CommonParameters>]
```

### LiteralPath

```
Get-FileHash [-LiteralPath] <String[]> [[-Algorithm] <String>] [<CommonParameters>]
```

### StreamParameterSet

```
Get-FileHash [-InputStream] <Stream> [[-Algorithm] <String>] [<CommonParameters>]
```

## Description

`Get-FileHash`コマンドレットは、指定されたハッシュアルゴリズムを使用して、ファイルのハッシュ値を計算します。
ハッシュ値とは、ファイルの内容に対応する一意の値です。 ハッシュは、ファイルの内容をファイル名や拡張子などの指定で識別するのではなく、ファイルの内容に一意の値を割り当てます。 ファイル名と拡張子は、ファイルの内容を変更することなく、ハッシュ値を変更することもなく、変更できます。 同様に、ファイルの内容を変更しても、名前や拡張子を変更する必要はありません。 ただし、ファイルの内容を 1 文字でも変更すると、ファイルのハッシュ値は変更されます。

ハッシュ値の目的は、ファイルの内容が変更されていないことを確認するための、暗号化による安全な方法を提供することです。 MD5 や SHA1 などの一部のハッシュアルゴリズムは、攻撃から安全ではないと見なされなくなりましたが、セキュリティで保護されたハッシュアルゴリズムの目的は、誤って、または悪意のある、または不正な試行によってファイルの内容を変更できないようにし、同じハッシュ値を維持することです。 また、ハッシュ値を使用して、2 つの異なるファイルの内容がまったく同じかどうかを確認することもできます。 2 つのファイルのハッシュ値が同一の場合は、ファイルの内容も同一です。

既定では、 `Get-FileHash` コマンドレットは SHA256 アルゴリズムを使用しますが、ターゲットオペレーティングシステムでサポートされている任意のハッシュアルゴリズムを使用できます。

## 例

### 例 1: ファイルのハッシュ値を計算する

この例では、コマンドレットを使用して、 `Get-FileHash` ファイルのハッシュ値を計算し `/etc/apt/sources.list` ます。 使用されるハッシュアルゴリズムは、既定の **SHA256** です。 出力をコマンドレットにパイプし `Format-List` て、出力を一覧として書式設定します。

```powershell
Get-FileHash /etc/apt/sources.list | Format-List
```

```Output
Algorithm : SHA256
Hash      : 3CBCFDDEC145E3382D592266BE193E5BE53443138EE6AB6CA09FF20DF609E268
Path      : /etc/apt/sources.list
```

### 例 2: ISO ファイルのハッシュ値を計算する

この例では、 `Get-FileHash` コマンドレットと **SHA384** アルゴリズムを使用して、管理者がインターネットからダウンロードした ISO ファイルのハッシュ値を計算します。 出力をコマンドレットにパイプし `Format-List` て、出力を一覧として書式設定します。

```powershell
Get-FileHash C:\Users\user1\Downloads\Contoso8_1_ENT.iso -Algorithm SHA384 | Format-List
```

```Output
Algorithm : SHA384
Hash      : 20AB1C2EE19FC96A7C66E33917D191A24E3CE9DAC99DB7C786ACCE31E559144FEAFC695C58E508E2EBBC9D3C96F21FA3
Path      : C:\Users\user1\Downloads\Contoso8_1_ENT.iso
```

### 例 3: ストリームのハッシュ値を計算する

この例では、 **System .Net WebClient** を使用して、 [Powershell リリースページ](https://github.com/PowerShell/PowerShell/releases/tag/v6.2.4)からパッケージをダウンロードします。 また、リリースページには、各パッケージファイルの SHA256 ハッシュも記載されています。 発行されたハッシュ値を、計算したものと比較することができ `Get-FileHash` ます。

```powershell
$wc = [System.Net.WebClient]::new()
$pkgurl = 'https://github.com/PowerShell/PowerShell/releases/download/v6.2.4/powershell_6.2.4-1.debian.9_amd64.deb'
$publishedHash = '8E28E54D601F0751922DE24632C1E716B4684876255CF82304A9B19E89A9CCAC'
$FileHash = Get-FileHash -InputStream ($wc.OpenRead($pkgurl))
$FileHash.Hash -eq $publishedHash
```

```Output
True
```

### 例 4: 文字列のハッシュを計算する

PowerShell には、文字列のハッシュを計算するコマンドレットが用意されていません。 ただし、文字列をストリームに書き込んで、の **InputStream** パラメーターを使用してハッシュ値を取得することはでき `Get-FileHash` ます。

```powershell
$stringAsStream = [System.IO.MemoryStream]::new()
$writer = [System.IO.StreamWriter]::new($stringAsStream)
$writer.write("Hello world")
$writer.Flush()
$stringAsStream.Position = 0
Get-FileHash -InputStream $stringAsStream | Select-Object Hash
```

```Output
Hash
----
64EC88CA00B268E5BA1A35678A1B5316D212F4F366B2477232534A8AECA37F3C
```

## PARAMETERS

### -アルゴリズム

指定したファイルまたはストリームの内容のハッシュ値を計算するために使用する暗号ハッシュ関数を指定します。 暗号ハッシュ関数には、同じハッシュ値を持つ2つの異なるファイルを検索することが不可能であるというプロパティがあります。 ハッシュ関数は、データの整合性を確保するために、よくデジタル署名と共に使用されます。 このパラメーターの有効値は、次のとおりです。

- SHA1
- SHA256
- SHA384
- SHA512
- MD5

値を指定しなかった場合、またはパラメーターを省略した場合、既定値は SHA256 です。

セキュリティ上の理由により、安全と見なされなくなった MD5 と SHA1 は、単純な変更の検証にのみ使用し、攻撃や改ざんからの保護が必要なファイルのハッシュ値の生成には使用しないでください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: SHA1, SHA256, SHA384, SHA512, MD5

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputStream

入力ストリームを指定します。

```yaml
Type: System.IO.Stream
Parameter Sets: StreamParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath

ファイルのパスを指定します。 **Path** パラメーターと異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符は、文字がエスケープシーケンスとして解釈されないように PowerShell に指示します。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

1つ以上のファイルへのパスを配列として指定します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

1つ以上のファイルへのパスを含む文字列をコマンドレットにパイプすることができ `Get-FileHash` ます。

## 出力

### Microsoft. Powershell. FileHash

`Get-FileHash` 指定したファイルへのパス、計算されたハッシュの値、およびハッシュの計算に使用されるアルゴリズムを表すオブジェクトを返します。

## 注

## 関連リンク

[Format-List](Format-List.md)

