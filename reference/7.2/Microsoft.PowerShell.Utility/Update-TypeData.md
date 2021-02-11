---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/update-typedata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-TypeData
ms.openlocfilehash: 1314d388bff5a46bcf335f3da7908a65233aa46e
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602404"
---
# Update-TypeData

## 構文
セッションで、拡張型データを更新します。

## 構文

### の場合 (既定)

```
Update-TypeData [[-AppendPath] <String[]>] [-PrependPath <String[]>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### DynamicTypeSet

```
Update-TypeData [-MemberType <PSMemberTypes>] [-MemberName <String>] [-Value <Object>]
 [-SecondValue <Object>] [-TypeConverter <Type>] [-TypeAdapter <Type>]
 [-SerializationMethod <String>] [-TargetTypeForDeserialization <Type>]
 [-SerializationDepth <Int32>] [-DefaultDisplayProperty <String>]
 [-InheritPropertySerializationSet <Nullable`1>] [-StringSerializationSource <String>]
 [-DefaultDisplayPropertySet <String[]>] [-DefaultKeyPropertySet <String[]>]
 [-PropertySerializationSet <String[]>] -TypeName <String> [-Force] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### TypeDataSet

```
Update-TypeData [-Force] [-TypeData] <TypeData[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## 説明

`Update-TypeData`コマンドレットは、ファイルをメモリに再度読み込んで `Types.ps1xml` 新しい拡張型データを追加することによって、セッションの拡張型データを更新します。

既定では、PowerShell は必要に応じて拡張型データを読み込みます。 パラメーターを指定しない場合は、 `Update-TypeData` 追加したすべての `Types.ps1xml` 種類のファイルを含め、セッションに読み込まれたすべてのファイルを再読み込みします。 のパラメーターを使用し `Update-TypeData` て、新しい型ファイルを追加したり、拡張型データを追加したり置換したりすることができます。

コマンドレットを使用すると、 `Update-TypeData` すべての型データを事前に読み込むことができます。 この機能は、型を開発し、テスト目的でそれらの新しい型を読み込むときに特に役立ちます。

Windows PowerShell 3.0 以降では、を使用し `Update-TypeData` て、ファイルを使用せずに、セッションの拡張型データを追加および置換できます `Types.ps1xml` 。 動的、つまりファイルを使用せずに追加される型データは、現在のセッションのみに追加されます。 すべてのセッションに型データを追加するには、 `Update-TypeData` PowerShell プロファイルにコマンドを追加します。 詳細については、「 [about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md)」を参照してください。

また、Windows PowerShell 3.0 以降では、コマンドレットを使用して、現在のセッションの拡張型を取得したり、コマンドレットを使用して `Get-TypeData` 現在の `Remove-TypeData` セッションから拡張型を削除したりできます。

プロパティで発生した例外、またはプロパティをコマンドに追加した場合 `Update-TypeData` 、エラーは報告されません。 これは、書式設定および出力の際に、多くの一般的な型で発生する例外を抑制します。 .NET のプロパティを取得する場合は、次の例に示すように、代わりにメソッド構文を使用して例外の抑制を回避できます。

`"hello".get_Length()`

メソッド構文は .NET プロパティでのみ使用できます。 コマンドレットの実行によって追加されたプロパティ `Update-TypeData` は、メソッド構文を使用できません。

PowerShell でのファイルの詳細につい `Types.ps1xml` ては、「 [about_Types.ps1xml](../Microsoft.PowerShell.Core/About/about_Types.ps1xml.md)」を参照してください。

## 例

### 例 1: 拡張型を更新する

```powershell
Update-TypeData
```

このコマンドは、 `Types.ps1xml` セッションで既に使用されているファイルから拡張型の構成を更新します。

### 例 2: 型を複数回更新する

この例では、同じセッションで型ファイル内の型を複数回更新する方法を示します。

最初のコマンドは、ファイルから拡張型の構成を更新して、 `Types.ps1xml` `TypesA.types.ps1xml` ファイルとファイルを最初に処理し `TypesB.types.ps1xml` ます。

2番目のコマンドは、 `TypesA.types.ps1xml` ファイルの型を追加または変更した場合など、をもう一度更新する方法を示しています。 ファイルに対して前のコマンドを繰り返すか、 `TypesA.types.ps1xml` `Update-TypeData` パラメーターを指定せずにコマンドを実行することができ `TypesA.types.ps1xml` ます。これは、が既に現在のセッションの型ファイルリストに存在するためです。

```powershell
Update-TypeData -PrependPath TypesA.types.ps1xml, TypesB.types.ps1xml
Update-TypeData -PrependPath TypesA.types.ps1xml
```

### 例 3: DateTime オブジェクトにスクリプトプロパティを追加する

この例では、を使用して、現在のセッションの system.string `Update-TypeData` オブジェクト (コマンドレットによって返されるオブジェクトなど) に **Quarter** スクリプトプロパティ **を** 追加し `Get-Date` ます。

```powershell
Update-TypeData -TypeName "System.DateTime" -MemberType ScriptProperty -MemberName "Quarter" -Value {
  if ($this.Month -in @(1,2,3)) {"Q1"}
  elseif ($this.Month -in @(4,5,6)) {"Q2"}
  elseif ($this.Month -in @(7,8,9)) {"Q3"}
  else {"Q4"}
}
(Get-Date).Quarter
```

```Output
Q1
```

この `Update-TypeData` コマンドは、 **TypeName** パラメーターを使用 **して system.string** 型を指定し **、MemberName** パラメーターを使用して新しいプロパティの名前を指定し、 **MemberType** プロパティを使用して **scriptproperty** 型を指定し、 **Value** パラメーターを使用して年間四半期を決定するスクリプトを指定します。

**Value** プロパティの値は、現在の年間の四半期を計算するスクリプトです。 スクリプトブロックは、 `$this` オブジェクトの現在のインスタンスを表す自動変数と In 演算子を使用して、月の値が各整数配列に表示されるかどうかを判断します。 オペレーターの詳細については `-in` 、「 [about_Comparison_Operators](../Microsoft.PowerShell.Core/about/about_Comparison_Operators.md)」を参照してください。

2 番目のコマンドは、現在の日付の新しい Quarter プロパティを取得します。

### 例 4: 既定でリスト内に表示される型を更新する

この例では、プロパティが指定されていない場合に、既定でリスト内に表示される型のプロパティを設定する方法を示します。 型データはファイルで指定されていないため `Types.ps1xml` 、現在のセッションでのみ有効です。

```powershell
Update-TypeData -TypeName "System.DateTime" -DefaultDisplayPropertySet "DateTime, DayOfYear, Quarter"
Get-Date | Format-List
```

```Output
Thursday, March 15, 2012 12:00:00 AM
DayOfYear : 75
Quarter   : Q1
```

最初のコマンドは、コマンドレットを使用して、system.string `Update-TypeData` 型の既定のリストプロパティを設定します。 このコマンドは、**TypeName** パラメーターを使用して型を指定し、**DefaultDisplayPropertySet** パラメーターを使用して一覧の既定のプロパティを指定します。 選択したプロパティには、前の例で追加した新しい **Quarter** スクリプトプロパティが含まれています。

2番目のコマンドは、コマンドレットを使用して、 `Get-Date` 現在の日付を表す system.string オブジェクトを取得します **。** このコマンドは、パイプライン演算子 () を使用して、 `|` **DateTime** オブジェクトをコマンドレットに送信し `Format-List` ます。 コマンドでは、 `Format-List` 一覧に表示するプロパティが指定されていないため、PowerShell はコマンドによって設定された既定値を使用し `Update-TypeData` ます。

### 例 5: パイプを使用したオブジェクトの型データを更新する

```powershell
Get-Module | Update-TypeData -MemberType ScriptProperty -MemberName "SupportsUpdatableHelp" -Value {
  if ($this.HelpInfoUri) {$True} else {$False}
}
Get-Module -ListAvailable | Format-Table Name, SupportsUpdatableHelp
```

```Output
Name                             SupportsUpdatableHelp
----                             ---------------------
Microsoft.PowerShell.Diagnostics                  True
Microsoft.PowerShell.Host                         True
Microsoft.PowerShell.Management                   True
Microsoft.PowerShell.Security                     True
Microsoft.PowerShell.Utility                      True
Microsoft.WSMan.Management                        True
PSDiagnostics                                    False
PSScheduledJob                                    True
PSWorkflow                                        True
ServerManager                                     True
TroubleshootingPack                              False
```

この例では、オブジェクトをにパイプするときに `Update-TypeData` 、 `Update-TypeData` オブジェクト型の拡張型データを追加する方法を示します。

この方法は、 `Get-Member` コマンドレットまたはメソッドを使用して `Get-Type` オブジェクトの型を取得するよりも高速です。 ただし、オブジェクトのコレクションをにパイプする場合は `Update-TypeData` 、最初のオブジェクト型の型データを更新してから、コレクション内の他のすべてのオブジェクトに対してエラーを返します。これは、メンバーが型に対して既に定義されているためです。

最初のコマンドは、 `Get-Module` コマンドレットを使用して PSScheduledJob モジュールを取得します。 このコマンドは、モジュールオブジェクトをコマンドレットにパイプします。このコマンドレットは、 `Update-TypeData`  `Get-Module` コマンドで **ListAvailable** パラメーターを使用したときに返される Moduleinfogrouping 型など、PSModuleInfo 型とそれから派生した型の型データを更新します。

これらのコマンドは、インポートされた `Update-TypeData` すべてのモジュールに **Supportsupthe help** script プロパティを追加します。 **Value** パラメーターの値は、 `$True` モジュールの **helpinfouri** プロパティが設定されている場合はを返し、それ以外の場合はを返すスクリプトです `$False` 。

2番目のコマンドは、モジュールオブジェクトをから `Get-Module` コマンドレットにパイプします。このコマンドレットは、 `Format-Table` リスト内のすべてのモジュールの **Name** プロパティと **supportsuphelp** プロパティを表示します。

## パラメーター

### -AppendPath

省略可能なファイルへのパスを指定し `.ps1xml` ます。 指定されたファイルは、組み込みファイルが読み込まれた後にリストされる順序で、読み込まれます。 また、 **Appendpath** 値をにパイプすることもでき `Update-TypeData` ます。

```yaml
Type: System.String[]
Parameter Sets: FileSet
Aliases: PSPath, Path

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -DefaultDisplayProperty

他のプロパティが指定されていない場合に、コマンドレットによって表示される型のプロパティを指定し `Format-Wide` ます。

型の標準および拡張プロパティの名前を入力します。 このパラメーターの値には、同じコマンドに追加される型の名前を指定できます。

この値は、ファイル内の型に対して定義されているワイドビューがない場合にのみ有効です `Format.ps1xml` 。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DefaultDisplayPropertySet

型の 1 つまたは複数のプロパティを指定します。 これらのプロパティは、他のプロパティが指定されていない場合に、コマンドレットによって表示され `Format-List` ます。

型の標準および拡張プロパティの名前を入力します。 このパラメーターの値には、同じコマンドに追加される型の名前を指定できます。

この値は、ファイル内の型に対して定義されているリストビューがない場合にのみ有効です `Format.ps1xml` 。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DefaultKeyPropertySet

型の 1 つまたは複数のプロパティを指定します。 これらのプロパティは、 `Group-Object` `Sort-Object` 他のプロパティが指定されていない場合に、コマンドレットとコマンドレットで使用されます。

型の標準および拡張プロパティの名前を入力します。 このパラメーターの値には、同じコマンドに追加される型の名前を指定できます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

型データがその型に対して既に指定されている場合でも、コマンドレットで指定された型データを使用することを示します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: DynamicTypeSet, TypeDataSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InheritPropertySerializationSet

シリアル化されたプロパティのセットが継承されているかどうかを示します。 既定値は `$Null` です。 このパラメーターの有効値は、次のとおりです。

- `$True`. プロパティ セットは継承されます。
- `$False`. プロパティ セットは継承されません。
- `$Null`. 継承は定義されていません。

このパラメーターは、 **Serializationmethod** パラメーターの値がの場合にのみ有効です `SpecificProperties` 。 このパラメーターの値がの場合は `$False` 、 **Propertyserializationset** パラメーターが必要です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Nullable`1[System.Boolean]
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MemberName

プロパティまたはメソッドの名前を指定します。

このパラメーターを **TypeName**、**MemberType**、**Value**、および **SecondValue** パラメーターと共に使用して、型のプロパティまたはメソッドを追加または変更できます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MemberType

追加または変更するメンバーの型を指定します。

このパラメーターを **TypeName**、**MemberType**、**Value**、および **SecondValue** パラメーターと共に使用して、型のプロパティまたはメソッドを追加または変更できます。 このパラメーターの有効値は、次のとおりです。

- AliasProperty
- CodeMethod
- CodeProperty
- Noteproperty
- ScriptMethod
- ScriptProperty

これらの値の詳細については、「 [PSMemberTypes Enumeration](/dotnet/api/system.management.automation.psmembertypes)」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSMemberTypes
Parameter Sets: DynamicTypeSet
Aliases:
Accepted values: NoteProperty, AliasProperty, ScriptProperty, CodeProperty, ScriptMethod, CodeMethod

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PrependPath

オプションファイルへのパスを指定し `.ps1xml` ます。 指定されたファイルは、組み込みファイルが読み込まれる前にリストされる順序で、読み込まれます。

```yaml
Type: System.String[]
Parameter Sets: FileSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PropertySerializationSet

シリアル化するプロパティの名前を指定します。 このパラメーターは、**SerializationMethod** パラメーターの値が **SpecificProperties** の場合に使用します。

```yaml
Type: System.String[]
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecondValue

**AliasProperty**、**ScriptProperty**、**CodeProperty**、または **CodeMethod** メンバーの追加値を指定します。

型のプロパティまたはメソッドを追加または変更するには、このパラメーターを **TypeName**、 **MemberType**、 **Value**、および **SecondValue** パラメーターと共に使用します。

**MemberType** パラメーターの値がの場合 `AliasProperty` 、 **SecondValue** パラメーターの値はデータ型である必要があります。 PowerShell は、alias プロパティの値を指定された型に変換します (つまり、キャストします)。 たとえば、文字列プロパティの代替名を提供するエイリアスプロパティを追加する場合、 **SecondValue** の値を指定して、エイリアス化された文字列値を整数に変換することも **できます。**

**MemberType** パラメーターの値がの場合は `ScriptProperty` 、 **SecondValue** パラメーターを使用して、追加のスクリプトブロックを指定できます。 **Value** パラメーターの値内のスクリプト ブロックは、変数の値を取得します。 **SecondValue** パラメーターの値内のスクリプト ブロックは、変数の値を取得します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Object
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SerializationDepth

文字列としてシリアル化する型オブジェクトのレベル数を指定します。 既定値は、 `1` オブジェクトとそのプロパティをシリアル化します。 値は、 `0` オブジェクトをシリアル化しますが、そのプロパティはシリアル化しません。 値は、 `2` オブジェクト、そのプロパティ、およびプロパティ値内のオブジェクトをシリアル化します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: 1
Accept pipeline input: False
Accept wildcard characters: False
```

### -SerializationMethod

型のシリアル化メソッドを指定します。 シリアル化メソッドは、シリアル化される型のプロパティとそれらのシリアル化に使用する方法を決定します。 このパラメーターの有効値は、次のとおりです。

- `AllPublicProperties`. 型のすべてのパブリック プロパティをシリアル化します。 **SerializationDepth** パラメーターを使用して、子プロパティをシリアル化するかどうかを決定できます。
- `String`. 文字列として、型をシリアル化します。 **StringSerializationSource** を使用して、シリアル化の結果として使用する型のプロパティを指定します。 指定しない場合、型はオブジェクトの **ToString** メソッドを使用してシリアル化されます。
- `SpecificProperties`. この型の指定されたプロパティのみをシリアル化します。 **PropertySerializationSet** パラメーターを使用して、シリアル化される型のプロパティを指定します。
  また、**InheritPropertySerializationSet** パラメーターを使用してプロパティ セットを継承するかどうかを決定することも、**SerializationDepth** パラメーターを使用して子プロパティをシリアル化するかどうかを決定することもできます。

PowerShell では、シリアル化メソッドは **Psstandardmembers** 内部オブジェクトに格納されます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -StringSerializationSource

型のプロパティの名前を指定します。 指定されたプロパティの値は、シリアル化の結果として使用されます。 このパラメーターは、 **Serializationmethod** パラメーターの値が String の場合にのみ有効です。

```yaml
Type: System.String
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TargetTypeForDeserialization シリアル化

この型のオブジェクトが、逆シリアル化時に変換される型を指定します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Type
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeAdapter

型アダプターの種類 ( **microsoft.powershell.cim.ciminstanceadapter など** など) を指定します。 型アダプターを使用すると、PowerShell は型のメンバーを取得できます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Type
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeConverter

さまざまな型の間で値を変換する型コンバーターを指定します。 型の型コンバーターが定義されている場合、型コンバーターのインスタンスが変換に使用されます。

**System.Type** 値 (**System.ComponentModel.TypeConverter** または **System.Management.Automation.PSTypeConverter** クラスから派生したもの) を入力します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Type
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeData

このコマンドレットによってセッションに追加されるデータ型の配列を指定します。 **Typedata** オブジェクトを含む変数、またはコマンドなどの **typedata** オブジェクトを取得するコマンドを入力し `Get-TypeData` ます。 パイプを使用して **Typedata** オブジェクトをにパイプすることもでき `Update-TypeData` ます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.Runspaces.TypeData[]
Parameter Sets: TypeDataSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -TypeName

拡張する型の名前を指定します。

**System** 名前空間内の型の場合は、短い名前を入力してください。 それ以外の場合は、完全な型名が必要です。 ワイルドカードはサポートされていません。

パイプを使用して型名をにすることができ `Update-TypeData` ます。 パイプを使用してオブジェクトをにパイプすると `Update-TypeData` 、オブジェクト `Update-TypeData` の型名と型のデータがオブジェクト型に取得されます。

このパラメーターを **MemberName**、 **MemberType**、 **Value** 、 **SecondValue** の各パラメーターと共に使用して、型のプロパティまたはメソッドを追加または変更します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DynamicTypeSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Value

プロパティまたはメソッドの値を指定します。

、、、またはのメンバーを追加する場合は、 `AliasProperty` `CodeProperty` `ScriptProperty` `CodeMethod` **SecondValue** パラメーターを使用して追加情報を追加できます。

このパラメーターを **MemberName**、 **MemberType**、 **Value** 、 **SecondValue** の各パラメーターと共に使用して、型のプロパティまたはメソッドを追加または変更します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Object
Parameter Sets: DynamicTypeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、 **Appendpath**、 **TypeName**、または **typedata** パラメーターの値を含む文字列をに渡すことができ `Update-TypeData` ます。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## メモ

## 関連リンク

[about_Types.ps1xml](../Microsoft.PowerShell.Core/About/about_Types.ps1xml.md)

[Get-TypeData](Get-TypeData.md)

[Remove-TypeData](Remove-TypeData.md)

