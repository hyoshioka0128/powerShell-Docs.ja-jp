---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-host?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Host
ms.openlocfilehash: 0775940f210cb028d7a0919bb8e5ca9f256b70d8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603019"
---
# Get-Host

## 概要
現在のホスト プログラムを表すオブジェクトを取得します。

## SYNTAX

```
Get-Host [<CommonParameters>]
```

## Description

コマンドレットは、Windows PowerShell をホストしている `Get-Host` プログラムを表すオブジェクトを取得します。

既定の表示には、Windows PowerShell のバージョン番号とホストで使用されている現在の地域と言語の設定が含まれていますが、ホスト オブジェクトには、現在実行中の Windows PowerShell のバージョンの詳細情報、現在のカルチャ、Windows PowerShell の UI カルチャなど多くの情報が含まれています。 このコマンドレットを使用すると、テキストや背景色など、ホストプログラムのユーザーインターフェイスの機能をカスタマイズすることもできます。

## 例

### 例 1: PowerShell コンソールホストに関する情報を取得する

```
PS C:\> Get-Host
Name             : ConsoleHost
Version          : 2.0
InstanceId       : e4e0ab54-cc5e-4261-9117-4081f20ce7a2
UI               : System.Management.Automation.Internal.Host.InternalHostUserInterface
CurrentCulture   : en-US
CurrentUICulture : en-US
PrivateData      : Microsoft.PowerShell.ConsoleHost+ConsoleColorProxy
IsRunspacePushed : False
Runspace         : System.Management.Automation.Runspaces.LocalRunspace
```

このコマンドは、この例の Windows PowerShell の現在のホスト プログラムである Windows PowerShell コンソールに関する情報を表示します。 これには、ホストの名前、ホストで実行されている Windows PowerShell のバージョン、現在のカルチャおよび UI カルチャが含まれます。

Version、UI、CurrentCulture、CurrentUICulture、PrivateData、および Runspace プロパティには、それぞれ非常に便利なプロパティを持つオブジェクトが含まれています。 後で示す例では、これらのプロパティを説明します。

### 例 2: PowerShell ウィンドウのサイズを変更する

```powershell
PS C:\> $H = Get-Host
PS C:\> $Win = $H.UI.RawUI.WindowSize
PS C:\> $Win.Height = 10
PS C:\> $Win.Width  = 10
PS C:\> $H.UI.RawUI.Set_WindowSize($Win)
```

このコマンドは、Windows PowerShell ウィンドウを 10 x 10 ピクセルにサイズ変更します。

### 例 3: ホストの PowerShell のバージョンを取得する

```powershell
PS C:\> (Get-Host).Version | Format-List -Property *
Major         : 2
Minor         : 0
Build         : -1
Revision      : -1
MajorRevision : -1
MinorRevision : -1
```

このコマンドは、ホストで実行されている Windows PowerShell のバージョンに関する詳細情報を取得します。
これらの値は表示できますが、変更することはできません。

の Version プロパティに `Get-Host` は、  system.string オブジェクトが含まれています。 このコマンドは、パイプライン演算子 (|) を使用して、バージョンオブジェクトをコマンドレットに送信し `Format-List` ます。 この `Format-List` コマンドは、 *Property* パラメーターを all (*) 値と共に使用して、version オブジェクトのすべてのプロパティとプロパティ値を表示します。

### 例 4: ホストの現在のカルチャを取得する

```powershell
PS C:\> (Get-Host).CurrentCulture | Format-List -Property *
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
```

このコマンドは、ホストで実行されている Windows PowerShell に設定されている現在のカルチャに関する詳細情報を取得します。 これは、コマンドレットによって返される情報と同じです `Get-Culture` 。

同様に、 **CurrentUICulture** プロパティはを返すのと同じオブジェクトを返し `Get-UICulture` ます。

