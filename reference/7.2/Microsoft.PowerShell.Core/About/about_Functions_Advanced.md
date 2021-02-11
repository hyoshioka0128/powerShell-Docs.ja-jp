---
description: には、スクリプトを使用してコマンドレットを作成するための高度な関数が導入されています。
Locale: en-US
ms.date: 06/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions_advanced?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Functions_Advanced
ms.openlocfilehash: a0b8b027c91f2adedfcecd07bfc80392c1b1e071
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599708"
---
# <a name="about-functions-advanced"></a>関数の詳細

## <a name="short-description"></a>概要
には、スクリプトを使用してコマンドレットを作成するための高度な関数が導入されています。

## <a name="long-description"></a>詳細説明

コマンドレットは、PowerShell のパイプラインセマンティクスに関与する単一のコマンドです。 これには、バイナリコマンドレット、高度なスクリプト関数、CDXML、ワークフローが含まれます。

高度な関数を使用すると、PowerShell 関数として記述されたコマンドレットを作成できます。 高度な関数を使用すると、バイナリコマンドレットを記述してコンパイルしなくても、コマンドレットを簡単に作成できます。 バイナリコマンドレットは、C# などの .NET 言語で記述された .NET クラスです。

高度な関数は、属性を使用し `CmdletBinding` て、コマンドレットのように機能する関数として識別します。 属性は、コマンドレット `CmdletBinding` としてクラスを識別するためにコンパイル済みのコマンドレットクラスで使用されるコマンドレット属性に似ています。 この属性の詳細については、「 [about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)」を参照してください。

次の例は、名前を受け取り、指定された名前を使用してあいさつを出力する関数を示しています。 また、この関数は、コンパイルされたコマンドレットの動詞と名詞のペアのような動詞 (送信) と名詞 (あいさつ) のペアを含む名前を定義します。 ただし、関数には動詞と名詞の名前を付ける必要はありません。

```powershell
function Send-Greeting
{
    [CmdletBinding()]
    Param(
        [Parameter(Mandatory=$true)]
        [string] $Name
    )

    Process
    {
        Write-Host ("Hello " + $Name + "!")
    }
}
```

関数のパラメーターは、パラメーター属性を使用して宣言されます。
この属性は単独で使用することも、別名属性またはその他のいくつかのパラメーター検証属性と組み合わせることもできます。 パラメーターを宣言する方法 (実行時に追加される動的パラメーターを含む) の詳細については、「 [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)」を参照してください。

前の関数の実際の処理は、プロセスブロック内で実行されます。これは、コマンドレットに渡されるデータを処理するためにコンパイルされたコマンドレットによって使用される ProcessingRecord メソッドに相当します。 このブロックは Begin および End ブロックと共に、 [about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md) のトピックで説明されています。

高度な関数は、コンパイルされたコマンドレットとは次の点で異なります。

- 高度な関数パラメーターのバインドでは、文字列の配列がブール型パラメーターにバインドされている場合、例外はスローされません。
- ValidateSet 属性と Validateset 属性は、名前付きパラメーターを渡すことはできません。
- 高度な関数をトランザクションで使用することはできません。

## <a name="see-also"></a>関連項目

[about_Functions](about_Functions.md)

[about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

[about_Functions_OutputTypeAttribute](about_Functions_OutputTypeAttribute.md)
