---
ms.date: 09/13/2016
ms.topic: reference
title: エラー情報を表示する
description: エラー情報を表示する
ms.openlocfilehash: 37a3adb91d0e616a5c7f27bcab866f8da139f969
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653051"
---
# <a name="displaying-error-information"></a>エラー情報を表示する

このトピックでは、ユーザーがエラー情報を表示する方法について説明します。

コマンドレットでエラーが発生した場合、既定では、エラー情報の表示は次のようなエラー出力になります。

```powershell
$ stop-service lanmanworkstation
You do not have sufficient permissions to stop the service Workstation.
```

ただし、ユーザーは変数をに設定することによって、エラーをカテゴリ別に表示でき `$ErrorView` `"CategoryView"` ます。 カテゴリビューでは、エラーの詳細な説明ではなく、エラーレコードからの特定の情報が表示されます。 このビューは、スキャンするエラーの一覧が長い場合に便利です。 カテゴリビューでは、前のエラーメッセージが次のように表示されます。

```powershell
$ $ErrorView = "CategoryView"
$ stop-service lanmanworkstation
CloseError: (System.ServiceProcess.ServiceController:ServiceController) [stop-service], ServiceCommandException
```

エラーカテゴリの詳細については、「 [Windows PowerShell エラーレコード](./windows-powershell-error-records.md)」を参照してください。

## <a name="see-also"></a>参照

[Windows PowerShell エラー レコード](./windows-powershell-error-records.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
