---
ms.date: 09/13/2016
ms.topic: reference
title: パラメーター入力を検証する
description: パラメーター入力を検証する
ms.openlocfilehash: a97b5c670e8c36463a85bbef1506f6311bdd5ec3
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660393"
---
# <a name="validating-parameter-input"></a>パラメーター入力を検証する

PowerShell では、コマンドレットパラメーターに渡す引数をいくつかの方法で検証できます。
PowerShell では、引数の長さ、範囲、および文字のパターンを検証できます。
使用可能な引数の数 (カウント) を検証できます。
これらの検証規則は、コマンドレットクラスのパブリックプロパティでパラメーター属性を使用して宣言された検証属性によって定義されます。

パラメーター引数を検証するために、PowerShell ランタイムは検証属性によって提供される情報を使用して、コマンドレットの実行前にパラメーターの値を確認します。
パラメーターの入力が有効でない場合、ユーザーはエラーメッセージを受け取ります。
各検証パラメーターでは、PowerShell によって適用される検証規則を定義します。

PowerShell では、次の属性に基づいて検証規則が適用されます。

### <a name="validatecount"></a>ValidateCount

パラメーターが受け入れることができる引数の最小数と最大数を指定します。
詳細については、「 [Validatecount 属性の宣言](./validatecount-attribute-declaration.md)」を参照してください。

### <a name="validatelength"></a>ValidateLength

パラメーター引数の文字数の最小値と最大値を指定します。
詳細については、「 [ValidateLength Attribute 宣言](./validatelength-attribute-declaration.md)」を参照してください。

### <a name="validatepattern"></a>ValidatePattern

パラメーター引数を検証する正規表現を指定します。
詳細については、「 [Validatepattern 属性の宣言](./validatepattern-attribute-declaration.md)」を参照してください。

### <a name="validaterange"></a>ValidateRange

パラメーター引数の最小値と最大値を指定します。
詳細については、「 [ValidateRange Attribute 宣言](./validaterange-attribute-declaration.md)」を参照してください。

### <a name="validateset"></a>ValidateSet

パラメーター引数の有効な値を指定します。
詳細については、「 [Validateset 属性の宣言](./validateset-attribute-declaration.md)」を参照してください。

## <a name="see-also"></a>参照

[パラメーター入力を検証する方法](./how-to-validate-parameter-input.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
