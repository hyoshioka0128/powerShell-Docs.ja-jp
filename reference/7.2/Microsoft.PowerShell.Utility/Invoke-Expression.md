---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-Expression
ms.openlocfilehash: 65ccc37b1c9122d54f3caf85f4546e1381558ca9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600787"
---
# Invoke-Expression

## 概要
ローカル コンピューター上でコマンドまたは式を実行します。

## SYNTAX

```
Invoke-Expression [-Command] <String> [<CommonParameters>]
```

## Description

`Invoke-Expression`コマンドレットは、指定された文字列をコマンドとして評価または実行し、式またはコマンドの結果を返します。 を指定しない場合 `Invoke-Expression` 、コマンドラインで送信された文字列は変更されずに返されます。

式は評価され、現在のスコープで実行されます。 詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

> [!CAUTION]
> スクリプトでコマンドレットを使用する場合は、十分な注意が必要 `Invoke-Expression` です。 を使用してユーザーが入力したコマンドを実行する場合は、コマンドを実行する `Invoke-Expression` 前に実行するのが安全であることを確認します。 一般的には、自由な入力を許可するのではなく、定義済みの入力オプションを使用してスクリプトを設計することをお勧めします。

## 例

### 例 1: 式を評価する

```powershell
$Command = "Get-Process"
$Command
```

```Output
Get-Process
```

```powershell
Invoke-Expression $Command
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id   ProcessName
-------  ------    -----      ----- -----   ------     --   -----------
296       4       1572       1956    20       0.53     1348   AdtAgent
270       6       1328       800     34       0.06     2396   alg
67        2       620        484     20       0.22     716    ati2evxx
1060      15      12904      11840   74       11.48    892    CcmExec
1400      33      25280      37544   223      38.44    2564   communicator
...
```

この例では、を使用して `Invoke-Expression` 式を評価する方法を示します。 を指定しない `Invoke-Expression` 場合、式は印刷されますが、評価されません。

最初のコマンドは、 `Get-Process` 変数に (文字列) の値を割り当て `$Command` ます。

2 番目のコマンドは、コマンド ラインで変数名を入力した場合の効果を示しています。 PowerShell は文字列をエコーします。

3番目のコマンドは、を使用し `Invoke-Expression` て文字列を評価します。

### 例 2: ローカルコンピューターでスクリプトを実行する

```powershell
Invoke-Expression -Command "C:\ps-test\testscript.ps1"
"C:\ps-test\testscript.ps1" | Invoke-Expression
```

これらのコマンドは、を使用し `Invoke-Expression` て、ローカルコンピューター上でスクリプト (TestScript.ps1) を実行します。 2 つのコマンドは同等です。 最初のコマンドでは、 **command** パラメーターを使用して、実行するコマンドを指定します。
2番目のコマンドは、パイプライン演算子 () を使用し `|` て、コマンド文字列をに送信し `Invoke-Expression` ます。

### 例 3: 変数でコマンドを実行する

```powershell
$Command = 'Get-Process | where {$_.cpu -gt 1000}'
Invoke-Expression $Command
```

この例では、変数に保存されているコマンド文字列を実行し `$Command` ます。

コマンド文字列は、現在のオブジェクトを表す変数 () が含まれているため、単一引用符で囲まれてい `$_` ます。 二重引用符で囲まれている場合は、変数 `$_` に保存される前に、変数がその値に置き換えられ `$Command` ます。

### 例 4: コマンドレットのヘルプ例を取得して実行する

```powershell
$Cmdlet_name = "Get-EventLog"
$Example_number = 1
$Example_code = (Get-Help $Cmdlet_name).examples.example[($Example_number-1)].code
Invoke-Expression $Example_code
```

このコマンドは、コマンドレットのヘルプトピックの最初の例を取得して実行し `Get-EventLog` ます。

別のコマンドレットの例を実行するには、変数の値 `$Cmdlet_name` をコマンドレットの名前に変更します。 また、変数を、 `$Example_number` 実行するサンプル番号に変更します。 例の番号が有効でない場合、コマンドは失敗します。

## PARAMETERS

### -Command

実行するコマンドまたは式を指定します。 コマンドまたは式を入力するか、コマンドまたは式を含む変数を入力します。 **Command** パラメーターは必須です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.string または PSObject

パイプを使用して、コマンドを表すオブジェクトをにパイプすることができ `Invoke-Expression` ます。
`$Input`コマンド内の入力オブジェクトを表すには、自動変数を使用します。

## 出力

### PSObject

呼び出されたコマンド ( **command** パラメーターの値) によって生成される出力を返します。

## 注

ほとんどの場合、PowerShell の call 演算子を使用して式を呼び出し、同じ結果を達成します。
呼び出し演算子は安全な方法です。 詳細については、「 [about_Operators](../microsoft.powershell.core/about/about_operators.md#call-operator-)」を参照してください。

## 関連リンク

[Invoke-Command](../Microsoft.PowerShell.Core/Invoke-Command.md)

[about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)

