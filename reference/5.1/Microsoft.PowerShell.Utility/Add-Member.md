---
external help file: Microsoft.PowerShell.Commands.Utility.dll-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 4/26/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/add-member?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Add-Member
ms.openlocfilehash: 4d251a558d1623e2e0573812921f0e1f273356cf
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388146"
---
# Add-Member

## 概要
PowerShell オブジェクトのインスタンスにカスタムプロパティとメソッドを追加します。

## SYNTAX

### TypeNameSet (既定値)

```
Add-Member -InputObject <PSObject> -TypeName <String> [-PassThru] [<CommonParameters>]
```

### 注 Propertymultiメンバセット

```
Add-Member -InputObject <PSObject> [-TypeName <String>] [-Force] [-PassThru]
 [-NotePropertyMembers] <IDictionary> [<CommonParameters>]
```

### NotePropertySingleMemberSet

```
Add-Member -InputObject <PSObject> [-TypeName <String>] [-Force] [-PassThru] [-NotePropertyName] <String>
 [-NotePropertyValue] <Object> [<CommonParameters>]
```

### セット

```
Add-Member -InputObject <PSObject> [-MemberType] <PSMemberTypes> [-Name] <String> [[-Value] <Object>]
 [[-SecondValue] <Object>] [-TypeName <String>] [-Force] [-PassThru] [<CommonParameters>]
```

## Description

`Add-Member`コマンドレットを使用すると、PowerShell オブジェクトのインスタンスにメンバー (プロパティとメソッド) を追加できます。 たとえば、オブジェクトの説明が含まれている、またはオブジェクトを変更するスクリプトを実行する **Scriptmethod** メンバーを追加することができます。

を使用するに `Add-Member` は、オブジェクトをにパイプ処理するか、 `Add-Member` **InputObject** パラメーターを使用してオブジェクトを指定します。

**MemberType** パラメーターは、追加するメンバーの種類を示します。 **Name** パラメーターを指定すると、新しいメンバーに名前が割り当てられ、 **value** パラメーターによってメンバーの値が設定されます。

追加するプロパティとメソッドは、指定したオブジェクトの特定のインスタンスにのみ追加されます。 `Add-Member` では、オブジェクトの種類は変更されません。 新しいオブジェクトの種類を作成するには、コマンドレットを使用し `Add-Type` ます。

また、コマンドレットを使用して、 `Export-Clixml` 追加のメンバーを含むオブジェクトのインスタンスをファイルに保存することもできます。 次に、コマンドレットを使用して、エクスポートされた `Import-Clixml` ファイルに格納されている情報からオブジェクトのインスタンスを再作成します。

Windows PowerShell 3.0 以降で `Add-Member` は、オブジェクトへのノートプロパティの追加を容易にする新機能が追加されています。
**NotePropertyName** パラメーターと **NotePropertyValue** パラメーターを使用してノート プロパティを定義することも、ノート プロパティの名前と値のハッシュ テーブルを受け取る **NotePropertyMembers** パラメーターを使用することもできます。

また、Windows PowerShell 3.0 以降では、出力オブジェクトを生成する **PassThru** パラメーターを使用する必要性が減っています。 `Add-Member` では、新しいメンバーをさらに多くの型の入力オブジェクトに直接追加します。 詳細については、 **PassThru** パラメーターの説明を参照してください。

## 例

### 例 1: PSObject に note プロパティを追加する

次の例では、ファイルを表す **FileInfo** オブジェクトに "Done" という値を持つ **Status** note プロパティを追加し `Test.txt` ます。

最初のコマンドは、 `Get-ChildItem` コマンドレットを使用して、ファイルを表す **FileInfo** オブジェクトを取得し `Test.txt` ます。 変数に保存 `$a` します。

2番目のコマンドは、のオブジェクトに note プロパティを追加し `$a` ます。

3番目のコマンドは、ドット表記を使用して、内のオブジェクトの **Status** プロパティの値を取得し `$a` ます。 出力に示されているように、値は "Done" です。

