---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 01/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/disable-psremoting?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disable-PSRemoting
ms.openlocfilehash: f2f9fb5ac13413b1ace74a995db9c3e78ac22d41
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347008"
---
# Disable-PSRemoting

## 概要
PowerShell エンドポイントがリモート接続を受信できないようにします。

## SYNTAX

```
Disable-PSRemoting [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

この `Disable-PSRemoting` コマンドレットは、ローカルコンピューター上のすべての PowerShell バージョン6以降のセッションエンドポイント構成へのリモートアクセスをブロックします。 Windows PowerShell エンドポイントの構成には影響しません。 Windows PowerShell セッションエンドポイントの構成を無効にするには、 `Disable-PSRemoting` Windows powershell セッション内からコマンドを実行します。

すべての PowerShell バージョン6以降のセッションエンドポイント構成へのリモートアクセスを再度有効にするには、コマンドレットを使用し `Enable-PSRemoting` ます。 すべての Windows PowerShell セッションエンドポイント構成へのリモートアクセスを再度有効にするには、 `Enable-PSRemoting` Windows powershell セッション内からを実行します。

> [!NOTE]
> ローカル Windows コンピューターへのすべての PowerShell リモートアクセスを無効にする場合は、PowerShell バージョン6以降のセッション内、および Windows PowerShell セッション内から、このコマンドを実行する必要があります。 Windows PowerShell は、既定ではすべての Windows マシンにインストールされます。

特定のセッションエンドポイント構成へのリモートアクセスを無効にしてから再度有効にするには、 `Enable-PSSessionConfiguration` `Disable-PSSessionConfiguration` コマンドレットとコマンドレットを使用します。 個々のエンドポイントの特定のアクセス構成を設定するには、 `Set-PSSessionConfiguration` コマンドレットを **accessmode** パラメーターと共に使用します。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

> [!NOTE]
> を実行した後も、 `Disable-PSRemoting` ローカルコンピューターでループバック接続を行うことができます。 ループバック接続は、から発信され、同じローカルコンピューターに接続する PowerShell リモートセッションです。 外部ソースからのリモートセッションはブロックされたままになります。 ループバック接続の場合は、 **EnableNetworkAccess** パラメーターに従って暗黙的な資格情報を使用する必要があります。 ループバック接続の詳細については、「 [新規-PSSession](New-PSSession.md)」を参照してください。

このコマンドレットは、Windows プラットフォームでのみ使用できます。 Linux または macOS バージョンの PowerShell では使用できません。 このコマンドレットを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

## 例

### 例 1: すべての PowerShell セッション構成へのリモートアクセスを禁止する

この例では、コンピューター上のすべての PowerShell セッションエンドポイント構成にリモートアクセスできないようにします。

```powershell
Disable-PSRemoting
```

```Output
WARNING: PowerShell remoting has been disabled only for PowerShell 6+ configurations and does not affect
 Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect all PowerShell
 remoting configurations.

WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.
```

### 例 2: 確認メッセージを表示せずにすべての PowerShell セッション構成にリモートアクセスすることを禁止する

この例では、メッセージを表示せずに、コンピューター上のすべての PowerShell セッションエンドポイント構成をリモートアクセスできないようにします。

```powershell
Disable-PSRemoting -Force
```

```Output
WARNING: PowerShell remoting has been disabled only for PowerShell 6+ configurations and does not affect
 Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect all PowerShell
 remoting configurations.

WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.
```

### 例 3: このコマンドレットを実行した場合の影響

この例は、コマンドレットを使用した場合の効果を示して `Disable-PSRemoting` います。 このコマンドシーケンスを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

セッション構成を無効にすると、 `New-PSSession` コマンドレットはローカルコンピューター ("ループバック" とも呼ばれます) へのリモートセッションを作成しようとします。 リモートアクセスはローカルコンピューターで無効になっているため、コマンドは失敗します。

```powershell
Disable-PSRemoting -Force
New-PSSession -ComputerName localhost -ConfigurationName PowerShell.6
```

```Output
WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.

