---
ms.date: 12/12/2018
keywords: dsc,powershell,リソース,ギャラリー,セットアップ
title: 構成にパラメーターを追加する
description: DSC 構成をパラメーター化し、ユーザー入力に基づいて構成をいっそう動的にすることができます。
ms.openlocfilehash: 72f3cf9efb5d99170e71992bed86a20a57132250
ms.sourcegitcommit: 62282bb9c36fea3b4290b9263c1cd8e9ac216e29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96470334"
---
# <a name="add-parameters-to-a-configuration"></a>構成にパラメーターを追加する

関数と同様に、[構成](configurations.md)もパラメーター化し、ユーザー入力に基づいて構成をいっそう動的にすることができます。 手順は、「[Functions with Parameters (パラメーターを使用する関数)](/powershell/module/microsoft.powershell.core/about/about_functions)」で説明されているものと似ています。

この例では、"スプーラー" サービスを "実行中" に構成する基本的な構成を使用します。

```powershell
Configuration TestConfig
{
    # It is best practice to explicitly import any required resources or modules.
    Import-DSCResource -Module PSDesiredStateConfiguration

    Node localhost
    {
        Service 'Spooler'
        {
            Name = 'Spooler'
            State = 'Running'
        }
    }
}
```

## <a name="built-in-configuration-parameters"></a>組み込み構成パラメーター

関数とは異なり、[CmdletBinding](/powershell/module/microsoft.powershell.core/about/about_functions_cmdletbindingattribute) 属性では機能は追加されません。 [共通パラメーター](/powershell/module/microsoft.powershell.core/about/about_commonparameters)に加えて、構成では次の組み込みパラメーターを定義せずに使用できます。

|        パラメーター        |                                         説明                                          |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| `-InstanceName`         | [複合構成](compositeconfigs.md)の定義で使用されます                             |
| `-DependsOn`            | [複合構成](compositeconfigs.md)の定義で使用されます                             |
| `-PSDSCRunAsCredential` | [複合構成](compositeconfigs.md)の定義で使用されます                             |
| `-ConfigurationData`    | 構成で使用する構造化[構成データ](configData.md)を渡すために使用されます。 |
| `-OutputPath`           | "\<computername\>.mof" ファイルがコンパイルされる場所を指定するために使用されます                      |

## <a name="adding-your-own-parameters-to-configurations"></a>構成に独自のパラメーターを追加する

組み込みパラメーターに加えて、独自のパラメーターを構成に追加することもできます。
関数と同様、パラメーター ブロックを構成の宣言内で直接指定します。 構成のパラメーター ブロックは、すべての **ノード** 宣言の外部で、すべての "*インポート*" ステートメントより上に置く必要があります。 パラメーターを追加することにより、構成をいっそう堅牢で動的にすることができます。

```powershell
Configuration TestConfig
{
    param
    (

    )
```

### <a name="add-a-computername-parameter"></a>ComputerName パラメーターを追加する

追加する可能性がある最初のパラメーターは `-Computername` で、構成に渡した `-Computername` に対して ".mof" ファイルを動的にコンパイルできるようにします。 関数と同様、ユーザーが `-ComputerName` に値を渡さない場合のため、既定値を定義することもできます

```powershell
param
(
    [String]
    $ComputerName="localhost"
)
```

構成内では、Node ブロックを定義するときに独自の `-ComputerName` パラメーターを指定できます。

```powershell
Node $ComputerName
{

}
```

### <a name="calling-your-configuration-with-parameters"></a>パラメーターを持つ構成を呼び出す

構成にパラメーターを追加した後は、コマンドレットの場合と同じように使用できます。

```powershell
TestConfig -ComputerName "server01"
```

### <a name="compiling-multiple-mof-files"></a>複数の .mof ファイルをコンパイルする

Node ブロックでは、コンマ区切りのコンピューター名のリストを受け取ることもでき、それぞれに対して ".mof" ファイルが生成されます。 次の例を実行し、`-ComputerName` パラメーターに渡されるすべてのコンピューターに対して ".mof" ファイルを生成できます。

