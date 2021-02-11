---
ms.date: 09/13/2016
ms.topic: reference
title: トランザクションをサポートする方法
description: トランザクションをサポートする方法
ms.openlocfilehash: 5691c246830dab9f4808801c04353ebfb2c3e981
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666962"
---
# <a name="how-to-support-transactions"></a>トランザクションをサポートする方法

この例は、コマンドレットにトランザクションのサポートを追加する基本的なコード要素を示しています。

> [!IMPORTANT]
> Windows PowerShell によるトランザクションの処理方法の詳細については、「 [トランザクションについ][about_Transactions]て」を参照してください。

## <a name="to-support-transactions"></a>トランザクションをサポートするには

1. コマンドレットの属性を宣言する場合は、コマンドレットがトランザクションをサポートするように指定します。
   コマンドレットでトランザクションがサポートされている場合は、実行時に Windows PowerShell によって `UseTransaction` コマンドレットにパラメーターが追加されます。

    ```csharp
    [Cmdlet(VerbsCommunications.Send, "GreetingTx",
            SupportsTransactions=true )]
    ```

2. 入力処理メソッドの1つで、ブロックを追加して、 `if` トランザクションが使用可能かどうかを確認します。
   ステートメントが `if` に解決される場合 `true` 、このステートメント内のアクションは、現在のトランザクションのコンテキスト内で実行できます。

    ```csharp
    if (TransactionAvailable())
    {
      using (CurrentPSTransaction)
      {
        WriteObject("Hello " + name + "  from within a transaction.");
      }
    }
    ```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)

<!-- External URLs -->

[about_Transactions]: /powershell/module/Microsoft.PowerShell.Core/About/about_Transactions
