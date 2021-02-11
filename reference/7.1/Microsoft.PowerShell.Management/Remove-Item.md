---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/remove-item?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-Item
ms.openlocfilehash: c4a0243c528cbe1a28cad99cf6fa8d91018d9b3c
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "97692675"
---
# Remove-Item

## 概要
指定した項目を削除します。

## SYNTAX

### パス (既定値)

```
Remove-Item [-Path] <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>]
 [-Recurse] [-Force] [-Credential <PSCredential>] [-WhatIf] [-Confirm] [-Stream <String[]>]
 [<CommonParameters>]
```

### LiteralPath

```
Remove-Item -LiteralPath <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>]
 [-Recurse] [-Force] [-Credential <PSCredential>] [-WhatIf] [-Confirm] [-Stream <String[]>]
 [<CommonParameters>]
```

## Description

`Remove-Item`コマンドレットでは、1つ以上の項目を削除します。 多くのプロバイダーでサポートされているため、ファイル、フォルダー、レジストリキー、変数、エイリアス、関数など、さまざまな種類の項目を削除できます。

## 例

### 例 1: 任意のファイル名拡張子を持つファイルを削除する

この例では、フォルダーからドット () を含む名前を持つすべてのファイルを削除し `.` `C:\Test` ます。 コマンドではドットを指定するため、ファイル名拡張子のないフォルダーやファイルは削除されません。

```powershell
Remove-Item C:\Test\*.*
```

### 例 2: フォルダー内のドキュメントファイルの一部を削除する

次の例では、現在のフォルダーから、 `.doc` ファイル名拡張子とが含まれていない名前を持つすべてのファイルを削除し `*1*` ます。

```powershell
Remove-Item * -Include *.doc -Exclude *1*
```

ワイルドカード文字 () を使用して、 `*` 現在のフォルダーの内容を指定します。 この例では、 **Include** パラメーターと **Exclude** パラメーターを使用して、削除するファイルを指定しています。

### 例 3: 非表示の読み取り専用ファイルを削除する

このコマンドは、 *非表示* のファイルと *読み取り* 専用ファイルの両方を削除します。

```powershell
Remove-Item -Path C:\Test\hidden-RO-file.txt -Force
```

**Path** パラメーターを使用してファイルを指定します。 **Force** パラメーターを使用して削除します。 **Force** を使用しない場合、_読み取り_ 専用または _非表示_ のファイルは削除できません。

### 例 4: サブフォルダー内のファイルを再帰的に削除する

このコマンドは、現在のフォルダーとすべてのサブフォルダー内のすべての CSV ファイルを再帰的に削除します。

の **再帰** パラメーターには `Remove-Item` 既知の問題があるため、この例のコマンドはを使用して `Get-ChildItem` 目的のファイルを取得し、パイプライン演算子を使用してに渡し `Remove-Item` ます。

```powershell
Get-ChildItem * -Include *.csv -Recurse | Remove-Item
```

コマンドで `Get-ChildItem` は、 **Path** の値が () になっています。この値は、 `*` 現在のフォルダーの内容を表します。 **Include** を使用して CSV ファイルの種類を指定し、**再帰を使用** して、再帰的な取得を行います。 ファイルの種類としてパス (など) を指定しようとする `-Path *.csv` と、コマンドレットは検索の対象を子項目を持たないファイルと解釈し、 **再帰** は失敗します。

### 例 5: サブキーを再帰的に削除する

このコマンドは、"OldApp" レジストリキーとそのすべてのサブキーと値を削除します。 キーを `Remove-Item` 削除するためにを使用します。 パスが指定されていますが、省略可能なパラメーター名 (**path**) は省略されています。

**再帰** パラメーターは、"oldapp" キーのすべての内容を再帰的に削除します。 キーにサブキーが含まれていて、 **再帰** パラメーターを省略した場合、キーの内容を削除するかどうかを確認するメッセージが表示されます。

```powershell
Remove-Item HKLM:\Software\MyCompany\OldApp -Recurse
```

### 例 6: 特殊文字を使用したファイルの削除

次の例では、角かっこやかっこなどの特殊文字を含むファイルを削除する方法を示します。

```powershell
Get-ChildItem
```

```Output
    Directory: C:\temp\Downloads

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a---          6/1/2018  12:19 PM           1362 myFile.txt
-a---          6/1/2018  12:30 PM           1132 myFile[1].txt
-a---          6/1/2018  12:19 PM           1283 myFile[2].txt
-a---          6/1/2018  12:19 PM           1432 myFile[3].txt

```