New-PSSession : [localhost] Connecting to remote server localhost failed with the following error
 message : Access is denied. For more information, see the about_Remote_Troubleshooting Help topic.
At line:1 char:1
+ New-PSSession -ComputerName localhost -ConfigurationName PowerShell.6
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : OpenError: (System.Management.A\u2026tion.RemoteRunspace:RemoteRunspace)
 [New-PSSession], PSRemotingTransportException
+ FullyQualifiedErrorId : AccessDenied,PSSessionOpenFailed
```

### 例 4: このコマンドレットと Enable-PSRemoting を実行した場合の影響

この例では、 `Disable-PSRemoting` コマンドレットとコマンドレットを使用して、のセッション構成への影響を示し `Enable-PSRemoting` ます。

`Disable-PSRemoting` は、すべての PowerShell セッションエンドポイント構成へのリモートアクセスを無効にするために使用されます。 **Force** パラメーターにより、すべてのユーザー メッセージが表示されなくなります。 `Get-PSSessionConfiguration` `Format-Table` コマンドレットおよびコマンドレットは、コンピューター上のセッション構成を表示します。

出力には、ネットワークトークンを持つすべてのリモートユーザーが、エンドポイント構成へのアクセスを拒否されていることが示されています。 ローカルコンピューター上の Administrators グループは、ローカル (ループバックとも呼ばれます) で暗黙的な資格情報を使用して接続されている限り、エンドポイント構成へのアクセスが許可されます。

```powershell
Disable-PSRemoting -force
Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Auto

Enable-PSRemoting -Force
Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Auto
```

```Output
Name               Permission
----               ----------
PowerShell.6       NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed ...
PowerShell.6.2.0   NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed ...

Name               Permission
----               ----------
PowerShell.6       NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed ...
PowerShell.6.2.0   NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed ...
```

この `Enable-PSRemoting` コマンドレットは、コンピューター上のすべての PowerShell セッションエンドポイント構成へのリモートアクセスを再度有効にします。 **Force** パラメーターを指定すると、すべてのユーザープロンプトが表示されなくなり、プロンプトなしで WinRM サービスが再起動されます。 新しい出力は、 **Accessdenied** セキュリティ記述子がすべてのセッション構成から削除されたことを示しています。

### 例 5: 無効なセッションエンドポイント構成を使用したループバック接続

この例では、エンドポイントの構成を無効にする方法を示し、無効になっているエンドポイントへのループバック接続を成功させる方法を示します。 `Disable-PSRemoting` すべての PowerShell セッションエンドポイント構成を無効にします。

```powershell
Disable-PSRemoting -Force
```

```Output
WARNING: PowerShell remoting has been disabled only for PowerShell 6+ configurations and does not affect
 Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect all PowerShell
 remoting configurations.

WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.
```

```powershell
New-PSSession -ComputerName localhost -ConfigurationName powershell.6 -Credential (Get-Credential)
```

```Output
PowerShell credential request
Enter your credentials.
User: UserName
Password for user UserName: ************

New-PSSession: [localhost] Connecting to remote server localhost failed with the following error message
 : Access is denied. For more information, see the about_Remote_Troubleshooting Help topic.
```

```powershell
New-PSSession -ComputerName localhost -ConfigurationName powershell.6 -EnableNetworkAccess
```

```Output
 Id Name       Transport ComputerName  ComputerType   State   ConfigurationName   Availability
 -- ----       --------- ------------  ------------   -----   -----------------   ------------
 1  Runspace1  WSMan     localhost     RemoteMachine  Opened  powershell.6           Available
