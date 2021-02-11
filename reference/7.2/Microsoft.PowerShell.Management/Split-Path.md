---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/split-path?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Split-Path
ms.openlocfilehash: e2498c02d42123e207b49e622654d3cb881fc0fc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602418"
---
# Split-Path

## 概要
指定されたパス部分を返します。

## SYNTAX

### ParentSet (既定値)

```
Split-Path [-Path] <String[]> [-Parent] [-Resolve] [-Credential <PSCredential>]
 [<CommonParameters>]
```

### LeafSet

```
Split-Path [-Path] <String[]> -Leaf [-Resolve] [-Credential <PSCredential>] [<CommonParameters>]
```

### LeafBaseSet

```
Split-Path [-Path] <String[]> -LeafBase [-Resolve] [-Credential <PSCredential>]
 [<CommonParameters>]
```

### ExtensionSet

```
Split-Path [-Path] <String[]> -Extension [-Resolve] [-Credential <PSCredential>]
 [<CommonParameters>]
```

### QualifierSet

```
Split-Path [-Path] <String[]> -Qualifier [-Resolve] [-Credential <PSCredential>]
 [<CommonParameters>]
```

### NoQualifierSet

```
Split-Path [-Path] <String[]> -NoQualifier [-Resolve] [-Credential <PSCredential>]
 [<CommonParameters>]
```

### IsAbsoluteSet

```
Split-Path [-Path] <String[]> [-Resolve] -IsAbsolute [-Credential <PSCredential>]
 [<CommonParameters>]
```

### LiteralPathSet

```
Split-Path -LiteralPath <String[]> [-Resolve] [-Credential <PSCredential>] [<CommonParameters>]
```

## Description

`Split-Path`このコマンドレットは、親フォルダー、サブフォルダー、ファイル名など、指定されたパスの部分のみを返します。 また、分割されたパスで参照されている項目を取得したり、パスが相対パスか絶対パスかを判断したりすることもできます。

このコマンドレットを使用すると、選択したパスの一部だけを取得または送信できます。

## 例

### 例 1: パスの修飾子を取得する

```powershell
Split-Path -Path "HKCU:\Software\Microsoft" -Qualifier
```

```Output
HKCU:
```

このコマンドは、パスの修飾子だけを返します。 修飾子はドライブです。

### 例 2: ファイル名を表示する

```powershell
Split-Path -Path "C:\Test\Logs\*.log" -Leaf -Resolve
```

```Output
Pass1.log
Pass2.log
...
```

このコマンドを実行すると、分割されたパスが参照しているファイルが表示されます。 このパスは最後の項目 (リーフとも呼ばれます) に分割されるので、コマンドはファイル名のみを表示します。

**Resolve** パラメーターは、分割パスを表示 `Split-Path` するのではなく、分割パスによって参照される項目を表示するように指示します。

すべて `Split-Path` のコマンドと同様に、このコマンドは文字列を返します。 ファイルを表す **FileInfo** オブジェクトは返しません。

### 例 3: 親コンテナーを取得する

```powershell
Split-Path -Path "C:\WINDOWS\system32\WindowsPowerShell\V1.0\about_*.txt"
```

```Output
C:\WINDOWS\system32\WindowsPowerShell\V1.0
```

このコマンドは、パスの親コンテナーだけを返します。 分割を指定するパラメーターが含まれていないので、で `Split-Path` は、 **親** である分割位置の既定値が使用されます。

### 例 4: 絶対パスであるかどうかを判断する

```powershell
Split-Path -Path ".\My Pictures\*.jpg" -IsAbsolute
```

```Output
False
```

このコマンドは、パスが相対パスと絶対パスのどちらであるかを特定します。 この場合、パスはドット () で表される現在のフォルダーに対して相対的であるため、 `.` を返し `$False` ます。

### 例 5: 指定したパスに場所を変更する

```powershell
PS C:\> Set-Location (Split-Path -Path $profile)
PS C:\Documents and Settings\User01\My Documents\WindowsPowerShell>
```

このコマンドは、PowerShell プロファイルが格納されているフォルダーに場所を変更します。

かっこ内のコマンドは、を使用して、 `Split-Path` 組み込み変数に格納されているパスの親だけを返し `$Profile` ます。 **親** パラメーターは、既定の分割位置パラメーターです。
このため、コマンドから省略できます。 かっこでは、最初にコマンドを実行するように PowerShell を指示します。 これは、長いパス名を持つフォルダーに移動する場合に便利な方法です。

### 例 6: パイプラインを使用してパスを分割する