```powershell
$A = Get-ChildItem c:\ps-test\test.txt
$A | Add-Member -NotePropertyName Status -NotePropertyValue Done
$A.Status
```

```Output
Done
```

### 例 2: PSObject にエイリアスプロパティを追加する

次の例では、ファイルを表すオブジェクトに **Size** エイリアスプロパティを追加し `Test.txt` ます。 新しいプロパティは、 **Length** プロパティのエイリアスです。

最初のコマンドは、 `Get-ChildItem` コマンドレットを使用して、 `Test.txt` **FileInfo** オブジェクトを取得します。

2番目のコマンドは、 **Size** エイリアスプロパティを追加します。
3番目のコマンドは、ドット表記を使用して、新しい **Size** プロパティの値を取得します。

```powershell
$A = Get-ChildItem C:\Temp\test.txt
$A | Add-Member -MemberType AliasProperty -Name Size -Value Length
$A.Size
```

```Output
2394
```

### 例 3: 文字列に StringUse note プロパティを追加する

この例では、 **Stringuse** note プロパティを文字列に追加します。
`Add-Member`は、 **文字列** 入力オブジェクトに型を追加できないため、 **PassThru** パラメーターを指定して出力オブジェクトを生成することができます。 例の最後のコマンドは、新しいプロパティを表示します。

この例では、 **注 Propertymembers** パラメーターを使用します。 **NotePropertyMembers** パラメーターの値はハッシュ テーブルです。 キーはノートプロパティの名前である **Stringuse** で、値はノートプロパティの値である **Display** です。

```powershell
$A = "A string"
$A = $A | Add-Member -NotePropertyMembers @{StringUse="Display"} -PassThru
$A.StringUse
```

```Output
Display
```

### 例 4: FileInfo オブジェクトにスクリプトメソッドを追加する

この例では、ファイルサイズを最も近いメガバイトに計算する **FileInfo** オブジェクトに **sizeinmb** スクリプトメソッドを追加します。 2番目のコマンド **ScriptBlock** は、型の **round** static メソッドを使用して、 `[math]` ファイルサイズを2番目の小数点以下桁数に丸める ScriptBlock を作成します。

**値** パラメーターは、現在の `$This` オブジェクトを表す自動変数も使用します。 `$This`変数は、新しいプロパティとメソッドを定義するスクリプトブロックでのみ有効です。

最後のコマンドは、ドット表記を使用して、変数内のオブジェクトに対して新しい **Sizeinmb** スクリプトメソッドを呼び出し `$A` ます。

```powershell
$A = Get-ChildItem C:\Temp\test.txt
$S = {[math]::Round(($this.Length / 1MB), 2)}
$A | Add-Member -MemberType ScriptMethod -Name "SizeInMB" -Value $S
$A.SizeInMB()
```

```Output
0.43
```

### 例 5: オブジェクトのすべてのプロパティを別のオブジェクトにコピーする

この関数は、1 つのオブジェクトのすべてのプロパティを別のオブジェクトにコピーします。

このループでは、 `foreach` コマンドレットを使用して `Get-Member` 、 **From** オブジェクトの各プロパティを取得します。 ループ内のコマンド `foreach` は、各プロパティの系列で実行されます。

この `Add-Member` コマンドは、 **From** オブジェクトのプロパティを、 **lastproperty** として **to** オブジェクトに追加します。 値は、 **値** パラメーターを使用してコピーされます。 **Force** パラメーターを使用して、同じメンバー名を持つメンバーを追加します。

```powershell
function Copy-Property ($From, $To)
{
    $properties = Get-Member -InputObject $From -MemberType Property
    foreach ($p in $properties)
    {
        $To | Add-Member -MemberType NoteProperty -Name $p.Name -Value $From.$($p.Name) -Force
    }
}
```

### 例 6: カスタムオブジェクトを作成する

この例では、 **Asset** カスタムオブジェクトを作成します。