```

を初めて使用する場合は、 `New-PSSession` ローカルコンピューターへのリモートセッションの作成が試行されます。 **ConfigurationName** パラメーターは、無効になっている PowerShell エンドポイントを指定するために使用されます。 資格情報は、 **Credential** パラメーターを使用してコマンドに明示的に渡されます。 この種類の接続はネットワークスタックを経由し、ループバックではありません。 その結果、無効なエンドポイントへの接続は失敗し、" **アクセスが拒否されまし** た" というエラーが表示されます。

2つ目の使用では、 `New-PSSession` ローカルコンピューターへのリモートセッションの作成も試行します。
この場合、ネットワークスタックをバイパスするループバック接続であるため、成功します。

次の条件が満たされると、ループバック接続が作成されます。

- 接続先のコンピューター名は ' localhost ' です。
- 資格情報は渡されません。 現在ログインしているユーザー (暗黙的な資格情報) は、接続に使用されます。
- **EnableNetworkAccess** スイッチパラメーターが使用されます。

ループバック接続の詳細については、「 [新しい PSSession](New-PSSession.md) ドキュメント」を参照してください。

### 例 6: すべての PowerShell リモート処理エンドポイント構成を無効にする

この例では、コマンドを実行して `Disable-PSRemoting` も Windows PowerShell エンドポイントの構成に影響がないことを示します。 `Get-PSSessionConfiguration` Windows PowerShell 内で実行すると、すべてのエンドポイントの構成が表示されます。 Windows PowerShell エンドポイントの構成が無効になっていないことがわかります。

```powershell
Disable-PSRemoting -Force
powershell.exe -command 'Get-PSSessionConfiguration'
```

```Output
WARNING: PowerShell remoting has been disabled only for PowerShell 6+ configurations and does not affect
 Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect all PowerShell
 remoting configurations.

WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.

Name          : microsoft.powershell
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote
                Management Users AccessAllowed

Name          : microsoft.powershell.workflow
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : microsoft.powershell32
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote
                Management Users AccessAllowed

Name          : PowerShell.6
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators
                AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : PowerShell.6.2.2
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators
                AccessAllowed, BUILTIN\Remote Management Users AccessAllowed
```

```powershell
powershell.exe -command 'Disable-PSRemoting -Force'
powershell.exe -command 'Get-PSSessionConfiguration'
```

```Output
WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting or
Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to members of the
Administrators group on the computer.

Name          : microsoft.powershell
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators
                AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : microsoft.powershell.workflow
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management
                Users AccessAllowed

Name          : microsoft.powershell32
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators
                AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : PowerShell.6
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators
                AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : PowerShell.6.2.2
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators
                AccessAllowed, BUILTIN\Remote Management Users AccessAllowed
```

これらのエンドポイント構成を無効にするには、 `Disable-PSRemoting` Windows PowerShell セッション内からコマンドを実行する必要があります。 ここで、 `Get-PSSessionConfiguration` Windows PowerShell 内からを実行すると、すべてのエンドポイント構成が無効になっていることがわかります。

### 例 7: カスタムセキュリティ記述子を持つセッション構成へのリモートアクセスを禁止する

この例では、 `Disable-PSRemoting` コマンドレットを使用して、カスタムセキュリティ記述子を持つセッション構成を含むすべてのセッション構成へのリモートアクセスを無効にする方法を示します。

`Register-PSSessionConfiguration`**テスト** セッション構成を作成します。 **FilePath** パラメーターでは、セッションをカスタマイズするセッション構成ファイルを指定します。 **Showsecurity記述子 ui** パラメーターを使用すると、セッション構成のアクセス許可を設定するダイアログボックスが表示されます。 [アクセス許可] ダイアログボックスで、指定されたユーザーに対してカスタムのフルアクセス許可を作成します。

`Get-PSSessionConfiguration`およびコマンドレットでは、 `Format-Table` セッション構成とそのプロパティが表示されます。 出力には、 **テスト** セッション構成で、指定されたユーザーに対する対話型アクセスと特殊なアクセス許可が許可されていることが示されています。

`Disable-PSRemoting` すべてのセッション構成へのリモートアクセスを無効にします。

```powershell
Register-PSSessionConfiguration -Name Test -FilePath .\TestEndpoint.pssc -ShowSecurityDescriptorUI -Force
Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Wrap

Disable-PSRemoting -Force
Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Wrap
New-PSSession -ComputerName localhost -ConfigurationName Test
```

```Output
Name               Permission
----               ----------
PowerShell.6       NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed,
                   BUILTIN\Remote Management Users AccessAllowed
