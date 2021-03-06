---
ms.date: 08/14/2018
keywords: PowerShell, コマンドレット
title: リモート コマンドの実行
ms.assetid: d6938b56-7dc8-44ba-b4d4-cd7b169fd74d
ms.openlocfilehash: 2001b5509acde6ec4259bb1442944958a67aa66f
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62058915"
---
# <a name="running-remote-commands"></a>リモート コマンドの実行

1 つの PowerShell コマンドを使用し、1 台から数百台のコンピューターでコマンドを実行できます。 Windows PowerShell は、WMI、RPC、WS-Management など、さまざまなテクノロジを使用してリモート コンピューティングをサポートします。

PowerShell Core では、WMI、WS-Management、および SSH リモート処理をサポートしています。 RPC のサポートは終了しました。

PowerShell Core でのリモート処理の詳細については、次の記事を参照してください。

- [PowerShell Core での SSH リモート処理][ssh-remoting]
- [PowerShell Core での WSMan リモート処理][wsman-remoting]

## <a name="windows-powershell-remoting-without-configuration"></a>構成なしでの Windows PowerShell リモート処理

多くの Windows PowerShell コマンドレットは、1 台以上のリモート コンピューターのデータの収集や設定の変更が可能な ComputerName パラメーターを持っています。 これらのコマンドレットは、さまざまな通信プロトコルを使用して、特別な構成を行わなくてもすべての Windows オペレーティング システム上で動作します。

これらのコマンドレットには、次のものがあります。

- [Restart-Computer](/powershell/module/microsoft.powershell.management/restart-computer)
- [Test-Connection](/powershell/module/microsoft.powershell.management/test-connection)
- [Clear-EventLog](/powershell/module/microsoft.powershell.management/clear-eventlog)
- [Get-EventLog](/powershell/module/microsoft.powershell.management/get-eventlog)
- [Get-HotFix](/powershell/module/microsoft.powershell.management/get-hotfix)
- [Get-Process](/powershell/module/microsoft.powershell.management/get-process)
- [Get-Service](/powershell/module/microsoft.powershell.management/get-service)
- [Set-Service](/powershell/module/microsoft.powershell.management/set-service)
- [Get-WinEvent](/powershell/module/microsoft.powershell.diagnostics/get-winevent)
- [Get-WmiObject](/powershell/module/microsoft.powershell.management/get-wmiobject)

通常、特別な構成なしでリモート処理をサポートするコマンドレットは、ComputerName パラメーターを指定し、Session パラメーターは必要ありません。 セッションでこれらのコマンドレットを見つけるには、次のように入力します。

```powershell
Get-Command | where { $_.parameters.keys -contains "ComputerName" -and $_.parameters.keys -notcontains "Session"}
```

## <a name="windows-powershell-remoting"></a>Windows PowerShell のリモート処理

Windows PowerShell リモート処理では、WS-Management プロトコルを使用して、1 台以上のリモート コンピューターで Windows PowerShell コマンドを実行できます。 永続的な接続の確立、対話型のセッションの開始、およびリモート コンピューターでのスクリプトの実行が可能です。

Windows PowerShell のリモート処理を使用するには、リモート管理のためにリモート コンピューターを構成する必要があります。
手順を含む詳細については、「[about_Remote_Requirements](/powershell/module/microsoft.powershell.core/about/about_remote_requirements)」をご覧ください。

Windows PowerShell のリモート処理を構成すると、それ以降は、多くのリモート処理の戦略が使用可能になります。
この記事では、その一部のみを示します。 詳細については、「[About Remote](/powershell/module/microsoft.powershell.core/about/about_remote)」(リモート処理について) を参照してください。

### <a name="start-an-interactive-session"></a>対話型セッションの開始

1 台のリモート コンピューターとの対話型セッションを開始するには、[Enter-PSSession](/powershell/module/microsoft.powershell.core/enter-pssession) コマンドレットを使用します。
たとえば、Server01 リモート コンピューターと対話型セッションを開始するには、次のように入力します。

```powershell
Enter-PSSession Server01
```

コマンド プロンプトの画面が更新され、リモート コンピューターの名前が表示されます。 プロンプトに入力されたコマンドはリモート コンピューターで実行され、結果はローカル コンピューターに表示されます。

対話型セッションを終了するには、次のように入力します。

```powershell
Exit-PSSession
```

Enter-PSSession コマンドレットおよび Exit-PSSession コマンドレットの詳細については、次を参照してください。

- [Enter-PSSession](/powershell/module/microsoft.powershell.core/enter-pssession)
- [Exit-PSSession](/powershell/module/microsoft.powershell.core/exit-pssession)

### <a name="run-a-remote-command"></a>リモート コマンドの実行

1 台または複数のコンピューターでコマンドを実行するには、[Invoke-Command](/powershell/module/microsoft.powershell.core/invoke-command) コマンドレットを使用します。 たとえば、[Get-UICulture](/powershell/module/microsoft.powershell.utility/get-uiculture) コマンドをリモート コンピューター Server01 と Server02 で実行するには、次のように入力します。

```powershell
Invoke-Command -ComputerName Server01, Server02 -ScriptBlock {Get-UICulture}
```

