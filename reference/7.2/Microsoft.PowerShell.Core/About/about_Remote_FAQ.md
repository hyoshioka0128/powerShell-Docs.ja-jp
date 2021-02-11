---
description: PowerShell でのリモートコマンドの実行に関する質問と回答が含まれています。
Locale: en-US
ms.date: 07/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_FAQ
ms.openlocfilehash: da3516697c58e3273538ed2ed93a7a54fcd9bb49
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600386"
---
# <a name="about-remote-faq"></a>about_Remote_FAQ

## <a name="short-description"></a>簡単な説明
PowerShell でのリモートコマンドの実行に関する質問と回答が含まれています。

## <a name="long-description"></a>長い説明

リモートで作業する場合は、1台のコンピューター ("ローカルコンピューター" と呼ばれます) に PowerShell でコマンドを入力しますが、コマンドは別のコンピューター ("リモートコンピューター" と呼ばれます) で実行します。 リモート操作のエクスペリエンスは、リモートコンピューターで可能な限り直接作業するのと同じです。

注: PowerShell リモート処理を使用するには、リモートコンピューターがリモート処理用に構成されている必要があります。 詳細については、「[about_Remote_Requirements](about_Remote_Requirements.md)」を参照してください。

### <a name="must-both-computers-have-powershell-installed"></a>両方のコンピューターに PowerShell がインストールされている必要がありますか。

はい。 リモートで作業するには、ローカルコンピューターとリモートコンピューターに、PowerShell、Microsoft .NET Framework、および Web Services for Management (WS-MANAGEMENT) プロトコルが必要です。 特定のコマンドを実行するために必要なすべてのファイルとその他のリソースは、リモートコンピューター上に存在する必要があります。

Windows PowerShell 3.0 を実行しているコンピューターと Windows PowerShell 2.0 を実行しているコンピューターは、リモート接続してリモートコマンドを実行できます。
ただし、セッションから切断して再接続する機能など、一部の機能は両方のコンピューターが Windows PowerShell 3.0 を実行している場合にのみ機能します。

リモートコンピューターに接続するためのアクセス許可、PowerShell を実行するためのアクセス許可、およびデータストア (ファイルやフォルダーなど) にアクセスするためのアクセス許可、およびリモートコンピューター上のレジストリが必要です。

詳細については、「[about_Remote_Requirements](about_Remote_Requirements.md)」を参照してください。

### <a name="how-does-remoting-work"></a>リモート処理のしくみ

リモートコマンドを送信すると、コマンドはネットワーク経由でリモートコンピューター上の PowerShell エンジンに送信され、リモートコンピューターの PowerShell クライアントで実行されます。 コマンドの結果はローカルコンピューターに送り返され、ローカルコンピューターの PowerShell セッションに表示されます。

コマンドを送信し、出力を受信するために、PowerShell は WS-Management プロトコルを使用します。 WS-Management プロトコルの詳細については、Windows ドキュメントの「 [WS-Management プロトコル](/windows/win32/winrm/ws-management-protocol) 」を参照してください。

Windows PowerShell 3.0 以降では、リモートセッションはリモートコンピューターに格納されます。 これにより、セッションを切断し、別のセッションまたは別のコンピューターから再接続できます。その際、コマンドを中断したり、状態を失うことはありません。

### <a name="is-powershell-remoting-secure"></a>PowerShell リモート処理はセキュリティで保護されていますか。

リモートコンピューターに接続すると、システムはローカルコンピューターのユーザー名とパスワードの資格情報、またはコマンドで指定した資格情報を使用してリモートコンピューターにログインします。 資格情報と転送の残りの部分は暗号化されます。

さらに保護を追加するには、HTTP ではなく Secure Sockets Layer (SSL) を使用して Windows リモート管理 (WinRM) 要求をリッスンするようにリモートコンピューターを構成できます。 次に、  `Invoke-Command` `New-PSSession` `Enter-PSSession` 接続を確立するときに、、、およびコマンドレットの UseSSL パラメーターを使用できます。 このオプションは、HTTP ではなく、より安全な HTTPS チャネルを使用します。