PowerShell.6.2.0   NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed,
                   BUILTIN\Remote Management Users AccessAllowed
Test               NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed,
                   User01 AccessAllowed

WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.

Name               Permission
----               ----------
PowerShell.6       NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed,
                   BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed
PowerShell.6.2.0   NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed,
                   BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed
Test               NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed,
                   BUILTIN\Administrators AccessAllowed, User01 AccessAllowed

New-PSSession : [localhost] Connecting to remote server localhost failed with the following error message
 : Access is denied. For more information, see the about_Remote_Troubleshooting Help topic.
At line:1 char:1
+ New-PSSession -ComputerName localhost -ConfigurationName Test
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : OpenError: (System.Management.A\u2026tion.RemoteRunspace:RemoteRunspace)
 [New-PSSession], PSRemotingTransportException
+ FullyQualifiedErrorId : AccessDenied,PSSessionOpenFailed
```

ここで `Get-PSSessionConfiguration` 、 `Format-Table` コマンドレットとコマンドレットは、すべてのネットワークユーザーに対する **accessdenied** セキュリティ記述子が、 **テスト** セッション構成を含むすべてのセッション構成に追加されることを示しています。 他のセキュリティ記述子は変更されませんが、"network_deny_all" セキュリティ記述子が優先されます。 これは、 `New-PSSession` **テスト** セッション構成に接続するためにを使用した場合に示されます。

### 例 8: 選択したセッション構成へのリモートアクセスを再度有効にする

この例では、選択したセッション構成にのみリモート アクセスを再度有効にする方法を示します。 すべてのセッション構成を無効にした後、特定のセッションを再び有効にします。

`Set-PSSessionConfiguration`コマンドレットを使用して、 **PowerShell. 6** セッション構成を変更します。 **Accessmode** パラメーターの値を **remote** に設定すると、構成へのリモートアクセスが有効になります。

```powershell
Disable-PSRemoting -Force
Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Auto

Set-PSSessionConfiguration -Name PowerShell.6 -AccessMode Remote -Force
Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Auto
```

```Output
WARNING: Disabling the session configurations does not undo all the changes made by the Enable-PSRemoting
 or Enable-PSSessionConfiguration cmdlet. You might have to manually undo the changes by following these steps:
    1. Stop and disable the WinRM service.
    2. Delete the listener that accepts requests on any IP address.
    3. Disable the firewall exceptions for WS-Management communications.
    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to
       members of the Administrators group on the computer.

Name                 Permission
----                 ----------
PowerShell.6         NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Adm ...
PowerShell.6.2.0     NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Adm ...

Name                 Permission
----                 ----------
PowerShell.6         NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\ ...
PowerShell.6.2.0     NT AUTHORITY\NETWORK AccessDenied, NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Adm ...
```

## PARAMETERS

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

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

### なし

パイプを使用してこのコマンドレットにオブジェクトを送ることはできません。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- セッション構成を無効にしても、 `Enable-PSRemoting` またはコマンドレットによって行われたすべての変更が元に戻されるわけではありません `Enable-PSSessionConfiguration` 。 場合によっては次の変更を手動で元に戻す必要があります。

  1. WinRM サービスを停止して無効にする。
  2. 任意の IP アドレスで要求を受け入れるリスナーを削除する。
  3. WS-Management 通信用のファイアウォールの例外を無効にする。
  4. LocalAccountTokenFilterPolicy の値を 0 に復元して、リモート アクセスをコンピューターの Administrators グループのメンバーに制限する。

- セッションエンドポイント構成は、セッションの環境を定義する設定のグループです。
  コンピューターに接続するすべてのセッションでは、コンピューターに登録されているセッションエンドポイント構成のいずれかを使用する必要があります。 すべてのセッションエンドポイント構成へのリモートアクセスを拒否することにより、リモートユーザーがコンピューターに接続するセッションを確立するのを効果的に防ぐことができます。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSRemoting](Enable-PSRemoting.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSession](New-PSSession.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)
