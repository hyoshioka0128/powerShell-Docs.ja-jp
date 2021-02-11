---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 11/02/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/new-filecatalog?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-FileCatalog
ms.openlocfilehash: 572a1cfe5b6b69397f792f38153f36d5e038c161
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94343362"
---
# New-FileCatalog

## 概要
`New-FileCatalog` ファイルの信頼性を検証するために使用できるファイルハッシュのカタログファイルを作成します。

## SYNTAX

```
New-FileCatalog [-CatalogVersion <Int32>] [-CatalogFilePath] <String> [[-Path] <String[]>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`New-FileCatalog` 一連のフォルダーとファイルに対して [Windows カタログファイル](/windows-hardware/drivers/install/catalog-files) を作成します。 このカタログファイルには、指定されたパス内のすべてのファイルのハッシュが含まれています。 その後、ユーザーは、カタログの作成時以降にフォルダーに変更が加えられたかどうかをユーザーが検証できるように、カタログをファイルと共に配布できます。

カタログ バージョン 1 と 2 がサポートされています。 バージョン1は、(非推奨) SHA1 ハッシュアルゴリズムを使用してファイルハッシュを作成し、バージョン2は SHA256 を使用します。 Windows Server 2008 R2 と Windows 7 はカタログ バージョン 2 に対応していません。 カタログ バージョン 2 は Windows 8、Windows Server 2012 以降のオペレーティング システムで利用する必要があります。

## 例

### 例 1: のファイルカタログを作成する `Microsoft.PowerShell.Utility`

```powershell
New-FileCatalog -Path $PSHOME\Modules\Microsoft.PowerShell.Utility -CatalogFilePath \temp\Microsoft.PowerShell.Utility.cat -CatalogVersion 2.0
```

```Output
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         11/2/2018 11:58 AM            950 Microsoft.PowerShell.Utility.cat
```

## PARAMETERS

### -CatalogFilePath

カタログファイル (.cat) を配置するファイルまたはフォルダーへのパス。 フォルダーパスを指定した場合は、既定のファイル名が `catalog.cat` 使用されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -CatalogVersion

`1.0`は `2.0` 、カタログのバージョンを指定するために、またはを有効な値として受け入れます。 `1.0` 可能な限り回避する必要があります。セキュリティで保護されていない SHA-1 ハッシュアルゴリズムが使用されるのに `2.0` 対し、は SECURE sha-256 アルゴリズムを使用し `1.0` ますが、は Windows 7 および Server 2008r2 で唯一サポートされているアルゴリズムです。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

カタログファイルに含める必要があるファイルまたはフォルダーへのパスまたはパスの配列を指定します。 フォルダーが指定されている場合は、フォルダー内のすべてのファイルも含められます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
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

### System.String

パイプラインは、カタログファイル名として使用される文字列を受け取ります。

## 出力

### システム. IO. FileInfo

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク

[Test-FileCatalog](Test-FileCatalog.md)

[PowerShellGet](/powerShell/module/powershellget)
