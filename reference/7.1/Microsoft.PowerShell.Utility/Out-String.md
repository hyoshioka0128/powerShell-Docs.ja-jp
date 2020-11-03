---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/29/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/out-string?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-String
ms.openlocfilehash: 16dc25e3468eaf3126b3286cfd71bfea9627c015
ms.sourcegitcommit: c8d1ffeab215e74e87ea1b0af8cd606c1a6a80ab
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "93220128"
---
# Out-String

## 概要
入力オブジェクトを文字列として出力します。

## SYNTAX

### None Wline書式設定 (既定)

```
Out-String [-Width <Int32>] [-NoNewline] [-InputObject <PSObject>] [<CommonParameters>]
```

### StreamFormatting 設定

```
Out-String [-Stream] [-Width <Int32>] [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

`Out-String`コマンドレットは、入力オブジェクトを文字列に変換します。 既定では、は `Out-String` 文字列を累積して1つの文字列として返しますが、 **Stream** パラメーターを使用して `Out-String` 、一度に1行を返すように指示したり、文字列の配列を作成したりすることができます。 オブジェクトを簡単に操作できないときに、このコマンドレットを使用すると、従来のシェルで行う場合と同じように文字列出力を検索して操作することができます。

## 例

### 例 1: 現在のカルチャを取得し、データを文字列に変換する

この例では、現在のユーザーの地域設定を取得し、オブジェクトデータを文字列に変換します。

```powershell
$C = Get-Culture | Select-Object -Property *
Out-String -InputObject $C -Width 100
```

```Output
Parent                         : en
LCID                           : 1033
KeyboardLayoutId               : 1033
Name                           : en-US
IetfLanguageTag                : en-US
DisplayName                    : English (United States)
NativeName                     : English (United States)
EnglishName                    : English (United States)
TwoLetterISOLanguageName       : en
ThreeLetterISOLanguageName     : eng
ThreeLetterWindowsLanguageName : ENU
CompareInfo                    : CompareInfo - en-US
TextInfo                       : TextInfo - en-US
IsNeutralCulture               : False
CultureTypes                   : SpecificCultures, InstalledWin32Cultures, FrameworkCultures
NumberFormat                   : System.Globalization.NumberFormatInfo
DateTimeFormat                 : System.Globalization.DateTimeFormatInfo
Calendar                       : System.Globalization.GregorianCalendar
OptionalCalendars              : {System.Globalization.GregorianCalendar,
                                 System.Globalization.GregorianCalendar}
UseUserOverride                : True
IsReadOnly                     : False
```

`$C`変数にはSelected.System が格納され **ます。グローバリゼーション** オブジェクト。 オブジェクトは、 `Get-Culture` パイプラインの出力をに送信した結果です `Select-Object` 。 **Property** パラメーターは、アスタリスク ( `*` ) ワイルドカードを使用して、オブジェクトに含まれるすべてのプロパティを指定します。

`Out-String`**InputObject** パラメーターを使用して、変数に格納されている **CultureInfo** オブジェクトを指定し `$C` ます。 内のオブジェクト `$C` は、文字列に変換されます。

> [!NOTE]
> 配列を表示するには `Out-String` 、出力を変数に格納し、配列インデックスを使用して要素を表示します。 配列インデックスの詳細については、「 [about_Arrays](../microsoft.powershell.core/about/about_arrays.md)」を参照してください。
>
> `$str = Out-String -InputObject $C -Width 100`

### 例 2: オブジェクトの操作

この例では、オブジェクトの使用と文字列の使用の違いを示します。 コマンドを実行すると、 **gcm** というテキストを含むエイリアスが表示され `Get-Command` ます。

```powershell
Get-Alias | Out-String -Stream | Select-String -Pattern "gcm"
```

```Output
Alias           gcm -> Get-Command
```

`Get-Alias` 各エイリアスに対して1つずつ、 **システム管理のオートメーション** オブジェクトを取得し、そのオブジェクトをパイプラインの下に送信します。 `Out-String`**Stream** パラメーターを使用して、すべてのオブジェクトを1つの文字列に連結するのではなく、各オブジェクトを文字列に変換します。 **System.string** オブジェクトはパイプラインを介して送信され、 `Select-String` **Pattern** パラメーターを使用してテキスト **gcm** の一致を検索します。

> [!NOTE]
> **ストリーム** パラメーターを省略した場合、は `Select-String` を返す単一の文字列で **gcm** というテキストを検索するため、すべてのエイリアスがコマンドによって表示され `Out-String` ます。

### 例 3: Width パラメーターを使用して、切り捨てが行われないようにします。

のほとんどの出力 `Out-String` は次の行にラップされますが、に渡される前に、書式設定システムによって出力が切り捨てられる場合があり `Out-String` ます。 **Width** パラメーターを使用して切り捨てを回避することができます。

```powershell
PS> @{TestKey = ('x' * 200)} | Out-String
Name                           Value
----                           -----
TestKey                        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx...

PS> @{TestKey = ('x' * 200)} | Out-String -Width 250

Name                           Value
----                           -----
TestKey                        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## PARAMETERS

### -InputObject

文字列に書き込むオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -なし Wline

PowerShell フォーマッタによって生成された出力からすべての改行を削除します。 文字列オブジェクトの一部である改行は保持されます。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NoNewLineFormatting
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ストリーム

コマンドレットが入力オブジェクトの各行に対して個別の文字列を送信することを示します。 既定では、各オブジェクトのための文字列が蓄積されて 1 つの文字列として送信されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: StreamFormatting
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Width

出力の各行の文字数を指定します。 その他の文字は、使用されるフォーマッタコマンドレットに応じて、次の行に折り返されるか、または切り捨てられます。 **Width** パラメーターは、書式設定されているオブジェクトにのみ適用されます。 このパラメーターを省略した場合、ホスト プログラムの特性によって幅が決まります。 ターミナル (コンソール) ウィンドウでは、現在のウィンドウの幅が既定値として使用されます。 PowerShell コンソールウィンドウは、インストール時に既定で80文字の幅に設定されます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

オブジェクトをパイプラインからに送信でき `Out-String` ます。

## 出力

### System.String

`Out-String` 入力オブジェクトから作成された文字列を返します。

## 注

動詞を含むコマンドレットは、 `Out` オブジェクトを書式設定しません。 `Out`コマンドレットは、指定された表示先のフォーマッタにオブジェクトを送信します。

## 関連リンク

[about_Formatting](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)

[Out-Default](../Microsoft.PowerShell.Core/Out-Default.md)

[Out-File](Out-File.md)

[Out-Host](../Microsoft.PowerShell.Core/Out-Host.md)

[Out-Null](../Microsoft.PowerShell.Core/Out-Null.md)

[Out-GridView](Out-GridView.md)

[Out-Printer](Out-Printer.md)

