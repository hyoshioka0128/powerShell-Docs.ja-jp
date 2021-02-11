---
ms.date: 09/13/2016
ms.topic: reference
title: 実行空間のサンプル
description: 実行空間のサンプル
ms.openlocfilehash: 0171622f3ade3b341bc226f14398d6d293262f0c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657945"
---
# <a name="runspace-samples"></a>実行空間のサンプル

このセクションには、さまざまな種類の実行空間を使用してコマンドを同期および非同期で実行する方法を示すサンプルコードが含まれています。 Microsoft Visual Studio を使用してコンソールアプリケーションを作成し、このセクションのトピックからホストアプリケーションにコードをコピーすることができます。

## <a name="in-this-section"></a>このセクションの内容

> [!NOTE]
> カスタムホストインターフェイスを作成するホストアプリケーションのサンプルについては、「 [カスタムホストのサンプル](./custom-host-samples.md)」を参照してください。

 [Runspace01 サンプル](./runspace01-sample.md) このサンプルでは、 [System. Powershell](/dotnet/api/system.management.automation.powershell) クラスを使用して、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) コマンドレットを同期的に実行し、出力をコンソールウィンドウに表示する方法を示します。

 [Runspace02 サンプル](./runspace02-sample.md) このサンプル [では、system.servicemodel クラスを](/dotnet/api/system.management.automation.powershell) 使用して、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) および [Sort-Object](/powershell/module/Microsoft.PowerShell.Utility/Sort-Object) コマンドレットを同期的に実行する方法を示します。 これらのコマンドの結果 [は、system.string コントロールを](/dotnet/api/System.Windows.Forms.DataGridView) 使用して表示されます。

 [Runspace03 サンプル](./runspace03-sample.md) このサンプルでは、 [System. Powershell](/dotnet/api/system.management.automation.powershell) クラスを使用してスクリプトを同期的に実行する方法と、終了しないエラーを処理する方法を示します。 このスクリプトはプロセス名の一覧を受信し、これらのプロセスを取得します。 スクリプトの実行時に生成された終了しないエラーを含む、スクリプトの結果がコンソール ウィンドウに表示されます。

 [Runspace04 サンプル](./runspace04-sample.md) このサンプルでは、 [Powershell](/dotnet/api/system.management.automation.powershell) クラスを使用してコマンドを実行する方法と、コマンドの実行時にスローされる終了エラーをキャッチする方法を示します。 2 つのコマンドが実行され、最後のコマンドには無効なパラメーターの引数が渡されます。 その結果、オブジェクトは返されず、終了エラーがスローされます。

 [Runspace05 サンプル](./runspace05-sample.md) このサンプルでは、 [ tialsessionstate オブジェクトSystem.Management.Automation.Runspaces.Ini](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) にスナップインを追加して、実行空間を開いたときにスナップインのコマンドレットを使用できるようにする方法を示します。 スナップインには、 [GetProcessSample01 サンプル](../cmdlet/getprocesssample01-sample.md)で定義されている Get-Proc コマンドレットが用意されています。このコマンドレット [は、オブジェクトを](/dotnet/api/system.management.automation.powershell) 使用して同期的に実行されます。

 [Runspace06 サンプル](./runspace06-sample.md) このサンプルでは、 [ tialsessionstate オブジェクトSystem.Management.Automation.Runspaces.Ini](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) にモジュールを追加して、実行空間を開いたときにモジュールが読み込まれるようにする方法を示します。 モジュールには、 [GetProcessSample02 のサンプル](../cmdlet/getprocesssample02-sample.md)で定義されている Get-Proc コマンドレットが用意されています。このコマンドレットは、 [オブジェクトを](/dotnet/api/system.management.automation.powershell) 使用して同期的に実行されます。

 [Runspace07 サンプル](./runspace07-sample.md) このサンプルでは、実行空間を作成し、その実行空間を使用して、 [複数のコマンド](/dotnet/api/system.management.automation.powershell) レットを同期的に実行する方法を示します。

 [Runspace08 サンプル](./runspace08-sample.md) このサンプルでは、コマンドと引数を、 [システムの管理](/dotnet/api/system.management.automation.powershell) オブジェクトのパイプラインに追加する方法と、コマンドを同期的に実行する方法を示します。

 [Runspace09 サンプル](./runspace09-sample.md) このサンプルでは、スクリプトを [システム](/dotnet/api/system.management.automation.powershell) のパイプラインに追加する方法と、スクリプトを非同期で実行する方法を示します。 イベントはスクリプトの出力を処理するために使用されます。

 [Runspace10 サンプル](./runspace10-sample.md)このサンプルでは、既定の初期セッション状態を作成する方法、コマンドレットを[System.Management.Automation.Runspaces.Initialsessionstate](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState)に追加する方法、初期セッション状態を使用する実行空間を作成する方法、およびオブジェクトを使用してコマンドを実行する方法を示し[ます。](/dotnet/api/system.management.automation.powershell)

 [Runspace11 サンプル](./runspace11-sample.md) これは、既存のコマンドレットを呼び出すが、使用可能なパラメーターのセットを制限するプロキシコマンドを作成するために、 [system.object クラスを](/dotnet/api/System.Management.Automation.ProxyCommand) 使用する方法を示しています。 このプロキシ コマンドは、制約付き実行空間の作成に使用される最初のセッション状態に追加されます。 つまり、ユーザーはプロキシ コマンドを使用しないとコマンドレットの機能にアクセスできません。

## <a name="see-also"></a>関連項目
