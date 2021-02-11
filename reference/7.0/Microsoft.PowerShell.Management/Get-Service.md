---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 10/30/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-service?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Service
ms.openlocfilehash: 15bfe1123ba005a7b326c234fad360d6d9d6cc4d
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94345733"
---
# Get-Service

## 概要
コンピューター上のサービスを取得します。

## SYNTAX

### 既定値 (既定値)

```
Get-Service [[-Name] <String[]>] [-DependentServices] [-RequiredServices] [-Include <String[]>]
 [-Exclude <String[]>] [<CommonParameters>]
```

### DisplayName

```
Get-Service [-DependentServices] [-RequiredServices] -DisplayName <String[]> [-Include <String[]>]
 [-Exclude <String[]>] [<CommonParameters>]
```

### InputObject

```
Get-Service [-DependentServices] [-RequiredServices] [-Include <String[]>] [-Exclude <String[]>]
 [-InputObject <ServiceController[]>] [<CommonParameters>]
```

## Description

`Get-Service`コマンドレットは、コンピューター上のサービスを表すオブジェクトを取得します。これには、サービスの実行と停止も含まれます。 既定では、 `Get-Service` パラメーターを指定せずにを実行すると、すべてのローカルコンピューターのサービスが返されます。

サービス名またはサービスの表示名を指定して、特定のサービスのみを取得するようにこのコマンドレットに指示することも、このコマンドレットにサービスオブジェクトをパイプ処理することもできます。

## 例

### 例 1: コンピューター上のすべてのサービスを取得する

この例では、コンピューター上のすべてのサービスを取得します。 入力した場合と同じように動作し `Get-Service *` ます。 既定では、各サービスの状態、サービス名、表示名が表示されます。

```powershell
Get-Service
```

### 例 2: 検索文字列で始まるサービスを取得する

この例では、サービス名が WMI (Windows Management Instrumentation) で始まるサービスを取得します。

```powershell
Get-Service "wmi*"
```

### 例 3: 検索文字列を含むサービスを表示する

この例では、"network" という単語を含む表示名を持つサービスを表示します。 サービス名にネットワーク (xmlprov など) が含まれていない場合でも、表示名を検索するとネットワークに関連するサービスが検出されます。

```powershell
Get-Service -Displayname "*network*"
```

### 例 4: 検索文字列と除外で始まるサービスを取得する

この例では、WinRM サービスを除き、サービス名が **win** で始まるサービスのみを取得します。

```powershell
Get-Service -Name "win*" -Exclude "WinRM"
```

### 例 5: 現在アクティブなサービスを表示する

この例では、状態が Running のサービスのみが表示されます。

```powershell
Get-Service | Where-Object {$_.Status -eq "Running"}
```

`Get-Service` コンピューター上のすべてのサービスを取得し、そのオブジェクトをパイプラインの下に送信します。 `Where-Object`コマンドレットは、 **Status** プロパティが Running で実行されているサービスのみを選択します。

Status は、サービス オブジェクトの 1 つのプロパティにすぎません。 すべてのプロパティを表示するには、「」と入力 `Get-Service | Get-Member` します。

### 例 6: 依存サービスがあるコンピューター上のサービスを一覧表示する

この例では、依存サービスを持つサービスを取得します。

```powershell
Get-Service |
  Where-Object {$_.DependentServices} |
    Format-List -Property Name, DependentServices, @{
      Label="NoOfDependentServices"; Expression={$_.dependentservices.count}
    }
```

```Output
Name                  : AudioEndpointBuilder
DependentServices     : {AudioSrv}
NoOfDependentServices : 1

Name                  : Dhcp
DependentServices     : {WinHttpAutoProxySvc}
NoOfDependentServices : 1
...
```

`Get-Service`コマンドレットは、コンピューター上のすべてのサービスを取得し、そのオブジェクトをパイプラインの下に送信します。 `Where-Object`コマンドレットでは、 **dependentservices** プロパティが null ではないサービスを選択します。

結果はパイプラインからコマンドレットに送信され `Format-List` ます。 **Property** パラメーターには、サービスの名前、依存サービスの名前、および各サービスの依存サービスの数を表示する計算されたプロパティが表示されます。

### 例 7: プロパティ値によってサービスを並べ替える

