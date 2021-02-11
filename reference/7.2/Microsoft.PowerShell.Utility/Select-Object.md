---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/25/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Select-Object
ms.openlocfilehash: 80882d27c9c8fa2d7f9e1c8ca00a02cf73ae94e6
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602593"
---
# Select-Object

## 概要
オブジェクトまたはオブジェクトのプロパティを選択します。

## SYNTAX

### DefaultParameter (既定値)

```
Select-Object [-InputObject <PSObject>] [[-Property] <Object[]>] [-ExcludeProperty <String[]>]
 [-ExpandProperty <String>] [-Unique] [-Last <Int32>] [-First <Int32>] [-Skip <Int32>] [-Wait]
 [<CommonParameters>]
```

### SkipLastParameter

```
Select-Object [-InputObject <PSObject>] [[-Property] <Object[]>] [-ExcludeProperty <String[]>]
 [-ExpandProperty <String>] [-Unique] [-SkipLast <Int32>] [<CommonParameters>]
```

### IndexParameter

```
Select-Object [-InputObject <PSObject>] [-Unique] [-Wait] [-Index <Int32[]>] [<CommonParameters>]
```

### SkipIndexParameter

```
Select-Object [-InputObject <PSObject>] [-Unique] [-SkipIndex <Int32[]>] [<CommonParameters>]
```

## Description

`Select-Object`コマンドレットでは、オブジェクトまたはオブジェクトのセットの指定されたプロパティを選択します。 加えて、配列内の一意のオブジェクト、指定された数のオブジェクト、または指定された位置にあるオブジェクトを選択することもできます。

コレクションからオブジェクトを選択するには、**First**、**Last**、**Unique**、**Skip**、および **Index** パラメーターを使用します。 オブジェクトのプロパティを選択するには、**Property** パラメーターを使用します。 [プロパティ] を選択すると、は `Select-Object` 指定されたプロパティのみを持つ新しいオブジェクトを返します。

Windows PowerShell 3.0 以降では、には、使用されて `Select-Object` いないオブジェクトの作成および処理を防止する最適化機能が含まれています。

コマンド `Select-Object` パイプラインに **最初** のパラメーターまたは **インデックス** のパラメーターを含むコマンドを含めると、オブジェクトを生成するコマンドがパイプラインのコマンドの前に表示されていても、選択したオブジェクトの数が生成されるとすぐに、PowerShell はオブジェクトを生成するコマンドを停止し `Select-Object` ます。 この最適化動作を無効にするには、**Wait** パラメーターを使用します。

## 例

### 例 1: プロパティによってオブジェクトを選択する

この例では、プロセスオブジェクトの **名前**、 **ID**、およびワーキングセット (**WS**) プロパティを持つオブジェクトを作成します。

```powershell
Get-Process | Select-Object -Property ProcessName, Id, WS
```

### 例 2: プロパティによってオブジェクトを選択し、結果を書式設定する

この例では、コンピューター上のプロセスによって使用されているモジュールに関する情報を取得します。 コマンドレットを使用して、 `Get-Process` コンピューター上のプロセスを取得します。

コマンドレットを使用して、 `Select-Object` `[System.Diagnostics.ProcessModule]` によって出力された各インスタンスの **Modules** プロパティに含まれるインスタンスの配列を出力し `System.Diagnostics.Process` `Get-Process` ます。

コマンドレットの **Property** パラメーターは、 `Select-Object` プロセス名を選択します。 これ `ProcessName` により、すべてのインスタンスに **"メモ" プロパティ** が追加さ `[System.Diagnostics.ProcessModule]` れ、現在のプロセスの **ProcessName** プロパティの値が設定されます。

最後に、コマンドレットを使用して、 `Format-List` 各プロセスの名前とモジュールを一覧に表示します。

```powershell
Get-Process Explorer | Select-Object -Property ProcessName -ExpandProperty Modules | Format-List
```

