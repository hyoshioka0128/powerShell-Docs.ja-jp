---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/compare-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Compare-Object
ms.openlocfilehash: 6bed0e8d13cb834fab97dc0265ef7d46a2caea97
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599734"
---
# Compare-Object

## 構文
2 つのオブジェクトのセットを比較します。

## 構文

```
Compare-Object [-ReferenceObject] <PSObject[]> [-DifferenceObject] <PSObject[]>
 [-SyncWindow <Int32>] [-Property <Object[]>] [-ExcludeDifferent] [-IncludeEqual] [-PassThru]
 [-Culture <String>] [-CaseSensitive] [<CommonParameters>]
```

## 説明

コマンドレットでは、 `Compare-Object` 2 つのオブジェクトのセットを比較します。 オブジェクトの1つのセットが **参照** で、もう一方のオブジェクトセットが **違い** ます。

`Compare-Object` オブジェクト全体を比較するために使用できるメソッドがあるかどうかを確認します。 適切なメソッドが見つからない場合は、入力オブジェクトの **ToString ()** メソッドを呼び出し、文字列の結果を比較します。 比較に使用する1つ以上のプロパティを指定できます。 プロパティが指定されている場合、コマンドレットはこれらのプロパティの値のみを比較します。

比較の結果は、プロパティ値が **参照** オブジェクトにのみ表示されるか ()、 `<=` または **差分** オブジェクト () にのみ表示されるかを示し `=>` ます。 **Includeequal** パラメーターが使用されている場合、( `==` ) は、値が両方のオブジェクトにあることを示します。

**参照** または **差分** オブジェクトが null () の場合 `$null` 、は `Compare-Object` 終了エラーを生成します。

いくつかの例では、スプラッティングを使用して、コードサンプルの行の長さを減らしています。 詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md)」を参照してください。

## 例

### 例 1-2 つのテキストファイルの内容を比較する

この例では、2つのテキストファイルの内容を比較します。 この例では、次の2つのテキストファイルを使用します。それぞれの値は別々の行にあります。

- `Testfile1.txt` には、dog、squirrel、および鳥の値が含まれています。
- `Testfile2.txt` には、cat、鳥、および racoon の値が含まれています。

出力には、ファイル間で異なる行のみが表示されます。 `Testfile1.txt` は **参照** オブジェクト () で、 `<=` `Testfile2.txt` は **相違** オブジェクト ( `=>` ) です。 両方のファイルに表示されるコンテンツを含む行は表示されません。

```powershell
Compare-Object -ReferenceObject (Get-Content -Path C:\Test\Testfile1.txt) -DifferenceObject (Get-Content -Path C:\Test\Testfile2.txt)
```

```Output
InputObject SideIndicator
----------- -------------
cat         =>
racoon      =>
dog         <=
squirrel    <=
```

### 例 2-コンテンツの各行を比較し、その違いを除外する

この例では、 **excludedifferent** パラメーターを使用して、2つのテキストファイルの内容の各行を比較します。

PowerShell 7.1 の時点で、 **Excludedifferent** パラメーターを使用すると、 **includeequal** が推論され、出力には両方のファイルに含まれる行のみが含まれます ( **sideindicator** () を参照 `==` )。

```powershell
$objects = @{
  ReferenceObject = (Get-Content -Path C:\Test\Testfile1.txt)
  DifferenceObject = (Get-Content -Path C:\Test\Testfile2.txt)
}
Compare-Object @objects -ExcludeDifferent
```

```Output
InputObject SideIndicator
----------- -------------
bird        ==
```

<a id="ex3" />

### 例 3-PassThru パラメーターを使用する場合の相違点の表示

通常、は、 `Compare-Object` 次のプロパティを持つ **Pscustomobject** 種類を返します。

- 比較対象の **InputObject**
- 出力が属する入力オブジェクトを示す **Sideindicator** プロパティ

**PassThru** パラメーターを使用する場合、オブジェクトの **型** は変更されませんが、返されたオブジェクトのインスタンスには、 **sideindicator** という名前の追加の **プロパティ** があります。 **Sideindicator** は、出力が属する入力オブジェクトを示します。

次の例は、さまざまな出力の種類を示しています。

```powershell
$a = $True
Compare-Object -IncludeEqual $a $a
(Compare-Object -IncludeEqual $a $a) | Get-Member
```

