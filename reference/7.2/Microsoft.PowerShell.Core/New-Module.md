---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-module?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Module
ms.openlocfilehash: 487fd85bdcc918b7fb360f9c9d23388a06b35f86
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601027"
---
# New-Module

## 概要
メモリ内にのみ存在する新しい動的モジュールを作成します。

## SYNTAX

### ScriptBlock (既定値)

```
New-Module [-ScriptBlock] <ScriptBlock> [-Function <String[]>] [-Cmdlet <String[]>] [-ReturnResult]
 [-AsCustomObject] [-ArgumentList <Object[]>] [<CommonParameters>]
```

### 名前

```
New-Module [-Name] <String> [-ScriptBlock] <ScriptBlock> [-Function <String[]>] [-Cmdlet <String[]>]
 [-ReturnResult] [-AsCustomObject] [-ArgumentList <Object[]>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `New-Module` スクリプトブロックから動的モジュールを作成します。 関数や変数などの動的モジュールのメンバーは、セッションで直ちに利用可能になり、セッションを終了するまで使用できます。

静的モジュール同様、既定では動的モジュール内のコマンドレットと関数がエクスポートされ、変数およびエイリアスはありません。 ただし、Export-ModuleMember コマンドレットとのパラメーターを使用して、 `New-Module` 既定値をオーバーライドできます。

また、の **Ascustomobject** 出力パラメーターを使用し `New-Module` て、動的モジュールをカスタムオブジェクトとして返すこともできます。 関数などモジュールのメンバーは、セッションにインポートされる代わりに、カスタム オブジェクトのスクリプト メソッドとして実装されます。

動的モジュールはメモリにのみ存在し、ディスク上には存在しません。 すべてのモジュール同様、動的モジュールのメンバーはグローバル スコープの子であるプライベート モジュール スコープで実行されます。 Get-Module は動的モジュールを取得できませんが、Get-Command はエクスポートされたメンバーを取得できます。

動的モジュールをで使用できるようにするには `Get-Module` 、コマンドをパイプで `New-Module` インポートするか、を返すモジュールオブジェクトをパイプし `New-Module` `Import-Module` ます。 この操作により、動的モジュールが `Get-Module` 一覧に追加されますが、モジュールをディスクに保存したり、永続化したりすることはありません。

## 例

### 例 1: 動的モジュールを作成する

この例では、という関数を使用して、新しい動的モジュールを作成し `Hello` ます。 このコマンドは、新しい動的モジュールを表すモジュール オブジェクトを返します。

```powershell
New-Module -ScriptBlock {function Hello {"Hello!"}}
```

```Output
Name              : __DynamicModule_2ceb1d0a-990f-45e4-9fe4-89f0f6ead0e5
Path              : 2ceb1d0a-990f-45e4-9fe4-89f0f6ead0e5
Description       :
Guid              : 00000000-0000-0000-0000-000000000000
Version           : 0.0
ModuleBase        :
ModuleType        : Script
PrivateData       :
AccessMode        : ReadWrite
ExportedAliases   : {}
ExportedCmdlets   : {}
ExportedFunctions : {[Hello, Hello]}
ExportedVariables : {}
NestedModules     : {}
```

### 例 2: 動的モジュールと Get-Module と Get-Command の使用

この例では、コマンドレットによって動的モジュールが返されないことを示し `Get-Module` ます。 エクスポートされたメンバーは、コマンドレットによって返され `Get-Command` ます。

```powershell
new-module -scriptblock {function Hello {"Hello!"}}
```

```Output
Name              : __DynamicModule_2ceb1d0a-990f-45e4-9fe4-89f0f6ead0e5
Path              : 2ceb1d0a-990f-45e4-9fe4-89f0f6ead0e5
Description       :
Guid              : 00000000-0000-0000-0000-000000000000
Version           : 0.0
ModuleBase        :
ModuleType        : Script
PrivateData       :
AccessMode        : ReadWrite
ExportedAliases   : {}
ExportedCmdlets   : {}
ExportedFunctions : {[Hello, Hello]}
ExportedVariables : {}
NestedModules     : {}
```

```powershell
Get-Module