この例では、 **Status** プロパティの値によってサービスを昇順に並べ替えると、サービスを実行する前に停止したサービスが表示されることを示しています。 この理由は、 **Status** の値が列挙型であり、Stopped の値が1で、実行中の値が4であるためです。 詳細については、「 [ServiceControllerStatus](/dotnet/api/system.serviceprocess.servicecontrollerstatus)」を参照してください。

最初に実行中のサービスを一覧表示するには、コマンドレットの **降順** のパラメーターを使用し `Sort-Object` ます。

```powershell
Get-Service "s*" | Sort-Object status
```

```Output
Status   Name               DisplayName
------   ----               -----------
Stopped  stisvc             Windows Image Acquisition (WIA)
Stopped  SwPrv              MS Software Shadow Copy Provider
Stopped  SysmonLog          Performance Logs and Alerts
Running  Spooler            Print Spooler
Running  srservice          System Restore Service
Running  SSDPSRV            SSDP Discovery Service
Running  ShellHWDetection   Shell Hardware Detection
Running  Schedule           Task Scheduler
Running  SCardSvr           Smart Card
Running  SamSs              Security Accounts Manager
Running  SharedAccess       Windows Firewall/Internet Connectio...
Running  SENS               System Event Notification
Running  seclogon           Secondary Logon
```

### 例 8: サービスの依存サービスを取得する

この例では、WinRM サービスが必要とするサービスを取得します。 サービスの **ServicesDependedOn** プロパティの値が返されます。

```powershell
Get-Service "WinRM" -RequiredServices
```

### 例 9: パイプライン演算子を使用してサービスを取得する

この例では、ローカルコンピューター上の WinRM サービスを取得します。 サービス名の文字列は、引用符で囲まれて、パイプラインの下に送信され `Get-Service` ます。

```powershell
"WinRM" | Get-Service
```

## PARAMETERS

### -DependentServices

このコマンドレットが、指定されたサービスに依存するサービスのみを取得することを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: DS

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisplayName

取得するサービスの表示名を文字列配列として指定します。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: DisplayName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -除外

このコマンドレットによって操作から除外される1つ以上のサービスを、文字列配列として指定します。
このパラメーターの値は、 **Name** パラメーターを修飾します。 などの名前要素またはパターンを入力し `s*` ます。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Include

このコマンドレットによって操作に含まれる1つ以上のサービスを文字列配列として指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 などの名前要素またはパターンを入力し `s*` ます。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -InputObject

取得するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。 このコマンドレットには、サービスオブジェクトをパイプすることができます。

```yaml
Type: System.ServiceProcess.ServiceController[]
Parameter Sets: InputObject
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

取得するサービスのサービス名を指定します。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases: ServiceName

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -RequiredServices

このコマンドレットが、このサービスが必要とするサービスのみを取得することを示します。 このパラメーターは、サービスの **ServicesDependedOn** プロパティの値を取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: SDO, ServicesDependedOn

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### ServiceController (System.string)、System.string

このコマンドレットには、サービスオブジェクトまたはサービス名をパイプすることができます。

## 出力

### System.ServiceProcess.ServiceController

このコマンドレットは、コンピューター上のサービスを表すオブジェクトを返します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

PowerShell 6.0 以降では、 **ServiceController** オブジェクトに、 **UserName** 、 **Description** 、 **delayedautostart** 、 **binaryp name** 、および **startuptype** というプロパティが追加されています。

また、組み込みエイリアスであるを参照することもでき `Get-Service` `gsv` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

このコマンドレットは、現在のユーザーがサービスを表示するアクセス許可を持っている場合にのみ、サービスを表示できます。 このコマンドレットでサービスが表示されない場合は、サービスを表示するアクセス許可がない可能性があります。

システム上の各サービスのサービス名と表示名を確認するには、「」と入力 `Get-Service` します。 サービス名は [名前] 列に表示され、表示名は [ **DisplayName** ] 列に表示されます。

**Status** プロパティの値によって昇順に並べ替えた場合は、サービスを実行する前に停止したサービスが表示されます。 サービスの **status** プロパティは列挙値であり、状態名は整数値を表します。 並べ替え順序は、名前ではなく整数値に基づいています。 Stopped の値が1で、実行中の値が4であるため、[停止] が前に表示されます。 詳細については、「 [ServiceControllerStatus](/dotnet/api/system.serviceprocess.servicecontrollerstatus)」を参照してください。

## 関連リンク

[New-Service](New-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)

[Remove-Service](Remove-Service.md)