```Output
InputObject SideIndicator
----------- -------------
       True ==

   TypeName: System.Management.Automation.PSCustomObject
Name          MemberType   Definition
----          ----------   ----------
Equals        Method       bool Equals(System.Object obj)
GetHashCode   Method       int GetHashCode()
GetType       Method       type GetType()
ToString      Method       string ToString()
InputObject   NoteProperty System.Boolean InputObject=True
SideIndicator NoteProperty string SideIndicator===
```

```powershell
Compare-Object -IncludeEqual $a $a -PassThru
(Compare-Object -IncludeEqual $a $a -PassThru) | Get-Member
```

```Output
True

   TypeName: System.Boolean
Name          MemberType   Definition
----          ----------   ----------
CompareTo     Method       int CompareTo(System.Object obj), int CompareTo(bool value), int IComparable.CompareTo(Syst
Equals        Method       bool Equals(System.Object obj), bool Equals(bool obj), bool IEquatable[bool].Equals(bool ot
GetHashCode   Method       int GetHashCode()
GetType       Method       type GetType()
GetTypeCode   Method       System.TypeCode GetTypeCode(), System.TypeCode IConvertible.GetTypeCode()
ToBoolean     Method       bool IConvertible.ToBoolean(System.IFormatProvider provider)
ToByte        Method       byte IConvertible.ToByte(System.IFormatProvider provider)
ToChar        Method       char IConvertible.ToChar(System.IFormatProvider provider)
ToDateTime    Method       datetime IConvertible.ToDateTime(System.IFormatProvider provider)
ToDecimal     Method       decimal IConvertible.ToDecimal(System.IFormatProvider provider)
ToDouble      Method       double IConvertible.ToDouble(System.IFormatProvider provider)
ToInt16       Method       short IConvertible.ToInt16(System.IFormatProvider provider)
ToInt32       Method       int IConvertible.ToInt32(System.IFormatProvider provider)
ToInt64       Method       long IConvertible.ToInt64(System.IFormatProvider provider)
ToSByte       Method       sbyte IConvertible.ToSByte(System.IFormatProvider provider)
ToSingle      Method       float IConvertible.ToSingle(System.IFormatProvider provider)
ToString      Method       string ToString(), string ToString(System.IFormatProvider provider), string IConvertible.To
ToType        Method       System.Object IConvertible.ToType(type conversionType, System.IFormatProvider provider)
ToUInt16      Method       ushort IConvertible.ToUInt16(System.IFormatProvider provider)
ToUInt32      Method       uint IConvertible.ToUInt32(System.IFormatProvider provider)
ToUInt64      Method       ulong IConvertible.ToUInt64(System.IFormatProvider provider)
TryFormat     Method       bool TryFormat(System.Span[char] destination, [ref] int charsWritten)
SideIndicator NoteProperty string SideIndicator===
```

**PassThru** を使用する場合は、元のオブジェクトの **型 (system.string**) が返されます。 System.string オブジェクトの既定の形式によって表示される出力で、 **Sideindicator** プロパティが表示されないことに注意して **ください。** ただし **、返される** system.string オブジェクトには、追加された **"メモ" プロパティ** があります。

### 例 4-プロパティを使用して2つの単純なオブジェクトを比較する

この例では、長さが同じ2つの異なる文字列を比較します。

```powershell
Compare-Object -ReferenceObject 'abc' -DifferenceObject 'xyz' -Property Length -IncludeEqual
```

```Output
Length SideIndicator
------ -------------
     3 ==
```

### 例 5-プロパティを使用して複合オブジェクトを比較する

この例は、複雑なオブジェクトを比較する場合の動作を示しています。 この例では、PowerShell の異なるインスタンスに対して2つの異なるプロセスオブジェクトを格納します。 両方の変数には、同じ名前のプロセスオブジェクトが含まれています。 **プロパティ** パラメーターを指定せずにオブジェクトを比較すると、コマンドレットはオブジェクトが等しいと見なされます。 **InputObject** の値は、 **ToString ()** メソッドの結果と同じであることに注意してください。 System.icomparable クラス **には** **IComparable** インターフェイスがないため、コマンドレットはオブジェクトを文字列に変換し、結果を比較します。

```powershell
PS> Get-Process pwsh

 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
    101   123.32     139.10      35.81   11168   1 pwsh
     89   107.55      66.97      11.44   17600   1 pwsh

PS> $a = Get-Process -Id 11168
PS> $b = Get-Process -Id 17600
PS> $a.ToString()
System.Diagnostics.Process (pwsh)
PS> $b.ToString()
System.Diagnostics.Process (pwsh)
PS> Compare-Object $a $b -IncludeEqual

InputObject                       SideIndicator
-----------                       -------------
System.Diagnostics.Process (pwsh) ==

PS> Compare-Object $a $b -Property ProcessName, Id, CPU

ProcessName    Id       CPU SideIndicator
-----------    --       --- -------------
pwsh        17600   11.4375 =>
pwsh        11168 36.203125 <=
```

