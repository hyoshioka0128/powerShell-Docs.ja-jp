---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 07/16/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enable-PSRemoting
ms.openlocfilehash: a3d9506a0fd2adbb9cc093fb24f99e922dc8a938
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605447"
---
# Enable-PSRemoting

## 概要
リモート コマンドを受信するようにコンピューターを構成します。

## SYNTAX

```
Enable-PSRemoting [-Force] [-SkipNetworkProfileCheck] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Enable-PSRemoting`コマンドレットは、WS-Management テクノロジを使用して送信される PowerShell リモートコマンドを受信するようにコンピューターを構成します。 WS-Management ベースの PowerShell リモート処理は、現在、Windows プラットフォームでのみサポートされています。

PowerShell リモート処理は、Windows Server プラットフォームでは既定で有効になっています。 を使用し `Enable-PSRemoting` て、サポートされている他のバージョンの Windows で PowerShell リモート処理を有効にし、無効になった場合にリモート処理を再び有効にすることができます。

このコマンドは、コマンドを受信する各コンピューターで1回だけ実行する必要があります。 コマンドを送信するだけのコンピューターで実行する必要はありません。 構成では、リモート接続を受け入れるためにリスナーが開始されるため、必要な場合にのみ実行することをお勧めします。

コンピューターがパブリックネットワーク上にあるときに、Windows のクライアントバージョンで PowerShell リモート処理を有効にすることは、通常は禁止されていますが、 **SkipNetworkProfileCheck** パラメーターを使用してこの制限をスキップできます。 詳細については、**SkipNetworkProfileCheck** パラメーターの説明を参照してください。

複数の PowerShell インストールは、1台のコンピューターにサイドバイサイドで共存できます。 を実行する `Enable-PSRemoting` と、コマンドレットを実行している特定のインストールバージョンのリモート処理エンドポイントが構成されます。 `Enable-PSRemoting`Powershell 6.2 の実行中にを実行すると、powershell 6.2 を実行するリモート処理エンドポイントが構成されます。 `Enable-PSRemoting`Powershell 7-preview の実行中にを実行すると、powershell 7-preview を実行するリモート処理エンドポイントが構成されます。

`Enable-PSRemoting` 必要に応じて、2つのリモート処理エンドポイント構成を作成します。 エンドポイント構成が既に存在する場合は、単に有効になっていることが保証されます。 作成された構成は同じですが、名前が異なります。 1つは、セッションをホストする PowerShell のバージョンに対応する簡易名を持つことです。 その他の構成名には、セッションをホストする PowerShell のバージョンに関する詳細情報が含まれています。 たとえば、 `Enable-PSRemoting` powershell 6.2 で実行する場合は、 **powershell. 6**, **6.2.2** という名前の2つのエンドポイントが構成されます。
これにより、単純な名前の **powershell. 6** を使用して、powershell 6 ホストの最新バージョンへの接続を作成できます。 または、長い名前の **6.2.2** を使用して、特定の powershell ホストバージョンに接続することもできます。

新しく有効にされたリモート処理エンドポイントを使用するには `Invoke-Command` 、、、コマンドレットを使用してリモート接続を作成するときに、ConfigurationName パラメーターを使用して名前で指定する必要があり `New-PSSession` `Enter-PSSession` ます。 詳細については、例4を参照してください。

`Enable-PSRemoting`コマンドレットでは、次の操作を実行します。

- 次のタスクを実行する [set-wsmanquickconfig](../Microsoft.WSMan.Management/Set-WSManQuickConfig.md) コマンドレットを実行します。
  - WinRM サービスを開始します。
  - WinRM サービスのスタートアップの種類を自動に設定します。
  - 任意の IP アドレスで要求を受け入れるリスナーを作成します。
  - WS-Management 通信に対してファイアウォールの例外を有効にします。
  - 必要に応じて、simple および long name セッションエンドポイント構成を作成します。
  - すべてのセッション構成を有効にします。
  - リモートアクセスを許可するように、すべてのセッション構成のセキュリティ記述子を変更します。
