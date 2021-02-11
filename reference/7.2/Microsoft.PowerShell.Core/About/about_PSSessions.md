---
description: PowerShell セッション (PSSessions) について説明し、リモートコンピューターへの永続的な接続を確立する方法について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pssessions?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PSSessions
ms.openlocfilehash: edc7109f3f79376ac1d348d3c3cd65e3a8d3e69c
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602751"
---
# <a name="about-pssessions"></a>PSSessions について

## <a name="short-description"></a>簡単な説明
PowerShell セッション (PSSessions) について説明し、リモートコンピューターへの永続的な接続を確立する方法について説明します。

## <a name="long-description"></a>長い説明

リモートコンピューターで PowerShell コマンドを実行するには、コマンドレットの **ComputerName** パラメーターを使用するか、または powershell セッション (pssession) を作成し、pssession でコマンドを実行します。

PSSession を作成すると、PowerShell によって、リモートコンピューターへの永続的な接続が確立されます。 PSSession を使用して、リモートコンピューターで一連の関連コマンドを実行します。 同じ PSSession で実行されるコマンドは、変数、別名、関数の値などのデータを共有できます。

また、ローカルコンピューター上に PSSession を作成し、その中でコマンドを実行することもできます。
ローカル PSSession は、PowerShell リモート処理インフラストラクチャを使用して PSSession を作成および維持します。

Windows PowerShell 3.0 以降では、PSSessions は、そのセッションが作成されたセッションから独立しています。 アクティブな PSSessions は、リモートコンピューター (または、リモートエンドまたは接続の "サーバー側" にあるコンピューター) 上で保持されます。 その結果、PSSession から切断し、後で同じコンピューターまたは別のコンピューターから再接続することができます。

このトピックでは、PSSessions を作成、使用、取得、および削除する方法について説明します。 詳細については、「 [about_PSSession_Details](about_PSSession_Details.md)」を参照してください。

注: PSSessions は、PowerShell リモート処理インフラストラクチャを使用します。 PSSessions を使用するには、ローカルコンピューターとリモートコンピューターがリモート処理用に構成されている必要があります。
詳細については、「[about_Remote_Requirements](about_Remote_Requirements.md)」を参照してください。

Windows Vista 以降のバージョンの Windows では、ローカルコンピューター上に PSSession を作成するには、[管理者として実行] オプションを使用して PowerShell を起動する必要があります。

## <a name="what-is-a-session"></a>セッションとは何ですか。

セッションとは、PowerShell が実行される環境です。

PowerShell を起動するたびに、セッションが作成され、そのセッションでコマンドを実行できるようになります。 また、モジュールやスナップインなどの項目をセッションに追加したり、変数、関数、別名などの項目を作成したりすることもできます。 これらの項目はセッションにのみ存在し、セッションが終了すると削除されます。

また、ローカルコンピューターまたはリモートコンピューター上で、"PowerShell セッション" または "PSSessions" と呼ばれるユーザー管理セッションを作成することもできます。 既定のセッションと同様に、PSSession でコマンドを実行し、項目を追加および作成することができます。 ただし、自動的に開始されるセッションとは異なり、作成する pssession を制御できます。 これらの情報は、取得、作成、構成、および削除したり、接続を解除して再接続したり、同じ PSSession で複数のコマンドを実行したりすることができます。 PSSession は、削除するか、タイムアウトになるまで使用できます。

通常は、PSSession を作成して、リモートコンピューターで一連の関連コマンドを実行します。 リモートコンピューター上に PSSession を作成すると、PowerShell は、セッションをサポートするために、リモートコンピューターへの永続的な接続を確立します。

またはコマンドレットの **ComputerName** パラメーターを使用して `Invoke-Command` `Enter-PSSession` リモートコマンドを実行したり、対話型セッションを開始したりする場合、PowerShell はリモートコンピューター上に一時的なセッションを作成し、コマンドが完了するか、対話型セッションが終了するとすぐにセッションを閉じます。 これらの一時的なセッションを制御することはできません。また、複数のコマンドや単一の対話型セッションで使用することはできません。