```powershell
'C:\Documents and Settings\User01\My Documents\My Pictures' | Split-Path
```

```Output
C:\Documents and Settings\User01\My Documents
```

このコマンドは、パイプライン演算子 () を使用して、 `|` パスをに送信 `Split-Path` します。 パスを引用符で囲み、1 つのトークンであることを示します。

## PARAMETERS

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、 [Invoke コマンド](../Microsoft.PowerShell.Core/Invoke-Command.md)を使用します。

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

### -拡張機能

このコマンドレットがリーフの拡張のみを返すことを示します。 たとえば、パスでは、 `C:\Test\Logs\Pass1.log` のみが返さ `.log` れます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ExtensionSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -IsAbsolute

このコマンドレットが絶対パスの場合に $True を返し、相対パスの場合は $False することを示します。 絶対パスの長さはゼロより大きく、ドット () を使用して `.` 現在のパスを示すことはできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: IsAbsoluteSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -リーフ

このコマンドレットがパス内の最後の項目またはコンテナーだけを返すことを示します。 たとえば、パスでは、 `C:\Test\Logs\Pass1.log` Pass1 のみが返されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: LeafSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -LeafBase

このコマンドレットがリーフの基本名だけを返すことを示します。 たとえば、パスでは、 `C:\Test\Logs\Pass1.log` のみが返さ `Pass1` れます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: LeafBaseSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -LiteralPath

分割するパスを指定します。 **Path** とは異なり、**LiteralPath** の値は入力した内容のまま使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralPathSet
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NoQualifier

このコマンドレットが修飾子なしでパスを返すことを示します。 ファイルシステムまたはレジストリプロバイダーの場合、修飾子はやなどのプロバイダーパスのドライブです `C:` `HKCU:` 。 たとえば、パスでは、 `C:\Test\Logs\Pass1.log` のみが返さ `\Test\Logs\Pass1.log` れます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NoQualifierSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -親

このコマンドレットが、パスで指定された項目またはコンテナーの親コンテナーだけを返すことを示します。 たとえば、パスでは、 `C:\Test\Logs\Pass1.log` が返さ `C:\Test\Logs` れます。
**親** パラメーターは、既定の分割位置パラメーターです。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ParentSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

分割するパスを指定します。 ワイルドカード文字を使用できます。 パスにスペースが含まれる場合は、二重引用符で囲みます。 パイプを使用してこのコマンドレットへのパスをパイプすることもできます。

```yaml
Type: System.String[]
Parameter Sets: ParentSet, LeafSet, LeafBaseSet, ExtensionSet, QualifierSet, NoQualifierSet, IsAbsoluteSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -修飾子

このコマンドレットが、指定されたパスの修飾子だけを返すことを示します。 ファイルシステムまたはレジストリプロバイダーの場合、修飾子はやなどのプロバイダーパスのドライブです `C:` `HKCU:` 。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: QualifierSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -解決

このコマンドレットによって、パス要素を表示する代わりに、結果として得られる分割パスによって参照される項目が表示されることを示します。

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、このコマンドレットへのパスを含む文字列をパイプすることができます。

## 出力

### System.string、System.string

`Split-Path` 文字列を返します。 **Resolve** パラメーターを指定すると、は `Split-Path` 項目の場所を説明する文字列を返します。 **FileInfo** や **RegistryKey** オブジェクトなどの項目を表すオブジェクトは返しません。

**Isabsolute** パラメーターを指定すると、は `Split-Path` **ブール** 値を返します。

## 注

- 分割位置パラメーター (**Qualifier**、 **Parent**、 **Extension**、 **リーフ**、 **LeafBase**、および **noqualifier**) は排他的です。 各コマンドでいずれか 1 つだけを使用できます。

- **パスの名詞を** 含むコマンドレット ( **path** コマンドレット) は、パス名と共に使用され、すべての PowerShell プロバイダーが解釈できる簡潔な形式の名前を返します。 プログラムやスクリプトで、パス名の全部または一部を特定の形式で表示するために使用することを目的としています。 これらは、 **Dirname**、 **Normpath**、 **realpath**、 **Join**、またはその他のパスマニピュレーターを使用する方法で使用します。

- **Path** コマンドレットは、いくつかのプロバイダーと共に使用できます。 これには、ファイルシステム、レジストリ、および証明書プロバイダーが含まれます。

- `Split-Path` は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「about_Providers」を参照してください。

## 関連リンク

[Convert-Path](Convert-Path.md)

[Join-Path](Join-Path.md)

[Resolve-Path](Resolve-Path.md)

[Test-Path](Test-Path.md)