比較するプロパティを指定すると、コマンドレットによって相違点が示されます。

### 例 6-IComparable を実装する複合オブジェクトを比較する

オブジェクトが **IComparable** を実装する場合、コマンドレットはオブジェクトを比較する方法を検索します。オブジェクトの型が異なる場合 **は、比較するオブジェクトが** **referenceobject** の型に変換されます。

この例では、文字列を **TimeSpan** オブジェクトと比較しています。 最初の例では、文字列は **TimeSpan** に変換されるため、オブジェクトは同じになります。

```powershell
Compare-Object ([TimeSpan]"0:0:1") "0:0:1" -IncludeEqual
```

```Output
InputObject SideIndicator
----------- -------------
00:00:01    ==
```

```powershell
Compare-Object "0:0:1" ([TimeSpan]"0:0:1")
```

```Output
InputObject SideIndicator
----------- -------------
00:00:01    =>
0:0:1       <=
```

2番目のケースでは、オブジェクトが異なるように、 **TimeSpan** が文字列に変換されます。

## パラメーター

### -CaseSensitive

比較においては、大文字と小文字が区別されることを示します。

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

### -Culture

比較に使用するカルチャを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -異なるオブジェクト

**参照** オブジェクトと比較するオブジェクトを指定します。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ExcludeDifferent

このコマンドレットによって、比較対象のオブジェクトの特性のみが表示されることを示します。 オブジェクト間の違いは破棄されます。

**参照** オブジェクトと **差分** オブジェクトの間で一致する行のみを表示するには、 **excludedifferent** と **includeequal** を使用します。

**Excludedifferent** が **includeequal** を指定せずに指定されている場合、出力はありません。

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

### -IncludeEqual

**Includeequal** **参照** オブジェクトと **差分** オブジェクトの一致を表示します。

既定では、 **参照** オブジェクトと **差分** オブジェクトの違いも出力に含まれます。

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

### -PassThru

**PassThru** パラメーターを使用する場合、では、 `Compare-Object` 比較対象のオブジェクトを中心に **pscustomobject** 省略し、異なるオブジェクトを変更せずに返します。

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

### -プロパティ

比較する **参照** オブジェクトと **差分** オブジェクトのプロパティの配列を指定します。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 式 `<string>` または `<script block>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReferenceObject

比較の参照として使用されるオブジェクトの配列を指定します。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SyncWindow

`Compare-Object`オブジェクトのコレクション内で一致するものを検索するときに、隣接するオブジェクトの数を指定します。 `Compare-Object` コレクション内の同じ位置にオブジェクトが見つからない場合に、隣接するオブジェクトを調べます。 既定値はです `[Int32]::MaxValue` 。これは、が `Compare-Object` オブジェクトコレクション全体を検査することを意味します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: [Int32]::MaxValue
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

オブジェクトをパイプライン **内で他のオブジェクトパラメーターに** 渡すことができます。

## 出力

### なし

**参照** オブジェクトと **差分** オブジェクトが同じ場合、 **includeequal** パラメーターを使用しない限り、出力はありません。

### システムの管理. PSCustomObject

オブジェクトが異なる場合、では、 `Compare-Object` ラッパー内の異なるオブジェクトをラップし `PSCustomObject` て、 **sideindicator** プロパティを使用して相違点を参照します。

**PassThru** パラメーターを使用する場合、オブジェクトの **型** は変更されませんが、返されたオブジェクトのインスタンスには、 **sideindicator** という名前の追加の **プロパティ** があります。 **Sideindicator** は、出力が属する入力オブジェクトを示します。

## メモ

**PassThru** パラメーターを使用する場合、コンソールに表示される出力には、 **sideindicator** プロパティが含まれないことがあります。 によって出力されるオブジェクト型のの既定の書式ビューに `Compare-Object` は、 **sideindicator** プロパティは含まれません。 詳細については、この記事の [例 3](#ex3) を参照してください。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Group-Object](Group-Object.md)

[Measure-Object](Measure-Object.md)

[New-Object](New-Object.md)

[Select-Object](Select-Object.md)

[Sort-Object](Sort-Object.md)

[Tee-Object](Tee-Object.md)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)

[Get-Process](../Microsoft.PowerShell.Management/Get-Process.md)
