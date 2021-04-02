---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 03/30/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-variable?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Variable
ms.openlocfilehash: 71bb70dac3436c5293b2a34ce52e54e751786a48
ms.sourcegitcommit: 4d6ed6f7d747a9bbb3fcfcf6c981c5aa8a973a08
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2021
ms.locfileid: "106072263"
---
# New-Variable

## 構文
新しい変数を作成します。

## 構文

```
New-Variable [-Name] <String> [[-Value] <Object>] [-Description <String>] [-Option <ScopedItemOptions>]
 [-Visibility <SessionStateEntryVisibility>] [-Force] [-PassThru] [-Scope <String>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## 説明

`New-Variable`コマンドレットでは、Windows PowerShell で新しい変数を作成します。 変数の作成時に、変数に値を割り当てることも、作成後に値の割り当てまたは変更を行うこともできます。

のパラメーターを使用して、変数 `New-Variable` のプロパティの設定、変数のスコープの設定、および変数がパブリックかプライベートかの判断を行うことができます。

通常、新しい変数を作成するには、変数名とその値 (など) を入力し `$Var = 3` ますが、コマンドレットを使用してそのパラメーターを使用することもでき `New-Variable` ます。

## 例

### 例 1: 変数を作成する

```
New-Variable days
```

このコマンドは、days という名前の新しい変数を作成します。 **Name** パラメーターを入力する必要はありません。

### 例 2: 変数を作成して値を割り当てる

```
New-Variable -Name "zipcode" -Value 98033
```

このコマンドは、zipcode という名前の変数を作成し、値98033を割り当てます。

### 例 3: ReadOnly オプションを使用して変数を作成する

```
PS C:\> New-Variable -Name Max -Value 256 -Option ReadOnly
PS C:\> New-Variable -Name max -Value 1024

New-Variable : A variable with name 'max' already exists.
At line:1 char:1
+ New-Variable -Name max -Value 1024
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ResourceExists: (max:String) [New-Variable], SessionStateException
    + FullyQualifiedErrorId : VariableAlreadyExists,Microsoft.PowerShell.Commands.NewVariableCommand

PS C:\> New-Variable -Name max -Value 1024 -Force
```

この例では、のオプションを使用し `ReadOnly` `New-Variable` て、変数を上書きから保護する方法を示します。

最初のコマンドは、Max という名前の新しい変数を作成し、その値を256に設定します。 この例では、 **オプション** パラメーターに値を指定 `ReadOnly` しています。

2 番目のコマンドは、同じ名前の 2 番目の変数の作成を試みます。 その変数に読み取り専用オプションが設定されているため、このコマンドはエラーを返します。

3番目のコマンドは、 **Force** パラメーターを使用して、変数の読み取り専用保護を上書きします。
この場合、コマンドは、同じ名前を持つ新しい変数を正常に作成できます。

### 例 4: 変数に複数のオプションを割り当てる

```powershell
New-Variable -Name 'TestVariable' -Value 'Test Value' -Option AllScope,Constant
```

この例では、変数を作成し、 `AllScope` オプションとオプションを割り当て `Constant` ます。これにより、変数は現在のスコープで使用できるようになり、新しいスコープが作成され、変更または削除できなくなります。

### 例 5: プライベート変数を作成する

```
PS C:\> New-Variable -Name counter -Visibility Private

#Effect of private variable in a module.

PS C:\> Get-Variable c*

Name                           Value
----                           -----
Culture                        en-US
ConsoleFileName
ConfirmPreference              High
CommandLineParameters          {}

PS C:\> $counter
"Cannot access the variable '$counter' because it is a private variable"
At line:1 char:1
+ $counter
+ ~~~~~~~~
    + CategoryInfo          : PermissionDenied: (counter:String) [], SessionStateException
    + FullyQualifiedErrorId : VariableIsPrivate

PS C:\> Get-Counter
Name         Value
----         -----
Counter1     3.1415
...
```

このコマンドは、モジュール内のプライベート変数の動作を示しています。 モジュールには、 `Get-Counter` Counter という名前のプライベート変数を持つコマンドレットが含まれています。 このコマンドは、 **Visibility** パラメーターと値 Private を使用して変数を作成します。

サンプル出力は、プライベート変数の動作を示しています。 モジュールを読み込んだユーザーは、Counter 変数の値を表示または変更できませんが、モジュール内のコマンドによって Counter 変数を読み取ったり変更したりすることができます。

### 例 6: スペースを含む変数を作成する

```
PS C:\> New-Variable -Name 'with space' -Value 'abc123xyz'

