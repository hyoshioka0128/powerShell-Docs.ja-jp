---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 10/02/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/set-wsmanquickconfig?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-WSManQuickConfig
ms.openlocfilehash: e5d50af0551a183b8e9e1995fbd722c331a48486
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603203"
---
# Set-WSManQuickConfig

## 概要
ローカル コンピューターをリモート管理用に構成します。

## SYNTAX

### すべて

```
Set-WSManQuickConfig [-UseSSL] [-Force] [-SkipNetworkProfileCheck] [<CommonParameters>]
```

## Description

`Set-WSManQuickConfig`コマンドレットは、Web Services For management (ws-management) テクノロジを使用して送信される PowerShell リモートコマンドを受信するようにコンピューターを構成します。

`Set-WSManQuickConfig` は、次のアクションを実行します。

- WinRM サービスが実行されているかどうかを確認します。 WinRM サービスが実行されていない場合は、サービスが開始されます。
- WinRM サービスのスタートアップの種類を自動に設定します。
- 任意の IP アドレスで要求を受け入れるリスナーを作成します。 既定のトランスポートは **HTTP** です。
- WinRM トラフィックのファイアウォールの例外を有効にします。

を実行するに `Set-WSManQuickConfig` は、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

## 例

### 例 1: HTTP 経由でローカルコンピューターのリモート管理を有効にする

この例では、ローカルコンピューターのリモート管理を有効にするために必要な構成を設定します。 既定では、このコマンドは **HTTP** 上に WS-Management リスナーを作成します。

```powershell
Set-WSManQuickConfig
```

### 例 2: HTTPS 経由でローカルコンピューターのリモート管理を有効にする

この例では、ローカルコンピューターのリモート管理を有効にするために必要な構成を設定します。 **UseSSL** パラメーターは、コンピューターとの通信に **HTTPS** を使用することを指定します。

```powershell
Set-WSManQuickConfig -UseSSL
```

> [!NOTE]
> **HTTPS** には手動の構成が必要です。 詳細については、 **UseSSL** パラメーターの説明を参照してください。

## PARAMETERS

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

### -SkipNetworkProfileCheck

コンピューターがパブリックネットワーク上にある場合に、リモート処理用に Windows クライアントのバージョンを構成します。 このパラメーターは、同じローカル サブネット内のコンピューターに対してのみリモート アクセスを許可する、パブリック ネットワークのファイアウォール規則を有効にします。

このパラメーターは、Windows のサーバーバージョンには影響しません。既定では、パブリックネットワーク用のローカルサブネットファイアウォール規則があります。 サーバーバージョンの Windows でローカルサブネットファイアウォールルールが無効になっている場合は、 `Enable-PSRemoting` このパラメーターの値に関係なく、再度有効にします。

ローカルサブネットの制限を削除し、パブリックネットワーク上のすべての場所からのリモートアクセスを有効にするには、 `Set-NetFirewallRule` **netsecurity** モジュールのコマンドレットを使用します。

このパラメーターは、PowerShell 3.0 で導入されました。

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

### -UseSSL

リモートコンピューターへの接続を確立するために Secure Sockets Layer (SSL) プロトコルを使用することを指定します。 既定では、SSL は使用されません。

WS-Management は、ネットワーク経由で転送されるすべての PowerShell コンテンツを暗号化します。 **UseSSL** パラメーターを使用すると、HTTP ではなく HTTPS の追加の保護を指定できます。 このパラメーターを使用していて、接続に使用されているポートで SSL が使用できない場合、コマンドは失敗します。

**HTTPS** では、WinRM とファイアウォール規則を手動で構成する必要があります。 詳細については、「 [How to: CONFIGURE WINRM FOR HTTPS](https://support.microsoft.com/help/2019527/how-to-configure-winrm-for-https)」を参照してください。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは入力を受け入れません。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-PSRemoting](../Microsoft.PowerShell.Core/Enable-PSRemoting.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[方法: WINRM を HTTPS 用に構成する](https://support.microsoft.com/help/2019527/how-to-configure-winrm-for-https)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-PSSession](../Microsoft.PowerShell.Core/New-PSSession.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Test-WSMan](Test-WSMan.md)

