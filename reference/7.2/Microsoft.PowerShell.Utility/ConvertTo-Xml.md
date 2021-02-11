---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 03/12/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertto-xml?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertTo-Xml
ms.openlocfilehash: e461c07360b3d9eaf7d9a3c8875d34cd1fc841e8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600113"
---
# ConvertTo-Xml

## 概要
オブジェクトの XML ベースの表現を作成します。

## SYNTAX

```
ConvertTo-Xml [-Depth <Int32>] [-InputObject] <PSObject> [-NoTypeInformation] [-As <String>]
 [<CommonParameters>]
```

## Description

`ConvertTo-Xml`コマンドレットでは、1つ以上の .net オブジェクトの[XML ベース](/dotnet/api/system.xml.xmldocument)の表現を作成します。 このコマンドレットを使用するには、1つまたは複数のオブジェクトをコマンドレットにパイプするか、 **InputObject** パラメーターを使用してオブジェクトを指定します。

複数のオブジェクトをにパイプする場合 `ConvertTo-Xml` 、または **InputObject** パラメーターを使用して複数のオブジェクトを送信する場合、は、 `ConvertTo-Xml` すべてのオブジェクトの表現を含む単一のメモリ内 XML ドキュメントを返します。

このコマンドレットは [export-clixml](./Export-Clixml.md) に似ていますが、では `Export-Clixml` 、結果の XML が [共通言語基盤 (CLI) の xml](https://www.ecma-international.org/publications/standards/Ecma-335.htm) ファイルに格納されます。これは、 [export-clixml](./Import-Clixml.md)を使用してオブジェクトとして再インポートできます。 `ConvertTo-Xml` は、XML ドキュメントのメモリ内表現を返します。そのため、PowerShell で処理を続行できます。 `ConvertTo-Xml` には、オブジェクトを CLI XML に変換するオプションはありません。

## 例

### 例 1: 日付を XML に変換する

```
PS C:\> Get-Date | ConvertTo-Xml
```

このコマンドは、現在の日付 ( **DateTime** オブジェクト) を XML に変換します。

### 例 2: プロセスを XML に変換する

```
PS C:\> ConvertTo-Xml -As "Document" -InputObject (Get-Process) -Depth 3
```

このコマンドは、コンピューター上のすべてのプロセスを表すプロセス オブジェクトを XML ドキュメントに変換します。 オブジェクトは、3 レベルの深さに拡張されます。

## PARAMETERS

### -As

出力形式を決定します。
このパラメーターの有効値は、次のとおりです。

- 文字列 をオンにします。
1 つの文字列を返します。
- ストリーム。
文字列の配列を返します。
- ドキュメントです。
**XmlDocument** オブジェクトを返します。

既定値は Document です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Stream, String, Document

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -深さ

XML 表現に含める子オブジェクトのレベルを指定します。 既定値は 1 です。

たとえば、オブジェクトのプロパティにオブジェクトが含まれる場合、子オブジェクトのプロパティの XML 表現を保存するには、深さのレベルとして 2 を指定する必要があります。

既定値は、Types.ps1xml ファイル内のオブジェクトの型に対してオーバーライドできます。 詳細については、「about_Types.ps1xml」を参照してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

変換するオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。 パイプを使用してオブジェクトを **convertto-html** にパイプすることもできます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -NoTypeInformation

オブジェクト ノードから Type 属性を削除します。

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

### システム管理. PSObject

パイプを使用して任意のオブジェクトを **convertto-html** に送ることができます。

## 出力

### System.string または System.Xml.Xmlドキュメント

As パラメーターの値によっ *て* 、 **convertto-html** が返すオブジェクトの型が決まります。

## 注

## 関連リンク

[ConvertTo-Csv](ConvertTo-Csv.md)

[ConvertTo-Html](ConvertTo-Html.md)

[Export-Clixml](Export-Clixml.md)

[取得-日付](Get-Date.md)

[Import-Clixml](Import-Clixml.md)