- WinRM サービスを再起動して、上記の変更を有効にします。

Windows プラットフォームでこのコマンドレットを実行するには、[管理者として実行] オプションを使用して PowerShell を起動します。 このコマンドレットは、Linux または MacOS バージョンの PowerShell では使用できません。

> [!CAUTION]
> このコマンドレットは、Windows PowerShell によって作成されたリモートエンドポイント構成には影響しません。
> PowerShell バージョン6以降で作成されたエンドポイントにのみ影響します。 Windows PowerShell でホストされている PowerShell リモート処理エンドポイントを有効または無効にするには、 `Enable-PSRemoting` Windows powershell セッション内からコマンドレットを実行します。

## 例

### 例 1: リモートコマンドを受信するようにコンピューターを構成する

このコマンドは、リモート コマンドを受信するようにコンピューターを構成します。

```powershell
Enable-PSRemoting
```

```Output
WARNING: PowerShell remoting has been enabled only for PowerShell Core configurations and does not
affect Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect
all PowerShell remoting configurations.
```

### 例 2: 確認プロンプトを表示せずにリモートコマンドを受信するようにコンピューターを構成する

このコマンドは、リモート コマンドを受信するようにコンピューターを構成します。
**Force** パラメーターを指定すると、ユーザーにプロンプトが表示されません。

```powershell
Enable-PSRemoting -Force
```

```Output
WARNING: PowerShell remoting has been enabled only for PowerShell Core configurations and does not
affect Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect
all PowerShell remoting configurations.
```

### 例 3: クライアントでリモートアクセスを許可する

この例では、Windows オペレーティングシステムのクライアントバージョンでパブリックネットワークからのリモートアクセスを許可する方法を示します。 ファイアウォール規則の名前は、Windows のバージョンによって異なる場合があります。
を使用し `Get-NetFirewallRule` て、ルールの一覧を表示します。 ファイアウォール規則を有効にする前に、規則のセキュリティ設定を表示して、構成が環境に適していることを確認します。

```powershell
Get-NetFirewallRule -Name 'WINRM*' | Select-Object Name
```

```Output
Name
----
WINRM-HTTP-In-TCP-NoScope
WINRM-HTTP-In-TCP
WINRM-HTTP-Compat-In-TCP-NoScope
WINRM-HTTP-Compat-In-TCP
```

```powershell
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-NetFirewallRule -Name 'WINRM-HTTP-In-TCP' -RemoteAddress Any
```

既定では、は、 `Enable-PSRemoting` プライベートネットワークとドメインネットワークからのリモートアクセスを許可するネットワークルールを作成します。 このコマンドでは、**SkipNetworkProfileCheck** パラメーターを使用して、同じローカル サブネット内のパブリック ネットワークからのリモート アクセスを許可しています。 このコマンドは、確認メッセージを抑制する **Force** パラメーターを指定します。

**SkipNetworkProfileCheck** パラメーターは、既定で同じローカルサブネット内のパブリックネットワークからのリモートアクセスを許可する Windows オペレーティングシステムのサーバーバージョンには影響しません。

`Set-NetFirewallRule` **Netsecurity** モジュールのコマンドレットは、任意のリモートの場所からのパブリックネットワークからのリモートアクセスを許可するファイアウォール規則を追加します。 これには、異なるサブネット内の場所が含まれます。

### 例 4: 新しく有効化されたエンドポイント構成へのリモートセッションを作成する

この例では、コンピューターで PowerShell リモート処理を有効にする方法、構成されているエンドポイント名を検索する方法、およびいずれかのエンドポイントへのリモートセッションを作成する方法を示します。

最初のコマンドは、コンピューター上の PowerShell リモート処理を有効にします。

2番目のコマンドは、エンドポイントの構成を一覧表示します。

3番目のコマンドは、同じコンピューターに対してリモート PowerShell セッションを作成し、名前で **powershell. 6** エンドポイントを指定します。 リモートセッションは、最新の PowerShell 6 バージョン (6.2.2) でホストされます。

