---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 10/03/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/rename-item?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Rename-Item
ms.openlocfilehash: dec5c2c905486110e4885f29d236ab41d945fb96
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602011"
---
# Rename-Item

## 概要
PowerShell プロバイダーの名前空間内の項目の名前を変更します。

## SYNTAX

### ByPath (既定値)

```
Rename-Item [-Path] <String> [-NewName] <String> [-Force] [-PassThru] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm]  [<CommonParameters>]
```

### ByLiteralPath

```
Rename-Item -LiteralPath <String> [-NewName] <String> [-Force] [-PassThru]
 [-Credential <PSCredential>] [-WhatIf] [-Confirm]  [<CommonParameters>]
```

## Description

コマンドレットでは、 `Rename-Item` 指定した項目の名前を変更します。 このコマンドレットを使用しても、名前を変更する項目の内容には影響ありません。

を使用し `Rename-Item` て、新しい名前でパスを指定するなど、項目を移動することはできません。 項目を移動して名前を変更するには、コマンドレットを使用し `Move-Item` ます。

## 例

### 例 1: ファイルの名前を変更する

このコマンドにより、ファイルの名前がに変更さ `daily_file.txt` `monday_file.txt` れます。

```powershell
Rename-Item -Path "c:\logfiles\daily_file.txt" -NewName "monday_file.txt"
```

### 例 2: 項目の名前を変更して移動する

を使用し `Rename-Item` て項目の名前変更と移動を行うことはできません。 具体的には、path パラメーターに指定され **たパスと** 同じパスでない限り、 **NewName** パラメーターの値のパスを指定することはできません。 そうでない場合は、新しい名前のみが許可されます。

この例では、ディレクトリ内の現在のディレクトリにあるファイルの名前をに変更しようとして `project.txt` `old-project.txt` `D:\Archive` います。 結果は出力に示されるようなエラーになります。

```powershell
Rename-Item -Path "project.txt" -NewName "d:\archive\old-project.txt"
```

```Output
Rename-Item : can't rename because the target specified represents a path or device name.
At line:1 char:12
+ Rename-Item <<<<  -path project.txt -NewName d:\archive\old-project.txt
+ CategoryInfo          : InvalidArgument: (:) [Rename-Item], PS>  Move-Item -Path "project.txt" -De
stination "d:\archive\old-project.txt"
```

### 例 3: レジストリキーの名前を変更する

この例では、レジストリキーの名前を **宣伝** から **Marketing** に変更します。 コマンドが完了すると、キーの名前が変更されますが、キーのレジストリ エントリに変更はありません。

```powershell
Rename-Item -Path "HKLM:\Software\MyCompany\Advertising" -NewName "Marketing"
```

### 例 4: 複数のファイルの名前を変更する

この例では、現在のディレクトリ内のすべてのファイルの名前 `*.txt` をに変更 `*.log` します。

```powershell
Get-ChildItem *.txt
```

```Output
    Directory: C:\temp\files

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        10/3/2019   7:47 AM           2918 Friday.TXT
-a----        10/3/2019   7:46 AM           2918 Monday.Txt
-a----        10/3/2019   7:47 AM           2918 Wednesday.txt
```

```powershell
Get-ChildItem *.txt | Rename-Item -NewName { $_.Name -replace '.txt','.log' }
Get-ChildItem *.log
```

```Output
    Directory: C:\temp\files

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        10/3/2019   7:47 AM           2918 Friday.log
-a----        10/3/2019   7:46 AM           2918 Monday.log
-a----        10/3/2019   7:47 AM           2918 Wednesday.log
```

この `Get-ChildItem` コマンドレットは、ファイル拡張子を持つ現在のフォルダー内のすべてのファイルを取得し、それらのファイル `.txt` をにパイプし `Rename-Item` ます。 **Newname** の値は、値が **newname** パラメーターに送信される前に実行されるスクリプトブロックです。

スクリプトブロックでは、 `$_` 自動変数は、パイプラインを介してコマンドに渡される各ファイルオブジェクトを表します。 スクリプトブロックでは、演算子を使用して、 `-replace` 各ファイルのファイル拡張子をに置き換え `.log` ます。 演算子を使用した照合で `-replace` は大文字と小文字が区別されないことに注意してください。

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
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Force

非表示または読み取り専用のファイルや読み取り専用のエイリアスまたは変数など、変更できない項目の名前をコマンドレットで強制的に変更します。 コマンドレットでは、定数のエイリアスまたは変数を変更することはできません。
実装はプロバイダーごとに異なります。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

**Force** パラメーターを使用しても、コマンドレットはセキュリティ制限を上書きできません。

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

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NewName

項目の新しい名前を指定します。 パスと名前ではなく、名前のみを入力します。 **Path パラメーターで** 指定したパスとは異なるパスを入力すると、によって `Rename-Item` エラーが生成されます。
項目の名前を変更したり移動したりするには、を使用し `Move-Item` ます。

**NewName** パラメーターの値にワイルドカード文字を使用することはできません。 複数のファイルの名前を指定するには、正規表現で **Replace** 演算子を使用します。 Replace 演算子の詳細については、「 [about_Comparison_Operators](../Microsoft.PowerShell.Core/About/about_Comparison_Operators.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

パイプラインの項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -Path

名前を変更する項目のパスを指定します。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

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

### System.String

パイプを使用して、このコマンドレットへのパスを含む文字列をパイプすることができます。

## 出力

### None または名前が変更された項目を表すオブジェクト。

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、名前が変更された項目を表すオブジェクトを生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

`Rename-Item` は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PsProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Clear-Item](Clear-Item.md)

[Copy-Item](Copy-Item.md)

[Get-ChildItem](Get-ChildItem.md)

[Get-Item](Get-Item.md)

[Invoke-Item](Invoke-Item.md)

[Move-Item](Move-Item.md)

[New-Item](New-Item.md)

[Remove-Item](Remove-Item.md)

[Rename-ItemProperty](Rename-ItemProperty.md)

[Set-Item](Set-Item.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

