---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/new-variable?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Variable
ms.openlocfilehash: c77eb9c64022d028c3c4b2de6bf4551886b940e1
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601991"
---
# New-Variable

## 概要
新しい変数を作成します。

## SYNTAX

```
New-Variable [-Name] <String> [[-Value] <Object>] [-Description <String>] [-Option <ScopedItemOptions>]
 [-Visibility <SessionStateEntryVisibility>] [-Force] [-PassThru] [-Scope <String>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description
**新しい変数** のコマンドレットは、PowerShell で新しい変数を作成します。
変数の作成時に、変数に値を割り当てることも、作成後に値の割り当てまたは変更を行うこともできます。

**新しい変数** のパラメーターを使用して、変数のプロパティの設定、変数のスコープの設定、および変数がパブリックかプライベートかの判断を行うことができます。

通常は、変数名とその値 (など) を入力して新しい変数を作成し `$Var = 3` ますが、パラメーターを使用するには、 **新しい** 変数のコマンドレットを使用します。

## 例

### 例 1: 変数を作成する

```
PS C:\> New-Variable days
```

このコマンドは、days という名前の新しい変数を作成します。
*Name* パラメーターを入力する必要はありません。

### 例 2: 変数を作成して値を割り当てる

```
PS C:\> New-Variable -Name "zipcode" -Value 98033
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

この例では、 **New 変数** の ReadOnly オプションを使用して、変数を上書きから保護する方法を示します。

最初のコマンドは、Max という名前の新しい変数を作成し、その値を256に設定します。
この例では、 *オプション* パラメーターに ReadOnly の値を指定しています。

2 番目のコマンドは、同じ名前の 2 番目の変数の作成を試みます。
その変数に読み取り専用オプションが設定されているため、このコマンドはエラーを返します。

3番目のコマンドは、 *Force* パラメーターを使用して、変数の読み取り専用保護を上書きします。
この場合、コマンドは、同じ名前を持つ新しい変数を正常に作成できます。

### 例 4: プライベート変数を作成する

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

このコマンドは、モジュール内のプライベート変数の動作を示しています。
モジュールには、Counter という名前のプライベート変数を持つ Get-Counter コマンドレットが含まれています。
このコマンドは、 *Visibility* パラメーターと値 Private を使用して変数を作成します。

サンプル出力は、プライベート変数の動作を示しています。
モジュールを読み込んだユーザーは、Counter 変数の値を表示または変更できませんが、モジュール内のコマンドによって Counter 変数を読み取ったり変更したりすることができます。

### 例 5: スペースを含む変数を作成する

```
PS C:\> New-Variable -Name 'with space' -Value 'abc123xyz'

PS C:\> Get-Variable -Name 'with space'

Name                           Value
----                           -----
with space                     abc123xyz

PS C:\> ${with space}
abc123xyz
```

このコマンドは、スペースを含む変数を作成できることを示しています。
変数は、変数を中かっこで区切ることによって、変数を **取得** したり、変数に直接アクセスしたりすることができます。

## PARAMETERS

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

既定では、変数のオプション値が ReadOnly または Constant でない限り、変数を上書きできます。
詳細については、「 *オプション* パラメーター」を参照してください。

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
変数の **Options** プロパティの値を指定します。このパラメーターに指定できる値は次のとおりです。

- [なし] :
オプションを設定しません
(既定値は None です)。
- ReadOnly.
削除できます。
*Force* パラメーターを使用する場合を除き、を変更することはできません。
- プライベート。
変数は、現在のスコープ内でのみ使用可能です。
- Allscope オプション.
変数は、作成された任意の新しいスコープにコピーされます。
- 定数。
削除または変更できません。
定数は、変数を作成する場合にのみ有効です。
既存の変数のオプションを定数に変更することはできません。

セッション内のすべての変数の **Options** プロパティを表示するには、「」と入力 `Get-Variable | Format-Table -Property name, options -autosize` します。

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
作業中の項目を表すオブジェクトを返します。
既定では、このコマンドレットによる出力はありません。

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
新しい変数のスコープを指定します。
このパラメーターの有効値は、次のとおりです。

- 全体.
グローバルスコープで作成された変数は、PowerShell プロセス内のすべての場所でアクセスできます。
- Local。
ローカルスコープは、現在のスコープを参照します。これは、コンテキストによっては任意のスコープにすることができます。
- スクリプティング。
スクリプトスコープで作成された変数は、で作成されたスクリプトファイルまたはモジュール内でのみアクセスできます。
- プライベート。
プライベートスコープで作成された変数は、存在するスコープの外部ではアクセスできません。
プライベートスコープを使用すると、別のスコープに同じ名前の項目のプライベートバージョンを作成できます。
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数。0は現在のスコープ、1は親、親スコープの親は2など)。
負の数値は使用できません。

Scope パラメーターが指定されていない場合、Local は既定のスコープです。

詳細については、「about_Scopes」を参照してください。

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
変数を、作成元のセッションの外部で表示されるようにするかどうかを決定します。
このパラメーターは、他のユーザーに配信されるスクリプトおよびコマンドで使用する目的で設計されています。
このパラメーターの有効値は、次のとおりです。

- パブリック。
変数は表示されます
(Public が既定値です)。
- プライベート。
変数は表示されません。

変数がプライベートの場合、その変数は、Get-Variable で返されるような変数一覧または Variable: ドライブの表示には出現しません。
プライベート変数の値を読み取りまたは変更するコマンドは、エラーを返します。
ただし、コマンドが、変数の定義元のセッションで記述されている場合は、プライベート変数を使用するコマンドを実行できます。

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
コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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
パイプを使用して値を **新しい変数** にパイプすることができます。

## 出力

### None または system.string です。
*PassThru* パラメーターを使用すると、 **new 変数** は新しい変数を表す **system.string オブジェクト** を生成します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

## 関連リンク

[Clear-Variable](Clear-Variable.md)

[Get-Variable](Get-Variable.md)

[Remove-Variable](Remove-Variable.md)

[変数の設定](Set-Variable.md)