PS C:\> Get-Variable -Name 'with space'

Name                           Value
----                           -----
with space                     abc123xyz

PS C:\> ${with space}
abc123xyz
```

このコマンドは、スペースを含む変数を作成できることを示しています。 変数は、コマンドレットを使用して、 `Get-Variable` または変数を中かっこで区切ることによって直接アクセスできます。

## パラメーター

### -Description

変数の説明を指定します。

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

### -Force

コマンドレットによって、既存の読み取り専用変数と同じ名前の変数が作成されることを示します。

既定では、変数のオプションの値がまたはの場合を除き、変数を上書きでき `ReadOnly` `Constant` ます。 詳細については、「 **オプション** パラメーター」を参照してください。

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

### -Name

新しい変数の名前を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Option

変数の **Options** プロパティの値を指定します。 このパラメーターの有効値は、次のとおりです。

- `None` -オプションを設定しません。 `None` は既定値です。
- `ReadOnly` -削除できます。 **Force** パラメーターを使用する場合を除き、を変更することはできません。
- `Private` -変数は、現在のスコープでのみ使用できます。
- `AllScope` -変数は、作成された新しいスコープにコピーされます。
- `Constant` -削除または変更できません。 `Constant` 変数を作成する場合にのみ有効です。 既存の変数のオプションをに変更することはできません `Constant` 。

これらの値はフラグベースの列挙体として定義されます。 このパラメーターを使用すると、複数の値を組み合わせて複数のフラグを設定できます。 値は、値の配列として **オプション** パラメーターに渡すことも、その値のコンマ区切りの文字列として渡すこともできます。 コマンドレットでは、バイナリまたは演算を使用して値を結合します。 配列として値を渡すのが最も簡単なオプションであり、値に対してタブ補完を使用することもできます。

セッション内のすべての変数の Options プロパティを表示するには、「」と入力 `Get-Variable | Format-Table -Property name, options -AutoSize` します。

```yaml
Type: System.Management.Automation.ScopedItemOptions
Parameter Sets: (All)
Aliases:
Accepted values: None, ReadOnly, Constant, Private, AllScope, Unspecified

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -スコープ

新しい変数のスコープを指定します。 このパラメーターの有効値は、次のとおりです。

- `Global` -グローバルスコープで作成された変数は、PowerShell プロセス内のすべての場所でアクセスできます。
- `Local` -ローカルスコープは現在のスコープを参照します。これは、コンテキストによっては任意のスコープにすることができます。
- `Script` -スクリプトスコープで作成された変数は、で作成されたスクリプトファイルまたはモジュール内でのみアクセスできます。
- `Private` -プライベートスコープ内に作成された変数は、存在するスコープの外部ではアクセスできません。 プライベートスコープを使用すると、別のスコープに同じ名前の項目のプライベートバージョンを作成できます。
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数。0は現在のスコープ、1は親、親スコープの親は2など)。 負の数値は使用できません。

`Local` スコープパラメーターが指定されていない場合、は既定のスコープです。

詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

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

### -Value

変数の初期値を指定します。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -可視性

変数を、作成元のセッションの外部で表示されるようにするかどうかを決定します。 このパラメーターは、他のユーザーに配信されるスクリプトおよびコマンドで使用するために設計されています。 このパラメーターの有効値は、次のとおりです。

- `Public` -変数が表示されます。 `Public` は既定値です。
- `Private` -変数は表示されません。

変数がプライベートである場合は、によって返された変数 `Get-Variable` や、ドライブの表示形式など、変数の一覧に表示されません `Variable:` 。 プライベート変数の値を読み取りまたは変更するコマンドは、エラーを返します。 ただし、コマンドが、変数の定義元のセッションで記述されている場合は、プライベート変数を使用するコマンドを実行できます。

```yaml
Type: System.Management.Automation.SessionStateEntryVisibility
Parameter Sets: (All)
Aliases:
Accepted values: Public, Private

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

### System.Object

パイプを使用して値をにパイプすることができ `New-Variable` ます。

## 出力

### None または system.string です。

**PassThru** パラメーターを使用すると、に `New-Variable` よって新しい変数を表す system.string オブジェクトが生成されます。 それ以外の場合、このコマンドレットによる出力はありません。

## メモ

## 関連リンク

[Clear-Variable](Clear-Variable.md)

[Get-Variable](Get-Variable.md)

[Remove-Variable](Remove-Variable.md)

[変数の設定](Set-Variable.md)
