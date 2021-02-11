---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 04/09/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/set-strictmode?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-StrictMode
ms.openlocfilehash: 8773c69d638a1d04dc946cf448a44799341e663b
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390594"
---
# Set-StrictMode

## 概要
式、スクリプト、およびスクリプト ブロックでのコーディング規則を確立し、適用します。

## SYNTAX

### Version (既定値)

```
Set-StrictMode -Version <Version> [<CommonParameters>]
```

### オフ

```
Set-StrictMode [-Off] [<CommonParameters>]
```

## Description

`Set-StrictMode`このコマンドレットは、現在のスコープとすべての子スコープの厳格モードを構成し、オンまたはオフにします。 Strict モードがオンの場合、式、スクリプト、またはスクリプトブロックの内容が基本的なベストプラクティスのコーディング規則に違反すると、PowerShell によって終了エラーが生成されます。

**バージョン** パラメーターを使用して、適用するコーディング規則を決定します。

`Set-PSDebug -Strict` コマンドレットは、グローバルスコープに対して strict モードを有効にします。 `Set-StrictMode` は、現在のスコープとその子スコープにのみ影響します。 このため、スクリプトまたは関数で使用して、グローバルスコープから継承された設定をオーバーライドできます。

`Set-StrictMode`がオフの場合、PowerShell には次の動作があります。

- 初期化されていない変数は `$Null` 、型に応じて 0 (ゼロ) またはの値を持つと見なされます。
- 存在しないプロパティへの参照はを返します `$Null`
- 不適切な関数構文の結果は、エラー条件によって異なります。
- 配列内の無効なインデックスを使用して値を取得しようとすると、が返されます。 `$Null`

## 例

### 例 1: strict モードをバージョン1.0 として有効にする

```powershell
# Strict mode is off by default.
$a -gt 5
```

```Output
False
```

```powershell
Set-StrictMode -Version 1.0
$a -gt 5
```

```Output
InvalidOperation: The variable '$a' cannot be retrieved because it has not been set.
```

Strict モードがバージョン1.0 に設定されている場合、初期化されていない変数を参照しようとすると失敗します。

### 例 2: strict モードをバージョン2.0 として有効にする

```powershell
# Strict mode is off by default.
function add ($a, $b) {
    '$a = ' + $a
    '$b = ' + $b
    '$a+$b = ' + ($a + $b)
}
add 3 4
```

```Output
$a = 3
$b = 4
$a+$b = 7
```

```powershell
add(3,4)
```

```Output
$a = 3 4
$b =
$a+$b = 3 4
```

```powershell
Set-StrictMode -Version 2.0
add(3,4)
```

```Output
InvalidOperation: The function or command was called as if it were a method. Parameters should be separated by spaces. For information about parameters, see the about_Parameters Help topic.
```

```powershell
Set-StrictMode -Off
$string = "This is a string."
$null -eq $string.Month
```

```Output
True
```

```powershell
Set-StrictMode -Version 2.0
$string = "This is a string."
$null -eq $string.Month
```

```Output
PropertyNotFoundException: The property 'Month' cannot be found on this object. Verify that the property exists.
```

このコマンドは、の厳格モードをオンにし、バージョン2.0 に設定します。 その結果、関数呼び出しにかっことコンマを使用しているか、初期化されていない変数または存在しないプロパティを参照するメソッド構文を使用すると、PowerShell はエラーを返します。

サンプル出力は、バージョン 2.0 strict モードの効果を示しています。

バージョン 2.0 の厳格モードを使用しない場合、値 "(3,4)" は、何も追加されない 1 つの配列オブジェクトとして解釈されます。 バージョン 2.0 strict モードを使用すると、2つの値を送信するための正しくない構文として正しく解釈されます。

バージョン2.0 を使用しない場合、文字列の存在しない **Month** プロパティへの参照はのみを返し `$Null` ます。 バージョン2.0 を使用すると、参照エラーとして正しく解釈されます。

### 例 3: strict モードをバージョン3.0 として有効にする

Strict モードが **Off** に設定されている場合、無効または範囲外のインデックスの結果は null 値を返します。

```powershell
# Strict mode is off by default.
$a = @(1)
$null -eq $a[2]
$null -eq $a['abc']
```

```Output
True
True
```

```powershell
Set-StrictMode -Version 3
$a = @(1)
$null -eq $a[2]
$null -eq $a['abc']
```

```Output
OperationStopped: Index was outside the bounds of the array.

InvalidArgument: Cannot convert value "abc" to type "System.Int32". Error: "Input string was not in a correct format."
```

Strict モードがバージョン3以上に設定されている場合、インデックスが無効または範囲外の場合、エラーが発生します。

## PARAMETERS

### -Off

このコマンドレットが、現在のスコープとすべての子スコープの厳格モードをオフにすることを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Off
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Version

厳格モードでエラーとなる条件を指定します。 このパラメーターは、任意の有効な PowerShell バージョン番号を受け取ります。 3より大きい数値は、 **最新** のものとして扱われます。

このパラメーターの有効な値は次のとおりです。

- 1.0
  - 初期化されていない変数への参照を禁止します (文字列の初期化されていない変数を除く)
- 2.0
  - 初期化されていない変数への参照を禁止します。 これには、初期化されていない変数が文字列に含まれます。
  - オブジェクトの存在しないプロパティへの参照を禁止します。
  - メソッドを呼び出すための構文を使用する関数呼び出しを禁止します。
- 3.0
  - 初期化されていない変数への参照を禁止します。 これには、初期化されていない変数が文字列に含まれます。
  - オブジェクトの存在しないプロパティへの参照を禁止します。
  - メソッドを呼び出すための構文を使用する関数呼び出しを禁止します。
  - 範囲外または解決できない配列インデックスを禁止します。
- 最新
  - 使用可能な最新バージョンを選択します。 最新バージョンは最も厳格です。 新しいバージョンを PowerShell に追加した場合でも、スクリプトが使用可能な最も厳格なバージョンを使用するようにするには、この値を使用します。

> [!CAUTION]
> スクリプトで **最新****バージョン** を使用します。 **最新** のの意味は、PowerShell の新しいリリースで変更される可能性があります。 そのため、を使用する古いバージョンの PowerShell 用に記述 `Set-StrictMode -Version Latest` されたスクリプトには、新しいバージョンの powershell で実行するときに、より制限の厳しい規則が適用されます。

```yaml
Type: System.Version
Parameter Sets: Version
Aliases: v

Required: True
Position: Named
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

### なし

このコマンドレットによる戻り値はありません。

## 注

`Set-StrictMode` は、その子スコープに設定されているスコープ内でのみ有効です。 PowerShell のスコープの詳細については、「 [about_Scopes](about/about_Scopes.md)」を参照してください。

## 関連リンク

[Set-PSDebug](Set-PSDebug.md)