`New-Object`コマンドレットでは、 **PSObject** を作成します。 この例では、 **PSObject** を変数に保存し `$Asset` ます。

2番目のコマンドは、型アクセラレータを使用して、 `[ordered]` 名前と値の順序付けされた辞書を作成します。 このコマンドは、変数に結果を保存し `$D` ます。

3番目のコマンドは、コマンドレットの "dictionary **Propertymembers** " パラメーターを使用して、 `Add-Member` 変数内の辞書 `$D` を **PSObject** に追加します。
**TypeName** プロパティは、新しい名前である **Asset** を **PSObject** に割り当てます。

最後のコマンドは、新しい **Asset** オブジェクトをコマンドレットにパイプし `Get-Member` ます。 出力には、オブジェクトに **Asset** という型名と、順序付けされた辞書で定義した note プロパティがあることが示されています。

```powershell
$Asset = New-Object -TypeName PSObject
$d = [ordered]@{Name="Server30";System="Server Core";PSVersion="4.0"}
$Asset | Add-Member -NotePropertyMembers $d -TypeName Asset
$Asset | Get-Member
```

```Output
   TypeName: Asset

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
Name        NoteProperty System.String Name=Server30
PSVersion   NoteProperty System.String PSVersion=4.0
System      NoteProperty System.String System=Server Core
```

## PARAMETERS

### -Force

オブジェクトに同じ名前のカスタムメンバーがある場合でも、このコマンドレットによって新しいメンバーが追加されることを示します。
**Force** パラメーターを使用して、型の標準メンバーを置き換えることはできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: MemberSet, NotePropertySingleMemberSet, NotePropertyMultiMemberSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

新しいメンバーを追加するオブジェクトを指定します。
オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -MemberType

追加するメンバーの型を指定します。
このパラメーターは必須です。
このパラメーターの有効値は、次のとおりです。

- NoteProperty
- AliasProperty
- ScriptProperty
- CodeProperty
- ScriptMethod
- CodeMethod

これらの値の詳細については、「 [PSMemberTypes Enumeration](/dotnet/api/system.management.automation.psmembertypes) In THE PowerShell SDK」を参照してください。

すべてのオブジェクトにすべての型のメンバーがあるわけではありません。
オブジェクトに含まれていないメンバーの種類を指定すると、PowerShell はエラーを返します。

```yaml
Type: System.Management.Automation.PSMemberTypes
Parameter Sets: MemberSet
Aliases: Type
Accepted values: AliasProperty, CodeProperty, Property, NoteProperty, ScriptProperty, Properties, PropertySet, Method, CodeMethod, ScriptMethod, Methods, ParameterizedProperty, MemberSet, Event, Dynamic, All

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

このコマンドレットによって追加されるメンバーの名前を指定します。

```yaml
Type: System.String
Parameter Sets: MemberSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -注 Propertymembers

ノート プロパティの名前と値のハッシュ テーブルまたは順序付けされた辞書を指定します。
ノート プロパティの名前をキーとして持ち、ノート プロパティの値を値として持つハッシュ テーブルまたは辞書を指定します。

PowerShell のハッシュテーブルと順序付けされたディクショナリの詳細については、「 [about_Hash_Tables](../Microsoft.PowerShell.Core/About/about_Hash_Tables.md)」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Collections.IDictionary
Parameter Sets: NotePropertyMultiMemberSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -注 Propertyname

ノートプロパティの名前を指定します。

このパラメーターは、 **NotePropertyValue** パラメーターと一緒に使用します。
このパラメーターは省略可能です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: NotePropertySingleMemberSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -注 Propertyvalue

メモのプロパティ値を指定します。

このパラメーターは、 **メモの propertyname** パラメーターと共に使用します。
このパラメーターは省略可能です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Object
Parameter Sets: NotePropertySingleMemberSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。
既定では、このコマンドレットによる出力はありません。