```Output
ProcessName       : explorer
ModuleName        : explorer.exe
FileName          : C:\WINDOWS\explorer.exe
BaseAddress       : 140697278152704
ModuleMemorySize  : 3919872
EntryPointAddress : 140697278841168
FileVersionInfo   : File:             C:\WINDOWS\explorer.exe
                    InternalName:     explorer
                    OriginalFilename: EXPLORER.EXE.MUI
                    FileVersion:      10.0.17134.1 (WinBuild.160101.0800)
                    FileDescription:  Windows Explorer
                    Product:          Microsoft Windows Operating System
                    ProductVersion:   10.0.17134.1
...
```

### 例 3: 最も多くのメモリを使用するプロセスを選択する

この例では、最も多くのメモリを使用している5つのプロセスを取得します。 `Get-Process`コマンドレットは、コンピューター上のプロセスを取得します。 `Sort-Object`コマンドレットは、メモリ (ワーキングセット) の使用状況に応じてプロセスを並べ替えます。このコマンドレットは、結果として `Select-Object` 得られるオブジェクトの配列の最後の5つのメンバーのみを選択します。

コマンドレットを含むコマンドでは、 **Wait** パラメーターは必要ありません。これは、が `Sort-Object` `Sort-Object` すべてのオブジェクトを処理してから、コレクションを返すためです。 `Select-Object`最適化は、オブジェクトが処理されたときにオブジェクトを個別に返すコマンドに対してのみ使用できます。

```powershell
Get-Process | Sort-Object -Property WS | Select-Object -Last 5
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VS(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
2866     320       33432      45764   203   222.41   1292 svchost
577      17        23676      50516   265    50.58   4388 WINWORD
826      11        75448      76712   188    19.77   3780 Ps
1367     14        73152      88736   216    61.69    676 Ps
1612     44        66080      92780   380   900.59   6132 INFOPATH
```

### 例 4: 配列から一意の文字を選択する

この例では、の **unique** パラメーターを使用して、 `Select-Object` 文字の配列から一意の文字を取得します。

```powershell
"a","b","c","a","a","a" | Select-Object -Unique
```

```Output
a
b
c
```

### 例 5: イベントログで最新および最も古いイベントを選択する

この例では、Windows PowerShell イベントログの最初の (最新の) イベントと最後の (最も古い) イベントを取得します。

`Get-EventLog` Windows PowerShell ログ内のすべてのイベントを取得し、変数に保存し `$a` ます。
次 `$a` に、はコマンドレットにパイプ処理され `Select-Object` ます。 この `Select-Object` コマンドは、 **Index** パラメーターを使用して、変数内のイベントの配列からイベントを選択し `$a` ます。 最初のイベントのインデックスは 0 です。 最後のイベントのインデックスは、の項目数から1を引いた値です `$a` 。

```powershell
$a = Get-EventLog -LogName "Windows PowerShell"
$a | Select-Object -Index 0, ($A.count - 1)
```

### 例 6: 最初のオブジェクト以外のすべてを選択する

この例では、最初のファイルを除き、Servers.txt ファイルに一覧表示されている各コンピューターに新しい PSSession を作成します。

`Select-Object` コンピューター名の一覧で、最初のコンピューター以外のすべてを選択します。 結果として得られるコンピューターの一覧は、コマンドレットの **ComputerName** パラメーターの値として設定され `New-PSSession` ます。

```powershell
New-PSSession -ComputerName (Get-Content Servers.txt | Select-Object -Skip 1)
```

### 例 7: ファイル名を変更し、いくつかを選択して確認する

この例では、読み取り専用属性を持つテキストファイルのベース名に "-ro" サフィックスを追加し、最初の5つのファイルを表示して、ユーザーが効果のサンプルを確認できるようにします。

`Get-ChildItem`**ReadOnly** 動的パラメーターを使用して、読み取り専用のファイルを取得します。 結果として得られるファイルはコマンドレットにパイプ処理され、ファイル名が変更され `Rename-Item` ます。 の **Passthru** パラメーターを使用して、 `Rename-Item` 名前が変更されたファイルをコマンドレットに送信し `Select-Object` ます。このコマンドレットは、表示する最初の5を選択します。

