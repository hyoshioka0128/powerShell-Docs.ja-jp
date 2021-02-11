---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/update-formatdata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-FormatData
ms.openlocfilehash: 91d0274e485573de96d9960fa49f6d327156a79a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603034"
---
# Update-FormatData

## 概要
現在のセッションの書式設定データを更新します。

## SYNTAX

```
Update-FormatData [[-AppendPath] <String[]>] [-PrependPath <String[]>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

このコマンドレットは、書式設定 `Update-FormatData` データをフォーマットファイルから現在のセッションに再読み込みします。 このコマンドレットを使用すると、PowerShell を再起動しなくても書式設定データを更新できます。

パラメーターを指定せずに、 `Update-FormatData` 以前に読み込まれた書式設定ファイルを再読み込みします。
のパラメーターを使用し `Update-FormatData` て、新しい書式設定ファイルをセッションに追加できます。

書式設定ファイルは、ファイル名拡張子を持つ XML 形式のテキストファイルです `format.ps1xml` 。 ファイル内の書式設定データは、セッションでの Microsoft .NET Framework オブジェクトの表示を定義します。

PowerShell が起動すると、PowerShell ソースコードから形式データが読み込まれます。 ただし、カスタム format.ps1xml ファイルを作成して、現在のセッションの書式を更新することができます。 を使用すると、 `Update-FormatData` PowerShell を再起動せずに、書式設定データを現在のセッションに再度読み込むことができます。 このコマンドレットは、書式設定ファイルを追加または変更した後でセッションを中断したくない場合に便利です。

PowerShell でのファイルの書式設定の詳細については、「 [about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)」を参照してください。

## 例

### 例 1: 以前に読み込まれた書式設定ファイルを再読み込みする

```powershell
Update-FormatData
```

このコマンドは、以前に読み込まれた書式設定ファイルを再読み込みします。

### 例 2: フォーマットファイルとトレースおよびログの書式設定ファイルを再読み込みする

```powershell
Update-FormatData -AppendPath "trace.format.ps1xml, log.format.ps1xml"
```

このコマンドは、Trace.format.ps1xml と Log.format.ps1xml の 2 つの新しいファイルを含む書式設定ファイルをセッションに再読み込みします。

コマンドで **Appendpath** パラメーターが使用されるため、新しいファイルの書式設定データは、組み込みファイルの書式設定データの後に読み込まれます。

**Appendpath** パラメーターが使用されるのは、組み込みファイルで参照されていないオブジェクトの書式設定データが新しいファイルに含まれているためです。

### 例 3: 書式設定ファイルを編集して再読み込みする

```powershell
Update-FormatData -PrependPath "c:\test\NewFiles.format.ps1xml"

# Edit the NewFiles.format.ps1 file.

Update-FormatData
```

この例では、書式設定ファイルを編集した後で再読み込みする方法を示します。

最初のコマンドは、NewFiles.format.ps1xml ファイルをセッションに追加します。 **PrependPath** パラメーターを使用します。これは、組み込みファイルで参照されるオブジェクトの書式設定データがファイルに含まれているためです。

NewFiles.format.ps1xml ファイルを追加し、これらのセッションでテストした後、作成者はファイルを編集します。

2番目のコマンドは、コマンドレットを使用して、 `Update-FormatData` 書式設定ファイルを再読み込みします。 NewFiles.format.ps1xml ファイルは既に読み込まれているので、 `Update-FormatData` パラメーターを使用せずに、自動的に再読み込みします。

## PARAMETERS

### -AppendPath

このコマンドレットによってセッションに追加されるフォーマットファイルを指定します。 ファイルは、PowerShell が組み込みの書式設定ファイルを読み込んだ後に読み込まれます。

.NET オブジェクトを書式設定する場合、PowerShell では、各 .NET 型に対して検出された最初の書式定義を使用します。 **Appendpath** パラメーターを使用すると、PowerShell は、追加する書式設定データを検出する前に、組み込みファイルからデータを検索します。

組み込みの書式設定ファイルで参照されない .NET オブジェクトを書式設定するファイルを追加するには、このパラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PSPath, Path

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -PrependPath

このコマンドレットによってセッションに追加されるフォーマットファイルを指定します。 ファイルは、PowerShell が組み込みの書式設定ファイルを読み込む前に読み込まれます。

.NET オブジェクトを書式設定する場合、PowerShell では、各 .NET 型に対して検出された最初の書式定義を使用します。 **PrependPath** パラメーターを使用すると、PowerShell は、組み込みファイルの書式設定データを検出する前に、追加するファイルからデータを検索します。

組み込みの書式設定ファイルでも参照される .NET オブジェクトを書式設定するファイルを追加するには、このパラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、追加パスを含む文字列をにパイプすることができ `Update-FormatData` ます。

## 出力

### なし

このコマンドレットは出力を返しません。

## 注

- `Update-FormatData` では、モジュールからインポートされたセッションのコマンドの書式設定データも更新されます。 モジュールのフォーマットファイルが変更された場合は、コマンドを実行して、インポートした `Update-FormatData` コマンドの書式設定データを更新できます。 モジュールをもう一度インポートする必要はありません。

## 関連リンク

[Get-FormatData](Get-FormatData.md)

[Export-FormatData](Export-FormatData.md)
