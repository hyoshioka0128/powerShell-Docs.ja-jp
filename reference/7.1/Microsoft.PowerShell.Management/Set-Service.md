---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 10/25/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-service?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-Service
ms.openlocfilehash: cf9c44fecde650ab0b4747aea5910da49638f297
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94344322"
---
# Set-Service

## 概要
サービスを開始、停止、および中断し、そのプロパティを変更します。

## SYNTAX

### 名前 (既定値)

```
Set-Service [-Name] <String> [-DisplayName <String>] [-Credential <PSCredential>]
 [-Description <String>] [-StartupType <ServiceStartupType>] [-Status <String>]
 [-SecurityDescriptorSddl <String>] [-Force] [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObject

```
Set-Service [-InputObject] <ServiceController> [-DisplayName <String>] [-Credential <PSCredential>]
 [-Description <String>] [-StartupType <ServiceStartupType>] [-SecurityDescriptorSddl <String>]
 [-Status <String>] [-Force] [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Set-Service`コマンドレットは、 **Status** 、 **Description** 、 **DisplayName** 、 **startuptype** などのサービスのプロパティを変更します。 `Set-Service` サービスを開始、停止、一時停止、または一時停止できます。 サービスを識別するには、サービス名を入力するか、サービスオブジェクトを送信します。 または、サービス名またはサービスオブジェクトをパイプラインからに送信 `Set-Service` します。

## 例

### 例 1: 表示名を変更する

この例では、サービスの表示名が変更されています。 元の表示名を表示するには、を使用 `Get-Service` します。

```powershell
Set-Service -Name LanmanWorkstation -DisplayName "LanMan Workstation"
```

`Set-Service`**name** パラメーターを使用して、サービスの名前 **LanmanWorkstation** を指定します。 **DisplayName** パラメーターでは、新しい表示名である **LanMan Workstation** を指定します。

### 例 2: サービスのスタートアップの種類を変更する

この例では、サービスのスタートアップの種類を変更する方法を示します。

```powershell
Set-Service -Name BITS -StartupType Automatic
Get-Service BITS | Select-Object -Property Name, StartType, Status
```

```Output
Name  StartType   Status
----  ---------   ------
BITS  Automatic  Running
```

`Set-Service`**name** パラメーターを使用して、サービスの名前 ( **ビット** ) を指定します。 **Startuptype** パラメーターを指定すると、サービスが **自動** に設定されます。

`Get-Service`**Name** パラメーターを使用して、 **BITS** サービスを指定し、オブジェクトをパイプラインの下に送信します。 `Select-Object`**Property** パラメーターを使用して、 **BITS** サービスの状態を表示します。

### 例 3: サービスの説明を変更する

この例では、BITS サービスの説明を変更し、結果を表示します。

`Get-CimInstance`コマンドレットは、サービスの **説明** を含む **Win32_Service** オブジェクトを返すために使用されます。

```powershell
Get-CimInstance Win32_Service -Filter 'Name = "BITS"'  | Format-List  Name, Description
```

```Output
Name        : BITS
Description : Transfers files in the background using idle network bandwidth. If the service is
              disabled, then any applications that depend on BITS, such as Windows Update or MSN
              Explorer, will be unable to automatically download programs and other information.
```

```powershell
Set-Service -Name BITS -Description "Transfers files in the background using idle network bandwidth."
Get-CimInstance Win32_Service -Filter 'Name = "BITS"' | Format-List  Name, Description
```

```Output
Name        : BITS
Description : Transfers files in the background using idle network bandwidth.
```

`Get-CimInstance` オブジェクトをパイプライン内でに送信 `Format-List` し、サービスの名前と説明を表示します。 比較のために、説明が更新される前と後にコマンドが実行されます。

`Set-Service`**Name** パラメーターを使用して、 **BITS** サービスを指定します。 **Description** パラメーターは、サービスの説明の更新されたテキストを指定します。

### 例 4: サービスを開始する

この例では、サービスが開始されています。

```powershell
Set-Service -Name WinRM -Status Running -PassThru
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  WinRM              Windows Remote Management (WS-Manag...
```

`Set-Service` では、 **Name** パラメーターを使用してサービス ( **WinRM** ) を指定しています。 **Status** パラメーターは、 **実行されている** 値を使用してサービスを開始します。 **PassThru** パラメーターは、結果を表示する **ServiceController** オブジェクトを出力します。

### 例 5: サービスを中断する

この例では、パイプラインを使用してサービスを一時停止します。

```powershell
Get-Service -Name Schedule | Set-Service -Status Paused
```

`Get-Service`**Name** パラメーターを使用して **Schedule** サービスを指定し、オブジェクトをパイプラインの下に送信します。 `Set-Service`**Status** パラメーターを使用して、サービスを **一時停止** に設定します。

### 例 6: サービスを停止する

この例では、変数を使用してサービスを停止します。

```powershell
$S = Get-Service -Name Schedule
Set-Service -InputObject $S -Status Stopped
```

`Get-Service`**Name** パラメーターを使用して、サービス、 **スケジュール** を指定します。 オブジェクトは変数に格納され `$S` ます。 `Set-Service`**InputObject** パラメーターを使用し、格納されているオブジェクトを指定し `$S` ます。 **Status** パラメーターは、サービスを **Stopped** に設定します。

### 例 7: リモートシステムでサービスを停止する

この例では、リモートコンピューター上のサービスを停止します。
詳細については、「 [Invoke-Command](../Microsoft.PowerShell.Core/Invoke-Command.md)」を参照してください。

```powershell
$Cred = Get-Credential
$S = Get-Service -Name Schedule
Invoke-Command -ComputerName server01.contoso.com -Credential $Cred -ScriptBlock {
  Set-Service -InputObject $S -Status Stopped
}
```

`Get-Credential` ユーザー名とパスワードの入力を求め、資格情報を変数に格納し `$Cred` ます。 `Get-Service`**Name** パラメーターを使用して **Schedule** サービスを指定します。 オブジェクトは変数に格納され `$S` ます。

`Invoke-Command`**ComputerName** パラメーターを使用して、リモートコンピューターを指定します。 **Credential** パラメーターは、変数を使用して `$Cred` コンピューターにサインオンします。 **ScriptBlock** はを呼び出し `Set-Service` ます。 **InputObject** パラメーターは、格納されているサービスオブジェクトを指定し `$S` ます。 **Status** パラメーターは、サービスを **Stopped** に設定します。

### 例 8: サービスの資格情報を変更する

この例では、サービスの管理に使用される資格情報を変更します。

```powershell
$credential = Get-Credential
Set-Service -Name Schedule -Credential $credential
```

`Get-Credential` ユーザー名とパスワードの入力を求め、資格情報を変数に格納し `$credential` ます。 `Set-Service`**Name** パラメーターを使用して **Schedule** サービスを指定します。 **Credential** パラメーターは変数を使用 `$credential` し、 **Schedule** サービスを更新します。

### 例 9: サービスの SecurityDescriptor コードを変更する

この例では、サービスの **SecurityDescriptor** を変更します。

```powershell
$SDDL = "D:(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCLCSWLOCRRC;;;SU)"
Set-Service -Name "BITS" -SecurityDescriptorSddl $SDDL
```

**SecurityDescriptor** は変数に格納され `$SDDL` ます。 `Set-Service`**Name** パラメーターを使用して、 **BITS** サービスを指定します。 **Security記述子 sddl** パラメーターでは、を使用して、 `$SDDL` **BITS** サービスの **SecurityDescriptor** を変更します。

## PARAMETERS

### -Confirm

実行前に確認メッセージを表示 `Set-Service` します。

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

### -Credential

サービスが [サービスログオンアカウント](/windows/desktop/ad/about-service-logon-accounts)として使用するアカウントを指定します。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成されるような **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、このコマンドレットによってパスワードの入力が求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description

サービスの新しい説明を指定します。

サービスの説明は **、[コンピューターの管理] の [サービス]** に表示されます。 **説明** は、 `Get-Service` **ServiceController** オブジェクトのプロパティではありません。 サービスの説明を表示するには、 `Get-CimInstance` サービスを表す **Win32_Service** オブジェクトを返すを使用します。

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

### -DisplayName

サービスの新しい表示名を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: DN

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

サービスの停止モードを指定します。 このパラメーターは、が使用されている場合にのみ機能 `-Status Stopped` します。 有効にすると、 `Set-Service` 対象サービスが停止する前に依存サービスが停止されます。 既定では、他の実行中のサービスが対象のサービスに依存している場合に例外が発生します。

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

### -InputObject

変更するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトを含む変数を入力するか、コマンドなど、オブジェクトを取得するコマンドまたは式を入力し `Get-Service` ます。 パイプラインを使用して、サービスオブジェクトをに送信でき `Set-Service` ます。

```yaml
Type: System.ServiceProcess.ServiceController
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

変更するサービスのサービス名を指定します。 ワイルドカード文字は使用できません。 パイプラインを使用して、サービス名をに送信でき `Set-Service` ます。

```yaml
Type: System.String
Parameter Sets: Name
Aliases: ServiceName, SN

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -PassThru

変更されたサービスを表す **ServiceController** オブジェクトを返します。 既定では、は `Set-Service` 出力を生成しません。

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

### -StartupType

サービスの開始モードを指定します。

このパラメーターに指定できる値は次のとおりです。

- **自動** -サービスが開始されているか、システムの起動時にオペレーティングシステムによって開始されました。
  自動的に開始されるサービスが手動で開始されるサービスに依存する場合は、手動で開始されるサービスもシステムの起動時に自動的に開始されます。
- 自動 **Delayedstart** -システムが起動した直後に開始します。
- [ **無効** ]-サービスは無効になっており、ユーザーまたはアプリケーションが開始することはできません。
- **Invalidvalue** -何の効果もありません。 コマンドレットではエラーは返されませんが、サービスの StartupType は変更されません。
- **手動** -サービスは、ユーザー、サービスコントロールマネージャー、またはアプリケーションによって手動でのみ開始されます。

```yaml
Type: Microsoft.PowerShell.Commands.ServiceStartupType
Parameter Sets: (All)
Aliases: StartMode, SM, ST, StartType
Accepted values: Automatic, AutomaticDelayedStart, Disabled, InvalidValue, Manual

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -状態

サービスの状態を指定します。

このパラメーターに指定できる値は次のとおりです。

- **一時停止** しました。 サービスを中断します。
- **実行中** 。 サービスを開始します。
- **停止済み** 。 サービスを停止します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Paused, Running, Stopped

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Security記述子 Sddl

**Sddl** 形式でサービスの **SecurityDescriptor** を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: sd

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

が実行された場合に何が起こるか `Set-Service` を示します。 コマンドレットは実行されません。

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

### ServiceController (System.string)、System.string

パイプラインを使用して、サービス名が含まれているサービスオブジェクトまたは文字列をに送信でき `Set-Service` ます。

## 出力

### System.ServiceProcess.ServiceController

既定では、は `Set-Service` オブジェクトを返しません。 **PassThru** パラメーターを使用して、 **ServiceController** オブジェクトを出力します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

`Set-Service` 昇格されたアクセス許可が必要です。 [ **管理者として実行** ] オプションを使用します。

`Set-Service` 現在のユーザーがサービスを管理するアクセス許可を持っている場合にのみ、サービスを制御できます。 コマンドが正常に動作しない場合は、必要なアクセス許可がない可能性があります。

サービスのサービス名または表示名を検索するには、を使用 `Get-Service` します。 サービス名は [ **名前** ] 列に、表示名は [ **DisplayName** ] 列に表示されます。

## 関連リンク

[Get-Service](Get-Service.md)

[New-Service](New-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)

[Remove-Service](Remove-Service.md)
