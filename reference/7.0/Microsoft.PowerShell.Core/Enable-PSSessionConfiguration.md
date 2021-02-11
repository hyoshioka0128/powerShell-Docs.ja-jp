---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/26/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-pssessionconfiguration?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enable-PSSessionConfiguration
ms.openlocfilehash: 6c8c64161ef674e3a9babaa7ad18c33b3f18cbf0
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94344441"
---
# Enable-PSSessionConfiguration

## 概要
ローカル コンピューター上のセッション構成を有効にします。

## SYNTAX

```
Enable-PSSessionConfiguration [[-Name] <String[]>] [-Force] [-SecurityDescriptorSddl <String>]
 [-SkipNetworkProfileCheck] [-NoServiceRestart] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Enable-PSSessionConfiguration`コマンドレットで `Disable-PSSessionConfiguration` は、または `Disable-PSRemoting` コマンドレットやの **accessmode** パラメーターを使用して、無効になっている登録済みのセッション構成を有効にし `Register-PSSessionConfiguration` ます。 これは、ユーザーに応じてカスタマイズされたセッション構成を管理するために、システム管理者向けに設計された高度なコマンドレットです。

パラメーターを指定しない場合、では、 `Enable-PSSessionConfiguration` セッションに使用される既定の構成である、Microsoft の PowerShell 構成が有効になり **ます。**

`Enable-PSSessionConfiguration` 影響を受けるセッション構成のセキュリティ記述子から **Deny_All** 設定を削除し、任意の IP アドレスで要求を受け入れるリスナーをオンにして、WinRM サービスを再起動します。 PowerShell 3.0 以降では、 `Enable-PSSessionConfiguration` セッション構成 () の **Enabled** プロパティの値も True に設定され `WSMan:\<computer>\PlugIn\<SessionConfigurationName>\Enabled` ます。 ただし、では、 `Enable-PSSessionConfiguration` **Network_Deny_All** `AccessMode=Local` ローカルコンピューターのユーザーのみがセッション構成を使用できるようにする Network_Deny_All () セキュリティ記述子の設定が削除または変更されることはありません。

## 例

### 例 1: 既定のセッションを再度有効にする

この例では、コンピューター上の **Microsoft PowerShell** の既定のセッション構成を再度有効にします。

```powershell
Enable-PSSessionConfiguration
```

### 例 2: 指定されたセッションを再度有効にする

この例では、コンピューター上の **MaintenanceShell** および **adminshell** セッション構成を再度有効にします。

```powershell
Enable-PSSessionConfiguration -Name MaintenanceShell, AdminShell
```

### 例 3: すべてのセッションを再度有効にする

この例では、コンピューター上のすべてのセッション構成を再度有効にします。 これらのコマンドは同等です。
そのため、いずれかを使用できます。

```powershell
Enable-PSSessionConfiguration -Name *
Get-PSSessionConfiguration | Enable-PSSessionConfiguration
```

`Enable-PSSessionConfiguration` 既に有効になっているセッション構成を有効にした場合、ではエラーは生成されません。

### 例 4: セッションを再度有効にし、新しいセキュリティ記述子を指定する

この例では、 **MaintenanceShell** セッション構成を再度有効にし、構成の新しいセキュリティ記述子を指定します。

```powershell
$sddl = "O:NSG:BAD:P(A;;GXGWGR;;;BA)(A;;GAGR;;;S-1-5-21-123456789-188441444-3100496)S:P"
Enable-PSSessionConfiguration -Name MaintenanceShell -SecurityDescriptorSDDL $sddl
```

## PARAMETERS

### -Force

コマンドレットによって確認メッセージが表示されないことを示し、プロンプトを表示せずに WinRM サービスを再起動します。 サービスを再起動すると、構成の変更が有効になります。

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

有効にするセッション構成の名前を指定します。 1 つまたは複数の構成名を入力します。
ワイルドカード文字を使用できます。

パイプを使用して、構成名またはセッション構成オブジェクトを含む文字列をにパイプすることもでき `Enable-PSSessionConfiguration` ます。

このパラメーターを省略すると、に `Enable-PSSessionConfiguration` よって、 **Microsoft PowerShell** セッション構成が有効になります。

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

コマンドレットがサービスを再起動しないことを示します。

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

### -Security記述子 Sddl

このコマンドレットがセッション構成のセキュリティ記述子を置き換えるセキュリティ記述子を指定します。

このパラメーターを省略した場合、は `Enable-PSSessionConfiguration` セキュリティ記述子からすべて拒否項目を削除します。

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

### -SkipNetworkProfileCheck

コンピューターがパブリックネットワーク上にある場合に、このコマンドレットによってセッション構成が有効になることを示します。 このパラメーターは、同じローカル サブネット内のコンピューターに対してのみリモート アクセスを許可する、パブリック ネットワークのファイアウォール規則を有効にします。 既定では、は `Enable-PSSessionConfiguration` パブリックネットワークで失敗します。

このパラメーターは、Windows オペレーティングシステムのクライアントバージョン向けに設計されています。 Windows オペレーティングシステムのサーバーバージョンには、パブリックネットワーク用のローカルサブネットファイアウォール規則があります。 ただし、ローカルサブネットファイアウォールルールがサーバーバージョンの Windows オペレーティングシステムで無効になっている場合は、このパラメーターによって再度有効になります。

ローカルサブネットの制限を削除し、パブリックネットワーク上のすべての場所からのリモートアクセスを有効にするには、 `Set-NetFirewallRule` NetSecurity モジュールのコマンドレットを使用します。 詳細については、「`Enable-PSRemoting`」を参照してください。

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

このコマンドレットを使用するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動する必要があります。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

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
