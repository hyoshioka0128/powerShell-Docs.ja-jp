---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 12/11/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/receive-pssession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Receive-PSSession
ms.openlocfilehash: 4bc7ba3a8129dd332b13bee30e386dd459d92226
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601422"
---
# Receive-PSSession

## 概要

切断されたセッションのコマンドの結果を取得します。

## SYNTAX

### セッション (既定)

```
Receive-PSSession [-Session] <PSSession> [-OutTarget <OutTarget>] [-JobName <String>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### Id

```
Receive-PSSession [-Id] <Int32> [-OutTarget <OutTarget>] [-JobName <String>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### コンピューターのセッション名

```
Receive-PSSession [-ComputerName] <String> [-ApplicationName <String>] [-ConfigurationName <String>]
 -Name <String> [-OutTarget <OutTarget>] [-JobName <String>] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [-Port <Int32>]
 [-UseSSL] [-SessionOption <PSSessionOption>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ComputerInstanceId

```
Receive-PSSession [-ComputerName] <String> [-ApplicationName <String>] [-ConfigurationName <String>]
 -InstanceId <Guid> [-OutTarget <OutTarget>] [-JobName <String>] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>] [-Port <Int32>]
 [-UseSSL] [-SessionOption <PSSessionOption>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ConnectionUriSessionName セッション名

```
Receive-PSSession [-ConfigurationName <String>] [-ConnectionUri] <Uri> [-AllowRedirection]
 -Name <String> [-OutTarget <OutTarget>] [-JobName <String>] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [-SessionOption <PSSessionOption>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ConnectionUriInstanceId

```
Receive-PSSession [-ConfigurationName <String>] [-ConnectionUri] <Uri> [-AllowRedirection]
 -InstanceId <Guid> [-OutTarget <OutTarget>] [-JobName <String>] [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [-CertificateThumbprint <String>]
 [-SessionOption <PSSessionOption>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceId

```
Receive-PSSession -InstanceId <Guid> [-OutTarget <OutTarget>] [-JobName <String>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### セッション

```
Receive-PSSession -Name <String> [-OutTarget <OutTarget>] [-JobName <String>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`Receive-PSSession`コマンドレットは、切断された PowerShell セッション (**PSSession**) で実行されているコマンドの結果を取得します。 セッションが現在接続されている場合、は、 `Receive-PSSession` セッションが切断されたときに実行されていたコマンドの結果を取得します。 セッションがまだ切断されている場合、は `Receive-PSSession` セッションに接続し、中断されたすべてのコマンドを再開して、セッションで実行されているコマンドの結果を取得します。

このコマンドレットは、PowerShell 3.0 で導入されました。

`Receive-PSSession`コマンドの代わりに、またはを使用でき `Connect-PSSession` ます。
`Receive-PSSession` は、他のセッションまたは他のコンピューターで開始された切断または再接続されたセッションに接続できます。

`Receive-PSSession` `Disconnect-PSSession` コマンドレットまたは `Invoke-Command` **indisconnectedsession** パラメーターを使用して意図的に切断された pssession で動作します。 または、ネットワークの中断によって意図せず切断されました。

コマンドレットを使用し `Receive-PSSession` て、コマンドが実行または中断されていないセッションに接続する場合、は `Receive-PSSession` セッションに接続しますが、出力またはエラーは返しません。

切断されたセッションの機能の詳細については、「[about_Remote_Disconnected_Sessions](./About/about_Remote_Disconnected_Sessions.md)」を参照してください。

いくつかの例では、スプラッティングを使用して、行の長さを減らし、読みやすさを向上させます。 詳細については、「 [about_Splatting](./About/about_Splatting.md)」を参照してください。

## 例

### 例 1: PSSession に接続する

この例では、リモートコンピューター上のセッションに接続し、セッションで実行されているコマンドの結果を取得します。

```powershell
Receive-PSSession -ComputerName Server01 -Name ITTask
```

は、 `Receive-PSSession` **ComputerName** パラメーターを使用してリモートコンピューターを指定します。 **Name** パラメーターは、Server01 コンピューター上の ittask セッションを識別します。 この例では、ITTask セッションで実行されていたコマンドの結果を取得します。

コマンドは **Outtarget** パラメーターを使用しないため、結果はコマンドラインに表示されます。

### 例 2: 切断されたセッションのすべてのコマンドの結果を取得する

この例では、2台のリモートコンピューター上の切断されたすべてのセッションで実行されているすべてのコマンドの結果を取得します。

セッションが切断されていない場合、またはコマンドを実行していない場合、は `Receive-PSSession` セッションに接続せず、出力もエラーも返しません。

```powershell
Get-PSSession -ComputerName Server01, Server02 | Receive-PSSession
```

`Get-PSSession`**ComputerName** パラメーターを使用して、リモートコンピューターを指定します。 オブジェクトは、パイプラインの下位に送信され `Receive-PSSession` ます。

### 例 3: セッションで実行されているスクリプトの結果を取得する

この例では、コマンドレットを使用して、 `Receive-PSSession` リモートコンピューターのセッションで実行されていたスクリプトの結果を取得します。

```powershell
$parms = @{
  ComputerName = "Server01"
  Name = "ITTask"
  OutTarget = "Job"
  JobName = "ITTaskJob01"
  Credential = "Domain01\Admin01"
}
Receive-PSSession @parms
```

```Output
Id     Name            State         HasMoreData     Location
--     ----            -----         -----------     --------
16     ITTaskJob01     Running       True            Server01
```

このコマンドでは、**ComputerName** および **Name** の 2　つのパラメーターを使用して、切断されたセッションを特定します。
この例では、 **Outtarget** パラメーターに job という値を指定して、 `Receive-PSSession` 結果をジョブとして返すように指示しています。 **JobName** パラメーターは、再接続されたセッションのジョブの名前を指定します。
**Credential** パラメーターは、 `Receive-PSSession` ドメイン管理者のアクセス許可を使用してコマンドを実行します。

出力は、が `Receive-PSSession` 現在のセッションのジョブとして結果を返したことを示しています。 ジョブの結果を取得するには、コマンドを使用します。 `Receive-Job`

### 例 4: ネットワークが停止した後に結果を取得する

この例では、コマンドレットを使用して、ネットワークの停止によっ `Receive-PSSession` てセッション接続が中断された後にジョブの結果を取得します。 PowerShell は、次の4分間にセッションの再接続を自動的に1回試行します。これにより、4分間のすべての試行が失敗した場合にのみ、作業が破棄されます。

```
PS> $s = New-PSSession -ComputerName Server01 -Name AD -ConfigurationName ADEndpoint
PS> $s

Id  Name   ComputerName    State        ConfigurationName     Availability
--  ----   ------------    -----        -----------------     ------------
8   AD      Server01       Opened       ADEndpoint               Available


PS> Invoke-Command -Session $s -FilePath \\Server12\Scripts\SharedScripts\New-ADResolve.ps1

Running "New-ADResolve.ps1"

# Network outage
# Restart local computer
# Network access is not re-established within 4 minutes


PS> Get-PSSession -ComputerName Server01

Id  Name   ComputerName    State          ConfigurationName      Availability
--  ----   ------------    -----          -----------------      ------------
1  Backup  Server01        Disconnected   Microsoft.PowerShell           None
8  AD      Server01        Disconnected   ADEndpoint                     None


PS> Receive-PSSession -ComputerName Server01 -Name AD -OutTarget Job -JobName AD

Job Id   Name      State         HasMoreData     Location
--       ----      -----         -----------     --------
16       ADJob     Running       True            Server01


PS> Get-PSSession -ComputerName Server01

Id  Name    ComputerName    State         ConfigurationName     Availability
--  ----    ------------    -----         -----------------     ------------
1  Backup   Server01        Disconnected  Microsoft.PowerShell          Busy
8  AD       Server01        Opened        ADEndpoint               Available
```

コマンドレットでは、 `New-PSSession` Server01 コンピューター上にセッションを作成し、そのセッションを変数に保存し `$s` ます。 変数は、 `$s` **状態** が開かれていて、 **可用性** が使用可能であることを示します。 これらの値は、セッションに接続していて、セッションでコマンドを実行できることを示しています。

`Invoke-Command`コマンドレットは、変数内のセッションでスクリプトを実行し `$s` ます。 スクリプトが開始され、データを返し始めたところで、ネットワークが停止し、セッションが中断しました。 ユーザーは、セッションを終了し、ローカル コンピューターを再起動する必要があります。

コンピューターが再起動すると、ユーザーは PowerShell を起動し、コマンドを実行して `Get-PSSession` Server01 コンピューター上のセッションを取得します。 出力には、Server01 コンピューターに **AD** セッションがまだ存在していることが示されています。 この **状態** は、 **AD** セッションが切断されていることを示します。 **Availability** 値が None の場合は、セッションがどのクライアントセッションにも接続されていないことを示します。

`Receive-PSSession`コマンドレットは、 **AD** セッションに再接続し、セッションで実行されたスクリプトの結果を取得します。 このコマンドは、 **Outtarget** パラメーターを使用して、 **adjob** という名前のジョブに結果を要求します。 このコマンドはジョブオブジェクトを返し、出力はスクリプトがまだ実行されていることを示します。

`Get-PSSession`コマンドレットは、ジョブの状態を確認するために使用されます。 出力は、 `Receive-PSSession` コマンドレットが **AD** セッションに再接続されたことを確認します。このセッションは、現在開いていて、コマンドで使用できます。 また、スクリプトは実行を再開し、スクリプトの結果を取得しています。

### 例 5: 切断されたセッションに再接続する

この例では、コマンドレットを使用して、 `Receive-PSSession` 意図的に切断されたセッションに再接続し、セッションで実行されていたジョブの結果を取得します。

```
PS> $parms = @{
      InDisconnectedSession = $True
      ComputerName = "Server01", "Server02", "Server30"
      FilePath = "\\Server12\Scripts\SharedScripts\Get-BugStatus.ps1"
      Name = "BugStatus"
      SessionOption = @{IdleTimeout = 86400000}
      ConfigurationName = "ITTasks"
    }
PS> Invoke-Command @parms
PS> Exit


PS> $s = Get-PSSession -ComputerName Server01, Server02, Server30 -Name BugStatus
PS> $s

Id  Name   ComputerName    State         ConfigurationName     Availability
--  ----   ------------    -----         -----------------     ------------
1  ITTask  Server01        Disconnected  ITTasks                       None
8  ITTask  Server02        Disconnected  ITTasks                       None
2  ITTask  Server30        Disconnected  ITTasks                       None


PS> $Results = Receive-PSSession -Session $s
PS> $s

Id  Name   ComputerName    State         ConfigurationName     Availability
--  ----   ------------    -----         -----------------     ------------
1  ITTask  Server01        Opened        ITTasks                  Available
8  ITTask  Server02        Opened        ITTasks                  Available
2  ITTask  Server30        Opened        ITTasks                  Available


PS> $Results

Bug Report - Domain 01
----------------------
ComputerName          BugCount          LastUpdated
--------------        ---------         ------------
Server01              121               Friday, December 30, 2011 5:03:34 PM
```

`Invoke-Command`コマンドレットは、3台のリモートコンピューター上でスクリプトを実行します。 このスクリプトでは、複数のデータベースからデータを収集して集計するため、スクリプトが完了するまでに長い時間がかかることがよくあります。 このコマンドは、スクリプトを開始する **Indisconnectedsession** パラメーターを使用して、セッションを直ちに切断します。 **Sessionoption** パラメーターは、切断されたセッションの **IdleTimeout** 値を拡張します。 切断されたセッションは、切断された時点からアイドル状態と見なされます。 コマンドが完了し、セッションに再接続できるように、十分な長さのアイドルタイムアウトを設定することが重要です。 IdleTimeout を設定できるのは、 **PSSession** を作成したときだけです。また、その接続を切断した場合にのみ、 **IdleTimeout** を変更できます。 **PSSession** に接続するとき、またはその結果を受信するときに、 **IdleTimeout** 値を変更することはできません。 コマンドを実行すると、ユーザーは PowerShell を終了し、コンピューターを閉じます。

次に、ユーザーが Windows を再開し、PowerShell を起動して、を使用して `Get-PSSession` スクリプトが実行されていたセッションを取得します。 コマンドは、コンピューター名、セッション名、セッション構成の名前を指定してセッションを識別し、そのセッションを変数に保存し `$s` ます。 変数の値 `$s` が表示され、セッションが切断されているがビジー状態ではないことが示されます。

`Receive-PSSession`コマンドレットは、変数内のセッションに接続 `$s` し、その結果を取得します。
このコマンドは、変数に結果を保存し `$Results` ます。 `$s`変数が表示され、セッションが接続されていて、コマンドで使用できることが示されます。

スクリプトの結果 `$Results` 変数が PowerShell コンソールに表示されます。 結果のいずれかが予期しないものである場合、ユーザーはセッションでコマンドを実行して根本原因を調査できます。

### 例 6: 切断されたセッションでジョブを実行する

この例では、切断されたセッションで実行されているジョブに対して何が起こるかを示します。

```
PS> $s = New-PSSession -ComputerName Server01 -Name Test
PS> $j = Invoke-Command -Session $s { 1..1500 | Foreach-Object {"Return $_"; sleep 30}} -AsJob
PS> $j

Id     Name           State         HasMoreData     Location
--     ----           -----         -----------     --------
16     Job1           Running       True            Server01


PS> $s | Disconnect-PSSession

Id Name   ComputerName    State         ConfigurationName     Availability
-- ----   ------------    -----         -----------------     ------------
1  Test   Server01        Disconnected  Microsoft.PowerShell          None


PS> $j

Id     Name           State         HasMoreData     Location
--     ----           -----         -----------     --------
16     Job1           Disconnected  True            Server01


PS> Receive-Job $j -Keep

Return 1
Return 2


PS> $s2 = Connect-PSSession -ComputerName Server01 -Name Test
PS> $j2 = Receive-PSSession -ComputerName Server01 -Name Test
PS> Receive-Job $j

Return 3
Return 4
```

コマンドレットにより、 `New-PSSession` Server01 コンピューター上にテストセッションが作成されます。 このコマンドでは、セッションが `$s` 変数に保存されます。

`Invoke-Command`コマンドレットは、変数内のセッションでコマンドを実行し `$s` ます。 このコマンドは、 **AsJob** パラメーターを使用して、ジョブとしてコマンドを実行し、現在のセッションにジョブオブジェクトを作成します。
このコマンドは、変数に保存されているジョブオブジェクトを返し `$j` ます。 変数は、 `$j` ジョブオブジェクトを表示します。

変数内のセッションオブジェクト `$s` がに送信され、 `Disconnect-PSSession` セッションが切断されます。

`$j`変数が表示され、変数内のジョブオブジェクトを切断した場合の影響が示され `$j` ます。 ジョブの状態が、Disconnected になっています。

は、 `Receive-Job` 変数のジョブで実行され `$j` ます。 出力には、ジョブがセッションの前に出力を返し、ジョブが切断されたことが示されています。

`Connect-PSSession`コマンドレットは、同じクライアントセッションで実行されます。 コマンドは、Server01 コンピューター上のテストセッションに再接続し、セッションを変数に保存し `$s2` ます。

`Receive-PSSession`コマンドレットは、セッションで実行されていたジョブの結果を取得します。 コマンドは同じセッションで実行されるので、 `Receive-PSSession` 既定では結果をジョブとして返し、同じジョブオブジェクトを再利用します。 このコマンドは、ジョブを変数に保存し `$j2` ます。 `Receive-Job`コマンドレットは、変数内のジョブの結果を取得し `$j` ます。

## PARAMETERS

### -AllowRedirection

このコマンドレットがこの接続を代替 Uniform Resource Identifier (URI) にリダイレクトできることを示します。

**ConnectionURI** パラメーターを使用すると、リモートの送信先は別の URI にリダイレクトするように指示を返すことができます。 既定では、PowerShell は接続をリダイレクトしませんが、このパラメーターを使用して接続をリダイレクトできるようにすることができます。

また、**MaximumConnectionRedirectionCount** セッション オプション値を変更することで、接続をリダイレクトする回数を制限することもできます。 コマンドレットの **Maximumredirection** パラメーターを使用する `New-PSSessionOption` か、Preference 変数の **MaximumConnectionRedirectionCount** プロパティを設定し `$PSSessionOption` ます。 既定値は 5 です。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ConnectionUriSessionName, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ApplicationName

アプリケーションを指定します。 このコマンドレットは、指定されたアプリケーションを使用するセッションにのみ接続します。

接続 URI のアプリケーション名セグメントを入力します。 たとえば、次の接続 URI では、WSMan はアプリケーション名 `http://localhost:5985/WSMAN` です。

セッションのアプリケーション名は、セッションの **Runspace.ConnectionInfo.AppName** プロパティに保存されています。

パラメーターの値は、セッションを選択してフィルター処理するために使用されます。 セッションで使用されるアプリケーションは変更されません。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerSessionName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -認証

切断されたセッションに再接続するためにコマンドでユーザー資格情報を認証するために使用するメカニズムを指定します。 このパラメーターの有効値は、次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential

既定値は Default です。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!CAUTION]
> ユーザー資格情報が認証対象のリモートコンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: ComputerInstanceId, ComputerSessionName, ConnectionUriSessionName, ConnectionUriInstanceId
Aliases:
Accepted values: Default, Basic, Negotiate, NegotiateWithImplicitCredential, Credssp, Digest, Kerberos

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

切断されたセッションに接続するためのアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 証明書は、ローカルユーザーアカウントにしかマップできず、ドメインアカウントでは使用できません。

証明書の拇印を取得するに `Get-Item` は、PowerShell ドライブでまたはコマンドを使用し `Get-ChildItem` `Cert:` ます。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerSessionName, ConnectionUriSessionName, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

切断されたセッションが格納されているコンピューターを指定します。 セッションは、サーバー側または接続の受信側にあるコンピューターに格納されます。 既定値はローカル コンピューターです。

1台のコンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名 (FQDN) を入力します。
ワイルドカード文字は使用できません。 ローカルコンピューターを指定するには、コンピューター名、ドット ( `.` )、 `$env:COMPUTERNAME` 、または localhost を入力します。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerSessionName
Aliases: Cn

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ConfigurationName

セッション構成の名前を指定します。 このコマンドレットは、指定されたセッション構成を使用するセッションにのみ接続します。

セッション構成の構成名または完全修飾リソース URI を入力します。 構成名のみを指定する場合は、次のスキーマ URI が先頭に付加されます。

`http://schemas.microsoft.com/powershell`.

セッションの構成名は、セッションの **ConfigurationName** プロパティに保存されています。

パラメーターの値は、セッションを選択してフィルター処理するために使用されます。 セッションが使用するセッション構成が変更されることはありません。

セッション構成の詳細については、「 [about_Session_Configurations](./About/about_Session_Configurations.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: ComputerInstanceId, ComputerSessionName, ConnectionUriSessionName, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ConnectionUri

切断されたセッションに再接続するために使用する接続エンドポイントを定義する URI を指定します。

URI は完全修飾名にする必要があります。 文字列の形式は次のとおりです。

`<Transport>://<ComputerName>:<Port>/<ApplicationName>`

既定値は、次のとおりです。

`http://localhost:5985/WSMAN`

接続 URI を指定しない場合は、 **UseSSL**、 **ComputerName**、 **Port**、および **APPLICATIONNAME** パラメーターを使用して接続 uri の値を指定できます。

URI の **Transport** セグメントの有効な値は、HTTP および HTTPS です。 トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合、セッションは標準のポート80を使用して HTTP と443が HTTPS 用に作成されます。 PowerShell リモート処理に既定のポートを使用するには、HTTP の場合はポート5985、HTTPS の場合は5986を指定します。

対象のコンピューターが接続を別の URI にリダイレクトする場合、コマンドで **Allowredirection** パラメーターを使用しない限り、PowerShell によってリダイレクトが禁止されます。

```yaml
Type: System.Uri
Parameter Sets: ConnectionUriSessionName, ConnectionUriInstanceId
Aliases: URI, CU

Required: True
Position: 0
Default value: http://localhost:5985/WSMAN
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

切断されたセッションに接続するためのアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ComputerInstanceId, ComputerSessionName, ConnectionUriSessionName, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

切断されたセッションの ID を指定します。 **Id** パラメーターは、切断されたセッションが現在のセッションに既に接続されている場合にのみ機能します。

セッションがローカルコンピューターに格納されているが、現在のセッションに接続されていない場合、このパラメーターは有効ですが、有効ではありません。

```yaml
Type: System.Int32
Parameter Sets: Id
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -InstanceId

切断されたセッションのインスタンス ID を指定します。 インスタンス ID は、ローカルコンピューターまたはリモートコンピューター上の **PSSession** を一意に識別する GUID です。 インスタンス ID は、 **PSSession** の **InstanceID** プロパティに格納されます。

```yaml
Type: System.Guid
Parameter Sets: ComputerInstanceId, ConnectionUriInstanceId, InstanceId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -JobName

が返すジョブのフレンドリ名を指定し `Receive-PSSession` ます。

`Receive-PSSession`**Outtarget** パラメーターの値が job の場合、または切断されたセッションで実行されているジョブが現在のセッションで開始された場合に、ジョブを返します。

切断されたセッションで実行されているジョブが現在のセッションで開始された場合、PowerShell は、セッションの元のジョブオブジェクトを再利用し、 **JobName** パラメーターの値を無視します。

切断されたセッションで実行されているジョブが別のセッションで開始された場合は、PowerShell によって新しいジョブオブジェクトが作成されます。 通常は既定の名前を使用ますが、このパラメータを使用して名前を変更することもできます。

**Outtarget** パラメーターの既定値または明示的な値が Job でない場合、コマンドは成功しますが、 **JobName** パラメーターを指定しても効果はありません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

切断されたセッションのフレンドリ名を指定します。

```yaml
Type: System.String
Parameter Sets: ComputerSessionName, ConnectionUriSessionName, SessionName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutTarget

セッションの結果を返す方法を指定します。 このパラメーターの有効値は、次のとおりです。

- **ジョブ**。 ジョブ オブジェクトで結果を非同期的に返します。 ジョブに名前を付けたり、名前を変更したりするには、**JobName** パラメーターを使用します。
- **ホスト**。 コマンドラインに (同期的に) 結果を返します。 コマンドを再開しようとしている場合、または結果が多数のオブジェクトで構成される場合には、応答が遅くなる可能性があります。

**Outtarget** パラメーターの既定値は Host です。 切断されたセッションで受信しているコマンドが現在のセッションで開始された場合、 **Outtarget** パラメーターの既定値は、コマンドが開始されたフォームです。 コマンドがジョブとして開始された場合、既定ではジョブとして返されます。 それ以外の場合は、既定でホストプログラムに返されます。

ホスト プログラムは通常、返されたオブジェクトを直ちにコマンド ラインに返します。ただし、この動作は変更することができます。

```yaml
Type: Microsoft.PowerShell.Commands.OutTarget
Parameter Sets: (All)
Aliases:
Accepted values: Default, Host, Job

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Port

セッションへの再接続に使用するリモートコンピューターのネットワークポートを指定します。 リモートコンピューターに接続するには、接続が使用するポートでリッスンしている必要があります。 既定のポートは5985で、これは HTTP 用の WinRM ポート、および HTTPS 用の WinRM ポートである5986です。

代替ポートを使用する前に、そのポートでリッスンするようにリモートコンピューター上の WinRM リスナーを構成する必要があります。 リスナーを構成するには、PowerShell プロンプトで次の2つのコマンドを入力します。

`Remove-Item -Path WSMan:\Localhost\listener\listener* -Recurse`

`New-Item -Path WSMan:\Localhost\listener -Transport http -Address * -Port \<port-number\>`

必要な場合を除き、 **Port** パラメーターは使用しないでください。 コマンドで設定されたポートは、コマンドを実行するすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。

```yaml
Type: System.Int32
Parameter Sets: ComputerInstanceId, ComputerSessionName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -セッション

切断されたセッションを指定します。 **Pssession** を含む変数、またはコマンドなどの **pssession** を作成または取得するコマンドを入力し `Get-PSSession` ます。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: Session
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -SessionOption

セッションの詳細オプションを指定します。 コマンドレットを使用して作成する **sessionoption** オブジェクト、 `New-PSSessionOption` またはキーがセッションオプションの名前で、値がセッションオプションの値であるハッシュテーブルを入力します。

オプションの既定値は、設定されている場合は、ユーザー設定変数の値によって決まり `$PSSessionOption` ます。 それ以外の場合、既定値はセッション構成で設定されたオプションによって決まります。

セッションオプションの値は、ユーザー設定変数およびセッション構成で設定されたセッションの既定値よりも優先され `$PSSessionOption` ます。 ただし、セッション構成で設定された最大値、クォータ、または制限よりも優先されるわけではありません。

既定値を含むセッションオプションの説明については、「」を参照してください `New-PSSessionOption` 。 **$PSSessionOption** ユーザー設定変数の詳細については、「 [about_Preference_Variables](./About/about_Preference_Variables.md)」を参照してください。 セッション構成の詳細については、「 [about_Session_Configurations](./About/about_Session_Configurations.md)」を参照してください。

```yaml
Type: System.Management.Automation.Remoting.PSSessionOption
Parameter Sets: ComputerInstanceId, ComputerSessionName, ConnectionUriSessionName, ConnectionUriInstanceId
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSSL

このコマンドレットが、切断されたセッションに接続するために Secure Sockets Layer (SSL) プロトコルを使用することを示します。 既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で送信されるすべての PowerShell コンテンツを暗号化します。 **UseSSL** は、HTTP 接続ではなく HTTPS 接続を使用してデータを送信することによって、セキュリティをさらに高める役割を果たします。

このパラメーターを使用していて、コマンドに使用されているポートで SSL が使用できない場合、コマンドは失敗します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerInstanceId, ComputerSessionName
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. 実行空間

コマンドレットによって返されるオブジェクトなど、セッションオブジェクトをこのコマンドレットにパイプすることができ `Get-PSSession` ます。

### System.Int32

このコマンドレットには、セッション Id をパイプすることができます。

### System.Guid

パイプを使用して、このコマンドレットのセッションのインスタンス Id をパイプすることができます。

### System.String

このコマンドレットには、セッション名をパイプすることができます。

## 出力

### システム管理. ジョブまたは PSObject

このコマンドレットは、切断されたセッションで実行されたコマンドの結果を返します (存在する場合)。 **Outtarget** パラメーターの値または既定値が job の場合、は `Receive-PSSession` ジョブオブジェクトを返します。 それ以外の場合には、コマンドの結果を示すオブジェクトを返します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

`Receive-PSSession` 切断されたセッションからのみ結果を取得します。 PowerShell 3.0 以降のバージョンを実行しているコンピューターは、接続されている、または終了したセッションのみを切断し、再接続することができます。

切断されたセッションで実行されていたコマンドが結果を生成しなかった場合、または結果が既に別のセッションに戻されている場合、は `Receive-PSSession` 出力を生成しません。

セッションの出力バッファーモードは、セッションが切断されたときにセッション内のコマンドが出力を管理する方法を決定します。 セッションの **OutputBufferingMode** オプションの値が Drop で、出力バッファーがいっぱいの場合、コマンドは出力の削除を開始します。 `Receive-PSSession` この出力を回復できません。 出力バッファーモードオプションの詳細については、 [新しい-PSSessionOption](New-PSSessionOption.md) と [new-pstransportoption](New-PSTransportOption.md) コマンドレットのヘルプ記事を参照してください。

Pssession に接続するとき **、または** 結果を受信するときに、 **pssession** のアイドルタイムアウト値を変更することはできません。 の **sessionoption** パラメーターは、 `Receive-PSSession` **IdleTimeout** 値を持つ **sessionoption** オブジェクトを受け取ります。 ただし、 **Sessionoption** オブジェクトの **idletimeout** 値と変数の **idletimeout** 値 `$PSSessionOption` は、 **PSSession** に接続するとき、または結果を受信するときに無視されます。

- **Pssession の作成時、** またはコマンドレットを使用して pssession のアイドルタイムアウト **を設定** および変更したり、 `New-PSSession` `Invoke-Command` **pssession** から切断したりすることができます。
- **PSSession** の **IdleTimeout** プロパティは、切断されたセッションをリモートコンピューターで保持する期間を決定するため、切断されたセッションにとって重要です。 セッションが切断されると、その時点からアイドル状態であると判断されます。これは、そのセッションでコマンドを実行している場合であっても同じです。

コマンドレットの **AsJob** パラメーターを使用してリモートセッションでジョブの開始ジョブを開始すると、ジョブがリモートセッションで実行されている場合で `Invoke-Command` も、現在のセッションでジョブオブジェクトが作成されます。 リモートセッションを切断すると、現在のセッションのジョブオブジェクトがジョブから切断されます。 ジョブオブジェクトには、返された結果が含まれていますが、切断されたセッションのジョブから新しい結果を受け取ることはありません。

実行中のジョブを含むセッションに別のクライアントが接続した場合、元のセッションの元のジョブオブジェクトに配信された結果は、新しく接続されたセッションでは使用できません。 再接続したセッションでは、元のジョブ オブジェクトに配信されなかった結果のみ利用できます。

同様に、セッションでスクリプトを開始し、セッションから切断した場合、セッションに接続している別のクライアントが切断する前にスクリプトがセッションに配信された結果が返されます。

切断するセッションでデータが失われないようにするには、コマンドレットの **Indisconnectedsession** パラメーターを使用し `Invoke-Command` ます。 このパラメーターは現在のセッションに結果が返されないようにするものであるため、セッションに再接続した時点で結果がすべて利用できるようになります。

コマンドレットを使用して `Invoke-Command` リモートセッションでコマンドを実行することで、データの損失を防ぐこともでき `Start-Job` ます。 この場合には、リモート セッションでジョブ オブジェクトが作成されます。 コマンドレットを使用し `Receive-PSSession` てジョブの結果を取得することはできません。 代わりに、コマンドレットを使用して `Connect-PSSession` セッションに接続し、コマンドレットを使用して `Invoke-Command` `Receive-Job` セッションでコマンドを実行します。

実行中のジョブを含むセッションを切断して再接続すると、元のジョブオブジェクトが再利用されるのは、ジョブが切断されて同じセッションに再接続され、再接続するコマンドで新しいジョブ名が指定されていない場合のみです。 セッションが別のクライアントセッションに再接続された場合、または新しいジョブ名を指定した場合は、PowerShell によって新しいセッションの新しいジョブオブジェクトが作成されます。

**PSSession** を切断すると、セッション状態は Disconnected になり、可用性は None になります。

- **State** プロパティの値は、現在のセッションに関連付けられています。 値が Disconnected の場合は、 **PSSession** が現在のセッションに接続されていないことを示します。 ただし、 **PSSession** がすべてのセッションから切断されているわけではありません。 別のセッションに接続されている可能性があるためです。
  セッションに接続または再接続できるかどうかを確認するには、**Availability** プロパティを使用します。
- **Availability** の値が None の場合は、セッションに接続できることを示します。 値が Busy の場合は、PSSession が別のセッションに接続されているため、 **PSSession** に接続できないことを示します。
- セッションの **State** プロパティの値の詳細については、「 [RunspaceState](/dotnet/api/system.management.automation.runspaces.runspacestate)」を参照してください。
- セッションの **Availability** プロパティの値の詳細については、「 [RunspaceAvailability](/dotnet/api/system.management.automation.runspaces.runspaceavailability)」を参照してください。

## 関連リンク

[about_PSSessions](./About/about_PSSessions.md)

[about_Remote](./About/about_Remote.md)

[about_Remote_Disconnected_Sessions](./About/about_Remote_Disconnected_Sessions.md)

[about_Session_Configurations](./About/about_Session_Configurations.md)

[Connect-PSSession](Connect-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Exit-PSSession](Exit-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)

[New-PSSessionOption](New-PSSessionOption.md)

[New-PSSessionOption](New-PSTransportOption.md)

[Remove-PSSession](Remove-PSSession.md)