ホストオブジェクトの **CurrentCulture** **プロパティには、system.string オブジェクトが** 含まれています。 このコマンドは、パイプライン演算子 (|) を使用して、 **CultureInfo** オブジェクトをコマンドレットに送信し `Format-List` ます。 この `Format-List` コマンドは、 *Property* パラメーターを all (*) 値と共に使用して、 **CultureInfo** オブジェクトのすべてのプロパティとプロパティ値を表示します。

### 例 5: 現在のカルチャの DateTimeFormat を取得する

```powershell
PS C:\> (Get-Host).CurrentCulture.DateTimeFormat | Format-List -Property *
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
```

このコマンドは、Windows PowerShell に使用されている現在のカルチャの DateTimeFormat に関する詳細情報を返します。

ホストオブジェクトの **CurrentCulture** プロパティには、 **CultureInfo** オブジェクトが含まれています。このオブジェクトには、多くの便利なプロパティがあります。 **DateTimeFormat** プロパティには、多くの便利なプロパティを持つ **DateTimeFormatInfo** オブジェクトが含まれています。

オブジェクトのプロパティに格納されているオブジェクトの型を検索するには、コマンドレットを使用し `Get-Member` ます。 オブジェクトのプロパティ値を表示するには、コマンドレットを使用し `Format-List` ます。

### 例 6: ホストの RawUI プロパティを取得する

```
PS C:\> (Get-Host).UI.RawUI | Format-List -Property *
ForegroundColor       : DarkYellow
BackgroundColor       : DarkBlue
CursorPosition        : 0,390
WindowPosition        : 0,341
CursorSize            : 25
BufferSize            : 120,3000
WindowSize            : 120,50
MaxWindowSize         : 120,81
MaxPhysicalWindowSize : 182,81
KeyAvailable          : False
WindowTitle           : Windows PowerShell 2.0 (04/11/2008 00:08:14)
```

このコマンドは、ホストオブジェクトの **Rawui** プロパティのプロパティを表示します。 これらの値を変更すると、ホスト プログラムの外観を変更することができます。

### 例 7: PowerShell コンソールの背景色を設定する

```powershell
PS C:\> (Get-Host).UI.RawUI.BackgroundColor = "Black"
PS C:\> cls
```

これらのコマンドは、Windows PowerShell コンソールの背景色を黒に変更します。 **Cls** コマンドは、関数のエイリアスで、 `Clear-Host` 画面をクリアし、画面全体を新しい色に変更します。

この変更は、現在のセッションでのみ有効です。 すべてのセッションでコンソールの背景色を変更するには、コマンドを Windows PowerShell プロファイルに追加します。

### 例 8: エラーメッセージの背景色を設定する

```
PS C:\> $Host.PrivateData.ErrorBackgroundColor = "white"
```

このコマンドは、エラー メッセージの背景色を白に変更します。

このコマンドは、 `$Host` 現在のホストプログラムのホストオブジェクトを含む自動変数を使用します。 `Get-Host` はを含む同じオブジェクトを返し `$Host` ます。そのため、これらを区別して使用することができます。

このコマンドは、ErrorBackgroundColor プロパティとしての **Privatedata** プロパティを使用し `$Host` ます。 オブジェクトのすべてのプロパティを表示するには、「」を参照してください `$Host` 。PrivateData プロパティ、「」と入力 `$host.privatedata | format-list *` します。

## PARAMETERS

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システムの管理. 内部ホストのホスト

`Get-Host` は、system.servicemodel. **内部ホスト** オブジェクトを返します。

## 注

`$Host`自動変数には、を返すのと同じオブジェクトが含まれて `Get-Host` おり、これを同じ方法で使用できます。 同様に、 `$PSCulture` および `$PSUICulture` 自動変数には、ホストオブジェクトの CurrentCulture プロパティと CurrentUICulture プロパティに格納されているものと同じオブジェクトが含まれています。 これらの機能は置き換えて使用できます。

詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

## 関連リンク

[Clear-Host](../Microsoft.PowerShell.Core/Clear-Host.md)

[Read-Host](Read-Host.md)

[Write-Host](Write-Host.md)

