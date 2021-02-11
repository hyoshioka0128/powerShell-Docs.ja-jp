---
description: 関数をコンパイル済みのコマンドレットのように機能させるための属性について説明します。
Locale: en-US
ms.date: 06/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions_cmdletbindingattribute?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Functions_CmdletBindingAttribute
ms.openlocfilehash: f1463bafc462ae2a266f5b1f6f4d71e3422f5831
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603243"
---
# <a name="about-functions-cmdletbindingattribute"></a>関数のバージョン情報の属性

## <a name="short-description"></a>簡単な説明
関数をコンパイル済みのコマンドレットのように機能させるための属性について説明します。

## <a name="long-description"></a>長い説明

`CmdletBinding`属性は、C# で記述されたコンパイル済みコマンドレットのように動作する関数の属性です。 コマンドレットの機能へのアクセスを提供します。

PowerShell は、コンパイルされた `CmdletBinding` コマンドレットのパラメーターをバインドするのと同じ方法で、属性を持つ関数のパラメーターをバインドします。 `$PSCmdlet`属性を持つ関数では自動変数を使用できますが、 `CmdletBinding` 変数は使用でき `$Args` ません。

属性を持つ関数では `CmdletBinding` 、位置パラメーターが一致しない不明なパラメーターおよび位置指定引数によって、パラメーターのバインドが失敗します。

> [!NOTE]
> コンパイルされたコマンドレットは required 属性を使用し `Cmdlet` `CmdletBinding` ます。これは、このトピックで説明されている属性に似ています。

## <a name="syntax"></a>構文

次の例は、属性のすべての省略可能な引数を指定する関数の形式を示して `CmdletBinding` います。 各引数の簡単な説明を次に示します。

```powershell
{
    [CmdletBinding(ConfirmImpact=<String>,
    DefaultParameterSetName=<String>,
    HelpURI=<URI>,
    SupportsPaging=<Boolean>,
    SupportsShouldProcess=<Boolean>,
    PositionalBinding=<Boolean>)]

    Param ($Parameter1)
    Begin{}
    Process{}
    End{}
}
```

## <a name="confirmimpact"></a>ConfirmImpact

**Confirmimpact** **引数は、メソッドの** 呼び出しによって関数のアクションを確認するタイミングを指定します。 値を指定する **メソッドを** 呼び出すと、 **confirmimpact** 引数がユーザー設定変数の値以上の場合にのみ、確認プロンプトが表示され `$ConfirmPreference` ます。 (引数の既定値は **Medium** です)。この引数は、 **supports指定** の引数も指定されている場合にのみ指定します。

確認要求の詳細については、「 [確認の要求](/powershell/scripting/developer/cmdlet/requesting-confirmation)」を参照してください。

## <a name="defaultparametersetname"></a>DefaultParameterSetName

**DefaultParameterSetName** 引数には、使用するパラメーターセットを決定できない場合に PowerShell が使用するパラメーターセットの名前を指定します。 この問題を回避するには、各パラメーターの unique パラメーターに必須パラメーターを設定します。

## <a name="helpuri"></a>HelpURI

この **引数は、関数** について説明するヘルプトピックのオンラインバージョンのインターネットアドレスを指定します。 パラメーターの値は **、"** http" または "https" で始まる必要があります。

パラメーター **の値は、** 関数に対してを返す **commandinfo** オブジェクト **のプロパティの** 値に使用され `Get-Command` ます。

ただし、ヘルプファイルがコンピューターにインストールされていて、ヘルプファイルの [関連 **リンク**] セクションの最初のリンクの値が uri である場合、またはコメントベースのヘルプの最初のディレクティブの値が uri である場合は、 `.Link` ヘルプファイル内の uri が関数のすべてのプロパティの値として使用されます。

コマンドレットでは、 `Get-Help` コマンドでの **online** パラメーターが指定されている場合に、関数ヘルプトピックのオンラインバージョンを検索するために、の値を使用し `Get-Help` ます。

## <a name="supportspaging"></a>SupportsPaging

