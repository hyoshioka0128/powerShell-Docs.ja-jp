---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 03/12/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/convert-path?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Convert-Path
ms.openlocfilehash: 3d39ea9498cec2669fa075d4630b7691671fea32
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602405"
---
# Convert-Path

## 概要
PowerShell パスから PowerShell プロバイダーパスへのパスを変換します。

## SYNTAX

### パス (既定値)

```
Convert-Path [-Path] <String[]> [<CommonParameters>]
```

### LiteralPath

```
Convert-Path -LiteralPath <String[]> [<CommonParameters>]
```

## Description

`Convert-Path`コマンドレットは、powershell パスのパスを powershell プロバイダーのパスに変換します。

## 例

### 例 1: 作業ディレクトリを標準のファイルシステムパスに変換する

この例では、ドット () で表される現在の作業ディレクトリを `.` 標準のファイルシステムパスに変換します。

```
PS C:\> Convert-Path .
C:\
```

### 例 2: プロバイダーのパスを標準のレジストリパスに変換する

この例では、PowerShell プロバイダーのパスを標準のレジストリパスに変換します。

```powershell
PS C:\> Convert-Path HKLM:\Software\Microsoft
HKEY_LOCAL_MACHINE\Software\Microsoft
```

### 例 3: パスを文字列に変換する

この例では、ファイルシステムプロバイダーである現在のプロバイダーのホームディレクトリへのパスを文字列に変換します。

```powershell
PS C:\> Convert-Path ~
C:\Users\User01
```

## PARAMETERS

### -LiteralPath

文字列配列として、変換するパスを指定します。 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

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

### -Path

変換する PowerShell パスを指定します。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、パスをこのコマンドレットにパイプすることはできますが、リテラルパスは使用できません。

## 出力

### System.String

このコマンドレットは、変換されたパスを含む文字列を返します。

## 注

パスの名詞を含むコマンドレットは、パス名を操作し、すべての PowerShell プロバイダーが解釈できる簡潔な形式の名前を返します。 プログラムやスクリプトで、パス名の全部または一部を特定の形式で表示するために使用することを目的としています。 **Dirname**、 **Normpath**、 **realpath**、 **Join**、またはその他のパスマニピュレーターを使用する場合と同じように使用します。

パス コマンドレットは、FileSystem、Registry、Certificate など、いくつかのプロバイダーで使用できます。

このコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

`Convert-Path` 既存のパスのみを変換します。 まだ存在しない場所を変換するために使用することはできません。

## 関連リンク

[Join-Path](Join-Path.md)

[Resolve-Path](Resolve-Path.md)

[Split-Path](Split-Path.md)

[Test-Path](Test-Path.md)

[Get-PSProvider](Get-PSProvider.md)