### <a name="do-all-remote-commands-require-powershell-remoting"></a>すべてのリモートコマンドに PowerShell リモート処理が必要ですか?

いいえ。 いくつかのコマンドレットには、リモートコンピューターからオブジェクトを取得できる **ComputerName** パラメーターがあります。

これらのコマンドレットは、PowerShell リモート処理を使用しません。 そのため、powershell リモート処理用にコンピューターが構成されていない場合や、コンピューターが PowerShell リモート処理の要件を満たしていない場合でも、PowerShell を実行している任意のコンピューターで使用できます。

これらのコマンドレットには、次のものが含まれます。

- `Get-Process`
- `Get-Service`
- `Get-WinEvent`
- `Get-EventLog`
- `Test-Connection`

**ComputerName** パラメーターを使用してすべてのコマンドレットを検索するには、次のように入力します。

```powershell
Get-Help * -Parameter ComputerName
# or
Get-Command -ParameterName ComputerName
```

特定のコマンドレットの **ComputerName** パラメーターに PowerShell リモート処理が必要かどうかを確認するには、パラメーターの説明を参照してください。 パラメーターの説明を表示するには、次のように入力します。

```powershell
Get-Help <cmdlet-name> -Parameter ComputerName
```

次に例を示します。

```powershell
Get-Help Get-Process -Parameter ComputerName
```

他のすべてのコマンドについては、コマンドレットを使用し `Invoke-Command` ます。

### <a name="how-do-i-run-a-command-on-a-remote-computer"></a>リモートコンピューターでコマンドを実行操作方法には

リモートコンピューターでコマンドを実行するには、コマンドレットを使用し `Invoke-Command` ます。

コマンドを中かっこ () で囲み、 `{}` スクリプトブロックにします。 の **ScriptBlock** パラメーターを使用し `Invoke-Command` て、コマンドを指定します。

の **ComputerName** パラメーターを使用して `Invoke-Command` 、リモートコンピューターを指定できます。 または、リモートコンピューターへの永続的な接続 (セッション) を作成し、の **session** パラメーターを使用して、その `Invoke-Command` セッションでコマンドを実行できます。

たとえば、次のコマンドは、 `Get-Process` コマンドをリモートで実行します。

```powershell
Invoke-Command -ComputerName Server01, Server02 -ScriptBlock {Get-Process}

#  - OR -

Invoke-Command -Session $s -ScriptBlock {Get-Process}
```

リモートコマンドを中断するには、「 <kbd>CTRL</kbd>C」と入力し + <kbd></kbd>ます。 中断要求はリモートコンピューターに渡され、そこでリモートコマンドが終了します。

リモートコマンドの詳細については、「about_Remote」と、リモート処理をサポートするコマンドレットのヘルプトピックを参照してください。

### <a name="can-i-just-telnet-into-a-remote-computer"></a>リモートコンピューターに telnet でサインインすることはできますか。

コマンドレットを使用し `Enter-PSSession` て、リモートコンピューターとの対話型セッションを開始できます。

PowerShell プロンプトで次のように入力します。

```powershell
Enter-PSSession <ComputerName>
```

リモートコンピューターに接続していることを示すために、コマンドプロンプトが変更されます。

```
<ComputerName>\C:>
```

入力したコマンドは、リモートコンピューターで直接入力した場合と同じように、リモートコンピューター上で実行されます。

対話型セッションを終了するには、次のように入力します。

```powershell
Exit-PSSession
```

対話型セッションは、WS-Management プロトコルを使用する永続的なセッションです。 これは Telnet の使用と同じではありませんが、同様のエクスペリエンスを提供します。

詳細については、「`Enter-PSSession`」を参照してください。

### <a name="can-i-create-a-persistent-connection"></a>永続的な接続を作成できますか。

はい。 リモートコマンドを実行するには、リモートコンピューターの名前、NetBIOS 名、またはその IP アドレスを指定します。 または、リモートコンピューターに接続されている PowerShell セッション (PSSession) を指定することによって、リモートコマンドを実行できます。

