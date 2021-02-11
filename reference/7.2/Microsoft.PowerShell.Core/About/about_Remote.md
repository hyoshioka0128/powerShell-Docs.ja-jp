---
description: PowerShell でリモートコマンドを実行する方法について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote に関するページ
ms.openlocfilehash: b47efbaaebdd75be5f15be449e194113a0d6ebd7
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599286"
---
# <a name="about-remote"></a>リモートの概要

## <a name="short-description"></a>概要
PowerShell でリモートコマンドを実行する方法について説明します。

## <a name="long-description"></a>詳細説明

一時接続または固定接続を使用して、1台のコンピューターまたは複数のコンピューターでリモートコマンドを実行できます。 また、1台のリモートコンピューターで対話型セッションを開始することもできます。

このトピックでは、さまざまな種類のリモートコマンドを実行する方法を示す一連の例を示します。 これらの基本的なコマンドを実行した後、これらのコマンドで使用される各コマンドレットについて説明しているヘルプトピックを参照してください。 これらのトピックでは、詳細を説明し、ニーズに合わせてコマンドを変更する方法を説明します。

注: PowerShell リモート処理を使用するには、ローカルコンピューターとリモートコンピューターがリモート処理用に構成されている必要があります。 詳細については、「[about_Remote_Requirements](about_Remote_Requirements.md)」を参照してください。

## <a name="how-to-start-an-interactive-session-enter-pssession"></a>対話型セッションを開始する方法 (ENTER PSSESSION)

リモートコマンドを実行する最も簡単な方法は、リモートコンピューターとの対話型セッションを開始することです。

セッションが開始されると、入力したコマンドはリモートコンピューター上で直接入力した場合と同様に、リモートコンピューター上で実行されます。 各対話型セッションでは、1台のコンピューターにのみ接続できます。

対話型セッションを開始するには、Enter-PSSession コマンドレットを使用します。 次のコマンドは、Server01 コンピューターとの対話型セッションを開始します。

```powershell
Enter-PSSession Server01
```

コマンドプロンプトが変更され、Server01 コンピューターに接続されていることが示されます。

```
Server01\PS>
```

これで、Server01 コンピューターにコマンドを入力できます。

対話型セッションを終了するには、次のように入力します。

```powershell
Exit-PSSession
```

詳細については、「PSSession」を参照してください。

## <a name="how-to-use-cmdlets-that-have-a-computername-parameter-to-get-remote-data"></a>COMPUTERNAME パラメーターを持つコマンドレットを使用してリモートデータを取得する方法

いくつかのコマンドレットには、リモートコンピューターからオブジェクトを取得できる ComputerName パラメーターがあります。

これらのコマンドレットは WS-MANAGEMENT ベースの PowerShell リモート処理を使用しないため、PowerShell を実行している任意のコンピューターでこれらのコマンドレットの ComputerName パラメーターを使用できます。 コンピューターは、PowerShell リモート処理用に構成する必要はありません。また、コンピューターがリモート処理のシステム要件を満たしている必要はありません。

次のコマンドレットには ComputerName パラメーターがあります。

```
Clear-EventLog    Limit-EventLog
Get-Counter       New-EventLog
Get-EventLog      Remove-EventLog
Get-HotFix        Restart-Computer
Get-Process       Show-EventLog
Get-Service       Stop-Computer
Get-WinEvent      Test-Connection
Get-WmiObject     Write-EventLog
```

たとえば、次のコマンドは、Server01 リモートコンピューター上のサービスを取得します。

```powershell
Get-Service -ComputerName Server01
```

通常、特別な構成なしでリモート処理をサポートするコマンドレットには **ComputerName** パラメーターがあり、 **セッション** パラメーターはありません。 セッションでこれらのコマンドレットを見つけるには、次のように入力します。

```powershell
Get-Command | Where-Object {
  $_.Parameters.Keys -contains 'ComputerName' -and
  $_.Parameters.Keys -notcontains 'Session'
}
```

## <a name="how-to-run-a-remote-command"></a>リモートコマンドを実行する方法

リモートコンピューターで他のコマンドを実行するには、Invoke-Command コマンドレットを使用します。

1つのコマンドまたはいくつかの関連のないコマンドを実行するには、Invoke-Command の ComputerName パラメーターを使用して、リモートコンピューターを指定します。 ScriptBlock パラメーターを使用して、コマンドを指定します。

たとえば、次のコマンドは、Server01 コンピューターで Get-Culture コマンドを実行します。

```powershell
Invoke-Command -ComputerName Server01 -ScriptBlock {Get-Culture}
```

ComputerName パラメーターは、1つまたは複数のコンピューターで1つのコマンドまたは複数の関連のないコマンドを実行する状況を想定して設計されています。 リモートコンピューターへの永続的な接続を確立するには、Session パラメーターを使用します。

## <a name="how-to-create-a-persistent-connection-pssession"></a>永続的な接続を作成する方法 (PSSESSION)

