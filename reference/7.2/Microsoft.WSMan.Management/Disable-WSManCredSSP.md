---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/disable-wsmancredssp?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disable-WSManCredSSP
ms.openlocfilehash: 3260c88d57e98c0072af8621600a02901c561acb
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601226"
---
# Disable-WSManCredSSP

## 概要
コンピューターの CredSSP 認証を無効にします。

## SYNTAX

```
Disable-WSManCredSSP [-Role] <String> [<CommonParameters>]
```

## Description
**Enable-wsmancredssp** コマンドレットは、クライアントまたはサーバーコンピューターでの Credential Security Support Provider (CredSSP) 認証を無効にします。
CredSSP 認証を使用すると、ユーザーの資格情報が認証対象のリモートコンピューターに渡されます。

このコマンドレットを使用して、 *Role* パラメーターに client を指定することにより、クライアントで CredSSP を無効にします。
このコマンドレットは、次の操作を実行します。

- クライアントで CredSSP を無効にします。 このコマンドレットは、WS-Management 設定 **\<localhost|computername\> \Client\Auth\CredSSP** を false に設定します。
- クライアントの Windows CredSSP ポリシー **AllowFreshCredentials** から WSMan/* 設定を削除します。

このコマンドレットを使用して、 *ロール* でサーバーを指定することにより、サーバーで CredSSP を無効にします。
このコマンドレットは、次の操作を実行します。

- サーバーの CredSSP を無効にします。 このコマンドレットは、WS-Management 設定 **\<localhost|computername\> \ Service\ Auth\ CredSSP** を false に設定します。

注意: CredSSP 認証では、ユーザーの資格情報がローカルコンピューターからリモートコンピューターに委任されます。
そのため、リモート操作のセキュリティ リスクが高まります。
リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡された資格情報を使用してネットワーク セッションが制御される場合があります。

## 例

### 例 1: クライアントで CredSSP を無効にする

```
PS C:\> Disable-WSManCredSSP -Role Client
```

このコマンドは、クライアントで CredSSP を無効にして、サーバーへの委任を防止します。

### 例 2: サーバーで CredSSP を無効にする

```
PS C:\> Disable-WSManCredSSP -Role Server
```

このコマンドは、サーバーで CredSSP を無効にして、クライアントからの委任を防止します。

## PARAMETERS

### -ロール
CredSSP をクライアントとして、またはサーバーとして無効にするかどうかを指定します。
このパラメーターに指定できる値は、Client および Server です。

クライアントを指定すると、このコマンドレットは次の操作を実行します。

- クライアントで CredSSP を無効にします。 このコマンドレットは WS-Management 設定 **\<localhost|computername\> \Client\Auth\CredSSP** を false に設定します。
- クライアントの Windows CredSSP ポリシー **AllowFreshCredentials** から WSMan/* 設定を削除します。

Server を指定した場合、このコマンドレットは次の操作を実行します。

- サーバーの CredSSP を無効にします。 このコマンドレットは、WS-Management 設定 **\<localhost|computername\> \ Service\ Auth\ CredSSP** を false に設定します。

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
このコマンドレットは入力を受け取りません。

## 出力

### なし
このコマンドレットは出力を生成しません。

## 注

* CredSSP 認証を有効にするには、Enable-WSManCredSSP コマンドレットを使用します。

*

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

