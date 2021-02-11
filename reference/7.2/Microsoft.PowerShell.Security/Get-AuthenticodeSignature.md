---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 04/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-authenticodesignature?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-AuthenticodeSignature
ms.openlocfilehash: 16c61b1fd442eb68c458c3b524a8fc55d5eedcb6
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602791"
---
# Get-AuthenticodeSignature

## 概要
ファイルの Authenticode 署名に関する情報を取得します。

## SYNTAX

### ByPath (既定値)

```
Get-AuthenticodeSignature [-FilePath] <String[]> [<CommonParameters>]
```

### ByLiteralPath

```
Get-AuthenticodeSignature -LiteralPath <String[]> [<CommonParameters>]
```

### ByContent

```
Get-AuthenticodeSignature -SourcePathOrExtension <String[]> -Content <Byte[]> [<CommonParameters>]
```

## Description

`Get-AuthenticodeSignature`コマンドレットは、ファイルまたはファイルのコンテンツの Authenticode 署名に関する情報をバイト配列として取得します。 ファイルが署名されていない場合、情報は取得されますが、フィールドは空白になります。

## 例

### 例 1: ファイルの Authenticode 署名を取得する

```powershell
Get-AuthenticodeSignature -FilePath "C:\Test\NewScript.ps1"
```

このコマンドは、NewScript.ps1 ファイルの Authenticode 署名に関する情報を取得します。 **FilePath** パラメーターを使用してファイルを指定します。

### 例 2: 複数のファイルの Authenticode 署名を取得する

```powershell
Get-AuthenticodeSignature test.ps1, test1.ps1, sign-file.ps1, makexml.ps1
```

このコマンドは、コマンドラインに表示されている4つのファイルの Authenticode 署名に関する情報を取得します。 この例では、 **FilePath** パラメーターの名前 (省略可能) は省略されています。

### 例 3: 複数のファイルに対して有効な Authenticode 署名のみを取得する

```powershell
Get-ChildItem $PSHOME\*.* | ForEach-object {Get-AuthenticodeSignature $_} | Where-Object {$_.status -eq "Valid"}
```

このコマンドは、有効な Authenticode 署名があるディレクトリ内のすべてのファイルを一覧表示 `$PSHOME` します。 自動変数には、 `$PSHOME` PowerShell インストールディレクトリへのパスが含まれます。

このコマンドは、コマンドレットを使用して、 `Get-ChildItem` ディレクトリ内のファイルを取得し `$PSHOME` ます。 この例では、のパターンを使用 *します。* ディレクトリを除外する場合は (ただし、ファイル名にドットが含まれていないファイルも除外します)。

このコマンドは、パイプライン演算子 () を使用して、ファイルを `|` `$PSHOME` コマンドレットに送信し `ForEach-Object` ます。ここで、 `Get-AuthenticodeSignature` は各ファイルに対してを呼び出します。

コマンドの結果は、 `Get-AuthenticodeSignature` 状態が有効な `Where-Object` 署名オブジェクトのみを選択するコマンドに送信されます。

### 例 4: byte 配列として指定されたファイルコンテンツの Authenticode 署名を取得する

```powershell
Get-AuthenticodeSignature -Content (Get-Content foo.ps1 -AsByteStream) -SourcePathorExtension ps1
```

このコマンドは、ファイルの内容の Authenticode 署名に関する情報を取得します。 この例では、ファイル拡張子がファイルの内容と共に指定されています。

## PARAMETERS

### -コンテンツ

ファイルの内容をバイト配列として、Authenticode 署名を取得します。 このパラメーターは、 **Sourcepathorextension** パラメーターと共に使用する必要があります。 ファイルの内容は Unicode (16LE) 形式である必要があります。

```yaml
Type: System.Byte[]
Parameter Sets: ByContent
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -FilePath

検査するファイルのパスを指定します。 ワイルドカードを使用できますが、展開結果が単一のファイルを指す必要があります。 このパラメーターの値を指定する場合、コマンドラインに **FilePath** を入力する必要はありません。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -LiteralPath

検査するファイルへのパスを指定します。 **FilePath** とは異なり、 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符は、エスケープ文字として文字を解釈しないように PowerShell に指示します。

```yaml
Type: System.String[]
Parameter Sets: ByLiteralPath
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -SourcePathOrExtension

Authenticode 署名を取得する対象のコンテンツのファイルまたはファイルの種類へのパス。 このパラメーターは、ファイルの内容がバイト配列として渡される **コンテンツ** で使用されます。

```yaml
Type: System.String[]
Parameter Sets: ByContent
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用して、ファイルパスを含む文字列をにパイプすることができ `Get-AuthenticodeSignature` ます。

## 出力

### システム... Automation. 署名

`Get-AuthenticodeSignature` 取得する各署名の署名オブジェクトを返します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

PowerShell の Authenticode 署名の詳細については、「 [about_Signing](../Microsoft.PowerShell.Core/About/about_Signing.md)」を参照してください。

## 関連リンク

[Get-ExecutionPolicy](Get-ExecutionPolicy.md)

[Set-AuthenticodeSignature](Set-AuthenticodeSignature.md)

[Set-ExecutionPolicy](Set-ExecutionPolicy.md)

[about_Execution_Policies](../Microsoft.PowerShell.Core/About/about_Execution_Policies.md)

[about_Signing](../Microsoft.PowerShell.Core/About/about_Signing.md)
