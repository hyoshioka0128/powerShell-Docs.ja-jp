---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットからの確認を要求する
description: コマンドレットからの確認を要求する
ms.openlocfilehash: fd869d50b185cb4d38269640df58ec284a32da50
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646409"
---
# <a name="requesting-confirmation-from-cmdlets"></a>コマンドレットからの確認を要求する

コマンドレットは、Windows PowerShell 環境の外部にあるシステムを変更しようとしているときに確認を要求する必要があります。 たとえば、コマンドレットを使用してユーザーアカウントを追加しようとしたり、プロセスを停止したりする場合は、コマンドレットを実行する前に、ユーザーからの確認が必要になります。 これに対して、コマンドレットが Windows PowerShell 変数を変更しようとしている場合、コマンドレットで確認が必要になることはありません。

確認要求を行うには、コマンドレットで確認要求がサポートされていることを示す必要があります。また、確認要求メッセージを表示するに[は、コマンドレットと](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)[システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)の実行 (オプション) メソッドを呼び出す必要があります。

## <a name="supporting-confirmation-requests"></a>確認要求のサポート

確認要求をサポートするには、コマンドレットで `SupportsShouldProcess` コマンドレット属性のパラメーターをに設定する必要があり `true` ます。 これにより `Confirm` 、 `WhatIf` Windows PowerShell によって提供されるおよびコマンドレットパラメーターが有効になります。 `Confirm`パラメーターを使用すると、確認要求を表示するかどうかをユーザーが制御できます。 `WhatIf`パラメーターを使用すると、コマンドレットでメッセージを表示するかどうか、またはそのアクションを実行するかどうかを指定できます。 `Confirm`コマンドレットには、パラメーターとパラメーターを手動で追加しないでください `WhatIf` 。

次の例は、確認要求をサポートするコマンドレット属性の宣言を示しています。

```csharp
[Cmdlet(VerbsDiagnostic.Test, "RequestConfirmationTemplate1",
        SupportsShouldProcess = true)]
```

## <a name="calling-the-confirmation-request-methods"></a>確認要求メソッドの呼び出し

コマンドレットのコードで、システムを変更する操作を実行する前に、 [system.string メソッドを呼び出します。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) 呼び出しがの値を返す場合、操作は実行されず、コマンドレットは次の操作を処理するようにコマンドレットを設計 `false` します。

## <a name="calling-the-shouldcontinue-method"></a>"メソッドの続行" メソッドの呼び出し

ほとんどのコマンドレットでは、 [System.](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) ....................... ただし、場合によっては、追加の確認が必要になる場合があります。 このような場合は、 [system.](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) [..](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) ....................................... これにより、コマンドレットまたはプロバイダーは、確認プロンプトに対する **[はい] のすべて** の応答のスコープをより細かく制御できます。

コマンドレットで[system.servicemodel メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)呼び出す場合は、コマンドレットでスイッチパラメーターも指定する必要があります。 `Force` ユーザーがコマンドレットを呼び出したときにユーザーが指定した場合 `Force` 、 [](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)コマンドレットは引き続き system.object を呼び出す必要がありますが、このコマンドレットの呼び出しをバイパスする必要があり[ます。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)

ユーザーにプロンプトが表示されない非対話型環境から呼び出された場合は、例外が[スローされ](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)ます ()。 パラメーターを追加する `Force` と、非対話型環境で呼び出された場合でも、コマンドを実行できるようになります。

次の例では、.............................. コマンド[レット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue)[を呼び出す方法](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)を示します。

```csharp
if (ShouldProcess (...) )
{
  if (Force || ShouldContinue(...))
  {
     // Add code that performs the operation.
  }
}
```

コマンドレットの呼び出しの動作は、コマンドレットが呼び出される環境によって [異なる場合があります](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) 。 前のガイドラインを使用すると、ホスト環境に関係なく、コマンドレットを他のコマンドレットと一貫して動作させることができます。

System.servicemodel メソッドの呼び出しの例については、「確認[を要求する方法](./how-to-request-confirmations.md)」を参照して[ください。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)

## <a name="specify-the-impact-level"></a>影響レベルを指定します

コマンドレットを作成するときに、変更の影響レベル (重大度) を指定します。 これを行うには、 `ConfirmImpact` コマンドレット属性のパラメーターの値を High、Medium、または Low に設定します。 の値は、 `ConfirmImpact` コマンドレットのパラメーターも指定する場合にのみ指定でき `SupportsShouldProcess` ます。

ほとんどのコマンドレットでは、を明示的に指定する必要はありません `ConfirmImpact` 。  代わりに、[中] のパラメーターの既定の設定を使用します。 `ConfirmImpact`[高] に設定すると、既定で操作が確認されます。 ハードディスクボリュームの再フォーマットなど、非常に破壊的な操作を行う場合は、この設定を予約します。

## <a name="calling-non-confirmation-methods"></a>確認以外のメソッドの呼び出し

コマンドレットまたはプロバイダーがメッセージを送信する必要があるが、確認を要求しない場合は、次の3つのメソッドを呼び出すことができます。 [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject)メソッドを使用してこれらの型のメッセージを送信するのは避けてください。 [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject)出力は、コマンドレットまたはプロバイダーの通常の出力と混在するため、スクリプトの書き込みが困難になります。

- ユーザーに注意して操作を続行するには、コマンドレットまたはプロバイダー [が、system.string メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.WriteWarning) 呼び出すことができます。

- パラメーターを使用してユーザーが取得できる追加情報を提供するために、 `Verbose` コマンドレットまたはプロバイダー [は、system.string](/dotnet/api/System.Management.Automation.Cmdlet.WriteVerbose) メソッドを呼び出すことができます。

- 他の開発者や製品サポートに関するデバッグレベルの詳細を提供するために、コマンドレットまたはプロバイダーは、system.servicemodel [デバッグ](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug) メソッドを呼び出すことができます。 ユーザーは、パラメーターを使用してこの情報を取得でき `Debug` ます。

コマンドレットとプロバイダーは、まず次のメソッドを呼び出して、Windows PowerShell の外部でシステムを変更する操作の実行を試みる前に、確認を要求します。

- [System.... コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)

- [システムの管理..........](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)

これを行うには、ユーザーがコマンドを呼び出した方法に基づいて操作を確認するように求めるメッセージを表示して、[このメソッドを呼び出します。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