PowerShell では、"現在のセッション" は、使用しているセッションです。 "現在のセッション" は、一時的なセッションまたは PSSession を含む任意のセッションを参照できます。

## <a name="why-use-a-pssession"></a>PSSession を使用する理由

リモートコンピューターへの永続的な接続が必要な場合は、PSSession を使用します。
PSSession を使用すると、変数の値、関数の内容、エイリアスの定義など、データを共有する一連のコマンドを実行できます。

PSSession を作成しなくても、リモートコマンドを実行できます。 1つまたは複数のコンピューターで1つのコマンドまたは一連の関連のないコマンドを実行するには、リモート対応のコマンドレットの **ComputerName** パラメーターを使用します。

またはの **ComputerName** パラメーターを使用すると、PowerShell によって `Invoke-Command` `Enter-PSSession` リモートコンピューターへの一時的な接続が確立され、コマンドが完了するとすぐに接続が閉じられます。 作成したデータ要素は、接続が閉じられると失われます。

やなどの **ComputerName** パラメーターを持つ他のコマンド `Get-Eventlog` レット `Get-WmiObject` では、さまざまなリモート処理テクノロジを使用してデータを収集します。 None PSSession のような永続的な接続を作成します。

## <a name="how-to-create-a-pssession"></a>PSSession を作成する方法

PSSession を作成するには、 `New-PSSession` コマンドレットを使用します。 リモートコンピューター上に PSSession を作成するには、コマンドレットの **ComputerName** パラメーターを使用し `New-PSSession` ます。

たとえば、次のコマンドは、Server01 コンピューター上に新しい PSSession を作成します。

```powershell
New-PSSession -ComputerName Server01
```

コマンドを送信すると、は `New-PSSession` pssession を作成し、pssession を表すオブジェクトを返します。 PSSession の作成時に変数にオブジェクトを保存することも、コマンドを使用して `Get-PSSession` 後で pssession を取得することもできます。

たとえば、次のコマンドは、Server01 コンピューター上に新しい PSSession を作成し、結果として生成されたオブジェクトを $ps 変数に保存します。

```powershell
$ps = New-PSSession -ComputerName Server01
```

## <a name="how-to-create-pssessions-on-multiple-computers"></a>複数のコンピューターに PSSessions を作成する方法

複数のコンピューターで PSSessions を作成するには、コマンドレットの **ComputerName** パラメーターを使用し `New-PSSession` ます。 コンマ区切りの一覧で、リモートコンピューターの名前を入力します。

たとえば、Server01、Server02、および Server03 コンピューターで PSSessions を作成するには、次のように入力します。

```powershell
New-PSSession -ComputerName Server01, Server02, Server03
```

`New-PSSession` 各リモートコンピューターに1つの PSSession を作成します。

## <a name="how-to-get-pssessions"></a>PSSessions を取得する方法

現在のセッションで作成された pssession を取得するには、 `Get-PSSession` **ComputerName** パラメーターを指定せずにコマンドレットを使用します。 `Get-PSSession` 返されるオブジェクトと同じ型を返し `New-PSSession` ます。

次のコマンドは、現在のセッションで作成されたすべての pssession を取得します。

```powershell
Get-PSSession
```

PSSessions の既定の表示には、その ID と既定の表示名が表示されます。 セッションを作成するときに、別の表示名を割り当てることができます。

```output
Id   Name       ComputerName    State    ConfigurationName
---  ----       ------------    -----    ---------------------
1    Session1   Server01        Opened   Microsoft.PowerShell
2    Session2   Server02        Opened   Microsoft.PowerShell
3    Session3   Server03        Opened   Microsoft.PowerShell
```

また、PSSessions を変数に保存することもできます。 次のコマンドは、PSSessions を取得し、ps123 変数に保存し \$ ます。

```powershell
$ps123 = Get-PSSession
```

PSSession のコマンドレットを使用する場合、PSSession はその ID、名前、またはそのインスタンス ID (GUID) で参照できます。 次のコマンドは、その ID で PSSession を取得し、ps01 変数に保存し \$ ます。

