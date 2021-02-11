---
ms.date: 09/13/2016
ms.topic: reference
title: ユーザーの確認要求
description: ユーザーの確認要求
ms.openlocfilehash: 58dbe27635ca38886b728f585fec063645b3597e
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646301"
---
# <a name="users-requesting-confirmation"></a>ユーザーの確認要求

コマンド `true` レットの属性宣言のパラメーターに値を指定すると `SupportsShouldProcess` 、ユーザーは `Confirm` コマンドプロンプトでパラメーターを指定できます。

既定の環境では、ユーザーはパラメーターを指定することもできます。 `Confirm` `"-Confirm:$true` これにより、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) のメソッドが呼び出されたときに確認が要求されます。 これは、システムの管理をバイパス [します。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) 影響の大きい操作の場合でも、確認要求を処理します。

が指定されていない場合は、ユーザー `Confirm` [](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) `$ConfirmPreference` 設定変数が `ConfirmImpact` コマンドレットまたはプロバイダーの設定以上であるかどうかの確認を求めるメッセージが、システムによって呼び出されます。 の既定の設定 `$ConfirmPreference` は High です。 そのため、既定の環境では、影響の大きいアクション要求の確認を指定するコマンドレットとプロバイダーのみが対象となります。

が false の場合、またはが指定されている場合は、 `Confirm` `"-Confirm:$false` ユーザーからの確認要求が呼び出され、 [](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) `$ConfirmPreference` シェル変数は無視されます。

## <a name="remarks"></a>解説

- を指定するコマンドレットとプロバイダーについては、 `SupportsShouldProcess` `ConfirmImpact` これらのアクションは "中程度の影響" アクションとして処理され、既定ではプロンプトは表示されません。 影響レベルは、ユーザー設定変数の既定の設定よりも小さくなってい `$ConfirmPreference` ます。

- ユーザーがパラメーターを指定すると `Verbose` 、確認メッセージが表示されない場合でも、操作が通知されます。

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
