---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-item?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Item
ms.openlocfilehash: e848cc8c77e1d0dff6eb1f98d56c8ed37e44a653
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "97692807"
---
# Get-Item

## 概要
指定された場所にある項目を取得します。

## SYNTAX

### パス (既定値)

```
Get-Item [-Path] <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-Force]
 [-Credential <PSCredential>] [-Stream <String[]>] [<CommonParameters>]
```

### LiteralPath

```
Get-Item -LiteralPath <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>]
 [-Force] [-Credential <PSCredential>] [-Stream <String[]>] [<CommonParameters>]
```

## Description

`Get-Item`コマンドレットは、指定した場所にある項目を取得します。 ワイルドカード文字 () を使用して `*` 項目のすべての内容を要求しない限り、その場所にある項目の内容は取得されません。

このコマンドレットは、さまざまな種類のデータストア間を移動するために、PowerShell プロバイダーによって使用されます。

## 例

### 例 1: 現在のディレクトリを取得する

この例では、現在のディレクトリを取得します。 ドット ('. ') は、その内容ではなく、現在の場所にある項目を表します。

```powershell
Get-Item .
```

```output
Directory: C:\

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         7/26/2006  10:01 AM            ps-test
```

### 例 2: 現在のディレクトリ内のすべての項目を取得する

この例では、現在のディレクトリ内のすべての項目を取得します。 ワイルドカード文字 ( `*` ) は、現在の項目のすべての内容を表します。

```powershell
Get-Item *
```

```output
Directory: C:\ps-test

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         7/26/2006   9:29 AM            Logs
d----         7/26/2006   9:26 AM            Recs
-a---         7/26/2006   9:28 AM         80 date.csv
-a---         7/26/2006  10:01 AM         30 filenoext
-a---         7/26/2006   9:30 AM      11472 process.doc
-a---         7/14/2006  10:47 AM         30 test.txt
```

### 例 3: ドライブの現在のディレクトリを取得する

この例では、ドライブの現在のディレクトリを取得し `C:` ます。 ここで取得するオブジェクトは、ディレクトリのみを表します。その内容は表しません。

```powershell
Get-Item C:\
```

### 例 4: 指定したドライブの項目を取得する

この例では、ドライブ内の項目を取得し `C:` ます。 ワイルドカード文字 () は、コンテナー `*` だけでなく、コンテナー内のすべての項目を表します。

```powershell
Get-Item C:\*
```

PowerShell では、1つのアスタリスク () を使用し `*` て、従来のではなくコンテンツを取得し `*.*` ます。 形式は文字どおりに解釈されるため、 `*.*` ドットなしでディレクトリやファイル名を取得することはできません。

### 例 5: 指定したディレクトリ内のプロパティを取得する

この例では、ディレクトリの **LastAccessTime** プロパティを取得し `C:\Windows` ます。 **LastAccessTime** は、ファイルシステムディレクトリの1つのプロパティにすぎません。 ディレクトリのすべてのプロパティを表示するには、「」と入力 `(Get-Item <directory-name>) | Get-Member` します。

```powershell
(Get-Item C:\Windows).LastAccessTime
```

### 例 6: レジストリキーの内容を表示する

この例は、 **Microsoft PowerShell** レジストリキーの内容を示しています。 このコマンドレットを PowerShell レジストリプロバイダーと共に使用して、レジストリキーとサブキーを取得でき `Get-ItemProperty` ますが、レジストリ値とデータを取得するには、コマンドレットを使用する必要があります。

```powershell
Get-Item HKLM:\Software\Microsoft\Powershell\1\Shellids\Microsoft.Powershell\
```

### 例 7: 除外されているディレクトリ内の項目を取得する

この例では、ドット () を含む名前を持つ Windows ディレクトリ内の項目を取得 `.` しますが、では開始しません `w*` 。この例は、パスにワイルドカード文字 () が含まれている場合にのみ機能し、 `*` 項目の内容を指定します。

```powershell
Get-Item C:\Windows\*.* -Exclude "w*"
```

### 例 8: ハードリンク情報の取得

PowerShell 6.2 では、リンクの情報を取得するために代替ビューが追加されました。 ハードリンク情報を取得するには、出力をにパイプ処理します。 `Format-Table -View childrenWithHardlink`

