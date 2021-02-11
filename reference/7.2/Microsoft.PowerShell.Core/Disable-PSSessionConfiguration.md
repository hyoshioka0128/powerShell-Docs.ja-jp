---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/disable-pssessionconfiguration?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disable-PSSessionConfiguration
ms.openlocfilehash: c2f88431c0a09a2d4093c6523467a812812c10bc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603037"
---
# Disable-PSSessionConfiguration

## 概要
ローカル コンピューター上のセッション構成を無効にします。

## SYNTAX

```
Disable-PSSessionConfiguration [[-Name] <String[]>] [-Force] [-NoServiceRestart] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`Disable-PSSessionConfiguration`コマンドレットは、ローカルコンピューター上のセッション構成を無効にします。これにより、すべてのユーザーがセッション構成を使用してローカルコンピューター上にユーザー管理セッション (pssession) を作成できなくなります。 これは、ユーザーに応じてカスタマイズされたセッション構成を管理するために、システム管理者向けに設計された高度なコマンドレットです。

PowerShell 3.0 以降では、 `Disable-PSSessionConfiguration` コマンドレットにより、セッション構成 () の **Enabled** 設定が False に設定され `WSMan:\localhost\Plugins\<SessionConfiguration>\Enabled` ます。

PowerShell 2.0 では、 `Disable-PSSessionConfiguration` コマンドレットは、登録されている1つ以上のセッション構成のセキュリティ記述子に **Deny_All** エントリを追加します。

パラメーターを指定しない場合は、 `Disable-PSSessionConfiguration` セッションに使用される既定の構成である、 **Microsoft PowerShell** 構成を無効にします。 ユーザーが別の構成を指定していない限り、ローカル ユーザーとリモート ユーザーの両方がコンピューターに接続するセッションをできないように、効果的に防ぐことができます。

コンピューター上のすべてのセッション構成を無効にするには、を使用 `Disable-PSRemoting` します。

## 例

### 例 1: 既定の構成を無効にする

この例では、Microsoft PowerShell セッション構成を無効にします。

```powershell
Disable-PSSessionConfiguration
```

### 例 2: 登録されているすべてのセッション構成を無効にする

この例では、コンピューター上で登録されているすべてのセッション構成を無効にします。

```powershell
Disable-PSSessionConfiguration -Name *
```

### 例 3: 名前でセッション構成を無効にする

この例では、Microsoft で始まる名前を持つすべてのセッション構成を無効にします。 **Force** パラメーターを指定すると、コマンドレットからのすべてのユーザープロンプトが抑制されます。

```powershell
Disable-PSSessionConfiguration -Name Microsoft* -Force
```

### 例 4: パイプラインを使用してセッション構成を無効にする

この例では、 **MaintenanceShell** および **adminshell** セッション構成を無効にします。 パイプライン演算子 (|) は、の結果をに送信し `Get-PSSessionConfiguration` `Disable-PSSessionConfiguration` ます。

```powershell
Get-PSSessionConfiguration -Name MaintenanceShell, AdminShell | Disable-PSSessionConfiguration
```

### 例 5: セッション構成を無効にした場合の影響

この例では、実行前と実行後のアクセス許可、 `Disable-PSSessionConfiguration` およびセッション構成を無効にした場合の影響を示します。

```
PS> Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Auto

Name                   Permission
----                   ----------
MaintenanceShell       BUILTIN\Administrators AccessAllowed
microsoft.powershell   BUILTIN\Administrators AccessAllowed
microsoft.powershell32 BUILTIN\Administrators AccessAllowed

PS> Disable-PSSessionConfiguration -Name MaintenanceShell -Force
PS> Get-PSSessionConfiguration | Format-Table -Property Name, Permission -Auto

Name                   Permission
----                   ----------
MaintenanceShell       Everyone AccessDenied, BUILTIN\Administrators AccessAllowed
microsoft.powershell   BUILTIN\Administrators AccessAllowed
microsoft.powershell32 BUILTIN\Administrators AccessAllowed

PS> New-PSSession -ComputerName localhost -ConfigurationName MaintenanceShell

[localhost] Connecting to remote server failed with the following error message : Access is denied.
For more information, see the about_Remote_Troubleshooting Help topic.
+ CategoryInfo          : OpenError: (System.Manageme....RemoteRunspace:RemoteRunspace) [], PSRemotingTransportException
+ FullyQualifiedErrorId : PSSessionOpenFailed
```

> [!NOTE]
> 構成を無効にしても、コマンドレットを使用して構成を変更することはできません `Set-PSSessionConfiguration` 。 構成の使用は禁止されています。

## PARAMETERS

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

### -Name

無効にするセッション構成の名前の配列を指定します。 1 つまたは複数の構成名を入力します。 ワイルドカード文字を使用できます。 パイプを使用して、構成名またはセッション構成オブジェクトを含む文字列をにパイプすることもでき `Disable-PSSessionConfiguration` ます。

このパラメーターを省略すると、は、 `Disable-PSSessionConfiguration` Microsoft PowerShell セッション構成を無効にします。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -NoServiceRestart

WSMan サービスの再起動を防止するために使用されます。 構成を無効にするために、サービスを再起動する必要はありません。

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

### Register-pssessionconfiguration、System.string のように指定されています。

このコマンドレットには、セッション構成オブジェクトまたはセッション構成の名前を含む文字列をパイプすることができます。

## 出力

### なし

このコマンドレットはオブジェクトを返しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

このコマンドレットを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動する必要があります。

## 関連リンク

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSessionConfigurationFile](New-PSSessionConfigurationFile.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Test-PSSessionConfigurationFile](Test-PSSessionConfigurationFile.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)