またはの **ComputerName** パラメーターを使用すると `Invoke-Command` `Enter-PSSession` 、PowerShell によって一時的な接続が確立されます。 PowerShell は接続を使用して現在のコマンドのみを実行し、接続を閉じます。 これは、多くのリモートコンピューターであっても、1つのコマンドまたは複数の関連のないコマンドを実行するための非常に効率的な方法です。

コマンドレットを使用して PSSession を作成すると `New-PSSession` 、PowerShell は pssession の永続的な接続を確立します。 その後、データを共有するコマンドを含め、複数のコマンドを PSSession で実行できます。

通常は、PSSession を作成して、データを共有する一連の関連コマンドを実行します。 それ以外の場合、 **ComputerName** パラメーターによって作成される一時的な接続では、ほとんどのコマンドに対して十分です。

セッションの詳細については、「about_PSSessions」を参照してください。

### <a name="can-i-run-commands-on-more-than-one-computer-at-a-time"></a>一度に複数のコンピューターでコマンドを実行できますか。

はい。 コマンドレットの **ComputerName** パラメーターには `Invoke-Command` 複数のコンピューター名を指定し、 **Session** パラメーターは複数の pssession を受け入れます。

コマンドを実行すると、PowerShell は、指定された `Invoke-Command` すべてのコンピューターまたは指定された pssession のすべてでコマンドを実行します。

PowerShell では、数百の同時リモート接続を管理できます。 ただし、送信できるリモートコマンドの数は、コンピューターのリソースとその容量によって制限され、複数のネットワーク接続を確立して維持することができます。

詳細については、ヘルプトピックの「」の例を参照してください `Invoke-Command` 。

### <a name="where-are-my-profiles"></a>プロファイルはどこにありますか。

PowerShell プロファイルはリモートセッションで自動的に実行されないため、プロファイルによって追加されたコマンドはセッションに存在しません。 また、 `$profile` 自動変数は、リモートセッションでは設定されません。

セッションでプロファイルを実行するには、コマンドレットを使用し `Invoke-Command` ます。

たとえば、次のコマンドは、のセッションでローカルコンピューターからたとえばプロファイルを実行し `$s` ます。

```
Invoke-Command -Session $s -FilePath $profile
```

次のコマンドは、のセッションでリモートコンピューターからたとえばプロファイルを実行し `$s` ます。 変数が設定されていないため、この `$profile` コマンドはプロファイルへの明示的なパスを使用します。

```powershell
Invoke-Command -Session $s {
  . "$home\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"
}
```

このコマンドを実行すると、プロファイルによってセッションに追加されたコマンドがで使用できるように `$s` なります。

セッション構成のスタートアップスクリプトを使用して、セッション構成を使用するすべてのリモートセッションでプロファイルを実行することもできます。

PowerShell プロファイルの詳細については、「about_Profiles」を参照してください。
セッション構成の詳細については、「」を参照してください `Register-PSSessionConfiguration` 。

### <a name="how-does-throttling-work-on-remote-commands"></a>リモートコマンドでのスロットルの動作

PowerShell には、ローカルコンピューター上のリソースを管理するために、コマンドごとの調整機能が用意されています。この機能を使用すると、各コマンドに対して確立される同時リモート接続の数を制限できます。

既定値は32同時接続ですが、コマンドレットの **ThrottleLimit** パラメーターを使用して、特定のコマンドに対してカスタムスロットル制限を設定することができます。

調整機能を使用する場合は、セッション全体またはコンピューターに対してではなく、各コマンドに適用されることに注意してください。 複数のセッションまたは PSSessions で同時にコマンドを実行している場合、同時接続の数は、すべてのセッションの同時接続の合計になります。

**ThrottleLimit** パラメーターを使用してコマンドレットを検索するには、次のように入力します。

```
Get-Help * -Parameter ThrottleLimit
-or-
Get-Command -ParameterName ThrottleLimit
```

### <a name="is-the-output-of-remote-commands-different-from-local-output"></a>リモートコマンドの出力は、ローカル出力とは異なりますか。

PowerShell をローカルで使用する場合は、"live" .NET Framework オブジェクトを送受信します。"live" オブジェクトは、実際のプログラムまたはシステムコンポーネントに関連付けられているオブジェクトです。 メソッドを呼び出すか、またはライブオブジェクトのプロパティを変更すると、変更は実際のプログラムまたはコンポーネントに影響します。 また、プログラムまたはコンポーネントのプロパティが変更されると、それらを表すオブジェクトのプロパティも変更されます。