コンピューターに出力が返されます。

```output
LCID    Name     DisplayName               PSComputerName
----    ----     -----------               --------------
1033    en-US    English (United States)   server01.corp.fabrikam.com
1033    en-US    English (United States)   server02.corp.fabrikam.com
```

### <a name="run-a-script"></a>スクリプトの実行

1 台または多数のリモート コンピューターでスクリプトを実行するには、`Invoke-Command` コマンドレットの FilePath パラメーターを使用します。 スクリプトがローカル コンピューターでアクセス可能でなければなりません。 結果はローカル コンピューターに返されます。

たとえば、次のコマンドは、リモート コンピューター Server01 と Server02 で DiskCollect.ps1 スクリプトを実行します。

```powershell
Invoke-Command -ComputerName Server01, Server02 -FilePath c:\Scripts\DiskCollect.ps1
```

### <a name="establish-a-persistent-connection"></a>永続的な接続の確立

`New-PSSession` コマンドレットを使用して、リモート コンピューター上に永続的なセッションを作成します。 次の例では、Server01 および Server02 上にリモート セッションを作成します。 セッション オブジェクトは、`$s` 変数に格納されます。

```powershell
$s = New-PSSession -ComputerName Server01, Server02
```

セッションが確立されたので、それらで任意のコマンドを実行できます。 また、セッションは永続的であるため、1 つのコマンドからデータを収集して、別のコマンドでそのデータを利用できます。

たとえば、次のコマンドは $s 変数のセッションで Get-HotFix コマンドを実行し、結果を $h 変数に保存します。 $h 変数は $s のそれぞれのセッションで作成されますが、ローカル セッションには存在しません。

```powershell
Invoke-Command -Session $s {$h = Get-HotFix}
```

これで、同じセッション内の他のコマンドで、`$h` 変数のデータを使用できるようになりました。 結果はローカル コンピューターに表示されます。 たとえば、次のように入力します。

```powershell
Invoke-Command -Session $s {$h | where {$_.InstalledBy -ne "NTAUTHORITY\SYSTEM"}}
```

### <a name="advanced-remoting"></a>高度なリモート処理

Windows PowerShell のリモート管理はこれだけではありません。 Windows PowerShell と共にインストールされるコマンドレットを使用して、ローカルとリモートの両側からのリモート セッションの確立と構成、カスタマイズおよび制限されたセッションの作成、リモート セッションで実際に暗黙的に実行されるコマンドのリモート セッションからのユーザーによるインポート、リモート セッションのセキュリティの構成など、さまざまな処理を実行できます。

Windows PowerShell には、WSMan プロバイダーが含まれています。 プロバイダーは、ローカル コンピューターとリモート コンピューターの構成設定の階層内を移動できる `WSMAN:` ドライブを作成します。

WSMan プロバイダーの詳細については、[WSMan Provider](https://technet.microsoft.com/library/dd819476.aspx) (WSMan プロバイダー) および [WS-Management コマンドレット関するページ](/powershell/module/microsoft.powershell.core/about/about_ws-management_cmdlets)を参照するか、Windows PowerShell コンソールで `Get-Help wsman` と入力してください。

詳細については、次のドキュメントをご覧ください。

- [about_Remote のよく寄せられる質問](https://technet.microsoft.com/library/dd315359.aspx)
- [Register-PSSessionConfiguration](https://go.microsoft.com/fwlink/?LinkId=821508)
- [Import-PSSession](https://go.microsoft.com/fwlink/?LinkId=821821)

リモート処理のエラーに関するヘルプは、「[about_Remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx)」をご覧ください。

## <a name="see-also"></a>参照

- [about_Remote](https://technet.microsoft.com/library/9b4a5c87-9162-4adf-bdfe-fbc80b9b8970)
- [about_Remote_FAQ](https://technet.microsoft.com/library/e23702fd-9415-4a98-9975-390a4d3adc42)
- [about_Remote_Requirements](https://technet.microsoft.com/library/da213949-134c-4741-b307-81f4492ba1bd)
- [about_Remote_Troubleshooting](https://technet.microsoft.com/library/2f890148-8578-49ed-85ea-79a489dd6317)
- [about_PSSessions](https://technet.microsoft.com/library/7a9b4e0e-fa1b-47b0-92f6-6e2995d70acb)
- [about_WS-Management_Cmdlets](https://technet.microsoft.com/library/6ed3370a-ea10-45a5-9493-696aeace27ed)
- [Invoke-Command](/powershell/module/microsoft.powershell.core/invoke-command)
- [Import-PSSession](https://go.microsoft.com/fwlink/?LinkId=821821)
- [New-PSSession](https://go.microsoft.com/fwlink/?LinkId=821498)
- [Register-PSSessionConfiguration](https://go.microsoft.com/fwlink/?LinkId=821508)
- [WSMan プロバイダー](https://technet.microsoft.com/library/66fe1241-e08f-49ca-832f-a84c33ca8735)

[wsman-remoting]: WSMan-Remoting-in-PowerShell-Core.md
[ssh-remoting]: SSH-Remoting-in-PowerShell-Core.md