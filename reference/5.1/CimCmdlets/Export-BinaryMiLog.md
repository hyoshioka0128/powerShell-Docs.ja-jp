---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: CimCmdlets
ms.date: 02/20/2019
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/export-binarymilog?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-BinaryMiLog
ms.openlocfilehash: 1d202b7bbda359f859838475eb9f37730506bd1f
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103194364"
---
# Export-BinaryMiLog

## 概要
オブジェクトのバイナリエンコードされた表現を作成し、ファイルに格納します。

## SYNTAX

```
Export-BinaryMiLog [-InputObject <CimInstance>] [-Path] <String> [<CommonParameters>]
```

## Description

`Export-BinaryMILog`コマンドレットでは、オブジェクトまたはオブジェクトのバイナリベースの表現を作成し、ファイルに格納します。 その後、コマンドレットを使用して、 `Import-BinaryMiLog` そのファイルの内容に基づいて保存されたオブジェクトを再作成できます。

このコマンドレットはに似ていますが、では `Import-Clixml` 、結果と `Export-BinaryMILog` して得られるオブジェクトがバイナリエンコードファイルに格納される点が異なります。

## 例

### 例 1-CimInstances のバイナリ表現を作成する

```powershell
Get-CimInstance Win32_Process | Export-BinaryMiLog -Path "Processes.bmil"
```

このコマンドは、Path パラメーターで指定されたバイナリ MI ログファイルに **CimInstances** をエクスポートします。 このファイルをインポートして **CimInstances** を再作成する方法については、Import-BinaryMiLog の例を参照してください。

## PARAMETERS

### -InputObject

このコマンドレットへの入力を指定します。 このパラメーターを使用するか、またはこのコマンドレットへの入力をパイプで渡すことができます。

```yaml
Type: Microsoft.Management.Infrastructure.CimInstance
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

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

### Microsoft.Management.Infrastructure.CimInstance

## 出力

### System.Object

## 注

## 関連リンク

[Get-CimInstance](get-ciminstance.md)

[Import-BinaryMiLog](import-binarymilog.md)

[Import-Clixml](../microsoft.powershell.utility/import-clixml.md)
