---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/start-service?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Start-Service
ms.openlocfilehash: b1df4490da501111ccb44dea2645a333fdf5c27e
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603235"
---
# Start-Service

## 概要
停止しているサービスを 1 つ以上開始します。

## SYNTAX

### InputObject (既定値)

```
Start-Service [-InputObject] <ServiceController[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Default

```
Start-Service [-Name] <String[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### DisplayName

```
Start-Service [-PassThru] -DisplayName <String[]> [-Include <String[]>] [-Exclude <String[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Description

`Start-Service`コマンドレットは、指定された各サービスについて、Windows サービスコントローラーに開始メッセージを送信します。 サービスが既に実行中の場合は、メッセージが無視されます。エラーにはなりません。 サービスは、サービス名または表示名で指定できます。また、 **InputObject** パラメーターを使用して、開始するサービスを表すサービスオブジェクトを指定することもできます。

## 例

### 例 1: 名前を使用してサービスを開始する

この例では、ローカルコンピューターで EventLog サービスを開始します。 **Name** パラメーターは、サービス名でサービスを識別します。

```powershell
Start-Service -Name "eventlog"
```

### 例 2: サービスを開始せずに情報を表示する

この例では、表示名に "remote" を含むサービスを開始した場合の動作を示します。

```powershell
Start-Service -DisplayName *remote* -WhatIf
```

**DisplayName** パラメーターは、サービス名ではなく、表示名でサービスを識別します。 **WhatIf** パラメーターを指定すると、コマンドを実行したときに何が起こるかがコマンドレットによって表示されますが、変更は行われません。

### 例 3: サービスを開始し、テキストファイルにアクションを記録する

この例では、コンピューター上の Windows Management Instrumentation (WMI) サービスを開始し、services.txt ファイルにアクションのレコードを追加します。

```powershell
$s = Get-Service wmi
Start-Service -InputObject $s -PassThru | Format-List >> services.txt
```

まず、を使用して、 `Get-Service` WMI サービスを表すオブジェクトを取得し、それを変数に格納し `$s` ます。 次に、サービスを開始します。 **PassThru** パラメーターを指定しない場合、で `Start-Service` は出力は作成されません。 パイプライン演算子 (|) は、によってオブジェクトの出力を `Start-Service` コマンドレットに渡して、 `Format-List` オブジェクトをプロパティの一覧として書式設定します。 追加リダイレクト演算子 () は、 \> \> services.txt ファイルに出力をリダイレクトします。 既存のファイルの末尾に出力が追加されます。

### 例 4: 無効なサービスを開始する

この例は、サービスのスタートアップの種類が **無効になっ** ている場合にサービスを開始する方法を示しています。

```
PS> Start-Service tlntsvr
Start-Service : Service 'Telnet (TlntSvr)' cannot be started due to the following error: Cannot start service TlntSvr on computer '.'.
At line:1 char:14
+ Start-Service  <<<< tlntsvr

PS> Get-CimInstance win32_service | Where-Object Name -eq "tlntsvr"
ExitCode  : 0
Name      : TlntSvr
ProcessId : 0
StartMode : Disabled
State     : Stopped
Status    : OK

PS> Set-Service tlntsvr -StartupType manual
PS> Start-Service tlntsvr
```

Telnet サービス (tlntsvr) を最初に起動しようとして失敗します。 この `Get-CimInstance` コマンドは、Tlntsvr サービスの **StartMode** プロパティが無効に **なっ** ていることを示しています。 コマンドレットにより、 `Set-Service` 開始の種類が **手動** に変更されます。 次に、コマンドを再送信し `Start-Service` ます。 今度はコマンドが成功します。 コマンドが成功したことを確認するには、を実行 `Get-Service` します。

## PARAMETERS

### -DisplayName

開始するサービスの表示名を指定します。 ワイルドカード文字を使用できます。

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

このコマンドレットで省略されるサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 などの名前要素またはパターンを入力し `s*` ます。 ワイルドカード文字を使用できます。

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

このコマンドレットが開始するサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 などの名前要素またはパターンを入力し `s*` ます。 ワイルドカード文字を使用できます。

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

開始するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.ServiceProcess.ServiceController[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

開始するサービスのサービス名を指定します。

パラメーター名は省略可能です。 **名前** またはそのエイリアスである **ServiceName** を使用することも、パラメーター名を省略することもできます。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases: ServiceName

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -PassThru

サービスを表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### ServiceController (System.string)、System.string

パイプを使用して、サービスを表すオブジェクト、またはサービス名を含む文字列をこのコマンドレットに渡します。

## 出力

### None、ServiceController。

このコマンドレットは、 **PassThru** を指定した場合に、サービスを表す **ServiceController** オブジェクトを生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- また、組み込みエイリアスであるを参照することもでき `Start-Service` `sasv` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。
- `Start-Service` 現在のユーザーがこの操作を行うためのアクセス許可を持っている場合にのみ、サービスを制御できます。 コマンドが正常に機能しない場合は、必要なアクセス許可が与えられていない可能性があります。
- システム上のサービスのサービス名と表示名を確認するには、「」と入力 `Get-Service` します。
  サービス名は [ **名前** ] 列に表示され、表示名は [ **DisplayName** ] 列に表示されます。
- 開始できるのは、開始の種類が [手動]、[自動]、または [自動 (遅延開始)] のサービスだけです。 開始の種類が無効になっているサービスを開始することはできません。 コマンドが失敗し、メッセージが表示されない場合は `Start-Service` `Cannot start service \<service-name\> on computer` 、を使用して `Get-CimInstance` サービスの開始の種類を検索し、必要に応じて、コマンドレットを使用して `Set-Service` サービスの開始の種類を変更します。
- Performance Logs and Alerts (SysmonLog) などのいくつかのサービスは、処理しなければならない作業がないと自動的に停止します。 PowerShell がサービスを開始するときに、すぐに停止すると、次のメッセージが表示されます。 `Service \<display-name\> start failed.`

## 関連リンク

[Get-Service](Get-Service.md)

[New-Service](New-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)

[Remove-Service](Remove-Service.md)
