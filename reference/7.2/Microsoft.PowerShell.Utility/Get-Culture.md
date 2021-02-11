---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 11/01/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-culture?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Culture
ms.openlocfilehash: dc49f153adc22b7c2c943a4cc529ac155561228a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600593"
---
# Get-Culture

## 概要
オペレーティング システムの現在のカルチャ設定を取得します。

## SYNTAX

### CurrentCulture (既定値)

```
Get-Culture [-NoUserOverrides] [<CommonParameters>]
```

### 名前

```
Get-Culture [-Name <String[]>] [-NoUserOverrides] [<CommonParameters>]
```

### ListAvailable

```
Get-Culture [-ListAvailable] [<CommonParameters>]
```

## Description

`Get-Culture`コマンドレットは、現在のカルチャ設定に関する情報を取得します。 取得される情報には、システムの現在の言語設定 (キーボード レイアウトなど) や各種項目の表示形式 (数字、通貨、日付など) についての情報が含まれます。

また、コマンドレットを使用することもできます `Get-UICulture` 。このコマンドレットは、システム上の現在のユーザーインターフェイスカルチャと、International モジュールの [Set culture](/powershell/module/international/set-culture) コマンドレットを取得します。 ユーザー インターフェイス (UI) カルチャは、メニューやメッセージなどのユーザー インターフェイス要素に使用するテキスト文字列を決定します。

## 例

### 例 1: カルチャ設定を取得する

```powershell
Get-Culture
```

```Output
LCID             Name             DisplayName
----             ----             -----------
1033             en-US            English (United States)
```

このコマンドは、コンピューター上の地域設定についての情報を表示します。

### 例 2: culture オブジェクトのプロパティの書式を設定する

```powershell
PS C:\> $C = Get-Culture
PS C:\> $C | Format-List -Property *
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
CompareInfo                    : CompareInfo - 1033
TextInfo                       : TextInfo - 1033
IsNeutralCulture               : False
CultureTypes                   : SpecificCultures, InstalledWin32Cultures, FrameworkCultures
NumberFormat                   : System.Globalization.NumberFormatInfo
DateTimeFormat                 : System.Globalization.DateTimeFormatInfo
Calendar                       : System.Globalization.GregorianCalendar
OptionalCalendars              : {System.Globalization.GregorianCalendar, System.Globalization.GregorianCalendar}
UseUserOverride                : True
IsReadOnly                     : False

PS C:\> $C.Calendar
MinSupportedDateTime : 1/1/0001 12:00:00 AM
MaxSupportedDateTime : 12/31/9999 11:59:59 PM
AlgorithmType        : SolarCalendar
CalendarType         : Localized
Eras                 : {1}
TwoDigitYearMax      : 2029
IsReadOnly           : False

PS C:\> $C.DateTimeFormat
AMDesignator                     : AM
Calendar                         : System.Globalization.GregorianCalendar
DateSeparator                    : /
FirstDayOfWeek                   : Sunday
CalendarWeekRule                 : FirstDay
FullDateTimePattern              : dddd, MMMM dd, yyyy h:mm:ss tt
LongDatePattern                  : dddd, MMMM dd, yyyy
LongTimePattern                  : h:mm:ss tt
MonthDayPattern                  : MMMM dd
PMDesignator                     : PM
RFC1123Pattern                   : ddd, dd MMM yyyy HH':'mm':'ss 'GMT'
ShortDatePattern                 : M/d/yyyy
ShortTimePattern                 : h:mm tt
SortableDateTimePattern          : yyyy'-'MM'-'dd'T'HH':'mm':'ss
TimeSeparator                    : :
UniversalSortableDateTimePattern : yyyy'-'MM'-'dd HH':'mm':'ss'Z'
YearMonthPattern                 : MMMM, yyyy
AbbreviatedDayNames              : {Sun, Mon, Tue, Wed...}
ShortestDayNames                 : {Su, Mo, Tu, We...}
DayNames                         : {Sunday, Monday, Tuesday, Wednesday...}
AbbreviatedMonthNames            : {Jan, Feb, Mar, Apr...}
MonthNames                       : {January, February, March, April...}
IsReadOnly                       : False
NativeCalendarName               : Gregorian Calendar
AbbreviatedMonthGenitiveNames    : {Jan, Feb, Mar, Apr...}
MonthGenitiveNames               : {January, February, March, April...}

PS C:\> $C.DateTimeFormat.FirstDayOfWeek
Sunday
```

この例では、カルチャ オブジェクトに含まれている多数のデータが表示されます。 オブジェクトのプロパティとサブプロパティを表示する方法がわかります。

最初のコマンドは、 `Get-Culture` コマンドレットを使用して、コンピューター上の現在のカルチャ設定を取得します。
結果として得られるカルチャオブジェクトは変数に格納され `$C` ます。

2 番目のコマンドは、カルチャ オブジェクトのすべてのプロパティを表示します。 パイプライン演算子 () を使用して、 `|` のカルチャオブジェクトを `$C` コマンドレットに送信し `Format-List` ます。 **Property** パラメーターを使用して `*` 、オブジェクトのすべての () プロパティを表示します。 このコマンドは、のように省略でき `$c | fl *` ます。

その他のコマンドは、ドット表記でカルチャ オブジェクトのプロパティを指定することにより、そのプロパティの値を表示します。 この表記を使用すると、カルチャ オブジェクトの任意のプロパティの値を表示できます。

3番目のコマンドは、ドット表記を使用して、カルチャオブジェクトの **Calendar** プロパティの値を表示します。

4番目のコマンドは、ドット表記を使用して、culture オブジェクトの **DataTimeFormat** プロパティの値を表示します。

このプロパティには、多くのサブプロパティが含まれていることがわかります。 5番目のコマンドは、ドット表記を使用して、 **DateTimeFormat** プロパティの **FirstDayOfWeek** プロパティの値を表示します。

### 例 3: 特定のカルチャを取得する

フランスのフランス語の CultureInfo オブジェクトを取得します。

```powershell
Get-Culture -Name fr-FR
```

```Output
LCID             Name             DisplayName
----             ----             -----------
1036             fr-FR            French (France)
```

## PARAMETERS

### -ListAvailable

現在のオペレーティングシステムでサポートされているすべてのカルチャを取得します。

このパラメーターは、PowerShell 6.2 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ListAvailable
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

名前に基づいて特定のカルチャを取得します。

このパラメーターは、PowerShell 6.2 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -NoUserOverrides

現在のカルチャに対するユーザーの変更を無視します。

このパラメーターは、PowerShell 6.2 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: CurrentCulture, Name
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

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システムのグローバリゼーション

`Get-Culture` 現在のカルチャを表すオブジェクトを返します。

## 注

変数および変数を使用することもでき `$PsCulture` `$PsUICulture` ます。 変数には `$PsCulture` 現在のカルチャの名前が格納され、変数には `$PsUICulture` 現在の UI カルチャの名前が格納されます。

## 関連リンク

[設定-カルチャ](/powershell/module/international/set-culture)

[Get-UICulture](Get-UICulture.md)
