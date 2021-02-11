---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 03/12/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/resolve-path?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Resolve-Path
ms.openlocfilehash: 0481526aead285e3d5fb494218d1573e03216f2e
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602778"
---
# Resolve-Path

## 概要
パス中のワイルドカード文字を解決し、パスの内容を表示します。

## SYNTAX

### パス (既定値)

```
Resolve-Path [-Path] <String[]> [-Relative] [-Credential <PSCredential>] [<CommonParameters>]
```

### LiteralPath

```
Resolve-Path -LiteralPath <String[]> [-Relative] [-Credential <PSCredential>] [<CommonParameters>]
```

## Description

`Resolve-Path`コマンドレットは、指定された場所にあるワイルドカードパターンに一致する項目とコンテナーを表示します。 一致には、ファイル、フォルダー、レジストリキー、または PSDrive プロバイダーからアクセスできるその他のオブジェクトを含めることができます。

## 例

### 例 1: ホームフォルダーのパスを解決する

チルダ文字 (~) は、現在のユーザーのホームフォルダーの短縮表記です。 次の例 `Resolve-Path` では、完全修飾パス値を返します。

```powershell
PS C:\> Resolve-Path ~
```

```Output
Path
----
C:\Users\User01
```

### 例 2: Windows フォルダーのパスを解決する

```powershell
PS C:\> Resolve-Path -Path "windows"
```

```Output
Path
----
C:\Windows
```

C: ドライブのルートから実行すると、このコマンドは C: ドライブ内の Windows フォルダーのパスを返します。

### 例 3: Windows フォルダー内のすべてのパスを取得する

```powershell
PS C:\> "C:\windows\*" | Resolve-Path
```

このコマンドは、C:\Windows フォルダー内のすべてのフォルダーを返します。 このコマンドは、パイプライン演算子 (|) を使用して、パス文字列をに送信し `Resolve-Path` ます。

### 例 4: UNC パスを解決する

```powershell
PS C:\> Resolve-Path -Path "\\Server01\public"
```

このコマンドは、汎用名前付け規則 (UNC) パスを解決し、そのパス以下の共有を返します。

### 例 5: 相対パスを取得する

```powershell
PS C:\> Resolve-Path -Path "c:\prog*" -Relative
```

```Output
.\Program Files
.\Program Files (x86)
.\programs.txt
```

このコマンドは、C: ドライブのルートにあるディレクトリの相対パスを返します。

### 例 6: 角かっこを含むパスを解決する

この例では、 **LiteralPath** パラメーターを使用して、テスト \[ xml サブフォルダーのパスを解決し \] ます。
**LiteralPath** を使用すると、角かっこが正規表現ではなく通常の文字として扱われます。

```powershell
PS C:\> Resolve-Path -LiteralPath 'test[xml]'
```

## PARAMETERS

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。
既定値は現在のユーザーです。

User01 や Domain01\user01」などのユーザー名を入力するか、 **PSCredential** オブジェクトを渡します。 コマンドレットを使用して **PSCredential** オブジェクトを作成でき `Get-Credential` ます。 ユーザー名を入力すると、このコマンドレットによってパスワードの入力が求められます。

このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。

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

### -LiteralPath

解決するパスを指定します。
**LiteralPath** パラメーターの値は、型指定されたとおりに使用されます。
ワイルドカードとして解釈される文字はありません。
パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。
単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

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

解決する PowerShell パスを指定します。
このパラメーターは必須です。
パイプを使用してパス文字列をにパイプすることもでき `Resolve-Path` ます。
ワイルドカード文字を使用できます。

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

### -相対

このコマンドレットが相対パスを返すことを示します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用して、このコマンドレットへのパスを含む文字列を

## 出力

### システムの管理. PathInfo、System.string

**Pathinfo** オブジェクトを返します。 **相対** パラメーターを指定した場合、解決されたパスの文字列値を返します。

## 注

`*-Path`コマンドレットは、ファイルシステム、レジストリ、および証明書プロバイダーで動作します。

`Resolve-Path` は、任意のプロバイダーと連携するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「 [about_providers](../microsoft.powershell.core/about/about_providers.md)」を参照してください。

`Resolve-Path` は既存のパスのみを解決します。 まだ存在しない場所を解決するために使用することはできません。

## 関連リンク

[Convert-Path](Convert-Path.md)

[Join-Path](Join-Path.md)

[Split-Path](Split-Path.md)

[Test-Path](Test-Path.md)

