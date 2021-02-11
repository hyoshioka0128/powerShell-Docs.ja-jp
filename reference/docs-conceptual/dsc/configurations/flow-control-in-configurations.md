---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: 構成内での条件付きステートメントとループ
description: この記事では、条件ステートメントとループを使って Configuration をより動的にする方法を説明します。 条件ステートメントとループをパラメーターおよび構成データと組み合わせることで、Configuration をコンパイルするときの柔軟性と制御が増します。
ms.openlocfilehash: 7af8a360c17a0842fa2b95d1d1fb288323c327ef
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92658465"
---
# <a name="conditional-statements-and-loops-in-a-configuration"></a>Configuration 内での条件付きステートメントとループ

PowerShell のフロー制御キーワードを使って、[Configuration](configurations.md) をより動的にすることができます。 この記事では、条件ステートメントとループを使って `Configuration` をより動的にする方法を説明します。 条件ステートメントとループを[パラメーター](add-parameters-to-a-configuration.md)および[構成データ](configData.md)と組み合わせることで、`Configuration` をコンパイルするときの柔軟性と制御が増します。

関数またはスクリプト ブロックと同じように、`Configuration` 内で任意の PowerShell 言語の機能を使用できます。 使用するステートメントは、`.mof` ファイルをコンパイルするために `Configuration` を呼び出すときにのみ評価されます。 次の例では、シナリオを使って概念を説明します。 条件ステートメントとループは、パラメーターや構成データと共に使われることがよくあります。

この例では、 **Service** リソース ブロックでコンパイル時にサービスの現在の状態を取得し、現在の状態を保持する `.mof` ファイルを生成します。

> [!NOTE]
> 動的な Resource ブロックを使うと、IntelliSense の有効性は失われます。 PowerShell のパーサーでは、`Configuration` がコンパイルされるまで、指定された値が許容されるかどうかを判断できません。

```powershell
Configuration ServiceState
{
    # It is best practice to explicitly import any resources used in your Configurations.
    Import-DSCResource -Name Service -Module PSDesiredStateConfiguration
    Node localhost
    {
        Service Spooler
        {
            Name = "Spooler"
            State = $(Get-Service -Name 'spooler').Status
            StartType = $(Get-Service -Name 'spooler').StartType
        }
    }
}
```

さらに、`foreach` ループを使って、現在のコンピューター上のすべてのサービスに対して **Service** リソース ブロックを作成できます。

```powershell
Configuration ServiceState
{
    # It is best practice to explicitly import any resources used in your Configurations.
    Import-DSCResource -Name Service -Module PSDesiredStateConfiguration
    Node localhost
    {
        foreach ($service in $(Get-Service))
        {
            Service $service.Name
            {
                Name = $service.Name
                State = $service.Status
                StartType = $service.StartType
            }
        }
    }
}
```

また、`if` ステートメントを使用してオンラインになっているコンピューターに対してのみ `Configuration` を作成することもできます。

```powershell
Configuration ServiceState
{
    # It is best practice to explicitly import any resources used in your Configurations.
    Import-DSCResource -Name Service -Module PSDesiredStateConfiguration

    foreach ($computer in @('Server01', 'Server02', 'Server03'))
    {
        if (Test-Connection -ComputerName $computer)
        {
            Node $computer
            {
                Service "Spooler"
                {
                    Name = "Spooler"
                    State = "Started"
                }
            }
        }
    }
}
```

> [!NOTE]
> 上の例の動的なリソース ブロックでは、現在のコンピューターが参照されています。 このインスタンスでは、それはターゲット ノードではなく、`Configuration` を作成しているコンピューターです。

<!---
Mention Get-DSCConfigurationFromSystem
-->

## <a name="summary"></a>まとめ

まとめると、`Configuration` 内では任意の PowerShell 言語の機能を使うことができます。

これには次のものが含まれます。

- カスタム オブジェクト
- ハッシュテーブル
- 文字列操作
- リモート処理
- WMI と CIM
- ActiveDirectory オブジェクト
- その他...

`Configuration` で定義されているすべての PowerShell コードはコンパイル時に評価されますが、`Configuration` を含むスクリプトにコードを配置することもできます。 `Configuration` ブロックの外部にあるすべてのコードは、`Configuration` をインポートするときに実行されます。

## <a name="see-also"></a>関連項目

- [構成にパラメーターを追加する](add-parameters-to-a-configuration.md)
- [構成から構成データを分離する](configData.md)