```powershell
Get-ChildItem | Where-Object Name -Like '*`[*'
```

```Output
    Directory: C:\temp\Downloads

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a---          6/1/2018  12:30 PM           1132 myFile[1].txt
-a---          6/1/2018  12:19 PM           1283 myFile[2].txt
-a---          6/1/2018  12:19 PM           1432 myFile[3].txt

```

```powershell
Get-ChildItem | Where-Object Name -Like '*`[*' | ForEach-Object { Remove-Item -LiteralPath $_.Name }
Get-ChildItem
```

```Output
    Directory: C:\temp\Downloads

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a---          6/1/2018  12:19 PM           1362 myFile.txt
```

### 例 7: 代替データストリームを削除する

この例は、コマンドレットの **Stream** 動的パラメーターを使用して、代替データストリームを削除する方法を示して `Remove-Item` います。 Stream パラメーターは、Windows PowerShell 3.0 で導入されました。

```powershell
Get-Item C:\Test\Copy-Script.ps1 -Stream Zone.Identifier
```

```Output
   FileName: \\C:\Test\Copy-Script.ps1

Stream                   Length
------                   ------
Zone.Identifier              26

```

```powershell
Remove-Item C:\Test\Copy-Script.ps1 -Stream Zone.Identifier
Get-Item C:\Test\Copy-Script.ps1 -Stream Zone.Identifier
```

```Output
Get-Item : Could not open alternate data stream 'Zone.Identifier' of file 'C:\Test\Copy-Script.ps1'.
At line:1 char:1
+ Get-Item 'C:\Test\Copy-Script.ps1' -Stream Zone.Identifier
+ [!INCLUDE[]()][!INCLUDE[]()][!INCLUDE[]()][!INCLUDE[]()][!INCLUDE[]()]~~
    + CategoryInfo          : ObjectNotFound: (C:\Test\Copy-Script.ps1:String) [Get-Item], FileNotFoundE
   xception
    + FullyQualifiedErrorId : AlternateDataStreamNotFound,Microsoft.PowerShell.Commands.GetItemCommand

```

**Stream** パラメーターは、 `Get-Item` `Zone.Identifier` ファイルのストリームを取得し `Copy-Script.ps1` ます。 `Remove-Item`**stream** パラメーターを使用して、 `Zone.Identifier` ファイルのストリームを削除します。 最後に、 `Get-Item` ストリームが削除されたことがコマンドレットによって示され `Zone.Identifier` ます。

## PARAMETERS

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

**パス** パラメーターを修飾するフィルターを指定します。 [ファイルシステム](../Microsoft.PowerShell.Core/About/about_FileSystem_Provider.md)プロバイダーは、フィルターの使用をサポートする唯一のインストール済み PowerShell プロバイダーです。 **ファイルシステム** フィルター言語の構文については、 [about_Wildcards](../Microsoft.PowerShell.Core/About/about_Wildcards.md)を参照してください。 フィルターは他のパラメーターよりも効率的です。これは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、コマンドレットがオブジェクトを取得するときに、フィルターが適用されるためです。

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

非表示または読み取り専用のファイルや読み取り専用のエイリアスまたは変数など、変更できない項目をコマンドレットで強制的に削除します。 コマンドレットでは、定数のエイリアスまたは変数は削除できません。
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

文字列配列として、このコマンドレットによって操作に含まれる項目を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `"*.txt"` ます。 ワイルドカード文字を使用できます。 **Include** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

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

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

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

削除する項目のパスを指定します。
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

### -再帰

このコマンドレットが、指定された場所の項目と、その場所のすべての子項目を削除することを示します。

**Include** パラメーターと共に使用した場合、すべてのサブフォルダーまたはすべての子項目が **再帰** パラメーターによって削除されない可能性があります。 これは既知の問題です。 回避策として、 `Get-ChildItem -Recurse` `Remove-Item` このトピックの「例4」で説明されているように、コマンドの結果をにパイプ処理します。

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

### -ストリーム

> [!NOTE]
> このパラメーターは Windows でのみ使用できます。

**ストリーム** パラメーターは、FileSystem プロバイダーによってに追加される動的パラメーターです `Remove-Item` 。
このパラメーターはファイル システム ドライブでのみ機能します。

を使用する `Remove-Item` と、などの代替データストリームを削除でき `Zone.Identifier` ます。
ただし、インターネットからダウンロードされるファイルをブロックするセキュリティ チェックをなくす方法としては推奨されません。 ダウンロードしたファイルが安全であることを確認した場合は、コマンドレットを使用し `Unblock-File` ます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

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

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。 詳細については、以下の記事を参照してください。

- [about_Preference_Variables](../microsoft.powershell.core/about/about_preference_variables.md#confirmpreference)
- [about_Functions_CmdletBindingAttribute](../microsoft.powershell.core/about/about_functions_cmdletbindingattribute.md?#confirmimpact)

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

パイプを使用してパスを含む文字列をこのコマンドレットに送ることができます。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## 注

`Remove-Item`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PsProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

**再帰** パラメーターを使用せずに項目が含まれているフォルダーを削除しようとすると、コマンドレットによって確認メッセージが表示されます。 を使用する `-Confirm:$false` と、プロンプトは表示されません。 これは仕様です。

## 関連リンク

[Clear-Item](Clear-Item.md)

[Copy-Item](Copy-Item.md)

[Get-Item](Get-Item.md)

[Invoke-Item](Invoke-Item.md)

[Move-Item](Move-Item.md)

[New-Item](New-Item.md)

[Remove-ItemProperty](Remove-ItemProperty.md)

[Rename-Item](Rename-Item.md)

[Set-Item](Set-Item.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[about_Preference_Variables](../microsoft.powershell.core/about/about_preference_variables.md#confirmpreference)

[about_Functions_CmdletBindingAttribute](../microsoft.powershell.core/about/about_functions_cmdletbindingattribute.md?#confirmimpact)
