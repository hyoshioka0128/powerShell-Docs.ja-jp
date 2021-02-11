---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 08/20/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enable-WSManCredSSP
ms.openlocfilehash: bacafee683fa47d7be331b4e44d2ab75be5bdb23
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600806"
---
# Enable-WSManCredSSP

## 概要
コンピューターで Credential Security Support Provider (CredSSP) 認証を有効にします。

## SYNTAX

### すべて

```
Enable-WSManCredSSP [[-DelegateComputer] <String[]>] [-Force] [-Role] <String> [<CommonParameters>]
```

## Description

`Enable-WSManCredSSP`コマンドレットでは、クライアントまたはサーバーコンピューターで CredSSP 認証を有効にします。
CredSSP 認証を使用すると、ユーザーの資格情報が認証対象のリモートコンピューターに渡されます。 この種類の認証は、別のリモートセッションからリモートセッションを作成するコマンド用に設計されています。 たとえば、リモートコンピューターでバックグラウンドジョブを実行する場合は、この種類の認証を使用します。

`Enable-WSManCredSSP`**クライアント** または **サーバー** で CredSSP を有効にすることができます。 クライアントで CredSSP を有効にするには、 **Role** パラメーターに **client** を指定します。 サーバー認証が行われると、クライアントは明示的な資格情報をサーバーに委任します。 サーバーで CredSSP を有効にするには、 **Role** パラメーターに **server** を指定します。 サーバーは、クライアントの代理として機能します。 詳細については、「パラメーター」セクションの「 **Role** 」を参照してください。

> [!CAUTION]
> CredSSP 認証では、ユーザーの資格情報がローカルコンピューターからリモートコンピューターに委任されます。 そのため、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡された資格情報を使用してネットワーク セッションが制御される場合があります。

## 例

### 例 1: クライアントの資格情報を委任する

この例では、完全修飾ドメイン名を使用して、クライアントの資格情報をコンピューターに委任できます。

```powershell
Enable-WSManCredSSP -Role "Client" -DelegateComputer "Server02.fabrikam.com"
```

```Output
cfg         : http://schemas.microsoft.com/wbem/wsman/1/config/client/auth
lang        : en-US
Basic       : true
Digest      : true
Kerberos    : true
Negotiate   : true
Certificate : true
CredSSP     : true
```

### 例 2: ドメイン内のすべてのコンピューターにクライアントの資格情報を委任する

この例では、 **fabrikam.com** ドメイン内のすべてのコンピューターにクライアントの資格情報を委任できます。 アスタリスク ( `*` ) ワイルドカードは、すべてのコンピューターを指定します。

> [!NOTE]
> **DelegateComputer** パラメーターでワイルドカードを使用すると、必要以上に多くのコンピューターで CredSSP を有効にすることができます。

```powershell
Enable-WSManCredSSP -Role "Client" -DelegateComputer "*.fabrikam.com"
```

```Output
cfg         : http://schemas.microsoft.com/wbem/wsman/1/config/client/auth
lang        : en-US
Basic       : true
Digest      : true
Kerberos    : true
Negotiate   : true
Certificate : true
CredSSP     : true
```

### 例 3: 複数のコンピューターにクライアントの資格情報を委任する

この例では、クライアントの資格情報を複数のコンピューターに委任できます。

```powershell
$servers = "server02.fabrikam.com", "server03.fabrikam.com", "server04.fabrikam.com"
Enable-WSManCredSSP -Role "Client" -DelegateComputer $servers
```

```Output
cfg         : http://schemas.microsoft.com/wbem/wsman/1/config/client/auth
lang        : en-US
Basic       : true
Digest      : true
Kerberos    : true
Negotiate   : true
Certificate : true
CredSSP     : true
```

変数には、 `$servers` サーバー名の一覧が含まれています。 `Enable-WSManCredSSP`**role** パラメーターを使用して、**クライアント** ロールを指定します。 **DelegateComputer** は、変数からコンピューター名を取得し `$servers` ます。

### 例 4: コンピューターが代理として動作できるようにする

この例では、コンピューターを別ののデリゲートとして動作させることができます。 この `Enable-WSManCredSSP` コマンドレットは、前の例で示したように、クライアントで CredSSP 認証のみを有効にし、代理として動作できるリモートコンピューターを指定します。 リモートコンピューターがクライアントの代理として機能するためには、WSMan の **サービス** ノードの CredSSP 項目を true に設定する必要があります。 この例では、WSMan の **Service** ノードの CredSSP 項目を true に設定します。

```powershell
Enable-WSManCredSSP -Role "Server"
```

### 例 5: Set-Item を使用してコンピューターが代理人として機能することを許可する

この例では、コンピューターを別のコンピューターの代理として動作させることができます。 `Enable-WSManCredSSP`前の例で示したコマンドは、クライアントコンピューターでのみ CredSSP 認証を有効にし、クライアントコンピューターの代理として動作できるリモートコンピューターを指定します。 リモート コンピューターがクライアント コンピューターの代理として動作するためには、WSMan プロバイダーの Service ディレクトリの CredSSP 項目を true に設定する必要があります。

```powershell
Connect-WSMan -ComputerName "server02"
Set-Item -Path "WSMan:\server02\service\auth\credSSP" -Value $True
```

`Connect-WSMan` リモートコンピューター server02 への接続を作成します。 `Set-Item`**Path** パラメーターを使用して、 **WSMan** プロバイダーの場所を指定します。 **Value** パラメーターは、**サービス** 設定を true に設定します。

## PARAMETERS

### -DelegateComputer

クライアント資格情報を委任するサーバーを指定します。 完全修飾ドメイン名を使用することをお勧めします。

ワイルドカードは受け入れられますが、必要以上に多くのコンピューターで CredSSP を有効にすることができます。

**Role** パラメーターが **Client** の場合は、このパラメーターを指定する必要があります。 **Role** が **Server** の場合は、このパラメーターを指定しないでください。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ロール

クライアントとして、またはサーバーとして CredSSP を有効にするかどうかを指定します。 このパラメーターに指定できる値は、 **Client** および **Server** です。

**クライアント** を指定すると、次のアクションが実行されます。 これらの設定では、サーバー認証が実行されるときに、クライアントがサーバーに明示的な資格情報を委任することが許可されます。

- クライアントで CredSSP を有効にします。
- WS-Management 設定を `\<localhost|computername\>\Client\Auth\CredSSP` true に設定します。
- Windows CredSSP ポリシー **AllowFreshCredentials** をクライアントの **WSMan/Delegate** に設定します。

**サーバー** を指定した場合は、次のアクションが実行されます。 このポリシー設定では、サーバーがクライアントの代理として動作することが許可されます。

- サーバーで CredSSP を有効にします。
- WS-Management 設定を `\<localhost|computername\>\Service\Auth\CredSSP` true に設定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Client, Server

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは入力を受け入れません。

## 出力

### System.Xml.Xml要素

CredSSP 認証が正常に有効になっている場合、このコマンドレットは **XMLElement** オブジェクトを生成します。

## 注

CredSSP 認証を無効にするには、コマンドレットを使用し `Disable-WSManCredSSP` ます。

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

