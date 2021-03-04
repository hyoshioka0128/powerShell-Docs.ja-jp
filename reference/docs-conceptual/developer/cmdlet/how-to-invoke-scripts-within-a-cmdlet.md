---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット内でスクリプトを呼び出す方法
description: コマンドレット内でスクリプトを呼び出す方法
ms.openlocfilehash: 503ecb8913fe61ef3f5ec6fe969c22c2319a4f12
ms.sourcegitcommit: 1dfd5554b70c7e8f4e3df19e29c384a9c0a4b227
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101685344"
---
# <a name="how-to-invoke-scripts-within-a-cmdlet"></a>コマンドレット内でスクリプトを呼び出す方法

この例は、コマンドレットに指定されたスクリプトを呼び出す方法を示しています。 スクリプトはコマンドレットによって実行され、その結果は、一連の [システム管理](/dotnet/api/System.Management.Automation.PSObject) オブジェクトのコレクションとしてコマンドレットに返されます。

## <a name="to-invoke-a-script-block"></a>スクリプトブロックを呼び出すには

1. コマンドは、スクリプトブロックがコマンドレットに渡されたことを確認します。 スクリプトブロックが指定されている場合、コマンドは、必要なパラメーターを指定してスクリプトブロックを呼び出します。

    ```csharp
    if (script != null)
    {
      WriteDebug("Executing script block.");

      // Invoke the script block with the required arguments.
      Collection<PSObject> PSObjects =
                     script.Invoke(
                                   line,
                                   simpleMatch,
                                   caseSensitive
                                  );
    ```

2. 次に、返された一連の [システム](/dotnet/api/System.Management.Automation.PSObject) オブジェクトを反復処理し、必要な操作を実行します。

    ```csharp
    foreach (PSObject psObject in psObjects)
    {
      if (LanguagePrimitives.IsTrue(psObject))
      {
        result = new MatchInfo();
        result.Line = line;
        result.IgnoreCase = !caseSensitive;

        break;
      }
    }

    ```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
