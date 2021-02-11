---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/clear-content?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Clear-Content
ms.openlocfilehash: 8024dc8a4a041fad783f1234477ee4b5920e7b00
ms.sourcegitcommit: bf07cffb2a66dec94bf3576e197090f958701f18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2020
ms.locfileid: "99602190"
---
# Clear-Content

## 概要
項目の内容を削除します。項目自体は削除しません。

## SYNTAX

### パス (既定値)

```
Clear-Content [-Path] <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-Force]
 [-Credential <PSCredential>] [-WhatIf] [-Confirm] [-Stream <String>] [<CommonParameters>]
```

### LiteralPath

```
Clear-Content -LiteralPath <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-Force]
 [-Credential <PSCredential>] [-WhatIf] [-Confirm] [-Stream <String>] [<CommonParameters>]
```

## Description

コマンドレットは、ファイルからテキストを削除するなど `Clear-Content` 、項目の内容を削除しますが、項目は削除しません。
その結果、項目は残りますが、空になります。
は `Clear-Content` に似てい `Clear-Item` ますが、値を持つ項目ではなく、内容を持つ項目に対して機能します。

## 例

### 例 1: ディレクトリからすべてのコンテンツを削除する

```powershell
Clear-Content "..\SmpUsers\*\init.txt"
```

このコマンドを実行すると、SmpUsers ディレクトリのすべてのサブディレクトリにある init.txt ファイルからすべての内容が削除されます。
ファイル自体は削除されず、空になります。

### 例 2: ワイルドカードを使用してすべてのファイルの内容を削除する

```powershell
Clear-Content -Path "*" -Filter "*.log" -Force
```

このコマンドを実行すると、読み取り専用属性が指定されているファイルを含め、現在のディレクトリ内で、拡張子が ".log" であるすべてのファイルから内容が削除されます。 パスのアスタリスク () は、 \* 現在のディレクトリ内のすべての項目を表します。 **Force** パラメーターを指定すると、読み取り専用ファイルに対してコマンドが有効になります。 フィルターを使用して、パスに .log を指定する代わりに、.log ファイル名拡張子を持つファイルにコマンドを制限すると、 \* 操作が高速になります。

### 例 3: ストリームからすべてのデータをクリアする

この例では、 `Clear-Content` ストリームをそのまま残したまま、コマンドレットが代替データストリームからコンテンツをクリアする方法を示します。

最初のコマンドは、コマンドレットを使用して、 `Get-Content` `Zone.Identifier` インターネットからダウンロードされた Copy-Script.ps1 ファイル内のストリームの内容を取得します。

2番目のコマンドは、 `Clear-Content` コマンドレットを使用してコンテンツをクリアします。

3 番目のコマンドは、最初のコマンドを繰り返します。 コンテンツがクリアされていることを確認しますが、ストリームは残ります。 ストリームが削除された場合、コマンドはエラーを生成します。

このようなメソッドを使用して、代替データストリームの内容を消去できます。 ただし、インターネットからダウンロードされるファイルをブロックするセキュリティ チェックをなくす方法としては推奨されません。 ダウンロードしたファイルが安全であることを確認した場合は、コマンドレットを使用し `Unblock-File` ます。

```
PS C:\> Get-Content C:\Test\Copy-Script.ps1 -Stream Zone.Identifier

[ZoneTransfer]
ZoneId=3

PS C:\>Clear-Content C:\Test\Copy-Script.ps1 -Stream Zone.Identifier

PS C:\>Get-Content C:\Test\Copy-Script.ps1 -Stream Zone.Identifier
PS C:\>
```

## PARAMETERS

### -ストリーム

> [!NOTE]
> このパラメーターは Windows でのみ使用できます。

コンテンツの代替データストリームを指定します。 ストリームが存在しない場合は、このコマンドレットによって作成されます。 ワイルドカード文字はサポートされていません。

**Stream** は、FileSystem プロバイダーによってに追加される動的パラメーターです `Clear-Content` 。 このパラメーターはファイルシステムドライブでのみ機能し、ファイルとディレクトリの両方で代替データストリームの内容をクリアします。

コマンドレットを使用し `Clear-Content` て、のような、amy オルタネートデータストリームの内容を変更でき `Zone.Identifier` ます。 ただし、インターネットからダウンロードされたファイルをブロックするセキュリティチェックを省略する方法としては、この方法をお勧めしません。 ダウンロードしたファイルが安全であることを確認した場合は、コマンドレットを使用し `Unblock-File` ます。

このパラメーターは、PowerShell 3.0 で導入されました。 PowerShell 7.2 の場合、で `Clear-Content` は、ディレクトリおよびファイルから代替データストリームの内容を消去できます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、Invoke コマンドを使用します。

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

文字列配列として、このコマンドレットによってコンテンツへのパスから省略される文字列を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 「*.txt」などのパス要素またはパターンを入力します。 ワイルドカードを使用できます。

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

プロバイダーの形式や言語でフィルターを指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 ワイルドカードを使用できるかどうかなど、フィルターの構文はプロバイダーによって異なります。 フィルターは他のパラメーターよりも効率的です。これは、オブジェクトを取得した後に PowerShell がオブジェクトをフィルター処理するのではなく、オブジェクトを取得するときにプロバイダーが適用するためです。

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

ユーザーに確認せずに、直ちにコマンドを実行します。

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

このコマンドレットによってクリアされるコンテンツを文字列配列として指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 「*.txt」などのパス要素またはパターンを入力します。 ワイルドカードを使用できます。

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

内容を削除する項目へのパスを指定します。 **Path** パラメーターと異なり、**LiteralPath** の値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。
パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符は、どの文字もエスケープシーケンスとして解釈されないように PowerShell に指示します。

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

内容を削除する項目へのパスを指定します。 ワイルドカードを使用できます。 コンテナーのパスではなく、項目のパスを指定してください。 たとえば、ディレクトリのパスではなく、1 つ以上のファイルのパスを指定する必要があります。 ワイルドカードを使用できます。 このパラメーターは必須ですが、パラメーター名 (Path) は省略可能です。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
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

### なし

パイプを使用してオブジェクトをにパイプすることはできません `Clear-Content` 。

## 出力

### なし

このコマンドレットはオブジェクトを返しません。

## 注

は `Clear-Content` 、PowerShell FileSystem プロバイダーや、コンテンツを操作するその他のプロバイダーで使用できます。 PowerShell 証明書またはレジストリプロバイダーによって管理されている項目など、コンテンツと見なされない項目をクリアするには、を使用し `Clear-Item` ます。

`Clear-Content`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。
セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PsProvider` します。
詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Add-Content](Add-Content.md)

[Get-Content](Get-Content.md)

[Get-Item](Get-Item.md)

[Set-Content](Set-Content.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)
