---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/join-path?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Join-Path
ms.openlocfilehash: 08107eecce316c3799315b1d91a0e7cdab64579f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604960"
---
# Join-Path

## 概要
パスと子パスを 1 つのパスに結合します。

## SYNTAX

```
Join-Path [-Path] <String[]> [-ChildPath] <String> [[-AdditionalChildPath] <String[]>] [-Resolve]
 [-Credential <PSCredential>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Join-Path` パスと子パスを1つのパスに結合します。
パスの区切り記号はプロバイダーによって追加されます。

## 例

### 例 1: パスと子パスを結合する

```powershell
PS C:\> Join-Path -Path "path" -ChildPath "childpath"
```

```output
path\childpath
```

このコマンドは、を使用し `Join-Path` てパスを childpath と結合します。

コマンドはプロバイダーから実行されるため、 `FileSystem` `\` パスを結合するための区切り記号を提供します。

### 例 2: ディレクトリの区切り記号が既に含まれているパスを結合する

```powershell
PS C:\> Join-Path -Path "path\" -ChildPath "\childpath"
```

```output
path\childpath
```

既存のディレクトリの区切り記号 `\` と処理されるため `Path` 、との間に区切り記号が1つだけ存在します。 `ChildPath`

### 例 3: 子パスをパスに結合してファイルとフォルダーを表示する

```powershell
Join-Path "C:\win*" "System*" -Resolve
```

このコマンドは、C:\ Win \* パスと System 子パスを結合することによって参照されるファイルとフォルダーを表示し \* ます。
と同じファイルおよびフォルダーが表示され `Get-ChildItem` ますが、各項目の完全修飾パスが表示されます。
このコマンドでは、 `Path` および `ChildPath` 省略可能なパラメーター名は省略されています。

### 例 4: PowerShell レジストリプロバイダーで Join-Path を使用する

```powershell
PS HKLM:\> Join-Path -Path System -ChildPath *ControlSet* -Resolve
```

```output
HKLM:\System\ControlSet001
HKLM:\System\CurrentControlSet
```

このコマンドを実行すると、 `HKLM\System` を含むレジストリサブキーのレジストリキーが表示され `ControlSet` ます。

パラメーターは、 `Resolve` 現在のプロバイダーパスのワイルドカードを含む、結合されたパスの解決を試みます。 `HKLM:\`

### 例 5: 複数のパスルートと子パスを結合する

```powershell
Join-Path -Path C:, D:, E:, F: -ChildPath New
```

```output
C:\New
D:\New
E:\New
F:\New
```

このコマンドは `Join-Path` 、を使用して、複数のパスルートと子パスを結合します。

> [!NOTE]
> によって指定されたドライブ `Path` が存在する必要があります。存在しない場合、そのエントリの結合は失敗します。

### 例 6: ファイルシステムドライブのルートと子パスを結合する

```powershell
Get-PSDrive -PSProvider filesystem | ForEach-Object {$_.root} | Join-Path -ChildPath "Subdir"
```

```output
C:\Subdir
D:\Subdir
```

このコマンドは、コンソールの各 PowerShell ファイルシステムドライブのルートと Subdir 子パスを結合します。

このコマンドは、コマンドレットを使用して、 `Get-PSDrive` FileSystem プロバイダーによってサポートされる PowerShell ドライブを取得します。
ステートメントは、 `ForEach-Object` オブジェクトの Root プロパティのみ `PSDriveInfo` を選択し、指定された子パスと結合します。

出力には、コンピューター上の PowerShell ドライブに、C:\Program Files ディレクトリにマップされたドライブが含まれていることが示されています。

### 例 7: 不特定数のパスを結合する

```powershell
Join-Path a b c d e f g
```

```Output
a\b\c\d\e\f\g
```

パラメーターを使用すると `AdditionalChildPath` 、無制限の数のパスを結合できます。

この例では、パラメーター名が使用されていないため、" `Path` b" と " `ChildPath` c-g" をにバインドします。 `AdditionalChildPath`

## PARAMETERS

### -AdditionalChildPath

*Path* パラメーターの値に追加する要素を指定します。 `ChildPath`パラメーターは必須ですが、指定する必要もあります。

このパラメーターは、 `ValueFromRemainingArguments` 不特定数のパスに参加できるようにするプロパティで指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ChildPath

パラメーターの値に追加する要素を指定し `Path` ます。
ワイルドカードを使用できます。
パラメーター `ChildPath` は必須ですが、パラメーター名 ("ChildPath") は省略可能です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。
> 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、 [Invoke コマンド](../Microsoft.PowerShell.Core/Invoke-Command.md)を使用します。

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

### -Path

子パスを追加するメイン パスを 1 つ以上指定します。
ワイルドカードを使用できます。

の値によって、 `Path` どのプロバイダーがパスを結合し、パスの区切り記号が追加されます。
パラメーター `Path` は必須ですが、パラメーター名 ("Path") は省略可能です。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PSPath

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -解決

このコマンドレットが、現在のプロバイダーからの結合されたパスを解決しようとすることを示します。

- ワイルドカードを使用すると、コマンドレットは、結合されたパスに一致するすべてのパスを返します。
- ワイルドカードが使用されて **いない場合** 、パスが存在しない場合は、コマンドレットでエラーが発生します。

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

### System.String

このコマンドレットは、結果として得られるパスを含む文字列を返します。

## 注

パスの名詞を含むコマンドレット (Path コマンドレット) は、パス名を操作し、すべての PowerShell プロバイダーが解釈できる簡潔な形式の名前を返します。 プログラムやスクリプトで、パス名の全部または一部を特定の形式で表示するために使用することを目的としています。 Dirname、Normpath、Realpath、Join など、他のパス操作コマンドレットと同じように使用します。

Path コマンドレットは、、、およびプロバイダーを含むいくつかのプロバイダーで使用でき `FileSystem` `Registry` `Certificate` ます。

このコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。
セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。
詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Convert-Path](Convert-Path.md)

[Resolve-Path](Resolve-Path.md)

[Split-Path](Split-Path.md)

[Test-Path](Test-Path.md)

[Get-PSProvider](Get-PSProvider.md)

[Get-ChildItem](Get-ChildItem.md)

[Get-PSDrive](Get-PSDrive.md)