ただし、ほとんどのライブオブジェクトをネットワーク経由で転送することはできないため、PowerShell はリモートコマンドで送信されるオブジェクトのほとんどを "シリアル化" します。つまり、各オブジェクトは、送信用の一連の XML (XML [Export-clixml] の制約言語) データ要素に変換されます。

PowerShell は、シリアル化されたオブジェクトを受け取ると、その XML を逆シリアル化されたオブジェクト型に変換します。 逆シリアル化されたオブジェクトは、以前のプログラムまたはコンポーネントのプロパティの正確な記録ですが、"ライブ" ではなくなりました。つまり、コンポーネントに直接関連付けられなくなります。 また、これらのメソッドは、有効ではなくなったため、削除されます。

通常は、ライブオブジェクトを使用する場合と同様に、逆シリアル化されたオブジェクトを使用できますが、制限に注意する必要があります。 また、コマンドレットによって返されるオブジェクトには、 `Invoke-Command` コマンドの配信元を特定するのに役立つ追加のプロパティがあります。

DirectoryInfo オブジェクトや Guid など、オブジェクトの種類によっては、受信時にライブオブジェクトに変換されます。 これらのオブジェクトは、特別な処理や書式設定を必要としません。

リモート出力の解釈と書式設定の詳細については、「 [about_Remote_Output](about_Remote_Output.md)」を参照してください。

### <a name="can-i-run-background-jobs-remotely"></a>バックグラウンドジョブをリモートで実行できますか。

はい。 PowerShell バックグラウンドジョブは、セッションと対話せずに非同期的に実行される PowerShell コマンドです。 バックグラウンドジョブを開始すると、コマンドプロンプトがすぐに返されます。ジョブが長時間実行されている場合でも、ジョブの実行中にセッションで作業を続けることができます。

バックグラウンドジョブは常に一時的なセッションで非同期に実行されるため、他のコマンドが実行されている間でもバックグラウンドジョブを開始できます。

バックグラウンドジョブは、ローカルコンピューターまたはリモートコンピューターで実行できます。 既定では、バックグラウンドジョブはローカルコンピューター上で実行されます。 ただし、コマンドレットの **AsJob** パラメーターを使用し `Invoke-Command` て、バックグラウンドジョブとして任意のリモートコマンドを実行できます。 また、を使用して `Invoke-Command` コマンドをリモートで実行することもでき `Start-Job` ます。

PowerShell のバックグラウンドジョブの詳細については、[about_Jobs (about_Jobs)] および [about_Remote_Jobs (about_Remote_Jobs)] を参照してください。

### <a name="can-i-run-windows-programs-on-a-remote-computer"></a>リモートコンピューターで windows プログラムを実行できますか。

PowerShell リモートコマンドを使用して、リモートコンピューター上の Windows ベースのプログラムを実行できます。 たとえば、 `Shutdown.exe` リモートコンピューターでまたはを実行でき `Ipconfig.exe` ます。

ただし、PowerShell コマンドを使用してリモートコンピューター上のプログラムのユーザーインターフェイスを開くことはできません。

リモートコンピューターで Windows プログラムを起動しても、コマンドは完了せず、PowerShell コマンドプロンプトでは、プログラムが終了するまで、または<kbd>CTRL</kbd>C キーを押してコマンドを中断するまでは返されません + <kbd></kbd> 。 たとえば、リモートコンピューターでプログラムを実行した場合、 `Ipconfig.exe` が完了するまでコマンドプロンプトからは返されません `Ipconfig.exe` 。

リモートコマンドを使用してユーザーインターフェイスを持つプログラムを起動すると、プログラムのプロセスが開始されますが、ユーザーインターフェイスは表示されません。 PowerShell コマンドが完了していません。コマンドプロンプトは、プログラムプロセスを停止するか、または<kbd>CTRL</kbd>C キーを押すまで戻りません + <kbd></kbd>。これにより、コマンドが中断され、プロセスが停止します。