最後のコマンドは、リモートセッションの変数にアクセスして、 `$PSVersionTable` セッションをホストしている PowerShell のバージョンを表示します。

```powershell
Enable-PSRemoting -Force

Get-PSSessionConfiguration

$session = New-PSSession -ComputerName localhost -ConfigurationName PowerShell.6

Invoke-Command -Session $session -ScriptBlock { $PSVersionTable }
```

```Output
WARNING: PowerShell remoting has been enabled only for PowerShell Core configurations and does not
affect Windows PowerShell remoting configurations. Run this cmdlet in Windows PowerShell to affect
all PowerShell remoting configurations.

Name          : PowerShell.6
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed,
                BUILTIN\Remote Management Users AccessAllowed

Name          : PowerShell.6.2.2
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed,
                BUILTIN\Remote Management Users AccessAllowed

Name                           Value
----                           -----
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSEdition                      Core
PSRemotingProtocolVersion      2.3
Platform                       Win32NT
SerializationVersion           1.1.0.1
GitCommitId                    6.2.2
WSManStackVersion              3.0
PSVersion                      6.2.2
OS                             Microsoft Windows 10.0.18363
```

> [!NOTE]
> ファイアウォール規則の名前は、Windows のバージョンによって異なります。 コマンドレットを使用して、 `Get-NetFirewallRule` システム上のルールの名前を一覧表示します。

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

### -SkipNetworkProfileCheck

コンピューターがパブリックネットワーク上にある場合に、Windows オペレーティングシステムのクライアントバージョンでリモート処理を有効にすることを示します。 このパラメーターは、同じローカル サブネット内のコンピューターに対してのみリモート アクセスを許可する、パブリック ネットワークのファイアウォール規則を有効にします。

このパラメーターは、Windows オペレーティングシステムのサーバーバージョンには影響しません。既定では、パブリックネットワーク用のローカルサブネットファイアウォール規則があります。 サーバーのバージョンでローカルサブネットファイアウォールルールが無効になっている場合は、 `Enable-PSRemoting` このパラメーターの値に関係なく、再度有効にします。

ローカルサブネットの制限を削除し、パブリックネットワーク上のすべての場所からのリモートアクセスを有効にするには、 `Set-NetFirewallRule` **netsecurity** モジュールのコマンドレットを使用します。

このパラメーターは、PowerShell 3.0 で導入されました。

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

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### System.String

このコマンドレットは、結果を説明する文字列を返します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

Windows オペレーティングシステムのサーバーバージョンでは、は、 `Enable-PSRemoting` リモートアクセスを許可するプライベートネットワークとドメインネットワーク用のファイアウォール規則を作成し、同じローカルサブネット内のコンピューターからのみリモートアクセスを許可するパブリックネットワーク用のファイアウォール規則を作成します。

Windows オペレーティングシステムのクライアントバージョンでは、は、 `Enable-PSRemoting` 無制限のリモートアクセスを許可するプライベートネットワークとドメインネットワーク用のファイアウォール規則を作成します。 パブリック ネットワークに対して同じローカル サブネットからのリモート アクセスを許可するファイアウォール規則を作成するには、**SkipNetworkProfileCheck** パラメーターを使用します。

Windows オペレーティングシステムのクライアントまたはサーバーバージョンで、ローカルサブネットの制限を削除し、リモートアクセスを許可するパブリックネットワーク用のファイアウォール規則を作成するには、NetSecurity モジュールのコマンドレットを使用して `Set-NetFirewallRule` 次のコマンドを実行します。 `Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP-PUBLIC" -RemoteAddress Any`

`Enable-PSRemoting` すべてのセッション構成の **Enabled** プロパティの値をに設定することにより、すべてのセッション構成を有効に `$True` します。

`Enable-PSRemoting`**Deny_All** と **Network_Deny_All** の設定を削除します。 これにより、ローカルでの使用のために予約されているセッション構成へのリモートアクセスが可能になります。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Disable-PSRemoting](Disable-PSRemoting.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Remote](About/about_Remote.md)

[about_Session_Configurations](About/about_Session_Configurations.md)