Get-Command Hello
```

```Output
CommandType     Name   Definition
-----------     ----   ----------
Function        Hello  "Hello!"
```

### 例 3: 現在のセッションに変数をエクスポートする

この例では、コマンドレットを使用して、 `Export-ModuleMember` 変数を現在のセッションにエクスポートします。
コマンドを使用しない `Export-ModuleMember` 場合、関数のみがエクスポートされます。

```powershell
New-Module -ScriptBlock {$SayHelloHelp="Type 'SayHello', a space, and a name."; function SayHello ($name) { "Hello, $name" }; Export-ModuleMember -function SayHello -Variable SayHelloHelp}
$SayHelloHelp
```

```Output
Type 'SayHello', a space, and a name.
```

```powershell
SayHello Jeffrey
```

```Output
Hello, Jeffrey
```

出力には、変数と関数が両方ともセッションにエクスポートされたことが示されます。

### 例 4: Get-Module で動的モジュールを使用できるようにする

この例では、動的モジュールをにパイプ処理することによって、で動的モジュールを使用できるようにする方法を示し `Get-Module` `Import-Module` ます。

`New-Module` コマンドレットにパイプ処理されたモジュールオブジェクトを作成し `Import-Module` ます。 の **name** パラメーターは、 `New-Module` モジュールにフレンドリ名を割り当てます。 `Import-Module`は、既定ではオブジェクトを返さないため、このコマンドからの出力はありません。 `Get-Module`**GreetingModule** が現在のセッションにインポートされていること。

```powershell
New-Module -ScriptBlock {function Hello {"Hello!"}} -name GreetingModule | Import-Module
Get-Module
```

```Output
Name              : GreetingModule
Path              : d54dfdac-4531-4db2-9dec-0b4b9c57a1e5
Description       :
Guid              : 00000000-0000-0000-0000-000000000000
Version           : 0.0
ModuleBase        :
ModuleType        : Script
PrivateData       :
AccessMode        : ReadWrite
ExportedAliases   : {}
ExportedCmdlets   : {}
ExportedFunctions : {[Hello, Hello]}
ExportedVariables : {}
NestedModules     : {}
```

```powershell
Get-Command hello
```

```Output
CommandType     Name                                                               Definition
-----------     ----                                                               ----------
Function        Hello                                                              "Hello!"
```

コマンドレットは、動的モジュールによって `Get-Command` エクスポートされる関数を示し `Hello` ます。

### 例 5: エクスポートされた関数を持つカスタムオブジェクトを生成する

この例では、の **Ascustomobject** パラメーターを使用して、エクスポートされた `New-Module` 関数を表すスクリプトメソッドを含むカスタムオブジェクトを生成する方法を示します。

`New-Module`コマンドレットでは、とという2つの関数を持つ動的モジュールを作成し `Hello` `Goodbye` ます。 **Ascustomobject** パラメーターは、既定で生成されるオブジェクトではなく、カスタムオブジェクトを作成し `New-Module` ます。 このカスタムオブジェクトは変数に保存され `$m` ます。
`$m`変数に値が割り当てられていない可能性があります。

```powershell
$m = New-Module -ScriptBlock {
  function Hello ($name) {"Hello, $name"}
  function Goodbye ($name) {"Goodbye, $name"}
} -AsCustomObject
$m
$m | Get-Member
```

```Output
TypeName: System.Management.Automation.PSCustomObject

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
Goodbye     ScriptMethod System.Object Goodbye();
Hello       ScriptMethod System.Object Hello();
```

```powershell
$m.goodbye("Jane")
```

```Output
Goodbye, Jane
```

```powershell
$m.hello("Manoj")
```

```Output
Hello, Manoj
```

`$m`コマンドレットにパイプを `Get-Member` 設定すると、カスタムオブジェクトのプロパティとメソッドが表示されます。 出力は、オブジェクトに関数と関数を表すスクリプトメソッドがあることを示して `Hello` `Goodbye` います。
最後に、これらのスクリプトメソッドを呼び出し、結果を表示します。

### 例 6: スクリプトブロックの結果を取得する

この例では、 **Returnresult** パラメーターを使用して、モジュールオブジェクトを要求するのではなく、スクリプトブロックの実行結果を要求します。 新しいモジュールのスクリプトブロックは関数を定義し、 `SayHello` 関数を呼び出します。

```powershell
New-Module -ScriptBlock {function SayHello {"Hello, World!"}; SayHello} -ReturnResult
```

```Output
Hello, World!
```

## PARAMETERS

### -ArgumentList

スクリプトブロックに渡されるパラメーター値である引数の配列を指定します。 **ArgumentList** の動作の詳細については、「 [about_Splatting](about/about_Splatting.md#splatting-with-arrays)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsCustomObject 場合

このコマンドレットが、動的モジュールを表すカスタムオブジェクトを返すことを示します。 モジュールのメンバーは、カスタム オブジェクトのスクリプト メソッドとして実装されますが、セッションにインポートされません。 カスタム オブジェクトを変数に保存し、ドット表記を使用してメンバーを呼び出すことができます。

同じ名前を持つ複数のメンバーがモジュールに含まれている場合 (両方に名前が付けられた関数や変数など)、カスタムオブジェクトからアクセスできるのは、各名前を持つ1つのメンバーだけです。

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

### -コマンドレット

このコマンドレットがモジュールから現在のセッションにエクスポートするコマンドレットの配列を指定します。
コマンドレットのコンマ区切りのリストを入力します。 ワイルドカード文字を使用できます。 既定では、モジュール内のすべてのコマンドレットがエクスポートされます。

スクリプト ブロック内のコマンドレットは定義できませんが、コマンドレットがバイナリ モジュールからインポートされた場合、動的モジュールがコマンドレットを含めることができます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -関数

このコマンドレットがモジュールから現在のセッションにエクスポートする関数の配列を指定します。
関数のコンマ区切りのリストを入力します。 ワイルドカード文字を使用できます。 既定では、モジュールで定義されたすべての関数がエクスポートされます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Name

新しいモジュールの名前を指定します。 また、パイプを使用してモジュール名を New-Module に渡すこともできます。

既定値は、で始まる自動生成された名前で、その `__DynamicModule_` 後に動的モジュールのパスを指定する GUID が続きます。

```yaml
Type: System.String
Parameter Sets: Name
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ReturnResult

このコマンドレットによってスクリプトブロックが実行され、モジュールオブジェクトを返す代わりにスクリプトブロックの結果が返されることを示します。

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

### -ScriptBlock

動的モジュールの内容を指定します。 コンテンツを中かっこ () で囲み、 `{}` スクリプトブロックを作成します。 このパラメーターは必須です。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

このコマンドレットには、パイプを使用してモジュール名を指定できます。

## 出力

### PSModuleInfo、システムの管理...........

このコマンドレットは、既定で **PSModuleInfo** オブジェクトを生成します。 **Ascustomobject** いうパラメーターを使用すると、 **pscustomobject** オブジェクトが生成されます。 **Returnresult** パラメーターを使用すると、動的モジュールのスクリプトブロックを評価した結果が返されます。

## 注

また、エイリアスでを参照することもでき `New-Module` `nmo` ます。 詳細については、「 [about_Aliases](About/about_Aliases.md)」を参照してください。

## 関連リンク

[Export-ModuleMember](Export-ModuleMember.md)

[Get-Module](Get-Module.md)

[Import-Module](Import-Module.md)

[Remove-Module](Remove-Module.md)