```powershell
Get-Item -Path C:\PathWhichIsAHardLink | Format-Table -View childrenWithHardlink
```

### 例 9: 試験的な特徴 PSUnixFileStat の出力

Unix システムの PowerShell 7 では、試験的な機能 **PSUnixFileStat** が unix に似た出力を提供します。

```powershell
PS> Get-Item /Users
```

```Output
    Directory: /

UnixMode    User  Group   LastWriteTime      Size  Name
--------    ----  -----   -------------      ----  ----
drwxr-xr-x  root  admin   12/20/2019 11:46   192   Users
```

出力の一部である新しいプロパティは次のとおりです。

- **UnixMode** は、Unix システムで表されるファイルのアクセス許可です。
- **ユーザー** はファイルの所有者です
- **グループは** グループの所有者です
- **サイズ** は、Unix システムで表されるファイルまたはディレクトリのサイズです。

## PARAMETERS

### -ストリーム

> [!NOTE]
> このパラメーターは Windows でのみ使用できます。

指定した代替 NTFS ファイル システムをファイルから取得します。 ストリーム名を入力します。 ワイルドカードを利用できます。 すべてのストリームを取得するには、アスタリスク () を使用し `*` ます。 このパラメーターはフォルダーでは無効です。

**Stream** は、 **FileSystem** プロバイダーによってコマンドレットに追加される動的パラメーターです `Get-Item` 。
このパラメーターはファイル システム ドライブでのみ機能します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: No alternate file streams
Accept pipeline input: False
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
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -除外

このコマンドレットによって操作で除外される項目を文字列配列として指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカード文字を使用できます。 **Exclude** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Filter

**パス** パラメーターを修飾するフィルターを指定します。 [ファイルシステム](../Microsoft.PowerShell.Core/About/about_FileSystem_Provider.md)プロバイダーは、フィルターをサポートする唯一のインストール済み PowerShell プロバイダーです。 フィルターは他のパラメーターよりも効率的です。 プロバイダーは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、コマンドレットがオブジェクトを取得したときにフィルターを適用します。 フィルター文字列は、ファイルを列挙するために .NET API に渡されます。 API では、とのワイルドカードのみがサポートさ `*` `?` れています。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Force

このコマンドレットが、非表示の項目など、アクセスできない項目を取得することを示します。
実装はプロバイダーごとに異なります。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。 **Force** パラメーターを使用しても、コマンドレットはセキュリティ制限を上書きできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Include

文字列配列として、このコマンドレットによって操作に含まれる項目を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカード文字を使用できます。 **Include** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

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

項目のパスを指定します。 このコマンドレットは、指定した場所にある項目を取得します。 ワイルドカード文字を使用できます。 このパラメーターは必須ですが、パラメーター名の **パス** は省略可能です。

現在の場所を指定するには、ドット () を使用し `.` ます。 現在の場所のすべての項目を指定するには、ワイルドカード文字 ( `*` ) を使用します。

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

### 共通パラメーター

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用して、このコマンドレットへのパスを含む文字列をパイプすることができます。

## 出力

### System.Object

このコマンドレットは、取得したオブジェクトを返します。 この型は、パス内のオブジェクトの型によって決まります。

## 注

このコマンドレットには、内容ではなく項目だけを取得するため、 **再帰** パラメーターはありません。
項目の内容を再帰的に取得するには、を使用し `Get-ChildItem` ます。

レジストリ内を移動するには、このコマンドレットを使用してレジストリキーを取得し、を使用して `Get-ItemProperty` レジストリ値とデータを取得します。 レジストリ値はレジストリ キーのプロパティと見なされます。

このコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PsProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Clear-Item](Clear-Item.md)

[Copy-Item](Copy-Item.md)

[Invoke-Item](Invoke-Item.md)

[Move-Item](Move-Item.md)

[New-Item](New-Item.md)

[Remove-Item](Remove-Item.md)

[Rename-Item](Rename-Item.md)

[Set-Item](Set-Item.md)

[Get-ChildItem](Get-ChildItem.md)

[Get-ItemProperty](Get-ItemProperty.md)

[Get-PSProvider](Get-PSProvider.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)
