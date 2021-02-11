---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/get-wsmancredssp?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-WSManCredSSP
ms.openlocfilehash: 9830d1feb31e9cca735a7ac8d2ed9d2ec50380b5
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600383"
---
# Get-WSManCredSSP

## 概要
クライアントの Credential Security Support Provider 関連の構成を取得します。

## SYNTAX

```
Get-WSManCredSSP [<CommonParameters>]
```

## Description
**Enable-wsmancredssp** コマンドレットは、クライアントとサーバーの Credential Security Support Provider 関連の構成を取得します。
出力には、Credential Security Support Provider (CredSSP) 認証が有効か無効かが示されます。
このコマンドレットには、CredSSP の **AllowFreshCredentials** ポリシーの構成情報も表示されます。

CredSSP 認証を使用すると、ユーザーの資格情報が認証対象のリモートコンピューターに渡されます。
この種類の認証は、別のリモートセッションからリモートセッションを作成するコマンド用に設計されています。
たとえば、リモートコンピューターでバックグラウンドジョブを実行する場合は、この種類の認証を使用します。

このコマンドレットは、次を実行します。

- クライアントの WS-Management CredSSP 設定 (**\<localhost|computername\> \Client\Auth\CredSSP**) を取得します。
- Windows CredSSP ポリシー設定 **AllowFreshCredentials** を取得します。
- サーバー (**\<localhost|computername\> \ service\** 認証) の WS-Management CredSSP 設定を取得します。

注意: CredSSP 認証では、ユーザーの資格情報がローカルコンピューターからリモートコンピューターに委任されます。
そのため、リモート操作のセキュリティ リスクが高まります。
リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡された資格情報を使用してネットワーク セッションが制御される場合があります。

## 例

### 例 1: CredSSP の構成を表示する

```
PS C:\> Get-WSManCredSSP
```

このコマンドは、クライアントとサーバーの両方の CredSSP 構成情報を表示します。

出力には、このコンピューターで CredSSP が構成されているかどうかが示されます。

コンピューターで CredSSP が構成されている場合は、次の出力が表示されます。

`The machine is configured to allow delegating fresh credentials to the following target(s): wsman/server02.accounting.fabrikam.com`

コンピューターで CredSSP が構成されていない場合は、次の出力が表示されます。

`The machine is not configured to allow delegating fresh credentials.`

## PARAMETERS

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
このコマンドレットは入力を受け取りません。

## 出力

### なし
このコマンドレットは出力を生成しません。

## 注

* CredSSP 認証を無効にするには、Disable-WSManCredSSP コマンドレットを使用します。 CredSSP 認証を有効にするには、Enable-WSManCredSSP コマンドレットを使用します。

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

