---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 01/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-formatdata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-FormatData
ms.openlocfilehash: 28eab1709de12ec5d391009aacccfd4e973a8b9b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600981"
---
# Get-FormatData

## 概要
現在のセッションの書式設定データを取得します。

## SYNTAX

```
Get-FormatData [[-TypeName] <String[]>] [-PowerShellVersion <Version>] [<CommonParameters>]
```

## Description

`Get-FormatData`コマンドレットでは、現在のセッションの書式設定データを取得します。

セッションの書式設定データには、ディレクトリ内のファイルなどの書式設定ファイルからの書式設定データ、 `Format.ps1xml` `$PSHOME` セッションにインポートするモジュールのデータの書式設定、コマンドレットを使用してセッションにインポートするコマンドのデータの書式設定などがあり `Import-PSSession` ます。

このコマンドレットを使用すると、書式設定データを確認できます。 次に、コマンドレットを使用して `Export-FormatData` オブジェクトをシリアル化し、XML に変換して、ファイルに保存し `Format.ps1xml` ます。

PowerShell でのファイルの書式設定の詳細については、「 [about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)」を参照してください。

## 例

### 例 1: すべての書式設定データを取得する

この例では、セッションのすべての書式設定データを取得します。

```powershell
Get-FormatData
```

### 例 2: 型名で書式設定データを取得する

この例では、名前がで始まる書式設定データ項目を取得し `System.Management.Automation.Cmd` ます。

```powershell
Get-FormatData -TypeName 'System.Management.Automation.Cmd*'
```

### 例 3: 書式設定データオブジェクトを確認する

この例では、書式設定データ オブジェクトを取得し、そのプロパティを確認する方法を示します。

```powershell
$F = Get-FormatData -TypeName 'System.Management.Automation.Cmd*'
$F
```

```Output
TypeName        FormatViewDefinition
--------        --------------------
HelpInfoShort   {help , TableControl}
```

```powershell
$F.FormatViewDefinition[0].control
```

```Output
Headers          : {System.Management.Automation.TableControlColumnHeader,
                   System.Management.Automation.TableControlColumnHeader,
                   System.Management.Automation.TableControlColumnHeader,
                   System.Management.Automation.TableControlColumnHeader}
Rows             : {System.Management.Automation.TableControlRow}
AutoSize         : False
HideTableHeaders : False
GroupBy          :
OutOfBand        : False
```

```powershell
$F.FormatViewDefinition[0].control.Headers
```

```Output
Label       Alignment Width
-----       --------- -----
CommandType Undefined    15
Name        Undefined    50
Version     Undefined    10
Source      Undefined     0
```

### 例 4: 書式設定データを取得してエクスポートする

この例では、およびを使用し `Get-FormatData` `Export-FormatData` て、モジュールによって追加された書式設定データをエクスポートする方法を示します。

```powershell
$A = Get-FormatData
Import-Module bitstransfer
$B = Get-FormatData
Compare-Object $A $B
```

```Output
InputObject                                                SideIndicator
-----------                                                -------------
Microsoft.BackgroundIntelligentTransfer.Management.BitsJob =>
```

```powershell
Get-FormatData *bits* | Export-FormatData -FilePath c:\test\bits.format.ps1xml
Get-Content c:\test\bits.format.ps1xml
```

```Output
<?xml version="1.0" encoding="utf-8"?><Configuration><ViewDefinitions>
<View><Name>Microsoft.BackgroundIntelligentTransfer.Management.BitsJob</Name>
...
```

最初の4つのコマンドは `Get-FormatData` 、、、およびコマンドレットを使用して、 `Import-Module` `Compare-Object` **BitsTransfer** モジュールがセッションに追加するフォーマットの種類を識別します。

5番目のコマンドは、コマンドレットを使用して、 `Get-FormatData` **BitsTransfer** モジュールによって追加される形式の種類を取得します。 パイプライン演算子 () を使用して、 `|` format type オブジェクトをコマンドレットに送信します。これにより、 `Export-FormatData` XML に変換され、指定したファイルに保存され `format.ps1xml` ます。

最後のコマンドは、ファイルの内容の抜粋を示し `format.ps1xml` ます。

### 例 5: PowerShell の指定したバージョンに基づいて書式設定データを取得する

この例では、を使用して `Get-FormatData` 、指定した **TypeName** と PowerShell バージョンの書式データを取得する方法を示します。

```powershell
Get-FormatData -TypeName 'Microsoft.Powershell.Utility.FileHash' -PowerShellVersion $PSVersionTable.PSVersion

TypeNames                               FormatViewDefinition
---------                               --------------------
{Microsoft.Powershell.Utility.FileHash} {Microsoft.Powershell.Utility.FileHash}
```

## PARAMETERS

### -PowerShellVersion

このコマンドレットによって書式設定データに対して取得される PowerShell のバージョンを指定します。 ピリオドで区切られた2桁の数字を入力してください。

Powershell 5.1 では、旧バージョンの PowerShell を実行しているリモート処理コンピューターの互換性を向上させるために、このパラメーターが追加されました。

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeName

このコマンドレットが書式設定データに対して取得する型名を指定します。
型名を入力します。
ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システムの管理. ExtendedTypeDefinition

## 注

## 関連リンク

[Export-FormatData](Export-FormatData.md)

[Update-FormatData](Update-FormatData.md)