の **Wait** パラメーターを指定すると、 `Select-Object` `Get-ChildItem` 最初の5つの読み取り専用テキストファイルを取得した後、PowerShell がコマンドレットを停止できなくなります。 このパラメーターを指定しない場合、最初の 5 つの読み取り専用ファイルだけ名前が変更されます。

```powershell
Get-ChildItem *.txt -ReadOnly |
    Rename-Item -NewName {$_.BaseName + "-ro.txt"} -PassThru |
    Select-Object -First 5 -Wait
```

### 例 8:-ExpandProperty パラメーターの複雑さを示す

この例は、 **Expandproperty** パラメーターの複雑な例を示しています。

生成された出力はインスタンスの配列であることに注意して `[System.Int32]` ください。 インスタンスは、 **出力ビュー** の標準の書式設定規則に準拠しています。 これは、 *展開* されたプロパティに対しても当てはまります。 出力されたオブジェクトに特定の標準形式がある場合、展開されたプロパティは表示されない可能性があります。

```powershell
# Create a custom object to use for the Select-Object example.
$object = [pscustomobject]@{Name="CustomObject";Expand=@(1,2,3,4,5)}
# Use the ExpandProperty parameter to Expand the property.
$object | Select-Object -ExpandProperty Expand -Property Name
```

```Output
1
2
3
4
5
```

```powershell
# The output did not contain the Name property, but it was added successfully.
# Use Get-Member to confirm the Name property was added and populated.
$object | Select-Object -ExpandProperty Expand -Property Name | Get-Member
```

```Output
   TypeName: System.Int32

Name        MemberType   Definition
----        ----------   ----------
CompareTo   Method       int CompareTo(System.Object value), int CompareTo(int value), int IComparable.CompareTo(System.Object obj)...
Equals      Method       bool Equals(System.Object obj), bool Equals(int obj), bool IEquatable[int].Equals(int other)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
GetTypeCode Method       System.TypeCode GetTypeCode(), System.TypeCode IConvertible.GetTypeCode()
ToBoolean   Method       bool IConvertible.ToBoolean(System.IFormatProvider provider)
ToByte      Method       byte IConvertible.ToByte(System.IFormatProvider provider)
ToChar      Method       char IConvertible.ToChar(System.IFormatProvider provider)
ToDateTime  Method       datetime IConvertible.ToDateTime(System.IFormatProvider provider)
ToDecimal   Method       decimal IConvertible.ToDecimal(System.IFormatProvider provider)
ToDouble    Method       double IConvertible.ToDouble(System.IFormatProvider provider)
ToInt16     Method       int16 IConvertible.ToInt16(System.IFormatProvider provider)
ToInt32     Method       int IConvertible.ToInt32(System.IFormatProvider provider)
ToInt64     Method       long IConvertible.ToInt64(System.IFormatProvider provider)
ToSByte     Method       sbyte IConvertible.ToSByte(System.IFormatProvider provider)
ToSingle    Method       float IConvertible.ToSingle(System.IFormatProvider provider)
ToString    Method       string ToString(), string ToString(string format), string ToString(System.IFormatProvider provider)...
ToType      Method       System.Object IConvertible.ToType(type conversionType, System.IFormatProvider provider)
ToUInt16    Method       uint16 IConvertible.ToUInt16(System.IFormatProvider provider)
ToUInt32    Method       uint32 IConvertible.ToUInt32(System.IFormatProvider provider)
ToUInt64    Method       uint64 IConvertible.ToUInt64(System.IFormatProvider provider)
Name        NoteProperty string Name=CustomObject
```

### 例 9: オブジェクトのカスタムプロパティを作成する

次の例は、を使用して `Select-Object` 、任意のオブジェクトにカスタムプロパティを追加する方法を示しています。
存在しないプロパティ名を指定すると、は `Select-Object` 渡された各オブジェクトに対して、そのプロパティを " **メモ" プロパティ** として作成します。

