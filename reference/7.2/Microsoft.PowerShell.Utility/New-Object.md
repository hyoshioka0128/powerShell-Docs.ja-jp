---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Object
ms.openlocfilehash: 3b2db400c5a8a1c61ec30ecee07f91d29b5eb3c1
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602215"
---
# New-Object

## 概要
Microsoft .NET Framework または COM オブジェクトのインスタンスを作成します。

## SYNTAX

### Net (既定値)

```
New-Object [-TypeName] <String> [[-ArgumentList] <Object[]>] [-Property <IDictionary>] [<CommonParameters>]
```

### Com

```
New-Object [-ComObject] <String> [-Strict] [-Property <IDictionary>] [<CommonParameters>]
```

## Description

`New-Object`コマンドレットでは、.NET Framework または COM オブジェクトのインスタンスを作成します。

.NET Framework クラスの型または COM オブジェクトの ProgID のどちらかを指定できます。 既定では、.NET Framework クラスの完全修飾名を入力すると、そのクラスのインスタンスへの参照が返されます。 COM オブジェクトのインスタンスを作成するには、 **ComObject** パラメーターを使用して、オブジェクトの ProgID を値として指定します。

## 例

### 例 1: system.object オブジェクトを作成する

この例では、コンストラクターとして "1.2.3.4" 文字列を使用して **system.string オブジェクトを** 作成します。

```powershell
New-Object -TypeName System.Version -ArgumentList "1.2.3.4"
```

```Output
Major  Minor  Build  Revision
-----  -----  -----  --------
1      2      3      4
```

### 例 2: Internet Explorer COM オブジェクトを作成する

この例では、Internet Explorer アプリケーションを表す COM オブジェクトのインスタンスを2つ作成します。 最初のインスタンスは、 **プロパティ** パラメーターハッシュテーブルを使用して **Navigate2** メソッドを呼び出し、オブジェクトの **visible** プロパティをに設定して、 `$True` アプリケーションが表示されるようにします。
2番目のインスタンスは、個々のコマンドで同じ結果を取得します。

```powershell
$IE1 = New-Object -COMObject InternetExplorer.Application -Property @{Navigate2="www.microsoft.com"; Visible = $True}

# The following command gets the same results as the example above.
$IE2 = New-Object -COMObject InternetExplorer.Application`
$IE2.Navigate2("www.microsoft.com")`
$IE2.Visible = $True`
```

### 例 3: Strict パラメーターを使用して、終了しないエラーを生成する

この例では、 **厳密** なパラメーターを追加すると、 `New-Object` COM オブジェクトが相互運用機能アセンブリを使用しているときに、コマンドレットが終了しないエラーを生成することを示しています。

```powershell
$A = New-Object -COMObject Word.Application -Strict -Property @{Visible = $True}
```

```Output
New-Object : The object written to the pipeline is an instance of the type
"Microsoft.Office.Interop.Word.ApplicationClass" from the component's primary interop assembly. If
this type exposes different members than the IDispatch members, scripts written to work with this
object might not work if the primary interop assembly is not installed.

At line:1 char:14
+ $A = New-Object  <<<< -COM Word.Application -Strict; $a.visible=$true
```

### 例 4: Windows デスクトップを管理するための COM オブジェクトを作成する

この例では、COM オブジェクトを作成および使用して Windows デスクトップを管理する方法を示します。

最初のコマンドは、コマンドレットの **ComObject** パラメーターを使用して、 `New-Object` **アプリケーション** PROGID を使用して COM オブジェクトを作成します。 結果として得られるオブジェクトは変数に格納され `$ObjShell` ます。 2番目のコマンドは、変数をコマンドレットにパイプして、 `$ObjShell` `Get-Member` COM オブジェクトのプロパティとメソッドを表示します。 これらのメソッドの中には、 **ToggleDesktop** メソッドがあります。 3 番目のコマンドは、オブジェクトの **ToggleDesktop** メソッドを呼び出して、デスクトップ上の開いているウィンドウを最小化します。

```powershell
$Objshell = New-Object -COMObject "Shell.Application"
$objshell | Get-Member
$objshell.ToggleDesktop()
```

```Output
   TypeName: System.__ComObject#{866738b9-6cf2-4de8-8767-f794ebe74f4e}

