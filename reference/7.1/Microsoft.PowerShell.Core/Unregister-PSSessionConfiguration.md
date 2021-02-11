---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/unregister-pssessionconfiguration?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Unregister-PSSessionConfiguration
ms.openlocfilehash: 0ee32b680aee940df36d3219e4b24ab594e79284
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94345818"
---
# Unregister-PSSessionConfiguration

## 概要
登録されたセッション構成をコンピューターから削除します。

## SYNTAX

```
Unregister-PSSessionConfiguration [-Name] <String> [-Force] [-NoServiceRestart] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

コマンドレットでは、 `Unregister-PSSessionConfiguration` 登録されているセッション構成をコンピューターから削除します。 このコマンドレットは、システム管理者がユーザー用にカスタマイズされたセッション構成を管理するように設計されています。

変更を有効にするために、は `Unregister-PSSessionConfiguration` **WinRM** サービスを再起動します。 再起動が行われないようにするには、 **NoServiceRestart** パラメーターを指定します。

既定の **Microsoft PowerShell** または **microsoft.powershell32** セッション構成を誤って削除した場合は、コマンドレットを使用して `Enable-PSRemoting` 復元します。 詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

## 例

### 例 1: セッション構成を削除する

この例では、 **MaintenanceShell** セッション構成をコンピューターから削除します。

```powershell
Unregister-PSSessionConfiguration -Name "MaintenanceShell"
```

### 例 2: セッション構成を削除し、WinRM サービスを再起動する

この例では、 **MaintenanceShell** 構成を削除し、WinRM サービスを再起動します。 **Force** パラメーターを指定すると、プロンプトを表示せずに、 **WinRM** サービスを再起動するためのすべてのユーザーメッセージが抑制されます。

```powershell
Unregister-PSSessionConfiguration -Name MaintenanceShell -Force
```

### 例 3: すべてのセッション構成を削除する

この例では、コンピューター上のすべてのセッション構成を削除する2つの方法を示します。 どちらのコマンドも同じ効果を持ち、同義に使用できます。

```
Unregister-PSSessionConfiguration -Name *
Get-PSSessionConfiguration -Name * | Unregister-PSSessionConfiguration
```

### 例 4: 再起動せずに登録を解除する

この例では、 **NoServiceRestart** パラメーターを使用して、コンピューター上のセッションを中断するサービスの再起動を防止する効果を示します。

```
PS> Unregister-PSSessionConfiguration -Name "MaintenanceShell" -NoServiceRestart
PS> Get-PSSessionConfiguration -Name "MaintenanceShell"

Get-PSSessionConfiguration -Name MaintenanceShell : No Session Configuration matches criteria "MaintenanceShell".
+ CategoryInfo          : NotSpecified: (:) [Write-Error], WriteErrorException
+ FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException

PS> New-PSSession -ConfigurationName "MaintenanceShell"

Id Name      ComputerName    State    Configuration         Availability
-- ----      ------------    -----    -------------         ------------
1 Session1  localhost       Opened   MaintenanceShell      Available

PS> Restart-Service winrm
PS> New-PSSession -ConfigurationName MaintenanceShell

[localhost] Connecting to remote server failed with the following error message :
 The WS-Management service cannot process the request.
 The resource URI (http://schemas.microsoft.com/powershell/MaintenanceShell) was not found in the WS-Management catalog.
 The catalog contains the metadata that describes resources, or logical endpoints.
 For more information, see the about_Remote_Troubleshooting Help topic.
 + CategoryInfo          : OpenError: (System.Manageme....RemoteRunspace:RemoteRunspace) [], PSRemotingTransportException
 + FullyQualifiedErrorId : PSSessionOpenFailed
```

によって、 `Unregister-PSSessionConfiguration` **MaintenanceShell** セッション構成が削除されます。
ただし、このコマンドでは **NoServiceRestart** パラメーターが使用されるため、 **WinRM** サービスは再起動されず、変更はまだ完全に有効になりません。

次に、は `Get-PSSessionConfiguration` **MaintenanceShell** セッションを取得しようとします。 セッションは WS-Management リソーステーブルから削除されているため、で `Get-PSSessionConfiguration` 返すことはできません。

コマンドレットでは、 `New-PSSession` **MaintenanceShell** 構成を使用してセッションを作成します。 このコマンドは成功します。 次に、 **WinRM** サービスを再起動します。

最後に、 `New-PSSession` コマンドレットは、 **MaintenanceShell** 構成を使用するセッションを作成しようとします。 今回は、WinRM サービスの再起動時に **MaintenanceShell** 構成が削除されたため、セッションが失敗します。

## PARAMETERS

### -Force

コマンドレットによって確認メッセージが表示されないことを示し、プロンプトを表示せずに **WinRM** サービスを再起動します。 サービスを再起動すると、構成の変更が有効になります。

再起動を回避し、再起動メッセージを表示しないようにするには、 **NoServiceRestart** パラメーターを使用します。

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

削除するセッション構成の名前を指定します。 1 つのセッション構成名または構成名パターンを入力します。 ワイルドカード文字を使用できます。 このパラメーターは必須です。

パイプを使用してセッション構成をにパイプすることもでき `Unregister-PSSessionConfiguration` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -NoServiceRestart

このコマンドレットが **WinRM** サービスを再起動せず、サービスの再起動を求めるメッセージを表示しないことを示します。

既定では、コマンドを実行すると、 `Unregister-PSSessionConfiguration` 変更を有効にするために **WinRM** サービスを再起動するように求められます。 **WinRM** サービスが再起動されるまで、が見つからない場合でも、ユーザーは登録されていないセッション構成を引き続き使用できます `Get-PSSessionConfiguration` 。

プロンプトを表示せずに **WinRM** サービスを再起動するには、 **Force** パラメーターを指定します。 **WinRM** サービスを手動で再起動するには、コマンドレットを使用し `Restart-Service` ます。

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

### Register-pssessionconfiguration # のコマンド # には、

セッション構成オブジェクトをからこのコマンドレットにパイプすることができ `Get-PSSessionConfiguration` ます。

## 出力

### なし

このコマンドレットはオブジェクトを返しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

このコマンドレットを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動する必要があります。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSessionConfigurationFile](New-PSSessionConfigurationFile.md)

[New-PSSessionOption](New-PSSessionOption.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Test-PSSessionConfigurationFile](Test-PSSessionConfigurationFile.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)