```powershell
$ps01 = Get-PSSession -Id 1
```

Windows PowerShell 3.0 以降では、PSSessions はリモートコンピューター上で保持されます。 特定のリモートコンピューターで作成した PSSessions を取得するには、コマンドレットの **ComputerName** パラメーターを使用し `Get-PSSession` ます。 次のコマンドは、Server01 リモートコンピューター上で作成した pssession を取得します。 これには、現在のセッションで作成された pssession と、ローカルコンピューターまたは他のコンピューター上の他のセッションが含まれます。

```powershell
Get-PSSession -ComputerName Server01
```

Windows PowerShell 2.0 では、 `Get-PSSession` 現在のセッションで作成された pssession のみを取得します。 セッションがに接続され、ローカルコンピューターでコマンドを実行している場合でも、他のセッションまたは他のコンピューターで作成された pssession は取得されません。

## <a name="how-to-run-commands-in-a-pssession"></a>PSSession でコマンドを実行する方法

1つ以上の PSSessions でコマンドを実行するには、コマンドレットを使用し `Invoke-Command` ます。
セッションパラメーターを使用して、PSSessions と **ScriptBlock** パラメーターを指定してコマンドを指定します。

たとえば、 `Get-ChildItem` $ps 123 変数に保存された3つの PSSessions のそれぞれで ("dir") コマンドを実行するには、次のように入力します。

```powershell
Invoke-Command -Session $ps123 -ScriptBlock { Get-ChildItem }
```

## <a name="how-to-delete-pssessions"></a>PSSessions を削除する方法

PSSession が終了したら、コマンドレットを使用し `Remove-PSSession` て pssession を削除し、使用していたリソースを解放します。

```powershell
Remove-PSSession -Session $ps
```

または

```powershell
Remove-PSSession -Id 1
```

リモートコンピューターから PSSession を削除するには、コマンドレットの **ComputerName** パラメーターを使用し `Remove-PSSession` ます。

```powershell
Remove-PSSession -ComputerName Server01 -Id 1
```

PSSession を削除しない場合、PSSession はタイムアウトするまで使用可能な状態のままになります。

コマンドレットの **IdleTimeout** パラメーターを使用して、 `New-PSSessionOption` アイドル状態の PSSession の有効期限を設定することもできます。 詳細については、「 [New-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)」を参照してください。

## <a name="the-pssession-cmdlets"></a>PSSession のコマンドレット

PSSession のコマンドレットの一覧を表示するには、次のように入力します。

```powershell
Get-Help *-PSSession
```

- 接続-PSSession: PSSession を現在のセッションに接続します。
- 切断-PSSession: 現在のセッションから PSSession を切断します
- 入力-PSSession: 対話型セッションを開始します
- 終了-PSSession: 対話型セッションを終了します
- Get-help: 現在のセッションの PSSession を取得します。
- 新規-PSSession: ローカルコンピューターまたはリモートコンピューター上に新しい PSSession を作成します。
- 受信-PSSession: 切断されたセッションで実行されたコマンドの結果を取得します。
- -PSSession の削除: 現在のセッションの PSSession を削除します。

## <a name="for-more-information"></a>詳細情報

PSSessions の詳細については、「 [about_PSSession_Details](about_PSSession_Details.md)」を参照してください。

## <a name="see-also"></a>参照

[about_Remote](about_Remote.md)

[about_Remote_Disconnected_Sessions](about_Remote_Disconnected_Sessions.md)

[about_Remote_Requirements](about_Remote_Requirements.md)

[Connect-PSSession](xref:Microsoft.PowerShell.Core.Connect-PSSession)

[Disconnect-PSSession](xref:Microsoft.PowerShell.Core.Disconnect-PSSession)

[Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)

[Exit-PSSession](xref:Microsoft.PowerShell.Core.Exit-PSSession)

[Get-PSSession](xref:Microsoft.PowerShell.Core.Get-PSSession)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

[Remove-PSSession](xref:Microsoft.PowerShell.Core.Remove-PSSession)

