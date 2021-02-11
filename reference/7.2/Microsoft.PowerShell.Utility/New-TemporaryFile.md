---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 03/16/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-temporaryfile?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-TemporaryFile
ms.openlocfilehash: 1c66cd3b1cc2fccc58cd75c367b41c445deb1e72
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599300"
---
# New-TemporaryFile

## 概要
一時ファイルを作成します。

## SYNTAX

```
New-TemporaryFile [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

このコマンドレットは、スクリプトで使用できる一時ファイルを作成します。

コマンドレットでは、 `New-TemporaryFile` ファイル名拡張子を持つ空のファイルを作成し `.tmp` ます。
このコマンドレットは、ファイルに名前 `tmp<NNNN>.tmp` を `<NNNN>` 指定します。は、ランダムな16進数です。
コマンドレットにより、 **一時** フォルダーにファイルが作成されます。

このコマンドレットは、 [GetTempPath ()](/dotnet/api/system.io.path.gettemppath) メソッドを使用して、 **一時** フォルダーを検索します。 このメソッドは、次の順序で環境変数の存在を確認し、見つかった最初のパスを使用します。

- Windows プラットフォームの場合:

  1. TMP 環境変数によって指定されたパス。
  1. TEMP 環境変数によって指定されたパス。
  1. USERPROFILE 環境変数によって指定されたパス。
  1. Windows ディレクトリ。

- Windows 以外のプラットフォームの場合: は、TMPDIR 環境変数で指定されたパスを使用します。

## 例

### 例 1: 一時ファイルを作成する

```powershell
$TempFile = New-TemporaryFile
```

このコマンドは、 `.tmp` 一時フォルダーにファイルを生成し、そのファイルへの参照を変数に格納し `$TempFile` ます。 このファイルは、後でスクリプト内で使用できます。

## PARAMETERS

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

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

## 出力

### システム. IO. FileInfo

このコマンドレットは、一時ファイルを表す **FileInfo** オブジェクトを返します。

## 注

## 関連リンク

