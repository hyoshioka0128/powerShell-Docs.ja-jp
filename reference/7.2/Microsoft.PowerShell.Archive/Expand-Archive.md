---
external help file: Microsoft.PowerShell.Archive-help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Archive
ms.date: 07/17/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Expand-Archive
ms.openlocfilehash: a4cf8970156f524578f7c9747aef1ffbf9f3eb58
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603234"
---
# Expand-Archive

## 概要
指定したアーカイブ (zip 形式) ファイルからファイルを抽出します。

## SYNTAX

### パス (既定値)

```
Expand-Archive [-Path] <String> [[-DestinationPath] <String>] [-Force] [-PassThru] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### LiteralPath

```
Expand-Archive -LiteralPath <String> [[-DestinationPath] <String>] [-Force] [-PassThru] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットは、指定した `Expand-Archive` zip 形式のアーカイブファイルから、指定した保存先フォルダーにファイルを抽出します。 アーカイブファイルを使用すると、配布と保存を容易にするために、複数のファイルを1つの zip ファイルにパッケージ化し、必要に応じて圧縮することができます。

## 例

### 例 1: アーカイブの内容を抽出する

この例では、 **DestinationPath** パラメーターで指定したフォルダーに既存のアーカイブファイルの内容を抽出します。

```powershell
Expand-Archive -LiteralPath 'C:\Archives\Draft[v1].Zip' -DestinationPath C:\Reference
```

この例では、 **LiteralPath** パラメーターが使用されています。これは、ファイル名にワイルドカードとして解釈される可能性のある文字が含まれているためです。

### 例 2: 現在のフォルダー内のアーカイブの内容を抽出する

この例では、現在のフォルダーにある既存のアーカイブファイルの内容を、 **DestinationPath** パラメーターで指定したフォルダーに抽出します。

```powershell
Expand-Archive -Path Draftv2.Zip -DestinationPath C:\Reference
```

## PARAMETERS

### -DestinationPath

既定では、は、 `Expand-Archive` ZIP ファイルと同じ名前のフォルダーを現在の場所に作成します。 パラメーターを使用すると、別のフォルダーへのパスを指定できます。 ターゲットフォルダーが存在しない場合は作成されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: A folder in the current location
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

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

### -LiteralPath

アーカイブファイルのパスを指定します。 **Path** パラメーターと異なり、**LiteralPath** の値は入力したとおりに使用されます。 ワイルドカード文字はサポートされていません。 パスにエスケープ文字が含まれている場合は、各エスケープ文字を単一引用符で囲み、すべての文字をエスケープシーケンスとして解釈しないように PowerShell に指示します。

```yaml
Type: System.String
Parameter Sets: LiteralPath
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

コマンドレットによって、アーカイブから展開されたファイルの一覧が出力されます。

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

アーカイブファイルのパスを指定します。

```yaml
Type: System.String
Parameter Sets: Path
Aliases:

Required: True
Position: 0
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

### System.String

パイプを使用して、既存のアーカイブファイルへのパスを含む文字列をパイプすることができます。

## 出力

### System.servicemodel. FileSystemInfo

パラメーターを使用すると、 `-PassThru` アーカイブから展開されたファイルの一覧がコマンドレットによって出力されます。

## 注

[ZIP ファイルの仕様](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)では、ASCII 以外の文字を含むファイル名をエンコードするための標準的な方法は指定されていません。 コマンドレットでは、 `Compress-Archive` utf-8 エンコードを使用します。 他の ZIP アーカイブツールでは、別のエンコード方式を使用できます。 UTF-8 エンコードを使用して格納されていないファイル名を使用してファイルを抽出すると、は `Expand-Archive` アーカイブで見つかった生の値を使用します。 これにより、アーカイブに格納されているソースファイル名とは異なるファイル名が生成される可能性があります。

## 関連リンク

[Compress-Archive](compress-archive.md)