Invoke-Command コマンドレットの ComputerName パラメーターを使用すると、Windows PowerShell はコマンドに対してのみ接続を確立します。 そのため、コマンドが完了すると接続を閉じます。 コマンドで定義されている変数または関数はすべて失われます。

リモートコンピューターへの永続的な接続を作成するには、New-PSSession コマンドレットを使用します。 たとえば、次のコマンドは、Server01 コンピューターと Server02 コンピューター上に pssession を作成し、PSSessions を $s 変数に保存します。

```powershell
$s = New-PSSession -ComputerName Server01, Server02
```

## <a name="how-to-run-commands-in-a-pssession"></a>PSSESSION でコマンドを実行する方法

PSSession を使用すると、関数、別名、変数の値などのデータを共有する一連のリモートコマンドを実行できます。 PSSession でコマンドを実行するには、Invoke-Command コマンドレットの Session パラメーターを使用します。

たとえば、次のコマンドは、Invoke-Command コマンドレットを使用して、Server01 コンピューターと Server02 コンピューター上の pssession で Get-Process コマンドを実行します。
このコマンドは、各 PSSession の $p 変数にプロセスを保存します。

```powershell
Invoke-Command -Session $s -ScriptBlock {$p = Get-Process}
```

PSSession は永続的な接続を使用するので、$p 変数を使用するのと同じ PSSession で別のコマンドを実行できます。 次のコマンドは $p に保存されているプロセスの数をカウントします。

```powershell
Invoke-Command -Session $s -ScriptBlock {$p.count}
```

## <a name="how-to-run-a-remote-command-on-multiple-computers"></a>複数のコンピューターでリモートコマンドを実行する方法

複数のコンピューターでリモートコマンドを実行するには、Invoke-Command の ComputerName パラメーターの値にすべてのコンピューター名を入力します。 名前はコンマで区切ります。

たとえば、次のコマンドは、3台のコンピューターで Get-Culture コマンドを実行します。

```powershell
Invoke-Command -ComputerName S1, S2, S3 -ScriptBlock {Get-Culture}
```

複数の pssession でコマンドを実行することもできます。 次のコマンドは、Server01、Server02、および Server03 コンピューター上に pssession を作成し、各 pssession で Get-Culture コマンドを実行します。

```powershell
$s = New-PSSession -ComputerName S1, S2, S3
Invoke-Command -Session $s -ScriptBlock {Get-Culture}
```

コンピューターのローカルコンピューターの一覧を含めるには、ローカルコンピューターの名前を入力するか、ドット (.) を入力するか、「localhost」と入力します。

```powershell
Invoke-Command -ComputerName S1, S2, S3, localhost -ScriptBlock {Get-Culture}
```

## <a name="how-to-run-a-script-on-remote-computers"></a>リモートコンピューターでスクリプトを実行する方法

リモートコンピューターでローカルスクリプトを実行するには、コマンドの FilePath パラメーターを使用します。

たとえば、次のコマンドは、S1 および S2 コンピューターで Sample.ps1 スクリプトを実行します。

```powershell
Invoke-Command -ComputerName S1, S2 -FilePath C:\Test\Sample.ps1
```

スクリプトの結果がローカルコンピューターに返されます。 ファイルをコピーする必要はありません。

## <a name="how-to-stop-a-remote-command"></a>リモートコマンドを停止する方法

コマンドを中断するには、CTRL + C キーを押します。 割り込み要求は、リモートコマンドを終了するリモートコンピューターに渡されます。

## <a name="for-more-information"></a>詳細情報

- リモート処理のシステム要件の詳細については、「 [about_Remote_Requirements](about_Remote_Requirements.md)」を参照してください。

- リモート出力の書式設定のヘルプについては、「 [about_Remote_Output](about_Remote_Output.md)」を参照してください。

- リモート処理のしくみ、リモートデータの管理方法、特別な構成、セキュリティの問題、およびその他のよく寄せられる質問については、「 [about_Remote_FAQ](about_Remote_FAQ.md)」を参照してください。

- リモート処理エラーの解決については、「about_Remote_Troubleshooting」を参照してください。

- PSSessions と永続的な接続の詳細については、「 [about_PSSessions](about_PSSessions.md)」を参照してください。

- PowerShell のバックグラウンドジョブの詳細については、「 [about_Jobs](about_Jobs.md)」を参照してください。

## <a name="keywords"></a>KEYWORDS

about_Remoting

## <a name="see-also"></a>関連項目

[about_PSSessions](about_PSSessions.md)

[about_Remote_Disconnected_Sessions](about_Remote_Disconnected_Sessions.md)

[about_Remote_Requirements](about_Remote_Requirements.md)

[about_Remote_FAQ](about_Remote_FAQ.md)

[about_Remote_TroubleShooting](about_Remote_TroubleShooting.md)

[about_Remote_Variables](about_Remote_Variables.md)

[Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