```powershell
$customObject = 1 | Select-Object -Property MyCustomProperty
$customObject.MyCustomProperty = "New Custom Property"
$customObject
```

```Output
MyCustomProperty
----------------
New Custom Property
```

### 例 10: InputObject ごとに計算されるプロパティを作成する

この例では、を使用して `Select-Object` 、計算されたプロパティを入力に追加します。 **ScriptBlock** を **Property** パラメーターに渡すと、 `Select-Object` は渡された各オブジェクトに対して式を評価し、結果を出力に追加します。 **ScriptBlock** 内では、変数を使用して、 `$_` パイプライン内の現在のオブジェクトを参照できます。

既定で `Select-Object` は、は、プロパティの名前として **ScriptBlock** 文字列を使用します。 **Hashtable** を使用すると、各オブジェクトに追加されたカスタムプロパティとして **ScriptBlock** の出力にラベルを付けることができます。 に渡された各オブジェクトには、複数の計算されるプロパティを追加でき `Select-Object` ます。

```powershell
# Create a calculated property called $_.StartTime.DayOfWeek
Get-Process | Select-Object -Property ProcessName,{$_.StartTime.DayOfWeek}
```

```Output
ProcessName  $_.StartTime.DayOfWeek
----         ----------------------
alg                       Wednesday
ati2evxx                  Wednesday
ati2evxx                   Thursday
...
```

```powershell
# Add a custom property to calculate the size in KiloBytes of each FileInfo object you pass in.
# Use the pipeline variable to divide each file's length by 1 KiloBytes
$size = @{label="Size(KB)";expression={$_.length/1KB}}
# Create an additional calculated property with the number of Days since the file was last accessed.
# You can also shorten the key names to be 'l', and 'e', or use Name instead of Label.
$days = @{l="Days";e={((Get-Date) - $_.LastAccessTime).Days}}
# You can also shorten the name of your label key to 'l' and your expression key to 'e'.
Get-ChildItem $PSHOME -File | Select-Object Name, $size, $days
```

```Output
Name                        Size(KB)        Days
----                        --------        ----
Certificate.format.ps1xml   12.5244140625   223
Diagnostics.Format.ps1xml   4.955078125     223
DotNetTypes.format.ps1xml   134.9833984375  223
```

## PARAMETERS

### -ExcludeProperty

このコマンドレットによって操作から除外されるプロパティを指定します。 ワイルドカードを使用できます。

PowerShell 6 以降では、 **Excludeproperty** の **property** パラメーターを使用する必要がなくなりました。

```yaml
Type: System.String[]
Parameter Sets: DefaultParameter, SkipLastParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ExpandProperty

選択するプロパティを指定し、そのプロパティの展開を試みることを示します。

- 指定したプロパティが配列の場合は、配列の各値が出力に含まれます。
- 指定されたプロパティがオブジェクトの場合、オブジェクトのプロパティは **InputObject** ごとに展開されます。

どちらの場合も、出力されるオブジェクトの **型** は、展開されたプロパティの **型** と一致します。

**Property** パラメーターが指定されている場合、 `Select-Object` は、出力されたすべてのオブジェクトに対して、選択した各プロパティを **メモプロパティ** として追加しようとします。

> [!WARNING]
> 次のエラーが表示された場合: プロパティが既に存在するため、プロパティを処理できません `<PropertyName>` 。次の点を考慮してください。
> を使用する場合 `-ExpandProperty` 、 `Select-Object` 既存のプロパティを置き換えることはできないことに注意してください。
> これは、以下を意味します。
>
> - 展開されたオブジェクトに同じ名前のプロパティがある場合は、エラーが発生します。
> - *選択し* たオブジェクトに、*展開* されたオブジェクトのプロパティと同じ名前のプロパティがある場合は、エラーが発生します。

```yaml
Type: System.String
Parameter Sets: DefaultParameter, SkipLastParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -最初

入力オブジェクトの配列の先頭から選択するオブジェクトの数を指定します。