**Supportspaging** 引数は、関数に **First**、 **Skip**、および **includetotalcount** パラメーターを追加します。 これらのパラメーターを使用すると、ユーザーは非常に大きな結果セットから出力を選択できます。 この引数は、SQL データベースなどのデータ選択をサポートする大規模なデータストアからデータを返すコマンドレットと関数用に設計されています。

この引数は、Windows PowerShell 3.0 で導入されました。

- **First**: 最初の ' n ' 個のオブジェクトのみを取得します。
- **Skip**: 最初の ' n ' 個のオブジェクトを無視し、残りのオブジェクトを取得します。
- **Includetotalcount**: データセット内のオブジェクト (整数) の後にオブジェクトの数を報告します。 コマンドレットが合計数を判別できない場合は、"Unknown total count" を返します。

PowerShell には **Newtotalcount** が含まれています。これは、返される合計数の値を取得するヘルパーメソッドで、合計数の値の推定精度を含みます。

次のサンプル関数は、ページングパラメーターのサポートを高度な関数に追加する方法を示しています。

```powershell
function Get-Numbers {
    [CmdletBinding(SupportsPaging = $true)]
    param()

    $FirstNumber = [Math]::Min($PSCmdlet.PagingParameters.Skip, 100)
    $LastNumber = [Math]::Min($PSCmdlet.PagingParameters.First +
      $FirstNumber - 1, 100)

    if ($PSCmdlet.PagingParameters.IncludeTotalCount) {
        $TotalCountAccuracy = 1.0
        $TotalCount = $PSCmdlet.PagingParameters.NewTotalCount(100,
          $TotalCountAccuracy)
        Write-Output $TotalCount
    }
    $FirstNumber .. $LastNumber | Write-Output
}
```

## <a name="supportsshouldprocess"></a>SupportsShouldProcess

**Supports指定処理** 引数は、 **Confirm** パラメーターと **WhatIf** パラメーターを関数に追加します。 **Confirm** パラメーターは、パイプライン内の各オブジェクトに対してコマンドを実行する前に、ユーザーにプロンプトを表示します。 **WhatIf** パラメーターは、コマンドを実行するのではなく、コマンドによって実行される変更を一覧表示します。

## <a name="positionalbinding"></a>PositionalBinding

**Positionalbinding** 引数は、関数内のパラメーターが既定で位置指定されているかどうかを判断します。 既定値は `$True` です。 位置指定バインドを無効にするには、 **Positionalbinding** 引数を値と共に使用し `$False` ます。

**Positionalbinding** 引数は、Windows PowerShell 3.0 で導入されました。

パラメーターが位置指定の場合、パラメーター名は省略可能です。
PowerShell では、関数コマンド内の名前のないパラメーター値の順序や位置に従って、名前のないパラメーター値が関数パラメーターに関連付けられます。

パラメーターが位置指定されていない場合 ("名前付き" の場合)、コマンドにパラメーター名 (または名前の省略形または別名) が必要です。

**Positionalbinding** がの場合 `$True` 、関数のパラメーターは既定で位置指定されます。 PowerShell では、関数内で宣言されている順序で、パラメーターに位置番号が割り当てられます。

**Positionalbinding** がの場合 `$False` 、関数のパラメーターは既定では位置指定されません。 **パラメーター属性の** **Position** 引数がパラメーターで宣言されている場合を除き、パラメーターを関数で使用するときは、パラメーター名 (または別名または省略形) を含める必要があります。

**Parameter** 属性の **Position** 引数は、 **positionalbinding** の既定値よりも優先されます。 **Position** 引数を使用して、パラメーターの位置の値を指定できます。 **Position** 引数の詳細については、「 [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)」を参照してください。

## <a name="notes"></a>メモ

**SupportsTransactions** 引数は、高度な関数ではサポートされていません。

## <a name="keywords"></a>Keywords

about_Functions_CmdletBinding_Attribute

## <a name="see-also"></a>関連項目

[about_Functions](about_Functions.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_Advanced_Methods](about_Functions_Advanced_Methods.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Functions_OutputTypeAttribute](about_Functions_OutputTypeAttribute.md)