```powershell
Configuration TestConfig
{
    param
    (
        [String[]]
        $ComputerName="localhost"
    )

    # It is best practice to explicitly import any required resources or modules.
    Import-DSCResource -Module PSDesiredStateConfiguration

    Node $ComputerName
    {
        Service 'Spooler'
        {
            Name = 'Spooler'
            State = 'Running'
        }
    }
}

TestConfig -ComputerName "server01", "server02", "server03"
```

## <a name="advanced-parameters-in-configurations"></a>構成の高度なパラメーター

`-ComputerName` パラメーターに加えて、サービス名と状態のパラメーターを追加することができます。
次の例では、`-ServiceName` パラメーターでパラメーター ブロックを追加し、それを使用して **Service** リソース ブロックを動的に定義しています。 また、`-State` パラメーターを追加し、**Service** リソース ブロック内で **State** を動的に定義しています。

```powershell
Configuration TestConfig
{
    param
    (
        [String]
        $ServiceName,

        [String]
        $State,

        [String]
        $ComputerName="localhost"
    )

    # It is best practice to explicitly import any required resources or modules.
    Import-DSCResource -Module PSDesiredStateConfiguration

    Node $ComputerName
    {
        Service $ServiceName
        {
            Name = $ServiceName
            State = $State
        }
    }
}
```

> [!NOTE]
> より高度なシナリオでは、動的データを構造化された[構成データ](configData.md)に移動した方が合理的な場合があります。

現在の例の構成では、動的な `$ServiceName` を受け取っていますが、指定されないと、コンパイル エラーが発生します。 次の例のように、既定値を追加できます。

```powershell
[String]
$ServiceName="Spooler"
```

ただし、この場合は、`$ServiceName` パラメーターの値の指定をユーザーに単に強制する方が適切です。 `parameter` 属性を使用すると、構成のパラメーターにさらに検証とパイプラインのサポートを追加できます。

次の例のように、すべてのパラメーター宣言の上に `parameter` 属性ブロックを追加します。

```powershell
[parameter()]
[String]
$ServiceName
```

各 `parameter` 属性に対して引数を指定し、定義されているパラメーターの要素を制御できます。 次の例では、`$ServiceName` を **必須** パラメーターにしています。

```powershell
[parameter(Mandatory)]
[String]
$ServiceName
```

`$State` パラメーターでは、事前に定義されているセット (Running、Stopped など) 以外の値をユーザーが指定するのを防ぐため、`ValidationSet*` 属性でユーザーがそのような値を指定できないようにします。 次の例では、`ValidationSet` 属性を `$State` パラメーターに追加しています。 `$State` パラメーターを **必須** にはしたくないので、既定値を追加する必要があります。

```powershell
[ValidateSet("Running", "Stopped")]
[String]
$State="Running"
```

> [!NOTE]
> `validation` 属性を使用するときは、`parameter` 属性を指定する必要はありません。

`parameter` および検証属性について詳しくは、[関数の高度なパラメーター](/powershell/module/microsoft.powershell.core/about/about_Functions_Advanced_Parameters)に関する記事をご覧ください。

## <a name="fully-parameterized-configuration"></a>完全にパラメーター化された構成

ユーザーに `-InstanceName` と `-ServiceName` の指定を強制し、`-State` パラメーターを検証する、パラメーター化された構成ができました。

```powershell
Configuration TestConfig
{
    param
    (
        [parameter(Mandatory)]
        [String]
        $ServiceName,

        [ValidateSet("Running","Stopped")]
        [String]
        $State="Running",

        [String]
        $ComputerName="localhost"
    )

    # It is best practice to explicitly import any required resources or modules.
    Import-DSCResource -Module PSDesiredStateConfiguration

    Node $ComputerName
    {
        Service $ServiceName
        {
            Name = $ServiceName
            State = $State
        }
    }
}
```

## <a name="see-also"></a>関連項目

- [DSC 構成のヘルプを作成する](configHelp.md)
- [動的な構成](flow-control-in-configurations.md)
- [構成で構成データを使用する](configData.md)
- [構成と環境データを分離する](separatingEnvData.md)
