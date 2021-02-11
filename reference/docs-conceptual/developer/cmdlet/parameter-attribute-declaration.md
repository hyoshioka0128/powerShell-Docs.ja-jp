---
ms.date: 09/13/2016
ms.topic: reference
title: パラメーター属性の宣言
description: パラメーター属性の宣言
ms.openlocfilehash: bab48a94cb4b1e8501fb79c2f3ef71393fa2ee68
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92650346"
---
# <a name="parameter-attribute-declaration"></a>パラメーター属性の宣言

Parameter 属性は、コマンドレットパラメーターとしてコマンドレットクラスのパブリックプロパティを識別します。

## <a name="syntax"></a>構文

```csharp
[Parameter()]
[Parameter(Named Parameters...)]
```

#### <a name="parameters"></a>パラメーター

`Mandatory` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 `True` コマンドレットパラメーターが必要であることを示します。 コマンドレットが呼び出されたときに必要なパラメーターが指定されていない場合、Windows PowerShell はユーザーにパラメーター値の入力を求めます。 既定値は、`false` です。

`ParameterSetName` ([System.string](/dotnet/api/System.String)) 省略可能な名前付きパラメーター。 このコマンドレットパラメーターが属するパラメーターセットを指定します。 パラメーターセットが指定されていない場合、パラメーターはすべてのパラメーターセットに属します。

`Position` ([System.string](/dotnet/api/System.Int32)) 省略可能な名前付きパラメーター。 Windows PowerShell コマンド内のパラメーターの位置を指定します。

`ValueFromPipeline` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 `True` コマンドレットパラメーターがパイプラインオブジェクトからの値を受け取ることを示します。 コマンドレットがオブジェクトのプロパティだけでなく、完全なオブジェクトにアクセスする場合は、このキーワードを指定します。 既定値は、`false` です。

`ValueFromPipelineByPropertyName` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 `True` コマンドレットパラメーターが、このパラメーターと同じ名前または別名を持つパイプラインオブジェクトのプロパティから値を取得することを示します。 たとえば、コマンドレットにパラメーターがあり、パイプラインオブジェクトにもプロパティがある場合、 `Name` `Name` プロパティの値 `Name` は `Name` コマンドレットのパラメーターに割り当てられます。 既定値は、`false` です。

`ValueFromRemainingArguments` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 `True` コマンドレットパラメーターが、コマンドレットに渡される残りのすべての引数を受け入れることを示します。 既定値は、`false` です。

`HelpMessage` 省略可能な名前付きパラメーター。 パラメーターの短い説明を指定します。 コマンドレットが実行され、必須パラメーターが指定されていない場合、Windows PowerShell はこのメッセージを表示します。

`HelpMessageBaseName` 省略可能な名前付きパラメーター。リソース識別子が存在する場所を指定します。 たとえば、このパラメーターは、ローカライズするヘルプメッセージを含むリソースアセンブリを指定できます。

`HelpMessageResourceId` 省略可能な名前付きパラメーター。ヘルプメッセージのリソース識別子を指定します。

## <a name="remarks"></a>解説

- この属性を宣言する方法の詳細については、「 [コマンドレットのパラメーターを宣言する方法](./how-to-declare-cmdlet-parameters.md)」を参照してください。

- コマンドレットには、任意の数のパラメーターを指定できます。 ただし、ユーザーエクスペリエンスを向上させるには、パラメーターの数を制限します。

- パラメーターは、静的なパブリックでないフィールドまたはプロパティで宣言する必要があります。 パラメーターは、プロパティで宣言する必要があります。 プロパティはパブリック set アクセサーを持つ必要があり、 `ValueFromPipeline` または `ValueFromPipelineByPropertyName` キーワードが指定されている場合、プロパティはパブリック get アクセサーを持つ必要があります。

- 位置指定パラメーターを指定する場合は、パラメーターに設定されている位置指定パラメーターの数を5未満に制限します。 また、位置指定パラメーターは連続している必要はありません。 位置5、100、および250は、位置0、1、および2と同じように動作します。

- `Position`キーワードが指定されていない場合は、コマンドレットパラメーターを名前で参照する必要があります。

- パラメーターセットを使用する場合は、次の点に注意してください。

  - 各パラメーターセットには、少なくとも1つの一意のパラメーターが必要です。 コマンドレットの設計が適切であることは、可能であれば、この一意のパラメーターも必須であることを示します。 コマンドレットをパラメーターなしで実行するように設計されている場合、unique パラメーターを必須にすることはできません。

  - パラメーターセットには、同じ位置に複数の位置パラメーターを含めることはできません。

  - パラメーターセット内の1つのパラメーターだけがを宣言する必要があり `ValueFromPipeline = true` ます。 複数のパラメーターでを定義でき `ValueFromPipelineByPropertyName = true` ます。

  - 複数のパラメーターでを定義でき `ValueFromPipelineByPropertyName = true` ます。

- パラメーター名のガイドラインの詳細については、「 [コマンドレットパラメーター名](standard-cmdlet-parameter-names-and-types.md)」を参照してください。

- Parameter 属性は、system.servicemodel [属性](/dotnet/api/System.Management.Automation.ParameterAttribute) クラスによって定義されます。

## <a name="see-also"></a>参照

[System.string. Parameterattribute](/dotnet/api/System.Management.Automation.ParameterAttribute)

[コマンドレットのパラメーター名](standard-cmdlet-parameter-names-and-types.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