たとえば、PowerShell コマンドを使用してリモートコンピューターで実行する場合、 `Notepad` メモ帳のプロセスはリモートコンピューターで開始されますが、メモ帳のユーザーインターフェイスは表示されません。 コマンドを中断してコマンドプロンプトを復元するには、 <kbd>CTRL</kbd> + <kbd>C</kbd>キーを押します。

### <a name="can-i-limit-the-commands-that-users-can-run-remotely-on-my-computer"></a>ユーザーがコンピューター上でリモートで実行できるコマンドを制限できますか。

はい。 すべてのリモートセッションで、リモートコンピューター上のセッション構成のいずれかを使用する必要があります。 コンピューター上のセッション構成 (およびそれらのセッション構成に対するアクセス許可) を管理して、コンピューター上でコマンドをリモートで実行できるユーザーと実行可能なコマンドを決定できます。

セッション構成によって、セッションの環境が構成されます。 構成を定義するには、新しい構成クラスを実装するアセンブリを使用するか、セッションで実行されるスクリプトを使用します。 構成では、セッションで使用できるコマンドを特定できます。
また、構成には、1つのオブジェクトまたはコマンドでセッションがリモートで受信できるデータの量を制限する設定など、コンピューターを保護する設定を含めることができます。 また、構成を使用するために必要なアクセス許可を決定するセキュリティ記述子を指定することもできます。

`Enable-PSRemoting`コマンドレットにより、コンピューターに既定のセッション構成が作成されます。これには、microsoft.powershell32、および (64 ビットオペレーティングシステムのみ) が使用されます。 `Enable-PSRemoting` 構成のセキュリティ記述子を設定して、コンピューター上の Administrators グループのメンバーだけが使用できるようにします。

セッション構成コマンドレットを使用して、既定のセッション構成を編集したり、新しいセッション構成を作成したり、すべてのセッション構成のセキュリティ記述子を変更したりできます。

Windows PowerShell 3.0 以降では、 `New-PSSessionConfigurationFile` コマンドレットを使用して、テキストファイルを使用してカスタムセッション構成を作成できます。 このファイルには、言語モードを設定し、セッション構成を使用するセッションで使用できるコマンドレットとモジュールを指定するためのオプションが含まれています。

ユーザーは `Invoke-Command` 、、、またはコマンドレットを使用するときに、ConfigurationName パラメーターを使用して、 `New-PSSession` `Enter-PSSession` セッションに使用するセッション構成を示すことができます。  また、セッションのユーザー設定変数の値を変更することによって、セッションで使用される既定の構成を変更することもでき `$PSSessionConfigurationName` ます。

セッション構成の詳細については、セッション構成コマンドレットのヘルプを参照してください。 セッション構成コマンドレットを見つけるには、次のように入力します。

```powershell
Get-Command *PSSessionConfiguration
```

### <a name="what-are-fan-in-and-fan-out-configurations"></a>ファンとファンアウトの構成とは

複数のコンピューターに関連する最も一般的な PowerShell リモート処理のシナリオは、1つのローカルコンピューター (管理者のコンピューター) が多数のリモートコンピューターで PowerShell コマンドを実行する、1対多の構成です。 これは "ファンアウト" シナリオと呼ばれます。

ただし、一部の企業では、多数のクライアントコンピューターが、ファイルサーバーやキオスクなど、PowerShell を実行している1台のリモートコンピューターに接続するように構成されています。 これは、"ファンイン" 構成と呼ばれます。

PowerShell リモート処理では、ファンアウト構成とファンアウト構成の両方がサポートされます。

ファンアウト構成の場合、PowerShell では、Web Services for Management (WS-MANAGEMENT) プロトコルと、WS-MANAGEMENT の Microsoft 実装をサポートする WinRM サービスを使用します。 ローカルコンピューターがリモートコンピューターに接続すると、WS-Management によって接続が確立され、PowerShell のプラグインを使用してリモートコンピューターで PowerShell ホストプロセス (Wsmprovhost.exe) が開始されます。 ユーザーは、代替ポート、代替セッション構成、およびその他の機能を指定して、リモート接続をカスタマイズできます。