Name                 MemberType Definition
----                 ---------- ----------
AddToRecent          Method     void AddToRecent (Variant, string)
BrowseForFolder      Method     Folder BrowseForFolder (int, string, int, Variant)
CanStartStopService  Method     Variant CanStartStopService (string)
CascadeWindows       Method     void CascadeWindows ()
ControlPanelItem     Method     void ControlPanelItem (string)
EjectPC              Method     void EjectPC ()
Explore              Method     void Explore (Variant)
ExplorerPolicy       Method     Variant ExplorerPolicy (string)
FileRun              Method     void FileRun ()
FindComputer         Method     void FindComputer ()
FindFiles            Method     void FindFiles ()
FindPrinter          Method     void FindPrinter (string, string, string)
GetSetting           Method     bool GetSetting (int)
GetSystemInformation Method     Variant GetSystemInformation (string)
Help                 Method     void Help ()
IsRestricted         Method     int IsRestricted (string, string)
IsServiceRunning     Method     Variant IsServiceRunning (string)
MinimizeAll          Method     void MinimizeAll ()
NameSpace            Method     Folder NameSpace (Variant)
Open                 Method     void Open (Variant)
RefreshMenu          Method     void RefreshMenu ()
ServiceStart         Method     Variant ServiceStart (string, Variant)
ServiceStop          Method     Variant ServiceStop (string, Variant)
SetTime              Method     void SetTime ()
ShellExecute         Method     void ShellExecute (string, Variant, Variant, Variant, Variant)
ShowBrowserBar       Method     Variant ShowBrowserBar (string, Variant)
ShutdownWindows      Method     void ShutdownWindows ()
Suspend              Method     void Suspend ()
TileHorizontally     Method     void TileHorizontally ()
TileVertically       Method     void TileVertically ()
ToggleDesktop        Method     void ToggleDesktop ()
TrayProperties       Method     void TrayProperties ()
UndoMinimizeALL      Method     void UndoMinimizeALL ()
Windows              Method     IDispatch Windows ()
WindowsSecurity      Method     void WindowsSecurity ()
WindowSwitcher       Method     void WindowSwitcher ()
Application          Property   IDispatch Application () {get}
Parent               Property   IDispatch Parent () {get}
```

### 例 5: 複数の引数をコンストラクターに渡す

この例では、複数のパラメーターを受け取るコンストラクターを使用してオブジェクトを作成する方法を示します。 **ArgumentList** parameter を使用する場合、パラメーターは配列に配置する必要があります。

```powershell
$array = @('One', 'Two', 'Three')
$parameters = @{
    TypeName = 'System.Collections.Generic.HashSet[string]'
    ArgumentList = ([string[]]$array, [System.StringComparer]::OrdinalIgnoreCase)
}
$set = New-Object @parameters
```

PowerShell は、配列の各メンバーをコンストラクターのパラメーターにバインドします。

> [!NOTE]
> この例では、読みやすくするためにパラメータースプラッティングを使用します。 詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md)」を参照してください。

### 例 6: 1 つのパラメーターとして配列を受け取るコンストラクターを呼び出す

この例では、配列またはコレクションであるパラメーターを受け取るコンストラクターを使用してオブジェクトを作成する方法を示します。 配列パラメーターは、別の配列内にラップする必要があります。

```powershell
$array = @('One', 'Two', 'Three')
# This command throws an exception.
$set = New-Object -TypeName 'System.Collections.Generic.HashSet[string]' -ArgumentList $array
# This command succeeds.
$set = New-Object -TypeName 'System.Collections.Generic.HashSet[string]' -ArgumentList (,[string[]]$array)
$set
```

```Output
New-Object : Cannot find an overload for "HashSet`1" and the argument count: "3".
At line:1 char:8
+ $set = New-Object -TypeName 'System.Collections.Generic.HashSet[strin ...
+        ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : InvalidOperation: (:) [New-Object], MethodException
+ FullyQualifiedErrorId : ConstructorInvokedThrowException,Microsoft.PowerShell.Commands.NewObjectCommand

One
Two
Three
```

この例で最初にオブジェクトを作成しようとすると失敗します。 PowerShell は、の3つのメンバーをコンストラクターのパラメーターにバインドしようとしまし `$array` たが、コンストラクターは3つのパラメーターを受け取りません。 `$array`別の配列をラップすると、PowerShell がの3つのメンバーを `$array` コンストラクターのパラメーターにバインドできなくなります。

## PARAMETERS

### -ArgumentList

.NET Framework クラスのコンストラクターに渡す引数の配列を指定します。 コンストラクターが配列である1つのパラメーターを受け取る場合は、そのパラメーターを別の配列内にラップする必要があります。 次に例を示します。

`$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList (,$bytes)`

**ArgumentList** の動作の詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md#splatting-with-arrays)」を参照してください。

**ArgumentList** のエイリアスは、**Args** です。

```yaml
Type: System.Object[]
Parameter Sets: Net
Aliases: Args

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComObject

COM オブジェクトのプログラム識別子 (ProgID) を指定します。

```yaml
Type: System.String
Parameter Sets: Com
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -プロパティ

プロパティ値を設定し、新しいオブジェクトのメソッドを呼び出します。

プロパティまたはメソッドの名前をキー、プロパティの値またはメソッドの引数を値とするハッシュ テーブルを入力します。 `New-Object` オブジェクトを作成し、各プロパティ値を設定し、ハッシュテーブルに出現する順序で各メソッドを呼び出します。

新しいオブジェクトが **PSObject** クラスから派生していて、オブジェクトに存在しないプロパティを指定した場合、は、 `New-Object` 指定されたプロパティを "メモ" プロパティとしてオブジェクトに追加します。 オブジェクトが **PSObject** でない場合、コマンドは終了しないエラーを生成します。

```yaml
Type: System.Collections.IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Strict

作成しようとした COM オブジェクトが相互運用機能アセンブリを使用するときに、コマンドレットが終了しないエラーを生成することを示します。 この機能は、実際の COM オブジェクトと、COM 呼び出し可能ラッパーを含む .NET Framework オブジェクトとを識別します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Com
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeName

.NET Framework クラスの完全修飾名を指定します。 **TypeName** パラメーターと **ComObject** パラメーターの両方を指定することはできません。

```yaml
Type: System.String
Parameter Sets: Net
Aliases:

Required: True
Position: 0
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

### Object

`New-Object` 作成されたオブジェクトを返します。

## 注

- `New-Object` VBScript の CreateObject 関数の中で最もよく使用される機能を提供します。 VBScript のようなステートメントは、 `Set objShell = CreateObject("Shell.Application")` PowerShell でに変換でき `$objShell = New-Object -COMObject "Shell.Application"` ます。
- `New-Object` では、Windows スクリプトホスト環境で使用できる機能が拡張されており、コマンドラインやスクリプト内から .NET Framework オブジェクトを簡単に操作できるようになっています。

## 関連リンク

[about_Object_Creation](../Microsoft.PowerShell.Core/About/about_Object_Creation.md)

[Compare-Object](Compare-Object.md)

[Group-Object](Group-Object.md)

[Measure-Object](Measure-Object.md)

[Select-Object](Select-Object.md)

[Sort-Object](Sort-Object.md)

[Tee-Object](Tee-Object.md)

