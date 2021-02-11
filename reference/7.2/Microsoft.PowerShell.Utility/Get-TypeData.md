---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-typedata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-TypeData
ms.openlocfilehash: db5dc586f2a165d83c25bdf2addaeb625f9e1ba0
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601195"
---
# Get-TypeData

## 構文
現在のセッションで、拡張型データを取得します。

## 構文

```
Get-TypeData [[-TypeName] <String[]>] [<CommonParameters>]
```

## 説明

`Get-TypeData`コマンドレットは、現在のセッションの拡張型データを取得します。 これには、ファイルによってセッションに追加された型データ `Types.ps1xml` と、コマンドレットのパラメーターを使用して追加された動的な型データが含まれ `Update-TypeData` ます。

が返す拡張型データを使用すると、 `Get-TypeData` セッションの型データを調べて、 `Update-TypeData` `Remove-TypeData` コマンドレットおよびコマンドレットに送信できます。

拡張型データは、PowerShell のオブジェクトにプロパティとメソッドを追加します。 その追加されたプロパティとメソッドを、オブジェクトの型によって決定されるプロパティとメソッドを使用するのと同じ方法で使用することができます。 ただし、スクリプトを記述するときは、追加されたプロパティとメソッドがすべての PowerShell セッションに存在しない可能性があることに注意してください。

ファイルの詳細について `Types.ps1xml` は、「 [about_Types.ps1xml](../Microsoft.PowerShell.Core/About/about_Types.ps1xml.md)」を参照してください。 コマンドレットによって追加される動的な型データの詳細については `Update-TypeData` 、「」を参照してください `Update-TypeData` 。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: すべての拡張型データを取得する

この例では、現在のセッションのすべての拡張型データを取得します。

 ```powershell
Get-TypeData
```

### 例 2: 名前を指定して型を取得する

この例では、イベントを含む名前を持つ、現在のセッションのすべての型を取得します。

 ```powershell
"*Eventing*" | Get-TypeData
```

```Output
TypeName                                                  Members
--------                                                  -------
System.Diagnostics.Eventing.Reader.EventLogConfiguration  {}System.Diagnostics.Eventing.Reader.EventLogRecord
                                                          {}System.Diagnostics.Eventing.Reader.ProviderMetadata
                                                          {[ProviderName, System.Management.Automation.Runspaces.AliasProper...
```

### 例 3: プロパティ値を作成するスクリプトブロックを取得する

この例では、 **EventLogEntry** オブジェクトの **EventID** プロパティの値を作成するスクリプトブロックを取得します。

 ```powershell
(Get-TypeData *EventLogEntry*).Members.EventID
```

```Output
GetScriptBlock                     SetScriptBlock     IsHidden Name
--------------                     --------------     -------- ----
$this.get_EventID() -band 0xFFFF                         False EventID
```

### 例 4: 指定したオブジェクトのプロパティを定義するスクリプトブロックを取得する

この例では、PowerShell の **system.string オブジェクトの** **datetime** プロパティを定義するスクリプトブロックを取得します。

 ```powershell
(Get-TypeData -TypeName System.DateTime).Members["DateTime"].GetScriptBlock
if ((& { Set-StrictMode -Version 1; $this.DisplayHint }) -ieq  "Date") {
    "{0}" -f $this.ToLongDateString()
}
elseif ((& { Set-StrictMode -Version 1; $this.DisplayHint }) -ieq "Time") {
    "{0}" -f  $this.ToLongTimeString()
}
else {
    "{0} {1}" -f $this.ToLongDateString(), $this.ToLongTimeString()
}
```

このコマンドは、コマンドレットを使用して、 `Get-TypeData` **DataTime** 型の拡張型データを取得します。 コマンドは、**TypeData** オブジェクトの **Members** プロパティを取得します。

**Members** プロパティには、拡張型データで定義されているプロパティとメソッドのハッシュテーブルが含まれています。 Members ハッシュ テーブルの各キーは、プロパティ名またはメソッド名であり、各値は、プロパティ値またはメソッド値の定義です。

このコマンドは、**メンバー** の **DateTime** キーと **getscriptblock** プロパティ値を取得します。

出力には、PowerShell のすべての system.string オブジェクトの **datetime** プロパティの値を作成するスクリプトブロックが示されて **います。**

## パラメーター

### -TypeName

指定された名前を持つ型に対してのみ、型データを配列として指定します。 既定では、は、 `Get-TypeData` セッション内のすべての型を取得します。

型の名前または名前パターンを入力します。 System 名前空間の型であっても、フルネーム、またはワイルドカード文字を使用した名前パターンが必要です。 ワイルドカードがサポートされており、パラメーター名 **TypeName** は省略可能です。 パイプを使用して型名をにパイプすることもでき `Get-TypeData` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して型名をにすることができ `Get-TypeData` ます。

## 出力

### システムの管理.... TypeData

## メモ

`Get-TypeData` 現在のセッションの拡張型データのみを取得します。 このコマンドレットは、モジュールに定義されている一方で現在のセッションにインポートされていない拡張型のように、コンピューター上に存在していて現在のセッションに追加されていない拡張型データは取得しません。

## 関連リンク

[about_Types.ps1xml](../Microsoft.PowerShell.Core/About/about_Types.ps1xml.md)

[Remove-TypeData](Remove-TypeData.md)

[Update-TypeData](Update-TypeData.md)

