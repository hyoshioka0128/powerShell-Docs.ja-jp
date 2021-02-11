---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット内でコマンドレットとスクリプトを呼び出す
description: コマンドレット内でコマンドレットとスクリプトを呼び出す
ms.openlocfilehash: 246c61661f2d290e7e7ac62a8ad303b05bdc7582
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652647"
---
# <a name="invoking-cmdlets-and-scripts-within-a-cmdlet"></a>コマンドレット内でコマンドレットとスクリプトを呼び出す

コマンドレットは、コマンドレットの入力処理メソッド内から他のコマンドレットとスクリプトを呼び出すことができます。 これにより、コードを書き直しなくても、既存のコマンドレットとスクリプトの機能をコマンドレットに追加できます。

## <a name="the-invoke-method"></a>Invoke メソッド

すべてのコマンドレットで既存のコマンドレットを呼び出すことができます。これは、コマンドレットによってオーバーライドさ[れる、入力](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing)処理メソッド (たとえば、) から[、システムの呼び出しメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.Invoke)呼び出すことによって実行できます。 ただし、を呼び出すことが [できるのは、system.servicemodel クラスから](/dotnet/api/System.Management.Automation.Cmdlet) 直接派生したコマンドレットだけです。 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet)クラスから派生したコマンドレットを呼び出すことはできません。

System.servicemodel [*](/dotnet/api/System.Management.Automation.Cmdlet.Invoke) メソッドには、次のバリアントがあります。

System.string。この variant を[呼び出す](/dotnet/api/System.Management.Automation.Cmdlet.Invoke)と、コマンドレットオブジェクトが呼び出され、"T" 型のオブジェクトのコレクションが返されます。

Emumerator この variant を[呼び出す](/dotnet/api/System.Management.Automation.Cmdlet.Invoke)と、コマンドレットオブジェクトが呼び出され、厳密に型指定されたが返されます。 このバリアントを使用すると、ユーザーはコレクション内のオブジェクトを使用してカスタム操作を実行できます。

## <a name="examples"></a>例

|例|説明|
|-------------|-----------------|
|[コマンドレット内でのコマンドレットの呼び出し](./how-to-invoke-a-cmdlet-from-within-a-cmdlet.md)|この例は、別のコマンドレット内からコマンドレットを呼び出す方法を示しています。|
|[コマンドレット内でのスクリプトの呼び出し](./how-to-invoke-scripts-within-a-cmdlet.md)|この例では、別のコマンドレット内からコマンドレットに指定されたスクリプトを呼び出す方法を示します。|

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
