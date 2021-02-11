---
description: 条件付きテストに基づいてステートメントを実行するために使用できる言語コマンドについて説明します。
Locale: en-US
ms.date: 03/04/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_for?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_For
ms.openlocfilehash: 3f86505d60837e8e5fa4938092a48eeb10833560
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600396"
---
# <a name="about-for"></a>について

## <a name="short-description"></a>簡単な説明
条件付きテストに基づいてステートメントを実行するために使用できる言語コマンドについて説明します。

## <a name="long-description"></a>長い説明

`For`ステートメント (ループとも呼ばれ `For` ます) は、指定した条件がに評価されるときにコマンドブロックでコマンドを実行するループを作成するために使用できる言語構成要素です `$true` 。

ループの一般的な用途 `For` は、値の配列を反復処理し、これらの値のサブセットに対して操作を行うことです。 ほとんどの場合、配列内のすべての値を反復処理するには、ステートメントを使用することを検討してください `Foreach` 。

### <a name="syntax"></a>構文

ステートメントの構文を次に示し `For` ます。

```
for (<Init>; <Condition>; <Repeat>)
{
    <Statement list>
}
```

**Init** プレースホルダーは、ループが開始される前に実行される1つ以上のコマンドを表します。 通常は、ステートメントの **Init** 部分を使用して、開始値を持つ変数を作成および初期化します。

この変数は、ステートメントの次の部分でテストされる条件の基礎となり `For` ます。

**条件** のプレースホルダーは、 `For` `$true` またはの `$false` **ブール** 値に解決されるステートメントの部分を表します。 PowerShell は、ループが実行されるたびに条件を評価し `For` ます。 ステートメントがの場合は `$true` 、コマンドブロック内のコマンドが実行され、ステートメントが再び評価されます。 条件がまだの場合は、 `$true` **ステートメントリスト** 内のコマンドが再度実行されます。 このループは、条件がになるまで繰り返され `$false` ます。

**繰り返し** のプレースホルダーは、ループが繰り返されるたびに実行される、コンマで区切られた1つ以上のコマンドを表します。 通常、これは、ステートメントの **条件** 部分の内部でテストされる変数を変更するために使用されます。

**ステートメントの一覧** のプレースホルダーは、ループが入力または繰り返されるたびに実行される1つ以上のコマンドのセットを表します。 **ステートメントリスト** の内容は、中かっこで囲まれています。

### <a name="support-for-multiple-operations"></a>複数の操作のサポート

次の構文は、 **Init** ステートメントで複数の代入演算に対してサポートされています。

```powershell
# Comma separated assignment expressions enclosed in parenthesis.
for (($i = 0), ($j = 0); $i -lt 10; $i++)
{
    "`$i:$i"
    "`$j:$j"
}

# Sub-expression using the semicolon to separate statements.
for ($($i = 0;$j = 0); $i -lt 10; $i++)
{
    "`$i:$i"
    "`$j:$j"
}
```

次の構文は、 **Repeat** ステートメントで複数の代入演算に対してサポートされています。

```powershell
# Comma separated assignment expressions.
for (($i = 0), ($j = 0); $i -lt 10; $i++)
{
    "`$i:$i"
    "`$j:$j"
}

# Comma separated assignment expressions enclosed in parenthesis.
for (($i = 0), ($j = 0); $i -lt 10; ($i++), ($j++))
{
    "`$i:$i"
    "`$j:$j"
}

# Sub-expression using the semicolon to separate statements.
for ($($i = 0;$j = 0); $i -lt 10; $($i++;$j++))
{
    "`$i:$i"
    "`$j:$j"
}
```

> [!NOTE]
> プリインクリメントまたはポストインクリメント以外の操作は、すべての構文で動作しない場合があります。

複数の **条件** については、次の例に示すように論理演算子を使用します。

```powershell
for (($i = 0), ($j = 0); $i -lt 10 -and $j -lt 10; $i++,$j++)
{
    "`$i:$i"
    "`$j:$j"
}
```

詳細については、「 [about_Logical_Operators](about_Logical_Operators.md)」を参照してください。

### <a name="examples"></a>例

ステートメントには、少なくとも、ステートメントの `For` **Init**、 **Condition**、および **Repeat** 部分を囲むかっこと、ステートメントの **ステートメントリスト** 部分に中かっこで囲まれたコマンドが必要です。

今後の例では、ステートメントの外部にコードを意図的に表示することに注意して `For` ください。 後の例では、コードをステートメントに統合してい `For` ます。

たとえば、次のステートメントでは、 `For` `$i` CTRL + C キーを押してコマンドを手動で中断するまで、変数の値が継続的に表示されます。

```powershell
$i = 1
for (;;)
{
    Write-Host $i
}
```

次の例に示すように、ループが実行されるたびにの値が1ずつ増加するように、ステートメントの一覧にコマンドを追加でき `$i` ます。

```powershell
for (;;)
{
    $i++; Write-Host $i
}
```

CTRL + C キーを押してコマンドを中断するまで、このステートメントでは、ループが実行されるたびに1ずつインクリメントされるため、変数の値が継続的に表示され `$i` ます。

ステートメントのステートメントリストの部分で変数の値を変更するのではなく、 `For` 次のようにステートメントの **繰り返し** 部分を使用でき `For` ます。

```powershell
$i=1
for (;;$i++)
{
    Write-Host $i
}
```

このステートメントは、CTRL + C キーを押してコマンドを中断するまで、無限に繰り返されます。

`For`*条件* を使用して、ループを終了できます。 条件は、ステートメントの **条件** 部分を使用して配置でき `For` ます。 この `For` ループは、条件がと評価されたときに終了し `$false` ます。

次の例では、 `For` の値が10以下の場合、ループが実行 `$i` されます。

```powershell
$i=1
for(;$i -le 10;$i++)
{
    Write-Host $i
}
```

ステートメントの外部で変数を作成して初期化する代わりに `For` 、 `For` ステートメントの **Init** 部分を使用して、このタスクをループ内で実行でき `For` ます。

```powershell
for($i=1; $i -le 10; $i++){Write-Host $i}
```

セミコロンではなく復帰を使用して、ステートメントの **Init**、 **Condition**、および **Repeat** 部分を区切ることができ `For` ます。 次の例は、 `For` この代替構文を使用するを示しています。

```powershell
for ($i = 0
  $i -lt 10
  $i++){
  $i
}
```

この代替形式のステートメントは、 `For` powershell スクリプトファイルと powershell コマンドプロンプトで動作します。 ただし、 `For` コマンドプロンプトで対話型コマンドを入力する場合は、セミコロンでステートメント構文を使用する方が簡単です。

`For`ループは、 `Foreach` パターンを使用して配列またはコレクションの値をインクリメントできるため、ループよりも柔軟です。 次の例では、 `$i` ステートメントの **繰り返し** 部分で、変数が2ずつインクリメントされてい `For` ます。

```powershell
for ($i = 0; $i -le 20; $i += 2)
{
    Write-Host $i
}
```

ループは、 `For` 次の例のように1行に記述することもできます。

```powershell
for ($i = 0; $i -lt 10; $i++) { Write-Host $i }
```

## <a name="see-also"></a>関連項目

[about_Comparison_Operators](about_Comparison_Operators.md)

[about_Foreach](about_Foreach.md)