"ファンイン" 構成をサポートするために、PowerShell はインターネットインフォメーションサービス (IIS) を使用して WS-MANAGEMENT をホストし、PowerShell プラグインを読み込んで、PowerShell を起動します。 このシナリオでは、各 PowerShell セッションを個別のプロセスで開始するのではなく、すべての PowerShell セッションを同じホストプロセスで実行します。

IIS のホストとファンインリモート管理は、Windows XP または Windows Server 2003 ではサポートされていません。

ファンアウト構成では、ユーザーは接続 URI と HTTP エンドポイント (トランスポート、コンピューター名、ポート、アプリケーション名など) を指定できます。
IIS は、指定されたアプリケーション名を持つすべての要求をアプリケーションに転送します。 既定値は、PowerShell をホストできる WS-MANAGEMENT です。

また、認証メカニズムを指定し、HTTP および HTTPS エンドポイントからのリダイレクトを禁止または許可することもできます。

### <a name="can-i-test-remoting-on-a-single-computer-not-in-a-domain"></a>ドメイン内にない1台のコンピューターでリモート処理をテストできますか。

はい。 PowerShell リモート処理は、ローカルコンピューターがドメイン内にない場合でも使用できます。 リモート処理機能を使用して、セッションに接続し、同じコンピューター上にセッションを作成することができます。 これらの機能は、リモートコンピューターに接続する場合と同じように動作します。

ワークグループ内のコンピューターでリモートコマンドを実行するには、コンピューターの次の Windows 設定を変更します。

注意: これらの設定は、システム上のすべてのユーザーに影響を与え、悪意のある攻撃に対してシステムの脆弱性を高めることができます。 これらの変更を行うときは注意が必要です。

- Windows Vista、Windows 7、Windows 8:

  次のレジストリエントリを作成し、その値を 1: LocalAccountTokenFilterPolicy in に設定します。 ` HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`

  次の PowerShell コマンドを使用して、このエントリを追加できます。

    ```powershell
    $parameters = @{
      Path='HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System'
      Name='LocalAccountTokenFilterPolicy'
      propertyType='DWord'
      Value=1
    }
    New-ItemProperty @parameters
    ```

- Windows Server 2003、Windows Server 2008、Windows Server 2012、Windows Server 2012 R2:

  "ネットワークアクセス: ローカルアカウントの共有とセキュリティモデル" ポリシーの既定の設定は "Classic" であるため、変更は必要ありません。 設定が変更された場合は、その設定を確認します。

### <a name="can-i-run-remote-commands-on-a-computer-in-another-domain"></a>別のドメイン内のコンピューターでリモートコマンドを実行できますか。

はい。 通常、コマンドはエラーなしで実行されますが、、、またはコマンドレットの **Credential** パラメーターを使用して、 `Invoke-Command` `New-PSSession` `Enter-PSSession` リモートコンピューターの Administrators グループのメンバーの資格情報を指定することが必要になる場合があります。 これは、現在のユーザーがローカルコンピューターとリモートコンピューターの Administrators グループのメンバーである場合にも必要になることがあります。

ただし、リモートコンピューターがローカルコンピューターによって信頼されているドメインにない場合は、リモートコンピューターがユーザーの資格情報を認証できない可能性があります。

認証を有効にするには、次のコマンドを使用して、WinRM のローカルコンピューターの信頼されたホストの一覧にリモートコンピューターを追加します。 PowerShell プロンプトでコマンドを入力します。

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value <Remote-computer-name>
```

たとえば、Server01 コンピューターをローカルコンピューター上の信頼されたホストの一覧に追加するには、PowerShell プロンプトで次のコマンドを入力します。

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value Server01
```

### <a name="does-powershell-support-remoting-over-ssh"></a>PowerShell は SSH 経由のリモート処理をサポートしていますか。

はい。 詳細については、「 [SSH 経由の PowerShell リモート処理](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Remote](about_Remote.md)

[about_Profiles](about_Profiles.md)

[about_PSSessions](about_PSSessions.md)

[about_Remote_Jobs](about_Remote_Jobs.md)

[about_Remote_Variables](about_Remote_Variables.md)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)