```yaml
Type: System.Int32
Parameter Sets: DefaultParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -インデックス

インデックス値に基づいて配列からオブジェクトを選択します。 インデックスをコンマ区切りリスト形式で入力します。 配列内インデックスは 0 で始まります。0 は最初の値を表し、(n-1) は最後の値を表します。

```yaml
Type: System.Int32[]
Parameter Sets: IndexParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

パイプラインを介してコマンドレットに送信するオブジェクトを指定します。 このパラメーターを使用すると、オブジェクトをにパイプすることができ `Select-Object` ます。

オブジェクトを **inputobject** パラメーターに渡すと、パイプラインを使用する代わりに、は、 `Select-Object` 値がコレクションの場合でも、 **inputobject** を1つのオブジェクトとして扱います。 にコレクションを渡す場合は、パイプラインを使用することをお勧めし `Select-Object` ます。

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

### -Last

入力オブジェクトの配列の末尾から選択するオブジェクトの数を指定します。

```yaml
Type: System.Int32
Parameter Sets: DefaultParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -プロパティ

選択するプロパティを指定します。 これらのプロパティは、出力オブジェクトに **"メモ" プロパティ** メンバーとして追加されます。 ワイルドカードを使用できます。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 集計プロパティを作成するには、ハッシュ テーブルを使用します。

有効なキーは次のとおりです。

- 名前 (またはラベル)- `<string>`
- 式 `<string>` または `<script block>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: DefaultParameter, SkipLastParameter
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Skip

指定した数の項目をスキップします (選択しません)。 既定では、 **Skip** パラメーターは、配列またはオブジェクトのリストの先頭からカウントされますが、コマンドが **最後** のパラメーターを使用する場合は、リストまたは配列の末尾からカウントされます。

カウントが 0 から始まる **Index** パラメーターとは異なり、**Skip** パラメーターではカウントが 1 から始まります。

```yaml
Type: System.Int32
Parameter Sets: DefaultParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipLast

リストまたは配列の末尾から、指定された数の項目をスキップします (選択しません)。 **最後** のパラメーターと共に **Skip** を使用する場合と同じ方法で動作します。

0からカウントを開始する **Index** パラメーターとは異なり、 **SkipLast** パラメーターは1から始まります。

```yaml
Type: System.Int32
Parameter Sets: SkipLastParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -一意

入力オブジェクトのサブセットが同じプロパティと値を持つ場合にサブセットの 1 つのメンバーのみを選択することを指定します。

このパラメーターでは、大文字と小文字が区別されます。 その結果、大文字と小文字のみが異なる文字列は一意であると見なされます。

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

### -Wait

コマンドレットによる最適化がオフになっていることを示します。 PowerShell では、コマンドがコマンドパイプラインに出現する順序で実行され、すべてのオブジェクトを生成できます。 既定では、コマンド `Select-Object` パイプラインに **最初** のパラメーターまたは **インデックス** パラメーターを含むコマンドを含めると、選択したオブジェクト数が生成されるとすぐに、オブジェクトを生成するコマンドが PowerShell によって停止されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DefaultParameter, IndexParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipIndex

```yaml
Type: System.Int32[]
Parameter Sets: SkipIndexParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをにパイプすることができ `Select-Object` ます。

## 出力

### システム管理. PSObject

## 注

- コマンドレットは、組み込みエイリアスであるを使用して参照することもでき `Select-Object` `select` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

- の最適化機能 `Select-Object` は、オブジェクトが処理されるときにパイプラインにオブジェクトを書き込むコマンドに対してのみ使用できます。 処理したオブジェクトをバッファーに格納してコレクションとして書き込むコマンドには作用しません。 オブジェクトをすぐに書き込むのは、コマンドレットのデザイン上のベスト プラクティスです。 詳細については、「 _1 つのレコードをパイプラインに書き込む_ [」を参照](/powershell/scripting/developer/windows-powershell)してください。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[Group-Object](Group-Object.md)

[Sort-Object](Sort-Object.md)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)
