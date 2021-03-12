---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 02/20/2019
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/import-binarymilog?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-BinaryMiLog
ms.openlocfilehash: b31d12f06a8d2e8e226908db9663446baf09a124
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103194256"
---
# Import-BinaryMiLog

## 概要
エクスポートファイルの内容に基づいて、保存されたオブジェクトを再作成するために使用します。

## SYNTAX

```
Import-BinaryMiLog [-Path] <String> [<CommonParameters>]
```

## Description

このコマンドレットを使用して、によって作成されたエクスポートファイルの内容に基づいて、保存されたオブジェクトを再作成し `Export-BinaryMILog` ます。 このコマンドレットはに似ていますが、では `Import-Clixml` 、結果と `Export-BinaryMILog` して得られるオブジェクトがバイナリエンコードファイルに格納される点が異なります。

## 例

### 例 1-ファイルにエクスポートされたオブジェクトを復元する

```powershell
Import-BinaryMiLog -Path "Processes.bmil"
```

## PARAMETERS

### -Path

オブジェクトのバイナリ表現を格納するファイルのパスを指定します。 **Path** パラメーターは、ワイルドカードと相対パスをサポートしています。 このコマンドレットは、パスが複数のファイルに解決された場合にエラーを生成します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

## 出力

### System.Object

## 注

## 関連リンク
