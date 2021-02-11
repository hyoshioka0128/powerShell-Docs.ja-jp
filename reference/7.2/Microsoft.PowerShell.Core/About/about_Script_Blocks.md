---
description: スクリプトブロックとは何かを定義し、PowerShell プログラミング言語でスクリプトブロックを使用する方法について説明します。
Locale: en-US
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_script_blocks?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Script_Blocks
ms.openlocfilehash: 1ba5e92bedc03a2d61d528715ab13c5d96eb3075
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599555"
---
# <a name="about-script-blocks"></a>スクリプトブロックについて

## <a name="short-description"></a>簡単な説明

スクリプトブロックとは何かを定義し、PowerShell プログラミング言語でスクリプトブロックを使用する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell プログラミング言語では、スクリプトブロックは1つの単位として使用できるステートメントまたは式のコレクションです。
スクリプト ブロックは引数を受け取り、値を返すことができます。

構文的には、次の構文に示すように、スクリプトブロックは中かっこで囲まれたステートメントリストです。

```
{<statement list>}
```

スクリプトブロックは、スクリプトブロック内のすべてのコマンドの出力を、単一のオブジェクトまたは配列として返します。

キーワードを使用して戻り値を指定することもでき `return` ます。 キーワードは、 `return` スクリプトブロックから返された他の出力には影響しません。 ただし、キーワードは、 `return` その行のスクリプトブロックを終了します。 詳細については、「 [about_Return](about_Return.md)」を参照してください。

関数と同様に、スクリプトブロックにはパラメーターを含めることができます。 次の構文に示すように、Param キーワードを使用して名前付きパラメーターを割り当てます。

```
{
Param([type]$Parameter1 [,[type]$Parameter2])
<statement list>
}
```

> [!NOTE]
> スクリプトブロックでは、関数とは異なり、中かっこの外側でパラメーターを指定することはできません。

関数と同様に、スクリプトブロックには、、、、およびの各キーワードを含めることができ `DynamicParam` `Begin` `Process` `End` ます。 詳細については、「 [about_Functions](about_Functions.md) 」および「 [about_Functions_Advanced](about_Functions_Advanced.md)」を参照してください。

## <a name="using-script-blocks"></a>スクリプトブロックの使用

スクリプトブロックは、Microsoft .NET Framework 型のインスタンス `System.Management.Automation.ScriptBlock` です。 コマンドには、スクリプトブロックのパラメーター値を指定できます。 たとえば、コマンドレットには、次の `Invoke-Command` `ScriptBlock` 例に示すように、スクリプトブロックの値を受け取るパラメーターがあります。

```powershell
Invoke-Command -ScriptBlock { Get-Process }
```

```Output
Handles  NPM(K)    PM(K)     WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----     ----- -----   ------     -- -----------
999          28    39100     45020   262    15.88   1844 communicator
721          28    32696     36536   222    20.84   4028 explorer
...
```

`Invoke-Command` また、パラメーターブロックを含むスクリプトブロックを実行することもできます。
パラメーターは、 **ArgumentList** パラメーターを使用して位置によって割り当てられます。

```powershell
Invoke-Command -ScriptBlock { param($p1, $p2)
"p1: $p1"
"p2: $p2"
} -ArgumentList "First", "Second"
```

```Output
p1: First
p2: Second
```

前の例のスクリプトブロックでは、キーワードを使用し `param` て、パラメーターとを作成して `$p1` `$p2` います。 "First" という文字列は最初のパラメーター () にバインドされ、 `$p1` "Second" は () にバインドされ `$p2` ます。

**ArgumentList** の動作の詳細については、「 [about_Splatting](about_Splatting.md#splatting-with-arrays)」を参照してください。

変数を使用して、スクリプトブロックを格納および実行することができます。 次の例では、スクリプトブロックを変数に格納し、に渡し `Invoke-Command` ます。

```powershell
$a = { Get-Service BITS }
Invoke-Command -ScriptBlock $a
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  BITS               Background Intelligent Transfer Ser...
```

呼び出し演算子は、変数に格納されているスクリプトブロックを実行するもう1つの方法です。
と同様 `Invoke-Command` に、呼び出し演算子は、子スコープでスクリプトブロックを実行します。 呼び出し演算子を使用すると、スクリプトブロックでパラメーターを簡単に使用できるようになります。

```powershell
$a ={ param($p1, $p2)
"p1: $p1"
"p2: $p2"
}
&$a -p2 "First" -p1 "Second"
```

```Output
p1: Second
p2: First
```

割り当てを使用して、スクリプトブロックの出力を変数に格納できます。

```
PS>  $a = { 1 + 1}
PS>  $b = &$a
PS>  $b
2
```

```
PS>  $a = { 1 + 1}
PS>  $b = Invoke-Command $a
PS>  $b
2
```

呼び出し演算子の詳細については、「 [about_Operators](about_Operators.md)」を参照してください。

## <a name="using-delay-bind-script-blocks-with-parameters"></a>遅延バインドスクリプトブロックをパラメーターと共に使用する

パイプラインの入力 () または () を受け入れる型指定されたパラメーター `by Value` `by PropertyName` では、パラメーターで **遅延バインディング** スクリプトブロックを使用できます。
**遅延バインド** スクリプトブロック内では、パイプライン変数を使用してパイプされた in オブジェクトを参照でき `$_` ます。

```powershell
# Renames config.log to old_config.log
dir config.log | Rename-Item -NewName {"old_" + $_.Name}
```

より複雑なコマンドレットでは、遅延バインドスクリプトブロックを使用すると、パイプを使用してパイプを使用して他のパラメーターを設定することができます。

遅延をパラメーターとして **バインド** するスクリプトブロックに関する注意事項:

- **遅延バインド** スクリプトブロックで使用するパラメーター名は、明示的に指定する必要があります。
- パラメーターを型指定することはできません。また、パラメーターの型をまたはにすることはできません `[scriptblock]` `[object]` 。
- パイプライン入力を指定せずに **遅延バインディング** スクリプトブロックを使用すると、エラーが発生します。

  ```powershell
  Rename-Item -NewName {$_.Name + ".old"}
  ```

  ```Output
  Rename-Item : Cannot evaluate parameter 'NewName' because its argument is
  specified as a script block and there is no input. A script block cannot
  be evaluated without input.
  At line:1 char:23
  +  Rename-Item -NewName {$_.Name + ".old"}
  +                       ~~~~~~~~~~~~~~~~~~
      + CategoryInfo          : MetadataError: (:) [Rename-Item],
        ParameterBindingException
      + FullyQualifiedErrorId : ScriptBlockArgumentNoInput,
        Microsoft.PowerShell.Commands.RenameItemCommand
  ```

## <a name="see-also"></a>参照

[about_Functions](about_Functions.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Operators](about_Operators.md)