ほとんどのオブジェクトでは、 `Add-Member` 入力オブジェクトに新しいメンバーを追加します。
ただし、入力オブジェクトが文字列の場合、は `Add-Member` そのメンバーを入力オブジェクトに追加できません。
これらのオブジェクトに対しては、 **PassThru** パラメーターを使用して、出力オブジェクトを作成します。

Windows PowerShell 2.0 では、オブジェクトでは `Add-Member` なく、オブジェクトの **PSObject** ラッパーにのみメンバーが追加されました。
**PassThru** パラメーターを使用して、 **PSObject** ラッパーを持つ任意のオブジェクトの出力オブジェクトを作成します。

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

### -SecondValue

**AliasProperty** 、 **ScriptProperty** 、 **CodeProperty** 、または **CodeMethod** の各メンバーに関する省略可能な追加情報を指定します。

**エイリアスプロパティ** を追加するときに使用する場合、このパラメーターはデータ型である必要があります。
指定されたデータ型への変換は、 **エイリアスプロパティ** の値に追加されます。

たとえば、文字列プロパティの代替名を提供する **エイリアスプロパティ** を追加する場合、System.string の **SecondValue** パラメーターを指定して、対応する **エイリアスプロパティ** を使用してアクセスしたときに、その文字列プロパティの値を整数に変換する必要があることを示すことも **できます。**

**Scriptproperty** メンバーを追加するときに、 **SecondValue** パラメーターを使用して追加の **ScriptBlock** を指定できます。 **Value** パラメーターで指定された最初の **ScriptBlock** は、変数の値を取得するために使用されます。 **SecondValue** パラメーターで指定した2つ目の **ScriptBlock** は、変数の値を設定するために使用されます。

```yaml
Type: System.Object
Parameter Sets: MemberSet
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Value

追加されたメンバーの初期値を指定します。
**エイリアスプロパティ** 、 **codeproperty** 、 **Scriptproperty** 、または **codeproperty** メンバーを追加する場合は、 **SecondValue** パラメーターを使用して、省略可能な追加情報を指定できます。

```yaml
Type: System.Object
Parameter Sets: MemberSet
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeName

型の名前を指定します。

型が **System** 名前空間のクラス、または型アクセラレータを持つ型である場合は、型の短い名前を入力できます。 それ以外の場合は、完全な型名が必要です。
このパラメーターは、 **InputObject** が **PSObject** の場合にのみ有効です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: TypeNameSet, NotePropertyMultiMemberSet, NotePropertySingleMemberSet, MemberSet
Aliases:

Required: True (TypeNameSet), False (NotePropertyMultiMemberSet, NotePropertySingleMemberSet, MemberSet)
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して、任意のオブジェクトの種類をこのコマンドレットに渡します。

## 出力

### None または System.object

**PassThru** パラメーターを使用すると、このコマンドレットは、新しく拡張されたオブジェクトを返します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

メンバーを追加できるのは、 **PSObject** オブジェクトだけです。 オブジェクトが **PSObject** オブジェクトであるかどうかを判断するには、演算子を使用し `-is` ます。

たとえば、変数に格納されているオブジェクトをテストするには `$obj` 、「」と入力 `$obj -is [PSObject]` します。

**MemberType** 、 **Name** 、 **Value** 、 **SecondValue** の各パラメーターの名前は省略可能です。
パラメーター名を省略した場合、名前のないパラメーター値は、 **MemberType** 、 **Name** 、 **Value** 、 **SecondValue** の順に表示される必要があります。

パラメーター名を指定する場合は、パラメーターの順序に決まりはありません。

`$this`新しいプロパティとメソッドの値を定義するスクリプトブロックでは、自動変数を使用できます。
変数は、 `$this` プロパティとメソッドを追加するオブジェクトのインスタンスを参照します。 変数の詳細については `$this` 、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

## 関連リンク

[Export-Clixml](Export-Clixml.md)

[Get-Member](Get-Member.md)

[Import-Clixml](Import-Clixml.md)

[New-Object](New-Object.md)

[about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)
