---
ms.date: 07/10/2019
keywords: JEA, PowerShell, セキュリティ
title: JEA の使用
description: この記事では、JEA エンドポイントに接続して使用できるさまざまな方法について説明します。
ms.openlocfilehash: b3d81cc0aa76549c81136e5a1a5af28df9c6fa7a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92501543"
---
# <a name="using-jea"></a>JEA の使用

この記事では、JEA エンドポイントに接続して使用できるさまざまな方法について説明します。

## <a name="using-jea-interactively"></a>JEA を対話的に使う

JEA の構成をテストする場合、またはユーザー用のシンプルなタスクがある場合は、通常の PowerShell リモート処理セッションと同じ方法で JEA を使用できます。 複雑なリモート処理タスクには、[暗黙的なリモート処理](#using-jea-with-implicit-remoting)を使用することをお勧めします。 暗黙的なリモート処理では、ユーザーがローカルでデータ オブジェクトを操作できるようにします。

JEA を対話的に使用するには、次のものが必要です。

- 接続しているコンピューターの名前 (ローカル コンピューターにすることができます)
- そのコンピューターに登録されている JEA エンドポイントの名前
- そのコンピューター上の JEA エンドポイントにアクセスできる資格情報

この情報を前提として、[New-PSSession](/powershell/module/microsoft.powershell.core/New-PSSession) または [Enter-PSSession](/powershell/module/microsoft.powershell.core/enter-pssession) コマンドレットを使って JEA セッションを開始できます。

```powershell
$nonAdminCred = Get-Credential
Enter-PSSession -ComputerName localhost -ConfigurationName JEAMaintenance -Credential $nonAdminCred
```

現在のユーザー アカウントに JEA エンドポイントへのアクセス権がある場合は、**Credential** パラメーターを省略できます。

PowerShell プロンプトが `[localhost]: PS>` に変わると、現在、リモート JEA セッションとやりとりしている状態であることがわかります。 `Get-Command` を実行して、使用できるコマンドを確認できます。 使用できるパラメーターまたは許可されているパラメーター値に制限があるかどうかを確認するには、管理者に問い合わせてください。

JEA セッションは NoLanguage モードで動作することに注意してください。 通常、お客様が使用している方法の中には、PowerShell を利用できないものがある場合があります。 たとえば、変数を使ってデータを格納したり、コマンドレットから返されたオブジェクトのプロパティを調べたりすることはできません。 次の例では、NoLanguage モードで動作する同じコマンドを取得するための 2 つの方法を示しています。

```powershell
# Using variables is prohibited in NoLanguage mode. The following will not work:
# $vm = Get-VM -Name 'SQL01'
# Start-VM -VM $vm

# You can use pipes to pass data through to commands that accept input from the pipeline
Get-VM -Name 'SQL01' | Start-VM

# You can also wrap subcommands in parentheses and enter them inline as arguments
Start-VM -VM (Get-VM -Name 'SQL01')

# You can also use parameter sets that don't require extra data to be passed in
Start-VM -VMName 'SQL01'
```

この方法が難しくなるさらに複雑なコマンド呼び出しの場合は、[暗黙的なリモート処理](#using-jea-with-implicit-remoting)を使用するか、または必要な機能をラップする[カスタム関数の作成](role-capabilities.md#creating-custom-functions)を検討します。

## <a name="using-jea-with-implicit-remoting"></a>暗黙的なリモート処理での JEA の使用

PowerShell には、暗黙的なリモート処理モデルが用意されており、リモート マシンからプロキシ コマンドレットをインポートして、ローカル コマンドの場合と同じようにやりとりすることができます。 暗黙的なリモート処理は、この **Hey, Scripting Guy!** の [ブログ記事](https://devblogs.microsoft.com/scripting/remoting-the-implicit-way/)で説明されています。
暗黙的なリモート処理は、全言語モードで JEA コマンドレットを使うことができるので、JEA を使用するときに役立ちます。 タブ補完や変数を使用し、オブジェクトを操作し、ローカル スクリプトを使用して JEA エンドポイントに対してタスクを自動化することもできます。 プロキシ コマンドを呼び出すたびに、リモート マシン上の JEA エンドポイントにデータが送信されて、そこで実行されます。

暗黙的なリモート処理は、既存の PowerShell セッションからコマンドレットをインポートすることによって動作します。 必要に応じて、各プロキシ コマンドレットの名詞の前に選択した文字列でプレフィックスを付けることができます。 プレフィックスによって、リモート システムに対するコマンドを区別することができます。 すべてのプロキシ コマンドを含む一時スクリプト モジュールが作成され、ローカル PowerShell セッションの間にインポートされます。

```powershell
# Create a new PSSession to your JEA endpoint
$jeasession = New-PSSession -ComputerName 'SERVER01' -ConfigurationName 'JEAMaintenance'

# Import the entire PSSession and prefix each imported cmdlet with "JEA"
Import-PSSession -Session $jeasession -Prefix 'JEA'

# Invoke "Get-Command" on the remote JEA endpoint using the proxy cmdlet
Get-JEACommand
```

> [!IMPORTANT]
> 既定の JEA コマンドレットでの制約のため、一部のシステムでは JEA セッション全体をインポートできない場合があります。 これを回避するには、必要なコマンドの名前を `-CommandName` パラメーターで明示的に指定することで、そのコマンドだけを JEA セッションからインポートします。 今後の更新で問題が解決され、JEA セッション全体を対象システムにインポートできるようになります。

既定のパラメーターで JEA の制約があるため、JEA セッションをインポートできない場合は、次の手順に従って、既定のコマンドをインポートされるセットから除外します。 `Select-Object` のようなコマンドを引き続き使用できますが、リモートの JEA セッションからインポートされたものの代わりに、コンピューターにインストールされているローカル バージョンを使用するだけです。

```powershell
# Create a new PSSession to your JEA endpoint
$jeasession = New-PSSession -ComputerName 'SERVER01' -ConfigurationName 'JEAMaintenance'

# Get a list of all the commands on the JEA endpoint
$commands = Invoke-Command -Session $jeasession -ScriptBlock { Get-Command }

# Filter out the default cmdlets
$jeaDefaultCmdlets = 'Clear-Host', 'Exit-PSSession', 'Get-Command', 'Get-FormatData', 'Get-Help', 'Measure-Object', 'Out-Default', 'Select-Object'
$filteredCommands = $commands.Name | Where-Object { $jeaDefaultCmdlets -notcontains $_ }

# Import only commands explicitly added in role capabilities and prefix each imported cmdlet with "JEA"
Import-PSSession -Session $jeasession -Prefix 'JEA' -CommandName $filteredCommands
```

また、[Export-PSSession](/powershell/module/microsoft.powershell.utility/Export-PSSession) を使って、暗黙的なリモート処理からプロキシされたコマンドレットを保持することもできます。
暗黙的なリモート処理の詳細については、[Import-PSSession](/powershell/module/microsoft.powershell.utility/import-pssession) および [Import-Module](/powershell/module/microsoft.powershell.core/import-module) のドキュメントを参照してください。

## <a name="using-jea-programmatically"></a>JEA のプログラムでの使用

JEA は、社内ヘルプデスク アプリや Web サイトなどのオートメーション システムやユーザー アプリケーションでも使用できます。 この方法は、制約のない PowerShell エンドポイントと通信するアプリをビルドするための方法と同じです。 このプログラムは JEA によって課せられる制限を使用するように設計されていることを確実にします。

シンプルな 1 回限りのタスクでは、[Invoke-Command](/powershell/module/microsoft.powershell.core/invoke-command) を使って、コマンドを JEA セッションで実行できます。

```powershell
Invoke-Command -ComputerName 'SERVER01' -ConfigurationName 'JEAMaintenance' -ScriptBlock { Get-Process; Get-Service }
```

JEA セッションに接続しているときに使用できるコマンドを確認するには、`Get-Command` を実行し、結果を反復して指定できるパラメーターを確認します。

```powershell
$allowedCommands = Invoke-Command -ComputerName 'SERVER01' -ConfigurationName 'JEAMaintenance' -ScriptBlock { Get-Command }
$allowedCommands | Where-Object { $_.CommandType -in 'Function', 'Cmdlet' } | Format-Table Name, Parameters
```

C# アプリをビルドしている場合は、[WSManConnectionInfo](/dotnet/api/system.management.automation.runspaces.wsmanconnectioninfo) オブジェクトで構成名を指定することにより、JEA セッションに接続する PowerShell 実行空間を作成できます。

```csharp
// using System.Management.Automation;
var computerName = "SERVER01";
var configName   = "JEAMaintenance";
// See https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential
var creds        = // create a PSCredential object here

WSManConnectionInfo connectionInfo = new WSManConnectionInfo(
    false,                 // Use SSL
    computerName,          // Computer name
    5985,                  // WSMan Port
    "/wsman",              // WSMan Path
                           // Connection URI with config name
    string.Format(CultureInfo.InvariantCulture, "http://schemas.microsoft.com/powershell/{0}", configName),
    creds);                // Credentials

// Now, use the connection info to create a runspace where you can run the commands
using (Runspace runspace = RunspaceFactory.CreateRunspace(connectionInfo))
{
    // Open the runspace
    runspace.Open();

    using (PowerShell ps = PowerShell.Create())
    {
        // Set the PowerShell object to use the JEA runspace
        ps.Runspace = runspace;

        // Now you can add and invoke commands
        ps.AddCommand("Get-Command");
        foreach (var result in ps.Invoke())
        {
            Console.WriteLine(result);
        }
    }

    // Close the runspace
    runspace.Close();
}
```

## <a name="using-jea-with-powershell-direct"></a>PowerShell Direct での JEA の使用

Windows 10 と Windows Server 2016 の Hyper-V には [PowerShell Direct](/virtualization/hyper-v-on-windows/user-guide/powershell-direct) 機能があります。Hyper-V 管理者はこの機能を使用して、仮想マシンのネットワーク構成やリモート管理設定に関係なく、PowerShell で仮想マシンを管理できます。

JEA と PowerShell Direct を使用して、Hyper-V 管理者にご利用の VM への制限付きアクセスを与えることができます。
これは、ご利用の VM へのネットワーク接続が失われ、データセンター管理者にネットワーク設定を修正してもらう必要がある場合に役立つことがあります。

PowerShell Direct 経由で JEA を使用するために、追加の構成は必要ありません。 しかし、仮想マシン内で実行されているゲスト オペレーティング システムは、Windows 10、Windows Server 2016 以降である必要があります。 Hyper-V 管理者は、PSRemoting コマンドレットで `-VMName` または `-VMId` パラメーターを使って JEA エンドポイントに接続できます。

```powershell
# Entering a JEA session using PowerShell Direct when the VM name is unique
Enter-PSSession -VMName 'SQL01' -ConfigurationName 'NICMaintenance' -Credential 'localhost\JEAformyHoster'

# Entering a JEA session using PowerShell Direct using VM ids
$vm = Get-VM -VMName 'MyVM' | Select-Object -First 1
Enter-PSSession -VMId $vm.VMId -ConfigurationName 'NICMaintenance' -Credential 'localhost\JEAformyHoster'
```

Hyper-V 管理者が使用するために、システムの管理に必要な最小の権限を持つ、専用のユーザー アカウントを作成することをお勧めします。 制約のない PowerShell を使用する場合など、特権のないユーザーでも Windows コンピューターに既定でサインインできることに注意してください。 これにより、ファイル システムを参照し、ご利用の OS 環境について詳しく知ることができます。 Hyper-V 管理者をロック ダウンし、PowerShell Direct と JEA を使用して VM だけにアクセスできるように制限するには、Hyper-V 管理者の JEA アカウントへのローカル ログオン権限を禁止する必要があります。
